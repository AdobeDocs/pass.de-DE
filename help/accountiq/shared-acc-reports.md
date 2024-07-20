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

Die Berichte zu freigegebenen Konten bieten eine weitere Gruppe von Diagrammen und Diagrammen, die das Freigabeverhalten und die Nutzung des aktuellen Segments widerspiegeln. Beispiel: **[!UICONTROL Over Moderate Probability]** und **[!UICONTROL Over Low Probability]** für das aktuelle Segment.

## Wahrscheinlichkeit der Kontofreigabe {#accounts-sharing-probability}

Diese Ringdiagramme und Balkendiagramme zeigen die Prozentsätze (und absoluten Zahlen) der Abonnentenkonten, die in bestimmte Bereiche der Freigabewahrscheinlichkeit fallen. Diese Bereiche werden wie folgt definiert:

* Sehr hoch (80 %-100 %)
* Hoch (60 %-80 %)
* Moderieren (40 %-60 %)
* Niedrig (20 %-40 %)
* Sehr niedrig (0 %-20 %)

Die rote Linie markiert den im Bereich [Konten über dem Schwellenwert im aktuellen Segment](#threshold-selector) ausgewählten Schwellenwert und der hellrote Bereich enthält die Summe aller Konten über diesem Schwellenwert.

![](assets/accounts-sharing-probability-pie.png)

Das Balkendiagramm zeigt die Anzahl der Konten, die in jeden Bereich auf der Y-Achse für jeden Bereich fallen (dargestellt auf der x-Achse).

![](assets/accounts-sharing-probability-bar.png)

Auch hier markiert die rote Linie den aktuellen Schwellenwert, und der hellrote Bereich enthält die Summe aller Konten über diesem Schwellenwert.

>[!NOTE]
>
> Die Y-Achse des Balkendiagramms ist logarithmisch.

### Konten über dem Schwellenwert im aktuellen Segment{#threshold-selector}

In diesem Bedienfeld können Sie den Schwellenwert für die oben genannten Ringdiagramme und Balkendiagramme auswählen. Die vier Optionen sind:

* Konten **über sehr niedrige** Freigabe **Wahrscheinlichkeit**

* Konten **über niedrige** Freigabe **Wahrscheinlichkeit**

* Konten **über moderat** Freigabe **Wahrscheinlichkeit**

* Konten **über hohe** Freigabe **Wahrscheinlichkeit**

![](assets/threshold-selector-shared-accounts.png)

Nachdem Sie den Schwellenwert ausgewählt haben, zeigt das Bedienfeld den Prozentsatz (und die Anzahl) der Konten von allen Abonnentenkonten im ausgewählten Segment an.

## Anforderungen zur Segmentwiedergabe aus der Gesamtsumme {#play-request-out-total}

Das Ringdiagramm zeigt den Prozentsatz (und die Anzahl) der Wiedergabeanforderungen, die von Abonnenten im Segment gestellt werden, und ermöglicht den Vergleich der Wiedergabeanforderungen von Abonnenten, die nicht im definierten Segment enthalten sind.

![](assets/play-req-outof-total.png)

Wenn Sie den Cursor über das Ringdiagramm bewegen, werden auch die Prozentsätze und Zahlen der Abonnenten aus verschiedenen Wahrscheinlichkeitsbereichen angezeigt.

<!--![](assets/play-request-total.gif)-->

## Segmentdurchschnittliche Anzahl der Geräte pro Konto{#avg-devices-account}

Das Balkendiagramm zeigt die durchschnittliche Anzahl der Geräte jedes Typs, die aktuell von Abonnenten im aktuellen Segment verwendet werden, sowie der Geräte, die nicht im aktuellen Segment enthalten sind.

![](assets/avg-devices-per-acc.png)

## Segment-Postleitzahlen pro Zeitraum und Konto {#zip-codes-period-account}

Dieses Diagramm informiert Sie über die Anzahl der Abonnenten im aktuellen Segment, die für das angegebene Zeitintervall Inhalte von verschiedenen Orten (gemessen anhand der Postleitzahl) verbrauchen.

![](assets/zip-period-account.png)

>[!NOTE]
>
>Sie können in die Balken einzoomen, die mehr als einen Satz von Postleitzahlen darstellen, die durch ein Pluszeichen (**+** ) (z. B. 10+) dargestellt werden, indem Sie darauf doppelklicken.


## Segmentgeografische Spanne pro Zeitraum und Konto {#geo-span-period-account}

Dieses Balkendiagramm zeigt die Anzahl der Abonnentenkonten, die Inhalte von Orten konsumieren, die in verschiedene geografische Bereiche in Meilen fallen. Der Bereich basiert auf der maximalen Entfernung zwischen den Standorten, von denen ein Abonnent während des Zeitintervalls gestreamt hat.

![](assets/geogr-span-account.png)

>[!NOTE]
>
> Sie können die Balken heranzoomen, die mehrere geografische Entfernungen darstellen, dargestellt durch ein Pluszeichen (**+**) (z. B. 1000+), indem Sie darauf doppelklicken.

>[!MORELIKETHIS]
>
>* Erfahren Sie, wie Sie Berichte für die 1000 wichtigsten Abonnenten im ausgewählten Segment mithilfe von Filtern in Berichten zu freigegebenen Konten mit der Option [Top 1000-Konten exportieren](/help/accountiq/export-acc-information.md) exportieren.
