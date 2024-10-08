---
title: Berichte zur gleichzeitigen Überwachung der Nutzung
description: Berichte zur gleichzeitigen Überwachung der Nutzung
exl-id: 20220436-e748-4b22-8e7c-e074e0bfe242
source-git-commit: 36da78fd66cfbc86e7bea7575c757fef536c0755
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# Berichte zur gleichzeitigen Überwachung der Nutzung {#cm-usage-reports}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.



## Übersicht {#usage-rep-overview}

Der Dienst **Gebrauchsberichte zur gleichzeitigen Überwachung** ist über eine REST-API verfügbar, die Einblicke in die gleichzeitige Nutzung bietet, die von den Anwendungen des Kunden gemeldet wird.

## Voraussetzungen {#usage-rep-prerequisites}

Um auf das Produkt &quot;Gebrauchsberichte zur Überwachung der Parallelität&quot;zugreifen zu können, muss sich ein Kunde zunächst an das Supportteam zur Überwachung der Parallelität [1} wenden und die erforderlichen Schritte ausführen, um Ihnen den Zugriff auf das API-Produkt zu ermöglichen. ](mailto:tve-support@adobe.com) Weitere Informationen zum [CMU API-Zugriff](/help/concurrency-monitoring/cmu-api-access.md).

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
|:---------------|:------------------------------------------------------------------------------------------------------------------|
| year | Das vierstellige Jahr |
| month | Der Monat des Jahres (1-12) |
| day | Der Tag des Monats (1-31) |
| hour | Die Stunde des Tages |
| minute | Die Minute der Stunde[^1] |
| Applikation | Der Anwendungsname, der unter &quot;Parallelitätsüberwachung&quot;registriert ist und zum Verwalten von Sitzungen verwendet wird |
| application-id | Die in der Parallelitätsüberwachung registrierte Anwendungs-ID, die zum Verwalten von Sitzungen verwendet wird |
| channel | Die Kanalmetadaten, die während der Initialisierung der Sitzung gesendet wurden (als unbekannt markiert markiert, wenn keine Metadaten gesendet wurden) |
| mvpd | Der bei der Sitzungsverwaltung bereitgestellte MVPD |
| platform | Die Plattformmetadaten, die bei der Sitzungsinitialisierung bereitgestellt oder für eine Anwendung auf Konfigurationsebene vordefiniert wurden |

## Metriken und Aufschlüsselungen von gleichzeitigen Berichten {#concurrency-reports-metrics-breakdown}

Ab der Version 2.9.0 der Überwachung der Parallelität haben wir einen neuen Bericht eingeführt, um die gleichzeitige Verwendung zu verstehen: ein Histogramm für **Parallelitätsstufe** und **Aktivitätsebene**.

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
| Parallelitätsstufe | Stellt eine eindeutige **Stream-Aktivität dar, die in der Sitzungsinitialisierungsphase** für einen Benutzer genehmigt wurde, um feststellen zu können, wie viele gleichzeitige Streams **von einem Benutzer geöffnet wurden, und um die Auswirkungen der Anwendung einer bestimmten Parallelitätsbegrenzung zu verstehen.** |
| Aktivitätsebene | Stellt eine beliebige **Stream-Aktivität dar (unabhängig vom Status: gestartet, aktiv, gestoppt, abgelehnt)** für einen Benutzer, um feststellen zu können, wie viele gleichzeitige Streams von einem Benutzer geöffnet werden konnten, und um die Auswirkungen der Anwendung einer bestimmten Parallelitätsbegrenzung zu verstehen. |
| mvpd | Der bei der Sitzungsverwaltung bereitgestellte MVPD |

### Beispiele für Berichte

Um eine optimale Datengenauigkeit zu erzielen, empfehlen wir die auf dieser Seite dargestellten Berichte [Beispiele für CMU-Berichte](/help/concurrency-monitoring/cm-usage-reports-examples.md)

[^1]: Minuale Berichte sind standardmäßig nicht verfügbar. Wenden Sie sich an das Supportteam zur Überwachung der Parallelität [1}, um sie anzufordern.](mailto:tve-support@adobe.com)