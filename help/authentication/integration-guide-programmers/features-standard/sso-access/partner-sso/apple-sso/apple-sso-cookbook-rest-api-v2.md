---
title: Apple SSO-Cookbook (REST API v2)
description: Apple SSO-Cookbook (REST API v2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '3609'
ht-degree: 0%

---

# Apple SSO-Cookbook (REST API v2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Die Adobe Pass-Authentifizierungs-REST-API V2 unterstützt das Partner Single Sign-On (SSO) für Endbenutzer von Client-Anwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden.

Dieses Dokument dient als Erweiterung für die vorhandene [REST API V2 - Übersicht](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md), die eine allgemeine Ansicht bietet, sowie für das Dokument, in dem die Implementierung von [Single Sign-on mithilfe von Partnerflüssen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) beschrieben wird.

## Apple-Single-Sign-on mit Partnerflüssen {#cookbook}

### Voraussetzungen {#prerequisites}

Bevor Sie mit der einmaligen Anmeldung für Apple mithilfe von Partnerflüssen fortfahren, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss alle erforderlichen Daten für die `X-Device-Info`- und/oder `User-Agent`-Header erfassen, damit das Backend für die Adobe Pass-Authentifizierung die Geräteplattform und ihre Funktionen identifizieren kann. Weitere Informationen zu `X-Device-Info` Kopfzeile finden Sie in der Dokumentation [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md).

* Die Streaming-Anwendung muss Zugriff auf die auf Geräteebene gespeicherten Abonnementinformationen des Benutzers anfordern, für die der Benutzer der Anwendung die Berechtigung zum Fortfahren erteilen muss, ähnlich wie das Gewähren von Zugriff auf die Kamera oder das Mikrofon des Geräts. Diese Berechtigung muss pro Anwendung über das Apple-[Video-Abonnementkonto-Framework](https://developer.apple.com/documentation/videosubscriberaccount) angefordert werden und das Gerät speichert die Benutzerauswahl.

  Wir empfehlen, Benutzern, die sich weigern, Berechtigungen für den Zugriff auf Abonnementinformationen zu erteilen, einen Anreiz zu bieten, indem sie die Vorteile des Apple Single Sign-on-Benutzererlebnisses erläutern. Sie sollten jedoch beachten, dass die Benutzenden ihre Entscheidung ändern können, indem sie zu den Anwendungseinstellungen gehen (Zugriff auf TV-Anbieter) oder auf iOS und iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS *`Settings -> TV Provider`*.

  Die Streaming-Anwendung kann die Berechtigung des Benutzers anfordern, wenn die Anwendung in den Vordergrundzustand wechselt, da die Anwendung die Abonnementinformationen des Benutzers jederzeit auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen kann, bevor eine Benutzerauthentifizierung erforderlich wird.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
>
> * Die Streaming-Anwendung hat die [Onboarding-Voraussetzungen](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer) erfüllt, die für Programmierer gelten und für die Aktivierung des Apple Single Sign-on-Benutzererlebnisses erforderlich sind.

### Workflow {#workflow}

Führen Sie die angegebenen Schritte aus, um das Apple-Single Sign-on mithilfe von Partnerflüssen zu implementieren, wie im folgenden Diagramm dargestellt.

![Apple-Single-Sign-on mit Partnerflüssen](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*Apple-Single-Sign-on mit Partnerflüssen*

+++a. Registrierungsphase

1. **Abrufen von Client-Anmeldeinformationen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um die Client-Anmeldeinformationen abzurufen, indem sie den Endpunkt „Client Register“ aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request)Dokumentation zum Abrufen von Client-Anmeldeinformationen:
   >
   > * Alle _erforderlichen_ Parameter wie `software_statement`
   > * Alle _erforderlichen_ Kopfzeilen wie `Content-Type`, `X-Device-Info`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Client-Anmeldeinformationen zurückgeben:** Die Antwort des Client Register-Endpunkts enthält Informationen zu den Client-Anmeldeinformationen, die mit den empfangenen Parametern und Headern verknüpft sind.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Antwort zum Abrufen von Client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success)Anmeldeinformationen finden Sie in der API-Dokumentation.
   >
   > <br/>
   >
   > Das Client-Register validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der API-Dokumentation [Client-Anmeldeinformationen abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) bereitstellt.

   >[!TIP]
   >
   > Die Client-Anmeldeinformationen müssen zwischengespeichert und unbegrenzt verwendet werden.

1. **Zugriffs-Token abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um das Zugriffs-Token abzurufen, indem sie den Client-Token-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request)Dokumentation zum Abrufen von Zugriffstoken:
   >
   > * Alle _erforderlichen_ Parameter wie `client_id`, `client_secret` und `grant_type`
   > * Alle _erforderlichen_ Kopfzeilen wie `Content-Type`, `X-Device-Info`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Zugriffs-Token zurückgeben:** Die Antwort des Client-Token-Endpunkts enthält Informationen zum Zugriffs-Token, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Zugriffstoken-Antwort bereitgestellten Informationen finden ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) in der API-Dokumentation zum Abrufen von Zugriffstoken .
   >
   > <br/>
   >
   > Das Client-Token validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der API-Dokumentation [Zugriffstoken abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) bereitstellt.

   >[!TIP]
   >
   > Das Zugriffstoken muss im Cache gespeichert und nur innerhalb der angegebenen Dauer verwendet werden (z. B. 24-Stunden-Time-to-Live). Nach Ablauf muss die Streaming-Anwendung ein neues Zugriffstoken anfordern.

+++

+++B. Authentifizierungsphase überprüfen

1. Apple **Partner-Framework-Status abrufen:** Die Streaming-Anwendung ruft das von [ entwickelte Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) auf, um Benutzerberechtigungen und Anbieterinformationen abzurufen.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der Dokumentation ](https://developer.apple.com/documentation/videosubscriberaccount)Video Subscriber Account Framework):
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die Abonnementinformationen des Benutzers auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen und nur fortfahren, wenn der Benutzer dies erlaubt hat.
   > * Die Streaming-Anwendung muss einen [Delegaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für die `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen booleschen Wert angibt, der `false` für die [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)-Eigenschaft im `VSAccountMetadataRequest`-Objekt entspricht, um anzugeben, dass der Benutzer in dieser Phase nicht unterbrochen werden kann.

1. **Partner-Framework-Statusinformationen zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Benutzerberechtigungs-Zugriffsstatus wird gewährt.
   * Die Kennung der Benutzeranbieter-Zuordnung ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (falls verfügbar) ist gültig.

1. **Profile abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um alle Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt „Profiles“ gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request)Dokumentation zum Abrufen von Profilen:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, sodass die abgerufene Antwort ein Profil vom Typ „appleSSO“ enthalten kann.
   >
   > <br/>
   >
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Rückgabe von Informationen zu gefundenen Profilen:** Die Antwort des Endpunkts „Profile“ enthält Informationen zu den gefundenen Profilen, die mit den empfangenen Parametern und Kopfzeilen verknüpft sind.

1. **Wählen Sie ein Profil aus und fahren Sie mit Entscheidungsflüssen fort:** Wenn die Antwort des Endpunkts „Profile“ Profile enthält, verwendet die Streaming-Anwendung ihre interne Logik (letztendlich durch Interaktion mit dem Endbenutzer), um eines der verfügbaren Profile auszuwählen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.

1. **Fahren Sie mit dem Partnerauthentifizierungsfluss fort:** Wenn die Endpunktantwort „Profile“ kein Profil enthält, fährt die Streaming-Anwendung mit dem Partnerauthentifizierungsfluss fort.

+++

+++C. Authentifizierungsphase für Partner

1. **Konfiguration abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um die Liste der MVPDs mit einer aktiven Integration abzurufen, indem eine Anfrage an den Konfigurationsendpunkt gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request)Dokumentation zum Abrufen der Konfiguration für einen bestimmten Dienstleister:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier` und `X-Device-Info`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Rückgabekonfiguration:** Die Antwort des Konfigurationsendpunkts enthält Informationen über die MVPDs, die eine aktive Integration mit dem Dienstleister aufweisen.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Konfigurationsantwort bereitgestellten Informationen finden Sie in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response)Dokumentation zum Abrufen der Konfiguration für einen bestimmten Dienstleister.
   >
   > <br/>
   >
   > Der Konfigurationsendpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie die folgenden Details für jede MVPD verarbeitet, wenn sie fortfährt:
   >
   > * `enablePlatformServices`: Gibt an, ob die MVPD derzeit das Apple-Single Sign-on unterstützt.
   > * `displayInPlatformPicker`: Gibt an, ob die MVPD in der Apple-Auswahl angezeigt werden kann.
   > * `boardingStatus`: Gibt an, ob MVPD in Apple Single Sign-on integriert ist.

1. Apple **Partner-Framework-Status abrufen:** Die Streaming-Anwendung ruft das von [ entwickelte Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) auf, um Benutzerberechtigungen und Anbieterinformationen abzurufen.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der Dokumentation ](https://developer.apple.com/documentation/videosubscriberaccount)Video Subscriber Account Framework):
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die Abonnementinformationen des Benutzers auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen und nur fortfahren, wenn der Benutzer dies erlaubt hat.
   > * Die Streaming-Anwendung muss einen [Delegaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für die `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen booleschen Wert angibt, der `true` für die [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)-Eigenschaft im `VSAccountMetadataRequest`-Objekt entspricht, um anzugeben, dass der Benutzer in dieser Phase unterbrochen werden kann, um einen TV-Anbieter auszuwählen.

1. **Partner-Framework-Statusinformationen zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Benutzerberechtigungs-Zugriffsstatus wird gewährt.
   * Die Kennung der Benutzeranbieter-Zuordnung ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (falls verfügbar) ist gültig.

1. **Partnerauthentifizierungsanfrage abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um eine Authentifizierungssitzung zu initiieren, indem sie den Sitzungspartner-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request)Dokumentation zum Abrufen von Partnerauthentifizierungsanfragen:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider` und `partner`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ Kopfzeilen und Parameter
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, sodass die abgerufene Antwort eine Partner-Authentifizierungsanfrage (SAML-Anfrage) enthalten kann.
   >
   > <br/>
   >
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Nächste Aktion angeben:** Die Antwort des Sitzungspartner-Endpunkts enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu leiten.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response)Dokumentation zum Abrufen der Partnerauthentifizierungsanfrage .
   >
   > <br/>
   >
   > Der Sitzungspartner-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die einfache Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.
   >
   > <br/>
   >
   > Der Sitzungspartner-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die Bedingungen für das einmalige Anmelden des Partners erfüllt sind:
   >
   >  * Die Partnerkonfiguration für einmaliges Anmelden im Adobe Pass-Server muss gültig und aktiviert sein.
   >  * Die Payload des Partner-Frameworks, die über die Kopfzeile [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) empfangen wurde, muss gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung des Partner-Single-Sign-ons fehlschlägt, wird die Antwort standardmäßig auf den einfachen Authentifizierungsfluss eingestellt.

1. **Fahren Sie mit Entscheidungsflüssen fort:** Die Antwort „Sitzungspartner-Endpunkt“ enthält die folgenden Daten:
   * Das `actionName`-Attribut ist auf „authorize“ festgelegt.
   * Das `actionType`-Attribut ist auf „direct“ festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

1. **Mit einfachem Authentifizierungsfluss fortfahren:** Die Antwort des Sitzungspartner-Endpunkts enthält die folgenden Daten:
   * Das `actionName`-Attribut wird entweder auf „Authentifizieren“ oder auf „Fortsetzen“ festgelegt.
   * Das `actionType`-Attribut wird entweder auf „interaktiv“ oder auf „direkt“ festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert und die Partnerüberprüfung für einmaliges Anmelden fehlschlägt, kehrt der Adobe Pass-Server zum einfachen Authentifizierungsfluss zurück.

   Weitere Informationen zum grundlegenden Authentifizierungsablauf finden Sie in den folgenden Dokumenten:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Fahren Sie mit dem Erstellen und Abrufen eines Profils mithilfe des Antwortflusses zur Partnerauthentifizierung fort:** Die Antwort „Sitzungspartner-Endpunkt“ enthält die folgenden Daten:
   * Das `actionName`-Attribut ist auf „partner_profile“ festgelegt.
   * Das `actionType`-Attribut ist auf „direct“ festgelegt.
   * Das `authenticationRequest - type`-Attribut enthält das Sicherheitsprotokoll, das vom Partner-Framework für die Anmeldung bei MVPD verwendet wird (derzeit nur auf SAML festgelegt).
   * Das `authenticationRequest - request`-Attribut enthält die SAML-Anfrage, die an das Partner-Framework übergeben wird.
   * Das `authenticationRequest - attributesNames`-Attribut enthält die SAML-Attribute, die an das Partner-Framework übergeben werden.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert und die Partnervalidierung mit Single Sign-on erfolgreich ist, erhält die Streaming-Anwendung eine Antwort mit Aktionen und Daten, die an das Partner-Framework übergeben werden, um den Authentifizierungsfluss mit der MVPD zu starten.

1. **Abschließen der MVPD-Authentifizierung mit dem Partner-Framework** Leitet die im vorherigen Schritt erhaltene Partnerauthentifizierungsanfrage (SAML-Anfrage) an das [Framework für Videoabonnentenkonten](https://developer.apple.com/documentation/videosubscriberaccount) weiter. Wenn der Authentifizierungsfluss erfolgreich ist, führt die [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)-Interaktion mit der MVPD zu einer Partnerauthentifizierungsantwort (SAML-Antwort), die zusammen mit den Statusinformationen des Partner-Frameworks zurückgegeben wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der Dokumentation ](https://developer.apple.com/documentation/videosubscriberaccount)Video Subscriber Account Framework):
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die Abonnementinformationen des Benutzers auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen und nur fortfahren, wenn der Benutzer dies erlaubt hat.
   > * Die Streaming-Anwendung muss einen [Delegaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für die `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anforderung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Informationen zum Abonnentenkonto senden und die im vorherigen Schritt erhaltene Partnerauthentifizierungsanfrage (SAML-Anfrage) enthalten.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen booleschen Wert `true` für die [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) Eigenschaft im `VSAccountMetadataRequest` angibt, um anzugeben, dass die Benutzerin bzw. der Benutzer unterbrochen werden kann, um sich in dieser Phase beim ausgewählten TV-Anbieter zu authentifizieren.

1. **Antwort zur Partnerauthentifizierung zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Benutzerberechtigungs-Zugriffsstatus wird gewährt.
   * Die Kennung der Benutzeranbieter-Zuordnung ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (falls verfügbar) ist gültig.
   * Die Partnerauthentifizierungsantwort (SAML-Antwort) ist vorhanden und gültig.

1. **Profil mit Partnerauthentifizierungsantwort erstellen und abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um ein Profil zu erstellen und abzurufen, indem sie den Partner-Endpunkt „Profile“ aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der API-Dokumentation ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) Erstellen und Abrufen von Profilen mit Partnerauthentifizierungsantwort:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `partner` und `SAMLResponse`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ Kopfzeilen und Parameter
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie gültige Werte für den Partner-Framework-Status und die Partner-Authentifizierungsantwort (SAML-Antwort) enthält, sodass die abgerufene Antwort ein Profil vom Typ „appleSSO“ enthalten kann.
   >
   > <br/>
   >
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Rückgabeinformationen zum Partnerprofil:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum Partnerprofil, einschließlich des Attributs, das auf „appleSSO“ festgelegt `type`.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu [ in einer Profilantwort enthaltenen Informationen finden Sie in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response)Dokumentation Erstellen und Abrufen von Profilen mit Partnerauthentifizierungsantwort .
   >
   > <br/>
   >
   > Der Profilpartner-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.
   >
   > <br/>
   >
   > Der Partner-Endpunkt „Profile“ validiert die Anfragedaten, um sicherzustellen, dass die Bedingungen für das einmalige Anmelden des Partners erfüllt sind:
   >
   >  * Die Partnerkonfiguration für einmaliges Anmelden im Adobe Pass-Server muss gültig und aktiviert sein.
   >  * Die Payload des Partner-Frameworks, die über die Kopfzeile [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) empfangen wurde, muss gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung des Partner-Single-Sign-ons fehlschlägt, wird als Antwort standardmäßig der grundlegende Profilabruffluss verwendet.

1. **Fahren Sie mit Entscheidungsflüssen fort:** Die Streaming-Anwendung kann mit nachfolgenden Entscheidungsflüssen fortfahren.

+++

+++ D. Entscheidungsphase

1. Apple **Partner-Framework-Status abrufen:** Die Streaming-Anwendung ruft das von [ entwickelte Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) auf, um Benutzerberechtigungen und Anbieterinformationen abzurufen.

   >[!IMPORTANT]
   > 
   > Die Streaming-Anwendung kann diesen Schritt überspringen, wenn der ausgewählte Benutzerprofiltyp nicht „appleSSO“ lautet.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der Dokumentation ](https://developer.apple.com/documentation/videosubscriberaccount)Video Subscriber Account Framework):
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die Abonnementinformationen des Benutzers auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen und nur fortfahren, wenn der Benutzer dies erlaubt hat.
   > * Die Streaming-Anwendung muss einen [Delegaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für die `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen booleschen Wert angibt, der `false` für die [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)-Eigenschaft im `VSAccountMetadataRequest`-Objekt entspricht, um anzugeben, dass der Benutzer in dieser Phase nicht unterbrochen werden kann.

   >[!TIP]
   >
   > Die Streaming-Anwendung kann stattdessen einen zwischengespeicherten Wert für die Partner-Framework-Statusinformationen verwenden, den wir aktualisieren sollten, wenn die Anwendung vom Hintergrund- in den Vordergrundstatus wechselt. In diesem Fall muss die Streaming-Anwendung sicherstellen, dass sie zwischenspeichert und nur gültige Werte für den Partner-Framework-Status verwendet, wie im Schritt „Informationen zum Partner-Framework-Status zurückgeben“ beschrieben.

1. **Partner-Framework-Statusinformationen zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Benutzerberechtigungs-Zugriffsstatus wird gewährt.
   * Die Kennung der Benutzeranbieter-Zuordnung ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils ist gültig.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung kann diesen Schritt überspringen, wenn der ausgewählte Benutzerprofiltyp nicht „appleSSO“ lautet.

1. **Vorabautorisierungsentscheidungen abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um Vorabautorisierungsentscheidungen für eine Liste von Ressourcen zu erhalten, indem sie den Endpunkt Decisions Preauthorize aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request)Dokumentation zum Abrufen von Vorabautorisierungsentscheidungen mithilfe bestimmter MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization` und `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, bevor sie eine weitere Anfrage stellt, wenn das ausgewählte Profil ein Profil vom Typ „appleSSO“ ist. Dieser Schritt kann jedoch übersprungen werden, wenn der ausgewählte Benutzerprofiltyp nicht „appleSSO“ ist.
   >
   > <br/>
   >
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Entscheidungen zur Vorautorisierung zurückgeben:** Die Antwort des Endpunkts „Decisions Preauthorize“ enthält für jede Ressource eine `Permit` oder `Deny` Entscheidung:
   * Eine `Permit` Entscheidung bedeutet, dass die Ressource abspielbar ist. Die Antwort enthält kein Medien-Token, da der Vorautorisierungsfluss nicht zum Wiedergeben von Ressourcen verwendet werden darf.
   * Eine `Deny` Entscheidung bedeutet, dass die Ressource nicht abspielbar ist. Die Antwort enthält eine Fehler-Payload, die der Dokumentation [Erweiterte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response)Dokumentation zum Abrufen von Entscheidungen vor der Autorisierung mit bestimmten mvpd.
   >
   > <br/>
   >
   > Der Endpunkt Decisions Preauthorize validiert die Anfragedaten, um sicherzustellen, dass grundlegende Bedingungen erfüllt werden:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. Apple **Partner-Framework-Status abrufen:** Die Streaming-Anwendung ruft das von [ entwickelte Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) auf, um Benutzerberechtigungen und Anbieterinformationen abzurufen.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung kann diesen Schritt überspringen, wenn der ausgewählte Benutzerprofiltyp nicht „appleSSO“ lautet.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der Dokumentation ](https://developer.apple.com/documentation/videosubscriberaccount)Video Subscriber Account Framework):
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die Abonnementinformationen des Benutzers auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen und nur fortfahren, wenn der Benutzer dies erlaubt hat.
   > * Die Streaming-Anwendung muss einen [Delegaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für die `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen booleschen Wert angibt, der `false` für die [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)-Eigenschaft im `VSAccountMetadataRequest`-Objekt entspricht, um anzugeben, dass der Benutzer in dieser Phase nicht unterbrochen werden kann.

   >[!TIP]
   >
   > Die Streaming-Anwendung kann stattdessen einen zwischengespeicherten Wert für die Partner-Framework-Statusinformationen verwenden, den wir aktualisieren sollten, wenn die Anwendung vom Hintergrund- in den Vordergrundstatus wechselt. In diesem Fall muss die Streaming-Anwendung sicherstellen, dass sie zwischenspeichert und nur gültige Werte für den Partner-Framework-Status verwendet, wie im Schritt „Informationen zum Partner-Framework-Status zurückgeben“ beschrieben.

1. **Partner-Framework-Statusinformationen zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Benutzerberechtigungs-Zugriffsstatus wird gewährt.
   * Die Kennung der Benutzeranbieter-Zuordnung ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils ist gültig.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung kann diesen Schritt überspringen, wenn der ausgewählte Benutzerprofiltyp nicht „appleSSO“ lautet.

1. **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Decisions Authorize-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request)Dokumentation zum Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization` und `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, bevor sie eine weitere Anfrage stellt, wenn das ausgewählte Profil ein Profil vom Typ „appleSSO“ ist. Dieser Schritt kann jedoch übersprungen werden, wenn der ausgewählte Benutzerprofiltyp nicht „appleSSO“ ist.
   >
   > <br/>
   >
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Rückkehrautorisierungsentscheidung:** Die Endpunktantwort „Decisions Authorize“ enthält eine `Permit` oder `Deny` Entscheidung für die spezifische Ressource:
   * Eine `Permit` Entscheidung bedeutet, dass die Ressource abspielbar ist. Die Antwort enthält ein Medien-Token.
   * Eine `Deny` Entscheidung bedeutet, dass die Ressource nicht abspielbar ist. Die Antwort enthält eine Fehler-Payload, die der Dokumentation [Erweiterte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response)Dokumentation zum Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd.
   >
   > <br/>
   >
   > Der Decisions-Autorisierungs-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass grundlegende Bedingungen erfüllt werden:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

+++

+++ D. Abmeldephase

1. **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um den Abmeldefluss zu initiieren, indem sie den Adobe Pass-Abmelde-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden Sie in [ API-Dokumentation ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) Initiieren des Abmeldens für bestimmte MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Nächste Aktion angeben:** Die Antwort des Abmeldeendpunkts von Adobe Pass enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu führen:
   * Das `url` fehlt, da der Benutzer mit der Partner-(System-)Ebene interagieren muss, um den Abmeldefluss abzuschließen.
   * Das `actionName`-Attribut ist auf „partner_logout“ festgelegt.
   * Das `actionType`-Attribut ist auf „partner_interactive“ festgelegt.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss den Benutzer auffordern, den Abmeldevorgang auf Partnerebene abzuschließen, wie in den Attributen `actionName` und `actionType` angegeben, wenn der entfernte Benutzerprofiltyp „appleSSO“ ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Abmeldeantwort enthaltenen Informationen finden Sie in der API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response)Dokumentation zum Initiieren des Abmeldens für bestimmte mvpd.
   >
   > <br/>
   >
   > Der Adobe Pass-Abmeldeendpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

+++
