---
title: Grundlegende Autorisierung - Primäre Anwendung - Fluss
description: REST API V2 - Grundlegende Autorisierung - Primäre Anwendung - Fluss
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---


# Grundlegender Autorisierungsfluss innerhalb der Hauptanwendung {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

Mit dem **Autorisierungsfluss** innerhalb der Adobe Pass-Authentifizierungsberechtigungen kann die Streaming-Anwendung bestimmen, ob ein MVPD die Anfrage des Benutzers zum Streamen von Inhalten zulässt oder verweigert. Wenn die Entscheidung `Permit` lautet, enthält die Antwort ein Medien-Token. Der Adobe Pass-Server signiert das Medien-Token und ermöglicht es der Streaming-Anwendung, die Überprüfungsbibliothek für Medien-Token zu verwenden, um die Authentizität zu überprüfen, bevor der Stream veröffentlicht wird.

Die Verifizierung mit der Medien-Token-Überprüfungsbibliothek sollte für den Streaming-Anwendungs-Backend-Service erfolgen, der in der Berechtigungskette für die Freigabe eines Streams aus dem CDN verknüpft ist.

## Abrufen von Autorisierungsentscheidungen mit einer bestimmten mvpd {#retrieve-authorization-decisions-using-specific-mvpd}

### Voraussetzungen {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

Bevor Sie Autorisierungsentscheidungen mit einem bestimmten MVPD abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss über ein gültiges reguläres Profil verfügen, das mithilfe eines der grundlegenden Authentifizierungsflüsse erfolgreich für den MVPD erstellt wurde:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](./rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewählter mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* Die Streaming-Anwendung muss eine Autorisierungsentscheidung abrufen, bevor eine vom Benutzer ausgewählte Ressource wiedergegeben wird.

### Workflow {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Autorisierungsfluss mithilfe eines bestimmten MVPD zu implementieren, der in einer Primäranwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Abrufen von Autorisierungsentscheidungen mit einer bestimmten mvpd](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*Abrufen von Autorisierungsentscheidungen mit einer bestimmten mvpd*

1. **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Endpunkt Entscheidungsautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API:
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Profil.

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

1. **Stream mit Medien-Token starten:** Die Streaming-Anwendung verwendet das Medien-Token zur Wiedergabe des Inhalts.

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

1. **Entscheidungsdetails verarbeiten `Deny`:** Die Streaming-Anwendung verarbeitet die Fehlerinformationen aus der Antwort und kann sie verwenden, um optional eine bestimmte Meldung auf der Benutzeroberfläche anzuzeigen.
