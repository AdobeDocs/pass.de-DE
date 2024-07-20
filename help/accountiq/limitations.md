---
title: Einschränkungen
description: Erfahren Sie mehr über Einschränkungen und den Isolationsmodus MVPD für Programmierer in Account IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Einschränkungen {#limitations}

Die D2C- und TV-Überall-Versionen von Account IQ bieten Streaming-Anbietern Analysen zur Nutzung und zum Teilen von Abonnements. Die aktuelle Version 1.3 weist jedoch einige Einschränkungen auf, die in zukünftigen Versionen behoben werden.

* Der [Gesamtwert für die Freigabe](/help/accountiq/data-panels.md#overall-sharing-score) enthält derzeit die [Teilungsebene](/help/accountiq/data-panels.md#sharing-level) und die [Nutzung aus freigegebenen Konten](/help/accountiq/data-panels.md#usage-from-shared-accounts). Zukünftige Versionen werden weitere Metriken enthalten.

* Beim Definieren von Segmenten im Dashboard oder bei Nutzungsmustern können die Berichte [Videokategorien im Segment](/help/accountiq/data-panels.md#video-categories-segment), [Sharing score by channels and MVPDs](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds) und [Usage pattern Distribution for video categories](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) nur Daten für bis zu 20 [Videokategorien](product-concepts.md#video-category-def) anzeigen. Segmente mit mehr als 20 Videokategorien zeigen in diesen Berichten keine Daten an.

* Derzeit ist die Option, Kontostatistiken zu exportieren, auf den Export von 1000 Konten beschränkt.

* Beim Definieren von Vorgängen ist die Option zur Auswahl von [Segmenttyp](/help/accountiq/operations.md#segment) auf **Feste Anzahl von Konten** beschränkt. Die Option **Variablenanzahl Konten** wird in zukünftigen Versionen verfügbar sein.

* Die Abschnitte **Benchmarking**, **Detection Models**, **Actions** und **Settings** im linken Navigationsbereich sind derzeit deaktiviert und werden in einer zukünftigen Version verfügbar sein.

* Beim Erstellen von Vorgängen können Sie nur zwei Arten von [Aktionen](/help/accountiq/operations.md#action) anwenden: Regeln zur Überwachung der Parallelität und externe Aktionen.

* Derzeit können Sie nur [Vorgänge ](/help/accountiq/operations.md#create-new-operation) und [planen](/help/accountiq/operations.md#schedule) erstellen. Zukünftige Versionen werden es Ihnen ermöglichen, sie anzuhalten, wieder aufzunehmen und vollständig zu verwalten.

* Bei der Auswahl von Granularität und Zeitintervall können Sie Daten nur für eine Woche oder einen Monat gleichzeitig analysieren.

* Sie können der Segmentdefinition keine Isolationsmodus-MVPDs mit anderen MVPDs hinzufügen. Einige MVPDs identifizieren Abonnenten nicht eindeutig über mehrere Programmierkanäle hinweg. Daher werden diese MVPDs getrennt für TV-Programmierer behandelt.



