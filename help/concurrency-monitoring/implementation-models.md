---
title: Implementierungsmodelle
description: Implementierungsmodelle
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# Implementierungsmodelle {#imp-models}

## Serverseitige Richtlinien {#ss-policies}

Dieses Modell nutzt CM als politischen Entscheidungspunkt und delegiert so die Zugriffsentscheidung an den Dienst.

Da der Client keine Annahme in Bezug auf die angewendeten Richtlinien treffen sollte, muss die Implementierung die Entscheidung über die Sitzungsinitialisierung während der Wiedergabe von der Heartbeat-Antwort sowie regelmäßig überprüfen.
