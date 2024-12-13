---
title: Apple SSO-Cookbook (iOS/tvOS SDK)
description: Apple SSO-Cookbook (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1812'
ht-degree: 0%

---

# (Legacy) Apple SSO-Cookbook (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Der Adobe Pass Authentication AccessEnabler iOS/tvOS SDK unterstützt das Partner Single Sign-On (SSO) für Endbenutzer von Client-Anwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden.

Dieses Dokument dient als Erweiterung der bestehenden Dokumentation zu AccessEnabler iOS/tvOS SDK, die Sie ([) ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md).

## Cookbook {#apple-sso-cookbook-iostvos-sdk-cookbook}

Um von dem Apple SSO-Benutzererlebnis zu profitieren, muss die Anwendung den AccessEnabler iOS/tvOS SDK integrieren und die unten beschriebenen Schritte befolgen.

### Voraussetzungen {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### Erlaubnis {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Die Streaming-Anwendung muss Zugriff auf die auf Geräteebene gespeicherten Abonnementinformationen des Benutzers anfordern, für die der Benutzer der Anwendung die Erlaubnis zum Fortfahren erteilen muss, ähnlich wie bei der Bereitstellung von Zugriff auf die Kamera oder das Mikrofon des Geräts. Diese Berechtigung muss pro Anwendung über das Apple-[Video-Abonnementkonto-Framework](https://developer.apple.com/documentation/videosubscriberaccount) angefordert werden und das Gerät speichert die Benutzerauswahl.

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Wir empfehlen, Benutzern, die sich weigern, Berechtigungen für den Zugriff auf Abonnementinformationen zu erteilen, Anreize zu bieten, indem sie die Vorteile des Apple-Single-Sign-On-Benutzererlebnisses erklären. Beachten Sie jedoch, dass die Benutzenden ihre Entscheidung ändern können, indem sie zu den Anwendungseinstellungen gehen (Zugriff auf TV-Anbieter) oder auf iOS und iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS *`Settings -> TV Provider`*.

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Die Streaming-Anwendung kann die Berechtigung des Benutzers anfordern, wenn die Anwendung in den Vordergrundstatus wechselt, da die Anwendung jederzeit auf die Abonnementinformationen des ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) zugreifen [Berechtigung für den Zugriff) kann, bevor eine Benutzerauthentifizierung erforderlich wird.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Falls der Benutzer keinen Zugriff auf seine Abonnementinformationen gewährt oder die Kommunikation mit dem Video Subscriber Account Framework fehlschlägt, fällt der AccessEnabler iOS/tvOS SDK auf den regulären Authentifizierungsfluss zurück.

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

#### Callbacks {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Implementieren Sie die folgende Liste von [Callbacks](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) die spezifisch für den Apple SSO-Workflow sind.

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Rückruf, der ausgelöst wird, wenn die Apple MVPD-Auswahl geöffnet wird.
* [*dismissTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Rückruf, der ausgelöst wird, wenn die Apple MVPD-Auswahl geschlossen wird.

#### Fehlerberichterstattung {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>Profi-Tipp</u>** Implementieren Sie die folgende Liste von [erweiterten Fehler-Codes](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) die für den Apple SSO-Workflow spezifisch sind.

* ***N003*** - Der Benutzer hat die Option „Andere TV-Anbieter“ in der Apple MVPD-Auswahl ausgewählt.
* ***N004*** - Der Benutzer hat in der Apple MVPD-Auswahl einen TV-Anbieter ausgewählt, der vom aktuellen Anforderer nicht unterstützt wird (Integration oder Single Sign-On deaktiviert).
* ***N005*** - Der Benutzer hat entschieden, die reguläre MVPD- oder Apple MVPD-Auswahl abzubrechen.
* ***VSA403*** - Die Berechtigung des TV-Anbieters des Benutzers für die Anwendung wird verweigert.
* ***VSA404*** - Die Berechtigung des TV-Anbieters des Benutzers für die Anwendung ist unbestimmt.
* ***VSA503*** - Die Metadatenanfrage für das Videoabonnentenkonto ist fehlgeschlagen. Im Feld *Nachricht* wird mehr Kontext bereitgestellt.
* ***AAPL / APPL_ERROR*** - Die Metadatenanfrage für das Videoabonnentenkonto ist fehlgeschlagen. Weitere Kontextinformationen finden *im*.

### Authentifizierung {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>Tipp</u>** Führen Sie für die Implementierung(en) von iOS/iPadOS/tvOS die folgenden Schritte aus.

1. Die Anwendung müsste [ AccessEnabler](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) iOS/tvOS SDK initialisieren.


1. Die Anwendung müsste [die aktuelle Anfordererkennung festlegen](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Wichtig:** In diesem zweiten Schritt kann ein Trigger [erweiterter Fehlercode](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) auftreten, der spezifisch für den Apple SSO-Workflow ist, falls **einer der folgenden Punkte zutrifft**:

   * ***VSA403*** - Die Berechtigung des TV-Anbieters des Benutzers für die Anwendung wird verweigert.
   * ***VSA404*** - Die Berechtigung des TV-Anbieters des Benutzers für die Anwendung ist unbestimmt.
   * ***APPL*** - Bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Videoabonnentenkonto-Framework ist ein Fehler aufgetreten.

   In diesem zweiten Schritt wird versucht, das Apple SSO-Profil im Hintergrund gegen ein Adobe-Authentifizierungstoken einzutauschen, falls **alle oben genannten Werte** sind und **alle folgenden Werte zutreffen**:

   * Die Berechtigung des TV-Anbieters des Benutzers für die Anwendung wird gewährt.
   * Der Benutzer ist auf der Systemebene des Geräts bei seinem TV-Anbieterkonto angemeldet.
   * AccessEnabler iOS/tvOS SDK hat die TV Provider-Kennung des Benutzers aus dem Video-Abonnementkonto-Framework erhalten.
   * Die Integration des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE-Dashboard aktiviert.
   * Das Single Sign-On des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE-Dashboard aktiviert.
   * Der TV-Anbieter des Benutzers wird nicht über das Adobe Primetime TVE-Dashboard beeinträchtigt.
   * AccessEnabler iOS/tvOS SDK erhielt vom Videoabonnentenkonto-Framework eine SAML-Antwort des TV-Anbieters des Benutzers.

   **<u>Profi-Tipp:</u>** In diesem zweiten Schritt werden außer dem Callback [setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) keine weiteren Callbacks Trigger, da die Authentifizierung nicht explizit von der Anwendung initiiert wurde.


1. Die Anwendung müsste [den Authentifizierungsstatus überprüfen](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Wichtig:** Dieser dritte Schritt kann einen Trigger [erweiterten Fehlercode](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) enthalten, der spezifisch für den Apple SSO-Workflow ist, falls **einer der folgenden Punkte zutrifft**:

   * ***VSA403** - Der Benutzer ist bei seinem TV-Provider-Konto angemeldet unter
auf der Systemebene des Geräts, aber mit der Berechtigung des TV-Anbieters des Benutzers
Für die Anwendung verweigert.
   * ***VSA404** - Der Benutzer ist bei seinem TV-Provider-Konto angemeldet unter
auf der Systemebene des Geräts, aber mit der Berechtigung des TV-Anbieters des Benutzers
ist für die Anwendung unbestimmt.
   * ***APPL\_ERROR** - Der Benutzer ist bei seinem TV-Anbieter angemeldet
Konto auf der Geräte-Systemebene, aber die Kommunikation zwischen
AccessEnabler iOS/tvOS SDK und das Videoabonnentenkonto
Beim Framework ist ein Fehler aufgetreten.

   **Wichtig:** Dieser dritte Schritt führt zu einem Trigger des [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)-Callbacks mit *status* gleich 0, falls **einer der folgenden Werte zutrifft**:

   * Der Benutzer ist nicht auf der Systemebene des Geräts oder über einen regulären Authentifizierungsfluss bei seinem TV-Anbieter-Konto angemeldet.
   * Der Benutzer wird auf der Systemebene des Geräts oder über einen regulären Authentifizierungsfluss bei seinem TV-Anbieter-Konto angemeldet, aber die TTL des TV-Anbieter-Authentifizierungstokens des Benutzers wurde bestanden.
   * Die Benutzerin bzw. der Benutzer wird auf Systemebene des Geräts oder über einen regulären Authentifizierungsfluss bei ihrem TV-Anbieter-Konto angemeldet. Die TV-Anbieter-Integration der Benutzerin bzw. des Benutzers in die Anwendung wird jedoch über das Adobe Primetime TVE-Dashboard deaktiviert.
   * Die Benutzerin bzw. der Benutzer wird auf Systemebene des Geräts bei ihrem TV-Anbieterkonto angemeldet, aber das einmalige Anmelden der Benutzerin bzw. des TV-Anbieters mit der Anwendung wird über das Adobe Primetime TVE-Dashboard deaktiviert.
   * Der Benutzer wird auf Systemebene des Geräts bei seinem TV-Anbieter-Konto angemeldet, aber die TV-Anbieter-Berechtigung des Benutzers wird für die Anwendung verweigert.
   * Der Benutzer ist auf der Systemebene des Geräts bei seinem TV-Anbieter-Konto angemeldet, aber die Berechtigung des Benutzers für den TV-Anbieter für die Anwendung ist unbestimmt.
   * Der Benutzer ist auf der Systemebene des Geräts bei seinem TV-Anbieterkonto angemeldet, aber bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Video-Teilnehmerkonto-Framework ist ein Fehler aufgetreten.

   **Wichtig:** Dieser dritte Schritt führt zu einem Trigger des [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)-Callbacks mit *status* gleich 1, falls **alle oben genannten Werte „false“ sind.**


1. Die Anwendung müsste [die Authentifizierung initialisieren](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) falls die vorherige Prüfung des Authentifizierungsstatus den [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)-Rückruf mit *status* gleich 0 ausgelöst hat.

   **<u>Profi-Tipp:</u>** Implementieren Sie eine der folgenden AccessEnabler iOS/tvOS SDK API [getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) oder [getAuthentication:filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Wichtig:** Dieser vierte Schritt kann einen Trigger [erweiterten Fehlercode](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) enthalten, der spezifisch für den Apple SSO-Workflow ist, falls **einer der folgenden Punkte zutrifft**:

   * ***VSA403*** - Die Berechtigung des TV-Anbieters des Benutzers für die Anwendung wird verweigert.
   * ***VSA404*** - Die Berechtigung des TV-Anbieters des Benutzers für die Anwendung ist unbestimmt.
   * ***VSA503*** - Bei der Kommunikation zwischen dem AccessEnabler iOS/tvOS SDK und dem Videoabonnentenkonto-Framework ist ein Fehler aufgetreten.
   * ***N003*** - Der Benutzer hat die Option „Andere TV-Anbieter“ in der Apple MVPD-Auswahl ausgewählt.
   * ***N004*** - Der Benutzer hat in der Apple MVPD-Auswahl einen TV-Anbieter ausgewählt, der vom aktuellen Anforderer nicht unterstützt wird (Integration oder Single Sign-On deaktiviert).
   * ***N005*** - Der Benutzer hat entschieden, die reguläre MVPD- oder Apple MVPD-Auswahl abzubrechen.

   **Wichtig:** Dieser vierte Schritt würde auf den regulären Authentifizierungsfluss zurückgreifen, indem der [displayProviderDialog](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog)-Rückruf und **einer** der oben [erweiterten Fehler-Codes](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) ausgelöst werden, falls **einer der oben genannten wahr ist**.

   **Wichtig:** Dieser vierte Schritt würde auf den regulären Authentifizierungsfluss zurückgreifen, indem der [navigateToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url)- oder [navigateToUrl:useSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC)-Rückruf und **none** der [erweiterten Fehlercodes](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) ausgelöst wird, falls der Benutzer einen TV-Anbieter ausgewählt hat, der Apple SSO nicht unterstützt, aber in der Apple MVPD-Auswahl vorhanden ist.

   **<u>Profi-Tipp:</u>** Die AccessEnabler iOS/tvOS SDK ruft die API [setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) im Hintergrund auf, falls der Benutzer einen TV-Anbieter ausgewählt hat, der Apple SSO nicht unterstützt, aber in der Apple MVPD-Auswahl vorhanden ist.

   **Wichtig:** Dieser vierte Schritt würde versuchen, das Apple-SSO-Profil im Hintergrund gegen ein Adobe-Authentifizierungstoken einzutauschen, falls **alle oben genannten Punkte falsch sind** und **alle folgenden Punkte zutreffen**:

   * Die Berechtigung des TV-Anbieters des Benutzers für die Anwendung wird gewährt.
   * Der Benutzer ist auf der Systemebene des Geräts bei seinem TV-Anbieterkonto angemeldet bzw. meldet sich derzeit an.
   * AccessEnabler iOS/tvOS SDK hat die TV Provider-Kennung des Benutzers aus dem Video-Abonnementkonto-Framework erhalten.
   * Die Integration des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE-Dashboard aktiviert.
   * Das Single Sign-On des TV-Anbieters des Benutzers mit der Anwendung wird über das Adobe Primetime TVE-Dashboard aktiviert.
   * Der TV-Anbieter des Benutzers wird nicht über das Adobe Primetime TVE-Dashboard beeinträchtigt.
   * AccessEnabler iOS/tvOS SDK erhielt vom Videoabonnentenkonto-Framework eine SAML-Antwort des TV-Anbieters des Benutzers.

   **<u>Profi-Tipp:</u>** Dieser vierte Schritt führt unabhängig vom Ergebnis des [*statusStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus) *zu einem Trigger,* die Authentifizierung explizit von der Anwendung initiiert wurde.

### Metadaten {#apple-sso-cookbook-iostvos-sdk-metadata}

Die Anwendung hat die Möglichkeit, mithilfe der API &quot;*tokenSource“ (Benutzermetadaten) aus der AccessEnabler iOS SDK/tvOS-* zu ermitteln, ob die Authentifizierung als Ergebnis [ Anmeldung über ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) Partner-SSO erfolgt ist oder nicht.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### Abmelden {#apple-sso-cookbook-iostvos-sdk-logout}

Das [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) stellt keine API zum programmgesteuerten Abmelden von Personen bereit, die sich auf Gerätesystemebene bei ihrem TV Provider-Konto angemeldet haben. Damit die Abmeldung ihre volle Wirkung entfalten kann, muss sich der Endbenutzer daher explizit von *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS abmelden. Die andere Option, die der Benutzer haben könnte, besteht darin, dem Benutzer die Berechtigung zum Zugriff auf die Abonnementinformationen des Benutzers im Abschnitt mit den spezifischen Anwendungseinstellungen zu entziehen (Zugriff auf TV Provider-Berechtigungen).

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die AccessEnabler iOS/tvOS SDK [Logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) API.

>[!TIP]
>
> **<u>Profi-Tipp</u>** Führen Sie die folgenden Schritte für die tvOS-Implementierung(en) aus.

* Die Anwendung müsste [Abmelden) ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) AccessEnabler iOS/tvOS SDK starten. Dies würde die Sitzungsbereinigung auf MVPD-Seite nicht erleichtern.
* Die Anwendung muss den Benutzer anweisen/auffordern, sich nur im Falle eines Auslösens des Status-Codes [*VSA203* explizit von *`Settings -> Accounts -> TV Provider`* auf tvOS abzumelden](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).

>[!TIP]
>
> **<u>Profi-Tipp</u>** Führen Sie die folgenden Schritte für die Implementierung(en) von iOS/iPadOS aus.

* Die Anwendung müsste [Abmelden) ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) AccessEnabler iOS/tvOS SDK starten. Dies würde die Sitzungsbereinigung auf MVPD-Seite erleichtern.
* Die Anwendung müsste den Benutzer nur dann anweisen/auffordern, sich explizit von *`Settings -> TV Provider`* auf iOS/iPadOS abzumelden, wenn der [*VSA203*-Statuscode ausgelöst wird](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).
