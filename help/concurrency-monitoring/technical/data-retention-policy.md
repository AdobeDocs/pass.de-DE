---
title: Richtlinie zur Datenaufbewahrung
description: Richtlinie zur Datenaufbewahrung
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# Richtlinie zur Datenaufbewahrung {#data-retention-policy}

>[!WARNING]
>
>**Hinweis:** Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.


## Einführung {#introduction}

Adobe muss in seiner Rolle als Auftragsverarbeiter geeignete Maßnahmen ergreifen, um seine Kunden bei der Erfüllung von Zugriffs-, Löschungs- und anderen Anfragen von Einzelpersonen zu unterstützen. Die Anwendung angemessener, sicherer und zeitnaher Richtlinien zur Löschung ist ein wichtiger Bestandteil der Einhaltung dieser Verpflichtung.

## Definitionen {#definitions}

Eine Richtlinie zur Datenaufbewahrung bestimmt, wie lange Adobe die Kundendaten speichert. Die standardmäßige Richtlinie zur Datenaufbewahrung für die gleichzeitige Überwachung beträgt **25 Monate**.

| Aufbewahrungsfrist für Daten | Die Datenspeicherungsdauer ist die standardmäßige Datenspeicherungsdauer (25 Monate). |
|---|---|
| **Datenaufbewahrungsfenster** | Das Fenster Datenaufbewahrung definiert die Parameter, für die vollständige Daten angezeigt und gemeldet werden können. Das Datenspeicherungsfenster wird wie folgt bestimmt:<br/> *Startdatum* = aktuelles Datum - Datenspeicherungszeitraum <br/>*Enddatum* = aktuelles Datum |

## Datenerfassung {#data-collection}

*Clickstream-Daten* stellen Daten dar, die von Kundinnen und Kunden in den Sitzungs-Heartbeats freigegeben werden (z. B. subjectID, mvpdName und Metadaten). Alle benutzerdefinierten Metadatenfelder werden in den [Standard-Metadatenattributen“ &#x200B;](/help/concurrency-monitoring/technical/standard-metadata-attributes.md).

## Kundentypen {#customer-types}

### Aktuelle Kunden {#current-customers}

Sofern der Kunde keine Datenaufbewahrungserweiterungen erwirbt, erfüllt die gleichzeitige Überwachung die folgenden Anforderungen an die Kundendatenaufbewahrung:

* *Clickstream-Daten* die von Concurrency Monitoring erfasst werden, müssen spätestens **25 Monate** ab dem Datum der Erfassung gelöscht werden.

### Beendet gegangene Kunden {#terminated-customers}

Ein beendeter Kunde ist ein Kunde, der die Beziehung mit Adobe beendet hat und die gleichzeitige Überwachung nicht mehr verwendet.

* *Clickstream-Daten* die von Concurrency Monitoring erfasst werden, müssen innerhalb von **6 Monaten** Vertragsende des Kunden gelöscht werden.

## Löschen von Daten {#data-deletion}

Adobe behält sich das Recht vor, Daten für Zeiträume zu löschen, die über die Datenspeicherungsfrist hinausgehen, ohne dass eine Option zur Wiederherstellung besteht. Daten für aktuelle Kunden sollten auf rollierender monatlicher Basis gelöscht werden.
