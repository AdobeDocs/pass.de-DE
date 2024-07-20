---
title: Vorgänge für Konto-IQ
description: Der Betrieb in Account IQ umfasst die Durchführung von Automatisierungs- und Massenoperationen auf Abonnentenkonten und die Verfolgung ihrer Auswirkungen.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Aktivitäten {#operations-tab-next-steps}

Nachdem Sie die Nutzungsmuster Ihres Abonnenten analysiert und Instanzen der Kennwortfreigabe für ein ausgewähltes Segment mithilfe von [!DNL Account IQ] -Analyse identifiziert haben, können Sie gezielte Aktionen durch gezielte Verfahren durchführen, die in [!DNL Account IQ] als Vorgänge bezeichnet werden.

**Vorgänge** ermöglichen es Ihnen, die Freigabe von Anmeldedaten für eine Gruppe von Konten effektiv zu verfolgen und zu verwalten, um die Freigabe von Passwörtern zu verringern und das Erlebnis für geschätzte Abonnenten zu verbessern.

Sie können Aktionen auf ein definiertes [Segment](/help/accountiq/product-concepts.md#segment-def) anwenden, um die Kennwortfreigabe innerhalb eines bestimmten [Zeitintervalls](/help/accountiq/product-concepts.md#time-interval-def) zu adressieren und die Ausführung des Vorgangs für ein künftiges Datum zu planen. Diese Aktionen umfassen Einschränkungen zur Minimierung der Kennwortfreigabe oder zur Erleichterung von Einschränkungen für Nicht-Freigabe-Konten.

Mithilfe von Vorgängen können Sie nicht nur Aktionen und deren Umfang angeben, sondern auch deren Ergebnisse messen.

Indem Sie die Ergebnisse bewerten, können Sie Ihre Strategie verfeinern, um die Auswirkungen zu optimieren, sei es durch die Umwandlung von Kreditnehmern, die Verringerung der Kreditaufteilung oder die Verringerung der Abwanderung.

Sie können verschiedene Funktionen mit Vorgängen ausführen:

* [Anzeigen von Vorgangsberichten](#operation-reports)
* [Neuen Vorgang erstellen](#create-new-operation)
* [Stopp-Vorgang](#stop-operation)

## Anzeigen von Vorgangsberichten {#operation-reports}

Sie können die Auswirkungen eines Vorgangs mithilfe von Vorgangsberichten überprüfen. Um den Vorgangsbericht anzuzeigen, wählen Sie die Registerkarte **Vorgänge** unter **Aktionen** im linken Bereich der Account IQ-Anwendung aus. Eine Liste der im System verfügbaren Vorgänge wird angezeigt. Sie können auf wichtige Details zu jedem Vorgang in einem tabellarischen Format zugreifen. Zu den Details gehören:

* Name des Vorgangs
* Aktueller Status (wie &quot;Geplant&quot;, &quot;Wird ausgeführt&quot;, &quot;Beendet&quot;, &quot;Fehler&quot;oder &quot;Angehalten&quot;)
* Prozentsatz für den Fortschrittsabschluss
* Zielgruppe oder Segment, auf das der Vorgang angewendet wird
* Aktionstyp, der für den Vorgang ausgewählt wurde
* Startdatum des Vorgangs
* Enddatum des Vorgangs
* Datum der Erstellung des Vorgangs
* Datum der letzten Änderung des Vorgangs

![](assets/operations-page.png)

*Liste und Details der in Account IQ vorhandenen Vorgänge*

Wählen Sie den gewünschten **Vorgangsnamen** aus der Liste der Vorgänge aus. Die folgenden Berichte werden angezeigt:

### Vorgangsleistung {#operation-performance}

Die Vorgangsergebnisse enthalten eine Zusammenfassung der Anzahl der betroffenen Konten, des Vorgangsfortschritts und des Gesamt-Teilungswerts der Konten im Segment während des [Auswertungszeitraums des Vorgangs](/help/accountiq/product-concepts.md#evaluation-period-def).

![Bericht zur Vorgangsleistung](assets/operation-performance.png)

*Bericht zur Vorgangsleistung*

**A.** Betroffene Konten **B.** Vorgangsfortschritt **C.** Gesamtwert der Freigabe

#### Betroffene Konten {#impacted-accounts}

Diese Zahl zeigt die Anzahl der Abonnentenkonten an, die von der während des Evaluierungszeitraums des Vorgangs durchgeführten Aktion betroffen sind.

#### Vorgangsfortschritt {#operation-progress}

Dieser Messwert gibt die Anzahl der Tage und den Prozentsatz des Vorgangs an, der außerhalb des geplanten Zeitplans abgeschlossen wurde.

#### Gesamte Teilungsbewertung {#overall-sharing-score}

Dieses Liniendiagramm stellt den [Gesamt-Sharing-Wert](/help/accountiq/data-panels.md#overall-sharing-score) dar, der die Teilungsebene und Nutzung von freigegebenen Konten in jeder Woche während des Auswertungszeitraums des Vorgangs umfasst.

### Auswirkungen der Vorgänge: Konten im Segment {#impact-accounts}

Dieser Bericht wird als gestapeltes Spaltendiagramm angezeigt, das die Auswirkungen eines Vorgangs im Zeitverlauf veranschaulicht.

![Auswirkungen des Vorgangs auf Konten im Segmentdiagramm](assets/accounts-in-segment.png)

*Auswirkungen des Vorgangs auf Konten im Segmentdiagramm*

Die X-Achse stellt den [Auswertungszeitraum](/help/accountiq/product-concepts.md#evaluation-period-def) des Vorgangs dar, während die Y-Achse den Status der Konten im Segment des Vorgangs angibt. Jeder Balken im Diagramm ist in drei Farben unterteilt:

* Rosa stellt die Anzahl der Konten dar, die die Bedingungen des Segments für diesen Vorgang erfüllen.

* Blau stellt die Anzahl der aktiven Konten dar, die sich ursprünglich im Segment befanden, aber die Bedingungen des Segments während der einzelnen Wochen oder Monate im [Auswertungszeitraum des Vorgangs](/help/accountiq/product-concepts.md#evaluation-period-def) nicht erfüllten.

* Grau stellt die Konten dar, die während des Bewertungszeitraums inaktiv waren.

>[!NOTE]
>
>Der erste rosa Balken gibt die Anzahl der Konten an, die zu Beginn des Bewertungszeitraums die Bedingungen des Vorgangssegments erfüllen.

Im Laufe der Zeit zeigt das Diagramm Änderungen im Kontoverhalten im Vergleich zu den ursprünglichen Kriterien (z. B. eine Freigabewahrscheinlichkeit von über 90 und die Verwendung von mehr als 5 Geräten ist inaktiv geworden).

### Auswirkungen von Vorgängen: Metriken für freigegebene Konten {#impact-shared-accounts}

Die Metriken für freigegebene Konten bieten einen Überblick über die Teilungs- und Wiedergabeanforderungen der Abonnentenkonten im Segment des Vorgangs während des [Auswertungszeitraums des Vorgangs](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Freigabestufe {#share-level}

Dieses Liniendiagramm stellt die wöchentliche [Teilungsebene](/help/accountiq/data-panels.md#sharing-level) für den Auswertungszeitraum des Vorgangs dar.

![Kantengraph auf Teilungsebene](assets/share-level.png){width="550" align="left"}

*Kantengraph auf Teilungsebene*

#### Anzahl der Wiedergabeanforderungen {#play-requests}

Dieses Kantengraph stellt die [Wiedergabeanforderungen](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) dar, die jede Woche im Auswertungszeitraum des Vorgangs gesendet werden.

![Anzahl der Wiedergabeanforderungen, Kantengraph](assets/number-play-requests.png){width="550" align="left"}

*Anzahl der Wiedergabeanforderungen, Kantengraph*

### Auswirkung auf Vorgänge: Allgemeine Nutzungsmetriken {#impact-general-usage}

Die allgemeinen Nutzungsmetriken geben einen Überblick über die durchschnittliche Anzahl der Geräte, IPs und Standorte im Segment des Vorgangs während des [Auswertungszeitraums](/help/accountiq/product-concepts.md#evaluation-period-def) des Vorgangs.

#### Anzahl der Geräte {#devices}

Dieses Kantengraph stellt die durchschnittliche [Anzahl der Geräte](/help/accountiq/general-usage-reports.md#devices-week-account) dar, die jede Woche im Auswertungszeitraum des Vorgangs eingesetzt werden.

![Anzahl der Gerätezeilendiagramme](assets/number-devices.png){width="550" align="left"}

*Anzahl der Gerätezeilendiagramme*

#### Anzahl der IPs und Standorte {#IPs-locations}

Dieses Liniendiagramm stellt die durchschnittliche [Anzahl der IPs](/help/accountiq/general-usage-reports.md#ip-week-account) und [Standorte](/help/accountiq/general-usage-reports.md#locations-week-account) pro Woche im Auswertungszeitraum des Vorgangs dar.

![Liniendiagramm für die Anzahl der IPs und Standorte](assets/number-ips-locations.png){width="550" align="left"}

*Liniendiagramm für die Anzahl der IPs und Standorte*

Um den Bericht zu schließen und zur Hauptseite **Vorgänge** zurückzukehren, wählen Sie im linken Bereich unter **Aktionen** die Registerkarte **Vorgänge** aus.

## Neuen Vorgang erstellen {#create-new-operation}

Wenn Sie auf die Registerkarte **Vorgänge** unter **Aktionen** im linken Bereich gehen, wählen Sie oben auf der Seite **Vorgänge** die Option **Neuen Vorgang erstellen** aus.

Um einen neuen Vorgang zu erstellen, befolgen Sie die Anweisungen in den folgenden Abschnitten:

* [Vorgangsdetails](#operation-details)
* [Segment](#segment)
* [Aktion](#action)
* [Zeitplan](#schedule)

### Vorgangsdetails {#operation-details}

Geben Sie in diesem Abschnitt den Namen für den Vorgang in **Vorgangsname** ein.

>[!TIP]
>
>Beschreiben Sie den Zweck des Vorgangs oder die Art der Aktion in **Vorgangsname** zur schnellen Identifizierung. Die Option &quot;**Beschreibung und Tags hinzufügen**&quot;wird in zukünftigen Versionen verfügbar sein.

![Hinzufügen des Vorgangsnamens in Vorgangsdetails](assets/operation-details.png)

*Vorgangsname hinzufügen*

### Segment {#segment}

Klicken Sie in diesem Abschnitt auf **Segment auswählen** und wählen Sie ein Segment aus, für das Sie diesen Vorgang verwenden möchten. Erfahren Sie, wie Sie [ein Segment auswählen](/help/accountiq/segments-timeinterval.md#segment-selection).

Verwenden Sie nach Auswahl eines Segments <img alt= "Segmentzusammenfassung erweitern" src="./assets/expand-segment-summary.svg" width="25"> -Symbol, um die detaillierte Segmentzusammenfassung anzuzeigen. Weitere Informationen zu [Segmentzusammenfassung](segments-timeinterval.md#segment-summary).

![Segment und Zeitintervall auswählen](assets/select-segment-timeinterval.png)

*Segment und Zeitintervall auswählen*

>[!NOTE]
>
>Die im vorherigen Bild angezeigten [Videokategorien](product-concepts.md#video-category-def), z. B. **MVPDs**, **Programmierer** und **Kanäle**, stellen die Beschriftungen dar, die in der TV-Version von Account IQ verwendet werden. Wenn Sie als D2C-Dienst angemeldet sind, zeigen diese Beschriftungen die spezifischen Videokategorien Ihres Unternehmens an.

Verwenden Sie bei Bedarf <img alt= "Segment bearbeiten" src="./assets/edit-segment.svg" width="25"> Symbol zum Bearbeiten des ausgewählten Segments oder  <img alt= "Neues Segment erstellen" src="./assets/create-new-segment.svg" width="25"> Symbol, um ein neues Segment zu erstellen. Weitere Informationen finden Sie in den Anweisungen zum [Erstellen eines neuen Segments](work-with-segments.md#create-new-segment) oder [Bearbeiten eines Segments](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**Der Segmenttyp** mit dem Namen **[!UICONTROL Fixed number of accounts]** ist derzeit standardmäßig ausgewählt. Die Option &quot;**[!UICONTROL Variable number of accounts]**&quot; wird in den kommenden Versionen verfügbar sein.

Wählen Sie **Granularität und Zeitintervall** aus, um den Vorgang während eines bestimmten Zeitraums zu überwachen. Erfahren Sie mehr über [die Auswahl von Granularität und Zeitintervall](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Aktion {#action}

Wählen Sie in diesem Abschnitt eine **Aktion** aus, die Sie für das ausgewählte Segment im Dropdown-Menü ausführen möchten.

![Wählen Sie den Aktionstyp aus](assets/apply-actions.png)

*Wählen Sie den Aktionstyp aus*

Es stehen zwei Optionen zur Verfügung:

* Wählen Sie **CM Policy** für das in Account IQ integrierte gleichzeitige Überwachungssystem.

* Wählen Sie **Externe Aktionen** aus, um Workflows außerhalb von Account IQ zu erstellen und zu verarbeiten, die nicht in das Account IQ-System integriert sind.

>[!NOTE]
>
>Externe Aktionen beziehen sich möglicherweise nicht immer direkt auf die Freigabe von Passwörtern, können sich aber dennoch auf sie auswirken, wie z. B. die Einführung einer neuen Saison.

### Zeitplan {#schedule}

Wählen Sie in diesem Abschnitt aus der Datumsauswahl das **Startdatum** und das **Enddatum** aus, um die Aktivierung für den Vorgang festzulegen.

>[!IMPORTANT]
>
>Derzeit sind die standardmäßige Aktivierung **Startdatum** und **Enddatum** auf **On date** eingestellt. Die Option zur Auswahl von **Wenn eine Bedingung erfüllt ist** und **Manuell** ist in den kommenden Versionen verfügbar.

>[!NOTE]
>
>Stellen Sie sicher, dass sowohl das Start- als auch das Enddatum mit der Granularität übereinstimmen, die für die Auswertung in **Schritt 4** ausgewählt wurde.

* Wenn Sie sich für eine Granularität entschieden haben, die nach Wochen aggregiert wird, wählen Sie das Start- und Enddatum in Wochen aus (z. B. Woche 10).
* Wenn Sie sich für eine nach Monaten aggregierte Granularität entschieden haben, wählen Sie das Start- und Enddatum in Monaten aus.

![Wählen Sie das Start- und Enddatum aus der Datumsauswahl](assets/add-schedule.png) aus.

*Wählen Sie Start- und Enddatum aus der Datumsauswahl* aus.

**A.** Startdatumsauswahl **B.** Enddatumsauswahl

>[!NOTE]
>
>Das **Startdatum** muss nach dem Auswertungszeitraum und dem aktuellen Datum liegen, während das **Enddatum** nach dem Startdatum und dem aktuellen Datum liegen muss, damit Vorgänge im zukünftigen Zeitraum geplant und ausgeführt werden können.

Wählen Sie oben auf der Seite **Vorgänge** den Befehl **Speichervorgang** aus, um einen neuen Vorgang zu verarbeiten.

## Stopp-Vorgang {#stop-operation}

Sie können nur die Vorgänge stoppen, die sich derzeit im Status **Wird ausgeführt** befinden. Gehen Sie wie folgt vor, um einen vorhandenen Vorgang zu stoppen:

1. Navigieren Sie im linken Navigationsbereich der Account IQ-Anwendung zur Registerkarte **Vorgänge** unter **Aktionen** .
1. Wählen Sie das Menü **Optionen** des Vorgangs aus, den Sie stoppen möchten.

   ![Menü &quot;Optionen&quot;auswählen, um den Vorgang zu stoppen](assets/stop-operation.png)

   *Menü &quot;Optionen&quot;auswählen, um den Vorgang zu stoppen*

1. Wählen Sie **Stopp** aus.



