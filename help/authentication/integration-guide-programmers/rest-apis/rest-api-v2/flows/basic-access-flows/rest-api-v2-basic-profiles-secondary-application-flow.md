---
title: Grundlegende Profile - Sekundäre Anwendung - Fluss
description: REST API v2 - Grundprofile - Sekundäre Anwendung - Fluss
exl-id: 1fcefcfa-7534-4b85-b3b5-df513685d66b
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 0%

---

# Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird {#basic-profiles-flow-secondary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

>[!MORELIKETHIS]
>
> Stellen Sie sicher, dass Sie auch die häufig gestellten Fragen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) besuchen.

Der **Profilfluss** innerhalb der Adobe Pass-Authentifizierungsberechtigung ermöglicht der sekundären Anwendung den Zugriff auf Informationen über aktive Benutzeranmeldungen.

Mit dem Fluss „Grundlegende Profile“ können Sie Abfragen für die folgenden Szenarien durchführen:

* [Abrufen eines Profils für einen bestimmten Code](#retrieve-profile-for-specific-code)

## Abrufen eines Profils für einen bestimmten Code {#retrieve-profile-for-specific-code}

### Voraussetzungen {#prerequisites-retrieve-profile-for-specific-code}

Bevor Sie das Profil für einen bestimmten Authentifizierungs-Code abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die sekundäre Anwendung, deren `code` verwendet wird, um die interaktive Authentifizierung mit der MVPD durchzuführen, möchte das Profil für einen bestimmten Authentifizierungs-Code abrufen.

### Workflow {#workflow-retrieve-profile-for-specific-code}

Führen Sie die angegebenen Schritte aus, um den einfachen Profilabruffluss für einen bestimmten Authentifizierungs-Code zu implementieren, der in einer sekundären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Profil für bestimmten Code abrufen](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*Profil für bestimmten Code abrufen*

1. **Profil für bestimmten Code abrufen:** Die sekundäre Anwendung erfasst alle erforderlichen Daten, um Profilinformationen für diesen bestimmten Authentifizierungs-Code abzurufen, indem eine Anfrage an den Endpunkt „Profiles“ gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden [&#x200B; finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)Dokumentation zum Abrufen von Profilen für bestimmten Code:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider` und `code`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Profil anhand der empfangenen Parameter und Kopfzeilen.

1. **Informationen zum regulären Profil zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu [&#x200B; in einer Profilantwort bereitgestellten Informationen finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)Dokumentation zum Abrufen von Profilen für bestimmten Code.
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

1. **Authentifizierungsfluss erfolgreich abgeschlossen angeben:** Wenn die Endpunktantwort „Profile“ ein Profil enthält, verarbeitet die sekundäre Anwendung die Antwort und kann sie verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.

1. **Authentifizierungsfluss angeben, bei dem ein Problem aufgetreten ist:** Wenn die Endpunktantwort „Profile“ kein Profil enthält, verarbeitet die sekundäre Anwendung die Antwort und kann sie verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.
