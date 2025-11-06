---
title: Einfache Abmeldung - Primäre Anwendung - Fluss
description: REST API v2 - Einfache Abmeldung - Primäre Anwendung - Fluss
exl-id: 21dbff4a-0d69-4f81-b04f-e99d743c35b3
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# Grundlegender Abmeldefluss innerhalb der primären Anwendung {#basic-logout-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

Mit dem **Abmeldefluss** innerhalb der Adobe Pass-Authentifizierungsberechtigung kann die Streaming-Anwendung zwei Hauptschritte ausführen:

* Löschen der im Adobe Pass-Backend gespeicherten Standardprofile.
* Navigieren Sie mit einem Benutzeragenten (Browser) zum Abmelde-Endpunkt von MVPD und lösen Sie eine Bereinigung im MVPD-Backend aus.

Mit dem einfachen Abmeldefluss können Sie Abfragen für die folgenden Szenarien durchführen:

* [Starten des Abmeldens für ein bestimmtes MVPD mit dem Abmeldeendpunkt](#initiate-logout-for-specific-mvpd-with-logout-endpoint)
* [Starten des Abmeldens für einen bestimmten MVPD ohne Abmeldeendpunkt](#initiate-logout-for-specific-mvpd-without-logout-endpoint)

## Initiieren des Abmeldens für ein bestimmtes MVPD mit Abmeldeendpunkt {#initiate-logout-for-specific-mvpd-with-logout-endpoint}

### Voraussetzungen {#prerequisites-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Bevor Sie mit dem Abmelden für eine bestimmte MVPD mit einem Abmeldeendpunkt beginnen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss über ein gültiges reguläres Profil verfügen, das mithilfe eines der grundlegenden Authentifizierungsflüsse erfolgreich für die MVPD erstellt wurde:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen](rest-api-v2-basic-authentication-secondary-application-flow.md)
* Die Streaming-Anwendung muss den Abmeldefluss initiieren, wenn sie sich von der MVPD abmelden muss.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die MVPD unterstützt den Abmeldefluss und verfügt über einen Abmeldeendpunkt.

### Workflow {#workflow-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Abmeldefluss für eine bestimmte MVPD mit einem Abmelde-Endpunkt zu implementieren, der in einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Initiieren des Abmeldens für bestimmte MVPDs mit dem Abmeldeendpunkt](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-with-logout-endpoint.png)

*Initiieren des Abmeldens für bestimmte MVPDs mit dem Abmeldeendpunkt*

1. **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um den Abmeldefluss zu initiieren, indem sie den Adobe Pass-Abmelde-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden Sie in [ API-Dokumentation ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) Initiieren des Abmeldens für bestimmte MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Profil anhand der empfangenen Parameter und Kopfzeilen.

1. **Reguläres Profil löschen:** Der Adobe Pass-Server löscht das identifizierte reguläre Profil aus dem Adobe Pass-Backend.

1. **Nächste Aktion angeben:** Die Antwort des Abmeldeendpunkts von Adobe Pass enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu führen:
   * Das `url`-Attribut ist vorhanden, da die MVPD den Abmeldefluss unterstützt.
   * Das `actionName`-Attribut ist auf „logout“ festgelegt.
   * Das `actionType`-Attribut ist auf „interaktiv“ festgelegt.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Abmeldeantwort enthaltenen Informationen finden Sie in der API](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)Dokumentation zum Initiieren des Abmeldens für bestimmte mvpd.
   > 
   > <br/>
   > 
   > Der Adobe Pass-Abmeldeendpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **MVPD-Abmeldung initiieren:** Die Streaming-Anwendung liest die `url` und verwendet einen Benutzeragenten, um den Abmeldefluss mit der MVPD zu initiieren. Der Fluss kann mehrere Umleitungen zu MVPD-Systemen umfassen. Das Ergebnis ist jedoch, dass MVPD seine interne Bereinigung durchführt und die endgültige Abmeldebestätigung zurück an das Adobe Pass-Backend sendet.

1. **Abmeldevorgang abgeschlossen anzeigen:** Die Streaming-Anwendung kann warten, bis der Benutzeragent das bereitgestellte `redirectUrl` erreicht, und sie als Signal verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.

## Starten des Abmeldens für einen bestimmten MVPD ohne Abmeldeendpunkt {#initiate-logout-for-specific-mvpd-without-logout-endpoint}

### Voraussetzungen {#prerequisites-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Bevor Sie mit dem Abmelden für eine bestimmte MVPD ohne Abmeldeendpunkt beginnen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung muss über ein gültiges reguläres Profil verfügen, das mithilfe eines der grundlegenden Authentifizierungsflüsse erfolgreich für die MVPD erstellt wurde:
   * [Authentifizierung innerhalb der primären Anwendung durchführen](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung mit vorab ausgewähltem mvpd durchführen](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Authentifizierung innerhalb der sekundären Anwendung ohne vorab ausgewählte mvpd durchführen](rest-api-v2-basic-authentication-secondary-application-flow.md)
* Die Streaming-Anwendung muss den Abmeldefluss initiieren, wenn sie sich von der MVPD abmelden muss.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Die MVPD unterstützt den Abmeldefluss nicht und verfügt über keinen Abmeldeendpunkt.

### Workflow {#workflow-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Abmeldefluss für einen bestimmten MVPD ohne Abmeldeendpunkt in einer Primäranwendung zu implementieren, wie im folgenden Diagramm dargestellt.

![Initiieren der Abmeldung für eine bestimmte mvpd ohne Abmeldeendpunkt](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-without-logout-endpoint.png)

*Initiieren der Abmeldung für eine bestimmte mvpd ohne Abmeldeendpunkt*

1. **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um den Abmeldefluss zu initiieren, indem sie den Adobe Pass-Abmelde-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden Sie in [ API-Dokumentation ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) Initiieren des Abmeldens für bestimmte MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Profil anhand der empfangenen Parameter und Kopfzeilen.

1. **Reguläres Profil löschen:** Der Adobe Pass-Server löscht das identifizierte reguläre Profil.

1. **Nächste Aktion angeben:** Die Antwort des Abmeldeendpunkts von Adobe Pass enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu führen:
   * Das `url` fehlt, da die MVPD den Abmeldefluss nicht unterstützt.
   * Das `actionName`-Attribut wird auf „complete“ gesetzt.
   * Das `actionType`-Attribut ist auf „none“ festgelegt.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [ in einer Abmeldeantwort enthaltenen Informationen finden Sie in der API](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)Dokumentation zum Initiieren des Abmeldens für bestimmte mvpd.
   > 
   > <br/>
   > 
   > Der Adobe Pass-Abmeldeendpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **Abmelde-Abschluss angeben:** Die Streaming-Anwendung verarbeitet die Antwort und kann sie verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.
