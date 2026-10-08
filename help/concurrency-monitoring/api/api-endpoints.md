---
title: API-Endpunkte
description: Vollständige Liste der APIs zur Parallelitätsüberwachung
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%
---
# API-Endpunkte

## Core Session Management

| Endpunkt | Methode | Beschreibung |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | Erstellen einer neuen Streaming-Sitzung |
| `/sessions/{idp}/{subject}/{session}` | POST | Heartbeat senden, um Sitzung am Leben zu halten |
| `/sessions/{idp}/{subject}/{session}` | DELETE | Beenden einer Sitzung |
| `/runningStreams/{idp}/{subject}` | GET | Alle aktiven Sitzungen für ein Thema abrufen |

## Metadatenverwaltung

| Endpunkt | Methode | Beschreibung |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | Erforderliche Metadatenfelder für die Anwendung abrufen |
