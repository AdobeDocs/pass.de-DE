---
title: Einschränkungen
description: Erfahren Sie mehr über Einschränkungen und den Isolationsmodus für MVPD für Programmierer in Account IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Einschränkungen {#limitations}

Die Account IQ-Versionen D2C und TV Everywhere bieten Streaming-Anbietern Analysen zur Nutzung und zur Freigabe von Abonnements. Die aktuelle Version 1.3 weist jedoch einige Einschränkungen auf, die in zukünftigen Versionen behoben werden.

* Der [Gesamtwert für die Freigabe](/help/accountiq/data-panels.md#overall-sharing-score) umfasst derzeit [Freigabeebene](/help/accountiq/data-panels.md#sharing-level) und [Nutzung aus freigegebenen Konten](/help/accountiq/data-panels.md#usage-from-shared-accounts). Zukünftige Versionen werden weitere Metriken enthalten.

* Beim Definieren von Segmenten im Dashboard oder von Nutzungsmustern können in den Berichten [Videokategorien im Segment](/help/accountiq/data-panels.md#video-categories-segment), [Freigabe des Scores nach Kanälen und MVPDs](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds) und [Verteilung von Nutzungsmustern für Videokategorien](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) nur Daten für bis zu 20 [Videokategorien](product-concepts.md#video-category-def) angezeigt werden. Segmente mit mehr als 20 Videokategorien zeigen keine Daten in diesen Berichten an.

* Derzeit ist die Option zum Exportieren von Kontostatistiken auf den Export von 1000 Konten beschränkt.

* Bei der Definition von Vorgängen ist die Option [Segmenttyp](/help/accountiq/operations.md#segment) auf „Feste **von Konten“**. Die Option **Variable Anzahl von Konten** wird in zukünftigen Versionen verfügbar sein.

* Die **Benchmarking**, **Erkennungsmodelle**, **Aktionen** und **Einstellungen** im linken Navigationsbereich sind derzeit deaktiviert und werden in einer zukünftigen Version verfügbar sein.

* Beim Erstellen von Vorgängen können Sie nur zwei Arten von [Aktionen](/help/accountiq/operations.md#action) anwenden: Regeln zur Überwachung von Parallelitäten und externe Aktionen.

* Derzeit können Sie nur Vorgänge [erstellen](/help/accountiq/operations.md#create-new-operation) und [planen](/help/accountiq/operations.md#schedule). Zukünftige Versionen werden es Ihnen ermöglichen, diese zu pausieren, fortzusetzen und vollständig zu verwalten.

* Sie können Daten jeweils nur für eine Woche oder einen Monat analysieren, wenn Sie Granularität und Zeitintervall auswählen.

* Mit keinem anderen MVPD können Sie der Segmentdefinition MVPDs im Isolationsmodus hinzufügen. Einige MVPDs identifizieren Abonnentinnen und Abonnenten nicht eindeutig über mehrere Programmierkanäle hinweg. Daher werden diese MVPDs für TV Everywhere-Programmierer separat behandelt.



