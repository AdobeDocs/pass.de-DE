---
title: Einfache Authentifizierung - Sekundäre Anwendung - Fluss
description: REST API v2 - Standardauthentifizierung - Sekundäre Anwendung - Fluss
exl-id: 83bf592e-c679-4cfe-984d-710a9598c620
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '2006'
ht-degree: 0%

---

# Grundlegender Authentifizierungsfluss innerhalb der sekundären Anwendung {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

>[!MORELIKETHIS]
>
> Stellen Sie sicher, dass Sie auch die häufig gestellten Fragen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) besuchen.

Mit dem **Authentifizierungsfluss** innerhalb der Adobe Pass-Authentifizierungsberechtigung kann die Streaming-Anwendung überprüfen, ob ein Benutzer über ein gültiges MVPD-Konto verfügt. Für diesen Vorgang muss der Benutzer über ein gültiges MVPD-Konto verfügen und gültige Anmeldeinformationen auf der MVPD-Anmeldeseite eingeben.

Der Authentifizierungsfluss ist in den folgenden Fällen erforderlich:

* Wenn der/die Benutzende eine Anwendung zum ersten Mal öffnet.
* Wenn die vorherige Authentifizierung des Benutzers abgelaufen ist.
* Wenn sich der Benutzer beim MVPD-Konto abmeldet.
* Wenn sich der/die Benutzende mit einer anderen MVPD authentifizieren möchte.

In all diesen Fällen erhält die Anwendung, die einen der Profile-Endpunkte aufruft, eine leere Antwort oder ein oder mehrere Profile, jedoch für verschiedene MVPDs.

Der **Authentifizierungsfluss** erfordert, dass ein Benutzeragent (Browser) eine Reihe von Aufrufen vom Programm an das Adobe Pass-Backend, dann an die MVPD-Anmeldeseite und schließlich zurück an die Anwendung ausführt. Dieser Fluss kann mehrere Umleitungen zu MVPD-Systemen und die Verwaltung von Cookies oder Sitzungen für jede Domain umfassen, was ohne Benutzeragenten schwierig zu erreichen und zu schützen ist.

Basierend auf den Funktionen der Primäranwendung (Streaming-Anwendung) zur Unterstützung der Benutzerinteraktion zur Auswahl einer MVPD und zur Authentifizierung mit der ausgewählten MVPD in einem Benutzeragenten lauten die Authentifizierungsszenarien wie folgt:

* [Authentifizierung innerhalb der primären Anwendung durchführen](rest-api-v2-basic-authentication-primary-application-flow.md)
* [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### Voraussetzungen {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

Stellen Sie vor dem Starten des Authentifizierungsflusses in einer primären Anwendung und seiner Fertigstellung durch Benutzerinteraktion in einer sekundären Anwendung sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss eine MVPD auswählen.
* Die Streaming-Anwendung muss eine Authentifizierungssitzung starten, um sich mit der ausgewählten MVPD anzumelden.
* Die sekundäre Anwendung muss sich mit der ausgewählten MVPD in einem Benutzeragenten authentifizieren.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die Streaming-Anwendung unterstützt Benutzerinteraktionen bei der Auswahl einer MVPD.
> * Die sekundäre Anwendung (normalerweise auf einem sekundären Gerät) unterstützt die Benutzerinteraktion zur Authentifizierung bei der ausgewählten MVPD in einem Benutzeragenten.

### Workflow {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Authentifizierungsfluss zu implementieren, der in einer sekundären Anwendung mit einer vorab ausgewählten MVPD ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen*

1. **Authentifizierungssitzung erstellen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Authentifizierungssitzung zu initiieren, indem der Sessions-Endpunkt aufgerufen wird.

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
   > Die Streaming-Anwendung muss beim Erstellen der Authentifizierungssitzung in einem einzigen Aufruf alle erforderlichen Parameter bereitstellen.

1. **Nächste Aktion angeben:** Die Antwort des Sitzungs-Endpunkts enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu führen.

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

1. **Fahren Sie mit Entscheidungs-Flüssen fort**: Die Antwort des Sitzungs-Endpunkts enthält die folgenden Daten:
   * Das `actionName`-Attribut ist auf „authorize“ festgelegt.
   * Das `actionType`-Attribut ist auf „direct“ festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

1. **Authentifizierungs-Code anzeigen** Die Antwort des Sitzungs-Endpunkts enthält die folgenden Daten:
   * Die `code`, mit der die Authentifizierungssitzung in einer sekundären Anwendung fortgesetzt werden kann.
   * Das `actionName`-Attribut ist auf „Authenticate“ festgelegt.
   * Das `actionType`-Attribut ist auf „interaktiv“ festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert, zeigt die Streaming-Anwendung die `code` an, die verwendet werden können, um die Authentifizierungssitzung in einer sekundären Anwendung fortzusetzen.

1. **Authentifizierungs-Code validieren** Die sekundäre Anwendung validiert den bereitgestellten `code`, um sicherzustellen, dass sie mit der MVPD-Authentifizierung im Benutzeragenten fortfahren kann.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)Dokumentation zu Authentifizierungssitzungen abrufen:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider` und `code`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Rückgabeinformationen zur Authentifizierungssitzung:** Die Antwort des Sitzungs-Endpunkts enthält die folgenden Daten:
   * Das `existing`-Attribut enthält die vorhandenen Parameter, die bereits angegeben wurden.
   * Das Attribut `missing` enthält die fehlenden Parameter, die angegeben werden müssen, um den Authentifizierungsfluss abzuschließen.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Antwort zur Sitzungsvalidierung ](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) Informationen finden Sie in der API-Dokumentation zu Authentifizierungssitzungsinformationen abrufen .
   >
   > <br/>
   >
   > Der Sessions-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

   >[!TIP]
   >
   > Die sekundäre Anwendung kann Benutzer im Fall einer Fehlerantwort, die auf eine fehlende Authentifizierungssitzung hinweist, darüber informieren, dass die verwendete `code` ungültig ist, und ihnen empfehlen, eine neue Authentifizierungssitzung erneut zu verwenden.

1. **URL im Benutzeragenten öffnen:** Die sekundäre Anwendung öffnet einen Benutzeragenten, um den selbst berechneten `url` zu laden, und sendet eine Anfrage an den Authentifizierungsendpunkt. Dieser Fluss kann mehrere Weiterleitungen enthalten, die den Benutzer letztendlich zur Anmeldeseite von MVPD führen und gültige Anmeldeinformationen angeben.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)Dokumentation zu „Authentifizierung im Benutzeragenten durchführen“:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider` und `code`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Vollständige MVPD-Authentifizierung:** Wenn der Authentifizierungsfluss erfolgreich ist, speichert die Benutzeragenten-Interaktion ein reguläres Profil im Adobe Pass-Backend und erreicht die angegebene `redirectUrl`.

1. **Profil für bestimmten Code abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt „Profiles“ gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)Dokumentation zum Abrufen von Profilen für bestimmten Code:
   > 
   > * Alle _erforderlichen_ Parameter wie `serviceProvider` und `code`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

   >[!TIP]
   >
   > Die Streaming-Anwendung muss einen Abrufmechanismus implementieren, der die `code` verwendet, um zu überprüfen, ob das reguläre Profil erfolgreich generiert und gespeichert wurde.

1. **Informationen zum regulären Profil zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum regulären Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

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

## Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### Voraussetzungen {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

Stellen Sie vor dem Starten des Authentifizierungsflusses in einer primären Anwendung und seiner Fertigstellung durch Benutzerinteraktion in einer sekundären Anwendung sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss eine Authentifizierungssitzung starten, wenn sie sich anmelden muss.
* Die sekundäre Anwendung muss eine MVPD auswählen.
* Die sekundäre Anwendung muss sich mit der ausgewählten MVPD in einem Benutzeragenten authentifizieren.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die sekundäre Anwendung (normalerweise auf einem sekundären Gerät) unterstützt die Benutzerinteraktion bei der Auswahl einer MVPD.
> * Die sekundäre Anwendung (normalerweise auf einem sekundären Gerät) unterstützt die Benutzerinteraktion zur Authentifizierung bei der ausgewählten MVPD in einem Benutzeragenten.

### Workflow {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Authentifizierungsfluss in einer sekundären Anwendung ohne vorab ausgewählte MVPD zu implementieren, wie in der folgenden Abbildung dargestellt.

![Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen*

1. **Authentifizierungssitzung erstellen:** Die Streaming-Anwendung erfasst einige der erforderlichen Daten, um eine Authentifizierungssitzung durch Aufruf des Sitzungs-Endpunkts zu initiieren.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)Dokumentation zu „Authentifizierungssitzung erstellen“:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   > 
   > Die Streaming-Anwendung kann beim Erstellen der Authentifizierungssitzung nicht alle erforderlichen Parameter in einem einzigen Aufruf bereitstellen.

1. **Nächste Aktion angeben:** Die Antwort des Sitzungs-Endpunkts enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu leiten:
   * Die `code`, mit der die Authentifizierungssitzung in einer sekundären Anwendung fortgesetzt werden kann.
   * Das Attribut `actionName` ist auf „resume“ festgelegt.
   * Das `actionType`-Attribut ist auf „direct“ festgelegt.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)Dokumentation zu „Authentifizierungssitzung erstellen“.
   > 
   > <br/>
   > 
   > Der Sessions-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **Authentifizierungs-Code anzeigen** Die Streaming-Anwendung zeigt die `code` an, mit der die Authentifizierungssitzung in einer sekundären Anwendung fortgesetzt werden kann.

1. **Fehlende Parameter für Authentifizierungssitzung angeben:** Die sekundäre Anwendung erfasst alle fehlenden Daten, die erforderlich sind, um die Authentifizierungssitzung fortzusetzen, und ruft den Sitzungs-Endpunkt auf.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)Dokumentation zur Wiederaufnahme der Authentifizierungssitzung:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd`, `domainName` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Nächste Aktion angeben:** Die Antwort des Sitzungs-Endpunkts enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu führen.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)Dokumentation zu „Authentifizierungssitzung fortsetzen“.
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

   >[!TIP]
   >
   > Die sekundäre Anwendung kann Benutzer im Fall einer Fehlerantwort, die auf eine fehlende Authentifizierungssitzung hinweist, darüber informieren, dass die verwendete `code` ungültig ist, und ihnen empfehlen, eine neue Authentifizierungssitzung erneut zu verwenden.

1. **Vorhandenes Profil angeben:** Die Antwort des Sitzungs-Endpunkts enthält die folgenden Daten:
   * Das `actionName`-Attribut ist auf „authorize“ festgelegt.
   * Das `actionType`-Attribut ist auf „direct“ festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

1. **URL im Benutzeragenten öffnen** Die Antwort des Sitzungs-Endpunkts enthält die folgenden Daten:
   * Die `url`, die verwendet werden kann, um die interaktive Authentifizierung auf der Anmeldeseite von MVPD zu initiieren.
   * Das `actionName`-Attribut ist auf „Authenticate“ festgelegt.
   * Das `actionType`-Attribut ist auf „interaktiv“ festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert, öffnet die sekundäre Anwendung einen Benutzeragenten, um die angegebene `url` zu laden, und sendet eine Anfrage an den Authentifizierungsendpunkt. Dieser Fluss kann mehrere Weiterleitungen enthalten, die den Benutzer letztendlich zur Anmeldeseite von MVPD führen und gültige Anmeldeinformationen angeben.

1. **Vollständige MVPD-Authentifizierung:** Wenn der Authentifizierungsfluss erfolgreich ist, speichert die Benutzeragenten-Interaktion ein reguläres Profil im Adobe Pass-Backend und erreicht die angegebene `redirectUrl`.

1. **Profil für bestimmten Code abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt „Profiles“ gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [ finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)Dokumentation zum Abrufen von Profilen für bestimmten Code:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider` und `code`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

   >[!TIP]
   >
   > Die Streaming-Anwendung muss einen Abrufmechanismus implementieren, der die `code` verwendet, um zu überprüfen, ob das reguläre Profil erfolgreich generiert und gespeichert wurde.

1. **Informationen zum regulären Profil zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum regulären Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

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
