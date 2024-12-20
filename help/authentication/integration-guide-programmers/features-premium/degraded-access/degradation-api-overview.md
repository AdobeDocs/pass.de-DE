---
title: API-Übersicht zur Beeinträchtigung
description: API-Übersicht zur Beeinträchtigung
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# API-Übersicht zur Beeinträchtigung {#degradation-api-overview}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Stellen Sie vor der Verwendung der Degradation API sicher, dass die folgenden Voraussetzungen erfüllt sind:
>
> * Rufen Sie die Client-Anmeldeinformationen ab, wie in der API[Dokumentation zum Abrufen von Client](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)Anmeldeinformationen beschrieben.
> * Rufen Sie das Zugriffstoken ab, wie in der API[Dokumentation zum Abrufen des Zugriffstokens ](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Weitere Informationen zum Erstellen einer registrierten [ und zum Herunterladen der Software-Anweisung finden ](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) in der Dokumentation Übersicht zur Dynamic Client-Registrierung .

## Allgemeine Informationen {#general_info}

>[!NOTE]
>
>Diese API ist nicht allgemein verfügbar. Wenden Sie sich an Ihren Adobe-Support-Mitarbeiter, um Informationen zu Verfügbarkeitsaktualisierungen zu erhalten.

Diese Funktion bietet allen drei Hauptbeteiligten an einer Integration (Programmierern, MVPDs und Adobe) die Möglichkeit, bestimmte MVPD-Authentifizierungs- und Autorisierungsendpunkte vorübergehend zu umgehen. Normalerweise ist es der Programmierer, der eine solche Aktion initiiert, aber unabhängig davon, wer ein Degradation-Ereignis Trigger, hängt die Aktion von zuvor vereinbarten Vereinbarungen mit den betroffenen MVPDs ab.

Der Hauptanwendungsfall für diese Funktion tritt bei Live-Sport oder großen Events auf. In Szenarien mit derart hohem Traffic kann die Belastung eines bestimmten MVPD-Endpunkts zu hoch werden, was zu sehr langen Antwortzeiten für -Benutzende führt. Um während eines solchen Szenarios ein gutes Benutzererlebnis zu erhalten, kann der Programmierer beschließen, eine Degradationsregel Trigger, die Benutzende vorübergehend automatisch authentifizieren/automatisch autorisieren kann, oder eine MVPD deaktivieren, indem er sie aus der Liste der verfügbaren MVPDs entfernt.

Eine Degradationsregel wird nur für einen bestimmten Zeitraum angewendet. Obwohl die Hauptkunden für diese Funktion Sportkanäle und Live-Nachrichtenkanäle sind, möchte jeder Programmierer möglicherweise Zugriff auf diese Funktion haben, da die MVPDs-Dienste von Zeit zu Zeit heruntergehen.

Abbauhinweise:

- Diese Funktion ist für die Verwendung mit der Nutzungsüberwachungs-API konzipiert, die Echtzeitinformationen über die Anzahl der Authentifizierungen und Berechtigungen pro MVPD, die durchschnittliche Autorisierungslatenz und andere Metriken bereitstellt, die für eine vollständige Service-Übersicht erforderlich sind.
- Diese Funktion ermöglicht keine Umgehung des Adobe Pass-Authentifizierungsdienstes. Wenn die Adobe Pass-Authentifizierung deaktiviert ist, gibt es innerhalb des Service keinen Mechanismus, der verwendet werden kann, um Benutzenden das Anzeigen von Inhalten zu ermöglichen. Die Sites oder Apps können jedoch die Adobe Pass-Authentifizierung selbst umgehen.
- Adobe wird derzeit nicht Trigger-Abbau direkt - die Entscheidung muss immer bei einem bestimmten Programmierer, die solche Bedingungen mit MVPDs. In Zukunft könnte die Adobe Pass-Authentifizierung proaktiv Abbauregeln auslösen, falls Vereinbarungen (SLA-Schutz) mit MVPDs getroffen werden können.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
