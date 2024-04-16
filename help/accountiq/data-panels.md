---
title: Datenbedienfelder im Dashboard
description: Das Dashboard hilft, die Instanzen der Kennwortfreigabe zu identifizieren, indem es eine breite Palette von Abonnentendaten analysiert.
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 0%

---

# Datenbedienfelder im Dashboard {#data-panels}

Nachdem Sie ein Segment und ein Zeitintervall ausgewählt haben, zeigt das Dashboard verschiedene Datenbedienfelder, Tabellen und Diagramme an, die eine allgemeine Ansicht der Teilungsaktivität innerhalb des ausgewählten Segments widerspiegeln.

Die nachstehende Tabelle beschreibt die Verfügbarkeit und die Unterschiede zwischen den Datenfeldern in den verschiedenen [Versionen](/help/accountiq/versions-aiq.md) Konto IQ:

| Datenfelder | D2C-Dienste | TVE-Programmierer | TVE MVPDs |
|---|---|---|---|
| [Durchschnittliche Teilungsbewertung, aggregiert für das aktuelle Segment](#aggregated-sharing) | Verfügbar und konsistent | Verfügbar und konsistent | Verfügbar und konsistent |
| [Videokategorien im Segment](#video-categories-segment) | Verfügbar mit leichten Varianten | Verfügbar mit leichten Varianten | Verfügbar mit leichten Varianten |
| [Teilungsbewertung nach Kanälen und MVPDs](#sharin-score-by-channels-and-mvpds) | Nicht verfügbar | Verfügbar | Nicht verfügbar |
| [Wahrscheinlichkeit der Kontofreigabe](#accounts-sharing-probability) | Verfügbar und konsistent | Verfügbar und konsistent | Verfügbar und konsistent |
| [Anzahl der Konten und Nutzung nach Freigabe der Wahrscheinlichkeitsstufe](#number-of-accounts-usage-sharing-probability) | Verfügbar und konsistent | Verfügbar und konsistent | Verfügbar und konsistent |


## Durchschnittliche Teilungsbewertung, aggregiert für das aktuelle Segment {#aggregated-sharing}

Das Bedienfeld für die durchschnittliche Teilungsbewertung bietet eine erstrangige Übersicht, die die Menge und die Auswirkungen der Freigabe in Bezug auf Konten und Streaming-Volumen zusammenfasst.

Die Metriken helfen Ihnen dabei, die Größenordnung (von niedrig, mittel, hoch bis anormal) der Weitergabe von Anmeldedaten durch Ihre Abonnenten zu verstehen, gemessen in Bezug auf Konten und Verbrauch.

![](assets/aggregate-sharing-score.png)


*Bedienfeld für durchschnittliche Teilungsbewertung, aggregiert für das aktuelle Segment*

>[!NOTE]
>
> Der blaue Indikator im **Durchschnittliche Teilungsbewertung, aggregiert für das aktuelle Segment** für D2C-Dienste im Vergleich zu TV Anywhere unterschiedliche Zwecke erfüllt. Bei D2C-Diensten stellt er die **Service Average Index** wie im vorherigen Bild gezeigt. Wenn Sie sich als Programmierer oder MVPD anmelden, ändert sich diese Bezeichnung in **Durchschnittlicher Branchenindex**.

Die folgenden Metriken sind Komponenten des Bereichs &quot;Durchschnittliche Teilungsbewertung&quot;.

### Freigabestufe {#sharing-level}

Der Freigabebereich zeigt den Prozentsatz aller gemeinsam genutzten Abonnentenkonten innerhalb des definierten Segments während des ausgewählten Zeitintervalls an.

Der Prozentsatz wird anhand des Durchschnitts der Teilungswahrscheinlichkeit berechnet, die für jedes Konto im Segment berechnet wird. Diese Berechnung umfasst Konten, die im ausgewählten Zeitintervall mindestens einmal gestreamt haben.

Der Trend -Indikator zeigt die prozentuale Änderung des Werts der Metrik im Vergleich zum vorherigen Zeitintervall an.

![](assets/sharing-level.png){width="350" align="left"}


*Freigabestufe*

### Nutzung durch freigegebene Konten {#usage-from-shared-accounts}

Die Messung gibt den Prozentsatz der Nutzung durch die gemeinsam genutzten Konten unter allen Abonnentenkonten für das definierte Segment und den definierten Zeitraum an. Diese Bereiche mit den Namen &quot;Niedrig&quot;, &quot;Mittel&quot;, &quot;Hoch&quot;und &quot;Abnormal&quot;basieren auf Branchendurchschnitten.

Der Trend -Indikator, der einen Anstieg oder Rückgang der Nutzung durch freigegebene Konten im Vergleich zum vorherigen Zeitintervall darstellt.

![](assets/usage-4mshared-accounts.png){width="350" align="left"}


*Nutzung durch freigegebene Konten*

### Gesamte Teilungsbewertung {#overall-sharing-score}

Das Gesamt-Sharing-Ergebnis ist eine Kombination aus Sharing-Werten, einschließlich &quot;Sharing-Level&quot;und &quot;Nutzung von freigegebenen Konten&quot;.

Es bietet einen Wert, der die Gesamtauswirkungen der Freigabe widerspiegelt. Ihr Zweck ähnelt dem eines Kreditwerts, bei dem die Teilungsebene mit einer einzelnen Zahl zusammengefasst wird. In diesem Fall weist ein höherer Wert jedoch auf eine höhere Freigabestufe hin.

![](assets/overall-sharing-score.png){width="350" align="left"}


*Gesamte Teilungsbewertung*

## Videokategorien im Segment {#video-categories-segment}

Sie können die Spaltenüberschriften auswählen, um die Daten in allen Versionen von Konto IQ zu sortieren.

++ + D2C-Dienste: Regionen im Segment

Wenn Sie sich als D2C-Dienst anmelden, **Regionen im Segment** bietet eine vergleichende Übersicht über die verschiedenen aggregierten Teilungsbewertungen für die [Videokategorien](/help/accountiq/product-concepts.md#video-category-def) im aktuellen Segment.

![](assets/sharing-scores-by-regions-in-segment.png)

*Freigeben der Punktzahl nach Regionen im Segment*

>[!NOTE]
>
> Die [Videokategorien](product-concepts.md#video-category-def)  im vorherigen Bild angezeigt werden, z. B. **Regionen** im Segment ist nur ein Beispiel. Wenn Sie sich bei Konto IQ anmelden, wird in diesem Bereich die spezifische Videokategorie Ihres Unternehmens angezeigt.

Auswählen **Export** , um die Daten in einer CSV-Datei herunterzuladen. Lernen [Exportieren von Datenbereichsberichten](/help/accountiq/export-reports.md).

+++

+++ Programmierer: MVPDs im Segment

Bei der Anmeldung als Programmierer **MVPDs im Segment** bietet eine vergleichende Übersicht über die verschiedenen aggregierten Sharing-Werte für die MVPDs im aktuellen Segment.

![](assets/sharing-scores-by-mvpds-in-segment.png)

Auswählen **Export** , um die Daten in einer CSV-Datei herunterzuladen. Lernen [Exportieren von Datenbereichsberichten](/help/accountiq/export-reports.md).

+++

+++MVPDs: Programmierer im Segment

Bei der Anmeldung als MVPD **Programmierer im Segment** bietet einen vergleichenden Überblick über die verschiedenen aggregierten Teilungsbewertungen für die Programmierer im aktuellen Segment.

Wählen Sie die Spaltenüberschriften aus, um die Daten zu sortieren.

![](assets/sharing-scores-by-programmers-in-segment.png)

*Freigeben der Punktzahl durch Programmierer im Segment*

Auswählen **Export** , um die Daten in einer CSV-Datei herunterzuladen. Lernen [Exportieren von Datenbereichsberichten](/help/accountiq/export-reports.md).

+++

## Teilungsbewertung nach Kanälen und MVPDs  {#sharin-score-by-channels-and-mvpds}

Wenn Sie sich als Programmierer anmelden, bietet diese Tabelle einen vergleichenden Überblick über die Freigabe von Bewertungen der ausgewählten Kanäle für die MVPDs im aktuellen Segment.

Wählen Sie die Spaltenüberschriften aus, um die Daten zu sortieren.

![](assets/sharing-scores-by-channels-mvpds.png)


*Teilen von Bewertungen nach Kanälen und MVPDs*

## Wahrscheinlichkeit der Kontofreigabe {#accounts-sharing-probability}

Dieses Diagramm teilt in Bereiche mit Wahrscheinlichkeitsquintilien auf, die von sehr niedrig (0-20%) bis sehr hoch (80-100%) reichen. Mehr über die Bereiche von [Wahrscheinlichkeit der Kontofreigabe](#accounts-sharing-probability).

>[!NOTE]
>
>Das Balkendiagramm verwendet eine logarithmische Skala.


![](assets/dashboard-ac-sharing-prob.png)


*Anzahl und Prozentsatz der Abonnentenkonten in verschiedenen Wahrscheinlichkeitsbereichen für die Freigabe*


## Anzahl der Konten und Nutzung nach Freigabe der Wahrscheinlichkeitsstufe {#number-of-accounts-usage-sharing-probability}

Dieses Bedienfeld bietet eine tabellarische Ansicht von Konten, die in Bereiche mit Wahrscheinlichkeitsquintilien für gemeinsame Nutzung unterteilt sind, die von sehr niedrig (0-20 %) bis sehr hoch (80-100 %) reichen, wobei die zugehörige Nutzung jedes Quintils aus freigegebenen Konten erfolgt. Mehr über die Bereiche von [Wahrscheinlichkeit der Kontofreigabe](#accounts-sharing-probability).

![](assets/no-acc-usage-prob-level.png)

*Anzahl der Konten, Trends und Verwendungen, die in verschiedene Wahrscheinlichkeitsbereiche fallen*

Auswählen **Export** , um die Daten in einer CSV-Datei herunterzuladen. Lernen [Exportieren von Datenbereichsberichten](/help/accountiq/export-reports.md).

