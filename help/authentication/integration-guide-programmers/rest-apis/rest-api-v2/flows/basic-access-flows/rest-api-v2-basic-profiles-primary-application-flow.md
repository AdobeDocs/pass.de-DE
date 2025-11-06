---
title: Grundlegende Profile - Primäre Anwendung - Fluss
description: REST API v2 - Grundprofile - Primäre Anwendung - Fluss
exl-id: 19ddf382-9a32-4b94-aa84-7611c0e1780e
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '974'
ht-degree: 0%

---

# Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird {#basic-profiles-flow-primary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

>[!MORELIKETHIS]
>
> Stellen Sie sicher, dass Sie auch die häufig gestellten Fragen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) besuchen.

Der **Profilfluss** innerhalb der Adobe Pass-Authentifizierungsberechtigung ermöglicht der Streaming-Anwendung den Zugriff auf Informationen zu aktiven Benutzeranmeldungen.

Mit dem Fluss „Grundlegende Profile“ können Sie Abfragen für die folgenden Szenarien durchführen:

* [Profile abrufen](#retrieve-profiles)
* [Abrufen eines Profils für ein bestimmtes MVPD](#retrieve-profile-for-specific-mvpd)
* [Abrufen eines Profils für einen bestimmten Code](#retrieve-profile-for-specific-code)

## Profile abrufen {#retrieve-profiles}

### Voraussetzungen {#prerequisites-retrieve-profiles}

Bevor Sie Profile abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung möchte alle regulären Profile abrufen.

### Workflow {#workflow-retrieve-profiles}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Profilabruffluss in einer primären Anwendung zu implementieren, wie im folgenden Diagramm dargestellt.

![Profile abrufen](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profiles-within-primary-application.png)

*Profile abrufen*

1. **Profile abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um alle Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt „Profiles“ gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [&#x200B; in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)Dokumentation zum Abrufen von Profilen:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Reguläre Profile suchen:** Der Adobe Pass-Server identifiziert alle gültigen Profile anhand der empfangenen Parameter und Kopfzeilen.

1. **Informationen zu regulären Profilen zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zu den gefundenen Profilen, die mit den empfangenen Parametern und Kopfzeilen verknüpft sind.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den [&#x200B; in einer Profilantwort bereitgestellten Informationen finden &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) in der API-Dokumentation zum Abrufen von Profilen .
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

1. **Wählen Sie ein Profil aus und fahren Sie mit Entscheidungsflüssen fort:** Wenn die Antwort des Endpunkts „Profile“ Profile enthält, verwendet die Streaming-Anwendung ihre interne Logik (letztendlich durch Interaktion mit dem Endbenutzer), um eines der verfügbaren Profile auszuwählen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.

1. **Neuen einfachen Authentifizierungsfluss angeben:** Wenn die Endpunktantwort „Profile“ kein Profil enthält, gibt die Streaming-Anwendung an, dass der Benutzer einen neuen einfachen Authentifizierungsfluss starten soll.

## Abrufen eines Profils für ein bestimmtes MVPD {#retrieve-profile-for-specific-mvpd}

### Voraussetzungen {#prerequisites-retrieve-profile-for-specific-mvpd}

Bevor Sie das Profil für eine bestimmte MVPD abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung mit einer ausgewählten oder zwischengespeicherten `mvpd`-Kennung möchte das Standardprofil für eine bestimmte MVPD abrufen.

### Workflow {#workflow-retrieve-profile-for-specific-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Profilabruffluss für eine bestimmte MVPD zu implementieren, der in einer primären Anwendung ausgeführt wird, wie in der folgenden Abbildung dargestellt.

![Profil für bestimmte MVPD abrufen](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-mvpd.png)

*Profil für bestimmte MVPD abrufen*

1. **Profil für bestimmte mvpd abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um Profilinformationen für diese bestimmte MVPD abzurufen, indem sie eine Anfrage an den Endpunkt „Profiles“ sendet.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu folgenden Themen finden [&#x200B; in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)Dokumentation zum Abrufen von Profilen für bestimmte MVPD:
   >
   > * Alle _erforderlichen_ Parameter wie `serviceProvider` und `mvpd`
   > * Alle _erforderlichen_ Kopfzeilen wie `Authorization`, `AP-Device-Identifier`
   > * Alle _optionalen_ Parameter und Kopfzeilen

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert ein gültiges Profil anhand der empfangenen Parameter und Kopfzeilen.

1. **Informationen zum regulären Profil zurückgeben:** Die Antwort des Endpunkts „Profile“ enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu [&#x200B; in einer Profilantwort angegebenen Informationen finden Sie in der API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)Dokumentation zum Abrufen von Profilen für bestimmte mvpd.
   > 
   > <br/>
   > 
   > Der Profiles-Endpunkt validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die _erforderlichen_ Parameter und Kopfzeilen müssen gültig sein.
   > * Die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen entsprechend der Dokumentation [Erweiterte Fehlercodes](../../../../features-standard/error-reporting/enhanced-error-codes.md) bereitstellt.

1. **Fahren Sie mit Entscheidungsflüssen fort:** Wenn die Antwort des Endpunkts „Profile“ ein Profil enthält, verwendet die Streaming-Anwendung die Profilinformationen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.

1. **Neuen einfachen Authentifizierungsfluss angeben:** Wenn die Endpunktantwort „Profile“ kein Profil enthält, gibt die Streaming-Anwendung an, dass der Benutzer einen neuen einfachen Authentifizierungsfluss starten soll.

## Abrufen eines Profils für einen bestimmten Code {#retrieve-profile-for-specific-code}

### Voraussetzungen {#prerequisites-retrieve-profile-for-specific-code}

Bevor Sie das Profil für einen bestimmten Authentifizierungs-Code abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung verfügt über eine `code`, mit der die interaktive Authentifizierung mit der MVPD durchgeführt wird. Sie möchte das Profil für einen bestimmten Authentifizierungs-Code abrufen.

### Workflow {#workflow-retrieve-profile-for-specific-code}

Führen Sie die angegebenen Schritte aus, um den einfachen Profilabruffluss für einen bestimmten Authentifizierungs-Code zu implementieren, der in einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Profil für bestimmten Code abrufen](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-code.png)

*Profil für bestimmten Code abrufen*

1. **Profil für bestimmten Code abrufen:** Die Streaming-Anwendung sammelt alle erforderlichen Daten, um Profilinformationen für diesen bestimmten Authentifizierungs-Code abzurufen, indem sie eine Anfrage an den Endpunkt „Profiles“ sendet.

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

1. **Fahren Sie mit Entscheidungsflüssen fort:** Wenn die Antwort des Endpunkts „Profile“ ein Profil enthält, verwendet die Streaming-Anwendung die Profilinformationen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.

1. **Neuen einfachen Authentifizierungsfluss angeben:** Wenn die Endpunktantwort „Profile“ kein Profil enthält, gibt die primäre Anwendung an, dass der Benutzer einen neuen einfachen Authentifizierungsfluss starten soll.
