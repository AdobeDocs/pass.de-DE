---
title: Überblick über Apple SSO
description: Überblick über Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1452'
ht-degree: 0%

---

# Überblick über Apple SSO {#apple-sso-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#Introduction}

Apple bietet eine API, mit der sich Personen auf Geräteebene bei ihrem TV-Anbieterkonto anmelden können, sodass eine Authentifizierung nicht für jede App erforderlich ist.

Daher haben sich Apple und Adobe Pass Authentication zusammengeschlossen, um die Plattform Single Sign-On (SSO)-Benutzererfahrung im TV-System für iPhone, iPad und Apple TV-Eigentümer zu erstellen.

Um von der Single Sign-On (SSO)-Benutzererfahrung auf einem Apple-Gerät zu profitieren, muss eine Liste mit Voraussetzungen ausgefüllt werden.

</br>

## Voraussetzungen {#Prerequisites}

Die Voraussetzung kann für eine oder mehrere am TVE-Geschäft beteiligte Entitäten gelten, z. B. Programmierer, MVPDs, Adobe Pass-Authentifizierung oder Apple.

</br>

### Programmierer {#Programmer}

Um das Single Sign-On (SSO)-Benutzererlebnis nutzen zu können, muss ein Programmierer:

1. Verwenden Sie mindestens Xcode Version 8 und iOS/tvOS Version 10.

1. Verwenden Sie die [Single-Sign-On-Berechtigung für Videoabonnenten](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) für ihr Apple-Entwicklerkonto konfiguriert wurde. Wenden Sie sich an Apple, um die Aktivierung zu aktivieren [Framework für Video-Abonnentenkonten](https://developer.apple.com/documentation/videosubscriberaccount) für Ihre Apple-Team-ID.

1. Aktivieren Sie Single Sign-On (YES) für jede gewünschte Integration (Channel x MVPD) und die gewünschte Plattform (iOS/tvOS) über die [Adobe Primetime TVE-Dashboard](https://console.auth.adobe.com/).

1. Integrieren Sie die Apple SSO-Workflows mit einer der beiden folgenden Lösungen, die vom Adobe Pass-Authentifizierungsteam angeboten werden:

   - Die Adobe Pass Authentication REST API kann die SSO-Authentifizierung (Single Sign-On) für Endbenutzer von Clientanwendungen unterstützen, die auf iOS, iPadOS oder tvOS ausgeführt werden. Siehe auch [Apple SSO-Cookbook (REST-API)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - Das Adobe Pass Authentication AccessEnabler iOS/tvOS SDK unterstützt die SSO-Authentifizierung (Single Sign-On) für Endbenutzer von Clientanwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden. Siehe auch [Apple SSO-Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>Pro Tipp:</u>** Um Zugriff auf die Abonnementinformationen des Benutzers zu erhalten, muss der Benutzer der Anwendung die Berechtigung zum Fortfahren erteilen, ähnlich wie beim Gewähren des Zugriffs auf die Kamera oder das Mikrofon des Geräts. Diese Berechtigung muss pro Anwendung angefordert werden. Das Gerät speichert die Auswahl des Benutzers. Beachten Sie, dass der Benutzer seine Entscheidung ändern kann, indem er zu den Anwendungseinstellungen (Zugriffsberechtigung für TV-Anbieter) oder zum Abschnitt über *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS.

   - **<u>Pro Tipp:</u>** Es wird empfohlen, die Berechtigung des Benutzers anzufordern, wenn die Anwendung in den Vordergrund wechselt. Dies ist jedoch nur ein Vorschlag, da die Anwendung nach [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) die Abonnementinformationen des Benutzers zu jedem Zeitpunkt, bevor eine Benutzerauthentifizierung erforderlich ist. Außerdem fordern die AccessEnabler iOS/tvOS SDK-APIs bei Bedarf automatisch die Benutzerberechtigung an.

   - **<u>Pro Tipp:</u>** Es wird empfohlen, Benutzer, die die Erlaubnis zum Zugriff auf Abonnementinformationen verweigern, dazu anzuregen, indem die Vorteile des Single Sign-On (SSO)-Benutzererlebnisses erläutert werden. Beachten Sie, dass der Benutzer seine Entscheidung ändern kann, indem er zu den Anwendungseinstellungen (Zugriffsberechtigung für TV-Anbieter) oder zum Abschnitt über *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS.

Das Ergebnis sollte ein Erlebnis gemäß den folgenden Benutzerflüssen erzeugen, das Sie vor der Entwicklung Ihrer Anwendung/Ihrer Anwendungen konsultieren sollten:

- [iPhone/iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) Benutzerflüsse
- [APPLE TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) Benutzerflüsse


>[!IMPORTANT]
>
> Wenn die Single-Sign-On-Funktion **enabled** für iOS/tvOS **und** im Falle von Apple **integrierte (unterstützte) Auswahl** MVPDs, bei denen die Authentifizierungs-/Abmeldevorgänge von den SSO-Workflows von Apple ausgeführt werden, umfassen sowohl Apple- als auch Adobe Pass-Authentifizierungslösungen, während alle anderen Flüsse (Autorisierung, Vorabautorisierung, Metadaten usw.) wird ausschließlich von Adobe Pass Authentication betreut.


>[!IMPORTANT]
>
> Wenn die Single-Sign-On-Funktion **disabled** für iOS/tvOS **oder** im Falle von Apple **nicht integriert (nicht unterstützt)** MVPDs, bei denen die Authentifizierungs-/Abmelde-Workflows von den Apple SSO-Workflows auf die regulären Workflows fallen, die ausschließlich von der Adobe Pass-Authentifizierung gewartet werden.


>[!IMPORTANT]
>
> Ein Hauptgewinn des Apple SSO-Workflows ist der Benutzerfluss für die einmalige Authentifizierung, der auch auf Apple-TVs bereitgestellt werden kann, wenn die Single-Sign-On-Funktion aktiviert ist **enabled** für tvOS **und** im Falle von Apple **integriert (unterstützt)** MVPDs.


### MVPD {#MVPD}

Um das Single Sign-On (SSO)-Benutzererlebnis nutzen zu können, muss ein MVPD:



1. In den Apple SSO-Workflow auf Apple integriert werden. Wenden Sie sich an Apple, um das Onboarding zu erleichtern.
1. Stellen Sie eine JavaScript-TVML-Anwendung bereit, die das Anmeldeformular des Benutzers verarbeiten kann. Wenden Sie sich an Apple, um eine entsprechende Dokumentation zu erhalten.
1. Geben Sie einen string -Wert an, der die vom Apple während des Onboarding-Prozesses zugewiesene Provider-Kennung darstellt. Wenden Sie sich an die Adobe Pass-Authentifizierung, um Konfigurationsänderungen durchzuführen.

</br>

## FAQs {#FAQ}

1. Falls beim Apple SSO-Workflow etwas schiefgeht, kann die Anwendung mit dem AccessEnabler iOS/tvOS SDK zum regulären Authentifizierungsfluss zurückkehren?
   - Dies ist möglich, erfordert jedoch eine Konfigurationsänderung für die [Adobe Primetime TVE-Dashboard](https://console.auth.adobe.com/). Die *Single Sign-on aktivieren* muss auf *NO* für die gewünschte Integration (Channel x MVPD) und die gewünschte Plattform (iOS/tvOS).
   - Die Anwendung würde die Konfigurationsänderung erst bestätigen, nachdem [setRequest](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) API für den Fall, dass das AccessEnabler iOS/tvOS SDK verwendet wird.
1. Weiß die Anwendung, wann eine Authentifizierung aufgrund einer Anmeldung über die Plattform-SSO auf einem anderen Gerät oder in einer anderen Anwendung erfolgt ist?
   - Diese Informationen sind nicht verfügbar.
1. Weiß die Anwendung, wann eine Authentifizierung aufgrund einer Anmeldung über die Plattform-SSO auf demselben Gerät erfolgte?
   - Diese Informationen sind als Teil des Benutzer-Metadatenschlüssels verfügbar: *tokenSource*, der den Zeichenfolgenwert zurückgeben soll: in diesem Fall &quot;Apple&quot;.
1. Was passiert, wenn sich ein Benutzer anmeldet, indem er zum *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* im Abschnitt tvOS mit einem MVPD, der nicht in die Anwendung integriert ist?
   - Wenn der Benutzer die Anwendung startet, wird der Benutzer nicht über den Apple SSO-Workflow authentifiziert. Daher müsste die Anwendung auf einen normalen Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.
1. Was passiert, wenn sich ein Benutzer anmeldet, indem er zum *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* im Abschnitt tvOS mit einem MVPD, der über die *Single Sign-on aktivieren* auf *NO* auf [Adobe Primetime TVE-Dashboard](https://console.auth.adobe.com/) für die iOS/tvOS-Plattform?
   - Wenn der Benutzer die Anwendung startet, wird der Benutzer nicht über den Apple SSO-Workflow authentifiziert. Daher müsste die Anwendung auf einen normalen Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.
1. Was passiert, wenn ein Benutzer über ein MVPD verfügt, das nicht von Apple integriert (nicht unterstützt) wird, aber im Apple-Picker vorhanden ist?
   - Wenn der Benutzer die Anwendung startet, wählt der Benutzer das MVPD nur über den Apple SSO-Workflow aus, ohne den Authentifizierungsfluss abzuschließen. Daher muss die Anwendung auf den normalen Authentifizierungsfluss zurückgreifen, könnte jedoch den bereits ausgewählten MVPD verwenden.
1. Was passiert, wenn ein Benutzer über einen MVPD verfügt, der nicht von Apple integriert (nicht unterstützt) wird?
   - Wenn der Benutzer die Anwendung startet, wählt er über den Apple SSO-Workflow die Option &quot;Andere TV-Anbieter&quot;aus. Daher müsste die Anwendung auf einen normalen Authentifizierungsfluss zurückgreifen und eine eigene MVPD-Auswahl vorlegen.
1. Was passiert, wenn ein Benutzer über einen MVPD verfügt, der durch das Medium [Adobe Primetime TVE-Dashboard](https://console.auth.adobe.com/)?
   - Wenn der Benutzer die Anwendung startet, wird der Benutzer über den Abbaumechanismus und nicht über den Apple SSO-Workflow authentifiziert.
   - Das Erlebnis sollte für den Benutzer nahtlos sein, während die Anwendung über die *N010* Warncode, falls das AccessEnabler iOS/tvOS SDK verwendet wird.
1. Ändert sich die MVPD-Benutzer-ID zwischen Apple SSO und Nicht-Apple SSO-Authentifizierung?
   - Es wird erwartet, dass sich die Benutzer-ID nicht ändert, aber für jeden ausgewählten Anbieter überprüft werden muss.
1. Werden die Authentifizierungs-TTLs geändert?
   - Adobe Pass Authentication wird weiterhin die TTLs respektieren, die von den Programmierern für ihre Integration in jedes MVPD benötigt werden.
   - Beim Navigieren von einer Programmiereranwendung zu einer anderen Programmeranwendung über Apple SSO weist die zweite Anwendung die TTL der entsprechenden Programmierer x MVPD-Integration auf (sie gibt die TTL der ersten authentifizierten Anwendung nicht weiter)

|                                      | Adobe Pass Authentication TTL abgelaufen | Adobe Pass Authentication TTL valid |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Apple-Geräte-Token-TTL abgelaufen** | Benutzer ist NICHT authentifiziert (MVPD-Auswahl sollte angezeigt werden) | Der Benutzer ist authentifiziert und die TTL ist die verbleibende Zeit seines Adobe Pass-Authentifizierungstokens. |
| **Apple-Geräte-Token TTL gültig** | Der Benutzer ist still authentifiziert und erhält ein weiteres Adobe Pass-Authentifizierungstoken mit der im TVE-Dashboard angegebenen TTL | Der Benutzer ist authentifiziert und die TTL ist die verbleibende Zeit seines Adobe Pass-Authentifizierungstokens. |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
