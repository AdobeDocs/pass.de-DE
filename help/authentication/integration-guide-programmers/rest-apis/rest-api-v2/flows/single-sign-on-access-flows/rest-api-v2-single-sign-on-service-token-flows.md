---
title: Single Sign-On - Service-Token - Flüsse
description: REST API v2 - Single Sign-On - Service-Token - Flüsse
exl-id: b0082d2a-e491-4cb5-bb40-35ba10db6b1a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1848'
ht-degree: 0%

---

# Single Sign-on mit Service-Token-Flüssen{#single-sign-on-service-token-full-flows}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

Die Service-Token-Methode ermöglicht es mehreren Anwendungen, bei der Verwendung von Adobe Pass-Services eine eindeutige Benutzerkennung zu verwenden, um Single Sign-on (SSO) auf mehreren Geräten und Plattformen zu erzielen.

Die Programme sind für das Abrufen der Payload der eindeutigen Benutzerkennung mithilfe von externen Identity Services außerhalb von Adobe Pass-Systemen verantwortlich, z. B.:

* Ein DTC-Dienst (Direct-to-Consumer), bei dem sich Benutzer auf jedem Gerät mit denselben Anmeldeinformationen anmelden und mit derselben Benutzer-ID oder demselben Benutzerkontonamen verknüpft sind.
* Ein Authentifizierungsdienst eines Drittanbieters, wie Google oder Facebook, bei dem sich Benutzende auf jedem Gerät mit denselben Anmeldeinformationen anmelden und mit derselben E-Mail-Adresse verknüpft sind.

Die Programme sind dafür verantwortlich, diese Payload mit eindeutiger Benutzerkennung als Teil der `AD-Service-Token`-Kopfzeile für alle Anfragen einzuschließen, die sie angeben.

Weitere Informationen zu `AD-Service-Token`-Kopfzeile finden Sie in der Dokumentation [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)&quot;.

## Durchführen der Authentifizierung über Single Sign-on mit dem Service-Token {#performing-authentication-flow-using-service-token-single-sign-on-method}

### Voraussetzungen {#prerequisites-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Stellen Sie vor dem Ausführen des Authentifizierungsflusses über Single Sign-on mit einem Service-Token sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Der externe Identity Service muss konsistente Informationen als `JWS` Payload über alle Anwendungen hinweg über mehrere Geräte und Plattformen zurückgeben.
* Die erste Streaming-Anwendung muss für alle Anfragen, die sie angeben, die eindeutige Benutzerkennung abrufen und die `JWS` Payload als Teil der Kopfzeile [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)) einschließen.
* Die erste Streaming-Anwendung muss eine MVPD auswählen.
* Die erste Streaming-Anwendung muss eine Authentifizierungssitzung initiieren, um sich mit der ausgewählten MVPD anzumelden.
* Die erste Streaming-Anwendung muss sich mit der ausgewählten MVPD in einem Benutzeragenten authentifizieren.
* Die zweite Streaming-Anwendung muss für alle Anfragen, die sie spezifizieren, die eindeutige Benutzerkennung abrufen und die `JWS` Payload als Teil der [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)-Kopfzeile einschließen.

>[!IMPORTANT]
>
> Annahmen
> 
> <br/>
> 
> * Die erste Streaming-Anwendung unterstützt Benutzerinteraktionen bei der Auswahl einer MVPD.
> * Die erste Streaming-Anwendung unterstützt Benutzerinteraktionen zur Authentifizierung bei der ausgewählten MVPD in einem Benutzeragenten.

### Workflow {#workflow-steps-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Führen Sie die angegebenen Schritte aus, um den Authentifizierungsfluss durch Single Sign-on mithilfe eines Service-Tokens zu implementieren, wie im folgenden Diagramm dargestellt.

![Authentifizierung über Single Sign-on mithilfe des Service-Tokens durchführen](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-service-token-flow.png)

*Authentifizierung über Single Sign-on mithilfe des Service-Tokens durchführen*

1. **Mit Identity Service authentifizieren:** Die erste Streaming-Anwendung ruft den Identity Service außerhalb von Adobe Pass-Systemen auf, um die mit der eindeutigen Benutzerkennung verknüpfte `JWS` Payload abzurufen.

1. **Eindeutige Benutzerkennung als JWS zurückgeben:** Die erste Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Sicherheitsbedingungen erfüllt werden:
   * Payload ist nicht abgelaufen.
   * Payload ist signiert.

1. **Authentifizierungssitzung erstellen:** Die erste Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Authentifizierungssitzung zu initiieren, indem der Sessions-Endpunkt aufgerufen wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)Dokumentation zu „Authentifizierungssitzung erstellen“:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd`, `domainName` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Benutzerkennung enthält, bevor sie eine Anfrage stellt.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AD-Service-Token`-Kopfzeile finden Sie in der Dokumentation [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)&quot;.

1. **Nächste Aktion angeben:** Die Antwort des Sitzungs-Endpunkts enthält die erforderlichen Daten, um die erste Streaming-Anwendung bezüglich der nächsten Aktion anzuleiten.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)Dokumentation zu „Authentifizierungssitzung erstellen“.
   >
   > <br/>
   > 
   > Der Sessions-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **URL im Benutzeragenten öffnen** Die Antwort des Sitzungs-Endpunkts enthält die folgenden Daten:
   * Die `url`, die verwendet werden kann, um die interaktive Authentifizierung auf der Anmeldeseite von MVPD zu initiieren.
   * Das `actionName`-Attribut ist auf „Authenticate“ festgelegt.
   * Das `actionType`-Attribut ist auf „interaktiv“ festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert, öffnet die erste Streaming-Anwendung einen Benutzeragenten, um die angegebene `url` zu laden, und sendet eine Anfrage an den Authentifizierungsendpunkt. Dieser Fluss kann mehrere Weiterleitungen enthalten, die den Benutzer letztendlich zur Anmeldeseite von MVPD führen und gültige Anmeldeinformationen angeben.

1. **Vollständige MVPD-Authentifizierung:** Wenn der Authentifizierungsfluss erfolgreich ist, speichert die Benutzeragenten-Interaktion ein reguläres Profil im Adobe Pass-Backend und erreicht die angegebene `redirectUrl`.

1. **Profil für bestimmten Code abrufen:** Die erste Streaming-Anwendung sammelt alle erforderlichen Daten, um Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt „Profiles“ gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)Dokumentation zum Abrufen von Profilen für bestimmten Code:
   > 
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `code`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

   >[!TIP]
   >
   > Empfehlung: Die Streaming-Anwendung kann warten, bis der Benutzeragent den angegebenen `redirectUrl` erreicht hat, um zu überprüfen, ob das reguläre Profil erfolgreich generiert und gespeichert wurde.

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Profil anhand der empfangenen Parameter und Kopfzeilen.

1. **Informationen zum regulären Profil zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu [ in einer Profilantwort bereitgestellten Informationen finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)Dokumentation zum Abrufen von Profilen für bestimmten Code.
   >
   > <br/>
   > 
   > Der Profiles-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **Fahren Sie mit Entscheidungsflüssen fort:** Die erste Streaming-Anwendung kann mit nachfolgenden Entscheidungsflüssen fortfahren.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Benutzerkennung enthält, bevor sie eine Anfrage stellt.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AD-Service-Token`-Kopfzeile finden Sie in der Dokumentation [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)&quot;.

1. **Mit Identity Service authentifizieren:** Die zweite Streaming-Anwendung ruft den Identity Service außerhalb von Adobe Pass-Systemen auf, um die mit der eindeutigen Benutzerkennung verknüpfte `JWS` Payload abzurufen.

1. **Eindeutige Benutzerkennung als JWS zurückgeben:** Die zweite Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Sicherheitsbedingungen erfüllt werden:
   * Payload ist nicht abgelaufen.
   * Payload ist signiert.

1. **Profile abrufen:** Die zweite Streaming-Anwendung sammelt alle erforderlichen Daten, um alle Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt „Profiles“ gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)Dokumentation zum Abrufen von Profilen:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Benutzerkennung enthält, bevor sie eine Anfrage stellt.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AD-Service-Token`-Kopfzeile finden Sie in der Dokumentation [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)&quot;.

1. **Single-Sign-On-Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Single-Sign-On-Profil basierend auf den empfangenen Parametern und Kopfzeilen.

1. **Rückgabeinformationen zum Single-Sign-On-Profil zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Profilantwort bereitgestellten Informationen finden ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) in der API-Dokumentation zum Abrufen von Profilen .
   > 
   > <br/>
   > 
   > Der Profiles-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **Fahren Sie mit Entscheidungsflüssen fort:** Die zweite Streaming-Anwendung kann mit nachfolgenden Entscheidungsflüssen fortfahren.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Benutzerkennung enthält, bevor sie eine Anfrage stellt.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AD-Service-Token`-Kopfzeile finden Sie in der Dokumentation [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)&quot;.

## Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe des Service-Tokens {#performing-authorization-flow-using-service-token-single-sign-on-method}

### Voraussetzungen {#prerequisites-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Stellen Sie vor dem Ausführen des Autorisierungsflusses über Single Sign-on mit einem Service-Token sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Der externe Identity Service muss konsistente Informationen als `JWS` Payload über alle Anwendungen hinweg über mehrere Geräte und Plattformen zurückgeben.
* Die erste Streaming-Anwendung muss für alle Anfragen, die sie angeben, die eindeutige Benutzerkennung abrufen und die `JWS` Payload als Teil der Kopfzeile [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)) einschließen.
* Die zweite Streaming-Anwendung muss eine Autorisierungsentscheidung abrufen, bevor eine vom Benutzer ausgewählte Ressource wiedergegeben wird.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die erste Streaming-Anwendung hat die Authentifizierung durchgeführt und einen gültigen Wert für die Anforderungskopfzeile [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) enthalten.

### Workflow {#workflow-steps-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Führen Sie die angegebenen Schritte aus, um den Autorisierungsfluss durch Single Sign-on mithilfe eines Service-Tokens zu implementieren, wie im folgenden Diagramm dargestellt.

![Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe des Service-Tokens](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-service-token-flow.png)

*Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe des Service-Tokens*

1. **Mit Identity Service authentifizieren:** Die zweite Streaming-Anwendung ruft den Identity Service außerhalb von Adobe Pass-Systemen auf, um die mit der eindeutigen Benutzerkennung verknüpfte `JWS` Payload abzurufen.

1. **Eindeutige Benutzerkennung als JWS zurückgeben:** Die zweite Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Sicherheitsbedingungen erfüllt werden:
   * Payload ist nicht abgelaufen.
   * Payload ist signiert.

1. **Autorisierungsentscheidung abrufen:** Die zweite Streaming-Anwendung sammelt alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Decisions Authorize-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)Dokumentation zum Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization` und `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Benutzerkennung enthält, bevor sie eine Anfrage stellt.
   >
   > <br/>
   > 
   > Weitere Informationen zu `AD-Service-Token`-Kopfzeile finden Sie in der Dokumentation [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)&quot;.

1. **Single-Sign-On-Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Single-Sign-On-Profil basierend auf den empfangenen Parametern und Kopfzeilen.

1. **MVPD-Entscheidung für angeforderte Ressource abrufen:** Der Adobe Pass-Server ruft den MVPD-Autorisierungsendpunkt auf, um eine `Permit`- oder `Deny`-Entscheidung für die bestimmte Ressource abzurufen, die von der Streaming-Anwendung empfangen wurde.

1. **Rückgabe `Permit` Entscheidung mit Medien-Token:** Die Endpunktantwort „Entscheidungen autorisieren“ enthält eine `Permit` Entscheidung und ein Medien-Token.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)Dokumentation zum Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd.
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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **Stream mit Medien-Token starten:** Die zweite Streaming-Anwendung verwendet das Medien-Token, um den Inhalt abzuspielen.

1. **Rückgabe `Deny` Entscheidung mit Details:** Die Endpunktantwort „Decisions Authorize“ enthält eine `Deny` Entscheidung und eine Fehler-Payload, die der Dokumentation zu [Enhanced Error Codes](../../../../features-standard/error-reporting/enhanced-error-codes.md) entsprechen.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)Dokumentation zum Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd.
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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **Handhabung `Deny` Entscheidungsdetails:** Die zweite Streaming-Anwendung verarbeitet die Fehlerinformationen aus der Antwort und kann sie verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.

>[!NOTE]
>
> Die Schritte für den Vorautorisierungsfluss sind mit denen für den Autorisierungsfluss identisch, mit dem Unterschied, dass der verwendete Endpunkt derjenige ist, der in der Dokumentation [Abrufen von Vorautorisierungsentscheidungen mithilfe spezifischer mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) beschrieben ist.
