---
title: iOS/tvOS-Cookbook
description: iOS/tvOS-Cookbook
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '2424'
ht-degree: 0%

---

# (Legacy) iOS/tvOS SDK Cookbook {#iostvos-sdk-cookbook}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung {#intro}

In diesem Dokument werden die Berechtigungs-Workflows beschrieben, die ein Programm der höheren Ebene eines Programmierers über die APIs implementieren kann, die von der iOS/tvOS AccessEnabler-Bibliothek bereitgestellt werden.

Die Adobe Pass-Lösung für die Berechtigung zur Authentifizierung für iOS/tvOS ist letztendlich in zwei Domains unterteilt:

* Die Domain der Benutzeroberfläche : Dies ist die Anwendungsebene der oberen Ebene, die die Benutzeroberfläche implementiert und die von der AccessEnabler-Bibliothek bereitgestellten Services verwendet, um Zugriff auf eingeschränkte Inhalte zu gewähren.

* Die AccessEnabler-Domain - hier werden die Berechtigungs-Workflows in Form von Folgendem implementiert:

   * Netzwerkaufrufe an die Backend-Server von Adobe
   * Geschäftslogikregeln in Bezug auf die Authentifizierungs- und Autorisierungs-Workflows
   * Verwaltung verschiedener Ressourcen und Verarbeitung des Workflow-Status (z. B. des Token-Cache)

Das Ziel der AccessEnabler-Domain besteht darin, alle Komplexitäten der Berechtigungs-Workflows auszublenden und der Upper-Layer-Anwendung (über die AccessEnabler-Bibliothek) eine Reihe einfacher Berechtigungs-Primitive bereitzustellen, mit denen Sie die Berechtigungs-Workflows implementieren:

1. Anfordereridentität festlegen
1. Authentifizierung mit einem bestimmten Identitätsanbieter überprüfen und abrufen
1. Überprüfen und Abrufen der Autorisierung für eine bestimmte Ressource
1. Abmelden
1. Apple SSO-Flüsse durch Proxy des Apple VSA-Frameworks

Die Netzwerkaktivität des AccessEnabler erfolgt in einem eigenen Thread, sodass der UI-Thread nie blockiert wird. Daher muss der bidirektionale Kommunikationskanal zwischen den beiden Anwendungsdomänen einem vollständig asynchronen Muster folgen:

* Die Anwendungsebene der Benutzeroberfläche sendet Nachrichten über die API-Aufrufe, die von der AccessEnabler-Bibliothek verfügbar gemacht werden, an die AccessEnabler-Domain.
* Der AccessEnabler antwortet auf die Benutzeroberflächenebene über die im AccessEnabler-Protokoll enthaltenen Callback-Methoden, die die Benutzeroberflächenebene bei der AccessEnabler-Bibliothek registriert.

## Konfigurieren des Experience Cloud ID-Service (Besucher-ID) {#visitorIDSetup}

Die Konfiguration des Werts [&#x200B; &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/home.html)Experience Cloud ID[!DNL Analytics] ist wichtig. Nachdem ein `visitorID` festgelegt wurde, sendet die SDK diese Informationen zusammen mit jedem Netzwerkaufruf, und der [!DNL Adobe Pass]-Authentifizierungsserver erfasst diese Informationen. Sie können die Analysen aus dem Adobe Pass-Authentifizierungs-Service mit allen anderen Analyseberichten korrelieren, die Sie möglicherweise von anderen Anwendungen oder Websites haben. Informationen zum Einrichten von visitorID finden Sie [hier](#setOptions).

## Berechtigungsflüsse {#entitlement}

A. [Voraussetzungen](#prereqs) </br>
B. [Startfluss](#startup_flow) </br>
C. [Authentifizierungsfluss ohne Apple SSO](#authn_flow_wo_applesso) </br>
D. [Authentifizierungsfluss mit Apple SSO auf iOS](#authn_flow_with_applesso) </br>
E. [Authentifizierungsfluss mit Apple SSO auf tvOS](#authn_flow_with_applesso_tvOS) </br>
F. [AUTORISIERUNGSFLUSS](#authz_flow) </br>
G. [Medienfluss anzeigen](#media_flow) </br>
H. [Abmeldefluss ohne Apple SSO](#logout_flow_wo_AppleSSO) </br>
I. [Abmeldefluss mit Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Voraussetzungen {#prereqs}

1. Erstellen Sie Ihre Callback-Funktionen:
   * `setRequestorComplete()` </br>
   * Ausgelöst durch [setRequestor()](#$setReq), gibt Erfolg oder Fehler zurück. </br>
   * Der Erfolg zeigt an, dass Sie mit Berechtigungsaufrufen fortfahren können.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Wird nur von [`getAuthentication()`](#$getAuthN) ausgelöst, wenn der Benutzer keinen Provider (MVPD) ausgewählt hat und noch nicht authentifiziert ist. </br>
      * Der `mvpds` Parameter ist ein Array von Anbietern, die den Benutzenden zur Verfügung stehen.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Wird jedes Mal von `checkAuthentication()` ausgelöst. </br>
      * Wird nur von [`getAuthentication()`](#$getAuthN) ausgelöst, wenn der Benutzer bereits authentifiziert ist und einen Anbieter ausgewählt hat. </br>
      * Als Status wird Erfolg oder Fehler zurückgegeben. Der Fehler-Code beschreibt den Typ des Fehlers.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Wird durch [`getAuthentication()`](#$getAuthN) ausgelöst, nachdem der Benutzer eine MVPD ausgewählt hat. Der `url` gibt den Speicherort der Anmeldeseite von MVPD an.

   * `sendTrackingData(event, data)` </br>
      * Ausgelöst durch `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * Der `event` Parameter gibt an, welches Berechtigungsereignis aufgetreten ist. Der `data` Parameter ist eine Liste von Werten, die sich auf das Ereignis beziehen.

   * `setToken(token, resource)`

      * Wird durch [checkAuthorization()](#checkAuthZ) und [getAuthorization()](#$getAuthZ) nach erfolgreicher Autorisierung zum Anzeigen einer Ressource ausgelöst.
      * Der `token` ist das kurzlebige Medien-Token. Der `resource` ist der Inhalt, den die Benutzerin oder der Benutzer anzeigen darf.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Wird von [checkAuthorization()](#checkAuthZ) und [getAuthorization()](#$getAuthZ) nach einer nicht erfolgreichen Autorisierung ausgelöst.
      * Der `resource` ist der Inhalt, den die Benutzerin oder der Benutzer versucht hat anzuzeigen. Der `code` ist der Fehlercode, der angibt, welche Art von Fehler aufgetreten ist. Der `description` Parameter beschreibt den Fehler, der mit dem Fehlercode verbunden ist.

   * `selectedProvider(mvpd)` </br>
      * Ausgelöst durch [`getSelectedProvider()`](#getSelProv).
      * Der Parameter `mvpd` enthält Informationen zum vom Benutzer ausgewählten Anbieter.

   * `setMetadataStatus(metadata, key, arguments)`
      * Ausgelöst durch `getMetadata().`
      * Der `metadata` Parameter liefert die angeforderten spezifischen Daten. Der `key` Parameter ist der Schlüssel, der in der Anfrage [getMetadata()](#getMeta) verwendet wird. Der `arguments` Parameter ist dasselbe Wörterbuch, das an [getMetadata()](#getMeta) übergeben wurde.

   * [`preauthorizedResources(authorizedResources)`](#preauthResources)

      * Ausgelöst durch [`checkPreauthorizedResources()`](#checkPreauth).

      * Der `authorizedResources` Parameter stellt die Ressourcen dar, die der Benutzer verwendet
ist berechtigt, Folgendes anzuzeigen.

   * [`presentTvProviderDialog(viewController)`](#presentTvDialog)

      * Wird durch [getAuthentication()](#getAuthN) ausgelöst, wenn der aktuelle Anforderer mindestens MVPD mit SSO-Unterstützung unterstützt.
      * Der viewController-Parameter ist das Apple SSO-Dialogfeld und muss auf dem Hauptansichtscontroller angezeigt werden.

   * [`dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Wird durch eine Benutzeraktion ausgelöst (durch Auswahl von „Abbrechen“ oder „Andere TV-Anbieter“ im Apple-SSO-Dialogfeld).
      * Der viewController-Parameter ist das Apple SSO-Dialogfeld und muss vom Hauptansichtscontroller entfernt werden.

![](/help//authentication/assets/iOS-flows.png)

### B. Anlauffluss {#startup_flow}

1. Starten Sie die Anwendung auf höherer Ebene.</br>
1. Adobe Pass-</br> initiieren

   a. Rufen Sie [`init`](#$init) auf, um eine einzelne Instanz von Adobe Pass Authentication AccessEnabler zu erstellen.
   * **Abhängigkeit:** Native iOS/tvOS-Bibliothek für die Adobe Pass-Authentifizierung (AccessEnabler)

   b. Rufen Sie `setRequestor()` auf, um die Identität des Programmierers festzulegen, übergeben Sie die `requestorID` des Programmierers und (optional) ein Array von Adobe Pass-Authentifizierungsendpunkten. Bei tvOS müssen Sie auch den öffentlichen Schlüssel und das Geheimnis angeben. Weitere Informationen finden [&#x200B; in der &#x200B;](#create_dev)-Dokumentation .

   * **Abhängigkeit:** gültige Adobe Pass-Authentifizierungsanforderungs-ID (mit Ihrem Adobe Pass-Authentifizierungskonto arbeiten)
Manager (um dies zu arrangieren).

   * **Trigger:**
     [setRequestorComplete()](#$setReqComplete) Rückruf.

   >[!NOTE]
   >
   >Berechtigungsanfragen können erst abgeschlossen werden, wenn die Identität des Antragstellers vollständig ermittelt wurde. Dies bedeutet effektiv, dass während der [`setRequestor()`](#$setReq) alle nachfolgenden Berechtigungsanfragen ausgeführt werden. Beispielsweise werden [`checkAuthentication()`](#checkAuthN) blockiert.

   Sie haben zwei Implementierungsoptionen: Sobald die Anfordereridentifizierungsinformationen an den Backend-Server gesendet wurden, kann die Benutzeroberflächen-Anwendungsebene einen der beiden folgenden Ansätze wählen: </br>

   1. Warten Sie, bis der [`setRequestorComplete()`](#setReqComplete)-Callback ausgelöst wird (Teil des AccessEnabler-Delegaten). Diese Option bietet die größte Sicherheit, dass abgeschlossen [`setRequestor()`](#$setReq). Daher wird sie für die meisten Implementierungen empfohlen.

   1. Fahren Sie fort, ohne auf das Auslösen des [`setRequestorComplete()`](#setReqComplete)-Callbacks zu warten, und beginnen Sie mit der Ausgabe von Berechtigungsanfragen. Diese Aufrufe (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) werden von der AccessEnabler-Bibliothek in die Warteschlange gestellt, die die eigentlichen Netzwerkaufrufe nach dem [`setRequestor()`](#$setReq) ausführt. Diese Option kann gelegentlich unterbrochen werden, wenn beispielsweise die Netzwerkverbindung instabil ist.

1. Rufen Sie `checkAuthentication()` auf, um nach einer vorhandenen Authentifizierung zu suchen, ohne den vollständigen Authentifizierungsfluss zu initiieren.  Wenn dieser Aufruf erfolgreich ist, können Sie direkt mit dem Autorisierungsfluss fortfahren. Andernfalls fahren Sie mit dem Authentifizierungsfluss fort.

   * **Abhängigkeit:** Erfolgreicher Aufruf an [setRequestor()](#$setReq) (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).

   * **Trigger:** [setAuthenticationStatus()](#$setAuthNStatus) Callback.


### C. Authentifizierungsfluss ohne Apple SSO {#authn_flow_wo_applesso}

1. Rufen Sie [`getAuthentication()`](#$getAuthN) auf, um den Authentifizierungsfluss zu initiieren oder um eine Bestätigung zu erhalten, dass der Benutzer bereits vorhanden ist
authentifiziert.

   **Trigger:**

   * Der [setAuthenticationStatus()](#$setAuthNStatus)-Rückruf, wenn der Benutzer bereits authentifiziert ist. Fahren Sie in diesem Fall direkt mit dem [Autorisierungsfluss“ &#x200B;](#authz_flow).

   * Der [displayProviderDialog()](#$dispProvDialog) Rückruf, wenn der Benutzer noch nicht authentifiziert ist.

1. Zeigen Sie dem Benutzer die Liste der Anbieter, die an gesendet wurden.
   [`displayProviderDialog()`](#dispProvDialog).

1. Nachdem der Benutzer einen Anbieter ausgewählt hat, rufen Sie die URL des MVPD des Benutzers aus dem `navigateToUrl:` oder `navigateToUrl:useSVC:` Callback ab und öffnen Sie einen `UIWebView/WKWebView` oder `SFSafariViewController` Controller und leiten Sie diesen Controller an die URL weiter.

1. Durch die im vorherigen Schritt instanziierte `UIWebView/WKWebView` oder `SFSafariViewController` landet der Benutzer auf der Anmeldeseite von MVPD und gibt Anmeldedaten ein. Mehrere Umleitungsvorgänge finden innerhalb des Controllers statt.</br>

>[!NOTE]
>
>An dieser Stelle hat der Benutzer die Möglichkeit, den Authentifizierungsfluss abzubrechen. In diesem Fall ist Ihre Benutzeroberflächenebene dafür verantwortlich, den AccessEnabler über dieses Ereignis zu informieren, indem sie [setSelectedProvider()](#setSelProv) mit `null` als Parameter aufruft. Dadurch kann AccessEnabler seinen internen Status bereinigen und den Authentifizierungsfluss zurücksetzen.

1. Nach erfolgreicher Anmeldung durch den Benutzer erkennt Ihre Anwendungsebene das Laden einer bestimmten benutzerdefinierten URL. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht vom Controller geladen werden soll. Sie darf von Ihrer Anwendung nur als Signal interpretiert werden, dass der Authentifizierungsfluss abgeschlossen ist und dass es sicher ist, den `UIWebView/WKWebView` oder `SFSafariViewController` Controller zu schließen. Muss ein `SFSafariViewController`Controller verwendet werden, wird die spezifische benutzerdefinierte URL durch die **`application's custom scheme`** definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die **`ADOBEPASS_REDIRECT_URL`**-Konstante definiert (d. h. `adobepass://ios.app`).

1. Schließen Sie den Controller UIWebView/WKWebView oder SFSafariViewController und rufen Sie die API-Methode `handleExternalURL:url` AccessEnabler auf, die AccessEnabler anweist, das Authentifizierungstoken vom Backend-Server abzurufen.

1. (Optional) Rufen Sie [`checkPreauthorizedResources(resources)`](#$checkPreauth) auf, um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Der `resources` ist ein Array geschützter Ressourcen, das mit dem Authentifizierungstoken des Benutzers verknüpft ist. Ein Verwendungszweck für die Autorisierungsinformationen, die vom MVPD des Benutzers abgerufen werden, besteht darin, Ihre Benutzeroberfläche zu dekorieren (z. B. gesperrte/entsperrte Symbole neben geschützten Inhalten).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) Callback
   * **Ausführungspunkt:** Nach Abschluss des Authentifizierungsflusses

1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.

### D. Authentifizierungsfluss mit Apple SSO auf iOS {#authn_flow_with_applesso}

1. Rufen Sie [`getAuthentication()`](#$getAuthN) auf, um den Authentifizierungsfluss zu initiieren oder um eine Bestätigung zu erhalten, dass der Benutzer bereits authentifiziert ist.
   **Trigger:**

   * Der [presentTvProviderDialog()](#presentTvDialog) Rückruf, wenn der Benutzer nicht authentifiziert ist und der aktuelle Anforderer mindestens über eine MVPD verfügt, die SSO unterstützt. Wenn keine MVPDs SSO unterstützen, wird der klassische Authentifizierungsfluss verwendet.

1. Nachdem der Benutzer einen Anbieter ausgewählt hat, erhält die AccessEnabler-Bibliothek ein Authentifizierungstoken mit den vom VSA-Framework von Apple bereitgestellten Informationen.

1. Der [setAuthenticationsStatus()](#setAuthNStatus)Callback wird ausgelöst. An dieser Stelle sollte die Benutzerin bzw. der Benutzer mit Apple SSO authentifiziert werden.

1. [Optional] Rufen Sie [`checkPreauthorizedResources(resources)`](#$checkPreauth) auf, um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Der `resources` ist ein Array geschützter Ressourcen, das mit dem Authentifizierungstoken des Benutzers verknüpft ist. Ein Verwendungszweck für die Autorisierungsinformationen, die vom MVPD des Benutzers abgerufen werden, besteht darin, Ihre Benutzeroberfläche zu dekorieren (z. B. gesperrte/entsperrte Symbole neben geschützten Inhalten).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) Callback
   * **Ausführungspunkt:** Nach Abschluss des Authentifizierungsflusses

1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.

### E. Authentifizierungsfluss mit Apple SSO unter tvOS {#authn_flow_with_applesso_tvOS}

1. [`getAuthentication()`](#$getAuthN) aufrufen, um den
Authentifizierungsfluss oder , um die Bestätigung zu erhalten, dass der Benutzer bereits
authentifiziert.
   **Trigger:**
   * Der [`presentTvProviderDialog()`](#presentTvDialog) Callback, wenn der Benutzer nicht authentifiziert ist und der aktuelle Anforderer mindestens über MVPD verfügt, das SSO unterstützt. Wenn keine MVPDs SSO unterstützen, wird der klassische Authentifizierungsfluss verwendet.

1. Nachdem der Benutzer einen Anbieter ausgewählt hat, wird der [`status()`](#status_callback_implementation) Callback aufgerufen. Es wird ein Registrierungs-Code bereitgestellt und die AccessEnabler-Bibliothek beginnt damit, den Server für eine erfolgreiche Authentifizierung auf dem zweiten Bildschirm abzufragen.

1. Wenn der angegebene Registrierungs-Code verwendet wurde, um sich erfolgreich auf dem zweiten Bildschirm zu authentifizieren, wird der [`setAuthenticatiosStatus()`](#setAuthNStatus)-Callback ausgelöst. An dieser Stelle sollte die Benutzerin bzw. der Benutzer mit Apple SSO authentifiziert werden.
1. [Optional] Rufen Sie [`checkPreauthorizedResources(resources)`](#$checkPreauth) auf, um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Der `resources` ist ein Array geschützter Ressourcen, das mit dem Authentifizierungstoken des Benutzers verknüpft ist. Ein Verwendungszweck für die Autorisierungsinformationen, die vom MVPD des Benutzers abgerufen werden, besteht darin, Ihre Benutzeroberfläche zu dekorieren (z. B. gesperrte/entsperrte Symbole neben geschützten Inhalten).

   * **Trigger:** [`preauthorizedResources()`](#preauthResources) Callback

   * **Ausführungspunkt:** Nach Abschluss des Authentifizierungsflusses
1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.

### F. Autorisierungsfluss {#authz_flow}

1. Rufen Sie [getAuthorization()](#$getAuthZ) auf, um den Autorisierungsfluss zu initiieren.

   * **Dependency:** mit den MVPD(s) vereinbarte ResourceID(s).
   * Die Ressourcen-IDs müssen mit den IDs übereinstimmen, die auf anderen Geräten oder Plattformen verwendet werden, und sie müssen für alle MVPDs identisch sein. Informationen zu Ressourcen-IDs finden Sie unter [Ressourcenkennung](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)

1. Authentifizierung und Autorisierung überprüfen.

   * Wenn der Aufruf [getAuthorization()](#$getAuthZ) erfolgreich ist: Der Benutzer verfügt über gültige AuthN- und AuthZ-Token (der Benutzer ist authentifiziert und berechtigt, die angeforderten Medien zu sehen).

   * Wenn [getAuthorization()](#$getAuthZ) fehlschlägt: Untersuchen Sie die ausgelöste Ausnahme, um ihren Typ (AuthN, AuthZ oder etwas Anderes) zu ermitteln:
      * Wenn es sich um einen Authentifizierungsfehler (AuthN) handelte, starten Sie den Authentifizierungsfluss erneut.
      * Wenn es sich um einen Autorisierungsfehler (AuthZ) handelte, ist der Benutzer nicht berechtigt, die angeforderten Medien anzusehen, und dem Benutzer sollte eine Fehlermeldung angezeigt werden.
      * Wenn ein anderer Fehlertyp aufgetreten ist (Verbindungsfehler, Netzwerkfehler usw.), zeigen Sie dem Benutzer eine entsprechende Fehlermeldung an.

1. Validieren des Short Media Token.\
   Verwenden Sie die Media Token Verifier-Bibliothek der Adobe Pass-Authentifizierung, um das kurzlebige Medien-Token zu überprüfen, das vom obigen Aufruf [getAuthorization()](#$getAuthZ) zurückgegeben wurde:

   * Wenn die Validierung erfolgreich ist: Geben Sie die angeforderten Medien für den Benutzer wieder.
   * Wenn die Validierung fehlschlägt: Das AuthZ-Token war ungültig, sollte die Medienanfrage abgelehnt werden und dem Benutzer sollte eine Fehlermeldung angezeigt werden.


1. Kehren Sie zu Ihrem normalen Programmablauf zurück.

### G. Anzeigen des Medienflusses {#media_flow}

1. Der Benutzer wählt die anzuzeigenden Medien aus.
1. Sind die Medien geschützt? Ihre Anwendung prüft, ob das ausgewählte Medium geschützt ist:

   * Wenn das ausgewählte Medium geschützt ist, startet Ihre Anwendung den [Autorisierungsfluss](#authz_flow) oben.

   * Wenn das ausgewählte Medium nicht geschützt ist, können Sie es für wiedergeben
Der Benutzer.

### H. Abmeldefluss ohne Apple SSO {#logout_flow_wo_AppleSSO}

1. Rufen Sie [`logout()`](#$logout) auf, um den Benutzer abzumelden. AccessEnabler löscht alle zwischengespeicherten Werte und Token. Nach dem Löschen des Cache führt AccessEnabler einen Server-Aufruf durch, um die Server-seitigen Sitzungen zu bereinigen. Da der Server-Aufruf zu einer SAML-Umleitung an den IdP führen kann (dies ermöglicht die Sitzungsbereinigung auf der IdP-Seite), muss dieser Aufruf allen Umleitungen folgen. Aus diesem Grund muss dieser Aufruf in einem UIWebView/WKWebView- oder SFSafariViewController verarbeitet werden.

   a. Nach demselben Muster wie der Authentifizierungs-Workflow fordert die AccessEnabler-Domain über den `navigateToUrl:`- oder `navigateToUrl:useSVC:`-Callback die Benutzeroberflächenanwendungsschicht auf, einen UIWebView/WKWebView- oder SFSafariViewController-Controller zu erstellen, und weist diesen an, die im `url`-Parameter des Callbacks bereitgestellte URL zu laden. Dies ist die URL des Abmeldeendpunkts auf dem Backend-Server.

   b. Ihre Anwendung muss die Aktivität des `UIWebView/WKWebView or SFSafariViewController`-Controllers überwachen und den Zeitpunkt erkennen, zu dem sie eine bestimmte benutzerdefinierte URL lädt, während sie mehrere Weiterleitungen durchläuft. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht vom Controller geladen werden soll. Sie darf von Ihrer Anwendung nur als Signal interpretiert werden, dass der Abmeldefluss abgeschlossen ist und dass es sicher ist, den `UIWebView/WKWebView` oder `SFSafariViewController` Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss die Anwendung den `UIWebView/WKWebView or SFSafariViewController`-Controller schließen und die Methode &quot;`handleExternalURL:url`&quot; von AccessEnabler aufrufen. Wenn ein `SFSafariViewController`Controller“ verwendet werden muss, wird die spezifische benutzerdefinierte URL durch die **`application's custom scheme`** definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die **`ADOBEPASS_REDIRECT_URL`**-Konstante definiert (d. h. `adobepass://ios.app`).

   >[!NOTE]
   >
   >Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss insofern, als der Benutzer in keiner Weise mit dem UIWebView/WKWebView oder SFSafariViewController interagieren muss. Die Benutzeroberflächenanwendungsebene verwendet einen UIWebView/WKWebView oder SFSafariViewController, um sicherzustellen, dass alle Weiterleitungen befolgt werden. Daher ist es möglich (und empfohlen), den Controller während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.


### I. Abmeldefluss mit Apple SSO {#logout_flow_with_AppleSSO}

1. Rufen Sie [`logout()`](#$logout) auf, um den Benutzer abzumelden.
1. Der [status()](#status_callback_implementation) Callback wird mit der ID VSA203 aufgerufen.
1. Der Benutzer sollte angewiesen werden, sich auch über die Systemeinstellungen anzumelden. Andernfalls erfolgt eine erneute Authentifizierung, wenn die Anwendung neu gestartet wird.



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
