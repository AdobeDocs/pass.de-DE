---
title: Grundlegende Authentifizierung - Primäre Anwendung - Fluss
description: REST API V2 - Grundlegende Authentifizierung - Primäre Anwendung - Fluss
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---


# Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung {#basic-authentication-flow-performed-within-primary-application}

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

## Authentifizierung innerhalb der primären Anwendung durchführen {#perform-authentication-within-primary-application}

### Voraussetzungen {#prerequisites-perform-authentication-within-primary-application}

Bevor Sie die Authentifizierung über Benutzerinteraktionen in einer primären Anwendung durchführen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss einen MVPD auswählen.
* Die Streaming-Anwendung muss eine Authentifizierungssitzung starten, um sich mit dem ausgewählten MVPD anzumelden.
* Die Streaming-Anwendung muss sich mit dem ausgewählten MVPD in einem Benutzeragenten authentifizieren.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die Streaming-Anwendung unterstützt Benutzerinteraktionen zur Auswahl eines MVPD.
> * Die Streaming-Anwendung unterstützt Benutzerinteraktionen, um sich mit dem ausgewählten MVPD in einem Benutzeragenten zu authentifizieren.

### Workflow {#workflow-perform-authentication-completed-on-primary-application}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Authentifizierungsfluss zu implementieren, der in einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Führen Sie eine Authentifizierung innerhalb der primären Anwendung durch](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-primary-application.png)

*Führen Sie eine Authentifizierung innerhalb der primären Anwendung durch*

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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Fahren Sie mit den Entscheidungsflüssen fort:** Die Sitzungsendpunktantwort enthält die folgenden Daten:
   * Das Attribut `actionName` ist auf &quot;Autorisieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;direct&quot;festgelegt.

   Wenn das Adobe Pass-Backend ein gültiges Profil identifiziert, muss sich die Streaming-Anwendung nicht erneut mit dem ausgewählten MVPD authentifizieren, da bereits ein Profil vorhanden ist, das für nachfolgende Entscheidungsflüsse verwendet werden kann.

1. **URL im Benutzeragenten öffnen:** Die Sitzungsendpunktantwort enthält die folgenden Daten:
   * Der `url` , der verwendet werden kann, um die interaktive Authentifizierung innerhalb der MVPD-Anmeldeseite zu initiieren.
   * Das Attribut `actionName` ist auf &quot;Authentifizieren&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;interaktiv&quot;festgelegt.

   Wenn das Adobe Pass-Backend kein gültiges Profil identifiziert, öffnet die Streaming-Anwendung einen Benutzeragenten, der den bereitgestellten `url` lädt, und sendet eine Anfrage an den Authenticate-Endpunkt. Dieser Ablauf kann mehrere Umleitungen umfassen, die den Benutzer letztendlich zur MVPD-Anmeldeseite führen und gültige Anmeldeinformationen angeben.

1. **Vollständige MVPD-Authentifizierung:** Wenn der Authentifizierungsfluss erfolgreich ist, speichert die Benutzeragenten-Interaktion ein reguläres Profil im Adobe Pass-Backend und erreicht die bereitgestellte `redirectUrl`.

1. **Profil für bestimmten Code abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

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
