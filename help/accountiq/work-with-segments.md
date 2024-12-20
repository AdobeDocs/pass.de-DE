---
title: Arbeiten mit Segmenten
description: Verstehen und Verwenden von Segmenten. Erfahren Sie, wie Sie ein Segment erstellen und verwalten.
exl-id: 01431437-55f5-464d-8ee4-7a79ec553e4f
source-git-commit: 38402b411dd4fad3490115cff1854faa7f3cb293
workflow-type: tm+mt
source-wordcount: '1970'
ht-degree: 0%

---

# Arbeiten mit Segmenten {#work-with-segments}

[Segmente](product-concepts.md#segmet-def) sind eine Sammlung von Abonnentenkonten, mit denen Sie die Freigabe von Anmeldeinformationen unter benutzerdefinierten Bedingungen analysieren können. Sie können Segmente verwenden, um verschiedene Sätze von Abonnentenkonten zu untersuchen und entsprechende Datenberichte in Tabellen und Diagrammen zu generieren. In Account IQ gibt es zwei Segmenttypen:

1. **Standardsegment**: **Alle Konten in Ihren** Eigenschaften“ ist ein vorkonfiguriertes Segment im System, das alle aktiven Abonnentenkonten enthält, auf die keine spezifischen Bedingungen angewendet werden.

   >[!NOTE]
   >
   >Die Verwendung des Standardsegments kann die Anzeige bestimmter Tabellen wie [Videokategorien im Segment](data-panels.md#video-categories-segment), [Freigabe eines Punktwerts nach Kanälen und MVPDs](data-panels.md#sharin-score-by-channels-and-mvpds) und [Verteilung von Nutzungsmustern für Videokategorien](usage-patterns.md#usage-pattern-dis-video-categories) verhindern. Diese Tabellen können nur Daten für bis zu 20 Zeilen gleichzeitig aufnehmen und anzeigen. Die übrigen Tabellen, Diagramme und Berichte sind für standardmäßige und benutzerdefinierte Segmente identisch.

1. **Benutzerdefinierte Segmente**: Hierbei handelt es sich um maßgeschneiderte Segmente, mit denen Sie Abonnentenkonten aus bestimmten Kategorien gruppieren können, z. B. D2C-Inhaltstypen, Programmierer, Kanäle und MVPDs, um die Freigabe von Anmeldeinformationen unter benutzerdefinierten Bedingungen zu analysieren. Erfahren Sie mehr über das [Erstellen eines benutzerdefinierten Segments](#create-new-segment).

   >[!IMPORTANT]
   >
   >Alle in diesem Handbuch beschriebenen Verfahren basieren auf benutzerdefinierten Segmenten. Die Konzepte bleiben jedoch für standardmäßige und benutzerdefinierte Segmente gleich.

Wenn Sie zu **Aktionen** gehen und die Registerkarte **[!UICONTROL Segments]** im linken Bereich auswählen, wird eine Liste der im System verfügbaren Segmente angezeigt. Auf der Seite Segmente können Sie wichtige Details zu jedem Segment schnell in einem tabellarischen Format bewerten. Zu den Details gehören der Segmentname, die Anzahl der [Videokategorien](product-concepts.md#video-category-def), Metriken [Vorgänge](product-concepts.md#operation-def) unter Verwendung des aktuellen Segments, Datum und Uhrzeit der letzten Änderung sowie der Name des Segmenterstellers.

Mit Segmenten können Sie die folgenden Funktionen ausführen:

* [Neues Segment erstellen](#create-new-segment)
* [Segmente verwalten](#manage-segments)


## Neues Segment erstellen {#create-new-segment}

Der Prozess der Erstellung eines neuen Segments ist für D2C-Services und TV Everywhere ähnlich. Die Videokategorien unterscheiden sich für jede Version von Account IQ.

+++D2C-Dienste

Um ein Segment zu erstellen und das Freigabeverhalten von Abonnenten zu analysieren, wählen Sie oben rechts **[!UICONTROL Create new segment]** aus.

![Wählen Sie Neues Segment erstellen aus](assets/create-new-segment-d2c.png)

*Wählen Sie Neues Segment erstellen aus*

>[!NOTE]
>
>Die im vorherigen Bild angezeigten Videokategorien wie **Regionen** und **Inhaltstypen** sind nur Beispiele. Wenn Sie sich bei Account IQ anmelden, zeigen diese Kennzeichnungen die spezifischen Videokategorien Ihres Unternehmens an.

Dadurch wird eine Seite **Neues Segment** geöffnet, die die folgenden Elemente enthält:

![Neue Segmentseite](assets/d2c-new-segment-dialog.png)

*Neue Segmentseite*

**A.** Segmentkomponenten **B.** Segmentdefinition **C.** Segmentzusammenfassung

* **Segmentkomponenten**: Ein Inventar [Videokategorien](product-concepts.md##video-category-def) und berechneten Metriken, die zum Definieren eines Segments verwendet werden.

  >[!NOTE]
  >
  >Verwenden Sie **[!UICONTROL Show all]** , um die Liste der Segmentkomponenten zu erweitern. Um eine Komponente schnell zu finden, durchsuchen Sie ihren Namen **nach Segmentkomponenten suchen** anstatt in der gesamten Liste zu scrollen.

* **Segmentdefinition**: Eine Arbeitsfläche, auf der Sie verschiedene Segmentkomponenten per Drag-and-Drop verschieben können, um ein Segment zu erstellen.

* **Segmentzusammenfassung** Eine Zusammenfassung, die die qualifizierten Konten basierend auf den Komponenten in der Segmentdefinition schätzt und einen kurzen Überblick über das Segment während des Auswertungszeitraums bietet.

Führen Sie die folgenden Schritte aus, um ein Segment zu erstellen:

1. Geben Sie den Namen Ihres Segments in **Segmentname** ein, der in der Segmentliste und bei der Segmentauswahl angezeigt wird.
1. Geben Sie in „Segmentbeschreibung“ eine detaillierte Beschreibung **Segments**.
1. Ziehen Sie beispielsweise **Regionen und Inhaltstypen** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Regionen/Inhaltstypen** innerhalb der **Segmentdefinition** ab.

   >[!NOTE]
   >
   >Sie können ein Segment basierend auf Regionen oder Inhaltstypen erstellen. Zeigen Sie die zugehörigen Inhaltstypen einer Region aus einem Dropdown-Menü an.

   Wenn Sie zunächst einen **Inhaltstyp** im Abschnitt **Regionen/Inhaltstypen** hinzufügen, können Sie Inhaltstypen nur als nachfolgende Komponenten hinzufügen.

   Wenn Sie zunächst im Abschnitt **Regionen/Inhaltstypen** eine **Region** hinzufügen, wird ein Entscheidungsdialogfeld angezeigt.

   ![Hinzufügen der Segmentkomponente als Region oder ihrer Inhaltstypen ](assets/d2c-segment-basis-selector.png){width="550" align="left"}

   *Fügen Sie die Segmentkomponente als Region oder deren Dialogfeld „Inhaltstypen“ hinzu*

   Entscheiden Sie, ob Sie bestimmte Regionen oder ein Segment vergleichen möchten, basierend auf den Inhaltstypen, die mit einer Region verknüpft sind.

   Wählen Sie **[!UICONTROL As a region]** aus, um dem Abschnitt **Regionen/Inhaltstypen** Bereiche hinzuzufügen.

   Wählen Sie **[!UICONTROL As its content types]** aus, um Inhaltstypen einer Region hinzuzufügen.

1. Ziehen Sie **Metriken** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Metriken** der **Segmentdefinition** ab.

   ![Wählen Sie einen Operator aus und legen Sie einen Wert für die hinzugefügte Metrik fest](assets/component-metrics.png)

   *Wählen Sie einen Operator aus und weisen Sie der hinzugefügten Metrik einen Wert zu*

   Nachdem Sie Metriken in der Segmentdefinition hinzugefügt haben, wählen Sie einen Operator aus **[!UICONTROL Select an operator]** Dropdown-Menü aus und weisen Sie einen Wert mithilfe von **[!UICONTROL Select an option]** zu.

   Passen Sie Werte für bestimmte Metriken an, indem Sie den Aufwärtspfeil verwenden, um zu erhöhen, und den Abwärtspfeil, um zu verringern.

1. Ziehen Sie **Berechnete Metriken** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Berechnete Metriken** der **Segmentdefinition** ab.

   ![Wählen Sie einen Operator aus und legen Sie einen Wert für die hinzugefügte berechnete Metrik fest](assets/component-calculated-metrics.png)

   *Wählen Sie einen Operator aus und weisen Sie der hinzugefügten berechneten Metrik einen Wert zu*

   Nachdem Sie der Segmentdefinition berechnete Metriken hinzugefügt haben, **[!UICONTROL Select an operator]** Sie aus dem Dropdown-Menü aus und weisen Sie mithilfe von **[!UICONTROL Select an option]** einen Wert zu.

   >[!NOTE]
   >
   >Alle Metriken und berechneten Metriken, die Sie unter der Segmentdefinition ablegen, werden von entsprechenden Operatoren begleitet, um den jeweiligen Metriken und berechneten Metriken Werte zuzuweisen.

1. Überprüfen Sie die Segmentdetails in der **Segmentzusammenfassung**, um zu entscheiden, welche Änderungen Sie im gesamten Segment implementieren möchten.
1. Wählen Sie **[!UICONTROL Last week]** oder **[!UICONTROL Last month]** aus dem Dropdown **Menü „Bewertungszeitraum**, um Zusammenfassungswerte für die vergangene Woche oder den vergangenen Monat zu schätzen.
1. Wählen Sie **[!UICONTROL Update estimation]** aus, um die Anzahl der geschätzten qualifizierten Konten im aktuellen Segment basierend auf dem ausgewählten Auswertungszeitraum zu berechnen.
1. Wählen Sie **[!UICONTROL Save segment]** aus.

Das von Ihnen erstellte Segment ist jetzt in der Segmentliste verfügbar.

+++

+++TV Everywhere

Um ein Segment zu erstellen und das Freigabeverhalten von Abonnenten zu analysieren, wählen Sie oben rechts **[!UICONTROL Create new segment]** aus.

![Wählen Sie Neues Segment erstellen aus](assets/create-new-segment.png)

*Wählen Sie Neues Segment erstellen aus*

Dadurch wird eine Seite **Neues Segment** geöffnet, die die folgenden Elemente enthält:

![Neue Segmentseite](assets/new-segment-dialog.png)

*Neue Segmentseite*

**A.** Segmentkomponenten **B.** Segmentdefinition **C.** Segmentzusammenfassung

* **Segmentkomponenten**: Ein Inventar von Programmierern und Kanälen, MVPDs, Metriken und berechneten Metriken, die zum Definieren eines Segments verwendet werden.

  >[!NOTE]
  >
  >Verwenden Sie **[!UICONTROL Show all]** , um die Liste der Segmentkomponenten zu erweitern. Um eine Komponente schnell zu finden, durchsuchen Sie ihren Namen **nach Segmentkomponenten suchen** anstatt in der gesamten Liste zu scrollen.

* **Segmentdefinition**: Eine Arbeitsfläche, auf der Sie verschiedene Segmentkomponenten per Drag-and-Drop verschieben können, um ein Segment zu erstellen.

* **Segmentzusammenfassung** Eine Zusammenfassung, die die qualifizierten Konten basierend auf den Komponenten in der Segmentdefinition schätzt und einen kurzen Überblick über das Segment während des Auswertungszeitraums bietet.

Führen Sie die folgenden Schritte aus, um ein Segment zu erstellen:

1. Geben Sie den Namen Ihres Segments in **Segmentname** ein, der in der Segmentliste und bei der Segmentauswahl angezeigt wird.
1. Geben Sie in „Segmentbeschreibung“ eine detaillierte Beschreibung **Segments**.
1. Ziehen Sie **Programmierer und Kanäle** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Programmierer/Kanäle** innerhalb der **Segmentdefinition** ab.

   >[!NOTE]
   >
   >Sie können ein Segment entweder auf Basis von Programmierern oder Kanälen erstellen. Zeigen Sie die zugehörigen Kanäle mit einem Programmierer aus einem Dropdown-Menü an.

   Wenn Sie zunächst einen **Kanal** im Abschnitt **Programmierer/Kanäle** hinzufügen, können Sie Kanäle nur als nachfolgende Komponenten hinzufügen.

   Wenn Sie zunächst einen **Programmierer** im Abschnitt **Programmierer/Kanäle** hinzufügen, wird ein Entscheidungsdialogfeld angezeigt.

   ![Hinzufügen der Segmentkomponente als Programmierer oder deren Kanäle ](assets/segment-basis-selector.png){width="550" align="left"}


   *Hinzufügen der Segmentkomponente als Programmierer oder im Dialogfeld „Kanäle“*

   Entscheiden Sie, ob Sie bestimmte Programmierer oder ein Segment vergleichen möchten, basierend auf den Kanälen, die mit einem Programmierer verknüpft sind.

   Wählen Sie **[!UICONTROL As a programmer]** aus, um dem Abschnitt **Programmierer/Kanäle** Programmierer hinzuzufügen.

   Wählen Sie **[!UICONTROL As its channels]** aus, um alle Kanäle eines Programmierers hinzuzufügen.

1. Ziehen Sie **MVPDs** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **MVPDs** innerhalb der **Segmentdefinition** ab.

   >[!NOTE]
   >
   >Wenn Sie sich als Programmierer anmelden, wird eine MVPD mit dem Namen **xfinity** als eigenständige Option im Abschnitt **MVPDs** angezeigt. Sie können sie nicht mit einer anderen MVPD kombinieren.

1. Ziehen Sie **Metriken** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Metriken** der **Segmentdefinition** ab.

   ![Wählen Sie einen Operator aus und legen Sie einen Wert für die hinzugefügte Metrik fest](assets/component-metrics.png)

   *Wählen Sie einen Operator aus und weisen Sie der hinzugefügten Metrik einen Wert zu*

   Nachdem Sie Metriken in der Segmentdefinition hinzugefügt haben, wählen Sie einen Operator aus **[!UICONTROL Select an operator]** Dropdown-Menü aus und weisen Sie einen Wert mithilfe von **[!UICONTROL Select an option]** zu.

   Passen Sie Werte für bestimmte Metriken an, indem Sie den Aufwärtspfeil verwenden, um zu erhöhen, und den Abwärtspfeil, um zu verringern.

1. Ziehen Sie **Berechnete Metriken** aus den Segmentkomponenten im linken Bereich und legen Sie sie im Abschnitt **Berechnete Metriken** der **Segmentdefinition** ab.

   ![Wählen Sie einen Operator aus und legen Sie einen Wert für die hinzugefügte berechnete Metrik fest](assets/component-calculated-metrics.png)

   *Wählen Sie einen Operator aus und weisen Sie der hinzugefügten berechneten Metrik einen Wert zu*

   Nachdem Sie der Segmentdefinition berechnete Metriken hinzugefügt haben, **[!UICONTROL Select an operator]** Sie aus dem Dropdown-Menü aus und weisen Sie mithilfe von **[!UICONTROL Select an option]** einen Wert zu.

   >[!NOTE]
   >
   >Alle Metriken und berechneten Metriken, die Sie unter der Segmentdefinition ablegen, werden von entsprechenden Operatoren begleitet, um den jeweiligen Metriken und berechneten Metriken Werte zuzuweisen.

1. Überprüfen Sie die Segmentdetails in der **Segmentzusammenfassung**, um zu entscheiden, welche Änderungen Sie im gesamten Segment implementieren möchten.
1. Wählen Sie **[!UICONTROL Last week]** oder **[!UICONTROL Last month]** aus dem Dropdown **Menü „Bewertungszeitraum**, um Zusammenfassungswerte für die vergangene Woche oder den vergangenen Monat zu schätzen.
1. Wählen Sie **[!UICONTROL Update estimation]** aus, um die Anzahl der geschätzten qualifizierten Konten im aktuellen Segment basierend auf dem ausgewählten Auswertungszeitraum zu berechnen.
1. Wählen Sie **[!UICONTROL Save segment]** aus.

Das von Ihnen erstellte Segment ist jetzt in der Segmentliste verfügbar.
+++

## Segmente verwalten {#manage-segments}

Sie können ein Segment aus der Segmentliste auswählen und dann die folgenden Aktionen ausführen:

* [Bearbeiten eines Segments](#edit-segment)
* [Duplizieren eines Segments](#duplicate-segment)
* [Segment löschen](#delete-segment)

![Bearbeiten, Duplizieren oder Löschen eines Segments](assets/manage-segments-list.png)

*Segment zum Bearbeiten, Duplizieren oder Löschen auswählen*

**A.** [Standardsegment](#work-with-segments) **B.** [Videokategorien](product-concepts.md#video-category-def)

>[!NOTE]
>
>Die in diesem Abschnitt angezeigten Videokategorien wie **MVPDs**, **Programmierer** und **Kanäle** stellen die Bezeichnungen dar, die in der Account IQ-Version von TV Everywhere verwendet werden. Wenn Sie als D2C-Service angemeldet sind, zeigen diese Kennzeichnungen die spezifischen Videokategorien Ihres Unternehmens an.

Sie können das Standardsegment mit dem Namen „Alle **&quot; in Ihren Eigenschaften nicht bearbeiten, duplizieren oder**.

### Bearbeiten eines Segments {#edit-segment}

1. Navigieren Sie zur Registerkarte **[!UICONTROL Segments]** unter **Aktionen** im linken Bereich, um eine Liste der Segmente anzuzeigen.
1. Wählen Sie ein Segment aus, das Sie bearbeiten möchten.
1. Wählen Sie **[!UICONTROL Edit]** aus.
1. Ändern Sie Segmentdetails wie Segmentnamen, Beschreibung oder Komponenten in der **Segmentdefinition**.

   >[!TIP]
   >
   >Verwenden Sie **[!UICONTROL Clear all]** , um alle Segmentkomponenten innerhalb jedes Abschnitts unter Segmentdefinition gleichzeitig zu entfernen. Alternativ können Sie die Kreuzschaltfläche auswählen, um einzelne Elemente zu entfernen.

   ![Löschen Sie alle Segmentkomponenten in jedem Abschnitt unter Segmentdefinition ](assets/clear-all-components.png)

   *Wählen Sie Alle löschen aus, um alle Segmentkomponenten gleichzeitig zu entfernen*

1. Wählen Sie entweder **[!UICONTROL Update segment]** aus, um das vorhandene Segment zu aktualisieren, oder **[!UICONTROL Save as new segment]**, um ein neues Segment mit den Änderungen zu erstellen.

   >[!NOTE]
   >
   >Das Aktualisieren von Segmenten, die derzeit Vorgänge durchlaufen, ist nicht zulässig. Änderungen als neues Segment zu speichern, ist die einzige Option für Segmente mit laufenden Vorgängen.

### Duplizieren eines Segments {#duplicate-segment}

1. Navigieren Sie zur Registerkarte **[!UICONTROL Segments]** unter **Aktionen** im linken Bereich, um eine Liste der Segmente anzuzeigen.
1. Wählen Sie ein Segment aus, das Sie duplizieren möchten.
1. Wählen Sie **[!UICONTROL Duplicate]** aus.

Eine Kopie des ausgewählten Segments wird generiert und am Ende der Segmentliste platziert. Sie können die erforderlichen Details im duplizierten Segment bearbeiten und dann entweder das doppelte Segment aktualisieren oder es als neues Segment speichern.

### Segment löschen {#delete-segment}

1. Navigieren Sie zur Registerkarte **[!UICONTROL Segments]** unter **Aktionen** im linken Bereich, um eine Liste der Segmente anzuzeigen.
1. Wählen Sie ein Segment aus, das Sie entfernen möchten.

   Wählen Sie mehrere Segmente aus, um sie in einem einzigen Vorgang zu löschen. Sie können auch links neben dem Segmentnamen ein Kontrollkästchen aktivieren **um** Segmente gleichzeitig zu löschen.

   >[!NOTE]
   >
   > Sie können nur mehr als ein Segment oder alle Segmente löschen, wenn keines der Segmente von Vorgängen verwendet wird. Darüber hinaus ist das Löschen des Standardsegments mit dem Namen **Alle Konten in Ihren**&quot; nicht zulässig. Sie bleibt deaktiviert, wenn Sie versuchen, alle Segmente gleichzeitig zu löschen.

   ![Löschen von mehr als einem Segment](assets/delete-more-than-one-segment.png)

   *Wählen Sie mehrere Segmente aus, um mehr als ein Segment zu löschen*

1. Wählen Sie **[!UICONTROL Delete]** aus.
1. Bestätigen Sie, dass im Dialogfeld **[!UICONTROL Delete]** werden soll, um das Segment dauerhaft zu entfernen.

   >[!NOTE]
   >
   >Das Segment wird dauerhaft aus dem System gelöscht und Sie können diese Aktion nicht rückgängig machen.
