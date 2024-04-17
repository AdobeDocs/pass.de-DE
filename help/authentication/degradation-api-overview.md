---
title: Übersicht über die Abbau-API
description: Übersicht über die Abbau-API
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: f918d7f9f7b2af5b4364421f6703211e413eafb4
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# Übersicht über die Abbau-API {#degradation-api-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.
>Um die Abbauungs-API verwenden zu können, müssen Sie:
>- Bitten Sie das Supportteam um eine Softwareanweisung für Ihre registrierte Anwendung
>- Abrufen eines Zugriffstokens basierend auf [Dynamische Kundenregistrierung](dynamic-client-registration.md)
> 

>[!NOTE]
>
>Um die Abbauungs-API verwenden zu können, müssen Sie:
>- Bitten Sie das Supportteam um eine Softwareanweisung für Ihre registrierte Anwendung
>- Abrufen eines Zugriffstokens basierend auf [Dynamische Kundenregistrierung](dynamic-client-registration.md)
> 

## Allgemeine Informationen {#general_info}

>[!NOTE]
>
>Diese API ist nicht allgemein verfügbar. Wenden Sie sich an Ihren Adobe-Support-Mitarbeiter, um Updates der Verfügbarkeit zu erhalten.

Diese Funktion bietet allen drei Hauptparteien in einer Integration (Programmierer, MVPDs und Adobe) die Möglichkeit, bestimmte MVPD-Authentifizierungs- und Autorisierungsendpunkte vorübergehend zu umgehen. In der Regel ist es der Programmierer, der eine solche Aktion auslöst, aber unabhängig davon, wer ein Abbauereignis Trigger, hängt die Maßnahme von zuvor vereinbarten Vereinbarungen mit den betroffenen MVPDs ab.

Der Hauptanwendungsfall für diese Funktion tritt bei Live-Sportveranstaltungen oder großen Veranstaltungen auf. In solchen Szenarien mit hohem Traffic ist es möglich, dass die Auslastung eines bestimmten MVPD-Endpunkts zu hoch wird, was zu sehr langen Reaktionszeiten für Benutzer führt. Um während eines solchen Szenarios ein gutes Benutzererlebnis zu gewährleisten, kann der Programmierer entscheiden, eine Abbauregel Trigger, die Benutzer vorübergehend automatisch authentifizieren/automatisch autorisieren kann, oder eine MVPD zu deaktivieren, indem er sie aus der verfügbaren MVPDs-Liste entfernt.

Eine Abbauregel wird nur für einen bestimmten Zeitraum angewendet. Obwohl die Hauptkunden dieser Funktion Sportkanäle und Live-Nachrichtenkanäle sind, möchte jeder Programmierer möglicherweise Zugriff auf diese Funktion haben, da die MVPDs-Dienste von Zeit zu Zeit zurückgehen.

Notizen zur Verschlechterung:

- Diese Funktion ist für die Verwendung mit der API zur Nutzungsüberwachung konzipiert, die Echtzeitinformationen über die Anzahl der Authentifizierungen und Berechtigungen pro MVPD, die durchschnittliche Autorisierungslatenz und andere Metriken bereitstellt, die für eine vollständige Übersicht über den Service erforderlich sind.
- Diese Funktion lässt die Umgehung des Adobe Primetim-Authentifizierungsdienstes nicht zu. Wenn die Adobe Pass-Authentifizierung deaktiviert ist, gibt es keinen Mechanismus innerhalb des Dienstes, der verwendet werden kann, um Benutzern das Anzeigen von Inhalten zu ermöglichen. Die Sites oder Apps können jedoch allein die Adobe Pass-Authentifizierung umleiten.
- Adobe wird derzeit nicht direkt die Verschlechterung des Triggers bewirken - die Entscheidung muss immer bei einem bestimmten Programmierer liegen, der mit MVPDs diesen Bedingungen zugestimmt hat. In Zukunft könnte die Adobe Pass-Authentifizierung proaktiv Abbauregeln auslösen, wenn mit MVPDs Vereinbarungen (SLA-Schutz) getroffen werden können.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
