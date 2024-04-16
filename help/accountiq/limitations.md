---
title: Einschränkungen
description: Erfahren Sie mehr über Einschränkungen und den Isolationsmodus MVPD für Programmierer in Konto IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Einschränkungen {#limitations}

Die D2C- und TV-Überall-Versionen von Account IQ bieten Streaming-Anbietern Analysen zur Nutzung und zum Teilen von Abonnements. Die aktuelle Version 1.3 weist jedoch einige Einschränkungen auf, die in zukünftigen Versionen behoben werden.

* Die [Gesamte Teilungsbewertung](/help/accountiq/data-panels.md#overall-sharing-score) derzeit beinhaltet [Freigabestufe](/help/accountiq/data-panels.md#sharing-level) und [Nutzung durch freigegebene Konten](/help/accountiq/data-panels.md#usage-from-shared-accounts). Zukünftige Versionen werden weitere Metriken enthalten.

* Beim Definieren von Segmenten im Dashboard oder bei Nutzungsmustern wird die [Videokategorien im Segment](/help/accountiq/data-panels.md#video-categories-segment), [Teilungsbewertung nach Kanälen und MVPDs](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds), und [Verteilung der Nutzungsmuster für Videokategorien](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) -Berichte können nur Daten für bis zu 20 anzeigen [Videokategorien](product-concepts.md#video-category-def). Segmente mit mehr als 20 Videokategorien zeigen in diesen Berichten keine Daten an.

* Derzeit ist die Option, Kontostatistiken zu exportieren, auf den Export von 1000 Konten beschränkt.

* Bei der Definition von Vorgängen wird die Option zur Auswahl [Segmenttyp](/help/accountiq/operations.md#segment) beschränkt auf **Feste Anzahl von Konten**. Die **Variablenanzahl Konten** in künftigen Versionen verfügbar sein.

* Die **Benchmarking**, **Erkennungsmodelle**, **Aktionen**, und **Einstellungen** -Abschnitte im linken Navigationsbereich sind derzeit deaktiviert und werden in einer zukünftigen Version verfügbar sein.

* Beim Erstellen von Vorgängen können Sie nur zwei Arten von [Aktionen](/help/accountiq/operations.md#action) — Regeln zur Überwachung der Parallelität und externe Maßnahmen.

* Derzeit können Sie [erstellen](/help/accountiq/operations.md#create-new-operation) und [Zeitplan](/help/accountiq/operations.md#schedule) Vorgänge. Zukünftige Versionen werden es Ihnen ermöglichen, sie anzuhalten, wieder aufzunehmen und vollständig zu verwalten.

* Bei der Auswahl von Granularität und Zeitintervall können Sie Daten nur für eine Woche oder einen Monat gleichzeitig analysieren.

* Sie können der Segmentdefinition keine Isolationsmodus-MVPDs mit anderen MVPDs hinzufügen. Einige MVPDs identifizieren Abonnenten nicht eindeutig über mehrere Programmierkanäle hinweg. Daher werden diese MVPDs getrennt für TV-Programmierer behandelt.



