---
title: Exportieren von Informationen zu Konten mit hoher Sharing-Punktzahl
description: Exportieren Sie Informationen für Konten mit hoher Sharing-Punktzahl.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# Exportieren von Informationen zu Konten mit hoher Sharing-Punktzahl {#export-account-info-high-score}

[!UICONTROL Account IQ] bietet Ihnen die Möglichkeit, Kontofreigabe-Details für die 1000 wichtigsten Abonnentenkonten basierend auf ihren [Freigabewahrscheinlichkeiten](/help/accountiq/product-concepts.md#account-sharing-probability-def). Die Daten in der exportierten CSV-Datei werden in der absteigenden Reihenfolge der Freigabewahrscheinlichkeiten der Abonnentenkonten - der ausgewählten MVPDs in der [Segment](/help/accountiq/product-concepts.md#segment-def)für [angegebener Zeitrahmen](/help/accountiq/product-concepts.md#time-frame-def).

Die Option zum Exportieren der Kontofreigabeinformationen finden Sie unter [Allgemeine Nutzungsberichte](/help/accountiq/general-usage-reports.md) und [Berichte zu freigegebenen Konten](/help/accountiq/shared-acc-reports.md) Seiten.

>[!NOTE]
>
>Die Zahlen in der heruntergeladenen CSV-Datei unterscheiden sich von den Seiten Allgemeine Nutzung und Gemeinsame Konten-Berichte . Dies liegt daran, dass die Seite Allgemeine Nutzungsberichte über zusätzliche Filter verfügt, mit denen die Programmierer Schwellenwerte für die Anzahl der Geräte, IPs und Postleitzahlen auswählen können. Daher basieren die aus den allgemeinen Nutzungsberichten exportierten Daten auf dem angewendeten zusätzlichen Schwellenwertfilter.

![Exportoption in Allgemeine Verwendung](assets/export.png)

So exportieren Sie die Informationen zur Kontofreigabe:

1. Definieren Sie ein gewünschtes Segment entsprechend den Schritten unter [Definieren von Segmenten und Auswählen von Zeitrahmen](/help/accountiq/howto-select-segment-timeframe.md) zur Bewertung von [Segment und Zeitrahmen](/help/accountiq/segments-timeframe.md) Bedienfeld.

1. Wählen Sie die **[!UICONTROL Export top 1000 accounts]** Option zum Exportieren der Kontoinformationen für 1000 Abonnenten mit der höchsten Freigabewahrscheinlichkeit.

Wenn Sie die Exportoption verwenden, werden die Statistiken für 1000 Konten mit den höchsten Teilungswahrscheinlichkeiten (für einen bestimmten Zeitraum) in den Ordner Downloads Ihres lokalen Computers heruntergeladen.

>[!NOTE]
>
>Die heruntergeladene CSV-Datei kann mit jeder Anwendung geöffnet werden, die eine CSV-Datei liest, z. B. Microsoft Excel.

![exportierte Daten im CSV-Format](assets/exported-csv.png)

*Abbildung: Exportierte freigegebene Kontodaten im CSV-Format*

## Spalten im exportierten Bericht {#columns-in-export}

**Woche/ Monat**

Die Woche oder der Monat, die bzw. den Sie auf der **[!UICONTROL Granularity and Time Frame]** in der Segmentauswahl, für die die Freigabestatistiken gesucht werden.

**MVPD**

Wenn Sie Programmierer sind, zeigt die Spalte an, zu welchem MVPD das Abonnentenkonto gehört.

**Abonnenten-ID**

Spezifisches Konto, über das wir in einer Reihe sprechen.

**Minimale Anzahl der Geräte**

Die tatsächliche Anzahl der Geräte (die Stream-Inhalte) ist mit ziemlicher Sicherheit größer als die Mindestanzahl der Geräte, die für ein bestimmtes Konto angegeben sind.

>[!NOTE]
>
>Die tatsächliche Anzahl der Geräte (diese Stream-Inhalte) ist mit Sicherheit größer als die Mindestanzahl der Geräte, die für ein bestimmtes Konto angegeben sind.

**Mindestanzahl von Personen**

Die absolute Mindestanzahl von Personen, die mit diesen Geräten aktive Streaming-Inhalte verwendet haben.

>[!NOTE]
>
>Die tatsächliche Anzahl der Personen (dieser Stream-Inhalt) ist fast sicher viel größer als die Mindestanzahl der Personen, die für ein bestimmtes Konto angegeben sind.

**[!UICONTROL # IPs]**

Anzahl der IP-Adressen, von denen Inhalte gestreamt werden.

**[!UICONTROL # Locations]**

Anzahl der Speicherorte (basierend auf der Postleitzahl), von denen der Inhalt gestreamt wird.

**[!UICONTROL # Cities]**

Anzahl der Städte, in denen das Streaming stattgefunden hat.

**[!UICONTROL # States]**

Anzahl der Staaten, in denen das Streaming stattgefunden hat.

**[!UICONTROL # Clusters]**

Die Anzahl der einzelnen [Cluster](/help/accountiq/product-concepts.md#cluster-def) wo Streaming stattgefunden hat.

**[!UICONTROL Geographic span (miles)]**

Der maximale Abstand zwischen den Streaming-Speicherorten, die mit dem Konto verknüpft sind.

**[!UICONTROL # AuthN OK]**

Die Häufigkeit, mit der sich die Benutzer während des Zeitraums mit diesem Konto angemeldet haben.

**[!UICONTROL # AuthZ OK]**

Gibt an, wie oft ein MVPD einen Stream autorisiert oder Zugriff (auf Inhalte) auf dieses Konto gewährt hat.

>[!NOTE]
>
>Die **[!UICONTROL # AuthZ OK]** ist mit dem **[!UICONTROL # Play Requests]**; sie ist kleiner als die **[!UICONTROL # Play Requests]** weil Adobe die Berechtigungen, die für MVPDs kommen, in der Regel 24 Stunden lang zwischenspeichert.

**[!UICONTROL # Play Requests]**

Die tatsächliche Anzahl der Streams während des Zeitraums.

**[!UICONTROL # Channels]**

Gesamtzahl der verschiedenen Kanäle, die das Konto im Zeitraum angesehen hat.

>[!NOTE]
>
>**[!UICONTROL # Channels]** enthält die Kanäle, die nicht notwendigerweise zum angemeldeten Programmierer gehörten.
>
>Diese Nummer für das Konto wurde angezeigt, weil das Konto Ihren Kanal angesehen hat, aber während dieses Zeitraums auch auf andere Kanäle zugegriffen hat.

**Nutzungsmuster**

Die Zahlen in dieser Spalte sind Kennungen, die einem der 14 Muster zugeordnet sind, als die wir alle Benutzerkonten identifizieren.

*Tabelle: Nutzungsmuster-IDs in exportierter CSV-Zuordnung mit Nutzungsmustern*

| ID | 1 | 2 | 3 | 4 | 5 und 8 | 6 | 7 | 9 | 10 und 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Nutzungsmuster | Regulärer Benutzer | Reisende oder Pendler | Großfamilie | Enge Familie und Freunde | Social-Gruppenfreigabe | Große Gruppe von Freunden | Gleichzeitiges Streaming | Community-Freigabe | Unsicheres Verhalten | Kleine Familie | Zweites Heim | Ungewöhnliche Nutzung |

{style="table-layout:auto"}

**Freigabewahrscheinlichkeit**

Die Freigabewahrscheinlichkeit ist die Wahrscheinlichkeit, mit der das spezifische Konto seine Anmeldeinformationen teilt.

>[!NOTE]
>
> Die durchschnittliche Freigabewahrscheinlichkeit aller Konten (im ausgewählten Segment) wird zur Berechnung der [Freigabestufe](/help/accountiq/dashboard.md#sharing-level) des [Aggregierte Teilungsbewertung](/help/accountiq/dashboard.md#aggregated-sharing).
