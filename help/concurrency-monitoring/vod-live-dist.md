---
title: Unterscheiden von VOD- und Live-Inhalten in der gleichzeitigen Überwachung
description: Unterscheiden von VOD- und Live-Inhalten in der gleichzeitigen Überwachung
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# Wie: Unterscheidung zwischen VOD- und Live-Inhalten in der gleichzeitigen Überwachung {#dist-vod-live}

**F:** Kann der Dienst für die Überwachung der Parallelität zwischen dem Typ des wiedergegebenen Inhalts (Live-Inhalt vs. Video On-Demand) unterscheiden?



**A:** Die gleichzeitige Überwachung kann nicht direkt zwischen Live-Inhalt und Video On-Demand (VOD) unterscheiden. Der Videoplayer muss den Typ des wiedergegebenen Inhalts kennen und diese Informationen während der [Sitzungsinitialisierungsaufruf](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (erforderlich für die Überwachung der Parallelität). Der reguläre Workflow sieht wie folgt aus:

1. Die Kunden von Concurrency Monitoring definieren einen Satz von Metadaten, für die Regeln implementiert werden sollen (z. B. content-type=live|vod, device-type=mobile|console|desktop).
1. Das Team für die Überwachung der Parallelität implementiert die gewünschte Richtlinie. Beispiel:
   1. if content-type=live, max streams=3, latest wins
   1. if content-type=vod, max streams=1, latest wins

1. Wenn der Endbenutzer den Inhalt wiedergibt, muss der Videoplayer Werte für die Metadatenfelder senden, die als Teil einer Richtlinie erstellt wurden.

1. Der Dienst für die Überwachung der Parallelität gibt basierend auf der definierten Richtlinie und den empfangenen Werten eine Entscheidung aus (Wiedergabe/Stopp).

1. Die Entscheidung muss vom Videoplayer befolgt werden, damit das System funktioniert.



## Verwandte Informationen {#related-info-vod-live-dist}

* [Gleichzeitige Überwachung von Standard-Metadatenattributen](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [Übersicht über die Context Monitoring-API](/help/concurrency-monitoring/cm-api-overview.md)
