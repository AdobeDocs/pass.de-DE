---
title: Grundlegende Authentifizierung - Sekundäre Anwendung - Fluss
description: Grundlegende Authentifizierung - REST API V2 - Sekundäre Anwendung - Fluss
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '1756'
ht-degree: 0%

---


# Grundlegender Authentifizierungsfluss, der in der sekundären Anwendung ausgeführt wird {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

Mit dem **Authentifizierungsfluss** innerhalb der Adobe Pass-Authentifizierungsberechtigungen kann die Streaming-Anwendung überprüfen, ob ein Benutzer über ein gültiges MVPD-Konto verfügt. Für diesen Prozess muss der Benutzer über ein aktives MVPD-Konto verfügen und gültige Anmeldedaten auf der MVPD-Anmeldeseite eingeben.

In den folgenden Fällen ist ein Authentifizierungsfluss erforderlich:

* Wenn der Benutzer eine Anwendung zum ersten Mal öffnet.
* Wenn die vorherige Authentifizierung des Benutzers abgelaufen ist.
* Wenn sich der Benutzer vom MVPD-Konto abmeldet.
* Wenn der Benutzer sich mit einem anderen MVPD authentifizieren möchte.

In allen diesen Fällen erhält die Anwendung, die einen der Profile-Endpunkte aufruft, eine leere Antwort oder ein oder mehrere Profile, jedoch für verschiedene MVPDs.

Für den **Authentifizierungsfluss** muss ein Benutzeragent (Browser) eine Reihe von Aufrufen von der Anwendung zum Adobe Pass-Backend, dann zur MVPD-Anmeldeseite und schließlich zurück zur Anwendung durchführen. Dieser Ablauf kann mehrere Umleitungen zu MVPD-Systemen und die Verwaltung von Cookies oder Sitzungen umfassen, die für jede Domäne gespeichert sind. Dies kann ohne einen Benutzeragenten schwierig zu erreichen und zu sichern sein.

Basierend auf den Funktionen der primären Anwendung (Streaming-Anwendung), die Benutzerinteraktionen zur Auswahl eines MVPD und zur Authentifizierung mit dem ausgewählten MVPD in einem Benutzeragenten unterstützen, sind die Authentifizierungsszenarien:

* [Authentifizierung innerhalb der primären Anwendung durchführen](./rest-api-v2-basic-authentication-primary-application-flow.md)
* [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewählter mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewählter mvpd {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### Voraussetzungen {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

Bevor Sie den Authentifizierungsfluss in einer primären Anwendung starten und durch Benutzerinteraktionen in einer sekundären Anwendung abschließen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss einen MVPD auswählen.
* Die Streaming-Anwendung muss eine Authentifizierungssitzung starten, um sich mit dem ausgewählten MVPD anzumelden.
* Die sekundäre Anwendung muss sich mit dem ausgewählten MVPD in einem Benutzeragenten authentifizieren.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die Streaming-Anwendung unterstützt Benutzerinteraktionen zur Auswahl eines MVPD.
> * Die sekundäre Anwendung (normalerweise auf einem sekundären Gerät) unterstützt Benutzerinteraktionen, um sich bei dem ausgewählten MVPD in einem Benutzeragenten zu authentifizieren.

### Workflow {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Authentifizierungsfluss zu implementieren, der in einer sekundären Anwendung mit einem vorab ausgewählten MVPD durchgeführt wird, wie im folgenden Diagramm dargestellt.

![Führen Sie die Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durch](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*Führen Sie die Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durch*

1. **Erstellen einer Authentifizierungssitzung:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Authentifizierungssitzung durch Aufruf des Sitzungsendpunkts zu starten.

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
   > Die Streaming-Anwendung muss beim Erstellen der Authentifizierungssitzung alle erforderlichen Parameter in einem einzigen Aufruf bereitstellen.

1. **Geben Sie die nächste Aktion an:** Die Sitzungsendpunktantwort enthält die erforderlichen Daten, um die Streaming-Anwendung zur nächsten Aktion zu leiten.

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

1. **Fahren Sie mit den Entscheidungsflüssen fort:** Die Sitzungsendpunktantwort enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;Autorisieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;direct&quot;festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

1. **Authentifizierungscode anzeigen:** Die Sitzungsendpunktantwort enthält die folgenden Daten:
   * Der `code` , der verwendet werden kann, um die Authentifizierungssitzung in einer sekundären Anwendung wiederaufzunehmen.
   * Das Attribut `actionName` ist auf &quot;Authentifizieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;interaktiv&quot;festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil angibt, zeigt die Streaming-Anwendung die `code` an, die zur Wiederaufnahme der Authentifizierungssitzung in einer sekundären Anwendung verwendet werden kann.

1. **URL im Benutzeragenten öffnen:** Die sekundäre Anwendung öffnet einen Benutzeragenten, um den selbst berechneten `url` zu laden. Dadurch wird eine Anfrage an den Endpunkt Authentifizieren gesendet. Dieser Ablauf kann mehrere Umleitungen umfassen, die den Benutzer letztendlich zur MVPD-Anmeldeseite führen und gültige Anmeldeinformationen angeben.

1. **Vollständige MVPD-Authentifizierung:** Wenn der Authentifizierungsfluss erfolgreich ist, speichert die Benutzeragenten-Interaktion ein reguläres Profil im Adobe Pass-Backend und erreicht die bereitgestellte `redirectUrl`.

1. **Profil für bestimmten Code abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur [Abrufen des Profils für bestimmte Code-API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) .
   > 
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `code`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

   >[!NOTE]
   >
   > Empfehlung: Die Streaming-Anwendung kann mithilfe von `code` einen Abruffegel implementieren, um zu überprüfen, ob das reguläre Profil erfolgreich generiert und gespeichert wurde.

1. **Informationen zum regulären Profil zurückgeben:** Die Profil-Endpunktantwort enthält Informationen zum regulären Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

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

## Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### Voraussetzungen {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

Bevor Sie den Authentifizierungsfluss in einer primären Anwendung starten und durch Benutzerinteraktionen in einer sekundären Anwendung abschließen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss eine Authentifizierungssitzung starten, wenn sie sich anmelden muss.
* Die sekundäre Anwendung muss einen MVPD auswählen.
* Die sekundäre Anwendung muss sich mit dem ausgewählten MVPD in einem Benutzeragenten authentifizieren.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die sekundäre Anwendung (normalerweise auf einem sekundären Gerät) unterstützt Benutzerinteraktionen zur Auswahl eines MVPD.
> * Die sekundäre Anwendung (normalerweise auf einem sekundären Gerät) unterstützt Benutzerinteraktionen, um sich bei dem ausgewählten MVPD in einem Benutzeragenten zu authentifizieren.

### Workflow {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Authentifizierungsfluss zu implementieren, der in einer sekundären Anwendung ohne vorab ausgewählten MVPD durchgeführt wird, wie im folgenden Diagramm dargestellt.

![Führen Sie die Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durch](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*Führen Sie die Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durch*

1. **Erstellen einer Authentifizierungssitzung:** Die Streaming-Anwendung erfasst einige der erforderlichen Daten, um eine Authentifizierungssitzung durch Aufruf des Sitzungsendpunkts zu initiieren.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Erstellen einer Authentifizierungssitzung](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) .
   >
   > * Alle _erforderlichen_ Parameter, z. B. `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   > 
   > Die Streaming-Anwendung kann beim Erstellen der Authentifizierungssitzung nicht alle erforderlichen Parameter in einem einzelnen Aufruf bereitstellen.

1. **Geben Sie die nächste Aktion an:** Die Sitzungsendpunktantwort enthält die erforderlichen Daten, um die Streaming-Anwendung in Bezug auf die nächste Aktion zu leiten:
   * Der `code` , der verwendet werden kann, um die Authentifizierungssitzung in einer sekundären Anwendung wiederaufzunehmen.
   * Das Attribut `actionName` ist auf &quot;resume&quot;(Fortsetzen) festgelegt.
   * Das Attribut `actionType` ist auf &quot;direct&quot;festgelegt.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur API zur [Erstellen einer Authentifizierungssitzung](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) .
   > 
   > <br/>
   > 
   > Der Sitzungsendpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Authentifizierungscode anzeigen:** Die Streaming-Anwendung zeigt die `code` an, die verwendet werden kann, um die Authentifizierungssitzung in einer sekundären Anwendung wiederaufzunehmen.

1. **Stellen Sie die fehlenden Parameter für die Authentifizierungssitzung bereit:** Die sekundäre Anwendung erfasst alle fehlenden Daten, die zur Wiederaufnahme der Authentifizierungssitzung erforderlich sind, und ruft den Sitzungsendpunkt auf.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zur [Fortsetzen der Authentifizierungssitzung](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd`, `domainName` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Geben Sie die nächste Aktion an:** Die Sitzungsendpunktantwort enthält die erforderlichen Daten, um die Streaming-Anwendung zur nächsten Aktion zu leiten.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Sitzungsantwort bereitgestellten Informationen finden Sie in der API-Dokumentation zur [Wiederaufnahme der Authentifizierungssitzung ](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) .
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

1. **Vorhandenes Profil angeben:** Die Sitzungsendpunktantwort enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;Autorisieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;direct&quot;festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

1. **URL im Benutzeragenten öffnen:** Die Sitzungsendpunktantwort enthält die folgenden Daten:
   * Der `url` , der verwendet werden kann, um die interaktive Authentifizierung innerhalb der MVPD-Anmeldeseite zu initiieren.
   * Das Attribut `actionName` ist auf &quot;Authentifizieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;interaktiv&quot;festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert, öffnet die sekundäre Anwendung einen Benutzeragenten, um den bereitgestellten `url` zu laden. Dadurch wird eine Anfrage an den Endpunkt Authentifizieren gesendet. Dieser Ablauf kann mehrere Umleitungen umfassen, die den Benutzer letztendlich zur MVPD-Anmeldeseite führen und gültige Anmeldeinformationen angeben.

1. **Vollständige MVPD-Authentifizierung:** Wenn der Authentifizierungsfluss erfolgreich ist, speichert die Benutzeragenten-Interaktion ein reguläres Profil im Adobe Pass-Backend und erreicht die bereitgestellte `redirectUrl`.

1. **Profil für bestimmten Code abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur [Abrufen des Profils für bestimmte Code-API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `code`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

   >[!NOTE]
   >
   > Empfehlung: Die Streaming-Anwendung kann mithilfe von `code` einen Abruffegel implementieren, um zu überprüfen, ob das reguläre Profil erfolgreich generiert und gespeichert wurde.

1. **Informationen zum regulären Profil zurückgeben:** Die Profil-Endpunktantwort enthält Informationen zum regulären Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

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
