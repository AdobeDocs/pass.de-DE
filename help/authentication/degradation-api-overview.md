---
title: Übersicht über die Abbau-API
description: Übersicht über die Abbau-API
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 95c3b1cbce4a591ce387ae3b242721e50ba2ddb1
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Übersicht über die Abbau-API {#degradation-api-overview}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Stellen Sie vor Verwendung der Abbauungs-API sicher, dass die folgenden Voraussetzungen erfüllt sind:
>
> * Rufen Sie die Client-Anmeldeinformationen ab, wie in der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) beschrieben.
> * Rufen Sie das Zugriffstoken ab, wie in der API-Dokumentation [Zugriffstoken abrufen](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben.
>
> Weitere Informationen zum Erstellen einer registrierten Anwendung und Herunterladen der Softwareanweisung finden Sie in der Dokumentation zur [Übersicht über die dynamische Client-Registrierung](./dcr-api/dynamic-client-registration-overview.md) .

## Allgemeine Informationen {#general_info}

>[!NOTE]
>
>Diese API ist nicht allgemein verfügbar. Wenden Sie sich an Ihren Adobe-Support-Mitarbeiter, um Updates der Verfügbarkeit zu erhalten.

Diese Funktion bietet allen drei Hauptparteien in einer Integration (Programmierer, MVPDs und Adobe) die Möglichkeit, bestimmte MVPD-Authentifizierungs- und Autorisierungsendpunkte vorübergehend zu umgehen. Normalerweise ist es der Programmierer, der eine solche Aktion auslöst, aber unabhängig davon, wer ein Degradation-Ereignis Trigger, hängt die Maßnahme von den zuvor vereinbarten Vereinbarungen mit den betroffenen MVPDs ab.

Der Hauptanwendungsfall für diese Funktion tritt bei Live-Sportveranstaltungen oder großen Veranstaltungen auf. In solchen Szenarien mit hohem Traffic ist es möglich, dass die Auslastung eines bestimmten MVPD-Endpunkts zu hoch wird, was zu sehr langen Reaktionszeiten für Benutzer führt. Um während eines solchen Szenarios ein gutes Benutzererlebnis zu gewährleisten, kann der Programmierer entscheiden, eine Abbauregel Trigger, die Benutzer vorübergehend automatisch authentifizieren/automatisch autorisieren kann, oder eine MVPD zu deaktivieren, indem er sie aus der verfügbaren MVPDs-Liste entfernt.

Eine Abbauregel wird nur für einen bestimmten Zeitraum angewendet. Obwohl die Hauptkunden dieser Funktion Sportkanäle und Live-Nachrichtenkanäle sind, möchte jeder Programmierer möglicherweise Zugriff auf diese Funktion haben, da die MVPDs-Dienste von Zeit zu Zeit zurückgehen.

Notizen zur Verschlechterung:

- Diese Funktion ist für die Verwendung mit der API zur Nutzungsüberwachung konzipiert, die Echtzeitinformationen über die Anzahl der Authentifizierungen und Berechtigungen pro MVPD, die durchschnittliche Autorisierungslatenz und andere Metriken bereitstellt, die für eine vollständige Übersicht über den Service erforderlich sind.
- Diese Funktion lässt das Umgehen des Adobe Pass-Authentifizierungsdienstes nicht zu. Wenn die Adobe Pass-Authentifizierung deaktiviert ist, gibt es keinen Mechanismus innerhalb des Dienstes, der verwendet werden kann, um Benutzern das Anzeigen von Inhalten zu ermöglichen. Die Sites oder Apps können jedoch allein die Adobe Pass-Authentifizierung umleiten.
- Adobe wird derzeit nicht direkt die Verschlechterung des Triggers bewirken - die Entscheidung muss immer bei einem bestimmten Programmierer liegen, der mit MVPDs diesen Bedingungen zugestimmt hat. In Zukunft könnte die Adobe Pass-Authentifizierung proaktiv dazu beitragen, Abbauregeln auszulösen, wenn mit MVPDs Vereinbarungen (SLA-Schutz) getroffen werden können.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
