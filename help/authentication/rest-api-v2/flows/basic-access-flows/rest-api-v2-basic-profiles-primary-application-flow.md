---
title: Grundlegende Profile - Primäre Anwendung - Fluss
description: REST API V2 - Grundlegende Profile - Primäre Anwendung - Fluss
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '964'
ht-degree: 0%

---


# Durchfluss von Standardprofilen innerhalb der primären Anwendung {#basic-profiles-flow-primary-application}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

Der **Profilfluss** innerhalb der Adobe Pass-Authentifizierungsberechtigung ermöglicht der Streaming-Anwendung den Zugriff auf Informationen über aktive Benutzeranmeldungen.

Mit dem einfachen Profilfluss können Sie nach folgenden Szenarien abfragen:

* [Profile abrufen](#retrieve-profiles)
* [Profil für bestimmte mvpd abrufen](#retrieve-profile-for-specific-mvpd)
* [Profil für bestimmten Code abrufen](#retrieve-profile-for-specific-code)

## Profile abrufen {#retrieve-profiles}

### Voraussetzungen {#prerequisites-retrieve-profiles}

Stellen Sie vor dem Abrufen von Profilen sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung möchte alle regulären Profile abrufen.

### Workflow {#workflow-retrieve-profiles}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Abruffluss von Profilen zu implementieren, der in einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Profile abrufen](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profiles-within-primary-application.png)

*Profile abrufen*

1. **Profile abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um alle Profilinformationen abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Abrufen von Profilen](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) :
   >
   > * Alle _erforderlichen_ Parameter, z. B. `serviceProvider`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Reguläre Profile suchen:** Der Adobe Pass-Server identifiziert alle gültigen Profile anhand der empfangenen Parameter und Kopfzeilen.

1. **Informationen zu regulären Profilen zurückgeben:** Die Profil-Endpunktantwort enthält Informationen zu den gefundenen Profilen, die mit den empfangenen Parametern und Kopfzeilen verknüpft sind.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen, die in einer Profilantwort bereitgestellt werden, finden Sie in der Dokumentation zur API zum [Abrufen von Profilen](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) .
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

1. **Wählen Sie ein Profil aus und fahren Sie mit den Entscheidungsabläufen fort:** Wenn die Profil-Endpunktantwort Profile enthält, verwendet die Streaming-Anwendung ihre interne Logik (schließlich durch Interaktion mit dem Endbenutzer), um eines der verfügbaren Profile auszuwählen und die nachfolgenden Entscheidungsflüsse fortzusetzen.

1. **Neuen grundlegenden Authentifizierungsfluss angeben:** Wenn die Profil-Endpunktantwort kein Profil enthält, zeigt die Streaming-Anwendung an, dass der Benutzer einen neuen einfachen Authentifizierungsfluss initiiert.

## Profil für bestimmte mvpd abrufen {#retrieve-profile-for-specific-mvpd}

### Voraussetzungen {#prerequisites-retrieve-profile-for-specific-mvpd}

Bevor Sie das Profil für einen bestimmten MVPD abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung, die über eine ausgewählte oder zwischengespeicherte `mvpd` -Kennung verfügt, möchte das reguläre Profil für einen bestimmten MVPD abrufen.

### Workflow {#workflow-retrieve-profile-for-specific-mvpd}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Profilabruf-Fluss für einen bestimmten MVPD zu implementieren, der in einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Profil für bestimmte mvpd abrufen](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-mvpd.png)

*Profil für bestimmte mvpd abrufen*

1. **Profil für bestimmte MVPD abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen für diese bestimmte MVPD abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen des Profils für bestimmte mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) :
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `mvpd`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization`, `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Profil.

1. **Informationen zum regulären Profil zurückgeben:** Die Profil-Endpunktantwort enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Profilantwort finden Sie in der Dokumentation zur API [Profil für bestimmte mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) -API.
   > 
   > <br/>
   > 
   > Der Endpunkt Profile validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Mit Entscheidungsflüssen fortfahren:** Wenn die Profil-Endpunktantwort ein Profil enthält, verwendet die Streaming-Anwendung die Profilinformationen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.

1. **Neuen grundlegenden Authentifizierungsfluss angeben:** Wenn die Profil-Endpunktantwort kein Profil enthält, zeigt die Streaming-Anwendung an, dass der Benutzer einen neuen einfachen Authentifizierungsfluss initiiert.

## Profil für bestimmten Code abrufen {#retrieve-profile-for-specific-code}

### Voraussetzungen {#prerequisites-retrieve-profile-for-specific-code}

Bevor Sie das Profil für einen bestimmten Authentifizierungscode abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung mit dem Wert &quot;`code`&quot;zur Durchführung der interaktiven Authentifizierung mit dem MVPD möchte das Profil für einen bestimmten Authentifizierungscode abrufen.

### Workflow {#workflow-retrieve-profile-for-specific-code}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Profilabruf-Fluss für einen bestimmten Authentifizierungscode zu implementieren, der in einer primären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Profil für bestimmten Code abrufen](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-code.png)

*Profil für bestimmten Code abrufen*

1. **Profil für bestimmten Code abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen für diesen spezifischen Authentifizierungscode abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur [Abrufen des Profils für bestimmte Code-API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) .
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `code`
   > * Alle _erforderlichen_ -Kopfzeilen, z. B. `Authorization`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Profil.

1. **Informationen zum regulären Profil zurückgeben:** Die Profil-Endpunktantwort enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

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

1. **Mit Entscheidungsflüssen fortfahren:** Wenn die Profil-Endpunktantwort ein Profil enthält, verwendet die Streaming-Anwendung die Profilinformationen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.

1. **Neuen grundlegenden Authentifizierungsfluss angeben:** Wenn die Profil-Endpunktantwort kein Profil enthält, zeigt die primäre Anwendung den Benutzer an, einen neuen einfachen Authentifizierungsfluss zu initiieren.
