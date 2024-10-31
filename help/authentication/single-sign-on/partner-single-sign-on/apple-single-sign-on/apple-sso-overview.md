---
title: Überblick über Apple SSO
description: Überblick über Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1256'
ht-degree: 0%

---

# Überblick über Apple SSO {#apple-sso-overview}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Apple bietet Benutzern die Möglichkeit, sich auf Geräteebene bei ihrem TV-Anbieterkonto anzumelden, sodass eine Authentifizierung nicht für jede App erforderlich ist.

Adobe Pass-Authentifizierung arbeitet mit Apple zusammen, um das Single-Sign-On (SSO)-Benutzererlebnis im TV-System für iPhone, iPad und Apple TV-Eigentümer zu erstellen.

Um von der Single Sign-On (SSO)-Benutzererfahrung auf einem Apple-Gerät zu profitieren, müssen im Folgenden eine Liste der Voraussetzungen aufgeführt werden, die erfüllt sein müssen.

Das Endergebnis sollte ein Erlebnis gemäß den folgenden Benutzerflüssen erzeugen. Wir empfehlen Ihnen, sich vor Beginn der Entwicklung Ihrer Anwendung an Sie zu wenden:

* Single Sign-On (SSO) [Benutzerflüsse für iPhone- und iPad](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)-Geräte.
* Single Sign-On (SSO) [Benutzerflüsse für Apple TV](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)-Geräte.

## Voraussetzungen {#apple-sso-prerequisites}

Die Onboarding-Voraussetzungen können für eine oder mehrere am TVE-Geschäft beteiligte Entitäten gelten, z. B. Programmierer, MVPDs, Adobe Pass-Authentifizierung oder Apple.

### Programmierer {#apple-sso-prerequisites-programmer}

Um das Single Sign-On (SSO)-Benutzererlebnis nutzen zu können, muss ein Programmierer:

* Wenden Sie sich an Apple, um das [Video-Abonnentenkonto-Framework](https://developer.apple.com/documentation/videosubscriberaccount) als Teil Ihrer Apple-Team-ID zu aktivieren, und konfigurieren Sie die [Single-Sign-On-Berechtigung für Videoponnenten](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) als Teil Ihres Apple-Entwicklerkontos.

   * Verwenden Sie Xcode Version 8 oder höher und iOS/tvOS Version 10 oder höher.

* Aktivieren Sie Single Sign-On (SSO) für jede gewünschte Integration und Plattform (iOS/tvOS) über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication), indem Sie die Eigenschaft `Enable Single Sign On` auf `Yes` festlegen.

| Adobe zur Aktivierung der einmaligen Anmeldung | Apple **integriert (unterstützt)** MVPDs | Apple **Picker** MVPDs | Apple **Nicht integriert (nicht unterstützt)** MVPDs |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Ja (aktiviert) | Die Authentifizierungs- und Abmeldevorgänge umfassen sowohl Apple- als auch Adobe Pass-Authentifizierungslösungen, während alle anderen Flüsse (Autorisierung, Vorabautorisierung, Metadaten usw.) wird ausschließlich von Adobe Pass Authentication betreut. | Die Authentifizierungs- und Abmeldevorgänge werden auf die regulären Flüsse zurückgreifen, die ausschließlich von der Adobe Pass-Authentifizierung bereitgestellt werden. | Die Authentifizierungs- und Abmeldevorgänge werden auf die regulären Flüsse zurückgreifen, die ausschließlich von der Adobe Pass-Authentifizierung bereitgestellt werden. |
| Nein (deaktiviert) | Die Authentifizierungs- und Abmeldevorgänge werden auf die regulären Flüsse zurückgreifen, die ausschließlich von der Adobe Pass-Authentifizierung bereitgestellt werden. | Die Authentifizierungs- und Abmeldevorgänge werden auf die regulären Flüsse zurückgreifen, die ausschließlich von der Adobe Pass-Authentifizierung bereitgestellt werden. | Die Authentifizierungs- und Abmeldevorgänge werden auf die regulären Flüsse zurückgreifen, die ausschließlich von der Adobe Pass-Authentifizierung bereitgestellt werden. |

* Integrieren Sie die SSO-Benutzerflüsse (Single Sign-On) mithilfe einer der folgenden Lösungen, die von der Adobe Pass-Authentifizierung für Endbenutzer von Clientanwendungen angeboten werden, die auf iOS, iPadOS oder tvOS ausgeführt werden.

   * Die Adobe Pass Authentication REST API V2 unterstützt Single Sign-On (SSO) für Partner.

     Weitere Informationen finden Sie in der Dokumentation zum [Apple SSO Cookbook (REST API V2)](./apple-sso-cookbook-rest-api-v2.md) .

   * Die Adobe Pass Authentication REST API V1 unterstützt Single Sign-On (SSO) für Partner.

     Weitere Informationen finden Sie in der Dokumentation zum [Apple SSO Cookbook (REST API V1)](./apple-sso-cookbook-rest-api-v1.md) .

   * Das Adobe Pass Authentication AccessEnabler iOS/tvOS SDK unterstützt Single Sign-On (SSO) für Partner.

     Weitere Informationen finden Sie in der Dokumentation zum [Apple SSO Cookbook (iOS/tvOS SDK)](./apple-sso-cookbook-iostvos-sdk.md) .

### MVPD {#apple-sso-prerequisites-mvpd}

Um das Single Sign-On (SSO)-Benutzererlebnis nutzen zu können, muss ein MVPD:

* Wenden Sie sich an Apple, um den Onboarding-Prozess auf Apple-Seite zu starten.

   * Fordern Sie die technische Dokumentation zur Integration und Entwicklung einer JavaScript TVML-Anwendung an, die das Anmeldeformular für den Benutzer handhaben kann.

* Wenden Sie sich an die Adobe Pass-Authentifizierung , um den Onboarding-Prozess auf der Adobe zu starten.

   * Geben Sie den Zeichenfolgenwert an, der die vom Apple während des Onboarding-Prozesses zugewiesene TV-Anbieterkennung darstellt.

## FAQs {#FAQ}

* Falls etwas mit dem Apple SSO-Workflow fehlschlägt, kann die Anwendung mit dem Adobe Pass Authentication AccessEnabler iOS/tvOS SDK die Möglichkeit haben, zum regulären Authentifizierungsfluss zurückzukehren?

  Dies ist möglich, erfordert jedoch eine Konfigurationsänderung, die über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) vorgenommen wird, um das **Single Sign-On aktivieren** auf **NO** für die gewünschte Integration und Plattform (iOS/tvOS) festzulegen. Beachten Sie, dass die Clientanwendung die Konfigurationsänderung erst bestätigt, nachdem die API [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) aufgerufen wurde.


* Wird die Anwendung wissen, wann eine Authentifizierung aufgrund eines Anmeldevorgangs über die Apple SSO erfolgt ist?

  Diese Informationen sind als Teil des Benutzer-Metadatenschlüssels verfügbar: *tokenSource*, der den Zeichenfolgenwert zurückgeben sollte: &quot;Apple&quot;.


* Wird die Anwendung wissen, wann eine Authentifizierung aufgrund eines Anmeldevorgangs über Apple SSO in einer anderen Anwendung erfolgte?

  Diese Informationen sind nicht verfügbar.


* Was passiert, wenn sich ein Benutzer anmeldet, indem er den Abschnitt &quot;*`Settings -> TV Provider`*&quot;unter iOS/iPadOS oder &quot;*`Settings -> Accounts -> TV Provider`*&quot;unter tvOS mit einem MVPD aufruft, der nicht in die Anwendung integriert ist?

  Wenn der Benutzer die Anwendung startet, wird der Benutzer nicht über den Apple SSO-Workflow authentifiziert. Daher müsste die Anwendung auf den normalen Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.


* Was passiert, wenn sich ein Benutzer mit einem MVPD anmeldet, bei dem für die iOS/tvOS-Plattform das Feld *`Settings -> TV Provider`* unter iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* unter tvOS mit einem MVPD verwendet wird, für das das Feld **Single Sign On aktivieren** auf **NO** über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) eingestellt ist?

  Wenn der Benutzer die Anwendung startet, wird der Benutzer nicht über den Apple SSO-Workflow authentifiziert. Daher müsste die Anwendung auf den normalen Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.


* Was passiert, wenn ein Benutzer über ein MVPD verfügt, das nicht von Apple integriert (nicht unterstützt) wird, aber im Apple-Picker vorhanden ist?

  Wenn der Benutzer die Anwendung startet, wählt der Benutzer das MVPD nur über den Apple SSO-Workflow aus, ohne den Authentifizierungsfluss abzuschließen. Daher muss die Anwendung auf den normalen Authentifizierungsfluss zurückgreifen, könnte jedoch den bereits ausgewählten MVPD verwenden.


* Was passiert, wenn ein Benutzer über einen MVPD verfügt, der nicht von Apple integriert (nicht unterstützt) wird?

  Wenn der Benutzer die Anwendung startet, wählt er über den Apple SSO-Workflow die Option &quot;Andere TV-Anbieter&quot;aus. Daher müsste die Anwendung auf den normalen Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.


* Was passiert, wenn ein Benutzer über einen MVPD verfügt, der über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) beschädigt wird?

  Wenn der Benutzer die Anwendung startet, wird der Benutzer über den Abbaumechanismus und nicht über den Apple SSO-Workflow authentifiziert. Das Erlebnis sollte für den Benutzer nahtlos sein, während die Anwendung über den Warncode *N010* informiert wird, falls sie das Adobe Pass Authentication AccessEnabler iOS/tvOS SDK verwendet.


* Ändert sich die MVPD-Benutzer-ID zwischen Apple SSO- und Nicht-Apple SSO-Authentifizierungsflüssen?

  Es wird erwartet, dass sich die Benutzer-ID nicht ändert, aber für jeden ausgewählten Anbieter überprüft werden muss.


* Werden die Authentifizierungs-TTLs geändert?

  Adobe Pass Authentication wird weiterhin die TTLs respektieren, die von den Programmierern für ihre Integration in jedes MVPD benötigt werden. Beim Navigieren von einer Programmiereranwendung zu einer anderen Programmeranwendung über Apple SSO weist die zweite Anwendung die TTL der entsprechenden Programmierer x MVPD-Integration auf (sie gibt die TTL der ersten authentifizierten Anwendung nicht weiter)

|                                      | Adobe Pass Authentication TTL abgelaufen | Adobe Pass Authentication TTL valid |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Apple-Geräte-Token-TTL abgelaufen** | Benutzer ist NICHT authentifiziert (MVPD-Auswahl sollte angezeigt werden) | Der Benutzer ist authentifiziert und die TTL ist die verbleibende Zeit seines Adobe Pass-Authentifizierungstokens/-Profils. |
| **Apples Geräte-Token TTL valid** | Der Benutzer ist still authentifiziert und erhält ein weiteres Adobe Pass-Authentifizierungstoken/-profil mit der im TVE-Dashboard angegebenen TTL | Der Benutzer ist authentifiziert und die TTL ist die verbleibende Zeit seines Adobe Pass-Authentifizierungstokens/-Profils. |
