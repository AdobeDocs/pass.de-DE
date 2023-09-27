---
title: Segment und Zeitraum definieren
description: Segment und Zeitraum definieren
exl-id: 86fe010d-3202-4ce2-b803-ff44f5538d7e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---

# Segment und Zeitraum definieren {#define-segment}

Alle Analyseberichte in [!UICONTROL Account IQ] Beginnen Sie mit der Definition des Segments und der Auswahl des Zeitrahmens für die Auswertung. [Segment](/help/accountiq/product-concepts.md#segmet-def) bezieht sich auf alle Abonnenten oder Viewer, die Ihre Kriterien erfüllen (Abonnieren eines MVPD und Anzeigen bestimmter Kanäle).

![](assets/segment-panel.png)

*Abbildung: Auswahl von Segment und Zeitrahmen*

Am oberen Rand aller Berichtseiten in [!UICONTROL Account IQ], gibt es ein Bedienfeld zum Definieren von Segmenten durch Auswahl von MVPDs, Kanalprogrammierern sowie Granularität und Zeitrahmen.

## Segmentauswahl {#select-segment}

### Auswählen von MVPDs im Segment {#select-segment-mvpds}

So wählen Sie MVPDs aus **[!UICONTROL MVPDs in segment]** Option:

1. Klicken oder tippen Sie auf **[!UICONTROL MVPDs in segment]** Dropdown-Option.

   >[!NOTE]
   >
   >**Alle** MVPDs der Branche sind standardmäßig ausgewählt. Von hier aus können Sie einen der **Top-10-MVPDs nach Freigabe der Punktzahl**, **Die 10 MVPDs nach Verwendung**, **Die 10 MVPDs nach Konten** oder einzelne MVPDs. Um jedoch einzelne MVPDs auszuwählen, müssen Sie die Auswahl aufheben **Alle**.

1. Klicken oder tippen Sie auf die gewünschten MVPDs.

   Sie können einen MVPD aus der Auswahl entfernen, indem Sie die Auswahl aufheben.

1. Klicken oder tippen Sie **[!UICONTROL Apply selection]** , damit Ihre Auswahl wirksam wird. Andernfalls verlieren Sie die Auswahl, die Sie vorgenommen haben.

   >[!NOTE]
   >
   >Wenn Sie den Isolationsmodus auswählen, kann keiner der anderen MVPDs ausgewählt werden.

### Kanäle im Segment auswählen {#select-segment-channels}

So wählen Sie die gewünschten Programmierkanäle aus dem **[!UICONTROL Channels in segment]** Option:

1. Klicken oder tippen Sie auf **[!UICONTROL Channels in segment]** Dropdown-Option.

   >[!NOTE]
   >
   >**Alle** -Programmierkanäle für Ihr Unternehmen sind standardmäßig ausgewählt. Um einzelne Kanäle oder Programmierer auszuwählen, müssen Sie zunächst die Auswahl aufheben **Alle**.

1. Klicken oder tippen Sie auf die gewünschten Kanäle oder Programmierer.

   Die Listenelemente der obersten Ebene im **[!UICONTROL Channels in segment]** are [Programmierer](/help/accountiq/product-concepts.md#programmer-def) Unternehmen und die Listenelemente unter den Programmiernamen sind ihre [channels](/help/accountiq/product-concepts.md#channel-def). Sie können entweder einzelne Kanäle unter Programmierern auswählen oder Programmierer auswählen und alle Aktivitäten der Kanäle unter diesem Programmierer sind in den Berichts- und Diagrammergebnissen enthalten.

   ![](assets/programmer-channels.png)


   *Abbildung: In der Kanalauswahl aufgelistete Programmierer und Kanäle*

   >[!IMPORTANT]
   >
   >Die Ergebnisse der Auswahl einzelner Kanäle unter einem Programmierer sind nicht mit denen der Auswahl des Programmierers identisch.
   >
   >
   >Wenn Sie einzelne Kanäle auswählen, werden die Aktivitäten dieser Kanäle in einigen Berichten einzeln aufgeschlüsselt. Wenn Sie jedoch den übergeordneten Programmierer all dieser Kanäle auswählen, werden alle Aktivitäten dieser Kanäle eingeschlossen, aber nicht einzeln in Berichten aufgeschlüsselt.

1. Klicken oder tippen Sie **[!UICONTROL Apply selection]** , damit Ihre Auswahl wirksam wird.

>[!NOTE]
>
>Sie können nicht mehr als 10 Elemente in den MVPD- oder Programmierer-Pulldown-Menüs auswählen.

### Deaktivieren Sie MVPDs und Kanäle. {#deselect-segment-mvpds-channels}

Zusätzlich zur Änderung Ihrer Auswahl im **[!UICONTROL MVPDs in segment]** und **[!UICONTROL Channels in segment]** Segmentselektoren können Sie die Auswahl der zuvor ausgewählten MVPDs und Kanäle aufheben, indem Sie:

* Auswählen der **[!UICONTROL Remove]** Symbol (![Symbol entfernen](assets/remove-icon.png)) in den Namen dieser ausgewählten MVPDs und Kanäle angezeigt, die unter der Segmentauswahl angezeigt werden.

* Sie können auch **[!UICONTROL Clear Selection]** um alle zuvor ausgewählten MVPDs oder Kanäle zu entfernen.

![](assets/segment-panel-selection.png)

*Abbildung: Ausgewählte MVPDs und Kanäle in Segment- und Zeitrahmen-Bedienfeld*

## Granularität und Zeitrahmen-Auswahl {#granularity-timeframe}

So wählen Sie einen Zeitraum für die Auswertung aus:

1. Wählen Sie die **[!UICONTROL Granularity and time frame]** Datumsauswahl.

1. Wählen Sie entweder **[!UICONTROL Week]** oder **[!UICONTROL Month]** von **[!UICONTROL Aggregate By]** -Option, um die Granularität für Ihre Bewertung festzulegen.

   ![](assets/granularity-timeframe-weekwise.png)


   *Abbildung: Datumsauswahl zur Auswahl der Granularität und des Zeitrahmens*

1. Nachdem Sie die Granularität ausgewählt haben, können Sie die Nach-vorne- oder Nach-hinten-Pfeile verwenden, um in der Zeit vorwärts oder rückwärts zu wechseln.

1. Geben Sie einen Zeitraum in der Vergangenheit (in Monat oder Woche basierend auf der ausgewählten Granularität) für die Auswertung an.

1. Auswählen **[!UICONTROL Apply Selection]** , um sicherzustellen, dass Ihre Auswahl wirksam wird.
