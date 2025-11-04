---
title: Single Sign-On - Partner - Flüsse
description: REST API v2 - Single Sign-On - Partner - Flüsse
exl-id: 5735d67f-a311-4d03-ad48-93c0fcbcace5
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# Single Sign-on mit Partnerflüssen {#single-sign-on-partner-flows}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

>[!MORELIKETHIS]
>
> Stellen Sie sicher, dass Sie auch die häufig gestellten Fragen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) besuchen.

Die Partnermethode ermöglicht es mehreren Anwendungen, eine Partner-Framework-Status-Payload zu verwenden, um Single Sign-on (SSO) auf Geräteebene bei der Verwendung von Adobe Pass-Services zu erzielen.

Die Programme sind für das Abrufen der Status-Payload des Partner-Frameworks mithilfe von partnerspezifischen Frameworks oder Bibliotheken außerhalb von Adobe Pass-Systemen verantwortlich.

Die Programme sind dafür verantwortlich, diese Partner-Framework-Status-Payload als Teil der `AP-Partner-Framework-Status`-Kopfzeile für alle Anfragen einzuschließen, die sie angeben.

Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

Die Adobe Pass-Authentifizierungs-REST-API V2 unterstützt das Partner Single Sign-On (SSO) für Endbenutzer von Client-Anwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden.

Weitere Informationen zu Single Sign-on (SSO) für die Apple-Plattform finden Sie in der Dokumentation zum [Apple SSO Cookbook (REST API V2)](/help/premium-workflow/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md).

## Partnerauthentifizierungsanfrage abrufen {#retrieve-partner-authentication-request}

### Voraussetzungen {#prerequisites-retrieve-partner-authentication-request}

Stellen Sie vor dem Abrufen der Partnerauthentifizierungsanfrage sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Das Partner-Framework muss eine MVPD auswählen.
* Die Streaming-Anwendung muss die Partnerframework-Statusinformationen vom Partnerframework abrufen und an den Adobe Pass-Server übergeben.
* Die Streaming-Anwendung muss die Partnerauthentifizierungsanfrage vom Adobe Pass-Server abrufen und an das Partner-Framework übergeben.

>[!IMPORTANT]
>
> Annahmen
> 
> <br/>
> 
> * Das Partner-Framework unterstützt Benutzerinteraktionen bei der Auswahl einer MVPD.
> * Das Partner-Framework unterstützt Benutzerinteraktionen bei der Authentifizierung mit der ausgewählten MVPD.
> * Das Partner-Framework stellt Benutzerberechtigungen und Anbieterinformationen bereit.

### Workflow {#workflow-retrieve-partner-authentication-request}

Führen Sie die angegebenen Schritte aus, um die Partnerauthentifizierungsanfrage abzurufen, wie im folgenden Diagramm dargestellt.

![Partnerauthentifizierungsanfrage abrufen](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-partner-authentication-request-flow.png)

*Partnerauthentifizierungsanfrage abrufen*

1. **Partner-Framework-Status abrufen:** Die Streaming-Anwendung ruft das Partner-Framework außerhalb von Adobe Pass-Systemen auf, um Benutzerberechtigungen und Anbieterinformationen abzurufen.

1. **Partner-Framework-Statusinformationen zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Benutzerberechtigungs-Zugriffsstatus wird gewährt.
   * Die Kennung der Benutzeranbieter-Zuordnung ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (falls verfügbar) ist gültig.

1. **Partnerauthentifizierungsanfrage abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um eine Authentifizierungssitzung zu initiieren, indem sie den Sitzungspartner-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [&#x200B; in der API](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)Dokumentation zum Abrufen von Partnerauthentifizierungsanfragen:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider` und `partner`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ Kopfzeilen und Parameter
   >
   > <br/>
   >
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für den Partnerframework-Status enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Nächste Aktion angeben:** Die Antwort des Sitzungspartner-Endpunkts enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu leiten.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [&#x200B; in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)Dokumentation zum Abrufen der Partnerauthentifizierungsanfrage .
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
   > Wenn die einfache Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.
   >
   > <br/>
   >
   > Der Sitzungspartner-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die Bedingungen für das einmalige Anmelden des Partners erfüllt sind:
   >
   >  * Die Partnerkonfiguration für einmaliges Anmelden im Adobe Pass-Server muss gültig und aktiviert sein.
   >  * Die Payload des Partner-Frameworks, die über die Kopfzeile [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) empfangen wurde, muss gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung des Partner-Single-Sign-ons fehlschlägt, wird die Antwort standardmäßig auf den einfachen Authentifizierungsfluss eingestellt.

1. **Fluss zum Abrufen von Profilen mit der Antwort zur Partnerauthentifizierung fortsetzen:** Die Antwort „Sitzungspartner-Endpunkt“ enthält die folgenden Daten:
   * Das `actionName`-Attribut ist auf „partner_profile“ festgelegt.
   * Das `actionType`-Attribut ist auf „direct“ festgelegt.
   * Das `authenticationRequest - type`-Attribut enthält das Sicherheitsprotokoll, das vom Partner-Framework für die Anmeldung bei MVPD verwendet wird (derzeit nur auf SAML festgelegt).
   * Das `authenticationRequest - request`-Attribut enthält die SAML-Anfrage, die an das Partner-Framework übergeben wird.
   * Das `authenticationRequest - attributesNames`-Attribut enthält die SAML-Attribute, die an das Partner-Framework übergeben werden.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert und die Partnervalidierung mit Single Sign-on erfolgreich ist, erhält die Streaming-Anwendung eine Antwort mit Aktionen und Daten, die an das Partner-Framework übergeben werden, um den Authentifizierungsfluss mit der MVPD zu starten.

   Weitere Informationen zum Abruf von Profilen mithilfe einer Partnerauthentifizierungsantwort finden Sie im Abschnitt [Erstellen und Abrufen von Profilen mithilfe einer Partnerauthentifizierungsantwort](#create-and-retrieve-profile-using-partner-authentication-response).

1. **Mit einfachem Authentifizierungsfluss fortfahren:** Die Antwort des Sitzungspartner-Endpunkts enthält die folgenden Daten:
   * Das `actionName`-Attribut wird entweder auf „Authentifizieren“ oder auf „Fortsetzen“ festgelegt.
   * Das `actionType`-Attribut wird entweder auf „interaktiv“ oder auf „direkt“ festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert und die Partnerüberprüfung für einmaliges Anmelden fehlschlägt, kehrt der Adobe Pass-Server zum einfachen Authentifizierungsfluss zurück.

   Weitere Informationen zum grundlegenden Authentifizierungsablauf finden Sie in den folgenden Dokumenten:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](../basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Fahren Sie mit Entscheidungsflüssen fort:** Die Antwort „Sitzungspartner-Endpunkt“ enthält die folgenden Daten:
   * Das `actionName`-Attribut ist auf „authorize“ festgelegt.
   * Das `actionType`-Attribut ist auf „direct“ festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

   >[!IMPORTANT]
   >
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für den Partnerframework-Status enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

## Erstellen und Abrufen eines Profils mithilfe der Antwort zur Partnerauthentifizierung {#create-and-retrieve-profile-using-partner-authentication-response}

### Voraussetzungen {#prerequisites-create-and-retrieve-profile-using-partner-authentication-response}

Stellen Sie vor dem Abrufen des Profils mit einer Antwort der Partnerauthentifizierung sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Das Partner-Framework muss eine Authentifizierung mit der ausgewählten MVPD durchführen.
* Die Streaming-Anwendung muss die Antwort zur Partnerauthentifizierung zusammen mit Partnerframework-Statusinformationen vom Partnerframework abrufen und an den Adobe Pass-Server übergeben.

>[!IMPORTANT]
>
> Annahme
>
> * Das Partner-Framework unterstützt Benutzerinteraktionen bei der Auswahl einer MVPD.
> * Das Partner-Framework unterstützt Benutzerinteraktionen bei der Authentifizierung mit der ausgewählten MVPD.
> * Das Partner-Framework stellt Benutzerberechtigungen und Anbieterinformationen bereit.

### Workflow {#workflow-create-and-retrieve-profile-using-partner-authentication-response}

Führen Sie die angegebenen Schritte aus, um den Profilabruffluss mithilfe einer Partnerauthentifizierungsantwort zu implementieren, wie im folgenden Diagramm dargestellt.

![Erstellen und Abrufen von Profilen mithilfe der Antwort zur Partnerauthentifizierung](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-profile-using-partner-authentication-response-flow.png)

*Authentifiziertes Profil mithilfe einer Antwort der Partnerauthentifizierung erstellen und abrufen*

1. **Vollständige MVPD-Authentifizierung mit Partnerframework:** Wenn der Authentifizierungsfluss erfolgreich ist, erzeugt die Partnerframework-Interaktion mit der MVPD eine Partnerauthentifizierungsantwort (SAML-Antwort), die zusammen mit den Statusinformationen des Partnerframeworks zurückgegeben wird.

1. **Antwort zur Partnerauthentifizierung zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Benutzerberechtigungs-Zugriffsstatus wird gewährt.
   * Die Kennung der Benutzeranbieter-Zuordnung ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (falls verfügbar) ist gültig.

1. **Profil mit Partnerauthentifizierungsantwort erstellen und abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um ein Profil zu erstellen und abzurufen, indem sie den Partner-Endpunkt „Profile“ aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [&#x200B; finden Sie in der API-Dokumentation &#x200B;](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) Erstellen und Abrufen von Profilen mit Partnerauthentifizierungsantwort:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `partner` und `SAMLResponse`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ Kopfzeilen und Parameter
   >
   > <br/>
   > 
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für den Partnerframework-Status enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Partnerprofil erstellen und speichern:** Der Adobe Pass-Server erstellt und speichert ein Partnerprofil, nachdem sichergestellt wurde, dass alle Bedingungen erfüllt sind.

1. **Rückgabeinformationen zum Partnerprofil:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum Partnerprofil, einschließlich des Attributs, das auf „appleSSO“ festgelegt `type`.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu [&#x200B; in einer Profilantwort enthaltenen Informationen finden Sie in der API](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)Dokumentation Erstellen und Abrufen von Profilen mit Partnerauthentifizierungsantwort .
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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.
   >
   > <br/>
   >
   > Der Partner-Endpunkt „Profile“ validiert die Anfragedaten, um sicherzustellen, dass die Bedingungen für das einmalige Anmelden des Partners erfüllt sind:
   >
   >  * Die Partnerkonfiguration für einmaliges Anmelden im Adobe Pass-Server muss gültig und aktiviert sein.
   >  * Die Payload des Partner-Frameworks, die über die Kopfzeile [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) empfangen wurde, muss gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung des Partner-Single-Sign-ons fehlschlägt, wird als Antwort standardmäßig der grundlegende Profilabruffluss verwendet.

1. **Fahren Sie mit Entscheidungsflüssen fort:** Die Streaming-Anwendung kann mit nachfolgenden Entscheidungsflüssen fortfahren.

   >[!IMPORTANT]
   >
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für den Partnerframework-Status enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AP-Partner-Framework-Status` Kopfzeile finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).
