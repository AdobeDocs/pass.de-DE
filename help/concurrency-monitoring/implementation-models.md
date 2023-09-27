---
title: Implementierungsmodelle
description: Implementierungsmodelle
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---


# Implementierungsmodelle {#imp-models}

## Serverseitige Richtlinien {#ss-policies}

Dieses Modell nutzt CM als politischen Entscheidungspunkt und delegiert so die Zugriffsentscheidung an den Dienst.

Da der Client keine Annahme in Bezug auf die angewendeten Richtlinien treffen sollte, muss die Implementierung die Entscheidung über die Sitzungsinitialisierung während der Wiedergabe von der Heartbeat-Antwort sowie regelmäßig überprüfen.
