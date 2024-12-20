---
title: Einfache Vorabautorisierung - Primäre Anwendung - Fluss
description: REST API v2 - Einfache Vorautorisierung - Primäre Anwendung - Fluss
exl-id: f557f6c3-d5b2-4ec8-be51-91a90fbd31c0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# Grundlegender Vorautorisierungsfluss innerhalb der primären Anwendung {#basic-preauthorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

Mit dem **Vorautorisierungsfluss** innerhalb der Berechtigung für die Adobe Pass-Authentifizierung kann die Streaming-Anwendung bestimmen, ob ein MVPD den Zugriff des Benutzers auf eine Liste von Ressourcen zulassen oder verweigern kann. Durch diese Überprüfung wird sichergestellt, dass die Anwendung dem Benutzer genaue Informationen über die Inhalte präsentieren kann, die er möglicherweise anzeigen kann.

## Abrufen von Entscheidungen vor der Autorisierung mithilfe bestimmter MVPD {#retrieve-preauthorization-decisions-using-specific-mvpd}

### Voraussetzungen {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

Bevor Sie Entscheidungen vor der Autorisierung mit einer bestimmten MVPD abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss über ein gültiges reguläres Profil verfügen, das mithilfe eines der grundlegenden Authentifizierungsflüsse erfolgreich für die MVPD erstellt wurde:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen](rest-api-v2-basic-authentication-secondary-application-flow.md)
* Die Streaming-Anwendung möchte Entscheidungen zur Vorabautorisierung abrufen, um eine Liste der Ressourcen zusammen mit ihren zugehörigen Status anzuzeigen.

### Workflow {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Vorautorisierungsfluss mit einem bestimmten MVPD zu implementieren, der in einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Abrufen von Entscheidungen vor Autorisierung mithilfe bestimmter MVPD](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*Abrufen von Entscheidungen vor Autorisierung mithilfe bestimmter MVPD*

1. **Vorabautorisierungsentscheidungen abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um Vorabautorisierungsentscheidungen für eine Liste von Ressourcen zu erhalten, indem sie den Endpunkt Decisions Preauthorize aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [ in der API](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)Dokumentation zum Abrufen von Vorabautorisierungsentscheidungen mithilfe bestimmter MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization` und `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Profil anhand der empfangenen Parameter und Kopfzeilen.

1. **Abrufen von MVPD-Entscheidungen für angeforderte Ressourcen:** Der Adobe Pass-Server ruft den Vorautorisierungsendpunkt von MVPD auf, um eine `Permit`- oder `Deny` für jede Ressource abzurufen, die von der Streaming-Anwendung empfangen wurde.

1. **Entscheidungen zur Vorautorisierung zurückgeben:** Die Antwort des Endpunkts „Decisions Preauthorize“ enthält für jede Ressource eine `Permit` oder `Deny` Entscheidung:
   * Eine `Permit` Entscheidung bedeutet, dass die Ressource abspielbar ist. Die Antwort enthält kein Medien-Token, da der Vorautorisierungsfluss nicht zum Wiedergeben von Ressourcen verwendet werden darf.
   * Eine `Deny` Entscheidung bedeutet, dass die Ressource nicht abspielbar ist. Die Antwort enthält eine Fehler-Payload, die der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der API](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)Dokumentation zum Abrufen von Entscheidungen vor der Autorisierung mit bestimmten mvpd.
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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **Entscheidungen vor Autorisierung verarbeiten:** Die Streaming-Anwendung verarbeitet die Antwort und kann sie verwenden, um optional den entsprechenden Status für jede Ressource auf der Benutzeroberfläche anzuzeigen.
