---
title: Versionshinweise zur Adobe Parallelüberwachung 2.9
description: Versionshinweise zur Adobe Parallelüberwachung 2.9
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Versionshinweise zur Überwachung der Parallelität 2.9 {#rn-cm29}

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben.

## Veröffentlichungsdatum {#release-date}

03/14/2019


## Versionsübersicht {#release-overview}

* Ab dieser Version haben wir einen neuen Bericht zum Verständnis der gleichzeitigen Verwendung eingeführt: ein Histogramm für die Parallelitätsstufe, das Folgendes anzeigt:

* die Anzahl der Benutzer, die bei jedem Granularitätsintervall die einzelnen Parallelitätsstufen erreicht haben (d. h. wie viele Benutzer je zwei gleichzeitige Streams, drei gleichzeitige Streams usw. gehabt haben).
* die Gesamtdauer für jede gleichzeitige Ebene in Minuten (der Durchschnittswert kann berechnet werden, indem dieser Wert einfach durch die obige Anzahl geteilt wird)
* die Gesamtzahl der Fälle, in denen Benutzer bei jeder gleichzeitigen Ebene aufgetreten sind, um die Auswirkungen einer bestimmten Regel sowohl in Bezug auf die betroffenen Benutzer als auch in Bezug auf das aggregierte Benutzererlebnis abzuschätzen. Weitere Informationen finden Sie im Abschnitt [Nutzungsberichte](/help/concurrency-monitoring/cm-usage-reports.md) Seite.

Wir haben auch den SQL Injection-Schutz verbessert und mehrere Fehlerbehebungen hinzugefügt.

## Bekannte Probleme {#known-issues}

Keine.
