---
title: Einzelne Abmeldung - Fluss
description: REST API v2 - Einzelne Abmeldung - Fluss
exl-id: d7092ca7-ea7b-4e92-b45f-e373a6d673d6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# Fluss für einmaliges Abmelden {#single-logout-flow}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

## Initiieren der einmaligen Abmeldung für eine bestimmte MVPD {#initiate-single-logout-for-specific-mvpd}

### Voraussetzungen {#prerequisites-initiate-single-logout-for-specific-mvpd}

Stellen Sie vor dem Initiieren des einmaligen Abmeldens für eine bestimmte MVPD sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die zweite Streaming-Anwendung muss über ein gültiges Single-Sign-On-Profil verfügen, das für den MVPD mit einem der Single-Sign-On-Authentifizierungsflüsse erfolgreich erstellt wurde:
   * [Durchführen der Authentifizierung über Single Sign-on unter Verwendung der Platform-Identität](rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Durchführen der Authentifizierung über Single Sign-on mit dem Service-Token](rest-api-v2-single-sign-on-service-token-flows.md)
* Die zweite Streaming-Anwendung muss den Ablauf der einmaligen Abmeldung initiieren, wenn sie sich von der MVPD abmelden muss.

>[!IMPORTANT]
> 
> Annahmen
>
> <br/>
> 
> * Die erste und die zweite Streaming-Anwendung erhalten dieselbe eindeutige Plattformkennungs-Payload wie `JWS` oder `JWE` oder dieselbe eindeutige Benutzerkennungs-Payload wie `JWS`.

### Workflow {#workflow-initiate-single-logout-for-specific-mvpd}

Führen Sie die angegebenen Schritte aus, um den einzelnen Abmeldefluss für eine bestimmte MVPD zu implementieren, wie in der folgenden Abbildung dargestellt.

![Initiieren des einmaligen Abmeldens für eine bestimmte mvpd](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*Initiieren des einmaligen Abmeldens für eine bestimmte mvpd*

1. **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um den Abmeldefluss zu initiieren, indem sie den Adobe Pass-Abmelde-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden Sie in [ API-Dokumentation ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) Initiieren des Abmeldens für bestimmte MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`, `mvpd` und `redirectUrl`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen
   >
   > <br/>
   >
   > Die Streaming-Anwendung muss vor einer Anfrage sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung oder die eindeutige Benutzerkennung enthält.
   >
   > <br/>
   > 
   > Weitere Informationen zu `Adobe-Subject-Token`-Header finden Sie in der Dokumentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .
   > 
   > <br/>
   > 
   > Weitere Informationen zu `AD-Service-Token`-Kopfzeile finden Sie in der Dokumentation [AD-Service-](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)&quot;.

1. **Profile mit regulärem und Single Sign-on suchen:** Der Adobe Pass-Server identifiziert sowohl Profile mit regulärem als auch mit Single Sign-on, die auf den empfangenen Parametern und Kopfzeilen basieren.

1. **Löschen von regulären und Single Sign-on-Profilen:** Der Adobe Pass-Server löscht die identifizierten regulären und Single Sign-on-Profile aus dem Adobe Pass-Backend.

1. **Nächste Aktion angeben:** Die Antwort des Abmeldeendpunkts von Adobe Pass enthält die erforderlichen Daten, um die Streaming-Anwendung bezüglich der nächsten Aktion zu führen.

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

1. **Abmeldevorgang abschließen:** Wenn die MVPD den Abmeldevorgang nicht unterstützt, verarbeitet die Streaming-Anwendung die Antwort und kann sie verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.

1. **MVPD-Abmeldung initiieren:** Wenn die MVPD den Abmeldefluss unterstützt, verarbeitet die Streaming-Anwendung die Antwort und verwendet einen Benutzeragenten, um den Abmeldefluss mit der MVPD zu initiieren. Der Fluss kann mehrere Umleitungen zu MVPD-Systemen umfassen. Das Ergebnis ist jedoch, dass MVPD seine interne Bereinigung durchführt und die endgültige Abmeldebestätigung zurück an das Adobe Pass-Backend sendet.

1. **Abmeldevorgang abgeschlossen anzeigen:** Die Streaming-Anwendung kann warten, bis der Benutzeragent das bereitgestellte `redirectUrl` erreicht, und sie als Signal verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.

>[!NOTE]
>
> Die Schritte für den Fluss der einmaligen Abmeldung sind dieselben wie oben, wenn er von der ersten Streaming-Anwendung initiiert wird.
