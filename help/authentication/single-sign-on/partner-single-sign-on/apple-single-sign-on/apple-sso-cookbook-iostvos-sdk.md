---
title: Apple SSO-Cookbook (iOS/tvOS SDK)
description: Apple SSO-Cookbook (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1811'
ht-degree: 0%

---

# Apple SSO-Cookbook (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Das Adobe Pass Authentication AccessEnabler iOS/tvOS SDK unterstützt Single Sign-On (SSO) für Endbenutzer von Clientanwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden.

Dieses Dokument dient als Erweiterung der vorhandenen AccessEnabler iOS/tvOS SDK-Dokumentation, die Sie [hier](/help/authentication/iostvos-sdk-api-reference.md) finden.

## Cookbook {#apple-sso-cookbook-iostvos-sdk-cookbook}

Um das Apple SSO-Benutzererlebnis nutzen zu können, muss die Anwendung das AccessEnabler iOS/tvOS SDK integrieren und die unten beschriebene Abfolge von Schritten befolgen.

### Voraussetzungen {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### Berechtigung {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>Pro Tipp:</u>** Die Streaming-Anwendung muss den Zugriff auf die auf Geräteebene gespeicherten Abonnementinformationen des Benutzers anfordern, für die der Benutzer der Anwendung die Berechtigung zum Fortfahren erteilen muss, ähnlich wie bei der Bereitstellung des Zugriffs auf die Kamera oder das Mikrofon des Geräts. Diese Berechtigung muss pro Anwendung mit dem [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) von Apple angefordert werden, und das Gerät speichert die Benutzerauswahl.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Es wird empfohlen, Benutzer, die sich weigern, Zugriff auf Abonnementinformationen zu gewähren, dazu anzuregen, die Vorteile des Single-Sign-On-Benutzererlebnisses für Apple zu erläutern. Beachten Sie jedoch, dass der Benutzer seine Entscheidung ändern kann, indem er zu den Anwendungseinstellungen (Zugriffsberechtigung für TV-Anbieter) oder zu *`Settings -> TV Provider`* unter iOS und iPadOS oder *`Settings -> Accounts -> TV Provider`* unter tvOS navigiert.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Die Streaming-Anwendung kann die Berechtigung des Benutzers anfordern, wenn die Anwendung in den Vordergrundzustand wechselt, da die Anwendung zu jedem Zeitpunkt überprüfen kann, ob die Anwendung über die Berechtigung zum Zugriff auf die Abonnementinformationen des Benutzers verfügt, bevor eine Benutzerauthentifizierung erforderlich ist.[](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)

>[!TIP]
>
> **<u>Pro Tipp:</u>** Wenn der Benutzer keinen Zugriff auf seine Abonnementinformationen gewährt oder die Kommunikation mit dem Video Subscriber Account Framework fehlschlägt, kehrt das AccessEnabler iOS/tvOS SDK zum regulären Authentifizierungsfluss zurück.

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

#### Rückrufe {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>Pro Tipp:</u>** Implementieren Sie die folgende Liste von [Callbacks](/help/authentication/iostvos-sdk-api-reference.md), die speziell für den Apple SSO-Workflow gelten.

* [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Callback wird ausgelöst, wenn die Apple-MVPD-Auswahl geöffnet wird.
* [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Callback ausgelöst, wenn die Apple-MVPD-Auswahl geschlossen wird.

#### Fehlerberichte {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>Pro Tipp:</u>** Implementieren Sie die folgende Liste von [erweiterten Fehlercodes](/help/authentication/error-reporting.md), die speziell für den Apple SSO-Workflow gelten.

* ***N003*** - Der Benutzer hat in der Apple-MVPD-Auswahl die Option &quot;Andere TV-Anbieter&quot;ausgewählt.
* ***N004*** - Der Benutzer hat einen Fernsehanbieter aus der Apple-MVPD-Auswahl ausgewählt, der vom aktuellen Anfragenden nicht unterstützt wird (Integration oder Single-Sign-On ist deaktiviert).
* ***N005*** - Der Benutzer hat beschlossen, die normale MVPD-Auswahl oder Apple-MVPD-Auswahl abzubrechen.
* ***VSA403*** - Die TV Provider-Berechtigung des Benutzers für die Anwendung wird verweigert.
* ***VSA404*** - Die TV Provider-Berechtigung des Benutzers wird für die Anwendung nicht festgelegt.
* ***VSA503*** - Die Metadatenanforderung für das Videoponnentenkonto ist fehlgeschlagen. Im Feld *message* wird mehr Kontext bereitgestellt.
* ***AAPL / APPL_ERROR*** - Die Metadatenanfrage für das Video-Abonnentenkonto ist fehlgeschlagen. Im Feld *Details* wird mehr Kontext bereitgestellt.

### Authentifizierung {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>Tipp:</u>** Gehen Sie wie folgt vor, um die iOS/iPadOS/tvOS-Implementierung/en zu implementieren.

1. Die Anwendung muss das AccessEnabler iOS/tvOS-SDK [initialisieren](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement).


1. Die Anwendung muss [die aktuelle Anfragenkennung ](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3) festlegen.

   **Wichtig:** Mit diesem zweiten Schritt wird möglicherweise ein [erweiterter Fehlercode](/help/authentication/error-reporting.md) Trigger, der für den Apple SSO-Workflow spezifisch ist, falls **einer der folgenden Werte wahr** ist:

   * ***VSA403*** - Die TV Provider-Berechtigung des Benutzers für die Anwendung wird verweigert.
   * ***VSA404*** - Die TV Provider-Berechtigung des Benutzers wird für die Anwendung nicht festgelegt.
   * ***APPL*** - Bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Video Subscriber Account Framework wurde ein Fehler festgestellt.

   In diesem zweiten Schritt wird versucht, das Apple SSO-Profil still gegen ein Adobe-Authentifizierungstoken zu tauschen, falls **alle oben genannten Werte falsch** und **alle folgenden Werte wahr** sind:

   * Die TV Provider-Berechtigung des Benutzers wird für die Anwendung gewährt.
   * Der Benutzer ist auf Geräteebene bei seinem TV-Provider-Konto angemeldet.
   * Das AccessEnabler iOS/tvOS SDK hat die TV Provider-Kennung des Benutzers aus dem Video Subscriber Account Framework erhalten.
   * Die Integration des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE Dashboard aktiviert.
   * Das Single-Sign-On des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE-Dashboard aktiviert.
   * Der Fernsehanbieter des Benutzers wird nicht über das Adobe Primetime TVE-Dashboard beschädigt.
   * Das AccessEnabler iOS/tvOS SDK hat die SAML-Antwort des Fernsehanbieters des Benutzers vom Video Subscriber Account Framework erhalten.

   **<u>Pro Tipp:</u>** In diesem zweiten Schritt werden neben dem Rückruf [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) keine weiteren Rückrufe Trigger, da die Authentifizierung nicht explizit von der Anwendung initiiert wurde.


1. Die Anwendung muss [den Authentifizierungsstatus](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn) überprüfen.

   **Wichtig:** Mit diesem dritten Schritt kann ein [erweiterter Fehlercode](/help/authentication/error-reporting.md) Trigger werden, der für den Apple SSO-Workflow spezifisch ist, falls **einer der folgenden Werte wahr** ist:

   * ***VSA403** - Der Benutzer ist bei seinem TV-Provider-Konto unter angemeldet.
die Systemebene des Geräts, aber die Berechtigung des TV-Anbieters des Benutzers lautet
abgelehnt wurde.
   * ***VSA404** - Der Benutzer ist bei seinem TV-Provider-Konto unter angemeldet.
die Systemebene des Geräts, aber die Berechtigung des TV-Anbieters des Benutzers
für die Anwendung unbestimmt ist.
   * ***APPL\_ERROR** - Der Benutzer ist bei seinem TV-Anbieter angemeldet.
-Konto auf der Gerätesystemebene, aber die Kommunikation zwischen
das AccessEnabler iOS/tvOS-SDK und das Video-Abonnentenkonto
Framework ist ein Fehler aufgetreten.

   **Wichtig:** Dieser dritte Schritt Trigger den Rückruf [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) mit *status* gleich 0, falls **einer der folgenden Werte wahr** ist:

   * Der Benutzer ist nicht beim TV-Provider-Konto auf Gerätesystemebene oder über einen normalen Authentifizierungsfluss angemeldet.
   * Der Benutzer ist auf Gerätesystemebene oder über einen normalen Authentifizierungsfluss bei seinem TV Provider-Konto angemeldet, aber die TTL des TV Provider-Authentifizierungstokens des Benutzers wurde übergeben.
   * Der Benutzer ist auf Gerätesystemebene oder über einen regelmäßigen Authentifizierungsfluss bei seinem TV-Provider-Konto angemeldet, aber die Integration des TV-Anbieters des Benutzers mit der Anwendung ist über das Adobe Primetime TVE-Dashboard deaktiviert.
   * Der Benutzer ist auf Geräteebene bei seinem TV-Provider-Konto angemeldet, aber die Single-Sign-On-Funktion des TV-Anbieters des Benutzers mit der Anwendung ist über das Adobe Primetime TVE-Dashboard deaktiviert.
   * Der Benutzer ist auf Geräteebene beim TV-Provider-Konto angemeldet, aber die TV-Provider-Berechtigung des Benutzers für die Anwendung wird verweigert.
   * Der Benutzer ist auf Gerätesystemebene bei seinem TV-Provider-Konto angemeldet, aber die TV Provider-Berechtigung des Benutzers für die Anwendung wird nicht festgelegt.
   * Der Benutzer ist auf Geräteebene beim TV-Provider-Konto angemeldet, bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Video Subscriber Account Framework ist jedoch ein Fehler aufgetreten.

   **Wichtig:** Dieser dritte Schritt Trigger den Rückruf [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) mit *status* gleich 1, falls **alle oben genannten Werte falsch sind.**


1. Die Anwendung muss [die Authentifizierung initialisieren](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn), falls die vorherige Authentifizierungsstatusprüfung den Rückruf [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) ausgelöst hat, bei dem *status* gleich 0 ist.

   **<u>Pro Tipp:</u>** Implementieren Sie eine der folgenden AccessEnabler iOS/tvOS SDK API [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) oder [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Wichtig:** Mit diesem vierten Schritt kann ein [erweiterter Fehlercode](/help/authentication/error-reporting.md) Trigger werden, der für den Apple SSO-Workflow spezifisch ist, falls **einer der folgenden Werte wahr** ist:

   * ***VSA403*** - Die TV Provider-Berechtigung des Benutzers für die Anwendung wird verweigert.
   * ***VSA404*** - Die TV Provider-Berechtigung des Benutzers wird für die Anwendung nicht festgelegt.
   * ***VSA503*** - Bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Video Subscriber Account Framework wurde ein Fehler festgestellt.
   * ***N003*** - Der Benutzer hat in der Apple-MVPD-Auswahl die Option &quot;Andere TV-Anbieter&quot;ausgewählt.
   * ***N004*** - Der Benutzer hat einen Fernsehanbieter aus der Apple-MVPD-Auswahl ausgewählt, der vom aktuellen Anfragenden nicht unterstützt wird (Integration oder Single-Sign-On ist deaktiviert).
   * ***N005*** - Der Benutzer hat beschlossen, die normale MVPD-Auswahl oder Apple-MVPD-Auswahl abzubrechen.

   **Wichtig:** Dieser vierte Schritt würde zum normalen Authentifizierungsfluss zurückfallen, indem der Rückruf [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) und der Rückruf **one** der oben genannten [erweiterten Fehlercodes](/help/authentication/error-reporting.md) ausgelöst werden, falls **einer der oben genannten Werte &quot;true**&quot;ist.

   **Wichtig:** Dieser vierte Schritt kehrt zum normalen Authentifizierungsfluss zurück, indem der Rückruf [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) oder [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) und der Rückruf **none** der oben genannten [erweiterten Fehlercodes](/help/authentication/error-reporting.md) ausgelöst werden, falls der Benutzer einen TV-Anbieter ausgewählt hat, der Apple SSO nicht unterstützt, aber in der Apple-MVPD-Auswahl.

   **<u>Pro Tipp:</u>** Das AccessEnabler iOS/tvOS-SDK ruft die API [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) still auf, falls der Benutzer einen TV-Anbieter ausgewählt hat, der die Apple-SSO nicht unterstützt, aber in der Apple-MVPD-Auswahl vorhanden ist.

   **Wichtig:** Dieser vierte Schritt würde versuchen, das Apple SSO-Profil still gegen ein Adobe-Authentifizierungstoken zu tauschen, falls **alle oben genannten Werte falsch** und **alle folgenden Werte wahr** sind:

   * Die TV Provider-Berechtigung des Benutzers wird für die Anwendung gewährt.
   * Der Benutzer ist angemeldet/meldet sich derzeit beim TV Provider-Konto auf Gerätesystemebene an.
   * Das AccessEnabler iOS/tvOS SDK hat die TV Provider-Kennung des Benutzers aus dem Video Subscriber Account Framework erhalten.
   * Die Integration des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE Dashboard aktiviert.
   * Das Single-Sign-On des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE-Dashboard aktiviert.
   * Der Fernsehanbieter des Benutzers wird nicht über das Adobe Primetime TVE-Dashboard beschädigt.
   * Das AccessEnabler iOS/tvOS SDK hat die SAML-Antwort des Fernsehanbieters des Benutzers vom Video Subscriber Account Framework erhalten.

   **<u>Pro Tipp:</u>** Dieser vierte Schritt Trigger den Rückruf [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus), unabhängig vom Ergebnis *status* , da die Authentifizierung explizit von der Anwendung initiiert wurde.

### Metadaten {#apple-sso-cookbook-iostvos-sdk-metadata}

Die Anwendung kann mithilfe der API &quot;*tokenSource&quot;* [Benutzermetadaten](/help/authentication/iostvos-sdk-api-reference.md#getMeta)&quot;aus dem AccessEnabler iOS/tvOS SDK bestimmen, ob die Authentifizierung durch eine Anmeldung über die Partner-SSO erfolgte oder nicht.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### Abmelden {#apple-sso-cookbook-iostvos-sdk-logout}

Das [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) bietet keine API zum programmgesteuerten Abmelden von Personen, die sich auf Geräteebene bei ihrem TV-Anbieterkonto angemeldet haben. Damit die Abmeldung vollständig wirksam wird, muss sich der Endbenutzer daher explizit von *`Settings -> TV Provider`* bei iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* bei tvOS abmelden. Die andere Option, die der Benutzer haben würde, besteht darin, die Berechtigung zum Zugriff auf die Abonnementinformationen des Benutzers aus dem Abschnitt für die spezifischen Anwendungseinstellungen (Zugriffsberechtigungen des TV-Anbieters) zu entziehen.

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die AccessEnabler iOS/tvOS SDK [logout](/help/authentication/iostvos-sdk-api-reference.md#logout) -API.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die tvOS-Implementierung/en durchzuführen.

* Die Anwendung muss [die Abmeldung ](/help/authentication/iostvos-sdk-api-reference.md#logout) vom AccessEnabler iOS/tvOS SDK initiieren. Dies würde die Sitzungsbereinigung auf der MVPD-Seite nicht erleichtern.
* Die Anwendung muss den Benutzer anweisen/auffordern, sich nur dann explizit von *`Settings -> Accounts -> TV Provider`* unter tvOS abzumelden, wenn der Statuscode [*VSA203* ausgelöst wird](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die iOS/iPadOS-Implementierung/iPadOS zu implementieren.

* Die Anwendung muss [die Abmeldung ](/help/authentication/iostvos-sdk-api-reference.md#logout) vom AccessEnabler iOS/tvOS SDK initiieren. Dies würde die Sitzungsbereinigung auf der MVPD-Seite erleichtern.
* Die Anwendung muss den Benutzer anweisen/auffordern, sich unter iOS/iPadOS nur dann explizit von *`Settings -> TV Provider`* abzumelden, wenn der Statuscode [*VSA203* ausgelöst wird](/help/authentication/error-reporting.md).
