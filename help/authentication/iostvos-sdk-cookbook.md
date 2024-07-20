---
title: iOS/tvOS-Cookbook
description: iOS/tvOS-Cookbook
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2402'
ht-degree: 0%

---

# iOS/tvOS-SDK-Cookbook {#iostvos-sdk-cookbook}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#intro}

In diesem Dokument werden die Berechtigungs-Workflows beschrieben, die die Anwendung eines Programmierers auf oberster Ebene über die APIs implementieren kann, die von der iOS/tvOS AccessEnabler-Bibliothek bereitgestellt werden.

Die Berechtigungslösung für die Adobe Pass-Authentifizierung für iOS/tvOS ist letztendlich in zwei Domänen unterteilt:

* Die UI-Domäne - dies ist die Anwendungsebene der obersten Ebene, die die Benutzeroberfläche implementiert und die von der AccessEnabler-Bibliothek bereitgestellten Dienste verwendet, um Zugriff auf eingeschränkte Inhalte zu ermöglichen.

* In der AccessEnabler-Domäne werden die Berechtigungs-Workflows in folgender Form implementiert:

   * Netzwerkaufrufe an Adobe-Backend-Server
   * Geschäftslogikregeln für die Authentifizierungs- und Autorisierungs-Workflows
   * Verwaltung verschiedener Ressourcen und Verarbeitung des Workflow-Status (z. B. Token-Cache)

Ziel der AccessEnabler-Domäne ist es, alle Komplexität der Berechtigungs-Workflows auszublenden und der oberen Ebene (über die AccessEnabler-Bibliothek) eine Reihe einfacher Berechtigungs-Primitive bereitzustellen, mit denen Sie die Berechtigungs-Workflows implementieren:

1. Anfragenidentität festlegen
1. Überprüfen und Abrufen der Authentifizierung für einen bestimmten Identitäts-Provider
1. Prüfen und Autorisieren einer bestimmten Ressource
1. Abmelden
1. Apple SSO-Flüsse durch Testen des Apple VSA-Frameworks

Die Netzwerkaktivität von AccessEnabler erfolgt in einem eigenen Thread, sodass der UI-Thread nie blockiert wird. Daher muss der bidirektionale Kommunikationskanal zwischen den beiden Anwendungsdomänen einem vollständig asynchronen Muster folgen:

* Die Anwendungsebene der Benutzeroberfläche sendet Nachrichten über die API-Aufrufe, die von der AccessEnabler-Bibliothek verfügbar gemacht werden, an die AccessEnabler-Domäne.
* Der AccessEnabler antwortet auf die UI-Ebene über die Callback-Methoden, die im AccessEnabler -Protokoll enthalten sind und die die UI-Ebene mit der AccessEnabler -Bibliothek registriert.

## Konfigurieren des Experience Cloud ID-Diensts (Besucher-ID) {#visitorIDSetup}

Die Konfiguration des Werts [Experience Cloud ID](https://experienceleague.adobe.com/docs/id-service/using/home.html) ist aus Sicht von [!DNL Analytics] wichtig. Sobald ein `visitorID` -Wert festgelegt ist, sendet das SDK diese Informationen zusammen mit jedem Netzwerkaufruf und der [!DNL Adobe Pass]-Authentifizierungsserver erfasst diese Informationen. Sie können die Analysen des Adobe Pass-Authentifizierungsdienstes mit allen anderen Analyseberichten korrelieren, die Sie möglicherweise aus anderen Anwendungen oder Websites haben. Informationen zum Einrichten der visitorID finden Sie [hier](#setOptions).

## Berechtigungsflüsse {#entitlement}

A. [Voraussetzungen](#prereqs) </br>
B. [Startup Flow](#startup_flow) </br>
C. [Authentifizierungsfluss ohne Apple SSO](#authn_flow_wo_applesso) </br>
D. [Authentifizierungsfluss mit Apple SSO auf iOS](#authn_flow_with_applesso) </br>
E. [Authentifizierungsfluss mit Apple SSO auf tvOS](#authn_flow_with_applesso_tvOS) </br>
F. [Autorisierungsfluss](#authz_flow) </br>
G. [Medienfluss anzeigen](#media_flow) </br>
H. [Abmeldefluss ohne Apple SSO](#logout_flow_wo_AppleSSO) </br>
I. [Abmeldefluss mit Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Voraussetzungen {#prereqs}

1. Erstellen Sie Ihre Callback-Funktionen:
   * `setRequestorComplete()` </br>
   * Wird durch [setRequestor()](#$setReq) ausgelöst und gibt Erfolg oder Fehler zurück. </br>
   * &quot;Erfolg&quot;bedeutet, dass Sie mit Berechtigungsaufrufen fortfahren können.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Wird von [`getAuthentication()`](#$getAuthN) nur ausgelöst, wenn der Benutzer keinen Anbieter (MVPD) ausgewählt hat und noch nicht authentifiziert ist. </br>
      * Der Parameter `mvpds` ist ein Array von Anbietern, die dem Benutzer zur Verfügung stehen.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Wird jedes Mal von `checkAuthentication()` ausgelöst. </br>
      * Wird nur von [`getAuthentication()`](#$getAuthN) ausgelöst, wenn der Benutzer bereits authentifiziert ist und einen Provider ausgewählt hat. </br>
      * Status zurückgegeben ist erfolgreich oder fehlgeschlagen, der Fehlertyp wird im Fehlercode beschrieben.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Wird durch [`getAuthentication()`](#$getAuthN) ausgelöst, nachdem der Benutzer einen MVPD ausgewählt hat. Der Parameter `url` stellt den Speicherort der Anmeldeseite des MVPD bereit.

   * `sendTrackingData(event, data)` </br>
      * Wird durch `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()` ausgelöst.
      * Der Parameter `event` gibt an, welches Berechtigungsereignis aufgetreten ist. Der Parameter `data` ist eine Liste von Werten, die sich auf das Ereignis beziehen.

   * `setToken(token, resource)`

      * Wird von [checkAuthorization()](#checkAuthZ) und [getAuthorization()](#$getAuthZ) nach einer erfolgreichen Autorisierung zum Anzeigen einer Ressource ausgelöst.
      * Der Parameter `token` ist das kurzlebige Medien-Token. Der Parameter `resource` ist der Inhalt, den der Benutzer anzeigen darf.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Wird von [checkAuthorization()](#checkAuthZ) und [getAuthorization()](#$getAuthZ) nach einer nicht erfolgreichen Autorisierung ausgelöst.
      * Der Parameter `resource` ist der Inhalt, den der Benutzer anzeigen wollte. Der Parameter `code` ist der Fehlercode, der angibt, welcher Fehlertyp aufgetreten ist. Der Parameter `description` beschreibt den Fehler, der mit dem Fehlercode verknüpft ist.

   * `selectedProvider(mvpd)` </br>
      * Wird durch [`getSelectedProvider()`](#getSelProv) ausgelöst.
      * Der Parameter `mvpd` enthält Informationen zum vom Benutzer ausgewählten Provider.

   * `setMetadataStatus(metadata, key, arguments)`
      * Wird durch `getMetadata().` ausgelöst
      * Der Parameter `metadata` stellt die spezifischen Daten bereit, die Sie angefordert haben. Der Parameter `key` ist der Schlüssel, der in der Anforderung [getMetadata()](#getMeta) verwendet wird, und der Parameter `arguments` ist dasselbe Wörterbuch, das an [getMetadata()](#getMeta) übergeben wurde.

   * [`preauthorizedResources(authorizedResources]`](#preauthResources)

      * Wird durch [`checkPreauthorizedResources()`](#checkPreauth) ausgelöst.

      * Der Parameter `authorizedResources` stellt die Ressourcen dar, die der Benutzer
ist zur Ansicht berechtigt.

   * [`presentTvProviderDialog(viewController)`](#presentTvDialog)

      * Wird von [getAuthentication()](#getAuthN) ausgelöst, wenn der aktuelle Anforderer mindestens MVPD unterstützt, das SSO-Unterstützung besitzt.
      * Der Parameter viewController ist das Apple SSO-Dialogfeld und muss auf dem Hauptansichtscontroller angezeigt werden.

   * [`dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Wird durch eine Benutzeraktion ausgelöst (durch die Auswahl von &quot;Abbrechen&quot;oder &quot;Andere TV-Anbieter&quot;im Dialogfeld &quot;Apple SSO&quot;).
      * Der Parameter viewController ist das Apple SSO-Dialogfeld und muss vom Hauptansichtscontroller verworfen werden.

![](assets/iOS-flows.png)

### B. Startup Flow {#startup_flow}

1. Starten Sie die Anwendung der obersten Ebene.</br>
1. Adobe Pass-Authentifizierung starten </br>

   a. Rufen Sie [`init`](#$init) auf, um eine einzelne Instanz von Adobe Pass Authentication AccessEnabler zu erstellen.
   * **Abhängigkeit:** Native iOS/tvOS-Bibliothek für Adobe Pass-Authentifizierung (AccessEnabler)

   b. Rufen Sie `setRequestor()` auf, um die Identität des Programmierers festzulegen. Geben Sie die `requestorID` des Programmierers und (optional) ein Array von Adobe Pass-Authentifizierungsendpunkten an. Für tvOS müssen Sie auch den öffentlichen Schlüssel und das Geheimnis angeben. Weitere Informationen finden Sie unter [Clientlose Dokumentation](#create_dev) .

   * **Abhängigkeit:** Gültige Adobe Pass Authentication RequestID (Work with your Adobe Pass Authentication Account)
Manager, um dies anzuordnen).

   * **Trigger:**
     [setRequestComplete()](#$setReqComplete) -Rückruf.

   >[!NOTE]
   >
   >Berechtigungsanfragen können erst abgeschlossen werden, wenn die Identität des Anfragenden vollständig ermittelt wurde. Dies bedeutet effektiv, dass während [`setRequestor()`](#$setReq) noch ausgeführt wird, alle nachfolgenden Berechtigungsanfragen ausgeführt werden. Beispielsweise sind [`checkAuthentication()`](#checkAuthN) blockiert.

   Sie haben zwei Implementierungsoptionen: Sobald die Identifizierungsinformationen des Anforderers an den Backend-Server gesendet wurden, kann die UI-Anwendungsschicht einen der beiden folgenden Ansätze wählen: </br>

   1. Warten Sie auf die Auslösung des [`setRequestorComplete()`](#setReqComplete) -Rückrufs (Teil des AccessEnabler -Delegats). Diese Option bietet die höchste Sicherheit, dass [`setRequestor()`](#$setReq) abgeschlossen ist. Daher wird sie für die meisten Implementierungen empfohlen.

   1. Fahren Sie fort, ohne auf das Auslösen des [`setRequestorComplete()`](#setReqComplete) -Rückrufs zu warten, und beginnen Sie mit der Ausgabe von Berechtigungsanfragen. Diese Aufrufe (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) werden von der AccessEnabler-Bibliothek in die Warteschlange gestellt, die die tatsächlichen Netzwerkaufrufe nach dem [`setRequestor()`](#$setReq) durchführt. Diese Option kann gelegentlich unterbrochen werden, wenn beispielsweise die Netzwerkverbindung instabil ist.

1. Rufen Sie `checkAuthentication()` auf, um nach einer vorhandenen Authentifizierung zu suchen, ohne den vollständigen Authentifizierungsfluss zu starten.  Wenn dieser Aufruf erfolgreich ist, können Sie direkt zum Autorisierungsfluss übergehen. Ist dies nicht der Fall, fahren Sie mit dem Authentifizierungsfluss fort.

   * **Abhängigkeit:** Ein erfolgreicher Aufruf von [setRequestor()](#$setReq) (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).

   * **Trigger:** [setAuthenticationStatus()](#$setAuthNStatus) -Rückruf.


### C. Authentifizierungsfluss ohne Apple SSO {#authn_flow_wo_applesso}

1. Rufen Sie [`getAuthentication()`](#$getAuthN) auf, um den Authentifizierungsfluss zu initiieren oder zu bestätigen, dass der Benutzer bereits
authentifiziert.

   **Trigger:**

   * Der Rückruf [setAuthenticationStatus()](#$setAuthNStatus) , wenn der Benutzer bereits authentifiziert ist. Fahren Sie in diesem Fall direkt mit dem Autorisierungsfluss [ fort.](#authz_flow)

   * Der Rückruf [displayProviderDialog()](#$dispProvDialog) , wenn der Benutzer noch nicht authentifiziert ist.

1. Präsentieren Sie den Benutzer mit der Liste der Anbieter, die an gesendet werden
   [`displayProviderDialog()`](#dispProvDialog).

1. Nachdem der Benutzer einen Anbieter ausgewählt hat, rufen Sie die URL des MVPD des Benutzers aus dem Rückruf `navigateToUrl:` oder `navigateToUrl:useSVC:` ab, öffnen Sie einen Controller `UIWebView/WKWebView` oder `SFSafariViewController` und leiten Sie diesen Controller an die URL weiter.

1. Durch die im vorherigen Schritt instanziierten `UIWebView/WKWebView` oder `SFSafariViewController` landet der Benutzer auf der Anmeldeseite des MVPD und gibt Anmeldedaten ein. Mehrere Umleitungsvorgänge finden innerhalb des Controllers statt.</br>

>[!NOTE]
>
>An dieser Stelle hat der Benutzer die Möglichkeit, den Authentifizierungsfluss abzubrechen. In diesem Fall ist Ihre UI-Schicht dafür verantwortlich, den AccessEnabler über dieses Ereignis zu informieren, indem sie [setSelectedProvider()](#setSelProv) mit `null` als Parameter aufruft. Dadurch kann AccessEnabler den internen Status bereinigen und den Authentifizierungsfluss zurücksetzen.

1. Nach erfolgreicher Anmeldung durch den Benutzer erkennt Ihre Anwendungsebene das Laden einer bestimmten benutzerdefinierten URL. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Authentifizierungsfluss abgeschlossen ist und dass es sicher ist, den Controller `UIWebView/WKWebView` oder `SFSafariViewController` zu schließen. Wenn ein `SFSafariViewController`Controller verwendet werden muss, wird die spezifische benutzerdefinierte URL durch den **`application's custom scheme`** definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die **`ADOBEPASS_REDIRECT_URL`** -Konstante definiert (d. h. `adobepass://ios.app`).

1. Schließen Sie den Controller UIWebView/WKWebView oder SFSafariViewController und rufen Sie die API-Methode `handleExternalURL:url` von AccessEnabler auf, die AccessEnabler anweist, das Authentifizierungstoken vom Backend-Server abzurufen.

1. (Optional) Rufen Sie [`checkPreauthorizedResources(resources)`](#$checkPreauth) auf, um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Der Parameter `resources` ist ein Array geschützter Ressourcen, die mit dem Authentifizierungstoken des Benutzers verknüpft sind. Eine Verwendung der Autorisierungsinformationen, die vom MVPD des Benutzers erhalten werden, besteht darin, Ihre Benutzeroberfläche zu dekorieren (z. B. gesperrte/entsperrte Symbole neben geschützten Inhalten).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback
   * **Ausführungspunkt:** Nach Abschluss des Authentifizierungsablaufs

1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.

### D. Authentifizierungsfluss mit Apple SSO auf iOS {#authn_flow_with_applesso}

1. Rufen Sie [`getAuthentication()`](#$getAuthN) auf, um den Authentifizierungsfluss zu initiieren oder um zu bestätigen, dass der Benutzer bereits authentifiziert ist.
   **Trigger:**

   * Der Rückruf [presentTvProviderDialog()](#presentTvDialog) , wenn der Benutzer nicht authentifiziert ist und der aktuelle Anfragende mindestens über MVPD verfügt, der SSO unterstützt. Wenn keine MVPDs SSO unterstützen, wird der klassische Authentifizierungsfluss verwendet.

1. Nachdem der Benutzer einen Anbieter ausgewählt hat, ruft die AccessEnabler-Bibliothek ein Authentifizierungstoken mit den Informationen ab, die vom VSA-Framework von Apple bereitgestellt werden.

1. Der Rückruf [setAuthenticationsStatus()](#setAuthNStatus) wird ausgelöst. An dieser Stelle sollte der Benutzer bei Apple SSO authentifiziert werden.

1. [Optional] Rufen Sie [`checkPreauthorizedResources(resources)`](#$checkPreauth) auf, um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Der Parameter `resources` ist ein Array geschützter Ressourcen, die mit dem Authentifizierungstoken des Benutzers verknüpft sind. Eine Verwendung der Autorisierungsinformationen, die vom MVPD des Benutzers erhalten werden, besteht darin, Ihre Benutzeroberfläche zu dekorieren (z. B. gesperrte/entsperrte Symbole neben geschützten Inhalten).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback
   * **Ausführungspunkt:** Nach Abschluss des Authentifizierungsablaufs

1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.

### E. Authentifizierungsfluss mit Apple SSO auf tvOS {#authn_flow_with_applesso_tvOS}

1. Rufen Sie [`getAuthentication()`](#$getAuthN) auf, um den
Authentifizierungsfluss oder um zu bestätigen, dass der Benutzer bereits
authentifiziert.
   **Trigger:**
   * Der Rückruf [`presentTvProviderDialog()`](#presentTvDialog) , wenn der Benutzer nicht authentifiziert ist und der aktuelle Anfragende mindestens über MVPD verfügt, der SSO unterstützt. Wenn keine MVPDs SSO unterstützen, wird der klassische Authentifizierungsfluss verwendet.

1. Nachdem der Benutzer einen Anbieter ausgewählt hat, wird der Rückruf [`status()`](#status_callback_implementation) aufgerufen. Es wird ein Registrierungs-Code bereitgestellt und die AccessEnabler-Bibliothek beginnt mit der Abfrage des Servers auf eine erfolgreiche Zweitbildschirmauthentifizierung.

1. Wenn der angegebene Registrierungs-Code für die erfolgreiche Authentifizierung auf dem zweiten Bildschirm verwendet wurde, wird der Rückruf [`setAuthenticatiosStatus()`](#setAuthNStatus) ausgelöst. An dieser Stelle sollte der Benutzer bei Apple SSO authentifiziert werden.
1. [Optional] Rufen Sie [`checkPreauthorizedResources(resources)`](#$checkPreauth) auf, um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Der Parameter `resources` ist ein Array geschützter Ressourcen, die mit dem Authentifizierungstoken des Benutzers verknüpft sind. Eine Verwendung der Autorisierungsinformationen, die vom MVPD des Benutzers erhalten werden, besteht darin, Ihre Benutzeroberfläche zu dekorieren (z. B. gesperrte/entsperrte Symbole neben geschützten Inhalten).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) callback

   * **Ausführungspunkt:** Nach Abschluss des Authentifizierungsablaufs
1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.

### F. Genehmigungsprozess {#authz_flow}

1. Rufen Sie [getAuthorization()](#$getAuthZ) auf, um den Autorisierungsfluss zu starten.

   * **Abhängigkeit:** Gültige ResourceID(s), die mit den MVPD(s) vereinbart wurde.
   * Die Ressourcen-IDs sollten mit denen auf anderen Geräten oder Plattformen übereinstimmen und sind über MVPDs hinweg identisch. Informationen zu Ressourcen-IDs finden Sie unter [Ermitteln geschützter Ressourcen](/help/authentication/identify-protected-resources.md)

1. Validieren Sie Authentifizierung und Autorisierung.

   * Wenn der Aufruf [getAuthorization()](#$getAuthZ) erfolgreich ist: Der Benutzer verfügt über gültige AuthN- und AuthZ-Token (der Benutzer ist authentifiziert und berechtigt, die angeforderten Medien anzuzeigen).

   * Wenn [getAuthorization()](#$getAuthZ) fehlschlägt: Untersuchen Sie die ausgelöste Ausnahme, um ihren Typ zu bestimmen (AuthN, AuthZ oder etwas Anderes):
      * Wenn es sich um einen Authentifizierungsfehler (AuthN) handelte, starten Sie den Authentifizierungsfluss neu.
      * Wenn es sich um einen Autorisierungsfehler (AuthZ) handelt, ist der Benutzer nicht berechtigt, das angeforderte Medium zu sehen und dem Benutzer sollte eine Fehlermeldung angezeigt werden.
      * Wenn ein anderer Fehlertyp aufgetreten ist (Verbindungsfehler, Netzwerkfehler usw.) zeigen Sie dem Benutzer eine entsprechende Fehlermeldung an.

1. Validieren Sie das Token für kurze Medien.\
   Verwenden Sie die Adobe Pass Authentication Media Token Verifier-Bibliothek, um das vom obigen Aufruf [getAuthorization()](#$getAuthZ) zurückgegebene kurzlebige Medien-Token zu überprüfen:

   * Wenn die Validierung erfolgreich ist: Wiedergabe des angeforderten Mediums für den Benutzer.
   * Wenn die Validierung fehlschlägt: Das AuthZ-Token war ungültig, die Medienanforderung sollte abgelehnt werden und dem Benutzer sollte eine Fehlermeldung angezeigt werden.


1. Kehren Sie zu Ihrem normalen Anwendungsfluss zurück.

### G. Medienfluss anzeigen {#media_flow}

1. Der Benutzer wählt das Medium aus, das angezeigt werden soll.
1. Sind die Medien geschützt? Ihre Anwendung prüft, ob die ausgewählten Medien geschützt sind:

   * Wenn das ausgewählte Medium geschützt ist, startet Ihre Anwendung den obigen [Autorisierungsfluss](#authz_flow).

   * Wenn das ausgewählte Medium nicht geschützt ist, geben Sie das Medium für
den Benutzer.

### H. Abmeldefluss ohne Apple SSO {#logout_flow_wo_AppleSSO}

1. Rufen Sie [`logout()`](#$logout) auf, um den Benutzer abzumelden. AccessEnabler löscht alle zwischengespeicherten Werte und Token. Nachdem der Cache gelöscht wurde, führt der AccessEnabler einen Server-Aufruf durch, um die Server-seitigen Sitzungen zu bereinigen. Da der Server-Aufruf zu einer SAML-Umleitung zum IdP führen kann (dies ermöglicht die Sitzungsbereinigung auf der IdP-Seite), muss dieser Aufruf allen Umleitungen folgen. Aus diesem Grund muss dieser Aufruf innerhalb eines UIWebView/WKWebView- oder SFSafariViewController-Controllers verarbeitet werden.

   a. Entsprechend dem gleichen Muster wie der Authentifizierungs-Workflow sendet die AccessEnabler-Domäne über den Callback `navigateToUrl:` oder `navigateToUrl:useSVC:` eine Anfrage an die UI-Anwendungsebene, um einen UIWebView/WKWebView- oder SFSafariViewController zu erstellen und anzuweisen, die im Parameter `url` des Rückrufs angegebene URL zu laden. Dies ist die URL des Abmelde-Endpunkts auf dem Backend-Server.

   b. Ihre Anwendung muss die Aktivität des `UIWebView/WKWebView or SFSafariViewController`-Controllers überwachen und den Zeitpunkt erkennen, zu dem sie eine bestimmte benutzerdefinierte URL lädt, da sie mehrere Umleitungen durchläuft. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Abmeldefluss abgeschlossen ist und dass es sicher ist, den Controller `UIWebView/WKWebView` oder `SFSafariViewController` zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung den `UIWebView/WKWebView or SFSafariViewController`-Controller schließen und die `handleExternalURL:url`API-Methode von AccessEnabler aufrufen. Wenn ein `SFSafariViewController`Controller verwendet werden muss, wird die spezifische benutzerdefinierte URL durch den **`application's custom scheme`** definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die **`ADOBEPASS_REDIRECT_URL`** -Konstante definiert (d. h. `adobepass://ios.app`).

   >[!NOTE]
   >
   >Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss insofern, als der Benutzer in keiner Weise mit dem UIWebView/WKWebView oder SFSafariViewController interagieren muss. Die UI-Anwendungsebene verwendet einen UIWebView/WKWebView- oder SFSafariViewController, um sicherzustellen, dass alle Umleitungen befolgt werden. Daher ist es möglich (und empfohlen), den Controller während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.


### I. Abmeldefluss mit Apple SSO {#logout_flow_with_AppleSSO}

1. Rufen Sie [`logout()`](#$logout) auf, um den Benutzer abzumelden.
1. Der Rückruf [status()](#status_callback_implementation) wird mit der ID VSA203 aufgerufen.
1. Der Benutzer sollte angewiesen werden, sich auch über die Systemeinstellungen anzumelden. Andernfalls wird eine erneute Authentifizierung durchgeführt, wenn die Anwendung neu gestartet wird.



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
