---
title: Segmente und Zeitintervall
description: Definieren Sie Kohorten oder wählen Sie Abonnentensegmente aus, um die Möglichkeiten zur Kontofreigabe und die Muster Ihrer Kanal-Viewer für die Verwendung grafischer Tools und Berichte in Konto IQ zu messen.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# Segmente und Zeitintervall {#segment-timeinterval}

Wenn Sie sich bei Konto IQ anmelden, können Sie den Abonnenten über das Segment- und Zeitintervall-Bedienfeld oberhalb des Dashboards definieren [Segment](product-concepts.md#segmet-def). Dieses Bedienfeld hilft bei der Filterung von Ergebnissen und der Anzeige von Berichten über das Verhalten und die Muster bei der Abonnentenfreigabe. Ein Segment namens **ALLE KONTEN IN IHREN EIGENSCHAFTEN** ist derzeit standardmäßig ausgewählt, wo Sie die folgenden Optionen anzeigen können:

![](assets/new-segment-selector-collapsed.png){align="left"}

*Segment- und Zeitintervallbereich mit reduzierter Segmentzusammenfassung*

**A.** Derzeit ausgewählter Segmentname **B.** Segmentliste öffnen **C.** Segment bearbeiten **D.** Neues Segment erstellen **E.** Granularitäts- und Zeitintervallauswahl **F.** Symbol zum Erweitern der Segmentzusammenfassung **G.** Reduzierte Segmentzusammenfassung **H.** Anzahl der Konten im Segment für das ausgewählte Intervall

>[!NOTE]
>
> Die zusammengestellte Segmentzusammenfassung zeigt die [Videokategorien](product-concepts.md#video-category-def) verwendet in der TV Anywhere-Version von Account IQ. Wenn Sie als D2C-Dienst angemeldet sind, zeigen diese Beschriftungen die spezifischen Videokategorien Ihres Unternehmens an.

Mehr dazu [Erstellen](work-with-segments.md#create-new-segment) und [Segmente verwalten](work-with-segments.md#manage-segment) aus dem **Segmente** im linken Bereich.

## Segmentauswahl {#segment-selection}

Gehen Sie wie folgt vor, um ein bestimmtes Segment auszuwählen:

1. Navigieren Sie zum **[!UICONTROL Open segment]** -Option im Segment- und Zeitintervallbereich.
1. Wählen Sie die **Segmentname** für die Sie die Berichte zur Kontofreigabe anzeigen möchten.

   ![](assets/open-segment.png){align="left"}

   *Segmentname auswählen*

   >[!NOTE]
   >
   > Die im vorherigen Bild angezeigten Videokategorien, z. B. **MVPDs**, **Programmierer**, und **Kanäle** stellen die Beschriftungen dar, die in der TV Anywhere-Version von Account IQ verwendet werden. Wenn Sie als D2C-Dienst angemeldet sind, zeigen diese Beschriftungen die spezifischen Videokategorien Ihres Unternehmens an.

1. Auswählen **[!UICONTROL Open segment]**.


## Granularität und Zeitintervallauswahl {#granularity-timeinterval}

Die **Granularität und Zeitintervall** -Selektor ermöglicht die Angabe von Datum und Dauer, die wöchentlich/monatlich aggregiert werden, um das Teilungsverhalten der Abonnenten zu beobachten. Die Standardauswahl ist die aktuelle Woche.

![Granularität und Zeitintervall](assets/granularity-timeinterval-weekwise.png){align="left"}

*Dialogfeld &quot;Granularität und Zeitintervall&quot;*

**A.** Granularitäts- und Zeitintervallauswahl **B.** Nach-rechts-Taste zum nächsten Monat/zur nächsten Woche **C.** Option zur Auswahl der Granularität nach Woche/Monat **D.** Aktuell ausgewähltes Zeitintervall **E.** Linkspfeil zum vorherigen Monat/zur vorherigen Woche

Sie können die Dauer wie folgt ändern:

1. Wählen Sie die **[!UICONTROL Granularity and Time Interval]** aus der Datumsauswahl.

1. Wählen Sie entweder **[!UICONTROL Week]** oder **[!UICONTROL Month]** von **[!UICONTROL Aggregate By]** -Option, um die Granularität für Ihre Bewertung festzulegen.

1. Nachdem Sie die Granularität ausgewählt haben, können Sie mit Vorwärts- oder Rückwärtspfeilen durch den Zeitraum navigieren.

1. Wählen Sie einen bestimmten Zeitraum für die Auswertung aus.

1. Auswählen **[!UICONTROL Apply]** , um sicherzustellen, dass Ihre Auswahl wirksam wird.

Auf diese Weise können Sie Ihre Problemanweisung als &quot;Abonnenten von MVPD A, die die Kanäle X, Y und Z während der ausgewählten Dezember-Woche angesehen haben&quot;definieren.

## Segmentzusammenfassung {#segment-summary}

Die Segmentzusammenfassung ist für D2C-Dienste und TV-überall ähnlich. Die Videokategorien unterscheiden sich für die jeweilige Version von Konto-IQ.

Auswählen <img alt= "Segmentzusammenfassung erweitern" src="./assets/expand-segment-summary.svg" width="25"> -Symbol, um die detaillierte Segmentzusammenfassung anzuzeigen. Er enthält außerdem Informationen zur Anzahl der Abonnentenkonten und zu ihren Wiedergabeanforderungen innerhalb des ausgewählten Zeitraums.

+++ D2C-Dienste

![](assets/segment-panel-d2c.png){align="left"}

*Segmentzusammenfassung für D2C-Dienste*

>[!NOTE]
>
>Die [Videokategorien](product-concepts.md#video-category-def) im vorherigen Bild angezeigt werden, z. B. **region** und **Content-Typen** im Segment sind nur Beispiele. Wenn Sie sich bei Konto IQ anmelden, zeigen diese Beschriftungen die spezifischen Videokategorien Ihres Unternehmens an.

Die **Segmentzusammenfassung** enthält die folgenden Bedingungen, die ein Segment definieren:

**[Regionen und Inhaltstypen](product-concepts.md#video-category-def) in Segment** verweisen auf die Metadatenbeschriftungen, die mit den Videostreams verknüpft sind, die von freigegebenen Konten, die in Berichten zur Kontofreigabe dargestellt werden, überwacht werden.

**[Metriken](product-concepts.md#metric) in Segment** bezieht sich auf Attribute oder Kriterien, die Abonnenten erfüllt haben müssen, um in Berichten zur Kontofreigabe identifiziert zu werden.

+++

+++ TV überall

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*Segmentzusammenfassung für Programmierer/MVPDs*

Die **Segmentzusammenfassung** enthält die folgenden Bedingungen, die ein Segment definieren:

**[Programmierer](product-concepts.md#programmer-def) in Segment**  bezieht sich auf Inhaltsanbieter, deren Video-Streams von freigegebenen Konten angesehen wurden, die in Berichten zur Kontofreigabe dargestellt werden.

**[Kanäle](product-concepts.md#channel-def) in Segment** bezeichnet Kanäle, deren Video-Streams von freigegebenen Konten angesehen wurden, die in Berichten zur Kontofreigabe dargestellt sind.

**[MVPDs](product-concepts.md#mvpd-def) in Segment** Beziehen Sie sich auf Distributoren für mehrere Videoprogramme, mit denen die Abonnenten verknüpft sind, um in Berichten zur Kontofreigabe identifiziert zu werden.

**[Metriken](product-concepts.md#metric) in Segment** bezieht sich auf Attribute oder Kriterien, die Abonnenten erfüllt haben müssen, um in Berichten zur Kontofreigabe identifiziert zu werden.

+++
