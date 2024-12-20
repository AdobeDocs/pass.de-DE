---
title: Vorgänge in Konto-IQ
description: Vorgänge in Account IQ beinhalten Maßnahmen zur Durchführung von Automatisierungen und Massenvorgängen für Abonnentenkonten und zur Nachverfolgung ihrer Auswirkungen.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Vorgänge {#operations-tab-next-steps}

Nachdem Sie die Nutzungsmuster Ihrer Abonnenten analysiert und Instanzen der Passwortfreigabe für ein ausgewähltes Segment mithilfe von [!DNL Account IQ] Analytics identifiziert haben, können Sie zielgerichtete Aktionen durch fokussierte Verfahren durchführen, die als Vorgänge in [!DNL Account IQ] bezeichnet werden.

**Vorgänge** ermöglichen es Ihnen, die Freigabe von Anmeldeinformationen für eine Gruppe von Konten effektiv zu verfolgen und zu verwalten, um die Freigabe von Passwörtern zu vermindern und das Erlebnis für wertvolle Abonnenten zu verbessern.

Sie können Aktionen auf ein definiertes [Segment“ anwenden](/help/accountiq/product-concepts.md#segment-def) um die Passwortfreigabe innerhalb eines bestimmten [Zeitintervalls“ ](/help/accountiq/product-concepts.md#time-interval-def) adressieren und den Vorgang so planen, dass er zu einem späteren Zeitpunkt ausgeführt wird. Zu diesen Aktionen gehören Einschränkungen, um die Freigabe von Passwörtern zu minimieren oder Einschränkungen für Nicht-Freigabe-Konten zu vereinfachen.

Mit Vorgängen können Sie nicht nur Aktionen und deren Umfang angeben, sondern auch deren Ergebnisse messen.

Durch die Auswertung der Ergebnisse können Sie Ihre Strategie verfeinern, um die Auswirkungen zu optimieren, indem Sie Kreditnehmer konvertieren, die Freigabe von Anmeldeinformationen reduzieren oder die Abwanderung verringern.

Sie können verschiedene Funktionen mit Vorgängen ausführen:

* [Vorgangsberichte anzeigen](#operation-reports)
* [Neuen Vorgang erstellen](#create-new-operation)
* [Stopp-Betrieb](#stop-operation)

## Vorgangsberichte anzeigen {#operation-reports}

Sie können die Auswirkungen eines Vorgangs in Vorgangsberichten überprüfen. Um den Vorgangsbericht anzuzeigen, wählen Sie **Vorgänge** Registerkarte unter **Aktionen** im linken Bereich der Account IQ-Anwendung aus. Eine Liste der im System verfügbaren Vorgänge wird angezeigt. Sie können auf wichtige Details zu jedem Vorgang im Tabellenformat zugreifen. Zu den Details gehören:

* Name des Vorgangs
* Aktueller Status (z. B. Geplant, Wird ausgeführt, Beendet, Fehler oder Gestoppt)
* Fortschritt - Fertigstellungsprozentsatz
* Zielgruppe oder Segment, auf die der Vorgang angewendet wird
* Für den Vorgang ausgewählter Aktionstyp
* Startdatum des Vorgangs
* Enddatum des Vorgangs
* Datum, an dem der Vorgang erstellt wurde
* Datum der letzten Änderung des Vorgangs

![](assets/operations-page.png)

*Liste und Details der bestehenden Vorgänge in Account IQ*

Wählen Sie den gewünschten **Vorgangsnamen** aus der Liste der Vorgänge aus. Die folgenden Berichte werden angezeigt:

### Betriebsleistung {#operation-performance}

Die Vorgangsleistung bietet ein Auslesen der obersten Zeile, bei dem die Anzahl der betroffenen Konten, der Vorgangsfortschritt und der Gesamtwert für die Freigabe der Konten im Segment während des [ des Vorgangs zusammengefasst ](/help/accountiq/product-concepts.md#evaluation-period-def).

![Bericht zur Betriebsleistung](assets/operation-performance.png)

*Bericht zur Betriebsleistung*

**A.** Betroffene Konten **B.** Vorgangsfortschritt **C.** Gesamt-Freigabewert

#### Betroffene Konten {#impacted-accounts}

Diese Zahl zeigt die Anzahl der abonnierten Konten an, die von der während des Auswertungszeitraums des Vorgangs durchgeführten Aktion betroffen sind.

#### Fortschritt des Vorgangs {#operation-progress}

Dieses Messgerät zeigt die Anzahl der Tage und den Prozentsatz des Vorgangs an, der außerhalb des geplanten Zeitplans abgeschlossen wurde.

#### Gesamtergebnis bei Freigabe {#overall-sharing-score}

Dieses Liniendiagramm stellt den [Gesamt-Freigabewert](/help/accountiq/data-panels.md#overall-sharing-score) dar, der die Freigabeebene und Nutzung von freigegebenen Konten in jeder Woche während des Auswertungszeitraums des Vorgangs umfasst.

### Auswirkungen des Vorgangs: Konten im Segment {#impact-accounts}

Dieser Bericht wird als gestapeltes Spaltendiagramm angezeigt, das die Auswirkungen eines Vorgangs im Zeitverlauf veranschaulicht.

![Auswirkungen des Vorgangs auf Konten im Segmentdiagramm](assets/accounts-in-segment.png)

*Auswirkungen des Vorgangs auf Konten im Segmentdiagramm*

Die X-Achse stellt den [ des Vorgangs dar](/help/accountiq/product-concepts.md#evaluation-period-def) während die Y-Achse den Status der Konten im Segment des Vorgangs angibt. Jeder Balken im Diagramm ist in drei Farben unterteilt:

* Rosa steht für die Anzahl der Konten, die die in diesem Vorgang verwendeten Bedingungen des Segments erfüllen.

* Blau stellt die Anzahl der aktiven Konten dar, die sich ursprünglich im Segment befanden, aber die Bedingungen des Segments während jeder Woche oder jedes Monats im (Evaluierungs[Zeitraum des Vorgangs nicht ](/help/accountiq/product-concepts.md#evaluation-period-def).

* Grau stellt die Konten dar, die während des Auswertungszeitraums inaktiv waren.

>[!NOTE]
>
>Der erste rosa Balken stellt die Anzahl der Konten dar, die die Bedingungen des Vorgangssegments zu Beginn des Auswertungszeitraums erfüllen.

Im Laufe der Zeit zeigt das Diagramm Änderungen im Account-Verhalten relativ zu den ursprünglichen Kriterien an (z. B. hat eine Sharing-Wahrscheinlichkeit von mehr als 90 und bei Verwendung von mehr als 5 Geräten ist es inaktiv geworden).

### Auswirkungen des Vorgangs: Metriken für freigegebene Konten {#impact-shared-accounts}

Die Metriken Freigegebene Konten bieten einen Überblick über die Freigabeebenen- und Wiedergabeanforderungen, die von den Abonnentenkonten im Segment des Vorgangs während des [ des Vorgangs ](/help/accountiq/product-concepts.md#evaluation-period-def) werden.

#### Freigabeebene {#share-level}

Dieses Liniendiagramm stellt die [Freigabeebene](/help/accountiq/data-panels.md#sharing-level) jede Woche über den Auswertungszeitraum des Vorgangs dar.

![Liniendiagramm auf Freigabeebene](assets/share-level.png){width="550" align="left"}

*Liniendiagramm auf Freigabeebene*

#### Anzahl der Wiedergabeanforderungen {#play-requests}

Dieses Liniendiagramm stellt die [Wiedergabeanforderungen](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) jede Woche im Auswertungszeitraum des Vorgangs dar.

![Liniendiagramm für die Anzahl der Wiedergabeanforderungen](assets/number-play-requests.png){width="550" align="left"}

*Liniendiagramm für die Anzahl der Wiedergabeanforderungen*

### Auswirkungen auf den Betrieb: allgemeine Nutzungsmetriken {#impact-general-usage}

Die allgemeinen Nutzungsmetriken bieten einen Überblick über die durchschnittliche Anzahl der Geräte, IPs und Standorte im Segment des Vorgangs während des [ des Vorgangs](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Anzahl der Geräte {#devices}

Dieses Liniendiagramm stellt den Durchschnitt ([ Anzahl der Geräte](/help/accountiq/general-usage-reports.md#devices-week-account) jede Woche im Auswertungszeitraum des Vorgangs dar.

![Anzahl der Geräte Liniendiagramm](assets/number-devices.png){width="550" align="left"}

*Anzahl der Geräte Liniendiagramm*

#### Anzahl der IPs und Speicherorte {#IPs-locations}

Dieses Liniendiagramm stellt den Durchschnitt ([ Anzahl der IPs](/help/accountiq/general-usage-reports.md#ip-week-account) und [Standorte](/help/accountiq/general-usage-reports.md#locations-week-account) pro Woche im Auswertungszeitraum des Vorgangs dar.

![Anzahl der IPs und Standorte Liniendiagramm](assets/number-ips-locations.png){width="550" align="left"}

*Anzahl der IPs und Standorte Liniendiagramm*

Um den Bericht zu schließen und zur Hauptseite **Vorgänge** zurückzukehren, wählen Sie **Vorgänge** unter **Aktionen** im linken Bereich aus.

## Neuen Vorgang erstellen {#create-new-operation}

Wenn Sie im linken Bereich auf **Vorgänge** unter **Aktionen** wechseln, wählen Sie **Neuen Vorgang erstellen** oben auf der Seite **Vorgänge** aus.

Um einen neuen Vorgang zu erstellen, befolgen Sie die Anweisungen in den folgenden Abschnitten:

* [Vorgangsdetails](#operation-details)
* [Segment](#segment)
* [Aktion](#action)
* [Zeitplan](#schedule)

### Vorgangsdetails {#operation-details}

Geben Sie in diesem Abschnitt den Namen für den Vorgang in &quot;**&quot;**.

>[!TIP]
>
>Beschreiben Sie den Zweck des Vorgangs oder die Art der Aktion in **Vorgangsname** zur schnellen Identifizierung. Die Option **Beschreibung und Tags hinzufügen** wird in zukünftigen Versionen verfügbar sein.

![Vorgangsnamen in Vorgangsdetails hinzufügen](assets/operation-details.png)

*Vorgangsnamen hinzufügen*

### Segment {#segment}

Klicken Sie in diesem Abschnitt auf **Segment auswählen** und wählen Sie ein Segment aus, für das Sie diesen Vorgang verwenden möchten. Erfahren Sie [wie Sie ein Segment auswählen](/help/accountiq/segments-timeinterval.md#segment-selection).

Nachdem Sie ein Segment ausgewählt haben, verwenden Sie <img alt= "Segmentzusammenfassung erweitern" src="./assets/expand-segment-summary.svg" width="25"> Symbol zum Anzeigen der detaillierten Segmentzusammenfassung. Weitere Informationen über [Segmentzusammenfassung](segments-timeinterval.md#segment-summary).

![Segment und Zeitintervall auswählen](assets/select-segment-timeinterval.png)

*Segment und Zeitintervall auswählen*

>[!NOTE]
>
>Die [Videokategorien](product-concepts.md#video-category-def) wie **MVPDs**, **Programmierer** und **Kanäle** stellen die in der Account IQ-Version von TV Everywhere verwendeten Beschriftungen dar. Wenn Sie als D2C-Service angemeldet sind, zeigen diese Kennzeichnungen die spezifischen Videokategorien Ihres Unternehmens an.

Verwenden Sie bei Bedarf <img alt= "Segment bearbeiten" src="./assets/edit-segment.svg" width="25"> Symbol zum Bearbeiten des ausgewählten Segments oder  <img alt= "Neues Segment erstellen" src="./assets/create-new-segment.svg" width="25"> Symbol zum Erstellen eines neuen Segments. Weitere Informationen finden Sie in den Anweisungen zum [Erstellen eines neuen Segments](work-with-segments.md#create-new-segment) oder [Bearbeiten eines Segments](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**Segmenttyp** namens **[!UICONTROL Fixed number of accounts]** ist derzeit standardmäßig ausgewählt. Die Option zur Auswahl von **[!UICONTROL Variable number of accounts]** wird in kommenden Versionen verfügbar sein.

Wählen Sie **Granularität und Zeitintervall** aus, um den Vorgang während eines bestimmten Zeitraums zu überwachen. Erfahren Sie mehr über [Auswahl der Granularität und des Zeitintervalls](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Aktion {#action}

Wählen Sie in diesem Abschnitt eine **Aktion** aus, die Sie für das ausgewählte Segment aus dem Dropdown-Menü ausführen möchten.

![Wählen Sie den Aktionstyp aus](assets/apply-actions.png)

*Wählen Sie den Aktionstyp aus*

Es stehen zwei Optionen zur Verfügung:

* Wählen Sie **CM-Richtlinie** für das in Account IQ integrierte System zur Überwachung von gleichzeitigen Zugriffen aus.

* Wählen Sie **Externe Aktionen** aus, um Workflows außerhalb von Account IQ und nicht in Account IQ zu erstellen und zu verarbeiten.

>[!NOTE]
>
>Externe Aktionen stehen möglicherweise nicht immer in direktem Zusammenhang mit der Passwortfreigabe, können sich aber dennoch darauf auswirken, z. B. der Start einer neuen Saison.

### Zeitplan {#schedule}

Wählen Sie in diesem Abschnitt **Startdatum** und **Enddatum** aus der Datumsauswahl aus, um die Aktivierung für den Vorgang festzulegen.

>[!IMPORTANT]
>
>Derzeit sind die standardmäßigen Aktivierungswerte **Startdatum** und **Enddatum** auf &quot;**&quot;**. Die Option zum Auswählen **Wenn eine Bedingung erfüllt ist** und **Manuell** wird in kommenden Versionen verfügbar sein.

>[!NOTE]
>
>Stellen Sie sicher, dass sowohl das Start- als auch das Enddatum mit der Granularität übereinstimmen, die für die Bewertung in **Schritt 4) ausgewählt**.

* Wenn Sie sich für eine Granularität entschieden haben, die nach Wochen aggregiert wird, wählen Sie das Start- und Enddatum in Wochen aus (z. B. Woche 10).
* Wenn Sie sich für eine Granularität, aggregiert nach Monaten, entschieden haben, wählen Sie das Start- und Enddatum in Monaten aus.

![Wählen Sie in der Datumsauswahl das Start- und das Enddatum aus](assets/add-schedule.png)

*Wählen Sie in der Datumsauswahl Startdatum und Enddatum aus*

**A.** Datumsauswahl für Start **B.** Datumsauswahl für Ende

>[!NOTE]
>
>Das **Startdatum** muss nach dem Auswertungszeitraum und dem aktuellen Datum liegen, während das **Enddatum** nach dem Startdatum und dem aktuellen Datum liegen muss, um Vorgänge in der zukünftigen Periode zu planen und auszuführen.

Wählen **oben auf der Seite** Vorgänge **die Option „Speichervorgang** aus, um einen neuen Vorgang zu verarbeiten.

## Stopp-Betrieb {#stop-operation}

Sie können nur Vorgänge stoppen, die sich derzeit im Status **Wird ausgeführt** befinden. Gehen Sie wie folgt vor, um einen vorhandenen Vorgang anzuhalten:

1. Navigieren Sie zur **Vorgänge** unter **Aktionen** im linken Navigationsbereich der Account IQ-Anwendung.
1. Wählen Sie **Menü** Optionen“ des Vorgangs aus, den Sie stoppen möchten.

   ![Optionsmenü auswählen, um den Vorgang anzuhalten](assets/stop-operation.png)

   *Optionsmenü auswählen, um den Vorgang anzuhalten*

1. Wählen Sie **STOP** aus.



