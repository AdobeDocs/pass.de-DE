---
title: Single Sign-On - Platform Identity - Flows
description: REST API V2 - Single Sign-On - Platform Identity - Flows
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '1830'
ht-degree: 0%

---


# Single Sign-on mit Platform-Identitätsflüssen {#single-sign-on-platform-identity-full-flows}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

Die Platform Identity-Methode ermöglicht es mehreren Anwendungen, eine eindeutige Plattformkennung zu verwenden, um Single Sign-On (SSO) auf Geräte- oder Plattformebene bei der Verwendung von Adobe Pass-Diensten zu erzielen.

Die Anwendungen sind für das Abrufen der Payload der eindeutigen Plattformkennung verantwortlich, die gerätespezifische Identitätsdienste oder Bibliotheken außerhalb von Adobe Pass-Systemen verwendet.

Die Anwendungen sind dafür verantwortlich, diese eindeutige Plattform-ID-Payload als Teil der `Adobe-Subject-Token` -Kopfzeile für alle Anforderungen einzuschließen, die sie angeben.

Weitere Informationen zum Header `Adobe-Subject-Token` finden Sie in der Dokumentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .

## Authentifizierung über Single Sign-on mithilfe der Plattformidentität durchführen {#perform-authentication-through-single-sign-on-using-platform-identity}

### Voraussetzungen {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

Bevor Sie den Authentifizierungsfluss durch Single Sign-on mit einer Plattformidentität durchführen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Plattform muss einen Identitätsdienst oder eine Bibliothek bereitstellen, der bzw. die für alle Anwendungen auf demselben Gerät oder derselben Plattform konsistente Informationen als `JWS` - oder `JWE` -Payload zurückgibt.
* Die erste Streaming-Anwendung muss die eindeutige Plattformkennung abrufen und die `JWS` - oder `JWE` -Payload als Teil des Headers [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) für alle Anforderungen einschließen, die sie angeben.
* Die erste Streaming-Anwendung muss einen MVPD auswählen.
* Die erste Streaming-Anwendung muss eine Authentifizierungssitzung starten, um sich mit dem ausgewählten MVPD anzumelden.
* Die erste Streaming-Anwendung muss sich mit dem ausgewählten MVPD in einem Benutzeragenten authentifizieren.
* Die zweite Streaming-Anwendung muss die eindeutige Plattformkennung abrufen und die `JWS` - oder `JWE` -Payload als Teil des Headers [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) für alle Anforderungen einschließen, die sie angeben.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die erste Streaming-Anwendung unterstützt Benutzerinteraktionen zur Auswahl eines MVPD.
> * Die erste Streaming-Anwendung unterstützt Benutzerinteraktionen, um sich mit dem ausgewählten MVPD in einem Benutzeragenten zu authentifizieren.

### Workflow {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

Führen Sie die angegebenen Schritte aus, um den Authentifizierungsfluss durch Single Sign-on mithilfe einer Plattformidentität zu implementieren, wie im folgenden Diagramm dargestellt.

![Führen Sie die Authentifizierung über Single Sign-on mithilfe der Plattformidentität durch](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*Führen Sie die Authentifizierung über Single Sign-on mithilfe der Plattformidentität durch*

1. **Plattformkennung abrufen:** Die erste Streaming-Anwendung ruft den Identitätsdienst oder die Bibliothek außerhalb der Adobe Pass-Systeme auf, um die mit der eindeutigen Plattformkennung verknüpfte `JWS` - oder `JWE` -Payload abzurufen.

1. **Rückgabe der eindeutigen Plattformkennung als JWS oder JWE:** Die erste Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Sicherheitsbedingungen erfüllt sind:
   * Die Nutzlast ist nicht abgelaufen.
   * Die Nutzlast wird signiert oder verschlüsselt.

1. **Erstellen einer Authentifizierungssitzung:** Die erste Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Authentifizierungssitzung durch Aufruf des Sitzungsendpunkts zu initiieren.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Erstellen einer Authentifizierungssitzung](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) .
   > 
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd`, `domainName` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `Adobe-Subject-Token` finden Sie in der Dokumentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .

1. **Geben Sie die nächste Aktion an:** Die Sitzungsendpunktantwort enthält die erforderlichen Daten, um die erste Streaming-Anwendung in Bezug auf die nächste Aktion zu leiten.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur API zur [Erstellen einer Authentifizierungssitzung](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) .
   > 
   > <br/>
   > 
   > Der Sitzungsendpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **URL im Benutzeragenten öffnen:** Die Sitzungsendpunktantwort enthält die folgenden Daten:
   * Der `url` , der verwendet werden kann, um die interaktive Authentifizierung innerhalb der MVPD-Anmeldeseite zu initiieren.
   * Das Attribut `actionName` ist auf &quot;Authentifizieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;interaktiv&quot;festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil angibt, öffnet die erste Streaming-Anwendung einen Benutzeragenten, der den bereitgestellten `url` lädt, und sendet eine Anfrage an den Authenticate-Endpunkt. Dieser Ablauf kann mehrere Umleitungen umfassen, die den Benutzer letztendlich zur MVPD-Anmeldeseite führen und gültige Anmeldeinformationen angeben.

1. **Vollständige MVPD-Authentifizierung:** Wenn der Authentifizierungsfluss erfolgreich ist, speichert die Benutzeragenten-Interaktion ein reguläres Profil im Adobe Pass-Backend und erreicht die bereitgestellte `redirectUrl`.

1. **Profil für bestimmten Code abrufen:** Die erste Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur [Abrufen des Profils für bestimmte Code-API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) .
   > 
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `code`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

   >[!NOTE]
   >
   > Empfehlung: Die Streaming-Anwendung kann warten, bis der Benutzeragent den bereitgestellten &quot;`redirectUrl`&quot;erreicht, um zu überprüfen, ob das reguläre Profil erfolgreich generiert und gespeichert wurde.

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Profil.

1. **Informationen zum regulären Profil zurückgeben:** Die Profil-Endpunktantwort enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen, die in einer Profilantwort bereitgestellt werden, finden Sie in der Dokumentation zur API für spezifischen Code-Code-Abruf-Profil](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) .[
   > 
   > <br/>
   > 
   > Der Endpunkt Profile validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Fahren Sie mit den Entscheidungsflüssen fort:** Die erste Streaming-Anwendung kann mit nachfolgenden Entscheidungsflüssen fortfahren.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `Adobe-Subject-Token` finden Sie in der Dokumentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .

1. **Plattformkennung abrufen:** Die zweite Streaming-Anwendung ruft den Identitätsdienst oder die Bibliothek außerhalb der Adobe Pass-Systeme auf, um die mit der eindeutigen Plattformkennung verknüpfte `JWS` - oder `JWE` -Payload abzurufen.

1. **Rückgabe der eindeutigen Plattformkennung als JWS oder JWE:** Die zweite Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Sicherheitsbedingungen erfüllt sind:
   * Die Nutzlast ist nicht abgelaufen.
   * Die Nutzlast wird signiert oder verschlüsselt.

1. **Profile abrufen:** Die zweite Streaming-Anwendung erfasst alle erforderlichen Daten, um alle Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Abrufen von Profilen](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) :
   > 
   > * Alle _erforderlichen_ Parameter, z. B. `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `Adobe-Subject-Token` finden Sie in der Dokumentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .

1. **Single Sign-On-Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Single-Sign-On-Profil.

1. **Rückgabe von Informationen zum Single Sign-On-Profil:** Die Profil-Endpunktantwort enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen, die in einer Profilantwort bereitgestellt werden, finden Sie in der Dokumentation zur API zum [Abrufen von Profilen](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) .
   >
   > <br/>
   > 
   > Der Endpunkt Profile validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Fahren Sie mit den Entscheidungsflüssen fort:** Die zweite Streaming-Anwendung kann mit nachfolgenden Entscheidungsflüssen fortfahren.

   >[!IMPORTANT]
   >
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `Adobe-Subject-Token` finden Sie in der Dokumentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .

## Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe der Plattformidentität{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### Voraussetzungen {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Bevor Sie den Autorisierungsfluss durch Single Sign-on mit einer Plattformidentität durchführen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Plattform muss einen Identitätsdienst oder eine Bibliothek bereitstellen, der bzw. die für alle Anwendungen auf demselben Gerät oder derselben Plattform konsistente Informationen als `JWS` - oder `JWE` -Payload zurückgibt.
* Die zweite Streaming-Anwendung muss die eindeutige Plattformkennung abrufen und die `JWS` - oder `JWE` -Payload als Teil des Headers [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) für alle Anforderungen einschließen, die sie angeben.
* Die zweite Streaming-Anwendung muss eine Autorisierungsentscheidung abrufen, bevor eine vom Benutzer ausgewählte Ressource wiedergegeben wird.

>[!IMPORTANT]
>
> Annahmen
> 
> <br/>
> 
> * Die erste Streaming-Anwendung hat eine Authentifizierung durchgeführt und einen gültigen Wert für den Anforderungsheader [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) enthalten.

### Workflow {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Führen Sie die angegebenen Schritte aus, um den Autorisierungsfluss durch Single Sign-on mithilfe einer Plattformidentität zu implementieren, wie in der folgenden Abbildung dargestellt.

![Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe der Plattformidentität](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*Abrufen von Autorisierungsentscheidungen über Single Sign-on mithilfe der Plattformidentität*

1. **Plattformkennung abrufen:** Die zweite Streaming-Anwendung ruft den Identitätsdienst oder die Bibliothek außerhalb der Adobe Pass-Systeme auf, um die mit der eindeutigen Plattformkennung verknüpfte `JWS` - oder `JWE` -Payload abzurufen.

1. **Rückgabe der eindeutigen Plattformkennung als JWS oder JWE:** Die zweite Streaming-Anwendung validiert die Antwortdaten, um sicherzustellen, dass die grundlegenden Sicherheitsbedingungen erfüllt sind:
   * Die Nutzlast ist nicht abgelaufen.
   * Die Nutzlast wird signiert oder verschlüsselt.

1. **Autorisierungsentscheidung abrufen:** Die zweite Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Endpunkt Entscheidungsautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API:
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `Adobe-Subject-Token` finden Sie in der Dokumentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .

1. **Single Sign-On-Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Single-Sign-On-Profil.

1. **MVPD-Entscheidung für angeforderte Ressource abrufen:** Der Adobe Pass-Server ruft den MVPD-Autorisierungsendpunkt auf, um eine `Permit` - oder `Deny` -Entscheidung für die spezifische Ressource zu erhalten, die von der Streaming-Anwendung empfangen wurde.

1. **Rückgabe `Permit` -Entscheidung mit Medien-Token:** Die Antwort des Endpunkts Entscheidungsautorisierung enthält eine `Permit` -Entscheidung und ein Medien-Token.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API .
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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Stream mit Medien-Token starten:** Die zweite Streaming-Anwendung verwendet das Medien-Token, um den Inhalt wiederzugeben.

1. **Rückgabe `Deny` -Entscheidung mit Details:** Die Antwort des Endpunkts Entscheidungsautorisierung enthält eine `Deny` -Entscheidung und eine Fehler-Payload, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API .
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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Entscheidungsdetails verarbeiten `Deny`:** Die zweite Streaming-Anwendung verarbeitet die Fehlerinformationen aus der Antwort und kann sie verwenden, um optional eine bestimmte Meldung auf der Benutzeroberfläche anzuzeigen.

>[!NOTE]
>
> Die Schritte für den Vorautorisierungsfluss sind mit denen für den Autorisierungsfluss identisch, mit dem Unterschied, dass der verwendete Endpunkt derjenige ist, der in der Dokumentation zum [Abrufen von Vorautorisierungsentscheidungen mithilfe bestimmter mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) beschrieben wird.
