---
title: Apple SSO-Cookbook (REST API V2)
description: Apple SSO-Cookbook (REST API V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '3442'
ht-degree: 0%

---

# Apple SSO-Cookbook (REST API V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Die Adobe Pass Authentication REST API V2 unterstützt Single Sign-On (SSO) für Endbenutzer von Clientanwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden.

Dieses Dokument dient als Erweiterung der vorhandenen [REST API V2 Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) -Ansicht auf hoher Ebene und des Dokuments, in dem die Implementierung von [Single Sign-on mithilfe von Partnerflüssen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) beschrieben wird.

## Apple-Single Sign-On mit Partnerflüssen {#cookbook}

### Voraussetzungen {#prerequisites}

Bevor Sie mit der Apple-Single-Sign-On mit Partnerflüssen fortfahren, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss alle erforderlichen Daten erfassen, die für die Header `X-Device-Info` und/oder `User-Agent` erforderlich sind, damit das Backend für die Adobe Pass-Authentifizierung die Geräteplattform und deren Funktionen identifizieren kann. Weitere Informationen zum Header `X-Device-Info` finden Sie in der Dokumentation [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) .

* Die Streaming-Anwendung muss Zugriff auf die auf Geräteebene gespeicherten Abonnementinformationen des Benutzers anfordern, für die der Benutzer der Anwendung die Berechtigung zum Fortfahren erteilen muss, ähnlich wie bei der Bereitstellung des Zugriffs auf die Kamera oder das Mikrofon des Geräts. Diese Berechtigung muss pro Anwendung mit dem [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) von Apple angefordert werden, und das Gerät speichert die Benutzerauswahl.

  Es wird empfohlen, Benutzer, die die Berechtigung zum Zugriff auf Abonnementinformationen verweigern, dazu anzuregen, die Vorteile des Single-Sign-On-Benutzererlebnisses für Apple zu erläutern. Beachten Sie jedoch, dass der Benutzer seine Entscheidung ändern kann, indem er zu den Anwendungseinstellungen (Zugriffsberechtigung für TV-Anbieter) oder zu *`Settings -> TV Provider`* unter iOS und iPadOS oder *`Settings -> Accounts -> TV Provider`* unter tvOS navigiert.

  Die Streaming-Anwendung kann die Berechtigung des Benutzers anfordern, wenn die Anwendung in den Vordergrund wechselt, da die Anwendung jederzeit nach [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers suchen kann, bevor eine Benutzerauthentifizierung erforderlich ist.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
>
> * Die Streaming-Anwendung hat die [Onboarding-Voraussetzungen](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer) erfüllt, die für einen Programmierer gelten und zur Aktivierung des Single Sign-On-Benutzererlebnisses für Apple erforderlich sind.

### Workflow {#workflow}

Führen Sie die angegebenen Schritte aus, um das Apple-Single-Sign-On mithilfe von Partnerflüssen zu implementieren, wie im folgenden Diagramm dargestellt.

![Apple-Single-Sign-On mit Partnerflüssen](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*Apple-Single-Sign-On mit Partnerflüssen*

+++A. Registrierungsphase

1. **Abrufen von Client-Anmeldeinformationen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Client-Anmeldeinformationen abzurufen, indem der Client Register-Endpunkt aufgerufen wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) .
   >
   > * Alle _erforderlichen_ Parameter, z. B. `software_statement`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Content-Type`, `X-Device-Info`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Zurückgeben von Client-Anmeldeinformationen:** Die Antwort des Client Register-Endpunkts enthält Informationen zu den Client-Anmeldeinformationen, die mit den empfangenen Parametern und Kopfzeilen verknüpft sind.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in der Antwort auf Client-Anmeldedaten finden Sie in der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) .
   >
   > <br/>
   >
   > Das Kundenregister validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert und zusätzliche Informationen bereitgestellt, die der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) entsprechen.

   >[!TIP]
   >
   > Empfehlung: Die Client-Anmeldeinformationen müssen zwischengespeichert werden und können unbegrenzt verwendet werden.

1. **Zugriffs-Token abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um das Zugriffstoken abzurufen, indem der Client-Token-Endpunkt aufgerufen wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation [Zugriffstoken abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) .
   >
   > * Alle _erforderlichen_ Parameter, wie `client_id`, `client_secret` und `grant_type`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Content-Type`, `X-Device-Info`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Zugriffstoken zurückgeben:** Die Client-Token-Endpunktantwort enthält Informationen zum Zugriffstoken, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen, die in einer Zugriffstoken-Antwort bereitgestellt werden, finden Sie in der API-Dokumentation zum [Abrufen des Zugriffstokens](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) .
   >
   > <br/>
   >
   > Das Client-Token validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert und zusätzliche Informationen bereitgestellt, die der API-Dokumentation [Zugriffstoken abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) entsprechen.

   >[!TIP]
   >
   > Empfehlung: Das Zugriffstoken darf nur innerhalb der angegebenen Dauer zwischengespeichert und verwendet werden (z. B. 24 Stunden Live-Zeit). Nach dem Ablauf muss die Streaming-Anwendung ein neues Zugriffstoken anfordern.

+++

+++B. Authentifizierungsphase überprüfen

1. **Abrufen des Partner-Framework-Status:** Die Streaming-Anwendung ruft das von Apple entwickelte [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) auf, um Benutzerberechtigungen und Anbieterinformationen zu erhalten.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zum [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) .
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * Die Streaming-Anwendung muss einen [Delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für den `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass für die Eigenschaft [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) im Objekt `VSAccountMetadataRequest` ein boolescher Wert gleich `false` angegeben wird, um anzugeben, dass der Benutzer in dieser Phase nicht unterbrochen werden kann.

1. **Rückgabe der Statusinformationen des Partner-Frameworks:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Zugriffsstatus für Benutzerberechtigungen wird gewährt.
   * Die Zuordnungskennung des Benutzeranbieters ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (sofern verfügbar) ist gültig.

1. **Profile abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um alle Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Abrufen von Profilen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) :
   >
   > * Alle _erforderlichen_ Parameter, z. B. `serviceProvider`
   > * Alle _erforderlichen_ Header, wie `Authorization`, `AP-Device-Identifier` und `AP-Partner-Framework-Status`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, sodass die abgerufene Antwort ein Profil vom Typ &quot;appleSSO&quot;enthalten kann.
   >
   > <br/>
   >
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Rückgabe von Informationen zu gefundenen Profilen:** Die Profil-Endpunktantwort enthält Informationen zu den gefundenen Profilen, die mit den empfangenen Parametern und Kopfzeilen verknüpft sind.

1. **Wählen Sie ein Profil aus und fahren Sie mit den Entscheidungsabläufen fort:** Wenn die Profil-Endpunktantwort Profile enthält, verwendet die Streaming-Anwendung ihre interne Logik (schließlich durch Interaktion mit dem Endbenutzer), um eines der verfügbaren Profile auszuwählen und die nachfolgenden Entscheidungsflüsse fortzusetzen.

1. **Fahren Sie mit dem Partner-Authentifizierungsfluss fort:** Wenn die Profil-Endpunktantwort kein Profil enthält, wird die Streaming-Anwendung mit dem Partner-Authentifizierungsfluss fortgesetzt.

+++

+++C. Partnerauthentifizierungsphase

1. **Konfiguration abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um die Liste der MVPDs abzurufen, die eine aktive Integration aufweisen, indem eine Anfrage an den Konfigurations-Endpunkt gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API für den [Abrufen der Konfiguration für bestimmte Service Provider](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) :
   >
   > * Alle _erforderlichen_ Parameter, z. B. `serviceProvider`
   > * Alle _erforderlichen_ Header, wie `Authorization`, `AP-Device-Identifier` und `X-Device-Info`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Rückgabekonfiguration:** Die Antwort des Konfigurationsendpunkts enthält Informationen zu den MVPDs, die eine aktive Integration mit dem Dienstleister haben.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Konfigurationsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur API des [Abrufen der Konfiguration für bestimmte Dienstleister](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) .
   >
   > <br/>
   >
   > Der Endpunkt Konfiguration validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die den [Enhanced Error Codes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entsprechen

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss sicherstellen, dass bei der weiteren Bearbeitung die folgenden für jeden MVPD bereitgestellten Details verarbeitet werden:
   >
   > * `enablePlatformServices`: Gibt an, ob das MVPD derzeit das Apple-Single-Sign-on unterstützt.
   > * `displayInPlatformPicker`: Gibt an, ob der MVPD in der Apple-Auswahl angezeigt werden kann.
   > * `boardingStatus`: Gibt an, ob der MVPD im Apple-Single-Sign-on integriert ist.

1. **Abrufen des Partner-Framework-Status:** Die Streaming-Anwendung ruft das von Apple entwickelte [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) auf, um Benutzerberechtigungen und Anbieterinformationen zu erhalten.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zum [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) .
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * Die Streaming-Anwendung muss einen [Delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für den `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass für die Eigenschaft [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) im Objekt `VSAccountMetadataRequest` ein boolescher Wert gleich `true` angegeben wird, um anzugeben, dass der Benutzer in dieser Phase unterbrochen werden kann, um einen TV-Anbieter auszuwählen.

1. **Rückgabe der Statusinformationen des Partner-Frameworks:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Zugriffsstatus für Benutzerberechtigungen wird gewährt.
   * Die Zuordnungskennung des Benutzeranbieters ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (sofern verfügbar) ist gültig.

1. **Anfrage zur Partnerauthentifizierung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um durch Aufruf des Endpunkts Sitzungspartner eine Authentifizierungssitzung zu starten.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zur [Abrufen der Partner-Authentifizierungsanforderung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `partner`
   > * Alle _erforderlichen_ Header wie `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ -Kopfzeilen und -Parameter
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, sodass die abgerufene Antwort eine Partner-Authentifizierungsanfrage (SAML-Anfrage) enthalten kann.
   >
   > <br/>
   >
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Nächste Aktion angeben:** Die Sitzungspartner-Endpunktantwort enthält die erforderlichen Daten, um die Streaming-Anwendung hinsichtlich der nächsten Aktion zu leiten.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur API für die [API-Anfrage zur Partnerauthentifizierung abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) .
   >
   > <br/>
   >
   > Der Sitzungspartner-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   >
   > Der Sitzungspartner-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die Single-Sign-On-Bedingungen des Partners erfüllt sind:
   >
   >  * Die Single-Sign-On-Konfiguration des Partners auf dem Adobe Pass-Server muss gültig und aktiviert sein.
   >  * Die Statusnutzlast des Partner-Frameworks, die über die Kopfzeile [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) empfangen wurde, muss gültig sein.
   >
   > <br/>
   >
   > Wenn die Überprüfung der Single Sign-On durch Partner fehlschlägt, wird die Antwort standardmäßig auf den grundlegenden Authentifizierungsfluss eingestellt.

1. **Fahren Sie mit den Entscheidungsflüssen fort:** Die Sitzungspartner-Endpunktantwort enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;Autorisieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;direct&quot;festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

1. **Fahren Sie mit dem grundlegenden Authentifizierungsfluss fort:** Die Sitzungspartner-Endpunktantwort enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;Authentifizieren&quot;oder &quot;Fortsetzen&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;interaktiv&quot;oder &quot;direkt&quot;eingestellt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert und die Partnerüberprüfung für Single Sign-On fehlschlägt, kehrt der Adobe Pass-Server zum grundlegenden Authentifizierungsfluss zurück.

   Weitere Informationen zum grundlegenden Authentifizierungsfluss finden Sie in den folgenden Dokumenten:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewählter mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Fahren Sie mit dem Abruf des Profils mithilfe des Antwortflusses zur Partnerauthentifizierung fort:** Die Antwort des Sitzungspartners enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;partner_profile&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;direct&quot;festgelegt.
   * Das Attribut `authenticationRequest - type` enthält das Sicherheitsprotokoll, das vom Partner-Framework für die MVPD-Anmeldung verwendet wird (derzeit nur SAML).
   * Das Attribut `authenticationRequest - request` enthält die SAML-Anforderung, die an das Partner-Framework übergeben wird.
   * Das Attribut `authenticationRequest - attributesNames` enthält die SAML-Attribute, die an das Partner-Framework übergeben werden.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert und die Single Sign-On-Validierung des Partners erfolgreich ist, erhält die Streaming-Anwendung eine Antwort mit Aktionen und Daten, die an das Partner-Framework übergeben werden, um den Authentifizierungsfluss mit dem MVPD zu starten.

1. **Vollständige MVPD-Authentifizierung mit Partner-Framework:** Weiterleiten der Partner-Authentifizierungsanforderung (SAML-Anfrage), die Sie im vorherigen Schritt erhalten haben, an das [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount). Wenn der Authentifizierungsfluss erfolgreich ist, erzeugt die Interaktion des [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) mit dem MVPD eine Antwort zur Partnerauthentifizierung (SAML-Antwort), die zusammen mit den Statusinformationen des Partner-Frameworks zurückgegeben wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zum [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) .
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * Die Streaming-Anwendung muss einen [Delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für den `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden und die im vorherigen Schritt abgerufene Partnerauthentifizierungsanforderung (SAML-Anfrage) enthalten.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass für die Eigenschaft [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) im Objekt `VSAccountMetadataRequest` ein boolescher Wert gleich `true` angegeben wird, um anzugeben, dass der Benutzer in dieser Phase zur Authentifizierung beim ausgewählten TV-Anbieter unterbrochen werden kann.

1. **Antwort zur Partner-Authentifizierung zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Zugriffsstatus für Benutzerberechtigungen wird gewährt.
   * Die Zuordnungskennung des Benutzeranbieters ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (sofern verfügbar) ist gültig.
   * Die Antwort zur Partnerauthentifizierung (SAML-Antwort) ist vorhanden und gültig.

1. **Profil mithilfe der Partner-Authentifizierungsantwort abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um ein Profil zu erstellen und abzurufen, indem der Endpunkt &quot;Profilpartner&quot;aufgerufen wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Abrufen des Profils mithilfe der Antwort zur Partnerauthentifizierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `partner` und `SAMLResponse`
   > * Alle _erforderlichen_ Header, wie `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ -Kopfzeilen und -Parameter
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie gültige Werte für den Partner-Framework-Status und die Partner-Authentifizierungsantwort (SAML-Antwort) enthält, sodass die abgerufene Antwort ein Profil vom Typ &quot;appleSSO&quot;enthalten kann.
   >
   > <br/>
   >
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Rückgabe von Informationen zum Partnerprofil:** Die Profil-Endpunktantwort enthält Informationen zum Partnerprofil, einschließlich des Attributs `type`, das auf &quot;appleSSO&quot;gesetzt ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Profilantwort bereitgestellten Informationen finden Sie in der Dokumentation zur API zum [Abrufen des Profils mithilfe der Antwort zur Partnerauthentifizierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) .
   >
   > <br/>
   >
   > Der Endpunkt &quot;Profile Partner&quot;validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   >
   > Der Endpunkt Profilpartner validiert die Anfragedaten, um sicherzustellen, dass die Single-Sign-On-Bedingungen des Partners erfüllt sind:
   >
   >  * Die Single-Sign-On-Konfiguration des Partners auf dem Adobe Pass-Server muss gültig und aktiviert sein.
   >  * Die Statusnutzlast des Partner-Frameworks, die über die Kopfzeile [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) empfangen wurde, muss gültig sein.
   >
   > <br/>
   >
   > Wenn die Überprüfung der Single Sign-On-Funktion des Partners fehlschlägt, wird die Antwort standardmäßig auf den grundlegenden Abruf des Profils festgelegt.

1. **Fahren Sie mit den Entscheidungsflüssen fort:** Die Streaming-Anwendung kann mit nachfolgenden Entscheidungsflüssen fortfahren.

+++

+++ D. Entscheidungsphasen

1. **Abrufen des Partner-Framework-Status:** Die Streaming-Anwendung ruft das von Apple entwickelte [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) auf, um Benutzerberechtigungen und Anbieterinformationen zu erhalten.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zum [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) .
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * Die Streaming-Anwendung muss einen [Delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für den `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass für die Eigenschaft [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) im Objekt `VSAccountMetadataRequest` ein boolescher Wert gleich `false` angegeben wird, um anzugeben, dass der Benutzer in dieser Phase nicht unterbrochen werden kann.

   >[!TIP]
   > 
   > Empfehlung: Die Streaming-Anwendung kann einen zwischengespeicherten Wert für die Statusinformationen des Partner-Frameworks verwenden. Wir empfehlen eine Aktualisierung, wenn die Anwendung von Hintergrund zu Vordergrund wechselt.

1. **Rückgabe der Statusinformationen des Partner-Frameworks:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Zugriffsstatus für Benutzerberechtigungen wird gewährt.
   * Die Zuordnungskennung des Benutzeranbieters ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (sofern verfügbar) ist gültig.

1. **Vorabautorisierungsentscheidungen abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Vorautorisierungsentscheidungen für eine Liste von Ressourcen zu erhalten, indem sie den Endpunkt Entscheidungsvorautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Vorautorisierungsentscheidungen mithilfe einer bestimmten mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) -API .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, bevor eine Anforderung weiter ausgeführt wird, wenn das ausgewählte Profil ein Profil vom Typ &quot;appleSSO&quot;ist.
   >
   > <br/>
   >
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Rückgabe von Entscheidungen bezüglich der Vorabautorisierung:** Die Antwort der Entscheidungsvorautorisierung des Endpunkts enthält eine `Permit` - oder `Deny` -Entscheidung für jede Ressource:
   * Eine &quot;`Permit`&quot;-Entscheidung bedeutet, dass die Ressource wiedergegeben werden kann. Die Antwort enthält kein Medien-Token, da der Fluss zur Vorabautorisierung nicht zum Abspielen von Ressourcen verwendet werden darf.
   * Eine &quot;`Deny`&quot;-Entscheidung bedeutet, dass die Ressource nicht wiedergegeben werden kann. Die Antwort enthält eine Fehler-Payload, die der Dokumentation [Verbesserte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Entscheidungsantwort finden Sie in der Dokumentation zur API [Abrufen von Vorautorisierungsentscheidungen mithilfe einer bestimmten mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) -API.
   >
   > <br/>
   >
   > Der Endpunkt Entscheidungsvorautorisierung validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entsprechen.

1. **Abrufen des Partner-Framework-Status:** Die Streaming-Anwendung ruft das von Apple entwickelte [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) auf, um Benutzerberechtigungen und Anbieterinformationen zu erhalten.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zum [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) .
   >
   > <br/>
   >
   > * Die Streaming-Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
   > * Die Streaming-Anwendung muss einen [Delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für den `VSAccountManager` bereitstellen.
   > * Die Streaming-Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen senden.
   > * Die Streaming-Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass für die Eigenschaft [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) im Objekt `VSAccountMetadataRequest` ein boolescher Wert gleich `false` angegeben wird, um anzugeben, dass der Benutzer in dieser Phase nicht unterbrochen werden kann.

   >[!TIP]
   >
   > Empfehlung: Die Streaming-Anwendung kann einen zwischengespeicherten Wert für die Statusinformationen des Partner-Frameworks verwenden. Wir empfehlen eine Aktualisierung, wenn die Anwendung von Hintergrund zu Vordergrund wechselt.

1. **Rückgabe der Statusinformationen des Partner-Frameworks:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Zugriffsstatus für Benutzerberechtigungen wird gewährt.
   * Die Zuordnungskennung des Benutzeranbieters ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (sofern verfügbar) ist gültig.

1. **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Endpunkt Entscheidungsautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) -API:
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, bevor eine Anforderung weiter ausgeführt wird, wenn das ausgewählte Profil ein Profil vom Typ &quot;appleSSO&quot;ist.
   >
   > <br/>
   >
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Rückgabe der Autorisierungsentscheidung:** Die Antwort des Endpunkts Entscheidungsautorisierung enthält eine `Permit` - oder `Deny` -Entscheidung für die jeweilige Ressource:
   * Eine &quot;`Permit`&quot;-Entscheidung bedeutet, dass die Ressource wiedergegeben werden kann. Die Antwort enthält ein Medien-Token.
   * Eine &quot;`Deny`&quot;-Entscheidung bedeutet, dass die Ressource nicht wiedergegeben werden kann. Die Antwort enthält eine Fehler-Payload, die der Dokumentation [Verbesserte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) -API .
   >
   > <br/>
   >
   > Der Endpunkt Entscheidungsautorisierung validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entsprechen.

+++

+++ D. Abmeldephase

1. **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um den Abmeldefluss durch Aufruf des Adobe Pass-Abmeldeendpunkts zu initiieren.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request)-API unter [Initiate logout .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Geben Sie die nächste Aktion an:** Die Antwort des Adobe Pass Logout-Endpunkts enthält die erforderlichen Daten, um die Streaming-Anwendung in Bezug auf die nächste Aktion zu leiten:
   * Das Attribut `url` fehlt, da der Benutzer mit der Partnerebene (System) interagieren muss, um den Abmeldefluss abzuschließen.
   * Das Attribut `actionName` ist auf &quot;partner_logout&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;partner_interactive&quot;festgelegt.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Abmeldeantwort finden Sie in der Dokumentation zur API für die MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response)-API unter [Initiate logout .
   >
   > <br/>
   >
   > Der Adobe Pass Logout-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) entsprechen.

   >[!IMPORTANT]
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie den Benutzer angibt, sich weiter von der Partnerebene abzumelden.

+++
