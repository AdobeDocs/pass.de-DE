---
title: API-Endpunkte
description: Vollständige Liste der APIs zur Parallelitätsüberwachung
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
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
