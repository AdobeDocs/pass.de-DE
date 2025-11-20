---
title: Versionshinweise zu Adobe Concurrency Monitoring 2.9
description: Versionshinweise zu Adobe Concurrency Monitoring 2.9
exl-id: fd793b1f-b704-492b-850c-dae6478b575a
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---

# Versionshinweise zur Parallelitätsüberwachung 2.9 {#rn-cm29}

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben.

## Veröffentlichungsdatum {#release-date}

03/14/2019


## Versionsübersicht {#release-overview}

* Mit dieser Version haben wir einen neuen Bericht zum Verständnis der gleichzeitigen Nutzung eingeführt: ein Histogramm für die Parallelitätsstufe, das Folgendes zeigt:

* Die Anzahl der Benutzenden, die jede Gleichzeitigkeitsstufe (d. h. wie viele Benutzende je zwei gleichzeitige Streams, drei gleichzeitige Streams usw.) während jedes Granularitätsintervalls erreicht haben
* Die Gesamtdauer für jede Parallelitätsstufe in Minuten (der Durchschnittswert kann berechnet werden, indem dieser Wert einfach durch die obige Zahl dividiert wird)
* Die Gesamtzahl der Fälle, in denen Benutzer auf jede Parallelitätsstufe gestoßen sind, um die Auswirkungen einer bestimmten Regel sowohl in Bezug auf betroffene Benutzer als auch auf das aggregierte Benutzererlebnis zu schätzen
Weitere Informationen finden Sie auf der Seite [Nutzungsberichte](/help/concurrency-monitoring/reports/cm-usage-reports.md).

Außerdem wurde der SQL-Injection-Schutz verbessert und es wurden mehrere Fehlerbehebungen hinzugefügt.

## Bekannte Probleme {#known-issues}

Keine.
