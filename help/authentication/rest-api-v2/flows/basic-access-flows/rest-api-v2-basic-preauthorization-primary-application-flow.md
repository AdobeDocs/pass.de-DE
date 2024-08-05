---
title: Grundlegende Vorautorisierung - Primäre Anwendung - Fluss
description: REST API V2 - Grundlegende Vorautorisierung - Primäre Anwendung - Fluss
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---


# Grundlegender Ablauf der Vorautorisierung innerhalb der Hauptanwendung {#basic-preauthorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

Mit dem **Vorabautorisierungsfluss** innerhalb der Adobe Pass-Authentifizierungsberechtigungen kann die Streaming-Anwendung bestimmen, ob ein MVPD den Zugriff des Benutzers auf eine Ressourcenliste zulassen oder verweigern kann. Diese Überprüfung stellt sicher, dass die Anwendung dem Benutzer genaue Informationen über den Inhalt präsentieren kann, den er anzeigen darf.

## Abrufen von Vorab-Autorisierungsentscheidungen mit einer bestimmten mvpd {#retrieve-preauthorization-decisions-using-specific-mvpd}

### Voraussetzungen {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

Bevor Sie Entscheidungen über die Vorabgenehmigung mit einem bestimmten MVPD abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss über ein gültiges reguläres Profil verfügen, das mithilfe eines der grundlegenden Authentifizierungsflüsse erfolgreich für den MVPD erstellt wurde:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](./rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewählter mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* Die Streaming-Anwendung möchte Entscheidungen zur Vorabautorisierung abrufen, um eine Liste der Ressourcen mit den zugehörigen Status anzuzeigen.

### Workflow {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Vorautorisierungsfluss mithilfe eines bestimmten MVPD zu implementieren, der innerhalb einer Primäranwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Abrufen von Vorautorisierungsentscheidungen mithilfe bestimmter mvpd](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*Abrufen von Vorautorisierungsentscheidungen mithilfe bestimmter mvpd*

1. **Vorabautorisierungsentscheidungen abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Vorautorisierungsentscheidungen für eine Liste von Ressourcen zu erhalten, indem sie den Endpunkt Entscheidungsvorautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Vorautorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) -API .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Profil.

1. **Abrufen von MVPD-Entscheidungen für angeforderte Ressourcen:** Der Adobe Pass-Server ruft den MVPD-Vorabautorisierungsendpunkt auf, um eine `Permit` - oder `Deny` -Entscheidung für jede von der Streaming-Anwendung empfangene Ressource zu erhalten.

1. **Rückgabe von Entscheidungen bezüglich der Vorabautorisierung:** Die Antwort der Entscheidungsvorautorisierung des Endpunkts enthält eine `Permit` - oder `Deny` -Entscheidung für jede Ressource:
   * Eine &quot;`Permit`&quot;-Entscheidung bedeutet, dass die Ressource wiedergegeben werden kann. Die Antwort enthält kein Medien-Token, da der Fluss zur Vorabautorisierung nicht zum Abspielen von Ressourcen verwendet werden darf.
   * Eine &quot;`Deny`&quot;-Entscheidung bedeutet, dass die Ressource nicht wiedergegeben werden kann. Die Antwort enthält eine Fehler-Payload, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Entscheidungsantwort finden Sie in der Dokumentation zur API [Abrufen von Vorautorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) -API.
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
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Vorautorisierungsentscheidungen verarbeiten:** Die Streaming-Anwendung verarbeitet die Antwort und kann sie verwenden, um optional den entsprechenden Status für jede Ressource in der Benutzeroberfläche anzuzeigen.
