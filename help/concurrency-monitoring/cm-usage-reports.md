---
title: Berichte zur gleichzeitigen Überwachung der Nutzung
description: Berichte zur gleichzeitigen Überwachung der Nutzung
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '723'
ht-degree: 0%

---


# Berichte zur gleichzeitigen Überwachung der Nutzung {#cm-usage-reports}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.



## Übersicht {#usage-rep-overview}

Die **Berichte zur gleichzeitigen Überwachung der Nutzung** -Dienst ist über eine REST-API verfügbar, die einen Einblick in die gleichzeitige Nutzung bietet, die von den Anwendungen des Kunden gemeldet wird.

## Voraussetzungen {#usage-rep-prerequisites}

Um auf das Produkt &quot;Gebrauchsberichte zur Überwachung der Parallelität&quot;zugreifen zu können, muss sich ein Kunde zunächst an die Überwachung der Parallelität wenden [Supportteam](mailto:tve-support@adobe.com) und führt die erforderlichen Schritte aus, um Ihnen den Zugriff auf das API-Produkt zu ermöglichen.

## Allgemeine Berichtsmetriken und -aufschlüsselungen {#general-rep-metrics-breakdown}

### Verwendungsberichte Benutzer können die folgenden Metriken überwachen:{#monitor-metrics}

| Name | Beschreibung |
|:---|:---|
| active-users | Anzahl unterschiedlicher Benutzerkonten, die während des Intervalls mindestens eine Streaming-Sitzung initiiert haben, aufgeschlüsselt nach Zeitgranularität |
| active-sessions | Anzahl der Streaming-Sitzungen, die Aktivitäten während des Intervalls gemeldet haben (umfasst keine fehlgeschlagenen Versuche, Sitzungen, die eine Abweisungsantwort für den Initialisierungsaufruf erhalten haben) |
| started-sessions | Anzahl der Streaming-Sitzungen, die innerhalb des Intervalls gestartet wurden |
| completed-sessions | Anzahl der Streaming-Sitzungen, die während des Intervalls beendet wurden (entweder explizit oder aufgrund eines Timeouts) |
| fehlgeschlagene Versuche | Anzahl der Sitzungsinitialisierungsversuche, die eine Abweisungsantwort erhalten haben |
| verworfene Sitzungen | Anzahl der Streaming-Sitzungen, die während der Wiedergabe (in Heartbeat) aufgrund einer Richtlinienverletzung beendet wurden. Diese Metrik ist nur dann sinnvoll, wenn eine FIFO- oder last_one_wins-Richtlinie angewendet wird. |
| kill-sessions | Anzahl der Streaming-Sitzungen, die explizit beim Start einer neuen Sitzung beendet wurden (über den Header X-Terminate ) |
| duration_0-15 | Anzahl der Streams mit einer Dauer zwischen 0 und 15 Minuten |
| duration_15-30 | Anzahl der Streams mit einer Dauer zwischen 15 und 30 Minuten |
| duration_30-60 | Anzahl der Streams mit einer Dauer zwischen 30 und 60 Minuten |
| duration_60-120 | Anzahl der Streams mit einer Dauer zwischen 60 und 120 Minuten |
| duration_2h-4h | Anzahl der Streams mit einer Dauer zwischen 2 Stunden (120 Minuten) und 4 Stunden |
| duration_4h-8h | Anzahl der Streams mit einer Dauer zwischen 4 Stunden und 8 Stunden |
| duration_8h-16h | Anzahl der Streams mit einer Dauer zwischen 8 Stunden und 16 Stunden |
| duration_16h-1d | Anzahl der Streams mit einer Dauer zwischen 16 Stunden und 1 Tag (24 Stunden) |
| duration_1d-3d | Anzahl der Streams mit einer Dauer zwischen 1 Tag und 3 Tagen |
| duration_3d-7d | Anzahl der Streams mit einer Dauer zwischen 3 Tagen und 7 Tagen |
| duration_1w-1m | Anzahl der Streams mit einer Dauer zwischen 1 Woche (7 Tage) und 1 Monat (30 Tage) |
| duration_over-1m | Anzahl der Streams mit einer Dauer von über 1 Monat (30 Tage) |

### Verwendungsberichte Benutzer können die oben aufgeführten Metriken nach den folgenden Dimensionen filtern: {#dimensions-2-filter-metrics}

| Dimension Name | Beschreibung |
|:---|:---|
| year | Das vierstellige Jahr |
| month | Der Monat des Jahres (1-12) |
| day | Der Tag des Monats (1-31) |
| hour | Die Stunde des Tages |
| minute | Die Stunde |
| Applikation | Der Anwendungsname, der unter &quot;Parallelitätsüberwachung&quot;registriert ist und zum Verwalten von Sitzungen verwendet wird |
| application-id | Die in der Parallelitätsüberwachung registrierte Anwendungs-ID, die zum Verwalten von Sitzungen verwendet wird |
| channel | Die Kanalmetadaten, die während der Initialisierung der Sitzung gesendet wurden (als unbekannt markiert markiert, wenn keine Metadaten gesendet wurden) |
| mvpd | Der bei der Sitzungsverwaltung bereitgestellte MVPD |
| platform | Die Plattformmetadaten, die bei der Sitzungsinitialisierung bereitgestellt oder für eine Anwendung auf Konfigurationsebene vordefiniert wurden |

## Metriken und Aufschlüsselungen von gleichzeitigen Berichten {#concurrency-reports-metrics-breakdown}

Ab der Concurrency Monitoring-Version 2.9.0 haben wir einen neuen Bericht zum Verständnis der gleichzeitigen Verwendung eingeführt: ein Histogramm für **Parallelitätsstufe** und **Aktivitätsebene**.

Dieser Bericht soll Ihnen vor allem helfen, die Auswirkungen der Festlegung einer Richtlinie mit einer bestimmten Parallelitätsbegrenzung zu verstehen und Ihnen genügend Informationen zu geben, damit Sie entscheiden können, ob Sie die Grenze erhöhen sollten.

### Verwendungsberichte Benutzer können die folgenden Metriken überwachen: {#metrics-usage-rep-users}

| Dimension Name | Beschreibung |
|:---|:---|
| Benutzer | Anzahl der Benutzer, die jede Concurrent/Activity-Ebene erreicht haben |

### Verwendungsberichte Benutzer können die oben aufgeführten Metriken nach den folgenden Dimensionen filtern: {#dimensions-to-filter-metrics}

| Dimension Name | Beschreibung |
|:---|:---|
| year | Das vierstellige Jahr |
| month | Der Monat des Jahres (1-12) |
| day | Der Tag des Monats (1-31) |
| Parallelitätsstufe | Bezeichnet eine beliebige Unterscheidung **Stream-Aktivität, die in der Sitzungsinitialisierungsphase genehmigt wurde** für einen Benutzer, um feststellen zu können, wie viele gleichzeitige Streams **wurden geöffnet** durch einen Benutzer und um die Auswirkungen der Anwendung einer bestimmten Parallelitätsbegrenzung zu verstehen. |
| Aktivitätsebene | Bezeichnet eine beliebige Unterscheidung **Stream-Aktivität (unabhängig von ihrem Status: gestartet, aktiv, gestoppt, abgelehnt)** für einen Benutzer, um feststellen zu können, wie viele gleichzeitige Streams von einem Benutzer geöffnet werden sollten, und um zu verstehen, wie sich die Anwendung einer bestimmten Parallelitätsbegrenzung auswirkt |
| mvpd | Der bei der Sitzungsverwaltung bereitgestellte MVPD |