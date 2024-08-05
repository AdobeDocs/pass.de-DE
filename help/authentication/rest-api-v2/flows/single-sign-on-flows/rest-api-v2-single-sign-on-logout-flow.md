---
title: Einzelabmeldung - Fluss
description: REST API V2 - Single Logout - Fluss
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 0%

---


# Einmaliger Abmeldefluss {#single-logout-flow}

## Einmalige Abmeldung für bestimmte mvpd starten {#initiate-single-logout-for-specific-mvpd}

### Voraussetzungen {#prerequisites-initiate-single-logout-for-specific-mvpd}

Bevor Sie die einmalige Abmeldung für einen bestimmten MVPD starten, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die zweite Streaming-Anwendung muss über ein gültiges Single Sign-On-Profil verfügen, das erfolgreich für den MVPD mithilfe eines der Single Sign-On-Authentifizierungsflüsse erstellt wurde:
   * [Authentifizierung über Single Sign-on mithilfe der Plattformidentität durchführen](./rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Führen Sie die Authentifizierung über Single Sign-on mithilfe des Service-Tokens durch.](./rest-api-v2-single-sign-on-service-token-flows.md)
* Die zweite Streaming-Anwendung muss den einmaligen Abmeldefluss initiieren, wenn sie sich von der MVPD abmelden muss.

>[!IMPORTANT]
> 
> Annahmen
>
> <br/>
> 
> * Die erste und zweite Streaming-Anwendung erhalten dieselbe eindeutige Plattform-ID-Payload wie `JWS` oder `JWE` oder dieselbe eindeutige Benutzer-ID-Payload wie `JWS`.

### Workflow {#workflow-initiate-single-logout-for-specific-mvpd}

Führen Sie die angegebenen Schritte aus, um den einmaligen Abmeldefluss für einen bestimmten MVPD zu implementieren, wie im folgenden Diagramm dargestellt.

![ Einmalige Abmeldung für bestimmte mvpd starten](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*Einmalige Abmeldung für bestimmte mvpd starten*

1. **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um den Abmeldefluss durch Aufruf des Adobe Pass-Abmeldeendpunkts zu initiieren.

   Weitere Informationen finden Sie in der Dokumentation zur MVPD](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)-API unter [Initiate logout .
   * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `redirectUrl`
   * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   * Alle Parameter und Kopfzeilen von _optional_

   >[!IMPORTANT]
   > 
   > Die Streaming-Anwendung muss sicherstellen, dass sie einen gültigen Wert für die eindeutige Plattformkennung oder die eindeutige Benutzer-ID enthält, bevor eine Anfrage gestellt wird.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `Adobe-Subject-Token` finden Sie in der Dokumentation [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .
   > 
   > <br/>
   > 
   > Weitere Informationen zum Header `AD-Service-Token` finden Sie in der Dokumentation zu [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) .

1. **Suchen Sie nach regulären und Single-Sign-On-Profilen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen sowohl reguläre als auch für Single-Sign-On-Profile.

1. **Löschen von normalen und Single Sign-On-Profilen:** Der Adobe Pass-Server löscht die identifizierten regulären und Single-Sign-On-Profile aus dem Adobe Pass-Backend.

1. **Geben Sie die nächste Aktion an:** Die Adobe Pass Logout-Endpunktantwort enthält die erforderlichen Daten, um die Streaming-Anwendung zur nächsten Aktion zu leiten.

   Weitere Informationen zu den Informationen in einer Abmeldeantwort finden Sie in der Dokumentation zur API für die MVPD](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)-API unter [Initiate logout .

   >[!IMPORTANT]
   >
   > Der Adobe Pass Logout-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Angeben, dass der Abmeldevorgang abgeschlossen ist:** Wenn der MVPD den Abmeldefluss nicht unterstützt, verarbeitet die Streaming-Anwendung die Antwort und kann sie verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.

1. **MVPD-Abmeldung initiieren:** Wenn das MVPD den Abmeldefluss unterstützt, verarbeitet die Streaming-Anwendung die Antwort und verwendet einen Benutzeragenten, um den Abmeldefluss mit dem MVPD zu initiieren. Der Fluss kann mehrere Umleitungen zu MVPD-Systemen umfassen. Das Ergebnis ist jedoch, dass der MVPD seine interne Bereinigung durchführt und die endgültige Abmeldebestätigung zurück an das Adobe Pass-Backend sendet.

1. **Angeben, dass die Abmeldung abgeschlossen ist:** Die Streaming-Anwendung kann warten, bis der Benutzeragent den angegebenen `redirectUrl` erreicht, und sie kann ihn als Signal verwenden, um optional eine bestimmte Meldung auf der Benutzeroberfläche anzuzeigen.

>[!NOTE]
>
> Die Schritte für den einzelnen Abmeldefluss sind mit den oben genannten Schritten identisch, wenn sie von der ersten Streaming-Anwendung initiiert werden.
