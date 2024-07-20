---
title: Berechtigungsfluss des Programmierers
description: Berechtigungsfluss des Programmierers
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# Berechtigungsfluss des Programmierers {#prog-entitlement-flow}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

In diesem Dokument wird der grundlegende Berechtigungsfluss aus der Perspektive des Programmierers beschrieben.  Informationen zu Funktionen und Anwendungsfällen, die über die hier behandelte grundlegende TVE-Integration hinausgehen, finden Sie unter [Anwendungsfälle für Programmierer](/help/authentication/programmer-use-cases.md).

Adobe Pass Authentication ermöglicht den Berechtigungsfluss zwischen Programmierern und MVPDs durch die Bereitstellung sicherer, konsistenter Schnittstellen für beide Seiten.  Auf der Seite des Programmierers bietet die Adobe Pass-Authentifizierung zwei allgemeine Arten von Berechtigungsschnittstellen:

1. AccessEnabler - Eine Client-Komponente, die eine Bibliothek mit APIs für Apps auf Geräten bereitstellt, die Web-Seiten rendern können (z. B. Web-Apps, Smartphone-/Tablet-Apps).
2. Clientlose API - RESTful-Webdienste für Geräte, die keine Webseiten rendern können (z. B. Set-Top-Boxen, Spielekonsolen, Smart-TVs). Die Anforderung, Web-Seiten zu rendern, entstammt der Anforderung des MVPD, dass sich Benutzer auf der MVPD-Website authentifizieren müssen.

Neben der hier dargestellten plattformneutralen Übersicht gibt es hier eine clientlose API-spezifische Übersicht: Clientlose API-Dokumentation. AccessEnabler wird nativ auf unterstützten Plattformen ausgeführt (AS/JS im Web, Objective-C unter iOS und Java unter Android). Die AccessEnabler-APIs sind auf allen unterstützten Plattformen konsistent. Alle Plattformen, die AccessEnabler nicht unterstützen, verwenden dieselbe Client-lose API.

Bei beiden Schnittstellentypen vermittelt die Adobe Pass-Authentifizierung den Berechtigungsfluss zwischen der App des Programmierers und dem MVPD des Benutzers sicher:

![](assets/prog-entitlement-flow.png)


*Abbildung: Adobe Pass-Authentifizierungs-Ökosystem*

>[!IMPORTANT]
>
>Beachten Sie im obigen Diagramm, dass es einen Teil des Berechtigungsflusses gibt, der nicht durch Adobe Pass-Authentifizierungsserver geleitet wird: die MVPD-Anmeldung. Benutzer müssen sich auf der Anmeldeseite ihres MVPD anmelden. Aufgrund dieser Anforderung muss die App des Programmierers Benutzer auf Geräten, die keine Web-Seiten rendern können, anweisen, zu einem Web-fähigen Gerät zu wechseln, um sich mit ihrem MVPD anzumelden. Danach müssen sie für den restlichen Berechtigungsfluss zum ursprünglichen Gerät zurückkehren.

## Berechtigungsfluss {#entitlement-flow}

Es gibt vier verschiedene Unterflüsse, aus denen die grundlegende Berechtigung besteht
Fluss:

1. [Startup-Fluss](/help/authentication/entitlement-flow.md#startup)
1. [Authentifizierungsfluss](/help/authentication/entitlement-flow.md#authentication)
1. [Autorisierungsfluss](/help/authentication/entitlement-flow.md#authorization)
1. [Logout FLow](/help/authentication/entitlement-flow.md#logout)

Beim ersten Besuch eines Benutzers auf der Website eines Programmierers wird der Berechtigungsfluss in der oben genannten Reihenfolge fortgesetzt. Bei nachfolgenden Besuchen, abhängig davon, ob die Authentifizierungs- und Autorisierungstoken abgelaufen sind oder nicht, oder abhängig von den Anzeigerichtlinien, durchläuft ein Benutzer möglicherweise nur einen oder zwei der Unterflüsse.

### Startup-Fluss {#startup}

Stellt die Identität des Programmierers und des Geräts her und führt Initialisierungsaufgaben aus. Dies ist eine Voraussetzung für alle nachfolgenden Berechtigungsaufrufe.

**AccessEnabler**

* **`setRequestor()`** - Stellt Ihre Identität mit dem AccessEnalber und damit mit den Adobe Pass-Authentifizierungsservern her. Dieser Aufruf ist ein Vorläufer zum Rest des Berechtigungsflusses. Beispiel in JavaScript:

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

**Clientlose API**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - Abhängig von der Plattform müssen möglicherweise vorbereitende Aufgaben ausgeführt werden, bevor Ihre App die Regcodierung aufruft. Weitere Informationen finden Sie in der Dokumentation zur **clientless API** . Beispielsweise müssen Sie bei den Xbox-Plattformen die vorgeschriebenen Sicherheitsschritte ausführen, bevor Sie regcode aufrufen.

### Authentifizierungsfluss {#authentication}

Bei erfolgreicher Authentifizierung wird ein AuthN-Token generiert, das an das Gerät und den Anforderer gebunden ist. Eine erfolgreiche Authentifizierung ist eine Voraussetzung für die Autorisierung.

**AccessEnabler**

* `checkAuthentication()` - Überprüft, ob ein gültiges zwischengespeichertes Authentifizierungstoken im lokalen Token-Cache vorhanden ist, ohne dass tatsächlich der vollständige Authentifizierungsfluss ausgelöst wird. Dadurch wird die Rückruffunktion `setAuthenticationStatus()` Trigger.
* `getAuthentication()` - Startet den vollständigen Authentifizierungsfluss. Bei erfolgreichem Abschluss generiert die Adobe Pass-Authentifizierung ein AuthN-Token und speichert es auf dem Client zwischen. Der Benutzer meldet sich auf seiner ausgewählten MVPDs-Site an, die je nach Plattform entweder in einem iFrame-, Popup-Fenster oder einer Webansicht angezeigt wird. Dadurch wird das displayProviderDialog() Trigger.

**Clientlose API**

* `<FQDN>/.../checkauthn` - Die Webdienstversion von `checkAuthentication()` oben.
* `<FQDN>/.../config` - Gibt die Liste der MVPDs an die App am 2. Bildschirm zurück.
* `<FQDN>/.../authenticate` - Startet den Authentifizierungsfluss von der 2. Bildschirmanwendung und leitet Benutzer zur Anmeldung zu ihrem ausgewählten MVPD weiter. Bei erfolgreichem Abschluss generiert die Adobe Pass-Authentifizierung ein AuthN-Token und speichert es auf dem Server. Der Benutzer kehrt dann zu seinem ursprünglichen Gerät zurück, um den Berechtigungsfluss abzuschließen.

Ein AuthN-Token gilt als gültig, wenn die folgenden beiden Punkte wahr sind:

* Das AuthN-Token ist nicht abgelaufen
* Der mit dem AuthN-Token verknüpfte MVPD befindet sich in der Liste der zulässigen MVPDs für die aktuelle Anforderer-ID

#### Allgemeiner Workflow für die anfängliche Authentifizierung von AccessEnabler {#generic-ae-initial-authn-flow}

1. Ihre App startet den Authentifizierungs-Workflow mit einem Aufruf von `getAuthentication()`, der nach einem gültigen zwischengespeicherten Authentifizierungstoken sucht. Diese Methode weist einen optionalen Parameter `redirectURL` auf. Wenn Sie keinen Wert für `redirectURL` angeben, wird der Benutzer nach einer erfolgreichen Authentifizierung an die URL zurückgegeben, von der aus die Authentifizierung initialisiert wurde.
1. Der AccessEnabler bestimmt den aktuellen Authentifizierungsstatus. Wenn der Benutzer derzeit authentifiziert ist, ruft der AccessEnabler Ihre `setAuthenticationStatus()` -Rückruffunktion auf und übergibt einen Authentifizierungsstatus, der auf den Erfolg hinweist.
1. Wenn der Benutzer nicht authentifiziert ist, setzt der AccessEnabler den Authentifizierungsfluss fort, indem er bestimmt, ob der letzte Authentifizierungsversuch des Benutzers mit einem bestimmten MVPD erfolgreich war. Wenn eine MVPD-ID zwischengespeichert wird UND das `canAuthenticate`-Flag wahr ist ODER ein MVPD mit `setSelectedProvider()` ausgewählt wurde, wird der Benutzer nicht mit dem MVPD-Auswahldialogfeld aufgefordert. Der Authentifizierungsfluss wird mit dem zwischengespeicherten Wert des MVPD fortgesetzt (d. h. mit dem MVPD, der bei der letzten erfolgreichen Authentifizierung verwendet wurde). Ein Netzwerkaufruf erfolgt an den Backend-Server und der Benutzer wird zur MVPD-Anmeldeseite weitergeleitet.

1. Wenn keine MVPD-ID zwischengespeichert wird UND kein MVPD mit `setSelectedProvider()` ausgewählt wurde ODER das Flag `canAuthenticate` auf &quot;false&quot;gesetzt ist, wird der Rückruf `displayProviderDialog()` aufgerufen. Dieser Rückruf weist Ihre App an, die Benutzeroberfläche zu erstellen, über die dem Benutzer eine Liste der MVPDs zur Auswahl angezeigt wird. Es wird ein Array von MVPD-Objekten bereitgestellt, die die erforderlichen Informationen zum Erstellen des MVPD-Selektors enthalten. Jedes MVPD-Objekt beschreibt eine MVPD-Entität und enthält Informationen wie die Kennung des MVPD und die URL, unter der das MVPD-Logo zu finden ist.

1. Nachdem ein MVPD ausgewählt wurde, muss Ihre App den AccessEnabler über die Auswahl des Benutzers informieren. Bei Nicht-Flash-Clients informieren Sie den AccessEnabler über die Benutzerauswahl, sobald der Benutzer den gewünschten MVPD auswählt, über einen Aufruf der `setSelectedProvider()` -Methode. Flash-Clients senden stattdessen eine freigegebene `MVPDEvent` vom Typ &quot;`mvpdSelection`&quot;, wobei der ausgewählte Provider übergeben wird.

1. Wenn der AccessEnabler über die MVPD-Auswahl des Benutzers informiert wird, wird ein Netzwerkaufruf an den Backend-Server gesendet und der Benutzer wird zur MVPD-Anmeldeseite weitergeleitet.

1. Im Authentifizierungs-Workflow kommuniziert der AccessEnabler mit der Adobe Pass-Authentifizierung und dem ausgewählten MVPD, um die Anmeldeinformationen des Benutzers (Benutzer-ID und Kennwort) abzurufen und seine Identität zu überprüfen. Während einige MVPDs zur Anmeldung zu ihrer eigenen Site weiterleiten, müssen Sie bei anderen ihre Anmeldeseite in einem iFrame anzeigen. Ihre Seite muss den Callback enthalten, der einen iFrame erstellt, falls der Kunde einen dieser MVPDs auswählt.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->

1. Sobald sich der Benutzer erfolgreich angemeldet hat, ruft der AccessEnabler das Authentifizierungstoken ab und informiert Ihre App darüber, dass der Authentifizierungsvorgang abgeschlossen ist. Der AccessEnabler ruft den Rückruf `setAuthenticationStatus()` mit dem Status-Code 1 auf und zeigt den Erfolg an. Wenn bei der Ausführung dieser Schritte ein Fehler auftritt, wird der Rückruf `setAuthenticationStatus()` mit dem Statuscode 0 ausgelöst, was auf einen Authentifizierungsfehler sowie einen entsprechenden Fehlercode hinweist.

>[!IMPORTANT]
>Comcast ist derzeit das einzige MVPD, das keine statische URL für das Logo bereitstellt. Programmierer sollten die aktuellsten Logos vom [XFINITY Developer&#39;s portal](https://developers.xfinity.com/products/tv-everywhere) abrufen.
>

### Autorisierungsfluss {#authorization}

Die Autorisierung ist eine Voraussetzung für die Anzeige geschützter Inhalte. Bei erfolgreicher Autorisierung wird ein AuthZ-Token zusammen mit einem kurzlebigen Media Token generiert, das der App des Programmierers aus Sicherheitsgründen bereitgestellt wird. Beachten Sie, dass Sie zur Unterstützung des Autorisierungs-Workflows zuvor die erforderliche Anfrageneinrichtung durchgeführt und den [Medien-Token-Verifikator](/help/authentication/media-token-verifier-int.md) integriert haben müssen. Nach Abschluss dieser Schritte können Sie die Autorisierung einleiten.

Ihre App initiiert eine Autorisierung, wenn ein Benutzer den Zugriff auf eine geschützte Ressource anfordert. Sie übergeben eine Ressourcen-ID, die die angeforderte Ressource angibt (z. B. einen Kanal, eine Folge usw.). Ihre App sucht zunächst nach einem gespeicherten Authentifizierungstoken. Wenn keine gefunden wird, starten Sie den Authentifizierungsprozess.

**AccessEnabler**

* `checkAuthorization()` - Prüft die Autorisierung, ohne den vollständigen Autorisierungsfluss zu starten. Dies wird häufig verwendet, um Statusinformationen zu aktualisieren, die in der Benutzeroberfläche der Programmierer-App angezeigt werden.

* `getAuthorization()` - Startet den vollständigen Autorisierungsfluss.

Sie stellen die folgenden Rückruffunktionen bereit, um die Ergebnisse von
den Autorisierungsaufruf:

* `setToken()` - Wenn die Authentifizierung zuvor erfolgreich war und die Autorisierung erfolgreich war, ruft der AccessEnabler Ihre `setToken()` -Rückruffunktion auf und übergibt das kurzlebige Medien-Token, was auf einen erfolgreichen Abschluss des Berechtigungsflusses für die Adobe Pass-Authentifizierung hinweist. (Bevor Benutzer geschützte Inhalte anzeigen können, prüft die App des Programmierers die Gültigkeit des Medien-Tokens mit dem Media Token Verifier.

* `tokenRequestFailed()` - Wenn der Benutzer für die angeforderte Ressource nicht autorisiert ist (oder die Abfrage aus einem anderen Grund fehlschlägt), ruft AccessEnabler diese Rückruffunktion (sowie Ihre eigenen Fehlerberichtsfunktionen) auf und übergibt Details zum Fehler.

**Clientlose API**

* `\<FQDN\>/.../authorize` - Startet den vollständigen Autorisierungsfluss.

#### Generischer AccessEnabler Authorization-Workflow {#generic-ae-authr-wf}

1. Stellen Sie mithilfe von `setReqestor()` eine Callback-Funktion bereit, die Ihre zugewiesene Programmierer-GUID beim Access Enabler registriert. Diese Rückruffunktion wird aufgerufen, wenn der AccessEnabler
erfolgreich heruntergeladen wurde.

1. Rufen Sie `getAuthorization()` auf, wenn ein Benutzer den Zugriff auf eine geschützte Ressource anfordert. Übergeben Sie mit &quot;`getAuthorization()`&quot; eine Ressourcen-ID, mit der die angeforderte Ressource spezifiziert wird (z. B. einen Kanal, eine Folge usw.). Der AccessEnabler sucht nach einem zwischengespeicherten Authentifizierungstoken, das mit der Autorisierungsanforderung übergeben wird. Wenn keine gefunden wird, wird der Authentifizierungsfluss initiiert.
1. Stellen Sie Callback-Funktionen bereit, um die Ergebnisse der Autorisierung zu verarbeiten:

   * `setToken()` - Wenn die Autorisierung erfolgreich ist oder der Benutzer zuvor autorisiert wurde, setzt der Access Enabler den Autorisierungsprozess fort, indem er Ihre `setToken()` -Callback-Funktion aufruft und das kurzlebige Autorisierungstoken übergibt.

   * `tokenRequestFailed()` - Wenn der Benutzer für die angeforderte Ressource nicht autorisiert ist (oder die Abfrage aus einem anderen Grund fehlschlägt), ruft AccessEnabler alle von Ihnen registrierten Fehlerberichtsfunktionen sowie den `tokenRequestFailed()` -Rückruf auf und übergibt Details zum Fehler.

### Abmeldefluss {#logout}

Löscht Token und andere mit dem Berechtigungsfluss des aktuellen Benutzers verknüpfte Daten.

**AccessEnabler**

* `logout()`

**Clientlose API**

* `\<FQDN\>/.../logout`

## Verhalten von AccessEnabler {#ae-behavior}

Alle AccessEnabler-API-Aufrufe sind asynchron (mit einer Ausnahme, die in den API-Referenzen angegeben ist). Sie können eine API beliebig oft aufrufen, es gibt jedoch keine starke Garantie dafür, dass die Aktionen ausgelöst werden
durch die Aufrufe in derselben Reihenfolge abgeschlossen werden, in der die Aufrufe getätigt wurden. (Eine Ausnahme hiervon ist die aktuelle Flash Player-Laufzeitumgebung. Wenn nicht mehrere Threads verwendet werden, wird sichergestellt, dass die Aufrufe *do* in der Reihenfolge abgeschlossen werden
sie werden aufgerufen.)

Um zwischen Antworten zu unterscheiden und Antworten mit Aufrufen zu kombinieren, wiederholen alle Rückrufe ihre Eingabeparameter. Dazu gehören `setToken()` und `tokenRequestFailed()`, die letztendlich durch `checkAuthorization()` ausgelöst werden. (Bei `checkAuthorization()` -Rückrufen wird die verwendete Ressource zurückverfolgt.) Mithilfe dieser Funktion können Sie ermitteln, welche Antwort dem jeweiligen Aufruf entspricht. Um diese Funktion verwenden zu können, können Sie etwa Folgendes kodieren:

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

### Häufig gestellte Fragen zum Verhalten von AEM {#ae-beh-faq}

**Frage. Was passiert, wenn ich einen zweiten AccessEnabler-Aufruf tätige, bevor der erste Aufruf abgeschlossen ist?**

Der erste Aufruf wird weiterhin ausgeführt, während der zweite Aufruf ausgeführt wird (asynchrone Kommunikation).

**Frage. Gibt es eine maximale Anzahl gleichzeitiger Aufrufe, die AccessEnabler unterstützen kann?**

Im AccessEnabler-Code wird keine Beschränkung explizit festgelegt, sodass Sie nur durch Ihre verfügbaren Systemressourcen und die Kapazität des MVPD eingeschränkt sind.

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
