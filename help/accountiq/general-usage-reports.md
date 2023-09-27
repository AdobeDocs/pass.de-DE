---
title: Allgemeine Nutzungsberichte
description: Allgemeine Nutzungsberichte
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# [!UICONTROL General Usage] Berichte {#general-usage-reports}

[!UICONTROL Account IQ] Berichte sind grundlegende Analysetools und Berichte, mit denen Sie Ihre Daten im Drilldown-Verfahren untersuchen können, um sie zu isolieren. [Kohorten](/help/accountiq/product-concepts.md#segmet-def), identifizieren Sie Anomalien und erstellen Sie ein Verständnis Ihrer Kontomerkmale.

[!UICONTROL General Usage] Die Berichtseite bietet Tools zum Erstellen von Metriken zu Untergruppen basierend auf der Anzahl der verwendeten Kontogeräte, erkannten IPs und den entsprechenden Postleitzahlen.

<!--Divide the content in cohorts.

Content filters
device filters

segment and definition replicate to cohorts. Number of people and number of account that ......
content consumption.....-->

Die Berichte basieren alle auf dem aktuellen Segment, das mithilfe von [Segmente und Zeitrahmen](/help/accountiq/howto-select-segment-timeframe.md) Bedienfeld. Sie können Ihre Auswahl anpassen und weiter einschränken, indem Sie unter [Momentaufnahme Übersicht - Konten oberhalb der Schwellenwerte](#snapshot-overview) Bedienfeld.

<!--To view General Usage Reports:

1. Select the desired MVPDs from the **MVPDs in Segment** option.

2. Select the desired programmer channels from the **Channels in Segment** Option.

3. Select an appropriate time frame from the **Granularity and time frame** option.

   Using the above options you have defined segments for your analysis. Based on your segment selection, following graphs and reports are displayed.

4. You can fine tune your selection and further narrow it down by specifying (number of devices, number of IPs, and number of zip codes) thresholds in [Snapshot Overview - Accounts above thresholds](#snapshot-overview) widget/panel.-->

## AuthN OK/AuthZ OK/Play Requests/Unique Subscribers {#authn-authz-playreq-uniquesubs}

Die Liniendiagramme hier geben Ihnen einen Überblick über die Änderungen im Zeitverlauf in den Werten von AuthN OK, AuthZ OK, Play Requests und Unique Subscribers in einem ausgewählten Zeitrahmen für das definierte Segment.

+++Programmer- **AuthN OK/AuthZ OK/Play Requests/Unique Subscribers**

![](assets/progr-line-graph-gu.png)


*Abbildung: AuthN OK/AuthZ OK/Play Requests/Unique Subscribers für Programmierer-Benutzer*


+++


+++MVPD- **AuthN OK/AuthZ OK/Unique Subscribers**

![](assets/mvpd-line-graph-gu.png)


*Abbildung: AuthN OK/AuthZ OK/Unique Subscribers für MVPD-Benutzer*


+++

Die X-Achse zeigt die Einheiten im aktuellen Zeitrahmen und die Y-Achse stellt die grundlegenden Metriken der Abonnentenaktivität während dieses Zeitraums dar. Mit den Liniendiagrammen können Sie die folgenden Werte für die Abonnenten von MVPDs und Kanälen vergleichen, die Sie im Segmentauswahlfenster ausgewählt haben:

* **AuthN OK**

  AuthN OK ist die Anzahl erfolgreicher Authentifizierungen. Weitere Informationen und Definitionen finden Sie unter [Produktkonzepte: AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK**

  AuthZ OK ist die Anzahl erfolgreicher Berechtigungen. Weitere Informationen und Definitionen finden Sie unter [Produktkonzepte: AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Abspielanforderungen**

  Wiedergabeanforderungen entsprechen der Anzahl der Wiedergabeanforderungen. Weitere Informationen und Definitionen finden Sie unter [Produktkonzepte: Wiedergabeanforderungen](/help/accountiq/product-concepts.md#play-requests-def)

  >[!NOTE]
  >
  >Das Liniendiagramm für Wiedergabeanforderungen ist für MVPD-Benutzer nicht verfügbar.


* **Unique Subscribers**

  Unique Abonnenten sind die Anzahl erfolgreicher Unique Abonnenten. Weitere Informationen und Definitionen finden Sie unter [Produktkonzepte: Unique Subscribers](/help/accountiq/product-concepts.md#unique-subscriber-def)

  >[!NOTE]
  >
  >Die Gesamtanzahl der Unique Abonnenten umfasst auch die Anzahl der eindeutigen Geräte, wenn die Nutzung von Adobe TempPass durch einen Programmierer (d. h. eine kostenlose Vorschau) Teil des Segments ist.

## Momentaufnahme Übersicht - Konten oberhalb der Schwellenwerte {#snapshot-overview}

Passen Sie Ihre Analyse und Berichte mithilfe dieses zusätzlichen Filters an, um verschiedene Verwendungsschwellen festzulegen. Nachdem Sie Ihr Segment (oder Ihre Kohorte) zur Analyse definiert haben, indem Sie die gewünschten MVPDs und Kanäle auswählen, können Sie auch die folgenden Filter für verwenden, um das Verhalten der Abonnenten zu analysieren:

* Schwellenwert für Anzahl Geräte

* IP-Schwellenwert

* Schwelle für Postleitzahlen

Wenn Sie Schwellenwerte in [Kontosegment - basierend auf ausgewählten Schwellenwerten](#account-segments-basedon-segments) -Bedienfeld können Sie die Auswirkung in folgenden Bereichen anzeigen:

* [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account)

* [Standorte pro Woche (oder Monat) pro Konto](#locations-week-account)

* [IPs pro Woche (oder Monat) und Konto](#ip-week-account)

* [Historische Ansicht des Kontosegments](#account-segment-historical-view)

>[!NOTE]
>
>Der Standardwert für jeden Schwellenwert ist 4. Auf der Seite &quot;Allgemeine Nutzung&quot;werden also Analysen für MVPDs angezeigt, bei denen Abonnenten vier (und mehr als vier) Geräte verwenden, Inhalte von vier (und mehr) verschiedenen geografischen Standorten und vier (und mehr) verschiedenen Postleitzahlen verbrauchen.

### Kontosegment - basierend auf ausgewählten Schwellenwerten {#account-segments-basedon-segments}

Die **Kontosegment - basierend auf ausgewählten Schwellenwerten** bietet Optionen zum Festlegen von Schwellenwerten (zwischen 1 und 10) für die Anzahl der Geräte, die Anzahl der IPs und die Anzahl der Postleitzahlen.

Das Diagramm zeigt Ihnen Folgendes:

* absolute Anzahl der Abonnentenkonten und

* prozentualen Anteil an den Gesamtabonnentenkonten in diesem Segment,

  die X Geräte-Anzahl, Y IP-Anzahl und Z-Anzahl von Postleitzahlen verwenden, um Inhalte aus Ihrem Kanal für die (definierten Segmente von) MVPDs für einen bestimmten Zeitraum zu nutzen.

![](assets/select-thresholds.png)

## Geräte pro Woche (oder Monat) pro Konto {#devices-week-account}

Die **Balkendiagramm** bietet Einblicke in das Nutzungsverhalten in die Art und Weise, wie Abonnenten ihre Geräte für den Zugriff auf Inhalte verwenden.

Die X-Achse zeigt die Anzahl der Konten und die Anzahl der Geräte auf der y-Achse an. Basierend auf dem Schwellenwert, den Sie für die Anzahl der Geräte pro Konto festgelegt haben, wird die absolute Anzahl der Abonnentenkonten angezeigt, die Inhalte von einer bestimmten Anzahl von Geräten in einer Woche verbrauchen.

![](assets/bar-gr-devices-w-acc.png)

Wenn Sie den Mauszeiger über eine Leiste bewegen (spezifisch für die Anzahl der Geräte), wird eine Beschriftung angezeigt, die Informationen über die Anzahl der Abonnentenkonten (und den Prozentsatz der Gesamt-Abonnentenkonten im Segment) enthält, die Inhalte von Streaming-Kanälen mit diesen vielen Geräten in einer Woche nutzen.

Das Diagramm markiert auch Folgendes:

* Eine rote Linie, die den festgelegten Schwellenwert markiert.

* Eine grüne Linie, die die durchschnittliche Anzahl verschiedener Geräte angibt, die von einem Abonnentenkonto pro Woche (oder Monat) verwendet werden.

Sie können den Schwellenwert mit dem wöchentlichen Durchschnitt der Anzahl verschiedener Geräte, die von einem Konto verwendet werden, vergleichen, um den Grad der Freigabe zu beurteilen.

Das Diagramm gibt auch einen Überblick über den Prozentsatz der Abonnentenkonten, die mehr Geräte als den festgelegten Schwellenwert verwenden.

Das Ringdiagramm hilft Ihnen dabei, das Ausmaß der Abonnentenkonten, die Kanalinhalte verbrauchen, auf einen Blick anhand von Geräten zu beurteilen, die über dem festgelegten Schwellenwert (in einem Zeitrahmen) liegen.

![](assets/donut-devices-w-acc.png)

## Standorte pro Woche (oder Monat) pro Konto {#locations-week-account}

liken [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account)können Sie mit der Metrik Standorte pro Woche (oder Monat) pro Konto die Nutzung des Abonnentenkontos von verschiedenen Orten aus analysieren, um die Freigabe von Passwörtern genauer zu identifizieren. Die X-Achse zeigt die Anzahl der Konten und die Anzahl der Standorte auf der y-Achse an.

Ergebnisse aus dieser Metrik kombiniert mit der Anzahl der [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account) und Anzahl der [IPs pro Woche (oder Monat) pro Konto](#ip-week-account) hilft Ihnen, die Instanzen der Kennwortfreigabe genauer zu beurteilen, sodass authentische Benutzer nicht berücksichtigt werden.

![](assets/graph-loc-week-acc.png)

Nachdem Sie ein Segment definiert und den Schwellenwert für die Anzahl der Standorte festgelegt haben, können Sie aus dem Diagramm identifizieren:

* Anzahl (und Prozentsatz) der Abonnenten, die Inhalte von (einer bestimmten) x Anzahl von Orten in einer Woche nutzen.

* Prozentsatz der Gesamtanzahl der Abonnentenkonten, die Inhalte von mehr Standorten als dem Schwellenwert anzeigen.

* Vergleichen Sie den wöchentlichen Durchschnitt (Anzahl verschiedener Standorte für ein Konto) mit dem Schwellenwert.

## IPs pro Woche (oder Monat) pro Konto {#ip-week-account}

Ähnlich wie [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account) und [Standorte pro Woche (oder Monat) pro Konto](#locations-week-account), die **Anzahl der IPs pro Woche und Konto** -Metrik können Sie die Kennwortfreigabe präziser und detaillierter analysieren.

Die X-Achse zeigt die Anzahl der Konten und die Anzahl der IPs auf der y-Achse an.

![](assets/graph-ip-week-acc.png)

Nachdem Sie ein Segment definiert haben (durch Auswahl von MVPDs und Kanälen) und den Schwellenwert für die Anzahl der IPs festgelegt haben, können Sie aus dem Diagramm identifizieren:

* Anzahl (und Prozentsatz) der Abonnenten, die Inhalte aus (einer bestimmten) x Anzahl von IP-Adressen in einer Woche nutzen.

* Prozentsatz der Gesamtanzahl der Abonnentenkonten, die Inhalte von mehr IP-Adressen als dem Schwellenwert anzeigen.

* Vergleichen Sie den wöchentlichen Durchschnitt (Anzahl verschiedener IPs für ein Konto) mit dem Schwellenwert.

## Kontosegment - Historische Ansicht {#account-segment-historical-view}

Mit dem Balkendiagramm für die Historische Ansicht können Sie die Nutzungsmetriken über verschiedene Zeitrahmen hinweg vergleichen. Außerdem werden die verschiedenen Nutzungsmetriken, wie z. B. [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account), [Standorte pro Woche (oder Monat) pro Konto](#locations-week-account), und [IPs pro Woche (oder Monat) pro Konto](#ip-week-account).

* Die X-Achse zeichnet den Zeitrahmen auf und zeigt die Anzahl der Teilnehmerkonten, Geräte, Standorte und IPs auf der y-Achse an.

* Die orangefarbenen Balken kennzeichnen Segmente in verschiedenen Zeitrahmen.

* Das Liniendiagramm zeichnet die Änderungen in [Geräte pro Woche (oder Monat) pro Konto](#devices-week-account), [Standorte pro Woche (oder Monat) pro Konto](#locations-week-account), und [IPs pro Woche (oder Monat) pro Konto](#ip-week-account) -Werte über den gesamten Zeitraum basierend auf dem Schwellenwert hinweg.

![](assets/historical-view.png)

* Die blauen Balken bezeichnen die Gesamtzahl der aktiven Abonnenten in der Branche über einen bestimmten Zeitraum.

* Sie können spezifische Legenden auswählen und sie helfen Ihnen, das Diagramm zu skalieren.

![](assets/historical-view-total.png)

>[!MORELIKETHIS]
>
>* Erfahren Sie, wie Sie Berichte für die 1000 wichtigsten Abonnenten im ausgewählten Segment mithilfe von Filtern im allgemeinen Nutzungsbericht mit [Export der 1000 wichtigsten Konten](/help/accountiq/export-acc-information.md) -Option.
