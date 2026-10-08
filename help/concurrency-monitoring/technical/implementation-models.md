---
title: Implementierungsmodelle
description: Implementierungsmodelle
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%
---
# Implementierungsmodelle {#imp-models}

## Server-seitige Richtlinien {#ss-policies}

Dieses Modell verwendet CM als Entscheidungspunkt für Richtlinien und delegiert so die Zugriffsentscheidung an den Service.

Da der Client keine Annahme in Bezug auf die angewendeten Richtlinien treffen sollte, muss die Implementierung die Entscheidung zur Sitzungsinitialisierung während der Wiedergabe über die Heartbeat-Antwort sowohl regelmäßig überprüfen als auch
