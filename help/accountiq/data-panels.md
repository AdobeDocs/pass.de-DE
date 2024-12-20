---
title: Datenbereiche im Dashboard
description: Das Dashboard hilft, die Instanzen der Passwortfreigabe zu identifizieren, indem es eine breite Palette von Abonnentendaten analysiert.
exl-id: 12abba05-7422-4bcc-8b11-76aca4911c0b
source-git-commit: 2bb570ab14a3295d46ee6dc0d38485697d63809c
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 0%

---

# Datenbereiche im Dashboard {#data-panels}

Nachdem Sie ein Segment und ein Zeitintervall ausgewählt haben, zeigt das Dashboard verschiedene Datenbereiche, Tabellen und Diagramme an, die eine allgemeine Ansicht der Freigabeaktivität innerhalb des ausgewählten Segments widerspiegeln.

In der folgenden Tabelle sind die Verfügbarkeit und die Unterschiede zwischen den Daten-Panels in verschiedenen [Versionen](/help/accountiq/versions-aiq.md) von Account IQ aufgeführt:

| Daten-Panels | D2C-Dienste | TVE-Programmierer | TVE MVPDs |
|---|---|---|---|
| [Durchschnittlicher Freigabewert aggregiert für das aktuelle Segment](#aggregated-sharing) | Verfügbar und konsistent | Verfügbar und konsistent | Verfügbar und konsistent |
| [Videokategorien in Segment](#video-categories-segment) | Erhältlich mit leichten Varianten | Erhältlich mit leichten Varianten | Erhältlich mit leichten Varianten |
| [Freigabe der Punktzahl nach Kanälen und MVPDs](#sharin-score-by-channels-and-mvpds) | Nicht verfügbar | Verfügbar | Nicht verfügbar |
| [Wahrscheinlichkeit der Kontofreigabe](#accounts-sharing-probability) | Verfügbar und konsistent | Verfügbar und konsistent | Verfügbar und konsistent |
| [Anzahl der Konten und Nutzung durch Freigabe der Wahrscheinlichkeitsstufe](#number-of-accounts-usage-sharing-probability) | Verfügbar und konsistent | Verfügbar und konsistent | Verfügbar und konsistent |


## Durchschnittlicher Freigabewert für das aktuelle Segment aggregiert {#aggregated-sharing}

Das Bedienfeld für die durchschnittliche Freigabe bietet eine Anzeige in der oberen Zeile, die die Anzahl und Auswirkung der Freigabe in Bezug auf Konten und Streaming-Volumen zusammenfasst.

Die Metriken helfen Ihnen, das Ausmaß (von niedrig, mittel, hoch bis abnormal) der Freigabe von Anmeldeinformationen durch Ihre Abonnenten zu verstehen, gemessen in Bezug auf Konten und Verbrauch.

![](assets/aggregate-sharing-score.png)


*Bedienfeld für die durchschnittliche Freigabe für das aktuelle Segment aggregiert*

>[!NOTE]
>
> Der blaue Indikator im **Durchschnittliche Freigabebewertung aggregiert für das aktuelle Segment** dient für D2C-Dienste anderen Zwecken als für TV Everywhere. Bei D2C-Services stellt er den **Service Average Index** dar, wie im vorherigen Bild gezeigt. Wenn Sie sich als Programmierer oder MVPD anmelden, ändert sich diese Bezeichnung in **Industry Average Index**.

Die folgenden Metriken sind Komponenten des Bedienfelds „Durchschnittliche Freigabe“.

### Freigabeebene {#sharing-level}

Die Anzeige für die Freigabeebene gibt den Prozentsatz aller freigegebenen Teilnehmerkonten innerhalb des definierten Segments während des ausgewählten Zeitintervalls an.

Der Prozentsatz wird anhand eines Durchschnitts der für jedes Konto im Segment berechneten Freigabewahrscheinlichkeit berechnet. Diese Berechnung umfasst Konten, die im ausgewählten Zeitintervall mindestens einmal gestreamt wurden.

Der Trend-Indikator zeigt die prozentuale Änderung im Wert der Metrik gegenüber dem vorherigen Zeitintervall an.

![](assets/sharing-level.png){width="350" align="left"}


*Freigabeebene*

### Nutzung von freigegebenen Konten {#usage-from-shared-accounts}

Die Anzeige gibt den Prozentsatz der Nutzung durch die freigegebenen Konten unter allen abonnierten Konten für das definierte Segment und den definierten Zeitraum an. Diese Bereiche werden als „niedrig“, &quot;Medium&quot;, „hoch“ und „abnormal“ bezeichnet und basieren auf Durchschnittswerten der Branche.

Der Trend-Indikator, der einen Anstieg oder Rückgang der Nutzung von freigegebenen Konten im Vergleich zum vorherigen Zeitintervall darstellt.

![](assets/usage-4mshared-accounts.png){width="350" align="left"}


*Nutzung von freigegebenen Konten*

### Gesamtergebnis bei Freigabe {#overall-sharing-score}

Der Gesamtwert für die Freigabe ist eine Kombination aus Freigabewerten, einschließlich „Freigabestufe“ und „Nutzung aus freigegebenen Konten“.

Die Bewertung spiegelt die Gesamtwirkung der Freigabe wider. Sein Zweck ähnelt dem einer Kreditwürdigkeit, wobei die Freigabeebene mit einer einzigen Zahl zusammengefasst wird. Aber in diesem Fall zeigt eine höhere Punktzahl eine höhere Freigabestufe an.

![](assets/overall-sharing-score.png){width="350" align="left"}


*Gesamtergebnis der Freigabe*

## Videokategorien im Segment {#video-categories-segment}

Sie können die Spaltenüberschriften auswählen, um die Daten in allen Versionen von Account IQ zu sortieren.

+++D2C-Dienste: Regionen im Segment

Wenn Sie sich als D2C-Service anmelden **bietet die Tabelle „Regionen in Segment** eine vergleichende Ansicht der verschiedenen aggregierten Freigabewerte für [Videokategorien](/help/accountiq/product-concepts.md#video-category-def) im aktuellen Segment.

![](assets/sharing-scores-by-regions-in-segment.png)

*Freigabe des Scores nach Regionen im Segment*

>[!NOTE]
>
> Die [Videokategorien](product-concepts.md#video-category-def) im vorherigen Bild, z. B. **Regionen** in Segment, ist nur ein Beispiel. Wenn Sie sich bei Account IQ anmelden, zeigt dieses Bedienfeld die spezifische Videokategorie Ihres Unternehmens an.

Wählen **Exportieren**, um die Daten in einer CSV-Datei herunterzuladen. Erfahren Sie [wie Sie Daten-Panel-Berichte exportieren](/help/accountiq/export-reports.md).

+++

+++Programmierer: MVPDs im Segment

Wenn Sie sich als Programmierer anmelden, bietet **Tabelle „MVPDs in**&quot; eine vergleichende Ansicht der verschiedenen aggregierten Freigabewerte für die MVPDs im aktuellen Segment.

![](assets/sharing-scores-by-mvpds-in-segment.png)

Wählen **Exportieren**, um die Daten in einer CSV-Datei herunterzuladen. Erfahren Sie [wie Sie Daten-Panel-Berichte exportieren](/help/accountiq/export-reports.md).

+++

+++MVPDs: Programmierer im Segment

Wenn Sie sich als MVPD anmelden **bietet die Tabelle** Programmierer in Segment“ eine vergleichende Ansicht der verschiedenen aggregierten Freigabewerte für die Programmierer im aktuellen Segment.

Spaltenüberschriften auswählen, um die Daten zu sortieren.

![](assets/sharing-scores-by-programmers-in-segment.png)

*Freigabe des Scores durch Programmierer im Segment*

Wählen **Exportieren**, um die Daten in einer CSV-Datei herunterzuladen. Erfahren Sie [wie Sie Daten-Panel-Berichte exportieren](/help/accountiq/export-reports.md).

+++

## Scores nach Kanälen und MVPDs teilen  {#sharin-score-by-channels-and-mvpds}

Wenn Sie sich als Programmierer anmelden, bietet diese Tabelle eine vergleichende Ansicht der gemeinsamen Nutzung der Bewertungen der ausgewählten Kanäle für die MVPDs im aktuellen Segment.

Spaltenüberschriften auswählen, um die Daten zu sortieren.

![](assets/sharing-scores-by-channels-mvpds.png)


*Freigabe von Scores nach Kanälen und MVPDs*

## Wahrscheinlichkeit der Kontofreigabe {#accounts-sharing-probability}

Dieses Diagramm teilt Quintile der gemeinsamen Wahrscheinlichkeit in Bereiche von sehr niedrig (0-20%) bis sehr hoch (80-100%) auf. Erfahren Sie mehr über die Bereiche der [Wahrscheinlichkeit der Kontofreigabe](#accounts-sharing-probability).

>[!NOTE]
>
>Das Balkendiagramm verwendet eine logarithmische Skala.


![](assets/dashboard-ac-sharing-prob.png)


*Anzahl und Prozentsatz von Abonnentenkonten in verschiedenen Freigabewahrscheinlichkeitsbereichen*


## Anzahl der Konten und Nutzung durch Freigabe der Wahrscheinlichkeitsstufe {#number-of-accounts-usage-sharing-probability}

Dieses Bedienfeld bietet eine tabellarische Übersicht über Konten, die in Bereiche von sehr niedrigen (0-20 %) bis sehr hohen (80-100 %) Quintilen mit Teilungswahrscheinlichkeit unterteilt sind, wobei die zugehörige Nutzung jedes Quintils von freigegebenen Konten ausgeht. Erfahren Sie mehr über die Bereiche der [Wahrscheinlichkeit der Kontofreigabe](#accounts-sharing-probability).

![](assets/no-acc-usage-prob-level.png)

*Anzahl der Konten, Trends und Nutzungsszenarien, die in verschiedene Wahrscheinlichkeitsbereiche fallen*

Wählen **Exportieren**, um die Daten in einer CSV-Datei herunterzuladen. Erfahren Sie [wie Sie Daten-Panel-Berichte exportieren](/help/accountiq/export-reports.md).
