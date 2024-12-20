---
title: Exportieren von Informationen für Konten mit hoher Freigabebewertung
description: Exportieren Sie Informationen für Konten mit hoher Freigabebewertung.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# Exportieren von Informationen für Konten mit hoher Freigabebewertung {#export-account-info-high-score}

Mit [!UICONTROL Account IQ] können Sie Details zur Kontofreigabe für die 1000 wichtigsten Abonnentenkonten auf Grundlage ihrer [Freigabewahrscheinlichkeiten“ ](/help/accountiq/product-concepts.md#account-sharing-probability-def). Sie können die Kontofreigabeinformationen für das aktuelle [Segment](/help/accountiq/product-concepts.md#segment-def) und [angegebenes Zeitintervall](/help/accountiq/product-concepts.md#time-interval-def) auf der Seite [Berichte über freigegebene Konten](/help/accountiq/shared-acc-reports.md) exportieren.

Führen Sie die Schritte aus, um die Kontofreigabeinformationen von Abonnentenkonten für ein bestimmtes Segment zu exportieren.

1. Melden Sie sich mit Ihren -Anmeldeinformationen an.
1. Navigieren Sie zur Registerkarte **Freigegebene Konten** unter dem Abschnitt **Berichte** .
1. Wählen Sie das gewünschte Segment und das gewünschte Zeitintervall im Bedienfeld Segment und Zeitintervall aus. Erfahren Sie [wie Sie ein Segment und ein Zeitintervall auswählen](segments-timeinterval.md).

   Weitere Informationen finden Sie in den Anweisungen zum [Erstellen eines Segments](work-with-segments.md#create-new-segment) oder [Bearbeiten eines Segments](work-with-segments.md#edit-segment).

1. Wählen Sie oben rechts im Bedienfeld Segment und Zeitintervall **[!UICONTROL Export top 1000 accounts]** aus.

   ![Die 1000 wichtigsten Konten exportieren](assets/export-top-1000-accounts.png)

   *Die Option „Die 1.000 wichtigsten Konten exportieren“*

Die Datei wird automatisch als CSV-Datei auf Ihren lokalen Computer heruntergeladen.

Diese Datei enthält die Daten für die 1000 wichtigsten Konten basierend auf der Freigabewahrscheinlichkeit der Abonnentenkonten im aktuellen Segment in absteigender Reihenfolge.

Im Folgenden finden Sie ein Beispiel für die exportierte CSV-Datei.

![Exportierte Daten in .csv-Datei](assets/exported-csv.png)

*Exportierte Daten in .csv-Datei*

## Spalten im exportierten Bericht {#columns-in-export}

**Woche/Monat**

Die ausgewählte Woche oder der ausgewählte Monat innerhalb der **[!UICONTROL Granularity and Time Interval]** in der Segmentauswahl.

**MVPD**

Wenn Sie ein Programmierer sind, zeigt die Spalte den Distributor an, bei dem das Konto abonniert wurde.

>[!NOTE]
>
> Die Spalte **MVPD** ist nur für die Versionen „TV Everywhere“ verfügbar.

**Abonnenten-ID**

Die eindeutige Kennung für das spezifische Konto.

**Mindestanzahl an Geräten**

Die Mindestanzahl der Geräte, von denen Benutzerinnen und Benutzer Inhalte aktiv streamen.

>[!NOTE]
>
>Die tatsächliche Anzahl von Geräten, die Inhalte streamen, ist größer als die für ein bestimmtes Konto angegebene Mindestanzahl von Geräten.

**Mindestanzahl an Personen**

Die Mindestanzahl der Personen, die mit diesen Geräten aktiv Inhalte gestreamt haben.

>[!NOTE]
>
>Die tatsächliche Anzahl von Personen, die Inhalte streamen, ist größer als die Mindestanzahl von Personen, die einem bestimmten Konto zugewiesen sind.

**[!UICONTROL # IPs]**

Die Anzahl der IP-Adressen, von denen Inhalte gestreamt werden.

**[!UICONTROL # Locations]**

Die Anzahl der Orte (basierend auf der Postleitzahl), von denen Inhalte gestreamt werden.

**[!UICONTROL # Cities]**

Die Anzahl der Städte, in denen die Streaming-Aktivität stattgefunden hat.

**[!UICONTROL # States]**

Die Anzahl der Status, in denen die Streaming-Aktivität stattgefunden hat.

**[!UICONTROL # Clusters]**

Die Anzahl der einzelnen [Cluster](/help/accountiq/product-concepts.md#cluster-def) in denen das Streaming stattgefunden hat.

**[!UICONTROL Geographic span (miles)]**

Die maximale Entfernung zwischen den Streaming-Speicherorten, die mit dem Konto verknüpft sind.

**[!UICONTROL # AuthN OK]**

Die Anzahl der Anmeldungen, die Benutzende während des angegebenen Zeitraums mit diesem Konto vornehmen.

>[!NOTE]
>
> Einige D2C-Services sehen möglicherweise keine **[!UICONTROL # AuthN OK]**, da diese möglicherweise nicht in den Unternehmensdaten enthalten sind.

**[!UICONTROL # AuthZ OK]**

Die Häufigkeit, mit der eine MVPD einen Stream autorisiert oder Zugriff auf Inhalte für dieses Konto gewährt hat.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** ist nicht für D2C-Dienste verfügbar.

>[!NOTE]
>
>Bei TV Everywhere wird **[!UICONTROL # AuthZ OK]** mit der Anzahl der **[# Play-Anfragen korreliert](/help/accountiq/product-concepts.md##play-requests-def)**. Es ist immer weniger als **[!UICONTROL # Play Requests]**, da Adobe die Berechtigungen von MVPDs in der Regel etwa 24 Stunden lang zwischenspeichert.


**[!UICONTROL # Play Requests]**

Die tatsächliche Anzahl von Streams, die während eines bestimmten Zeitraums aufgetreten sind.

>[!NOTE]
>
>Die Spalte [# Wiedergabeanforderungen](/help/accountiq/product-concepts.md##play-requests-def) ist in der MVPD-Version von TV Everywhere nicht verfügbar.

**[!UICONTROL # Channels]**

Die Gesamtzahl der Kanäle, die das Konto über einen bestimmten Zeitraum beobachtet hat.

>[!NOTE]
>
> Bei D2C-Diensten entspricht **[!UICONTROL # Channels]** der Anzahl der **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>Bei TV Everywhere enthalten sie die Kanäle, die möglicherweise nicht zum angemeldeten Programmierer gehören. Diese Nummer für das Konto umfasst Ihren Kanal und andere Kanäle, auf die während des angegebenen Zeitraums zugegriffen wurde.


**Nutzungsmuster**

Die Werte innerhalb dieser Spalten dienen als Kennungen, die einem der 14 Muster entsprechen, die wir zur Kategorisierung aller Benutzerkonten verwenden.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">Verwendungsmuster</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Regulärer Benutzer</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Reisender oder Pendler</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Großfamilie</td>
      </tr>
      <tr>
        <td>4</td>
        <td>Enge Familie und Freunde</td>
      </tr>
      </tr>
         <td>5 und 8</td>
         <td>Teilen durch soziale Gruppen</td>
      </tr>
      </tr>
         <td>6</td>
         <td>Große Gruppe von Freunden</td>
      </tr>
      </tr>
         <td>7</td>
         <td>Gleichzeitiges Streaming</td>
      </tr>
      </tr>
         <td>9</td>
         <td>Community-Freigabe</td>
      </tr>
      </tr>
         <td>10 und 11</td>
         <td>Unsicheres Verhalten</td>
      </tr>
      </tr>
         <td>12</td>
         <td>Kleine Familie</td>
      </tr>
      </tr>
         <td>13</td>
         <td>Zweitwohnung </td>
      </tr>
      </tr>
         <td>14</td>
         <td>Anomale Nutzung</td>
      </tr>
    </tbody>
  </table>

*Verwendungsmusterkennungen in exportierten CSV-Zuordnungen mit Verwendungsmustern*

**Teilungswahrscheinlichkeit**

Die Wahrscheinlichkeit, dass ein bestimmtes Konto seine Anmeldeinformationen teilt.

>[!NOTE]
>
> Der Durchschnitt der Freigabewahrscheinlichkeit aller Konten im ausgewählten Segment wird verwendet, um die [Freigabeebene](/help/accountiq/data-panels.md#sharing-level) des [durchschnittlichen Freigabewerts](/help/accountiq/data-panels.md#aggregated-sharing) zu berechnen.
