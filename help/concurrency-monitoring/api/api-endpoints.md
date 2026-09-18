---
title: API-Endpunkte
description: Vollständige Liste der APIs zur Parallelitätsüberwachung
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
source-git-commit: 39384d753e7808fa433f30d8dafabd531dbf3acf
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
