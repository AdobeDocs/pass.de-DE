---
title: Der Fluss der Programmierberechtigungen
description: Der Fluss der Programmierberechtigungen
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# Der Fluss der Programmierberechtigungen {#prog-entitlement-flow}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#overview}

In diesem Dokument wird der grundlegende Berechtigungsfluss aus der Sicht des Programmierers beschrieben.  Informationen zu Funktionen und Anwendungsfällen, die über die hier behandelte grundlegende TVE-Integration hinausgehen, finden Sie unter [Anwendungsfälle für Programmierer](/help/authentication/integration-guide-programmers/programmer-use-cases.md).

Die Adobe Pass-Authentifizierung vermittelt den Berechtigungsfluss zwischen Programmierern und MVPDs, indem sie sichere, konsistente Schnittstellen für beide Parteien bereitstellt.  Auf der Programmiererseite bietet die Adobe Pass-Authentifizierung zwei allgemeine Arten von Berechtigungsschnittstellen:

1. AccessEnabler - Eine Client-Komponente, die eine Bibliothek mit APIs für Apps auf Geräten bereitstellt, die Web-Seiten rendern können (z. B. Web-Apps, Smartphone-/Tablet-Apps).
2. Clientless-API - RESTful-Web-Services für Geräte, die Web-Seiten nicht rendern können (z. B. Set-Top-Boxen, Spielekonsolen, Smart-TVs). Die Anforderung für das Rendern von Web-Seiten stammt aus der Anforderung von MVPD, dass sich Benutzende auf der MVPD-Website authentifizieren müssen.

Zusätzlich zu der hier vorgestellten plattformneutralen Übersicht gibt es eine clientless-API-spezifische Übersicht hier: Dokumentation zu clientless-APIs. AccessEnabler wird nativ auf unterstützten Plattformen ausgeführt (AS/JS im Web, Objective-C auf iOS und Java auf Android). Die AccessEnabler-APIs sind auf den unterstützten Plattformen konsistent. Alle Plattformen, die AccessEnabler nicht unterstützen, verwenden dieselbe Client-lose API.

Bei beiden Schnittstellentypen vermittelt die Adobe Pass-Authentifizierung auf sichere Weise den Berechtigungsfluss zwischen der App des Programmierers und der MVPD des Benutzers:

![](../assets/prog-entitlement-flow.png)


*Abbildung: Adobe Pass-Authentifizierungs-Ökosystem*

>[!IMPORTANT]
>
>Beachten Sie im obigen Diagramm, dass es einen Teil des Berechtigungsflusses gibt, der nicht über Adobe Pass-Authentifizierungsserver läuft: die MVPD-Anmeldung. Benutzer müssen sich auf der Anmeldeseite ihrer MVPD anmelden. Aufgrund dieser Anforderung muss die Programmierer-App auf Geräten, die keine Web-Seiten rendern können, Benutzer zum Wechsel zu einem Web-fähigen Gerät auffordern, sich mit ihrem MVPD anzumelden. Danach kehren sie für den Rest des Berechtigungsflusses zum Originalgerät zurück.

## Berechtigungsfluss {#entitlement-flow}

Es gibt vier verschiedene Unterflüsse, aus denen sich die Grundberechtigung zusammensetzt
Fluss:

1. [Anlauffluss](/help/authentication/integration-guide-programmers/entitlement-flow.md#startup)
1. [Authentifizierungsfluss](/help/authentication/integration-guide-programmers/entitlement-flow.md#authentication)
1. [Autorisierungsfluss](/help/authentication/integration-guide-programmers/entitlement-flow.md#authorization)
1. [Abmeldefluss](/help/authentication/integration-guide-programmers/entitlement-flow.md#logout)

Beim ersten Besuch eines Benutzers auf einer Programmierer-Site wird der Berechtigungsfluss in der oben genannten Reihenfolge fortgesetzt. Bei nachfolgenden Besuchen kann es jedoch vorkommen, dass eine Benutzerin bzw. ein Benutzer je nachdem, ob Authentifizierungs- und Autorisierungs-Token abgelaufen sind oder nicht, oder je nach Anzeigerichtlinien nur einen oder zwei der Unterflüsse durchläuft.

### Anlauffluss {#startup}

Stellt die Identität des Programmierers und des Geräts her, führt Initialisierungsaufgaben aus. Dies ist eine Voraussetzung für alle nachfolgenden Berechtigungsaufrufe.

**AccessEnabler**

* **`setRequestor()`** - Stellt Ihre Identität mit dem AccessEnalber und darüber hinaus mit den Adobe Pass-Authentifizierungsservern her. Dieser Aufruf ist ein Vorläufer zum Rest des Berechtigungsflusses. Zum Beispiel in JavaScript:

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**Clientless-API**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - Je nach Plattform können erforderliche Aufgaben vorhanden sein, die abgeschlossen werden müssen, bevor Ihre App regcode aufruft. Weitere Informationen finden **in der** zur Client-losen API . Bei Xbox-Plattformen müssen Sie beispielsweise die vorgeschriebenen Sicherheitsschritte ausführen, bevor Sie Regcode aufrufen.

### Authentifizierungsfluss {#authentication}

Bei erfolgreicher Authentifizierung wird ein Authentifizierungs-Token generiert, das mit dem Gerät und dem Anforderer verknüpft ist. Eine erfolgreiche Authentifizierung ist eine Voraussetzung für die Autorisierung.

**AccessEnabler**

* `checkAuthentication()` - Prüft, ob im lokalen Token-Cache ein gültiges zwischengespeichertes Authentifizierungstoken vorhanden ist, ohne tatsächlich den vollständigen Authentifizierungsfluss auszulösen. Dadurch wird die Rückruffunktion `setAuthenticationStatus()` Trigger.
* `getAuthentication()` - Startet den vollständigen Authentifizierungsfluss. Bei erfolgreicher Authentifizierung mit Adobe Pass wird ein Authentifizierungs-Token generiert und auf dem Client zwischengespeichert. Der Benutzer meldet sich auf seiner ausgewählten MVPD-Site an, die je nach Plattform entweder in einem iFrame, Popup-Fenster oder einer Webansicht angezeigt wird. Dadurch wird die Funktion displayProviderDialog() Trigger.

**Clientless-API**

* `<FQDN>/.../checkauthn` - Die Webservice-Version von `checkAuthentication()` oben.
* `<FQDN>/.../config` - Gibt die Liste der MVPDs an die Mobile App für den zweiten Bildschirm zurück.
* `<FQDN>/.../authenticate` - Startet den Authentifizierungsfluss der Mobile App auf dem 2. Bildschirm und leitet Benutzer zur Anmeldung zur ausgewählten MVPD weiter. Bei Erfolg generiert die Adobe Pass-Authentifizierung ein Authentifizierungs-Token und speichert es auf dem Server. Die Benutzenden kehren dann auf ihr Originalgerät zurück, um den Berechtigungsfluss abzuschließen.

Ein AuthN-Token wird als gültig betrachtet, wenn die folgenden beiden Punkte zutreffen:

* Das Authentifizierungs-Token ist nicht abgelaufen
* Der mit dem AuthN-Token verknüpfte MVPD befindet sich auf der Liste der zulässigen MVPDs für die aktuelle Anforderer-ID

#### Workflow für die Erstauthentifizierung mit Generic Access Enabler {#generic-ae-initial-authn-flow}

1. Ihre Mobile App initiiert den Authentifizierungs-Workflow mit einem Aufruf an `getAuthentication()`, der auf ein gültiges zwischengespeichertes Authentifizierungstoken prüft. Diese Methode verfügt über einen optionalen `redirectURL`. Wenn Sie keinen Wert für `redirectURL` angeben, wird der Benutzer nach einer erfolgreichen Authentifizierung an die URL zurückgegeben, von der die Authentifizierung initialisiert wurde.
1. AccessEnabler bestimmt den aktuellen Authentifizierungsstatus. Wenn der Benutzer derzeit authentifiziert ist, ruft AccessEnabler Ihre `setAuthenticationStatus()`-Rückruffunktion auf und übergibt einen Authentifizierungsstatus, der auf Erfolg hinweist.
1. Wenn der Benutzer nicht authentifiziert ist, setzt AccessEnabler den Authentifizierungsfluss fort, indem festgestellt wird, ob der letzte Authentifizierungsversuch des Benutzers mit einer bestimmten MVPD erfolgreich war. Wenn eine MVPD-ID zwischengespeichert wird UND das `canAuthenticate`-Flag „true“ lautet ODER eine MVPD mithilfe von `setSelectedProvider()` ausgewählt wurde, wird der/die Benutzende nicht über das MVPD-Auswahldialogfeld aufgefordert. Der Authentifizierungsfluss verwendet weiterhin den zwischengespeicherten Wert der MVPD (d. h. dieselbe MVPD, die bei der letzten erfolgreichen Authentifizierung verwendet wurde). Ein Netzwerkaufruf erfolgt an den Backend-Server und der Anwender wird zur Anmeldeseite von MVPD weitergeleitet.

1. Wenn keine MVPD-ID zwischengespeichert ist UND keine MVPD mithilfe von `setSelectedProvider()` ausgewählt wurde ODER das `canAuthenticate`-Flag auf „false“ gesetzt ist, wird der `displayProviderDialog()`-Callback aufgerufen. Dieser Callback leitet Ihre App an die Benutzeroberfläche weiter, die dem Benutzer eine Liste der MVPDs zur Auswahl zeigt. Es wird ein Array von MVPD-Objekten bereitgestellt, das die erforderlichen Informationen zum Erstellen des MVPD-Selektors enthält. Jedes MVPD-Objekt beschreibt eine MVPD-Entität und enthält Informationen wie die ID der MVPD und die URL, unter der sich das MVPD-Logo befindet.

1. Nachdem eine MVPD ausgewählt wurde, muss die App den AccessEnabler über die Auswahl des Benutzers informieren. Bei Nicht-Flash-Clients informieren Sie den AccessEnabler über die Benutzerauswahl, sobald der Anwender die gewünschte MVPD auswählt, über einen Aufruf der `setSelectedProvider()`. Flash-Clients senden stattdessen einen freigegebenen `MVPDEvent` vom Typ &quot;`mvpdSelection`&quot; und übergeben den ausgewählten Provider.

1. Wenn der AccessEnabler über die MVPD-Auswahl des Benutzers informiert wird, wird ein Netzwerkaufruf an den Backend-Server durchgeführt und der Benutzer wird zur Anmeldeseite von MVPD weitergeleitet.

1. Im Authentifizierungs-Workflow kommuniziert AccessEnabler mit der Adobe Pass-Authentifizierung und der ausgewählten MVPD, um die Anmeldeinformationen des Benutzers (Benutzer-ID und Kennwort) anzufordern und seine Identität zu überprüfen. Während einige MVPDs für die Anmeldung auf ihre eigene Website umleiten, verlangen andere, dass Sie ihre Anmeldeseite in einem iFrame anzeigen. Ihre Seite muss den Callback enthalten, der einen iFrame erstellt, falls der Kunde eines dieser MVPDs auswählt.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->.

1. Nachdem sich der Benutzer erfolgreich angemeldet hat, ruft AccessEnabler das Authentifizierungstoken ab und informiert Ihre App darüber, dass der Authentifizierungsfluss abgeschlossen ist. Der AccessEnabler ruft den `setAuthenticationStatus()`-Callback mit dem Status-Code 1 auf und zeigt damit an, dass er erfolgreich war. Wenn während der Ausführung dieser Schritte ein Fehler auftritt, wird der `setAuthenticationStatus()`-Callback mit dem Status-Code 0 ausgelöst, was auf einen Authentifizierungsfehler hinweist, sowie mit einem entsprechenden Fehler-Code.

>[!IMPORTANT]
>Comcast ist derzeit die einzige MVPD, die keine statische URL für das Logo bereitstellt. Programmierer sollten die neuesten Logos vom [XFINITY-Entwicklerportal](https://developers.xfinity.com/products/tv-everywhere) abrufen.
>

### Autorisierungsfluss {#authorization}

Autorisierung ist eine Voraussetzung für die Anzeige geschützter Inhalte. Bei erfolgreicher Autorisierung wird ein AuthZ-Token zusammen mit einem kurzlebigen Medien-Token generiert, das der App des Programmierers zu Sicherheitszwecken bereitgestellt wird. Beachten Sie, dass Sie zur Unterstützung des Autorisierungs-Workflows zuvor die erforderliche Einrichtung des Anforderers durchgeführt und den [Media Token Verifier“ integriert ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) müssen. Sobald diese abgeschlossen sind, können Sie die Autorisierung starten.

Ihre App initiiert die Autorisierung, wenn ein Benutzer Zugriff auf eine geschützte Ressource anfordert. Sie übergeben eine Ressourcen-ID, die die angeforderte Ressource angibt (z. B. einen Kanal, eine Folge usw.). Ihre App sucht zunächst nach einem gespeicherten Authentifizierungstoken. Wenn keine gefunden wird, starten Sie den Authentifizierungsprozess.

**AccessEnabler**

* `checkAuthorization()` - Prüft die Autorisierung, ohne den vollständigen Autorisierungsfluss zu initiieren. Dies wird häufig verwendet, um Statusinformationen zu aktualisieren, die in der Benutzeroberfläche der Programmierer-App angezeigt werden.

* `getAuthorization()` - Startet den vollständigen Autorisierungsfluss.

Sie stellen die folgenden Rückruffunktionen zur Verfügung, um die Ergebnisse von
Der Autorisierungsaufruf:

* `setToken()` - Wenn die Authentifizierung zuvor erfolgreich war und die Autorisierung erfolgreich war, ruft AccessEnabler Ihre `setToken()`-Rückruffunktion auf und übergibt das kurzlebige Medien-Token, was einen erfolgreichen Abschluss des Berechtigungsflusses für die Adobe Pass-Authentifizierung anzeigt. (Bevor der Benutzer geschützte Inhalte anzeigen kann, prüft die App des Programmierers die Gültigkeit des Medien-Tokens mithilfe des Media Token Verifier.

* `tokenRequestFailed()` - Wenn der Benutzer für die angeforderte Ressource nicht autorisiert ist (oder die Abfrage aus einem anderen Grund fehlschlägt), ruft AccessEnabler diese Rückruffunktion (sowie Ihre eigenen Fehlerberichterstattungsfunktionen) auf und gibt Details zum Fehler weiter.

**Clientless-API**

* `\<FQDN\>/.../authorize` - Startet den vollständigen Autorisierungsfluss.

#### Allgemeiner Autorisierungs-Workflow von Access Enabler {#generic-ae-authr-wf}

1. Geben Sie eine Rückruffunktion an, die Ihre zugewiesene Programmierer-GUID mithilfe von `setReqestor()` beim Access Enabler registriert. Diese Rückruffunktion wird aufgerufen, wenn der AccessEnabler
erfolgreich heruntergeladen.

1. Rufen Sie `getAuthorization()` auf, wenn ein Benutzer Zugriff auf eine geschützte Ressource anfordert. Übergeben Sie mithilfe von `getAuthorization()` eine Ressourcen-ID, die die angeforderte Ressource angibt (z. B. einen Kanal, eine Folge usw.). AccessEnabler sucht nach einem zwischengespeicherten Authentifizierungstoken, das mit der Autorisierungsanfrage übergeben werden soll. Wenn keine gefunden wird, wird der Authentifizierungsfluss initiiert.
1. Stellen Sie Callback-Funktionen bereit, um die Ergebnisse der Autorisierung zu verarbeiten:

   * `setToken()` - Wenn die Autorisierung erfolgreich ist oder der Benutzer zuvor autorisiert wurde, setzt Access Enabler den Autorisierungsprozess fort, indem es Ihre `setToken()` Callback-Funktion aufruft und das kurzlebige Autorisierungs-Token übergibt.

   * `tokenRequestFailed()` - Wenn der Benutzer für die angeforderte Ressource nicht autorisiert ist (oder die Abfrage aus einem anderen Grund fehlschlägt), ruft AccessEnabler alle von Ihnen registrierten Fehlerberichterstattungsfunktionen sowie den `tokenRequestFailed()`-Callback auf und gibt Details zum Fehler weiter.

### Abmeldefluss {#logout}

Löscht Token und andere Daten, die mit dem Berechtigungsfluss des aktuellen Benutzers verknüpft sind.

**AccessEnabler**

* `logout()`

**Clientless-API**

* `\<FQDN\>/.../logout`

## Verhalten von Access Enabler {#ae-behavior}

Alle AccessEnabler-API-Aufrufe sind asynchron (mit einer Ausnahme, die in den API-Referenzen angegeben ist). Sie können eine API beliebig oft aufrufen. Es gibt jedoch keine Garantie dafür, dass die Aktionen ausgelöst werden
durch die Aufrufe werden in der gleichen Reihenfolge abgeschlossen wie die Aufrufe. (Eine Ausnahme stellt die aktuelle Flash Player-Laufzeit dar. Da es sich nicht um Multithread-Anwendungen handelt, werden Aufrufe *do* in der richtigen Reihenfolge ausgeführt
Sie werden genannt.)

Um zwischen Antworten zu unterscheiden und Antworten mit Aufrufen zu paaren, senden alle Callbacks ein Echo auf ihre Eingabeparameter. Dazu gehören `setToken()` und`tokenRequestFailed()` die letztendlich durch `checkAuthorization()` ausgelöst werden. (Bei `checkAuthorization()` Callbacks wird die verwendete Ressource zurückechoziert.) Mithilfe dieser Funktion können Sie unterscheiden, welche Antwort welchem Aufruf entspricht. Um diese Funktion zu verwenden, können Sie etwas wie das Folgende codieren:

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### Häufig gestellte Fragen zum AEM-Verhalten {#ae-beh-faq}

**Frage. Was passiert, wenn ich einen zweiten AccessEnabler-Aufruf durchführe, bevor der erste Aufruf abgeschlossen ist?**

Der erste Aufruf wird weiterhin ausgeführt, während der zweite Aufruf ausgeführt wird (asynchrone Kommunikation).

**Frage. Gibt es eine maximale Anzahl gleichzeitiger Aufrufe, die AccessEnabler unterstützen kann?**

Im AccessEnabler-Code ist explizit keine Beschränkung festgelegt, sodass Sie nur durch Ihre verfügbaren Systemressourcen sowie durch die Kapazität von MVPD eingeschränkt sind.

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
