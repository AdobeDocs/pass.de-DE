---
title: Richtlinie zur Datenaufbewahrung
description: Richtlinie zur Datenaufbewahrung
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Richtlinie zur Datenaufbewahrung {#data-retention-policy}

>[!WARNING]
>
>**Hinweis:** Der Inhalt auf dieser Seite wird nur zu Informationszwecken bereitgestellt. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Einführung {#introduction}

Adobe muss in seiner Rolle als Datenverarbeiter geeignete Maßnahmen ergreifen, um seine Kunden bei der Erfüllung von Zugriffs-, Lösch- und anderen Anfragen von Einzelpersonen zu unterstützen. Die Anwendung geeigneter, sicherer und zeitnaher Löschrichtlinien ist ein wichtiger Teil der Erfüllung dieser Verpflichtung.

## Definitionen {#definitions}

Eine Richtlinie zur Datenaufbewahrung bestimmt, wie lange Adobe Kundendaten speichert. Die standardmäßige Datenaufbewahrungsrichtlinie für die Überwachung der Parallelität ist **25 Monate**.

| Datenaufbewahrungszeitraum | Der Datenaufbewahrungszeitraum ist der standardmäßige Datenaufbewahrungszeitraum (25 Monate). |
|---|---|
| **Fenster zur Datenaufbewahrung** | Im Fenster zur Datenaufbewahrung werden die Parameter definiert, für die vollständige Daten angezeigt und in Berichten aufgeführt werden können. Das Datenspeicherungsfenster wird wie folgt bestimmt:<br/> *Startdatum* = aktuelles Datum - Datenaufbewahrungszeitraum <br/>*Enddatum* = aktuelles Datum |

## Datenerfassung {#data-collection}

*Clickstream-Daten* stellen Daten dar, die von Kunden in den Sitzungs-Heartbeats freigegeben werden (z. B. subjectID, mvpdName und Metadaten). Alle benutzerdefinierten Metadatenfelder werden in den [Standard-Metadatenattributen](/help/concurrency-monitoring/standard-metadata-attributes.md) referenziert.

## Kundentypen {#customer-types}

### Aktuelle Kunden {#current-customers}

Sofern der Kunde keine Datenaufbewahrungserweiterungen erwirbt, erfüllt die Überwachung der Gleichzeitigkeit die folgenden Anforderungen an die Kundendatenaufbewahrung:

* *Clickstream-Daten*, die von der gleichzeitigen Überwachung erfasst werden, müssen spätestens **25 Monate** ab dem Datum der Erfassung gelöscht werden.

### Terminierte Kunden {#terminated-customers}

Ein beendeter Kunde ist ein Kunde, der die Beziehung zu Adobe beendet hat und die gleichzeitige Überwachung nicht mehr verwendet.

* *Clickstream-Daten*, die von der gleichzeitigen Überwachung erfasst werden, müssen innerhalb von **6 Monaten** ab dem Datum der Vertragsbeendigung gelöscht werden.

## Datenlöschung {#data-deletion}

Adobe behält sich das Recht vor, Daten für Daten, die über die Datenaufbewahrungsdauer hinausgehen, ohne Wiederherstellungsoption zu löschen. Daten aktueller Kunden sollten monatlich rollierend gelöscht werden.
