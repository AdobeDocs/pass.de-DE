---
title: Grundlegende Profile - Sekundäre Anwendung - Fluss
description: REST API V2 - Grundlegende Profile - Sekundäre Anwendung - Fluss
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---


# Durchgang der einfachen Profile, der in der sekundären Anwendung ausgeführt wird {#basic-profiles-flow-secondary-application}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Der **Profilfluss** innerhalb der Adobe Pass-Authentifizierungsberechtigung ermöglicht der sekundären Anwendung den Zugriff auf Informationen über aktive Benutzeranmeldungen.

Mit dem einfachen Profilfluss können Sie nach folgenden Szenarien abfragen:

* [Profil für bestimmten Code abrufen](#retrieve-profile-for-specific-code)

## Profil für bestimmten Code abrufen {#retrieve-profile-for-specific-code}

### Voraussetzungen {#prerequisites-retrieve-profile-for-specific-code}

Bevor Sie das Profil für einen bestimmten Authentifizierungscode abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die sekundäre Anwendung, die über einen `code` verfügt, der zur Durchführung der interaktiven Authentifizierung mit dem MVPD verwendet wird, möchte das Profil für einen bestimmten Authentifizierungscode abrufen.

### Workflow {#workflow-retrieve-profile-for-specific-code}

Führen Sie die angegebenen Schritte aus, um den grundlegenden Profilabruf für einen bestimmten Authentifizierungscode zu implementieren, der in einer sekundären Anwendung ausgeführt wird, wie im folgenden Diagramm dargestellt.

![Profil für bestimmten Code abrufen](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*Profil für bestimmten Code abrufen*

1. **Profil für bestimmten Code abrufen:** Die sekundäre Anwendung erfasst alle erforderlichen Daten, um Profilinformationen für diesen spezifischen Authentifizierungscode abzurufen, indem sie eine Anfrage an den Endpunkt Profile sendet.

   Weitere Informationen finden Sie in der Dokumentation zur [Abrufen des Profils für bestimmte Code-API](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) .
   * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `code`
   * Alle _erforderlichen_ -Kopfzeilen, z. B. `Authorization`
   * Alle Parameter und Kopfzeilen von _optional_

1. **Reguläres Profil suchen:** Der Adobe Pass-Server identifiziert anhand der empfangenen Parameter und Kopfzeilen ein gültiges Profil.

1. **Informationen zum regulären Profil zurückgeben:** Die Profil-Endpunktantwort enthält Informationen zum gefundenen Profil, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   Weitere Informationen zu den Informationen, die in einer Profilantwort bereitgestellt werden, finden Sie in der Dokumentation zur API für spezifischen Code-Code-Abruf-Profil](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) .[

   >[!NOTE]
   >
   > Der Endpunkt Profile validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   > 
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Authentifizierungsfluss mit Erfolg angeben:** Wenn die Profil-Endpunktantwort ein Profil enthält, verarbeitet die sekundäre Anwendung die Antwort und kann sie verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.

1. **Gibt an, dass bei Authentifizierungsfluss ein Problem aufgetreten ist:** Wenn die Profil-Endpunktantwort kein Profil enthält, verarbeitet die sekundäre Anwendung die Antwort und kann sie verwenden, um optional eine bestimmte Nachricht auf der Benutzeroberfläche anzuzeigen.
