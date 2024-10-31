---
title: Single Sign-On - Partner - Fluss
description: REST API V2 - Single Sign-On - Partner - Flüsse
exl-id: 5735d67f-a311-4d03-ad48-93c0fcbcace5
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1444'
ht-degree: 0%

---

# Single Sign-on mit Partnerflüssen {#single-sign-on-partner-flows}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

Die Partner -Methode ermöglicht es mehreren Anwendungen, eine Partner-Framework-Status-Payload zu verwenden, um Single Sign-On (SSO) auf Geräteebene bei der Verwendung von Adobe Pass-Diensten zu erreichen.

Die Anwendungen sind für das Abrufen der Statusnutzlast des Partner-Frameworks mithilfe von Partner-spezifischen Frameworks oder Bibliotheken außerhalb von Adobe Pass-Systemen verantwortlich.

Die Anwendungen sind dafür verantwortlich, diese Partner-Framework-Status-Payload als Teil der `AP-Partner-Framework-Status` -Kopfzeile für alle Anforderungen einzubeziehen, die sie angeben.

Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

Die Adobe Pass Authentication REST API V2 unterstützt Single Sign-On (SSO) für Endbenutzer von Clientanwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden.

Weitere Informationen zum Single Sign-on (SSO) für die Apple-Plattform finden Sie in der Dokumentation zum [Apple SSO Cookbook (REST API V2)](/help/authentication/single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-cookbook-rest-api-v2.md) .

## Anfrage zur Partnerauthentifizierung abrufen {#retrieve-partner-authentication-request}

### Voraussetzungen {#prerequisites-retrieve-partner-authentication-request}

Bevor Sie die Partnerauthentifizierungsanforderung abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Das Partner-Framework muss einen MVPD auswählen.
* Die Streaming-Anwendung muss die Statusinformationen des Partner-Frameworks aus dem Partner-Framework abrufen und an den Adobe Pass-Server übergeben.
* Die Streaming-Anwendung muss die Partnerauthentifizierungsanforderung vom Adobe Pass-Server abrufen und an das Partner-Framework übergeben.

>[!IMPORTANT]
>
> Annahmen
> 
> <br/>
> 
> * Das Partner-Framework unterstützt die Benutzerinteraktion zur Auswahl eines MVPD.
> * Das Partner-Framework unterstützt Benutzerinteraktionen, um sich mit dem ausgewählten MVPD zu authentifizieren.
> * Das Partner-Framework stellt Benutzerberechtigungen und Anbieterinformationen bereit.

### Workflow {#workflow-retrieve-partner-authentication-request}

Führen Sie die angegebenen Schritte aus, um die Partnerauthentifizierungsanforderung abzurufen, wie im folgenden Diagramm dargestellt.

![Authentifizierungsanfrage von Partnern abrufen](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-partner-authentication-request-flow.png)

*Authentifizierungsanfrage von Partnern abrufen*

1. **Abrufen des Partner-Framework-Status:** Die Streaming-Anwendung ruft das Partner-Framework außerhalb der Adobe Pass-Systeme auf, um Benutzerberechtigungen und Anbieterinformationen zu erhalten.

1. **Rückgabe der Statusinformationen des Partner-Frameworks:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Zugriffsstatus für Benutzerberechtigungen wird gewährt.
   * Die Zuordnungskennung des Benutzeranbieters ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (sofern verfügbar) ist gültig.

1. **Anfrage zur Partnerauthentifizierung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um durch Aufruf des Endpunkts Sitzungspartner eine Authentifizierungssitzung zu starten.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zur [Abrufen der Partner-Authentifizierungsanforderung](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `partner`
   > * Alle _erforderlichen_ Header wie `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ -Kopfzeilen und -Parameter
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Nächste Aktion angeben:** Die Sitzungspartner-Endpunktantwort enthält die erforderlichen Daten, um die Streaming-Anwendung hinsichtlich der nächsten Aktion zu leiten.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur API für die [API-Anfrage zur Partnerauthentifizierung abrufen](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) .
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
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   >
   > Der Sitzungspartner-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die Single-Sign-On-Bedingungen des Partners erfüllt sind:
   >
   >  * Die Single-Sign-On-Konfiguration des Partners auf dem Adobe Pass-Server muss gültig und aktiviert sein.
   >  * Die Statusnutzlast des Partner-Frameworks, die über die Kopfzeile [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) empfangen wurde, muss gültig sein.
   >
   > <br/>
   >
   > Wenn die Überprüfung der Single Sign-On durch Partner fehlschlägt, wird die Antwort standardmäßig auf den grundlegenden Authentifizierungsfluss eingestellt.

1. **Fahren Sie mit dem Profilabruf-Fluss über die Antwort zur Partnerauthentifizierung fort:** Die Antwort des Sitzungspartners-Endpunkts enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;partner_profile&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;direct&quot;festgelegt.
   * Das Attribut `authenticationRequest - type` enthält das Sicherheitsprotokoll, das vom Partner-Framework für die MVPD-Anmeldung verwendet wird (derzeit nur SAML).
   * Das Attribut `authenticationRequest - request` enthält die SAML-Anforderung, die an das Partner-Framework übergeben wird.
   * Das Attribut `authenticationRequest - attributesNames` enthält die SAML-Attribute, die an das Partner-Framework übergeben werden.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert und die Single Sign-On-Validierung des Partners erfolgreich ist, erhält die Streaming-Anwendung eine Antwort mit Aktionen und Daten, die an das Partner-Framework übergeben werden, um den Authentifizierungsfluss mit dem MVPD zu starten.

   Weitere Informationen zum Profilabruf mit einer Antwort zur Partnerauthentifizierung finden Sie im Abschnitt [Profil mit Antwort zur Partnerauthentifizierung abrufen](#retrieve-profile-using-partner-authentication-response) .

1. **Fahren Sie mit dem grundlegenden Authentifizierungsfluss fort:** Die Sitzungspartner-Endpunktantwort enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;Authentifizieren&quot;oder &quot;Fortsetzen&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;interaktiv&quot;oder &quot;direkt&quot;eingestellt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert und die Partnerüberprüfung für Single Sign-On fehlschlägt, kehrt der Adobe Pass-Server zum grundlegenden Authentifizierungsfluss zurück.

   Weitere Informationen zum grundlegenden Authentifizierungsfluss finden Sie in den folgenden Dokumenten:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](../basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewählter mvpd](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Fahren Sie mit den Entscheidungsflüssen fort:** Die Sitzungspartner-Endpunktantwort enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;Autorisieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;direct&quot;festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

## Profil mithilfe der Antwort auf die Partnerauthentifizierung abrufen {#retrieve-profile-using-partner-authentication-response}

### Voraussetzungen {#prerequisites-retrieve-profile-using-partner-authentication-response}

Stellen Sie vor dem Abrufen des Profils mithilfe einer Antwort zur Partnerauthentifizierung sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Das Partner-Framework muss die Authentifizierung mit dem ausgewählten MVPD durchführen.
* Die Streaming-Anwendung muss die Antwort zur Partnerauthentifizierung zusammen mit den Statusinformationen des Partner-Frameworks vom Partner-Framework abrufen und an den Adobe Pass-Server übergeben.

>[!IMPORTANT]
>
> Annahme
>
> * Das Partner-Framework unterstützt die Benutzerinteraktion zur Auswahl eines MVPD.
> * Das Partner-Framework unterstützt Benutzerinteraktionen, um sich mit dem ausgewählten MVPD zu authentifizieren.
> * Das Partner-Framework stellt Benutzerberechtigungen und Anbieterinformationen bereit.

### Workflow {#workflow-retrieve-profile-using-partner-authentication-response}

Führen Sie die angegebenen Schritte aus, um den Abruffluss des Profils mithilfe einer Antwort zur Partnerauthentifizierung zu implementieren, wie im folgenden Diagramm dargestellt.

![Profil mithilfe der Antwort zur Partnerauthentifizierung abrufen](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-profile-using-partner-authentication-response-flow.png)

*Abrufen eines authentifizierten Profils mithilfe der Antwort zur Partnerauthentifizierung*

1. **Vollständige MVPD-Authentifizierung mit Partner-Framework:** Wenn der Authentifizierungsfluss erfolgreich ist, erzeugt die Partner-Framework-Interaktion mit dem MVPD eine Partner-Authentifizierungsantwort (SAML-Antwort), die zusammen mit den Statusinformationen des Partner-Frameworks zurückgegeben wird.

1. **Antwort zur Partner-Authentifizierung zurückgeben:** Die Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   * Der Zugriffsstatus für Benutzerberechtigungen wird gewährt.
   * Die Zuordnungskennung des Benutzeranbieters ist vorhanden und gültig.
   * Das Ablaufdatum des Benutzeranbieterprofils (sofern verfügbar) ist gültig.

1. **Profil mithilfe der Partner-Authentifizierungsantwort abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um ein Profil zu erstellen und abzurufen, indem der Endpunkt &quot;Profilpartner&quot;aufgerufen wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Abrufen des Profils mithilfe der Antwort zur Partnerauthentifizierung](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `partner` und `SAMLResponse`
   > * Alle _erforderlichen_ Header, wie `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` und `AP-Partner-Framework-Status`
   > * Alle _optionalen_ -Kopfzeilen und -Parameter
   >
   > <br/>
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .

1. **Partnerprofil erstellen und speichern:** Der Adobe Pass-Server erstellt und speichert ein Partnerprofil, nachdem sichergestellt wurde, dass alle Bedingungen erfüllt sind.

1. **Rückgabe von Informationen zum Partnerprofil:** Die Profil-Endpunktantwort enthält Informationen zum Partnerprofil, einschließlich des Attributs `type`, das auf &quot;appleSSO&quot;gesetzt ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Profilantwort bereitgestellten Informationen finden Sie in der Dokumentation zur API zum [Abrufen des Profils mithilfe der Antwort zur Partnerauthentifizierung](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) .
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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   >
   > Der Endpunkt Profilpartner validiert die Anfragedaten, um sicherzustellen, dass die Single-Sign-On-Bedingungen des Partners erfüllt sind:
   >
   >  * Die Single-Sign-On-Konfiguration des Partners auf dem Adobe Pass-Server muss gültig und aktiviert sein.
   >  * Die Statusnutzlast des Partner-Frameworks, die über die Kopfzeile [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) empfangen wurde, muss gültig sein.
   >
   > <br/>
   >
   > Wenn die Überprüfung der Single Sign-On-Funktion des Partners fehlschlägt, wird die Antwort standardmäßig auf den grundlegenden Abruf des Profils festgelegt.

1. **Fahren Sie mit den Entscheidungsflüssen fort:** Die Streaming-Anwendung kann mit nachfolgenden Entscheidungsflüssen fortfahren.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für den Partner-Framework-Status enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `AP-Partner-Framework-Status` finden Sie in der Dokumentation [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) .
