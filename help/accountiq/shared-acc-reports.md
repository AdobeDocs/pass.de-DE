---
title: Berichte zu freigegebenen Konten
description: Berichte zu freigegebenen Konten
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# Berichte zu freigegebenen Konten {#shared-accounts-reports}

Die Berichte zu freigegebenen Konten unterteilen die Metriken wie Anzahl der Geräte und Gerätetypen nach dem ausgewählten Bereich der Freigabewahrscheinlichkeit, z. B. **[!UICONTROL Over Moderate Probability]** und **[!UICONTROL Over Low Probability]** für das aktuelle Segment.

Diese Bereiche können dann als benutzerdefinierte Schwellenwerte dienen und die Diagramme werden anhand der ausgewählten Schwellenwerte aktualisiert.

Konto-IQ klassifiziert alle Abonnentenkonten des definierten Segments in die Konten mit den folgenden fünf Kategorien auf der Grundlage ihrer Freigabewahrscheinlichkeiten:

* Sehr hoch (80 %-100 %)
* Hoch (60 %-80 %)
* Moderieren (40 %-60 %)
* Niedrig (20 %-40 %)
* Sehr niedrig (0 %-20 %)

## Wahrscheinlichkeit der Kontofreigabe {#accounts-sharing-probability}

Das Ringdiagramm hier kategorisiert und zeigt die Prozentsätze (und absoluten Zahlen) der Abonnentenkonten aus verschiedenen Wahrscheinlichkeitskategorien.

Die rote Linie markiert den von den Benutzern in [Konten über dem Schwellenwert im aktuellen Segment](#threshold-selector) Bedienfeld.

![](assets/accounts-sharing-probability-pie.png)

Das Balkendiagramm zeigt die Anzahl der Konten auf der Y-Achse für verschiedene Kategorien von Teilungswahrscheinlichkeiten (auf der X-Achse dargestellt).

![](assets/accounts-sharing-probability-bar.png)

Die rote Linie markiert den Schwellenwert und kann im Balkendiagramm angepasst werden. Die im Balkendiagramm angepasste Schwelle spiegelt sich im Bereich der Schwelle im Ringdiagramm wider.

<!--![](assets/shared-accounts-rep.gif)-->

### Konten über dem Schwellenwert im aktuellen Segment{#threshold-selector}

In diesem Bedienfeld können Sie einen Bereich aus dem folgenden auswählen, um einen Schwellenwert für Abonnentenkonten festzulegen (basierend auf ihren Freigabewahrscheinlichkeiten):

* Konten **über sehr niedrig** Freigabe **wahrscheinlichkeit**

* Konten **über niedrig** Freigabe **wahrscheinlichkeit**

* Konten **moderieren** Freigabe **wahrscheinlichkeit**

* Konten **über Hoch** Freigabe **wahrscheinlichkeit**

![](assets/threshold-selector-shared-accounts.png)

Nachdem Sie den Schwellenwert ausgewählt haben, zeigt das Bedienfeld den Prozentsatz (und die Anzahl) der Konten von allen Abonnentenkonten im ausgewählten Segment an.

## Segment - Wiedergabe von Anforderungen insgesamt {#play-request-out-total}

Das Ringdiagramm zeigt den Prozentsatz (und die Anzahl) der Wiedergabeanforderungen, die von Abonnenten im Segment gestellt werden, und ermöglicht den Vergleich der Wiedergabeanforderungen von Abonnenten, die nicht im definierten Segment enthalten sind.

![](assets/play-req-outof-total.png)

Wenn Sie den Cursor auf das Ringdiagramm bewegen, werden auch die Prozentsätze und Zahlen der Abonnenten aus verschiedenen Wahrscheinlichkeitsbereichen angezeigt.

<!--![](assets/play-request-total.gif)-->

## Segmentdurchschnittliche Anzahl der Geräte pro Konto{#avg-devices-account}

Das Balkendiagramm zeigt die durchschnittliche Anzahl der Geräte jedes Gerätetyps, die von Abonnenten im aktuellen Segment und von Abonnenten, die sich nicht im aktuellen Segment befinden, verwendet werden.

![](assets/avg-devices-per-acc.png)

## Segment - Postleitzahlen pro Zeitraum pro Konto {#zip-codes-period-account}

Dieses Diagramm informiert Sie über die Anzahl der Abonnenten, die Inhalte von verschiedenen Orten in einem Zeitrahmen verbrauchen.

![](assets/zip-period-account.png)

Sie können heranzoomen, um die Details eines Balkens im Diagramm, das verschiedene Positionen abbildet, einzuschränken und anzuzeigen.

<!--![](assets/zip-code-period.gif)-->

## Segment - Geografischer Bereich/Zeitraum/Konto {#geo-span-period-account}

Dieses Balkendiagramm zeigt die Anzahl der Teilnehmerkonten in Bezug auf verschiedene geografische Bereiche in Meilen. Der Bereich basiert auf der maximalen Entfernung zwischen den Standorten, von denen ein Abonnent während des Zeitraums gestreamt hat.

<!--Total number of users ...

How many accounts are within 99 miles of each other.....and how many are apart. 

Based on points on the map.-->

![](assets/geogr-span-account.png)

Wenn Sie eine Leiste auswählen, die einen Bereich von geografischer Entfernung darstellt, erweitert sie den Bereich, um Ihnen weitere Details anzuzeigen.

<!--![](assets/geo-span-period-acc.gif)-->
