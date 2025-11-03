---
title: Single Sign-On - Platform-Identität - Flüsse
description: REST API v2 - Single Sign-On - Platform-Identität - Flüsse
exl-id: 5200e851-84e8-4cb4-b068-63b91a2a8945
source-git-commit: 2afe9ea2a814817757f1ab28484a84466da68d62
workflow-type: tm+mt
source-wordcount: '1855'
ht-degree: 0%

---

# Single Sign-on mit Platform-Identitätsflüssen {#single-sign-on-platform-identity-full-flows}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

>[!MORELIKETHIS]
>
> Stellen Sie sicher, dass Sie auch die häufig gestellten Fragen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) besuchen.

Die Platform-Identitätsmethode ermöglicht es mehreren Anwendungen, eine eindeutige Plattformkennung zu verwenden, um Single Sign-on (SSO) auf Geräte- oder Plattformebene bei der Verwendung von Adobe Pass-Services zu erzielen.

Die Programme sind für das Abrufen der eindeutigen Plattformkennungs-Payload mithilfe von gerätespezifischen Identitäts-Services oder Bibliotheken außerhalb von Adobe Pass-Systemen verantwortlich.

Die Programme sind dafür verantwortlich, diese eindeutige Plattformkennungs-Payload als Teil der `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token`-Kopfzeile für alle Anfragen einzuschließen, die sie angeben.

Weitere Informationen zur Kopfzeile `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` finden Sie in der Dokumentation [Adobe-Subject-](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) .

>[!MORELIKETHIS]
> 
> * [Amazon SSO-Cookbook](/help/premium-workflow/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
> * [Roku SSO Cookbook](/help/premium-workflow/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)

## Durchführen der Authentifizierung über Single Sign-on unter Verwendung der Platform-Identität {#perform-authentication-through-single-sign-on-using-platform-identity}

### Voraussetzungen {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

Bevor Sie den Authentifizierungsfluss über Single Sign-on mit einer Platform-Identität durchführen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Plattform muss einen Identity Service oder eine Bibliothek bereitstellen, der bzw. die konsistente Informationen als `JWS` oder `JWE` Payload über alle Anwendungen hinweg auf demselben Gerät oder auf derselben Plattform zurückgibt.
* Die erste Streaming-Anwendung muss die eindeutige Plattformkennung abrufen und die `JWS`- oder `JWE`-Payload als Teil der Kopfzeile [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) für alle Anfragen einschließen, die sie angeben.
* Die erste Streaming-Anwendung muss eine MVPD auswählen.
* Die erste Streaming-Anwendung muss eine Authentifizierungssitzung initiieren, um sich mit der ausgewählten MVPD anzumelden.
* Die erste Streaming-Anwendung muss sich mit der ausgewählten MVPD in einem Benutzeragenten authentifizieren.
* Die zweite Streaming-Anwendung muss die eindeutige Plattformkennung abrufen und die `JWS`- oder `JWE`-Payload als Teil der Kopfzeile [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) für alle Anfragen einschließen, die sie angeben.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die erste Streaming-Anwendung unterstützt Benutzerinteraktionen bei der Auswahl einer MVPD.
> * Die erste Streaming-Anwendung unterstützt Benutzerinteraktionen zur Authentifizierung bei der ausgewählten MVPD in einem Benutzeragenten.

### Workflow {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

Führen Sie die angegebenen Schritte aus, um den Authentifizierungsfluss durch Single Sign-on mithilfe einer Platform-Identität zu implementieren, wie im folgenden Diagramm dargestellt.

![Authentifizierung über Single Sign-on unter Verwendung der Platform-Identität](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*Authentifizierung über Single Sign-on unter Verwendung der Platform-Identität*

1. **Plattformkennung abrufen:** Die erste Streaming-Anwendung ruft den Identity Service oder die Identity Library außerhalb von Adobe Pass-Systemen auf, um die `JWS` oder `JWE` Payload abzurufen, die mit der eindeutigen Plattformkennung verknüpft ist.

1. **Eindeutige Plattformkennung als JWS oder JWE zurückgeben:** Die erste Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass grundlegende Sicherheitsbedingungen erfüllt werden:
   * Payload ist nicht abgelaufen.
   * Payload ist signiert oder verschlüsselt.

1. **Authentifizierungssitzung erstellen:** Die erste Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Authentifizierungssitzung zu initiieren, indem der Sessions-Endpunkt aufgerufen wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [&#x200B; in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)Dokumentation zu „Authentifizierungssitzung erstellen“:
   > 
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd`, `domainName` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   > 
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zur Kopfzeile `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` finden Sie in der Dokumentation [Adobe-Subject-](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) .

1. **Nächste Aktion angeben:** Die Antwort des Sitzungs-Endpunkts enthält die erforderlichen Daten, um die erste Streaming-Anwendung bezüglich der nächsten Aktion anzuleiten.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [&#x200B; in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)Dokumentation zu „Authentifizierungssitzung erstellen“.
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
   > Weitere Informationen zu folgenden [&#x200B; finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)Dokumentation zum Abrufen von Profilen für bestimmten Code:
   > 
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `code`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

   >[!TIP]
   >
   > Die Streaming-Anwendung muss warten, bis der Benutzeragent die angegebene `redirectUrl` erreicht, um zu überprüfen, ob das reguläre Profil erfolgreich generiert und gespeichert wurde.

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Profil anhand der empfangenen Parameter und Kopfzeilen.

1. **Informationen zum regulären Profil zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu [&#x200B; in einer Profilantwort bereitgestellten Informationen finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)Dokumentation zum Abrufen von Profilen für bestimmten Code.
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
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zur Kopfzeile `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` finden Sie in der Dokumentation [Adobe-Subject-](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) .

1. **Plattformkennung abrufen:** Die zweite Streaming-Anwendung ruft den Identity Service oder die Identity Library außerhalb von Adobe Pass-Systemen auf, um die `JWS` oder `JWE` Payload abzurufen, die mit der eindeutigen Plattformkennung verknüpft ist.

1. **Eindeutige Plattformkennung als JWS oder JWE zurückgeben:** Die zweite Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass grundlegende Sicherheitsbedingungen erfüllt werden:
   * Payload ist nicht abgelaufen.
   * Payload ist signiert oder verschlüsselt.

1. **Profile abrufen:** Die zweite Streaming-Anwendung sammelt alle erforderlichen Daten, um alle Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt „Profiles“ gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [&#x200B; in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)Dokumentation zum Abrufen von Profilen:
   > 
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   > 
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zur Kopfzeile `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` finden Sie in der Dokumentation [Adobe-Subject-](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) .

1. **Single-Sign-On-Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Single-Sign-On-Profil basierend auf den empfangenen Parametern und Kopfzeilen.

1. **Rückgabeinformationen zum Single-Sign-On-Profil zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [&#x200B; in einer Profilantwort bereitgestellten Informationen finden &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) in der API-Dokumentation zum Abrufen von Profilen .
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
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zur Kopfzeile `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` finden Sie in der Dokumentation [Adobe-Subject-](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) .

## Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe der Platform-Identität{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### Voraussetzungen {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Bevor Sie den Autorisierungsfluss über Single Sign-on mit einer Platform-Identität durchführen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Plattform muss einen Identity Service oder eine Bibliothek bereitstellen, der bzw. die konsistente Informationen als `JWS` oder `JWE` Payload über alle Anwendungen hinweg auf demselben Gerät oder auf derselben Plattform zurückgibt.
* Die zweite Streaming-Anwendung muss die eindeutige Plattformkennung abrufen und die `JWS`- oder `JWE`-Payload als Teil der Kopfzeile [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) für alle Anfragen einschließen, die sie angeben.
* Die zweite Streaming-Anwendung muss eine Autorisierungsentscheidung abrufen, bevor eine vom Benutzer ausgewählte Ressource wiedergegeben wird.

>[!IMPORTANT]
>
> Annahmen
> 
> <br/>
> 
> * Die erste Streaming-Anwendung hat eine Authentifizierung durchgeführt und einen gültigen Wert für die Anforderungskopfzeile [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)&rbrace; enthalten.

### Workflow {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Führen Sie die angegebenen Schritte aus, um den Autorisierungsfluss durch Single Sign-on mithilfe einer Platform-Identität zu implementieren, wie im folgenden Diagramm dargestellt.

![Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe der Platform-Identität](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe der Platform-Identität*

1. **Plattformkennung abrufen:** Die zweite Streaming-Anwendung ruft den Identity Service oder die Identity Library außerhalb von Adobe Pass-Systemen auf, um die `JWS` oder `JWE` Payload abzurufen, die mit der eindeutigen Plattformkennung verknüpft ist.

1. **Eindeutige Plattformkennung als JWS oder JWE zurückgeben:** Die zweite Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass grundlegende Sicherheitsbedingungen erfüllt werden:
   * Payload ist nicht abgelaufen.
   * Payload ist signiert oder verschlüsselt.

1. **Autorisierungsentscheidung abrufen:** Die zweite Streaming-Anwendung sammelt alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Decisions Authorize-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [&#x200B; in der API](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)Dokumentation zum Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization` und `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   > 
   > Bevor Sie eine Anfrage stellen, muss die Streaming-Anwendung sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zur Kopfzeile `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` finden Sie in der Dokumentation [Adobe-Subject-](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) .

1. **Single-Sign-On-Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Single-Sign-On-Profil basierend auf den empfangenen Parametern und Kopfzeilen.

1. **MVPD-Entscheidung für angeforderte Ressource abrufen:** Der Adobe Pass-Server ruft den MVPD-Autorisierungsendpunkt auf, um eine `Permit`- oder `Deny`-Entscheidung für die bestimmte Ressource abzurufen, die von der Streaming-Anwendung empfangen wurde.

1. **Rückgabe `Permit` Entscheidung mit Medien-Token:** Die Endpunktantwort „Entscheidungen autorisieren“ enthält eine `Permit` Entscheidung und ein Medien-Token.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [&#x200B; in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)Dokumentation zum Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd.
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
   > Weitere Informationen zu den [&#x200B; in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)Dokumentation zum Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd.
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
