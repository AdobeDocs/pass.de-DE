---
title: Arbeiten mit Segmenten
description: Segmente verstehen und verwenden. Erfahren Sie, wie Sie ein Segment erstellen und verwalten.
exl-id: 01431437-55f5-464d-8ee4-7a79ec553e4f
source-git-commit: 38402b411dd4fad3490115cff1854faa7f3cb293
workflow-type: tm+mt
source-wordcount: '1970'
ht-degree: 0%

---

# Arbeiten mit Segmenten {#work-with-segments}

[Segmente](product-concepts.md#segmet-def) sind eine Sammlung von Abonnentenkonten, mit denen Sie die Freigabe von Anmeldedaten unter benutzerdefinierten Bedingungen analysieren können. Sie können Segmente verwenden, um verschiedene Sätze von Abonnentenkonten zu untersuchen und entsprechende Datenberichte in Tabellen und Grafiken zu generieren. In Account IQ gibt es zwei Arten von Segmenten:

1. **Standardsegment**: **Alle Konten in Ihren Eigenschaften** sind ein vordefiniertes Segment im System, das alle aktiven Abonnentenkonten ohne spezifische Bedingungen enthält.

   >[!NOTE]
   >
   >Die Verwendung des Standardsegments kann die Anzeige bestimmter Tabellen wie [Videokategorien im Segment](data-panels.md#video-categories-segment), [Sharing-Wert nach Kanälen und MVPDs](data-panels.md#sharin-score-by-channels-and-mvpds) und [Gebrauchsmuster-Verteilung für Videokategorien](usage-patterns.md#usage-pattern-dis-video-categories) verhindern. Diese Tabellen können nur Daten für bis zu 20 Zeilen gleichzeitig aufnehmen und anzeigen. Die übrigen Tabellen, Diagramme und Berichte sind für standardmäßige und benutzerdefinierte Segmente identisch.

1. **Benutzerdefinierte Segmente**: Hierbei handelt es sich um maßgeschneiderte Segmente, mit denen Sie Abonnentenkonten aus bestimmten Kategorien wie D2C-Inhaltstypen, Programmierern, Kanälen und MVPDs gruppieren können, um die Freigabe von Berechtigungen unter benutzerdefinierten Bedingungen zu analysieren. Erfahren Sie mehr darüber, wie Sie [ein benutzerdefiniertes Segment erstellen](#create-new-segment).

   >[!IMPORTANT]
   >
   >Alle in diesem Handbuch beschriebenen Verfahren basieren auf benutzerdefinierten Segmenten. Die Konzepte für standardmäßige und benutzerdefinierte Segmente bleiben jedoch gleich.

Wenn Sie zur Registerkarte **Aktionen** gehen und im linken Bereich die Registerkarte **[!UICONTROL Segments]** auswählen, wird eine Liste der im System verfügbaren Segmente angezeigt. Auf der Segmentseite können Sie wichtige Details zu jedem Segment schnell in einem tabellarischen Format bewerten. Zu den Details gehören der Segmentname, die Anzahl der [Videokategorien](product-concepts.md#video-category-def), Metriken, [Vorgänge](product-concepts.md#operation-def) unter Verwendung des aktuellen Segments, Datum und Uhrzeit der letzten Änderung sowie der Name des Segmenterstellers.

Sie können die folgenden Funktionen mit Segmenten ausführen:

* [Neues Segment erstellen](#create-new-segment)
* [Segmente verwalten](#manage-segments)


## Neues Segment erstellen {#create-new-segment}

Der Prozess der Erstellung eines neuen Segments ähnelt dem von D2C-Diensten und TV Anywhere. Die Videokategorien unterscheiden sich für die jeweilige Version von Account IQ.

++ + D2C-Dienste

Um ein Segment zu erstellen und das Freigabeverhalten des Abonnenten zu analysieren, wählen Sie oben rechts **[!UICONTROL Create new segment]** aus.

![Wählen Sie Neues Segment erstellen](assets/create-new-segment-d2c.png)

*Wählen Sie Neues Segment erstellen*

>[!NOTE]
>
>Die im vorherigen Bild angezeigten Videokategorien wie **Regionen** und **Content-Typen** sind nur Beispiele. Wenn Sie sich bei Account IQ anmelden, zeigen diese Beschriftungen die spezifischen Videokategorien Ihres Unternehmens an.

Dadurch wird eine Seite **Neues Segment** geöffnet, die die folgenden Elemente enthält:

![Neue Segmentseite](assets/d2c-new-segment-dialog.png)

*Neue Segmentseite*

**A.** Segmentkomponenten **B.** Segmentdefinition **C.** Segmentzusammenfassung

* **Segmentkomponenten**: Ein Bestand von [Videokategorien](product-concepts.md##video-category-def) und berechneten Metriken, die zur Definition eines Segments verwendet werden.

  >[!NOTE]
  >
  >Verwenden Sie **[!UICONTROL Show all]** , um die Liste der Segmentkomponenten zu erweitern. Um eine Komponente schnell zu finden, suchen Sie ihren Namen in **Suchsegmentkomponenten**, anstatt durch die gesamte Liste zu scrollen.

* **Segmentdefinition**: Eine Arbeitsfläche, auf der Sie verschiedene Segmentkomponenten per Drag &amp; Drop verschieben können, um ein Segment zu erstellen.

* **Segmentzusammenfassung**: Eine Zusammenfassung, die die qualifizierten Konten auf der Grundlage der Komponenten in der Segmentdefinition schätzt und eine kurze Übersicht über das Segment während des Auswertungszeitraums bietet.

Führen Sie die folgenden Schritte aus, um ein Segment zu erstellen:

1. Geben Sie den Namen Ihres Segments in **Segmentname** ein, der in der Segmentliste und während der Segmentauswahl angezeigt wird.
1. Geben Sie eine detaillierte Beschreibung Ihres Segments in **Segmentbeschreibung** ein.
1. Ziehen Sie beispielsweise **Regionen und Inhaltstypen** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Regionen/Inhaltstypen** innerhalb der **Segmentdefinition** ab.

   >[!NOTE]
   >
   >Sie können ein Segment basierend auf Regionen oder Inhaltstypen erstellen. Zeigen Sie die zugehörigen Inhaltstypen einer Region über ein Dropdown-Menü an.

   Wenn Sie zunächst den Inhaltstyp **1} im Abschnitt** Regionen/Content-Typen **hinzufügen, können Sie nur Inhaltstypen als nachfolgende Komponenten hinzufügen.**

   Wenn Sie zunächst eine **Region** im Abschnitt **Regionen/Content-Typen** hinzufügen, wird ein Entscheidungsdialogfeld angezeigt.

   ![Hinzufügen der Segmentkomponente als Region oder ihrer Inhaltstypen ](assets/d2c-segment-basis-selector.png){width="550" align="left"}

   *Hinzufügen der Segmentkomponente als Region oder des Dialogfelds &quot;Inhaltstypen&quot;*

   Entscheiden Sie, ob Sie bestimmte Regionen oder ein Segment anhand der mit einer Region verknüpften Inhaltstypen vergleichen möchten.

   Wählen Sie **[!UICONTROL As a region]** aus, um dem Abschnitt **Regionen/Inhaltstypen** Regionen hinzuzufügen.

   Wählen Sie **[!UICONTROL As its content types]** aus, um Inhaltstypen einer Region hinzuzufügen.

1. Ziehen Sie **Metriken** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Metriken** innerhalb der **Segmentdefinition** ab.

   ![Wählen Sie einen Operator und legen Sie einen Wert für die hinzugefügte Metrik fest](assets/component-metrics.png)

   *Wählen Sie einen Operator aus und weisen Sie einen Wert für die hinzugefügte Metrik zu*

   Nachdem Sie Metriken zur Segmentdefinition hinzugefügt haben, wählen Sie einen Operator aus dem Dropdown-Menü **[!UICONTROL Select an operator]** und weisen Sie einen Wert mit **[!UICONTROL Select an option]** zu.

   Passen Sie die Werte für bestimmte Metriken an, indem Sie die Erhöhung mit dem Pfeil nach oben und die Verringerung mit dem Pfeil nach unten durchführen.

1. Ziehen Sie **Berechnete Metriken** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Berechnete Metriken** innerhalb der **Segmentdefinition** ab.

   ![Wählen Sie einen Operator und legen Sie einen Wert für die hinzugefügte berechnete Metrik fest](assets/component-calculated-metrics.png)

   *Wählen Sie einen Operator und weisen Sie einen Wert für die hinzugefügte berechnete Metrik zu*

   Nachdem Sie berechnete Metriken zur Segmentdefinition hinzugefügt haben, geben Sie **[!UICONTROL Select an operator]** aus dem Dropdown-Menü ein und weisen Sie einen Wert mit **[!UICONTROL Select an option]** zu.

   >[!NOTE]
   >
   >Alle Metriken und berechneten Metriken, die Sie unter der Segmentdefinition ablegen, werden von entsprechenden Operatoren begleitet, um den jeweiligen Metriken und berechneten Metriken Werte zuzuweisen.

1. Überprüfen Sie die Segmentdetails in der **Segmentzusammenfassung** , um zu entscheiden, welche Änderungen Sie für das Segment implementieren möchten.
1. Wählen Sie **[!UICONTROL Last week]** oder **[!UICONTROL Last month]** aus dem Dropdown-Menü **Auswertungszeitraum** aus, um die Zusammenfassungswerte für die letzte Woche oder den letzten Monat zu schätzen.
1. Wählen Sie **[!UICONTROL Update estimation]** aus, um die Anzahl der geschätzten qualifizierten Konten im aktuellen Segment basierend auf dem ausgewählten Auswertungszeitraum zu berechnen.
1. Wählen Sie **[!UICONTROL Save segment]** aus.

Das von Ihnen erstellte Segment ist jetzt in der Segmentliste verfügbar.

+++

+++TV Überall

Um ein Segment zu erstellen und das Freigabeverhalten des Abonnenten zu analysieren, wählen Sie oben rechts **[!UICONTROL Create new segment]** aus.

![Wählen Sie Neues Segment erstellen](assets/create-new-segment.png)

*Wählen Sie Neues Segment erstellen*

Dadurch wird eine Seite **Neues Segment** geöffnet, die die folgenden Elemente enthält:

![Neue Segmentseite](assets/new-segment-dialog.png)

*Neue Segmentseite*

**A.** Segmentkomponenten **B.** Segmentdefinition **C.** Segmentzusammenfassung

* **Segmentkomponenten**: Ein Bestand an Programmierern und Kanälen, MVPDs, Metriken und berechneten Metriken, die zur Definition eines Segments verwendet werden.

  >[!NOTE]
  >
  >Verwenden Sie **[!UICONTROL Show all]** , um die Liste der Segmentkomponenten zu erweitern. Um eine Komponente schnell zu finden, suchen Sie ihren Namen in **Suchsegmentkomponenten**, anstatt durch die gesamte Liste zu scrollen.

* **Segmentdefinition**: Eine Arbeitsfläche, auf der Sie verschiedene Segmentkomponenten per Drag &amp; Drop verschieben können, um ein Segment zu erstellen.

* **Segmentzusammenfassung**: Eine Zusammenfassung, die die qualifizierten Konten auf der Grundlage der Komponenten in der Segmentdefinition schätzt und eine kurze Übersicht über das Segment während des Auswertungszeitraums bietet.

Führen Sie die folgenden Schritte aus, um ein Segment zu erstellen:

1. Geben Sie den Namen Ihres Segments in **Segmentname** ein, der in der Segmentliste und während der Segmentauswahl angezeigt wird.
1. Geben Sie eine detaillierte Beschreibung Ihres Segments in **Segmentbeschreibung** ein.
1. Ziehen Sie **Programmierer und Kanäle** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Programmierer/Kanäle** innerhalb der **Segmentdefinition** ab.

   >[!NOTE]
   >
   >Sie können ein Segment entweder auf der Basis von Programmierern oder Kanälen erstellen. Zeigen Sie die zugehörigen Kanäle mit einem Programmierer über ein Dropdown-Menü an.

   Wenn Sie zunächst einen **Kanal** im Abschnitt **Programmierer/Kanäle** hinzufügen, können Sie Kanäle nur als nachfolgende Komponenten hinzufügen.

   Wenn Sie zunächst einen **Programmierer** im Abschnitt **Programmierer/Kanäle** hinzufügen, wird ein Entscheidungsdialogfeld angezeigt.

   ![Segmentkomponente als Programmierer oder dessen Kanäle hinzufügen ](assets/segment-basis-selector.png){width="550" align="left"}


   *Hinzufügen der Segmentkomponente als Programmierer oder dessen Dialogfeld &quot;Kanäle&quot;*

   Entscheiden Sie, ob Sie bestimmte Programmierer oder ein Segment anhand der mit einem Programmierer verknüpften Kanäle vergleichen möchten.

   Wählen Sie **[!UICONTROL As a programmer]** aus, um dem Abschnitt **Programmierer/Kanäle** Programmierer hinzuzufügen.

   Wählen Sie **[!UICONTROL As its channels]** aus, um alle Kanäle eines Programmierers hinzuzufügen.

1. Ziehen Sie **MVPDs** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **MVPDs** innerhalb der **Segmentdefinition** ab.

   >[!NOTE]
   >
   >Wenn Sie sich als Programmierer anmelden, wird ein MVPD mit dem Namen **xfinity** als eigenständige Option im Abschnitt **MVPDs** angezeigt. Sie können es nicht mit einem anderen MVPD kombinieren.

1. Ziehen Sie **Metriken** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Metriken** innerhalb der **Segmentdefinition** ab.

   ![Wählen Sie einen Operator und legen Sie einen Wert für die hinzugefügte Metrik fest](assets/component-metrics.png)

   *Wählen Sie einen Operator aus und weisen Sie einen Wert für die hinzugefügte Metrik zu*

   Nachdem Sie Metriken zur Segmentdefinition hinzugefügt haben, wählen Sie einen Operator aus dem Dropdown-Menü **[!UICONTROL Select an operator]** und weisen Sie einen Wert mit **[!UICONTROL Select an option]** zu.

   Passen Sie die Werte für bestimmte Metriken an, indem Sie die Erhöhung mit dem Pfeil nach oben und die Verringerung mit dem Pfeil nach unten durchführen.

1. Ziehen Sie **Berechnete Metriken** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Berechnete Metriken** innerhalb der **Segmentdefinition** ab.

   ![Wählen Sie einen Operator und legen Sie einen Wert für die hinzugefügte berechnete Metrik fest](assets/component-calculated-metrics.png)

   *Wählen Sie einen Operator und weisen Sie einen Wert für die hinzugefügte berechnete Metrik zu*

   Nachdem Sie berechnete Metriken zur Segmentdefinition hinzugefügt haben, geben Sie **[!UICONTROL Select an operator]** aus dem Dropdown-Menü ein und weisen Sie einen Wert mit **[!UICONTROL Select an option]** zu.

   >[!NOTE]
   >
   >Alle Metriken und berechneten Metriken, die Sie unter der Segmentdefinition ablegen, werden von entsprechenden Operatoren begleitet, um den jeweiligen Metriken und berechneten Metriken Werte zuzuweisen.

1. Überprüfen Sie die Segmentdetails in der **Segmentzusammenfassung** , um zu entscheiden, welche Änderungen Sie für das Segment implementieren möchten.
1. Wählen Sie **[!UICONTROL Last week]** oder **[!UICONTROL Last month]** aus dem Dropdown-Menü **Auswertungszeitraum** aus, um die Zusammenfassungswerte für die letzte Woche oder den letzten Monat zu schätzen.
1. Wählen Sie **[!UICONTROL Update estimation]** aus, um die Anzahl der geschätzten qualifizierten Konten im aktuellen Segment basierend auf dem ausgewählten Auswertungszeitraum zu berechnen.
1. Wählen Sie **[!UICONTROL Save segment]** aus.

Das von Ihnen erstellte Segment ist jetzt in der Segmentliste verfügbar.
+++

## Segmente verwalten {#manage-segments}

Sie können ein Segment aus der Segmentliste auswählen und dann die folgenden Aktionen ausführen:

* [Segment bearbeiten](#edit-segment)
* [Segment duplizieren](#duplicate-segment)
* [Segment löschen](#delete-segment)

![Bearbeiten, Duplizieren oder Löschen eines Segments](assets/manage-segments-list.png)

*Wählen Sie ein Segment zum Bearbeiten, Duplizieren oder Löschen aus*

**A.** [Standardsegment](#work-with-segments) **B.** [Videokategorien](product-concepts.md#video-category-def)

>[!NOTE]
>
>Die in diesem Abschnitt angezeigten Videokategorien wie **MVPDs**, **Programmierer** und **Kanäle** stellen die Beschriftungen dar, die in der TV-Version von Account IQ verwendet werden. Wenn Sie als D2C-Dienst angemeldet sind, zeigen diese Beschriftungen die spezifischen Videokategorien Ihres Unternehmens an.

Sie können das Standardsegment **Alle Konten in Ihren Eigenschaften** nicht bearbeiten, duplizieren oder löschen.

### Segment bearbeiten {#edit-segment}

1. Navigieren Sie zur Registerkarte **[!UICONTROL Segments]** unter **Aktionen** im linken Bereich, um eine Liste von Segmenten anzuzeigen.
1. Wählen Sie ein Segment aus, das Sie bearbeiten möchten.
1. Wählen Sie **[!UICONTROL Edit]** aus.
1. Ändern Sie Segmentdetails wie den Segmentnamen, die Beschreibung oder die Komponenten in der **Segmentdefinition**.

   >[!TIP]
   >
   >Verwenden Sie **[!UICONTROL Clear all]** , um alle Segmentkomponenten innerhalb jedes Abschnitts unter der Segmentdefinition gleichzeitig zu entfernen. Wählen Sie alternativ die Schaltfläche &quot;Kreuz&quot;aus, um einzelne Elemente zu entfernen.

   ![Löschen Sie alle Segmentkomponenten in jedem Abschnitt unter der Segmentdefinition ](assets/clear-all-components.png)

   *Wählen Sie Alle löschen aus, um alle Segmentkomponenten gleichzeitig zu entfernen*

1. Wählen Sie entweder **[!UICONTROL Update segment]** aus, um das vorhandene Segment zu aktualisieren, oder **[!UICONTROL Save as new segment]**, um ein neues Segment mit den Änderungen zu erstellen.

   >[!NOTE]
   >
   >Das Aktualisieren von Segmenten, die derzeit in Betrieb sind, ist nicht zulässig. Das Speichern von Änderungen als neues Segment ist die einzige Option für Segmente mit laufenden Vorgängen.

### Segment duplizieren {#duplicate-segment}

1. Navigieren Sie zur Registerkarte **[!UICONTROL Segments]** unter **Aktionen** im linken Bereich, um eine Liste von Segmenten anzuzeigen.
1. Wählen Sie ein Segment aus, das Sie duplizieren möchten.
1. Wählen Sie **[!UICONTROL Duplicate]** aus.

Eine Kopie des ausgewählten Segments wird generiert und am Ende der Segmentliste platziert. Sie können die erforderlichen Details im duplizierten Segment bearbeiten und dann entweder das doppelte Segment aktualisieren oder es als neues Segment speichern.

### Segment löschen {#delete-segment}

1. Navigieren Sie zur Registerkarte **[!UICONTROL Segments]** unter **Aktionen** im linken Bereich, um eine Liste von Segmenten anzuzeigen.
1. Wählen Sie ein Segment aus, das Sie entfernen möchten.

   Wählen Sie mehrere Segmente aus, um sie in einem einzigen Vorgang zu löschen. Sie können auch ein Kontrollkästchen links neben **Segmentname** aktivieren, um alle Segmente gleichzeitig zu löschen.

   >[!NOTE]
   >
   > Sie können nur mehr als ein Segment oder alle Segmente löschen, wenn keines der Segmente von Vorgängen verwendet wird. Außerdem ist das Löschen des Standardsegments namens **Alle Konten in Ihren Eigenschaften** nicht zulässig. Die Auswahl bleibt aufgehoben, wenn Sie versuchen, alle Segmente gleichzeitig zu löschen.

   ![Löschen mehrerer Segmente](assets/delete-more-than-one-segment.png)

   *Mehrere Segmente auswählen, um mehr als ein Segment zu löschen*

1. Wählen Sie **[!UICONTROL Delete]** aus.
1. Bestätigen Sie im Dialogfeld die Auswahl von **[!UICONTROL Delete]** , um das Segment dauerhaft zu entfernen.

   >[!NOTE]
   >
   >Das Segment wird dauerhaft aus dem System gelöscht und Sie können diese Aktion nicht rückgängig machen.
