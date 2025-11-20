---
title: Unterscheiden zwischen VOD und Live-Inhalten bei der Überwachung von gleichzeitigen Aufrufen
description: Unterscheiden zwischen VOD und Live-Inhalten bei der Überwachung von gleichzeitigen Aufrufen
exl-id: 51ba686a-7c1f-4403-9e8e-cd247bf9e345
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Vorgehensweise: Unterscheiden zwischen VOD und Live-Inhalten bei der gleichzeitigen Überwachung {#dist-vod-live}

**F:** Kann der Concurrency Monitoring-Service zwischen dem Typ von Inhalt unterscheiden, der wiedergegeben wird (Live-Inhalt vs. Video auf Anfrage)?



**A:** Parallelitätsüberwachung kann nicht direkt zwischen Live-Inhalten und Video-on-Demand (VOD) unterscheiden. Der Video-Player muss den Typ des abgespielten Inhalts kennen und diese Informationen während des [Sitzungsinitialisierungsaufrufs) senden ](/help/concurrency-monitoring/api/cm-api-overview.md#session-initial)erforderlich für die gleichzeitige Überwachung). Der reguläre Workflow sieht wie folgt aus:

1. Die Kundinnen und Kunden der Parallelitätsüberwachung definieren einen Satz von Metadaten, für die Regeln implementiert werden sollen (z. B. content-type=live|vod, device-type=mobile|console|desktop).
1. Das Team für Parallelitätsüberwachung implementiert die gewünschte Richtlinie. Beispiel:
   1. Wenn content-type=live, max streams=3, zuletzt gewinnt
   1. Wenn content-type=VOD, max streams=1, zuletzt gewinnt

1. Wenn der Endbenutzer die Inhalte abspielt, muss der Video-Player Werte für die Metadatenfelder senden, die als Teil einer Richtlinie festgelegt wurden.

1. Der Service zur Überwachung von Parallelitäten gibt basierend auf der definierten Richtlinie und den empfangenen Werten eine Entscheidung (Play/Stopp) aus.

1. Die Entscheidung muss vom Video-Player befolgt werden, damit das System funktioniert.



## Ergänzende Informationen {#related-info-vod-live-dist}

* [Standard-Metadatenattribute für die gleichzeitige Überwachung](/help/concurrency-monitoring/technical/standard-metadata-attributes.md)
* [Übersicht über die API zur Parallelitätsüberwachung](/help/concurrency-monitoring/api/cm-api-overview.md)
