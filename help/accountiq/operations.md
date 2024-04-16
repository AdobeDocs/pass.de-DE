---
title: Vorgänge für Konto-IQ
description: Der Betrieb von Account IQ umfasst die Durchführung von Aktionen zur Durchführung von Automatisierungen und Massenvorgängen auf Abonnentenkonten und zur Verfolgung ihrer Auswirkungen.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Aktivitäten {#operations-tab-next-steps}

Nachdem Sie die Nutzungsmuster Ihres Abonnenten analysiert und Instanzen der Kennwortfreigabe für ein ausgewähltes Segment mithilfe von [!DNL Account IQ] Analytics können Sie zielgerichtete Aktionen durch gezielte Verfahren, die als Vorgänge bezeichnet werden, in [!DNL Account IQ].

**Aktivitäten** ermöglichen es Ihnen, die Freigabe von Anmeldedaten für eine Gruppe von Konten effektiv zu verfolgen und zu verwalten, um die Freigabe von Passwörtern zu reduzieren und das Erlebnis für geschätzte Abonnenten zu verbessern.

Sie können Aktionen auf eine definierte [Segment](/help/accountiq/product-concepts.md#segment-def) , um die Kennwortfreigabe innerhalb einer bestimmten [Zeitintervall](/help/accountiq/product-concepts.md#time-interval-def) und die Ausführung des Vorgangs zu einem späteren Zeitpunkt planen. Diese Aktionen umfassen Einschränkungen zur Minimierung der Kennwortfreigabe oder zur Erleichterung von Einschränkungen für Nicht-Freigabe-Konten.

Mithilfe von Vorgängen können Sie nicht nur Aktionen und deren Umfang angeben, sondern auch deren Ergebnisse messen.

Indem Sie die Ergebnisse bewerten, können Sie Ihre Strategie verfeinern, um die Auswirkungen zu optimieren, sei es durch die Umwandlung von Kreditnehmern, die Verringerung der Kreditaufteilung oder die Verringerung der Abwanderung.

Sie können verschiedene Funktionen mit Vorgängen ausführen:

* [Anzeigen von Vorgangsberichten](#operation-reports)
* [Neuen Vorgang erstellen](#create-new-operation)
* [Stopp-Vorgang](#stop-operation)

## Anzeigen von Vorgangsberichten {#operation-reports}

Sie können die Auswirkungen eines Vorgangs mithilfe von Vorgangsberichten überprüfen. Um den Vorgangsbericht anzuzeigen, wählen Sie **Aktivitäten** Registerkarte unter **Aktionen** im linken Bereich der Konto-IQ-Anwendung. Eine Liste der im System verfügbaren Vorgänge wird angezeigt. Sie können auf wichtige Details zu jedem Vorgang in einem tabellarischen Format zugreifen. Zu den Details gehören:

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

*Liste und Details vorhandener Vorgänge in Konto IQ*

Wählen Sie die gewünschte **Vorgangsname** aus der Liste der Vorgänge. Die folgenden Berichte werden angezeigt:

### Vorgangsleistung {#operation-performance}

Die Vorgangsergebnisse enthalten eine Zusammenfassung der Anzahl der betroffenen Konten, des Vorgangsfortschritts und der allgemeinen Teilungsbewertung der Konten im Segment während der Operation [Bewertungszeitraum](/help/accountiq/product-concepts.md#evaluation-period-def).

![Betriebsleistungsbericht](assets/operation-performance.png)

*Betriebsleistungsbericht*

**A.** Betroffene Konten **B.** Vorgangsfortschritt **C.** Gesamte Teilungsbewertung

#### Betroffene Konten {#impacted-accounts}

Diese Zahl zeigt die Anzahl der Abonnentenkonten an, die von der während des Evaluierungszeitraums des Vorgangs durchgeführten Aktion betroffen sind.

#### Vorgangsfortschritt {#operation-progress}

Dieser Messwert gibt die Anzahl der Tage und den Prozentsatz des Vorgangs an, der außerhalb des geplanten Zeitplans abgeschlossen wurde.

#### Gesamte Teilungsbewertung {#overall-sharing-score}

Dieses Liniendiagramm stellt die [Gesamte Teilungsbewertung](/help/accountiq/data-panels.md#overall-sharing-score), der die Teilungsebene und die Nutzung von freigegebenen Konten in jeder Woche während des Auswertungszeitraums des Vorgangs umfasst.

### Auswirkungen der Vorgänge: Konten im Segment {#impact-accounts}

Dieser Bericht wird als gestapeltes Spaltendiagramm angezeigt, das die Auswirkungen eines Vorgangs im Zeitverlauf veranschaulicht.

![Auswirkungen von Vorgängen auf Konten in Segmentdiagrammen](assets/accounts-in-segment.png)

*Auswirkungen von Vorgängen auf Konten in Segmentdiagrammen*

Die X-Achse stellt die [Bewertungszeitraum](/help/accountiq/product-concepts.md#evaluation-period-def), während die Y-Achse den Status der Konten im Segment des Vorgangs angibt. Jeder Balken im Diagramm ist in drei Farben unterteilt:

* Rosa stellt die Anzahl der Konten dar, die die Bedingungen des Segments für diesen Vorgang erfüllen.

* Blau stellt die Anzahl der aktiven Konten dar, die sich ursprünglich im Segment befanden, aber die Bedingungen des Segments in den einzelnen Wochen oder Monaten in der Operation nicht erfüllten. [Bewertungszeitraum](/help/accountiq/product-concepts.md#evaluation-period-def).

* Grau stellt die Konten dar, die während des Bewertungszeitraums inaktiv waren.

>[!NOTE]
>
>Der erste rosa Balken gibt die Anzahl der Konten an, die zu Beginn des Bewertungszeitraums die Bedingungen des Vorgangssegments erfüllen.

Im Laufe der Zeit zeigt das Diagramm Änderungen im Kontoverhalten im Vergleich zu den ursprünglichen Kriterien (z. B. eine Freigabewahrscheinlichkeit von über 90 und die Verwendung von mehr als 5 Geräten ist inaktiv geworden).

### Auswirkungen von Vorgängen: Metriken für freigegebene Konten {#impact-shared-accounts}

Die Metriken für freigegebene Konten bieten einen Überblick über die Teilungs- und Wiedergabeanforderungen der Abonnentenkonten im Segment des Vorgangs während der Operation [Bewertungszeitraum](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Freigabestufe {#share-level}

Dieses Liniendiagramm stellt die [Freigabestufe](/help/accountiq/data-panels.md#sharing-level) jede Woche über den Evaluierungszeitraum des Vorhabens.

![Kantengraph auf Teilungsebene](assets/share-level.png){width="550" align="left"}

*Kantengraph auf Teilungsebene*

#### Anzahl der Wiedergabeanforderungen {#play-requests}

Dieses Liniendiagramm stellt die [Wiedergabeanforderungen](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) jede Woche im Evaluierungszeitraum des Vorhabens.

![Kantengraph für Wiedergabeanforderungen](assets/number-play-requests.png){width="550" align="left"}

*Kantengraph für Wiedergabeanforderungen*

### Auswirkung auf Vorgänge: Allgemeine Nutzungsmetriken {#impact-general-usage}

Die allgemeinen Nutzungsmetriken geben einen Überblick über die durchschnittliche Anzahl von Geräten, IPs und Standorten im Segment des Vorgangs während der [Bewertungszeitraum](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Anzahl der Geräte {#devices}

Dieses Liniendiagramm stellt den Durchschnittswert dar [Anzahl der Geräte](/help/accountiq/general-usage-reports.md#devices-week-account) jede Woche im Evaluierungszeitraum des Vorhabens.

![Gerätezeilendiagramm](assets/number-devices.png){width="550" align="left"}

*Gerätezeilendiagramm*

#### Anzahl der IPs und Standorte {#IPs-locations}

Dieses Liniendiagramm stellt den Durchschnittswert dar [Anzahl der IPs](/help/accountiq/general-usage-reports.md#ip-week-account) und [locations](/help/accountiq/general-usage-reports.md#locations-week-account) jede Woche im Evaluierungszeitraum des Vorhabens.

![Liniendiagramm für IPs und Standorte](assets/number-ips-locations.png){width="550" align="left"}

*Liniendiagramm für IPs und Standorte*

So schließen Sie den Bericht und kehren zur Hauptansicht zurück **Aktivitäten** Seite, auswählen **Aktivitäten** Registerkarte unter **Aktionen** im linken Bereich.

## Neuen Vorgang erstellen {#create-new-operation}

Wenn Sie zur **Aktivitäten** Registerkarte unter **Aktionen** Wählen Sie im linken Bereich die Option **Neuen Vorgang erstellen** oben im **Aktivitäten** Seite.

Um einen neuen Vorgang zu erstellen, befolgen Sie die Anweisungen in den folgenden Abschnitten:

* [Vorgangsdetails](#operation-details)
* [Segment](#segment)
* [Aktion](#action)
* [Zeitplan](#schedule)

### Vorgangsdetails {#operation-details}

Geben Sie in diesem Abschnitt den Namen für den Vorgang in **Vorgangsname**.

>[!TIP]
>
>Beschreiben Sie den Zweck des Vorgangs oder die Art der Aktion in **Vorgangsname** zur schnellen Identifizierung. Die Option **Beschreibung und Tags hinzufügen** wird in zukünftigen Versionen verfügbar sein.

![Hinzufügen des Vorgangsnamens in Vorgangsdetails](assets/operation-details.png)

*Vorgangsname hinzufügen*

### Segment {#segment}

Klicken Sie in diesem Abschnitt auf **Segment auswählen** und wählen Sie ein Segment aus, für das Sie diesen Vorgang verwenden möchten. Lernen [Auswählen eines Segments](/help/accountiq/segments-timeinterval.md#segment-selection).

Verwenden Sie nach Auswahl eines Segments <img alt= "Segmentzusammenfassung erweitern" src="./assets/expand-segment-summary.svg" width="25"> -Symbol, um die detaillierte Segmentzusammenfassung anzuzeigen. Mehr dazu [Segmentzusammenfassung](segments-timeinterval.md#segment-summary).

![Segment und Zeitintervall auswählen](assets/select-segment-timeinterval.png)

*Segment und Zeitintervall auswählen*

>[!NOTE]
>
>Die [Videokategorien](product-concepts.md#video-category-def) im vorherigen Bild angezeigt werden, z. B. **MVPDs**, **Programmierer**, und **Kanäle** stellen die Beschriftungen dar, die in der TV Anywhere-Version von Account IQ verwendet werden. Wenn Sie als D2C-Dienst angemeldet sind, zeigen diese Beschriftungen die spezifischen Videokategorien Ihres Unternehmens an.

Verwenden Sie bei Bedarf <img alt= "Segment bearbeiten" src="./assets/edit-segment.svg" width="25"> Symbol zum Bearbeiten des ausgewählten Segments oder  <img alt= "Neues Segment erstellen" src="./assets/create-new-segment.svg" width="25"> zum Erstellen eines neuen Segments. Weitere Informationen finden Sie in den Anweisungen unter [Erstellen eines neuen Segments](work-with-segments.md#create-new-segment) oder [Bearbeiten eines Segments](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**Segmenttyp** benannt **[!UICONTROL Fixed number of accounts]** ist derzeit standardmäßig ausgewählt. Die ausgewählte Option **[!UICONTROL Variable number of accounts]** wird in den kommenden Versionen verfügbar sein.

Auswählen **Granularität und Zeitintervall** zur Überwachung des Vorgangs während eines bestimmten Zeitraums. Weitere Informationen [Auswahl der Granularität und des Zeitintervalls](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Aktion {#action}

Wählen Sie in diesem Abschnitt eine **Aktion** die Sie im Dropdown-Menü für das ausgewählte Segment ausführen möchten.

![Auswählen des Aktionstyps](assets/apply-actions.png)

*Auswählen des Aktionstyps*

Es stehen zwei Optionen zur Verfügung:

* Auswählen **CM-Richtlinie** für das gleichzeitige Überwachungssystem, das mit Konto IQ integriert ist.

* Auswählen **Externe Aktionen** , um Workflows zu erstellen und zu verarbeiten, die außerhalb von Account IQ und nicht mit Account IQ integriert sind.

>[!NOTE]
>
>Externe Aktionen beziehen sich möglicherweise nicht immer direkt auf die Freigabe von Passwörtern, können sich aber dennoch auf sie auswirken, wie z. B. die Einführung einer neuen Saison.

### Zeitplan {#schedule}

Wählen Sie in diesem Abschnitt die **Startdatum** und **Enddatum** aus der Datumsauswahl, um die Aktivierung für den Vorgang festzulegen.

>[!IMPORTANT]
>
>Derzeit ist die Standardaktivierung **Startdatum** und **Enddatum** auf **Am Datum**. Die ausgewählte Option **Wenn eine Bedingung erfüllt ist** und **Manuell** wird in den kommenden Versionen verfügbar sein.

>[!NOTE]
>
>Stellen Sie sicher, dass sowohl das Start- als auch das Enddatum mit der Granularität übereinstimmen, die in der **Schritt 4**.

* Wenn Sie sich für eine Granularität entschieden haben, die nach Wochen aggregiert wird, wählen Sie das Start- und Enddatum in Wochen aus (z. B. Woche 10).
* Wenn Sie sich für eine nach Monaten aggregierte Granularität entschieden haben, wählen Sie das Start- und Enddatum in Monaten aus.

![Wählen Sie in der Datumsauswahl das Start- und Enddatum aus.](assets/add-schedule.png)

*Wählen Sie in der Datumsauswahl Start- und Enddatum aus.*

**A.** Datumsauswahl starten **B.** Enddatumsauswahl

>[!NOTE]
>
>Die **Startdatum** darf weder den Bewertungszeitraum noch das aktuelle Datum überschreiten, während die Variable **Enddatum** muss nach dem Startdatum und dem aktuellen Datum liegen, um Vorgänge im zukünftigen Zeitraum zu planen und auszuführen.

Auswählen **Vorgang &quot;Save&quot;** oben im **Aktivitäten** Seite, um einen neuen Vorgang zu verarbeiten.

## Stopp-Vorgang {#stop-operation}

Sie können nur die Vorgänge stoppen, die sich derzeit in **Läuft** -Status. Gehen Sie wie folgt vor, um einen vorhandenen Vorgang zu stoppen:

1. Navigieren Sie zum **Aktivitäten** Registerkarte unter **Aktionen** in der linken Navigation der Konto-IQ-Anwendung.
1. Auswählen **Optionen** Menü des Vorgangs, den Sie stoppen möchten.

   ![Menü &quot;Optionen&quot;auswählen, um den Vorgang anzuhalten](assets/stop-operation.png)

   *Menü &quot;Optionen&quot;auswählen, um den Vorgang zu beenden*

1. Auswählen **Anhalten**.



