---
title: Android SDK-Cookbook
description: Android SDK-Cookbook
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 0%

---

# Android SDK-Cookbook {#android-sdk-cookbook}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Einführung {#intro}

In diesem Dokument werden die Berechtigungs-Workflows beschrieben, die die Anwendung eines Programmierers auf oberster Ebene über die APIs implementieren kann, die von der Android AccessEnabler-Bibliothek bereitgestellt werden.


Die Berechtigungslösung für die Adobe Pass-Authentifizierung für Android ist letztendlich in zwei Domänen unterteilt:

- Die UI-Domäne - dies ist die Anwendungsebene der obersten Ebene, die die Benutzeroberfläche implementiert und die von der AccessEnabler-Bibliothek bereitgestellten Dienste verwendet, um Zugriff auf eingeschränkte Inhalte zu ermöglichen.
- In der AccessEnabler-Domäne werden die Berechtigungs-Workflows in folgender Form implementiert:
   - Netzwerkaufrufe an Adobe-Backend-Server
   - Geschäftslogikregeln für die Authentifizierungs- und Autorisierungs-Workflows
   - Verwaltung verschiedener Ressourcen und Verarbeitung des Workflow-Status (z. B. Token-Cache)

Ziel der AccessEnabler-Domäne ist es, alle Komplexität der Berechtigungs-Workflows auszublenden und der oberen Ebene (über die AccessEnabler-Bibliothek) eine Reihe einfacher Berechtigungs-Primitive bereitzustellen, mit denen Sie die Berechtigungs-Workflows implementieren:

1. Legen Sie die Anfragenkennung fest.

1. Überprüfen Sie die Authentifizierung für einen bestimmten Identitäts-Provider und rufen Sie ihn ab.

1. Überprüfen Sie die Autorisierung für eine bestimmte Ressource und erhalten Sie sie.

1. Abmelden.

Die Netzwerkaktivität von AccessEnabler erfolgt in einem anderen Thread, sodass der UI-Thread nie blockiert wird. Daher muss der bidirektionale Kommunikationskanal zwischen den beiden Anwendungsdomänen einem vollständig asynchronen Muster folgen:

- Die Anwendungsebene der Benutzeroberfläche sendet Nachrichten über die API-Aufrufe, die von der AccessEnabler-Bibliothek verfügbar gemacht werden, an die AccessEnabler-Domäne.
- Der AccessEnabler antwortet auf die UI-Ebene über die Callback-Methoden, die im AccessEnabler -Protokoll enthalten sind und die die UI-Ebene mit der AccessEnabler -Bibliothek registriert.

## Berechtigungsflüsse {#entitlement}

1. [Voraussetzungen](#prereqs)
1. [Startup-Fluss](#startup_flow)
1. [Authentifizierungsfluss](#authn_flow)
1. [Autorisierungsfluss](#authz_flow)
1. [Medienfluss anzeigen](#media_flow)
1. [Abmeldefluss](#logout_flow)

### A. Voraussetzungen {#prereqs}

1. Erstellen Sie Ihre Callback-Funktionen:
   - [`setRequestorComplete()`](#$setRequestorComplete)

     Wird durch `setRequestor()` ausgelöst und gibt Erfolg oder Fehler zurück.\
     &quot;Erfolg&quot;bedeutet, dass Sie mit Berechtigungsaufrufen fortfahren können.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

     Wird von `getAuthentication()` nur ausgelöst, wenn der Benutzer keinen Anbieter (MVPD) ausgewählt hat und noch nicht authentifiziert ist.\
     Der Parameter `mvpds` ist ein Array von Anbietern, die dem Benutzer zur Verfügung stehen.

   - [`setAuthenticationStatus(status, errorcode)`](#$setAuthNStatus)

     Wird jedes Mal von `checkAuthentication()` ausgelöst.\
     Wird nur von `getAuthentication()` ausgelöst, wenn der Benutzer bereits authentifiziert ist und einen Provider ausgewählt hat.

     Status zurückgegeben ist erfolgreich oder fehlgeschlagen, der Fehlertyp wird im Fehlercode beschrieben.

   - [navigateToUrl(url)](#$navigateToUrl)

     Wird durch `getAuthentication()` ausgelöst, nachdem der Benutzer einen MVPD ausgewählt hat. Der Parameter `url` stellt den Speicherort der Anmeldeseite des MVPD bereit.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

     Wird durch `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()` ausgelöst.\
     Der Parameter `event` gibt an, welches Berechtigungsereignis aufgetreten ist. Der Parameter `data` ist eine Liste von Werten, die sich auf das Ereignis beziehen.

   - [`setToken(token, resource)`](#$setToken)

     Wird von `checkAuthorization()` und `getAuthorization()` nach einer erfolgreichen Autorisierung zum Anzeigen einer Ressource ausgelöst.\
     Der Parameter `token` ist das kurzlebige Medien-Token. Der Parameter `resource` ist der Inhalt, den der Benutzer anzeigen darf.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

     Wird von `checkAuthorization()` und `getAuthorization()` nach einer nicht erfolgreichen Autorisierung ausgelöst.\
     Der Parameter `resource` ist der Inhalt, den der Benutzer anzeigen wollte. Der Parameter `code` ist der Fehlercode, der angibt, welcher Fehlertyp aufgetreten ist. Der Parameter `description` beschreibt den Fehler, der mit dem Fehlercode verknüpft ist.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

     Wird durch `getSelectedProvider()` ausgelöst.\
     Der Parameter `mvpd` enthält Informationen zum vom Benutzer ausgewählten Provider.

   - [`setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

     Wird durch `getMetadata().` ausgelöst\
     Der Parameter `metadata` stellt die spezifischen Daten bereit, die Sie angefordert haben. Der Parameter `key` ist der Schlüssel, der in der `getMetadata()` -Anfrage verwendet wird, und der Parameter `arguments` ist dasselbe Wörterbuch, das an `getMetadata()` übergeben wurde.

   - [`preauthorizedResources(resources)`](#$preauthResources)

     Wird durch `checkPreauthorizedResources()` ausgelöst.\
     Der Parameter `authorizedResources` zeigt die Ressourcen an, die der Benutzer anzeigen darf.


![](../../../../assets/android-entitlement-flows.png)


### B. Startup Flow {#startup_flow}

1. Starten Sie die Anwendung der obersten Ebene.
1. Adobe Pass-Authentifizierung initiieren

   a. Rufen Sie [`getInstance`](#$getInstance) auf, um eine einzelne Instanz von Adobe Pass Authentication AccessEnabler zu erstellen.

   - **Abhängigkeit:** Native Adobe Pass-Authentifizierung
Android Library (AccessEnabler)

   b. Rufen Sie` setRequestor()` auf, um die Identität des Programmierers zu ermitteln. Geben Sie die `requestorID` des Programmierers und (optional) ein Array von Adobe Pass-Authentifizierungsendpunkten an.

   - **Abhängigkeit:** Gültige Adobe Pass-Authentifizierungsanfrage-ID\
     (Wenden Sie sich an Ihren Adobe Pass Authentication Account Manager, um dies zu arrangieren.)

   - **Trigger:** setRequestorComplete()-Rückruf

   | NOTE |     |
   | --- | --- |  
   | ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) | Berechtigungsanfragen können erst abgeschlossen werden, wenn die Identität des Anfragenden vollständig ermittelt wurde. Dies bedeutet effektiv, dass, während setRequestor() noch ausgeführt wird, alle nachfolgenden Berechtigungsanfragen (z. B. `checkAuthentication()`) blockiert werden.<br><br>Sie haben zwei Implementierungsoptionen: Sobald die Identifizierungsinformationen des Anforderers an den Backend-Server gesendet wurden, kann die UI-Anwendungsebene einen der beiden folgenden Ansätze wählen:<br><br>1.  Warten Sie auf die Auslösung des `setRequestorComplete()` -Rückrufs (Teil des AccessEnabler -Delegats).  Diese Option bietet die höchste Sicherheit, dass `setRequestor()` abgeschlossen ist. Daher wird sie für die meisten Implementierungen empfohlen.<br>2.  Fahren Sie fort, ohne auf das Auslösen des `setRequestorComplete()` -Rückrufs zu warten, und beginnen Sie mit der Ausgabe von Berechtigungsanfragen. Diese Aufrufe (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) werden von der AccessEnabler-Bibliothek in die Warteschlange gestellt, die die tatsächlichen Netzwerkaufrufe nach der `setRequestor(). `Diese Option kann gelegentlich unterbrochen werden, wenn beispielsweise die Netzwerkverbindung instabil ist. |

1. Rufen Sie [checkAuthentication()](#$checkAuthN) auf, um nach einer vorhandenen Authentifizierung zu suchen, ohne den vollständigen Authentifizierungsfluss zu starten.   Wenn dieser Aufruf erfolgreich ist, können Sie direkt zum Autorisierungsfluss übergehen.  Ist dies nicht der Fall, fahren Sie mit dem Authentifizierungsfluss fort.

   - **Abhängigkeit:** Ein erfolgreicher Aufruf von `setRequestor()` (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).

   - **Trigger:** setAuthenticationStatus()-Rückruf

### C. Authentifizierungsfluss {#authn_flow}

1. Rufen Sie [`getAuthentication()`](#$getAuthN) auf, um den Authentifizierungsfluss zu initiieren oder um zu bestätigen, dass der Benutzer bereits authentifiziert ist.\
   **Trigger:**
   - Der Rückruf setAuthenticationStatus() , wenn der Benutzer bereits authentifiziert ist.  Fahren Sie in diesem Fall direkt mit dem Autorisierungsfluss [ fort.](#authz_flow)
   - Der Rückruf displayProviderDialog() , wenn der Benutzer noch nicht authentifiziert ist.

1. Präsentieren Sie den Benutzer mit der Liste der Provider, die an `displayProviderDialog()` gesendet werden.

1. Nachdem der Benutzer einen Anbieter ausgewählt hat, rufen Sie die URL des MVPD des Benutzers aus dem Rückruf `navigateToUrl()` ab.  Öffnen Sie eine WebView und leiten Sie dieses WebView-Steuerelement an die URL weiter.

1. Über die im vorherigen Schritt instanziierte WebView gelangt der Benutzer auf die Anmeldeseite des MVPD und gibt Anmeldedaten ein. In WebView finden verschiedene Umleitungsvorgänge statt.

   **Hinweis:** Zu diesem Zeitpunkt hat der Benutzer die Möglichkeit, den Authentifizierungsfluss abzubrechen. In diesem Fall ist Ihre UI-Schicht dafür verantwortlich, den AccessEnabler über dieses Ereignis zu informieren, indem sie `setSelectedProvider()` mit `null` als Parameter aufruft. Dadurch kann AccessEnabler den internen Status bereinigen und den Authentifizierungsfluss zurücksetzen.

1. Nach erfolgreicher Anmeldung durch den Benutzer erkennt Ihre Anwendungsebene das Laden einer &quot;benutzerdefinierten Umleitungs-URL&quot;(d. h.: `http://adobepass.android.app`). Diese benutzerdefinierte URL ist tatsächlich eine ungültige URL, die nicht von WebView geladen werden soll. Es ist ein Signal, dass der Authentifizierungsfluss abgeschlossen ist und die WebView geschlossen werden muss.

1. Schließen Sie das WebView-Steuerelement und rufen Sie `getAuthenticationToken()` auf, was den AccessEnabler anweist, das Authentifizierungstoken vom Backend-Server abzurufen.

1. [Optional] Rufen Sie [`checkPreauthorizedResources(resources)`](#$checkPreauth) auf, um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Der Parameter `resources` ist ein Array geschützter Ressourcen, die mit dem Authentifizierungstoken des Benutzers verknüpft sind.\
   **Trigger:** `preAuthorizedResources()` callback\
   **Ausführungspunkt:** Nach Abschluss des Authentifizierungsablaufs

1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.



### D. Genehmigungsprozess {#authz_flow}

1. Rufen Sie [getAuthorization()](#$getAuthZ) auf, um die Autorisierung zu initiieren.
Fluss.

   Abhängigkeit: Gültige ResourceID(s), die mit den MVPD(s) vereinbart wurde.

   **Hinweis:** ResourceIDs sollten mit denen auf anderen Geräten oder Plattformen übereinstimmen und über MVPDs hinweg identisch sein.

1. Validieren Sie Authentifizierung und Autorisierung.

   - Wenn der `getAuthorization()` -Aufruf erfolgreich ist: Der Benutzer verfügt über gültige AuthN- und AuthZ-Token (der Benutzer ist authentifiziert und berechtigt, die angeforderten Medien zu sehen).
   - Wenn `getAuthorization()` fehlschlägt: Untersuchen Sie die ausgelöste Ausnahme, um ihren Typ zu bestimmen (AuthN, AuthZ oder etwas Anderes):
      - Wenn es sich um einen Authentifizierungsfehler (AuthN) handelte, starten Sie den Authentifizierungsfluss neu.
      - Wenn es sich um einen Autorisierungsfehler (AuthZ) handelt, ist der Benutzer nicht berechtigt, das angeforderte Medium zu sehen und dem Benutzer sollte eine Fehlermeldung angezeigt werden.
      - Wenn ein anderer Fehlertyp aufgetreten ist (Verbindungsfehler, Netzwerkfehler usw.), zeigen Sie dem Benutzer eine entsprechende Fehlermeldung an.

1. Validieren Sie das Token für kurze Medien.\
   Verwenden Sie die Adobe Pass Authentication Media Token Verifier-Bibliothek, um das vom obigen `getAuthorization()`-Aufruf zurückgegebene kurzlebige Medien-Token zu überprüfen:

   - Wenn die Validierung erfolgreich ist: Wiedergabe des angeforderten Mediums für den Benutzer.
   - Wenn die Validierung fehlschlägt: Das AuthZ-Token war ungültig, die Medienanforderung sollte abgelehnt werden und dem Benutzer sollte eine Fehlermeldung angezeigt werden.

1. Kehren Sie zu Ihrem normalen Anwendungsfluss zurück.

### E. Medienfluss anzeigen {#media_flow}

1. Der Benutzer wählt das Medium aus, das angezeigt werden soll.
2. Sind die Medien geschützt?  Ihre Anwendung prüft, ob die ausgewählten Medien geschützt sind:
- Wenn das ausgewählte Medium geschützt ist, startet Ihre Anwendung den obigen [Autorisierungsfluss](#authz_flow).
- Wenn das ausgewählte Medium nicht geschützt ist, geben Sie das Medium für den Benutzer wieder.



### F. Abmeldefluss {#logout_flow}

1. Rufen Sie [`logout()`](#$logout) auf, um den Benutzer abzumelden.\
   AccessEnabler löscht alle zwischengespeicherten Werte und Token für das aktuelle MVPD für den aktuellen Anfragenden und auch für Anfragende mit Single Sign On. Nachdem der Cache gelöscht wurde, führt der AccessEnabler einen Server-Aufruf durch, um die Server-seitigen Sitzungen zu bereinigen.  Da der Server-Aufruf zu einer SAML-Umleitung zum IdP führen kann (dies ermöglicht die Sitzungsbereinigung auf der IdP-Seite), muss dieser Aufruf allen Umleitungen folgen. Aus diesem Grund muss dieser Aufruf innerhalb eines WebView-Steuerelements verarbeitet werden.

   a. Entsprechend demselben Muster wie der Authentifizierungs-Workflow sendet die AccessEnabler-Domäne eine Anfrage an die UI-Anwendungsebene (über den Rückruf `navigateToUrl()` ), um ein WebView-Steuerelement zu erstellen und dieses Steuerelement anzuweisen, die URL des Abmelde-Endpunkts auf den Backend-Server zu laden.

   b. Auch hier muss die Benutzeroberfläche die Aktivität des WebView-Steuerelements überwachen und den Zeitpunkt erkennen, zu dem das Steuerelement während mehrerer Umleitungen die benutzerdefinierte URL der Anwendung lädt (d. h.: `http://adobepass.android.app/`). Sobald dieses Ereignis stattfindet, schließt die UI-Anwendungsebene die WebView und der Abmeldevorgang ist abgeschlossen.

   **Hinweis:** Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss insofern, als der Benutzer in keiner Weise mit der WebView interagieren muss. Die UI-Anwendungsebene verwendet eine WebView, um sicherzustellen, dass alle Umleitungen befolgt werden. Daher ist es möglich (und empfohlen), das WebView-Steuerelement während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.



### Benutzerfluss für die Anmeldung mit mehreren MVPDs und Abmeldung {#user_flows}

[Hier](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf) haben Sie ein Dokument, in dem das Verhalten bei der Verwendung mehrerer MVPDs beschrieben wird und was passiert, wenn sich der Benutzer von einer Anwendung abmeldet.

Das beschriebene Verhalten ist bei Verwendung von Android SDK-Version >= 2.0.0 verfügbar.
