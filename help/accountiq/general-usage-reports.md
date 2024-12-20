---
title: Allgemeine Verwendungsberichte
description: Erfahren Sie mehr über allgemeine Nutzungsberichte
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: 4a8a73d6c67508e88ba3ffbb9033b7e339f4fe8f
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 0%

---

# [!UICONTROL General usage] Berichte {#general-usage-reports}

[!UICONTROL Account IQ] Berichte sind grundlegende Analyse-Tools, mit denen Sie Ihre Daten detailliert analysieren [Kohorten](/help/accountiq/product-concepts.md#segmet-def), Anomalien identifizieren und ein Verständnis Ihrer Kontomerkmale aufbauen können.

Auf [!UICONTROL General usage] Seite Berichte stehen Tools zur Verfügung, mit denen Untergruppenmetriken anhand der Anzahl der verwendeten Kontogeräte, der erkannten IP-Adressen und der entsprechenden Postleitzahlen ermittelt werden können.

Die Berichte basieren alle auf dem aktuellen Segment, das im Bedienfeld [Segmente und Zeitintervall](/help/accountiq/segments-timeinterval.md) ausgewählt wurde. Sie können Ihre Auswahl optimieren und weiter eingrenzen, indem Sie die Schwellenwerte (Anzahl der Geräte, Anzahl der IPs und Anzahl der Postleitzahlen) im Bedienfeld [Momentaufnahme - Konten über Schwellenwerten](#snapshot-overview) angeben.

## Play-Anfragen und eindeutige Abonnenten {#playreq-uniquesubs}

Das Liniendiagramm hier bietet Ihnen einen Überblick über die Änderungen von Werten im Zeitverlauf, wie z. B. Wiedergabeanforderungen und eindeutige Abonnenten in einem ausgewählten Zeitintervall für das definierte Segment.

+++ D2C-Services: Play-Anfragen/eindeutige Abonnenten

![](assets/d2c-line-graph-gu.png)


*Play-Anfragen/Eindeutige Abonnenten für D2C-Services*

+++

+++Programmierer: Wiedergeben von Anfragen/eindeutigen Abonnenten

![](assets/progr-line-graph-gu.png)


*Anfragen/eindeutige Abonnenten für Programmierer abspielen*

+++

+++MVPDs: Eindeutige Abonnenten

![](assets/mvpd-line-graph-gu.png)

*Eindeutige Abonnenten für MVPDs*

+++

<br/>

Die X-Achse stellt die Zeit basierend auf dem aktuellen Intervall dar, und die Y-Achse stellt grundlegende Abonnentenaktivitätsmetriken während dieses Zeitraums dar. Mit den Liniendiagrammen können Sie die Aktivität der Abonnentinnen und Abonnenten im aktuellen Segment visualisieren und vergleichen. Je nach Version von Account IQ umfassen die Metriken:

* **AuthN OK**: Anzahl der erfolgreichen Authentifizierungen. Lesen Sie mehr über [AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK**: Anzahl der erfolgreichen Autorisierungen. Lesen Sie mehr über [AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Play Requests**: Anzahl der Play Requests. Lesen Sie mehr über [Play-Anfragen](/help/accountiq/product-concepts.md#play-requests-def).

* **Eindeutige Abonnenten**: Anzahl der erfolgreichen eindeutigen Abonnenten. Lesen Sie mehr über [Unique Subscribers](/help/accountiq/product-concepts.md#unique-subscriber-def).

>[!NOTE]
>
>Die Verfügbarkeit von Metriken hängt von der Version von Account IQ ab.

## Momentaufnahme - Konten über Schwellenwerten {#snapshot-overview}

Optimieren Sie Ihre Analysen und Berichte mithilfe dieses zusätzlichen Filters, um verschiedene Nutzungsschwellen festzulegen. Nachdem Sie ein Segment ausgewählt haben, können Sie auch die folgenden Filter verwenden, um das Verhalten der Abonnenten weiter zu analysieren:

* Schwellenwert für die Anzahl von Geräten

* Schwellenwert für Anzahl von IPs

* Schwellenwert für die Anzahl der Postleitzahlen

Wenn Sie Schwellenwerte im Bedienfeld [Segmentbasierte Konten basierend auf ausgewählten Schwellenwerten](#account-segments-basedon-segments) aktualisieren, werden Sie die Auswirkungen in folgenden Bereichen sehen:

* [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account)

* [Standorte pro Woche (oder Monat) pro Konto](#locations-week-account)

* [IPs pro Woche (oder Monat) pro Konto](#ip-week-account)

* [Historische Ansicht der Segmentkonten](#account-segment-historical-view)

>[!NOTE]
>
>Für jeden Schwellenwert wird der Standardwert 4 festgelegt. Das bedeutet, dass auf der Seite „Allgemeine Nutzung“ die Analyse für Abonnentinnen und Abonnenten angezeigt wird, die mehr als vier Geräte verwenden, Inhalte von mehr als vier verschiedenen IP-Adressen *und* mehr als vier verschiedenen Postleitzahlen verwenden.

### Segmentbasierte Konten basierend auf ausgewählten Schwellenwerten {#account-segments-basedon-segments}

Das Bedienfeld **Konten segmentbasiert auf ausgewählten Schwellenwerten** bietet Ihnen Optionen zum Festlegen von Schwellenwerten (zwischen 1 und 10) für die Anzahl der Geräte, die Anzahl der IPs und die Anzahl der Postleitzahlen.

Das Diagramm zeigt Ihnen die:

* Absolute Anzahl von Abonnentenkonten.

* Prozentualer Anteil der gesamten abonnierten Konten im Segment, die die Anzahl der Geräte verwenden, ausgehend von der Anzahl der IPs, in der Anzahl der Postleitzahlen, wie durch die Schwellenwerte angegeben.

![](assets/select-thresholds.png)

## Geräte pro Woche (oder Monat) pro Konto {#devices-week-account}

Dieses Balkendiagramm bietet Einblicke in das Nutzungsverhalten in Bezug darauf, wie Abonnentinnen und Abonnenten ihre Geräte verwenden, um auf Inhalte zuzugreifen.

Die Diagramme für die X-Achse, Anzahl der Konten und Y-Achse, Anzahl der Geräte. Basierend auf dem Schwellenwert, den Sie für die Anzahl der Geräte pro Konto festgelegt haben, wird die absolute Anzahl der Abonnentenkonten markiert, die Inhalte von einer bestimmten Anzahl von Geräten in einer Woche konsumieren.

![](assets/bar-gr-devices-w-acc.png)

Wenn Sie den Mauszeiger über einen Balken bewegen (für die Anzahl der Geräte spezifisch), wird eine Beschriftung angezeigt, die Informationen über die Anzahl der Abonnentenkonten (und den Prozentsatz der gesamten Abonnentenkonten im Segment) angibt, die Streaming-Kanalinhalte auf diesen vielen Geräten in einer Woche verwenden.

Das Diagramm zeigt außerdem Folgendes:

* Eine rote Linie markiert den von Ihnen festgelegten Schwellenwert.

* Eine grüne Linie, um den Durchschnitt der Anzahl verschiedener Geräte zu markieren, die von einem Abonnentenkonto pro Woche (oder Monat) verwendet werden.

Der Ringdiagramm bietet eine alternative Ansicht der Geräte, die von Konten im aktuellen Segment oberhalb des festgelegten Schwellenwerts verwendet werden.

![](assets/donut-devices-w-acc.png)

## Standorte pro Woche (oder Monat) pro Konto {#locations-week-account}

Ähnlich wie bei der Metrik [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account) können Sie mit der Metrik Standorte pro Woche (oder Monat) pro Konto die Nutzung des Abonnentenkontos von verschiedenen Standorten aus analysieren. Die Diagramme X-Achse: Anzahl der Konten, Y-Achse: Anzahl der Standorte.

![](assets/graph-loc-week-acc.png)

Nachdem Sie den Schwellenwert für die Anzahl der Standorte festgelegt haben, können Sie das Diagramm verwenden, um Folgendes zu identifizieren:

* Anzahl (und Prozentsatz) der Abonnentinnen und Abonnenten, die Inhalte von (einem bestimmten) x Anzahl der Standorte in einer Woche nutzen.

* Prozentsatz der gesamten abonnierten Konten, die Inhalte von mehr Orten aus anzeigen als der Schwellenwert.

* Vergleichen Sie den wöchentlichen Durchschnitt (Anzahl der verschiedenen Standorte für ein Konto) mit dem Schwellenwert.

## IPs pro Woche (oder Monat) pro Konto {#ip-week-account}

Ähnlich wie bei der Metrik für **Anzahl der Standorte pro Woche pro Konto** können Sie mit der Metrik **Anzahl der IPs pro Woche** Konto) den Umfang der Änderung an der Quelle des Streaming für das aktuelle Segment bewerten.

Die Diagramme auf der X-Achse zeigen die Anzahl der Konten und auf der Y-Achse die Anzahl der IPs.

![](assets/graph-ip-week-acc.png)

Nachdem Sie ein Segment definiert und den Schwellenwert für die Anzahl der IPs festgelegt haben, können Sie das Diagramm verwenden, um Folgendes zu identifizieren:

* Anzahl (und Prozentsatz) der Abonnentinnen und Abonnenten, die Inhalte von einer bestimmten Anzahl von IPs in einer Woche nutzen.

* Prozentsatz der gesamten abonnierten Konten, die Inhalte von mehr IP-Adressen als dem Schwellenwert anzeigen.

* Vergleichen Sie den wöchentlichen Durchschnitt (Anzahl verschiedener IPs für ein Konto) mit dem Schwellenwert.

## Segment-Verlaufsansicht der Konten {#account-segment-historical-view}

Mit dem Balkendiagramm für die Verlaufsansicht können Sie die Nutzungsmetriken über verschiedene Zeitintervalle hinweg vergleichen. Außerdem werden die verschiedenen Nutzungsmetriken zusammengefasst dargestellt, z. B. [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account), [Standorte pro Woche (oder Monat) pro Konto](#locations-week-account) und [IPs pro Woche (oder Monat) pro Konto](#ip-week-account).

* Die X-Achse zeigt das Zeitintervall und die Y-Achse die Anzahl der Teilnehmerkonten, Geräte, Standorte und IPs.

* Die orangefarbenen Balken geben Segmente in verschiedenen Zeitintervallen an.

* Im Liniendiagramm werden die Änderungen der Werte [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account), [Standorte pro Woche (oder Monat) pro Konto](#locations-week-account) und [IPs pro Woche (oder Monat) ](#ip-week-account) Konto über das Zeitintervall basierend auf dem Schwellenwert dargestellt.

![](assets/historical-view.png)

* Die blauen Balken geben die Gesamtzahl der aktiven Abonnenten in der Branche für ein Zeitintervall an.

* Sie können bestimmte Legenden auswählen und sie helfen Ihnen beim Skalieren des Diagramms.

![](assets/historical-view-total.png)
