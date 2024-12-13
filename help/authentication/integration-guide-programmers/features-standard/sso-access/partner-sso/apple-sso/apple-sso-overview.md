---
title: Apple SSO - Übersicht
description: Apple SSO - Übersicht
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Apple SSO - Übersicht {#apple-sso-overview}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Apple bietet Benutzenden die Möglichkeit, sich auf der Systemebene des Geräts bei ihrem TV-Anbieterkonto anzumelden, sodass sie sich nicht mehr App für App authentifizieren müssen.

Die Adobe Pass-Authentifizierung ist eine Partnerschaft mit Apple eingegangen, um das Partner Single Sign-On (SSO)-Benutzererlebnis im TV Everywhere-Ökosystem für TV-Besitzer von iPhone, iPad und Apple zu schaffen.

Um das Single Sign-On (SSO)-Benutzererlebnis auf einem Apple-Gerät nutzen zu können, müssen Sie eine Liste der folgenden Voraussetzungen erfüllen.

Das Endergebnis sollte ein Erlebnis schaffen, das den folgenden Benutzerabläufen entspricht. Wir empfehlen Ihnen, es zu konsultieren, bevor Sie mit der Entwicklung Ihrer Anwendung beginnen:

* Single Sign-On (SSO) [Benutzerflüsse für iPhone- und iPad](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)-Geräte.
* Single Sign-On (SSO) [Benutzerflüsse für Apple TV](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)-Geräte.

## Voraussetzungen {#apple-sso-prerequisites}

Die Voraussetzungen für das Onboarding können für eine oder mehrere Entitäten gelten, die am TVE-Geschäft beteiligt sind, z. B. Programmierer, MVPDs, Adobe Pass-Authentifizierung oder Apple.

### Programmierer {#apple-sso-prerequisites-programmer}

Um von dem Single Sign-On (SSO)-Benutzererlebnis zu profitieren, muss ein Programmierer:

* Wenden Sie sich an Apple, um das [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) als Teil Ihrer Apple Team ID zu aktivieren, und konfigurieren Sie die [Berechtigung für Videoabonnenten-Single-Sign-On](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) als Teil Ihres Apple-Entwicklerkontos.

   * Verwenden Sie Xcode ab Version 8 und iOS/tvOS ab Version 10.

* Aktivieren Sie Single Sign-On (SSO) für jede gewünschte Integration und Plattform (iOS/tvOS) über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) indem Sie die `Enable Single Sign On` Eigenschaft auf `Yes` setzen.

| Adobe Single Sign-On aktivieren | Apple **Onboarded (Supported)** MVPDs | Apple **Picker** MVPDs | Apple **Nicht integriert (nicht unterstützt)** MVPDs |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Ja (aktiviert) | Die Authentifizierungs- und Abmeldevorgänge umfassen sowohl Apple- als auch Adobe Pass-Authentifizierungslösungen, während alle anderen Vorgänge (Autorisierung, Vorautorisierung, Metadaten usw.) ausschließlich von der Adobe Pass-Authentifizierung abgewickelt werden. | Die Authentifizierungs- und Abmeldeflüsse werden auf die regulären Flüsse zurückgesetzt, die ausschließlich von der Adobe Pass-Authentifizierung bedient werden. | Die Authentifizierungs- und Abmeldeflüsse werden auf die regulären Flüsse zurückgesetzt, die ausschließlich von der Adobe Pass-Authentifizierung bedient werden. |
| Nein (deaktiviert) | Die Authentifizierungs- und Abmeldeflüsse werden auf die regulären Flüsse zurückgesetzt, die ausschließlich von der Adobe Pass-Authentifizierung bedient werden. | Die Authentifizierungs- und Abmeldeflüsse werden auf die regulären Flüsse zurückgesetzt, die ausschließlich von der Adobe Pass-Authentifizierung bedient werden. | Die Authentifizierungs- und Abmeldeflüsse werden auf die regulären Flüsse zurückgesetzt, die ausschließlich von der Adobe Pass-Authentifizierung bedient werden. |

* Integrieren Sie die Single Sign-On (SSO)-Benutzerflüsse mit einer der folgenden Lösungen, die die Adobe Pass-Authentifizierung für Endbenutzer von Client-Anwendungen bietet, die auf iOS, iPadOS oder tvOS ausgeführt werden.

   * Die Adobe Pass-Authentifizierungs-REST-API v2 unterstützt das Partner Single Sign-On (SSO).

     Weitere Informationen finden Sie in der Dokumentation zum [Apple SSO Cookbook (REST API V2](apple-sso-cookbook-rest-api-v2.md).

   * Die alte Adobe Pass-Authentifizierungs-REST-API v1 unterstützt das Partner Single Sign-On (SSO).

     Weitere Informationen finden Sie in der Dokumentation zum [ (Legacy) Apple SSO Cookbook (REST API V1](../../../../legacy/sso-access/apple-sso-cookbook-rest-api-v1.md).

   * Die alte Adobe Pass Authentication Access Enabler iOS/tvOS SDK unterstützt das Partner Single Sign-On (SSO).

     Weitere Informationen finden Sie in der Dokumentation [(Legacy) Apple SSO Cookbook (iOS/tvOS SDK](../../../../legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md) .

### MVPD {#apple-sso-prerequisites-mvpd}

Um von dem Single Sign-On (SSO)-Benutzererlebnis zu profitieren, muss eine MVPD folgende Voraussetzungen erfüllen:

* Wenden Sie sich an Apple, um den Onboarding-Prozess für Apple einzuleiten.

   * Fragen Sie die technische Dokumentation an, wie eine JavaScript-TVML-Anwendung integriert und entwickelt werden kann, die mit dem Benutzeranmeldeformular umgehen kann.

* Kontaktieren Sie die Adobe Pass-Authentifizierung, um den Onboarding-Prozess auf der Adobe-Seite zu starten.

   * Geben Sie den Zeichenfolgenwert an, der die Kennung des TV-Anbieters darstellt, die von Apple während des Onboarding-Prozesses zugewiesen wurde.

## FAQs {#FAQ}

* Kann die Anwendung, die den Adobe Pass Authentication AccessEnabler iOS/tvOS SDK verwendet, auf den regulären Authentifizierungsfluss zurückgreifen, falls beim Apple SSO-Workflow Probleme auftreten?

  Dies ist möglich, erfordert jedoch eine Konfigurationsänderung über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication), um das **Single Sign-On aktivieren** auf **NEIN** für die gewünschte Integration und Plattform (iOS/tvOS) festzulegen. Beachten Sie, dass die Client-Anwendung die Konfigurationsänderung nur nach dem Aufruf der API [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) bestätigt.


* Weiß die Anwendung, wann eine Authentifizierung als Ergebnis einer Anmeldung über Apple SSO stattgefunden hat?

  Diese Informationen sind als Teil des Benutzer-Metadatenschlüssels verfügbar: *tokenSource*, der in diesem Fall den Zeichenfolgenwert &quot;Apple&quot; zurückgeben sollte.


* Weiß die Anwendung, wann eine Authentifizierung als Ergebnis einer Anmeldung über Apple SSO bei einer anderen Anwendung stattgefunden hat?

  Diese Informationen sind nicht verfügbar.


* Was passiert, wenn sich ein Benutzer mit einem MVPD, der nicht in die Anwendung integriert ist, beim *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS anmeldet?

  Wenn der Benutzer die Anwendung startet, wird der Benutzer nicht über den Apple SSO-Workflow authentifiziert. Daher muss die Anwendung auf den regulären Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.


* Was passiert, wenn sich ein(e) Benutzende(r) anmeldet, indem er/sie den Abschnitt *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS mit einem MVPD verwendet, bei dem **Single Sign On aktivieren** auf **NEIN** über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) für iOS/tvOS eingestellt ist?

  Wenn der Benutzer die Anwendung startet, wird der Benutzer nicht über den Apple SSO-Workflow authentifiziert. Daher muss die Anwendung auf den regulären Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.


* Was passiert, wenn ein Benutzer über eine MVPD verfügt, die nicht von Apple integriert (nicht unterstützt) wird, aber in der Apple-Auswahl vorhanden ist?

  Wenn der/die Benutzende die Anwendung startet, wählt er/sie die MVPD nur über den Apple-SSO-Workflow aus, ohne den Authentifizierungsfluss abzuschließen. Daher muss die Anwendung auf den regulären Authentifizierungsfluss zurückgreifen, kann jedoch die bereits ausgewählte MVPD verwenden.


* Was passiert, wenn ein Benutzer über eine MVPD verfügt, die nicht von Apple integriert (nicht unterstützt) wird?

  Wenn der Benutzer die Anwendung startet, wählt er die Auswahloption „Andere TV-Anbieter“ über den Apple SSO-Workflow aus. Daher muss die Anwendung auf den regulären Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.


* Was passiert, wenn eine Benutzerin oder ein Benutzer über eine MVPD verfügt, die über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) heruntergestuft ist?

  Wenn die Benutzerin oder der Benutzer die Anwendung startet, wird die Benutzerin oder der Benutzer über den Abbaumechanismus und nicht über den Apple SSO-Workflow authentifiziert. Das Erlebnis sollte für den Anwender nahtlos sein, während die Anwendung über den *N010*-Warncode informiert wird, falls sie den Adobe Pass Authentication AccessEnabler iOS/tvOS SDK verwendet.


* Ändert sich die MVPD-Benutzer-ID zwischen Apple SSO und Nicht-Apple-SSO-Authentifizierungsflüssen?

  Es wird erwartet, dass sich die Benutzer-ID nicht ändert, sie muss jedoch für jeden ausgewählten Anbieter überprüft werden.


* Werden sich die Authentifizierungs-TTLs ändern?

  Die Adobe Pass-Authentifizierung berücksichtigt weiterhin die TTLs, die von den Programmierern für ihre Integration in jede MVPD benötigt werden. Wenn Sie von einer Programmieranwendung zu einer anderen Programmieranwendung über Apple SSO navigieren, verfügt die zweite Anwendung über die TTL der zugehörigen Programmierer x MVPD-Integration (sie gibt nicht die TTL der ersten Anwendung frei, die sich authentifiziert)

|                                      | Adobe Pass-Authentifizierungs-TTL abgelaufen | Adobe Pass-Authentifizierungs-TTL gültig |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **TTL des Apple-Geräte-Tokens abgelaufen** | Benutzer ist NICHT authentifiziert (MVPD-Auswahl sollte angezeigt werden) | Der Benutzer wird authentifiziert und die TTL ist die verbleibende Zeit seines Adobe Pass-Authentifizierungstokens/-profils |
| Die TTL für das Geräte-Token von **Apple ist gültig** | Der Benutzer wird im Hintergrund authentifiziert und erhält ein weiteres Adobe Pass-Authentifizierungstoken/-profil mit der im TVE-Dashboard angegebenen TTL | Der Benutzer wird authentifiziert und die TTL ist die verbleibende Zeit seines Adobe Pass-Authentifizierungstokens/-profils |
