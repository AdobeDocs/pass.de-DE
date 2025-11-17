---
title: ESM-Dashboard
description: Erfahren Sie, wie Sie mit dem ESM-Dashboard Berechtigungs- und Ereignisdaten über MVPD-Partner hinweg überwachen können.
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# ESM-Dashboard {#esm-dashboard}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Das ESM-Dashboard bietet eine einheitliche Ansicht der Berechtigungs- und Ereignisdaten, mit der Sie die Leistung überwachen, Anomalien identifizieren und Benutzerzugriffsmuster in allen MVPD-Partnern verstehen können. In diesem Handbuch wird erläutert, wie Sie die Filter des Dashboards verwenden, Berichte interpretieren und Schlüsselmetriken in konfigurierbaren Zeitintervallen verstehen.

![ESM-Dashboard-Ansicht](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## Anwendungsszenarien {#use-cases}

- Visualisieren von Trends pro Plattform oder MVPD
- MVPD-Leistung vergleichen
- Verwendung durch den Kunden pro Anwendung

Weitere Informationen zu ESM-Daten und -Ereignissen finden Sie unter [Übersicht über die Überwachung des Berechtigungsdienstes](https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview).

## Berichte {#reports}

Die folgenden Berichte stehen zur Verfügung:

### Anforderungen abspielen {#play-requests}

Zeigt die Anzahl der Wiedergabeanforderungen für das ausgewählte Zeitintervall an.

**Diagrammstil** - Linie.

### Authentifizierungs-Konvertierung {#authentication-conversion}

Zeigt das Verhältnis zwischen erfolgreichen Authentifizierungsereignissen und der Gesamtzahl der Authentifizierungsversuche.

**Diagrammstil** - horizontaler Balken

### Konvertierung der Autorisierung {#authorization-conversion}

Zeigt das Verhältnis zwischen erfolgreichen Authentifizierungsereignissen und der Gesamtzahl der Authentifizierungsversuche.

**Diagrammstil** - horizontaler Balken

### Autorisierungslatenz {#authorization-latency}

Zeigt die durchschnittliche Latenz (in Millisekunden) für MVPD-Antworten.

**Diagrammstil** - Linie.

### Nutzung von MVPD {#mvpd-usage}

Zeigt einen Vergleich der Anzahl eindeutiger Benutzer pro MVPD.

**Diagrammstil** - Bereich gestapelt.

### Erfolgreiche Authentifizierungen {#successful-authentications}

Zeigt die Gesamtzahl der erfolgreichen Authentifizierungen für das ausgewählte Zeitintervall an.

**Diagrammstil** - Linie.

### Erfolgreiche Berechtigungen {#successful-authorizations}

Zeigt die Gesamtzahl erfolgreicher Autorisierungen (MVPDs „Erlauben“-Antworten) für das ausgewählte Zeitintervall an.

**Diagrammstil** - Linie.

## Aktionen {#actions}

### Anzeigen nach {#view-by}

Für jedes Diagramm gibt es ein Dropdown-Menü „Ansicht nach“, mit dem Sie genau auswählen können, welche Daten angezeigt werden sollen.

- **Aggregat** zeigt die Gesamtdaten an.
- **MVPDs/Kanäle/Plattformen** - zeigt die ausgewählten spezifischen Filter

### Herunterladen {#download}

Sie können die Rohdaten herunterladen:

- **Diagrammdaten als CSV herunterladen** - lädt Daten für ein bestimmtes Diagramm herunter
- **Alle Daten als CSV herunterladen** - Lädt alle ESM-Daten in allen Diagrammen herunter

## Filter {#filters}

Verwenden Sie Filter, um den Datensatz einzugrenzen und die Analyse zu fokussieren. Die folgenden Filter sind verfügbar:

- **Channel**: Umfasst alle verfügbaren Kanäle (Marken)
- **MVPD**: Konzentration auf einen oder mehrere Anbieter
- **Platform**: Web-, Mobil-, vernetztes TV- oder Gerätefamilie

Um einen neuen Filter hinzuzufügen, wählen Sie die Schaltfläche „Filter hinzufügen“.

Auf der Seite „Datensatzfilter“ können Sie benötigte Filter per Drag-and-Drop verschieben.

![ESM-Dashboard-Filter](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

Für jeden Abschnitt können Sie einzelne Filter entfernen oder die gesamte Auswahl löschen.

### Filtertipps {#filter-tips}

- Kombinieren Sie mehrere Filter, um eine Kohorte zu isolieren (z. B. einen MVPD auf einer mobilen Plattform für einen Kanal).
- Keine Filter hinzufügen, um vor dem Drill-in herauszuzoomen und eine Grundlinie zu erstellen.

## Zeitintervalle {#time-intervals}

Kontrollieren Sie das Analysefenster und die Granularität.

![ESM Dashboard-Zeitintervalle](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### Datumsbereich {#date-range}

**Vorgaben**: Heute, Aktuelle Woche, Letzte 7 Tage, Aktueller Monat, Letzte 30 Tage, Letzte 3 Monate, Letzte 6 Monate, Letzte 12 Monate

**Benutzerdefiniert**: Wählen Sie das gewünschte Zeitintervall aus

### Granularität {#granularity}

Täglich/Monatlich
