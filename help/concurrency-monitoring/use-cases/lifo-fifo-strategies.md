---
title: LIFO vs. FIFO-Strategien
description: Verstehen Sie den Unterschied zwischen LIFO- und FIFO-Strategien und wann die einzelnen Ansätze verwendet werden sollten
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---


# LIFO vs. FIFO-Strategien {#lifo-fifo-strategies}

Bei der Implementierung der Parallelitätsüberwachung müssen Sie zwischen zwei grundlegenden Strategien zur Konfliktbehandlung wählen, wenn Nutzungsbeschränkungen erreicht werden: **LIFO (Last In, First Out)** oder **FIFO (First In, First Out)**. Das Verständnis dieser Strategien ist für die Gestaltung des richtigen Benutzererlebnisses und die Implementierung der entsprechenden Fehlerbehandlung von entscheidender Bedeutung.

## Strategien für parallele Überwachungssitzungen {#concurrency-monitoring-session-strategies}

Sowohl LIFO als auch FIFO basieren auf **Stacktheorie** aus der Informatik:

### LIFO (Last In, First Out) - Stapelverhalten

Bei der Überwachung von gleichzeitigen Aufträgen:
- **Ältere Sitzungen sind vor neueren geschützt**
- **Neue Sitzungen werden blockiert, wenn die Grenzwerte erreicht sind**
- **Benutzer müssen bestehende Sitzungen manuell beenden, um neue Sitzungen zu starten**


### FIFO (First In, First Out) - Warteschlangenverhalten

Bei der Überwachung von gleichzeitigen Aufträgen:
- **Neue Sitzungen können ältere Sitzungen beenden** wenn die Grenzwerte erreicht sind
- **Der neueste Stream kann einen älteren Stream „rauswerfen“**
- **Benutzer können neue Inhalte starten, indem sie ersetzen, was sie angesehen haben**

## LIFO-Strategie {#lifo-strategy}

### Funktionsweise von LIFO

Im LIFO-Modus, wenn ein(e) Benutzende(r) versucht, einen neuen Stream zu starten und das gleichzeitige Limit erreicht:

1. **Neue Sitzung ist blockiert** mit einer 409-Konfliktantwort
2. **Bestehende Sitzungen bleiben unberührt**
3. **Benutzer muss eine** Sitzung manuell beenden, um fortzufahren

### LIFO-Flussdiagramm

![LIFO-Flussdiagramm](../assets/lifo-flow-diagram.png)

*Abbildung: LIFO-Strategiefluss (Letzter Eingang, Erster Ausgang) - Neue Sitzungen werden blockiert, wenn die Grenzwerte erreicht sind, sodass vorhandene Sitzungen manuell beendet werden müssen.*

### Verwendung von LIFO

**Verwenden von LIFO in folgenden Fällen:**
- **Benutzer erwarten, dass ihre aktuellen Inhalte vor** geschützt sind
- **Sie möchten bewusste Entscheidungen fördern** über Inhaltswechsel
- **Ihre Anwendung verfügt über eine begrenzte Benutzeroberflächen-Komplexität** um Konflikte zu lösen
- **Benutzende sehen sich Inhalte normalerweise über längere Zeiträume an**

**Beispiele:**
- Film-Streaming-Services, bei denen Benutzer Inhalte in voller Länge ansehen
- Plattformen für Lerninhalte, bei denen Unterbrechungen störend sind
- Anwendungen mit einfacher Benutzeroberfläche, die keine komplexe Sitzungsauswahl verarbeiten können

## FIFO-Strategie {#fifo-strategy}

### Funktionsweise von FIFO

Wenn ein(e) Benutzende(r) im FIFO-Modus versucht, einen neuen Stream zu starten, der das gleichzeitige Limit erreicht:

1. **Neue Sitzung ist zulässig** um zu beginnen
2. **Älteste Sitzung wird automatisch beendet** (oder der Benutzer wählt aus, welche beendet werden soll)
3. **Benutzer fährt mit neuen Inhalten fort**

### FIFO-Flussdiagramm

![FIFO-Flussdiagramm](../assets/fifo-flow-diagram.png)

*Abbildung: FIFO-Strategiefluss (First In, First Out) - Neue Sitzungen können beginnen, indem bestehende Sitzungen mit Benutzerauswahl beendet werden.*

### Verwendung von FIFO

**FIFO verwenden, wenn:**
- **Benutzer wechseln häufig zwischen Inhalten** (Kanaloberfliegen, Browsen)
- **Sie möchten die aktuelle Absicht des Benutzers gegenüber der** Aktivität priorisieren
- **Ihre Benutzeroberfläche kann die Sitzungsauswahl** Konflikte verarbeiten
- **Benutzer erwarten, dass sie neue Inhalte starten können** auch wenn die Grenzwerte erreicht sind

**Beispiele:**
- Live-TV-Anwendungen, bei denen Anwender häufig den Kanal wechseln
- Apps zur Inhaltserkennung, in denen Benutzer Inhalte durchsuchen und in der Vorschau anzeigen
- Mobile Apps, bei denen Benutzer eine sofortige Reaktion erwarten

### FIFO-Benutzererlebnis

Wenn im FIFO-Modus ein Konflikt auftritt:

1. **Dialogfeld anzeigen** mit allen aktiven Sitzungen
2. **Benutzer darf auswählen** welche Sitzung beendet werden soll
3. **Sitzungsdetails angeben** (Gerät, Inhalt, Dauer)
4. **Bestätigen Sie die Aktion** bevor Sie fortfahren.
5. **Neue Sitzung starten** nach Beendigung

## Zusammenfassung der wichtigsten Unterschiede {#key-differences-summary}

| Aspekt | FIFO | LIFO |
|-------------------------------|-----------------------------------------|-------------------------------|
| **Konfliktlösung** | Automatisches Beenden der ältesten Sitzung | Manuelle Beendigung erforderlich |
| **Fehlerbehandlung** | Keine Notwendigkeit, 409-Antworten zu verarbeiten | Muss 409-Antworten verarbeiten |
| **Benutzererlebnis** | Komplexere Benutzeroberfläche, aber reibungsloserer Ablauf | Einfachere Benutzeroberfläche, aber mehr Reibung |
| **Komplexität der Implementierung** | Höher (Benutzeroberfläche für die Sitzungsauswahl) | Niedriger Wert (einfache Fehlermeldungen) |
| **Anwendungsfall** | Inhaltswechsel, -erkennung | Erweiterte Anzeige, Schutz |


## Best Practices {#best-practices}

### Für LIFO-Implementierungen

1. **Klare Fehlermeldungen anzeigen** Erläuterung des Limits
2. **Einfacher Zugriff** auf das Sitzungsmanagement
3. **Aktive Sitzungen anzeigen** als Benutzerreferenz
4. **Implementieren des** in den Einstellungen der App
5. **Anzeige von Nutzungsindikatoren sollten angezeigt werden** bevor Konflikte auftreten

### Für FIFO-Implementierungen

1. **Bei Konflikten immer Benutzeroberfläche für** Sitzungsauswahl bereitstellen
2. **Aussagekräftige Sitzungsdetails anzeigen** (Gerät, Inhalt, Dauer)
3. **Implementieren von Bestätigungsdialogfeldern** um versehentliches Beenden zu verhindern
4. **Handhabung von Randfällen** bei denen die Beendigung fehlschlägt
5. **Klare Rückmeldung geben** was passiert


## Auswählen der Strategie {#choosing-your-strategy}

Beachten Sie bei der Wahl zwischen LIFO und FIFO folgende Faktoren:

1. **Benutzerverhaltensmuster** - Wie interagieren Benutzer normalerweise mit Ihren Inhalten?
2. **Content-Typ** - Live-TV vs. Filme vs. Bildungsinhalte
3. **Komplexität der Benutzeroberfläche** - Kann Ihre App mit einer komplexen Sitzungsauswahl umgehen?
4. **Benutzererwartungen** - Erwarten Benutzende, dass sie Inhalte einfach wechseln können?
5. **Geschäftsanforderungen** - Müssen bestimmte Arten von Anzeigen geschützt werden?
