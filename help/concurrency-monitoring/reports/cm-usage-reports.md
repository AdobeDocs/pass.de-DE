---
title: Berichte zur Nutzung der gleichzeitigen Überwachung
description: Berichte zur Nutzung der gleichzeitigen Überwachung
exl-id: 20220436-e748-4b22-8e7c-e074e0bfe242
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# Berichte zur Nutzung der gleichzeitigen Überwachung {#cm-usage-reports}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.



## Überblick {#usage-rep-overview}

Der Service **Nutzungsberichte zur gleichzeitigen Überwachung** ist über eine REST-API verfügbar, die insight zur gleichzeitigen Nutzung bereitstellt, wie von den Programmen des Kunden gemeldet.

## Voraussetzungen {#usage-rep-prerequisites}

Um auf das Produkt „Nutzungsberichte zur Parallelitätsüberwachung“ zugreifen zu können, muss sich ein Kunde zunächst an das [Supportteam“ wenden ](mailto:tve-support@adobe.com) führt die erforderlichen Schritte aus, um Ihnen Zugriff auf das API-Produkt zu ermöglichen. Weitere Informationen finden Sie unter [CMU-API-Zugriff](/help/concurrency-monitoring/reports/cmu-api-access.md).

## Allgemeine Berichtsmetriken und Aufschlüsselungen {#general-rep-metrics-breakdown}

### Usage Reports : Benutzer können die folgenden Metriken überwachen:{#monitor-metrics}

| -Name | Beschreibung |
|:---|:---|
| active-users | Anzahl unterschiedlicher Benutzerkonten, die während des Intervalls mindestens eine Streaming-Sitzung initiiert haben, aufgeschlüsselt nach Zeitgranularität |
| active-sessions | Anzahl der Streaming-Sitzungen, die während des Intervalls eine Aktivität gemeldet haben (es enthält keine fehlgeschlagenen Versuche, Sitzungen, die eine Ablehnungsantwort für den Initialisierungsaufruf erhalten haben) |
| started-sessions | Anzahl der Streaming-Sitzungen, die innerhalb des Intervalls gestartet wurden |
| completed-sessions | Anzahl der Streaming-Sitzungen, die während des Intervalls abgeschlossen wurden (entweder explizit oder aufgrund einer Zeitüberschreitung) |
| failed-attempts | Anzahl der Sitzungsinitialisierungsversuche, bei denen eine Antwort vom Typ „Ablehnen“ empfangen wurde |
| disjected-sessions | Anzahl der Streaming-Sitzungen, die während der Wiedergabe (im Heartbeat) aufgrund eines Richtlinienverstoßes beendet wurden. Diese Metrik ist nur dann sinnvoll, wenn eine FIFO- oder last_one_wins-Richtlinie umgesetzt wird. |
| failed-sessions | Anzahl der Streaming-Sitzungen, die beim Starten einer neuen Sitzung explizit beendet wurden (über den Header „X-Terminate„) |
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
| duration_over-1m | Anzahl der Streams mit einer Dauer über 1 Monat (30 Tage) |

### Benutzende von Nutzungsberichten können die oben aufgeführten Metriken nach den folgenden Dimensionen filtern: {#dimensions-2-filter-metrics}

| Dimension-Name | Beschreibung |
|:---------------|:------------------------------------------------------------------------------------------------------------------|
| Jahr | Das 4-stellige Jahr |
| Monat | Der Monat des Jahres (1-12) |
| Tag | Der Tag des Monats (1-31) |
| Stunde | Die Stunde des Tages |
| Minute | Die Minute der Stunde[^1] |
| Anwendung | Der bei der gleichzeitigen Überwachung registrierte Anwendungsname, der zur Verwaltung von Sitzungen verwendet wird |
| application-id | Die bei der gleichzeitigen Überwachung registrierte Anwendungs-ID, die zur Verwaltung von Sitzungen verwendet wird |
| Kanal | Die Kanalmetadaten, die bei der Initialisierung der Sitzung gesendet werden (werden als unbekannt markiert, wenn keine Metadaten gesendet werden) |
| mvpd | Die bei der Sitzungsverwaltung bereitgestellte MVPD |
| Plattform | Die Platform-Metadaten, die bei der Sitzungsinitialisierung bereitgestellt oder für eine Anwendung auf Konfigurationsebene vordefiniert werden |

## Metriken und Aufschlüsselungen des Parallelitätsberichts {#concurrency-reports-metrics-breakdown}

Seit Einführung der Parallelitätsüberwachung Version 2.9.0 haben wir einen neuen Bericht zum Verständnis der gleichzeitigen Nutzung eingeführt: ein Histogramm für **Gleichzeitigkeitsstufe** und **Aktivitätsebene**.

Der Hauptzweck dieses Berichts besteht darin, Ihnen zu helfen, die Auswirkungen der Festlegung einer Richtlinie mit bestimmten Parallelitätsbeschränkungen zu verstehen und Ihnen genügend Informationen zur Verfügung zu stellen, um zu entscheiden, ob Sie das Limit erhöhen sollten.

### Usage Reports : Benutzer können die folgenden Metriken überwachen: {#metrics-usage-rep-users}

| Dimension-Name | Beschreibung |
|:---|:---|
| Benutzer | Anzahl der Benutzer, die jede Parallelitäts-/Aktivitätsebene erreicht haben |

### Benutzende von Nutzungsberichten können die oben aufgeführten Metriken nach den folgenden Dimensionen filtern: {#dimensions-to-filter-metrics}

| Dimension-Name | Beschreibung |
|:---|:---|
| Jahr | Das 4-stellige Jahr |
| Monat | Der Monat des Jahres (1-12) |
| Tag | Der Tag des Monats (1-31) |
| Gleichzeitigkeitsgrad | Stellt eine bestimmte **Stream-Aktivität dar, die in der Sitzungsinitialisierungsphase genehmigt wurde** für einen Benutzer, um festzustellen, wie viele gleichzeitige Streams **von einem Benutzer geöffnet** und wie sich die Anwendung einer bestimmten Gleichzeitigkeitsgrenze auswirkt |
| Aktivitätsebene | Stellt eine bestimmte **Stream-Aktivität (unabhängig vom Status: gestartet, aktiv, gestoppt, abgelehnt)** für einen Benutzer dar, um festzustellen, wie viele gleichzeitige Streams von einem Benutzer geöffnet werden sollten, und um die Auswirkungen der Anwendung einer bestimmten Gleichzeitigkeitsgrenze zu verstehen |
| mvpd | Die bei der Sitzungsverwaltung bereitgestellte MVPD |

### Beispiele für Berichte

Um eine optimale Datengenauigkeit zu erzielen, empfehlen wir die auf dieser Seite vorgestellten Berichte [Beispiele für CMU-Berichte](/help/concurrency-monitoring/reports/cm-usage-reports-examples.md)

[^1]: Minimal verfügbare Berichte sind standardmäßig nicht verfügbar. Bitte wenden Sie sich an das [ (Support-Team](mailto:tve-support@adobe.com), um sie anzufordern.