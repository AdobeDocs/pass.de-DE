---
title: Berichte zu freigegebenen Konten
description: Berichte zu freigegebenen Konten
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# Berichte zu freigegebenen Konten {#shared-accounts-reports}

Die Berichte über freigegebene Konten bieten eine weitere Gruppe von Diagrammen und Diagrammen, die das Freigabeverhalten und den Verbrauch für das aktuelle Segment widerspiegeln. Beispiel: **[!UICONTROL Over Moderate Probability]** und **[!UICONTROL Over Low Probability]** für das aktuelle Segment.

## Wahrscheinlichkeit der Kontofreigabe {#accounts-sharing-probability}

Diese Ringdiagramme und Balkendiagramme zeigen die Prozentsätze (und absoluten Zahlen) der Abonnentenkonten an, die in bestimmte Bereiche der Teilungswahrscheinlichkeit fallen. Diese Bereiche werden wie folgt definiert:

* Sehr hoch (80 %-100 %)
* Hoch (60 %-80 %)
* Mittelmäßig (40 %-60 %)
* Niedrig (20 %-40 %)
* Sehr niedrig (0%-20%)

Die rote Linie markiert den Schwellenwert, der im Bedienfeld [Konten über Schwellenwert im aktuellen Segment](#threshold-selector) ausgewählt wurde, und der hellrote Bereich enthält die Summe aller Konten über diesem Schwellenwert.

![](assets/accounts-sharing-probability-pie.png)

Das Balkendiagramm zeigt die Anzahl der Konten, die in jeden Bereich auf der Y-Achse für jeden Bereich fallen (auf der X-Achse dargestellt).

![](assets/accounts-sharing-probability-bar.png)

Auch hier markiert die rote Linie den aktuellen Schwellenwert, und der hellrote Bereich enthält die Summe aller Konten über diesem Schwellenwert.

>[!NOTE]
>
> Die y-Achse des Balkendiagramms ist logarithmisch.

### Konten über Schwellenwert im aktuellen Segment{#threshold-selector}

In diesem Panel können Sie den Schwellenwert für die Ringdiagramme und Balkendiagramme oben auswählen. Die vier Optionen sind:

* Konten **über sehr niedrig** Freigabe **Wahrscheinlichkeit**

* Konten **über niedrig** Freigabe **Wahrscheinlichkeit**

* Konten **über mittelmäßig** Freigabe **Wahrscheinlichkeit**

* Konten **über hohe** Freigabe **Wahrscheinlichkeit**

![](assets/threshold-selector-shared-accounts.png)

Wenn Sie den Schwellenwert ausgewählt haben, zeigt das Bedienfeld den Prozentsatz (und die Anzahl) der Konten von allen abonnierten Konten im ausgewählten Segment an.

## Segmentwiedergabeanforderungen insgesamt {#play-request-out-total}

Das Ringdiagramm zeigt den Prozentsatz (und die Anzahl) der Wiedergabeanfragen von Abonnentinnen und Abonnenten im Segment an, sodass Sie die Wiedergabeanfragen von Abonnentinnen und Abonnenten vergleichen können, die nicht im definierten Segment liegen.

![](assets/play-req-outof-total.png)

Wenn Sie den Cursor über das Ringdiagramm bewegen, werden auch die prozentualen Teilnehmerzahlen und Zahlen aus verschiedenen Wahrscheinlichkeitsbereichen angezeigt.

<!--![](assets/play-request-total.gif)-->

## Segmentdurchschnittliche Anzahl der Geräte pro Konto{#avg-devices-account}

Das Balkendiagramm zeigt die durchschnittliche Anzahl von Geräten jedes Typs an, die derzeit von Abonnentinnen und Abonnenten im aktuellen Segment und von jenen, die nicht im aktuellen Segment sind, verwendet werden.

![](assets/avg-devices-per-acc.png)

## Segment-Postleitzahlen pro Zeitraum und Konto {#zip-codes-period-account}

Dieses Diagramm informiert Sie über die Anzahl der Abonnentinnen und Abonnenten im aktuellen Segment, die für das jeweilige Zeitintervall Inhalte von verschiedenen Orten (gemessen durch Postleitzahl) konsumieren.

![](assets/zip-period-account.png)

>[!NOTE]
>
>Sie können die Balken vergrößern, die mehr als einen Satz von Postleitzahlen darstellen, dargestellt mit einem **+** (Pluszeichen) (z. B. 10+), indem Sie darauf doppelklicken.


## Segment-geografische Spanne pro Zeitraum und Konto {#geo-span-period-account}

In diesem Balkendiagramm wird die Anzahl der Abonnentenkonten dargestellt, die Inhalte von Standorten in verschiedenen geografischen Bereichen in Meilen konsumieren. Der Bereich basiert auf der maximalen Entfernung zwischen den Orten, von denen ein Teilnehmer während des Zeitintervalls gestreamt hat.

![](assets/geogr-span-account.png)

>[!NOTE]
>
> Sie können in die Balken zoomen, die mehrere geografische Entfernungen repräsentieren, die durch ein **+** (Pluszeichen) dargestellt werden (z. B. 1000+), indem Sie darauf doppelklicken.

>[!MORELIKETHIS]
>
>* Erfahren Sie, wie Sie Berichte für die 1000 wichtigsten Abonnenten im ausgewählten Segment mithilfe von Filtern in Berichten zu freigegebenen Konten mit der Option [Die 1000 wichtigsten Konten exportieren](/help/accountiq/export-acc-information.md) exportieren.
