---
title: Versionshinweise zu Adobe Concurrency Monitoring 2.9
description: Versionshinweise zu Adobe Concurrency Monitoring 2.9
exl-id: fd793b1f-b704-492b-850c-dae6478b575a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 3%
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
