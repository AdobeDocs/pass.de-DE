---
title: Einfache Abmeldung - Primäre Anwendung - Fluss
description: REST API V2 - Einfache Abmeldung - Primäre Anwendung - Fluss
exl-id: 21dbff4a-0d69-4f81-b04f-e99d743c35b3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# Grundlegender Abmeldefluss innerhalb der primären Anwendung {#basic-logout-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) begrenzt.

Der **Abmeldefluss** innerhalb der Adobe Pass-Authentifizierungsberechtigung ermöglicht der Streaming-Anwendung die Durchführung von zwei Hauptschritten:

* Löschen Sie die im Adobe Pass-Backend gespeicherten regulären Profile.
* Verwenden Sie einen Benutzeragenten (Browser), um zum MVPD-Abmelde-Endpunkt zu navigieren und eine Bereinigung am MVPD-Backend auszulösen.

Mit dem einfachen Abmeldefluss können Sie nach den folgenden Szenarien abfragen:

* [Initiieren der Abmeldung für bestimmte mvpd mit Abmelde-Endpunkt](#initiate-logout-for-specific-mvpd-with-logout-endpoint)
* [Initiieren der Abmeldung für bestimmte mvpd ohne Abmelde-Endpunkt](#initiate-logout-for-specific-mvpd-without-logout-endpoint)

## Initiieren der Abmeldung für bestimmte mvpd mit Abmelde-Endpunkt {#initiate-logout-for-specific-mvpd-with-logout-endpoint}

### Voraussetzungen {#prerequisites-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Bevor Sie die Abmeldung für einen bestimmten MVPD mit einem Abmelde-Endpunkt starten, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss über ein gültiges reguläres Profil verfügen, das mithilfe eines der grundlegenden Authentifizierungsflüsse erfolgreich für den MVPD erstellt wurde:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewählter mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
* Die Streaming-Anwendung muss den Abmeldefluss initiieren, wenn sie sich von der MVPD abmelden muss.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Der MVPD unterstützt den Abmeldefluss und verfügt über einen Abmelde-Endpunkt.

### Workflow {#workflow-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Abmeldefluss für einen bestimmten MVPD mit einem Abmeldeendpunkt zu implementieren, der innerhalb einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Initiieren der Abmeldung für bestimmte mvpd mit Abmelde-Endpunkt](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-with-logout-endpoint.png)

*Initiieren der Abmeldung für bestimmte mvpd mit Abmelde-Endpunkt*

1. **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um den Abmeldefluss durch Aufruf des Adobe Pass-Abmeldeendpunkts zu initiieren.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur MVPD](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)-API unter [Initiate logout .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Profil.

1. **Reguläres Profil löschen:** Der Adobe Pass-Server löscht das identifizierte reguläre Profil aus dem Adobe Pass-Backend.

1. **Geben Sie die nächste Aktion an:** Die Antwort des Adobe Pass Logout-Endpunkts enthält die erforderlichen Daten, um die Streaming-Anwendung in Bezug auf die nächste Aktion zu leiten:
   * Das Attribut `url` ist vorhanden, da der MVPD den Abmeldefluss unterstützt.
   * Das Attribut `actionName` ist auf &quot;Abmeldung&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;interaktiv&quot;festgelegt.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Abmeldeantwort finden Sie in der Dokumentation zur API für die MVPD](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)-API unter [Initiate logout .
   > 
   > <br/>
   > 
   > Der Adobe Pass Logout-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) entsprechen.

1. **MVPD-Abmeldung initiieren:** Die Streaming-Anwendung liest die `url` und verwendet einen Benutzeragenten, um den Abmeldefluss mit dem MVPD zu initiieren. Der Fluss kann mehrere Umleitungen zu MVPD-Systemen umfassen. Das Ergebnis ist jedoch, dass der MVPD seine interne Bereinigung durchführt und die endgültige Abmeldebestätigung zurück an das Adobe Pass-Backend sendet.

1. **Angeben, dass die Abmeldung abgeschlossen ist:** Die Streaming-Anwendung kann warten, bis der Benutzeragent den angegebenen `redirectUrl` erreicht, und sie kann ihn als Signal verwenden, um optional eine bestimmte Meldung auf der Benutzeroberfläche anzuzeigen.

## Initiieren der Abmeldung für bestimmte mvpd ohne Abmelde-Endpunkt {#initiate-logout-for-specific-mvpd-without-logout-endpoint}

### Voraussetzungen {#prerequisites-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Bevor Sie die Abmeldung für einen bestimmten MVPD ohne Abmelde-Endpunkt starten, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss über ein gültiges reguläres Profil verfügen, das mithilfe eines der grundlegenden Authentifizierungsflüsse erfolgreich für den MVPD erstellt wurde:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewählter mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
* Die Streaming-Anwendung muss den Abmeldefluss initiieren, wenn sie sich von der MVPD abmelden muss.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Der MVPD unterstützt den Abmeldefluss nicht und hat keinen Abmeldeendpunkt.

### Workflow {#workflow-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Abmeldefluss für einen bestimmten MVPD ohne einen Abmelde-Endpunkt zu implementieren, der innerhalb einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Initiieren der Abmeldung für bestimmte mvpd ohne Abmelde-Endpunkt](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-without-logout-endpoint.png)

*Initiieren der Abmeldung für bestimmte mvpd ohne Abmelde-Endpunkt*

1. **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um den Abmeldefluss durch Aufruf des Adobe Pass-Abmeldeendpunkts zu initiieren.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur MVPD](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)-API unter [Initiate logout .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Profil.

1. **Reguläres Profil löschen:** Der Adobe Pass-Server löscht das identifizierte reguläre Profil.

1. **Geben Sie die nächste Aktion an:** Die Antwort des Adobe Pass Logout-Endpunkts enthält die erforderlichen Daten, um die Streaming-Anwendung in Bezug auf die nächste Aktion zu leiten:
   * Das Attribut `url` fehlt, da der MVPD den Abmeldefluss nicht unterstützt.
   * Das Attribut `actionName` ist auf &quot;complete&quot;festgelegt.
   * Das Attribut `actionType` ist auf &quot;none&quot;(keine) festgelegt.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Abmeldeantwort finden Sie in der Dokumentation zur API für die MVPD](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)-API unter [Initiate logout .
   > 
   > <br/>
   > 
   > Der Adobe Pass Logout-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) entsprechen.

1. **Angeben, dass die Abmeldung abgeschlossen ist:** Die Streaming-Anwendung verarbeitet die Antwort und kann sie verwenden, um optional eine bestimmte Meldung auf der Benutzeroberfläche anzuzeigen.
