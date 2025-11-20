---
title: Implementierungsmodelle
description: Implementierungsmodelle
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# Implementierungsmodelle {#imp-models}

## Server-seitige Richtlinien {#ss-policies}

Dieses Modell verwendet CM als Entscheidungspunkt für Richtlinien und delegiert so die Zugriffsentscheidung an den Service.

Da der Client keine Annahme in Bezug auf die angewendeten Richtlinien treffen sollte, muss die Implementierung die Entscheidung zur Sitzungsinitialisierung während der Wiedergabe über die Heartbeat-Antwort sowohl regelmäßig überprüfen als auch
