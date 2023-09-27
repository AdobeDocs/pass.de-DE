---
title: Apple SSO-Cookbook (iOS/tvOS SDK)
description: Apple SSO-Cookbook (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Apple SSO-Cookbook (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#Introduction}

Das Adobe Primetime Authentication AccessEnabler iOS/tvOS SDK unterstützt die SSO-Authentifizierung (Single Sign-On) für Endbenutzer von Clientanwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden, über einen so genannten Apple SSO-Workflow.

Beachten Sie, dass dieses Dokument als Erweiterung der vorhandenen AccessEnabler iOS/tvOS SDK-Dokumentation dient, die Sie hier finden [here](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Cookbook {#Cookbook}

Um von der Apple SSO-Benutzererfahrung zu profitieren, muss eine Anwendung das AccessEnabler iOS/tvOS SDK integrieren und die unten dargestellten Tipps befolgen.

</br>

### Voraussetzungen {#Prerequisites}

</br>

#### Berechtigung

>[!TIP]
>
> **<u>Pro Tipp:</u>** Um Zugriff auf die Abonnementinformationen des Benutzers zu erhalten, muss der Benutzer der Anwendung die Berechtigung zum Fortfahren erteilen, ähnlich wie beim Gewähren des Zugriffs auf die Kamera oder das Mikrofon des Geräts. Diese Berechtigung muss pro Anwendung angefordert werden. Das Gerät speichert die Auswahl des Benutzers. Beachten Sie, dass der Benutzer seine Entscheidung ändern kann, indem er zu den Anwendungseinstellungen (Zugriffsberechtigung für TV-Anbieter) oder zum Abschnitt über *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Es wird empfohlen, die Berechtigung des Benutzers anzufordern, wenn die Anwendung in den Vordergrund wechselt. Dies ist jedoch nur ein Vorschlag, da die Anwendung nach [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) die Abonnementinformationen des Benutzers zu jedem Zeitpunkt, bevor eine Benutzerauthentifizierung erforderlich ist. Außerdem fordern die AccessEnabler iOS/tvOS SDK-APIs bei Bedarf automatisch die Benutzerberechtigung an.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Wenn der Benutzer keinen Zugriff auf seine Abonnementinformationen gewährt oder die Kommunikation mit dem Video Subscriber Account Framework fehlschlägt, kehrt das AccessEnabler iOS/tvOS SDK zum regulären Authentifizierungsfluss zurück.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Es wird empfohlen, Benutzer, die die Erlaubnis zum Zugriff auf Abonnementinformationen verweigern, dazu anzuregen, indem die Vorteile des Single Sign-On (SSO)-Benutzererlebnisses erläutert werden. Beachten Sie, dass der Benutzer seine Entscheidung ändern kann, indem er zu den Anwendungseinstellungen (Zugriffsberechtigung für TV-Anbieter) oder zum Abschnitt über *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS.


```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

</br>

#### Rückrufe

>[!TIP]
>
> **<u>Pro Tipp:</u>** Implementieren Sie die folgende Liste von [Callbacks](/help/authentication/iostvos-sdk-api-reference.md) die spezifisch für den Apple SSO-Workflow sind.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Callback ausgelöst, wenn der Apple MVPD-Picker geöffnet wird.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Callback ausgelöst, wenn der Apple MVPD-Picker geschlossen wird.

</br>

#### Fehlerberichte

>[!TIP]
>
> **<u>Pro Tipp:</u>** Implementieren Sie die folgende Liste von [erweiterte Fehlercodes](/help/authentication/error-reporting.md) die spezifisch für den Apple SSO-Workflow sind.

- ***N003*** - Der Benutzer hat in der Apple MVPD-Auswahl die Option &quot;Andere TV-Anbieter&quot;ausgewählt.
- ***N004*** - Der Benutzer hat einen Fernsehanbieter aus der Apple MVPD-Auswahl ausgewählt, der vom aktuellen Anfragenden nicht unterstützt wird (Integration oder Single-Sign-On ist deaktiviert).
- ***N005*** - Der Benutzer hat beschlossen, den regulären MVPD-Picker oder die Apple-MVPD-Auswahl abzubrechen.
- ***VSA403*** - Die Berechtigung TV Provider des Benutzers für die Anwendung wird verweigert.
- ***VSA404*** - Die Berechtigung TV Provider des Benutzers für die Anwendung wird nicht festgelegt.
- ***VSA 503*** - Die Metadaten-Anfrage für das Video-Abonnentenkonto ist fehlgeschlagen. Mehr Kontext wird im Abschnitt *message* -Feld.
- ***AAPL / APPL_ERROR*** - Die Metadaten-Anfrage für das Video-Abonnentenkonto ist fehlgeschlagen. Mehr Kontext wird im Abschnitt *details* -Feld.

</br>

### Authentifizierung {#Authentication}

>[!TIP]
>
> **<u>Tipp:</u>** Gehen Sie wie folgt vor, um die Implementierung von iOS/iPadOS/tvOS durchzuführen.

1. Der Antrag müsste [initialize](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) das AccessEnabler iOS/tvOS-SDK.
1. Der Antrag müsste [Festlegen der aktuellen Anforderungs-ID](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Wichtig:** In diesem zweiten Schritt kann ein Trigger [erweiterter Fehlercode](/help/authentication/error-reporting.md) die für den Apple SSO-Workflow spezifisch ist, falls **eines der folgenden Ereignisse wahr ist**:

   - ***VSA403*** - Die Berechtigung TV Provider des Benutzers für die Anwendung wird verweigert.
   - ***VSA404*** - Die Berechtigung TV Provider des Benutzers für die Anwendung wird nicht festgelegt.
   - ***APPL*** - Bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Video Subscriber Account Framework ist ein Fehler aufgetreten.

   In diesem zweiten Schritt wird versucht, das Apple SSO-Profil für ein Adobe-Authentifizierungstoken still zu tauschen, falls **alle oben genannten Werte falsch sind** und **alle folgenden Punkte sind wahr**:

   - Die TV Provider-Berechtigung des Benutzers wird für die Anwendung gewährt.
   - Der Benutzer ist auf Geräteebene bei seinem TV-Provider-Konto angemeldet.
   - Das AccessEnabler iOS/tvOS-SDK hat die TV-Provider-Kennung des Benutzers aus dem Framework für Video-Abonnentenkonten erhalten.
   - Die Integration des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE Dashboard aktiviert.
   - Das Single-Sign-On des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE-Dashboard aktiviert.
   - Der Fernsehanbieter des Benutzers wird nicht über das Adobe Primetime TVE-Dashboard beschädigt.
   - Das AccessEnabler iOS/tvOS SDK hat die SAML-Antwort des Fernsehanbieters des Benutzers vom Framework für Videoponnentenkonten erhalten.

   **<u>Pro Tipp:</u>** In diesem zweiten Schritt werden neben dem [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) Callback, da die Authentifizierung nicht explizit von der Anwendung initiiert wurde.

1. Der Antrag müsste [Authentifizierungsstatus überprüfen](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Wichtig:** Dieser dritte Schritt könnte einen Trigger [erweiterter Fehlercode](/help/authentication/error-reporting.md) die für den Apple SSO-Workflow spezifisch ist, falls **eines der folgenden Ereignisse wahr ist**:

   - ***VSA403** - Der Benutzer ist auf Geräteebene beim TV-Provider-Konto angemeldet, aber die TV Provider-Berechtigung des Benutzers für die Anwendung wird verweigert.
   - ***VSA404** - Der Benutzer ist auf Geräteebene beim TV-Provider-Konto angemeldet, aber die TV-Provider-Berechtigung des Benutzers für die Anwendung wird nicht festgelegt.
   - ***APPL\_ERROR** - Der Benutzer ist auf Geräteebene beim TV-Provider-Konto angemeldet, bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Video Subscriber Account Framework ist jedoch ein Fehler aufgetreten.

   **Wichtig:** Dieser dritte Schritt wird den Trigger [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) Callback mit *status* gleich 0, in dem Fall **eines der folgenden Ereignisse wahr ist**:

   - Der Benutzer ist nicht beim TV-Provider-Konto auf Gerätesystemebene oder über einen normalen Authentifizierungsfluss angemeldet.
   - Der Benutzer ist auf Gerätesystemebene oder über einen normalen Authentifizierungsfluss bei seinem TV Provider-Konto angemeldet, aber die TTL des TV Provider-Authentifizierungstokens des Benutzers wurde übergeben.
   - Der Benutzer ist auf Gerätesystemebene oder über einen regelmäßigen Authentifizierungsfluss bei seinem TV-Provider-Konto angemeldet, aber die Integration des TV-Anbieters des Benutzers mit der Anwendung ist über das Adobe Primetime TVE-Dashboard deaktiviert.
   - Der Benutzer ist auf Geräteebene bei seinem TV-Provider-Konto angemeldet, aber die Single-Sign-On-Funktion des TV-Anbieters des Benutzers mit der Anwendung ist über das Adobe Primetime TVE-Dashboard deaktiviert.
   - Der Benutzer ist auf Geräteebene beim TV-Provider-Konto angemeldet, aber die TV-Provider-Berechtigung des Benutzers für die Anwendung wird verweigert.
   - Der Benutzer ist auf Gerätesystemebene bei seinem TV-Provider-Konto angemeldet, aber die TV Provider-Berechtigung des Benutzers für die Anwendung wird nicht festgelegt.
   - Der Benutzer ist auf Geräteebene beim TV-Provider-Konto angemeldet, bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Video Subscriber Account Framework ist jedoch ein Fehler aufgetreten.

   **Wichtig:** Dieser dritte Schritt wird den Trigger [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) Callback mit *status* gleich 1, in dem Fall **alle oben genannten Werte sind falsch.**


1. Der Antrag müsste [Authentifizierung initialisieren](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) falls die vorherige Prüfung des Authentifizierungsstatus die [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) Callback mit *status* gleich 0.

   **<u>Pro Tipp:</u>** Implementieren Sie eine der folgenden AccessEnabler iOS/tvOS SDK-API [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) oder [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Wichtig:** Dieser vierte Schritt könnte Trigger eines [erweiterter Fehlercode](/help/authentication/error-reporting.md) die für den Apple SSO-Workflow spezifisch ist, falls **eines der folgenden Ereignisse wahr ist**:

   - ***VSA403*** - Die Berechtigung TV Provider des Benutzers für die Anwendung wird verweigert.
   - ***VSA404*** - Die Berechtigung TV Provider des Benutzers für die Anwendung wird nicht festgelegt.
   - ***VSA 503*** - Bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Video Subscriber Account Framework ist ein Fehler aufgetreten.
   - ***N003*** - Der Benutzer hat in der Apple MVPD-Auswahl die Option &quot;Andere TV-Anbieter&quot;ausgewählt.
   - ***N004*** - Der Benutzer hat einen Fernsehanbieter aus der Apple MVPD-Auswahl ausgewählt, der vom aktuellen Anfragenden nicht unterstützt wird (Integration oder Single-Sign-On ist deaktiviert).
   - ***N005*** - Der Benutzer hat beschlossen, den regulären MVPD-Picker oder die Apple-MVPD-Auswahl abzubrechen.

   **Wichtig:** Dieser vierte Schritt würde zum normalen Authentifizierungsfluss zurückfallen, indem der [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) Callback und **one** der oben genannten [erweiterte Fehlercodes](/help/authentication/error-reporting.md), falls **einer der oben genannten ist wahr**.

   **Wichtig:** Dieser vierte Schritt würde zum normalen Authentifizierungsfluss zurückfallen, indem der [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) oder [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) Callback und **Keine** der oben genannten [erweiterte Fehlercodes](/help/authentication/error-reporting.md), falls der Benutzer einen TV-Anbieter ausgewählt hat, der Apple SSO nicht unterstützt, aber in der Apple-MVPD-Auswahl vorhanden ist.

   **<u>Pro Tipp:</u>** Das AccessEnabler iOS/tvOS-SDK ruft die [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API, falls der Benutzer einen TV-Anbieter ausgewählt hat, der die Apple SSO nicht unterstützt, aber in der Apple-MVPD-Auswahl vorhanden ist.

   **Wichtig:** In diesem vierten Schritt wird versucht, das Apple SSO-Profil still gegen ein Adobe-Authentifizierungstoken zu tauschen, falls **alle oben genannten Werte falsch sind** und **alle folgenden Punkte sind wahr**:

   - Die TV Provider-Berechtigung des Benutzers wird für die Anwendung gewährt.
   - Der Benutzer ist angemeldet/meldet sich derzeit beim TV Provider-Konto auf Gerätesystemebene an.
   - Das AccessEnabler iOS/tvOS-SDK hat die TV-Provider-Kennung des Benutzers aus dem Framework für Video-Abonnentenkonten erhalten.
   - Die Integration des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE Dashboard aktiviert.
   - Das Single-Sign-On des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE-Dashboard aktiviert.
   - Der Fernsehanbieter des Benutzers wird nicht über das Adobe Primetime TVE-Dashboard beschädigt.
   - Das AccessEnabler iOS/tvOS SDK hat die SAML-Antwort des Fernsehanbieters des Benutzers vom Framework für Videoponnentenkonten erhalten.



>**<u>Pro Tipp:</u>** Dieser vierte Schritt wird den Trigger [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) Rückruf, unabhängig von *status* Ergebnis, da die Authentifizierung explizit von der Anwendung initiiert wurde.


</br>

### Metadaten {#Metadata}

Die Anwendung kann mithilfe der Funktion &quot;&quot;bestimmen, ob die Authentifizierung infolge einer Anmeldung über die Plattform-SSO erfolgte oder nicht.*tokenSource&quot;* [Benutzermetadaten](/help/authentication/iostvos-sdk-api-reference.md#getMeta) API vom AccessEnabler iOS/tvOS SDK.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Abmelden {#Logout}

Die [Video-Abonnentenkonto](https://developer.apple.com/documentation/videosubscriberaccount) Framework stellt keine API zum programmatischen Abmelden von Personen bereit, die sich auf Gerätesystemebene bei ihrem TV-Anbieterkonto angemeldet haben. Damit die Abmeldung vollständig wirksam wird, muss sich der Endbenutzer daher explizit von abmelden *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS. Die andere Option, die der Benutzer haben würde, besteht darin, die Berechtigung zum Zugriff auf die Abonnementinformationen des Benutzers aus dem Abschnitt für die spezifischen Anwendungseinstellungen (Zugriffsberechtigungen des TV-Anbieters) zu entziehen.

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über AccessEnabler iOS/tvOS SDK [Abmelden](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>Pro Tipp:</u>** Führen Sie die folgenden Schritte für die tvOS-Implementierung/-Implementierungen aus.

- Der Antrag müsste [Logout starten](/help/authentication/iostvos-sdk-api-reference.md#logout) vom AccessEnabler iOS/tvOS SDK. Dies würde die Sitzungsbereinigung auf der MVPD-Seite nicht erleichtern.
- Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von abzumelden *`Settings -> Accounts -> TV Provider`* nur bei tvOS [*VSA203* Statuscode wird ausgelöst](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Pro Tipp:</u>** Führen Sie die folgenden Schritte für die iOS/iPadOS-Implementierung aus.

- Der Antrag müsste [Logout starten](/help/authentication/iostvos-sdk-api-reference.md#logout) vom AccessEnabler iOS/tvOS SDK. Dies würde die Sitzungsbereinigung auf der MVPD-Seite erleichtern.
- Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von abzumelden *`Settings -> TV Provider`* nur auf iOS/iPadOS [*VSA203* Statuscode wird ausgelöst](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
