---
title: Exportieren von Informationen zu Konten mit hoher Sharing-Punktzahl
description: Exportieren Sie Informationen für Konten mit hoher Sharing-Punktzahl.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# Exportieren von Informationen zu Konten mit hoher Sharing-Punktzahl {#export-account-info-high-score}

Mit [!UICONTROL Account IQ] können Sie Details zur Kontofreigabe für die Top-1000-Abonnentenkonten basierend auf ihren [Freigabewahrscheinlichkeiten](/help/accountiq/product-concepts.md#account-sharing-probability-def) exportieren. Sie können die Kontofreigabeinformationen für das aktuelle [Segment](/help/accountiq/product-concepts.md#segment-def) und das [angegebene Zeitintervall](/help/accountiq/product-concepts.md#time-interval-def) auf der Seite [Berichte zu freigegebenen Konten](/help/accountiq/shared-acc-reports.md) exportieren.

Führen Sie die Schritte aus, um die Kontofreigabe-Informationen von Abonnentenkonten für ein bestimmtes Segment zu exportieren.

1. Melden Sie sich mit Ihren Anmeldedaten an.
1. Navigieren Sie zur Registerkarte **Freigegebene Konten** im Abschnitt **Berichte** .
1. Wählen Sie das erforderliche Segment und das erforderliche Zeitintervall aus dem Segment- und Zeitintervallbereich aus. Erfahren Sie [, wie Sie ein Segment und ein Zeitintervall auswählen](segments-timeinterval.md).

   Lesen Sie bei Bedarf die Anweisungen zum [Erstellen eines Segments](work-with-segments.md#create-new-segment) oder [Bearbeiten eines Segments](work-with-segments.md#edit-segment).

1. Wählen Sie **[!UICONTROL Export top 1000 accounts]** in der oberen rechten Ecke des Segment- und Zeitintervallbereichs aus.

   ![Top-1000-Konten exportieren](assets/export-top-1000-accounts.png)

   *Wählen Sie die Option &quot;Top 1000-Konten exportieren&quot;*

Die Datei wird automatisch als CSV-Datei auf Ihren lokalen Computer heruntergeladen.

Diese Datei enthält die Daten für die Top-1000-Konten basierend auf den Teilungswahrscheinlichkeiten der Abonnentenkonten im aktuellen Segment in absteigender Reihenfolge.

Im Folgenden finden Sie ein Beispiel für die exportierte .csv -Datei.

![ exportierte Daten in .csv-Datei](assets/exported-csv.png)

*Exportierte Daten in .csv-Datei*

## Spalten im exportierten Bericht {#columns-in-export}

**Woche/Monat**

Die ausgewählte Woche oder der ausgewählte Monat innerhalb der **[!UICONTROL Granularity and Time Interval]** -Option in der Segmentauswahl.

**MVPD**

Wenn Sie Programmierer sind, zeigt die Spalte den Distributor an, mit dem das Konto angemeldet ist.

>[!NOTE]
>
> Die Spalte **MVPD** ist nur für TV Anywhere-Versionen verfügbar.

**Abonnenten-ID**

Die eindeutige Kennung für das spezifische Konto.

**Minimum # Devices**

Die Mindestanzahl der Geräte, von denen Benutzer Inhalte aktiv streamen.

>[!NOTE]
>
>Die tatsächliche Anzahl der Geräte-Streaming-Inhalte übersteigt die für ein bestimmtes Konto angegebene Mindestanzahl von Geräten.

**Minimum # persons**

Die Mindestanzahl von Einzelanwendern, die aktiv Inhalte mit diesen Geräten gestreamt.

>[!NOTE]
>
>Die tatsächliche Anzahl der Einzelanwender, die Streaming-Inhalte nutzen, übersteigt die Mindestanzahl der Personen, die einem bestimmten Konto zugewiesen sind.

**[!UICONTROL # IPs]**

Die Anzahl der IP-Adressen, von denen Inhalte gestreamt werden.

**[!UICONTROL # Locations]**

Die Anzahl der Speicherorte (basierend auf der Postleitzahl), von denen der Inhalt gestreamt wird.

**[!UICONTROL # Cities]**

Die Anzahl der Städte, in denen die Streaming-Aktivität stattgefunden hat.

**[!UICONTROL # States]**

Die Anzahl der Status, in denen die Streaming-Aktivität stattgefunden hat.

**[!UICONTROL # Clusters]**

Die Anzahl unterschiedlicher [Cluster](/help/accountiq/product-concepts.md#cluster-def), in denen Streaming stattgefunden hat.

**[!UICONTROL Geographic span (miles)]**

Der maximale Abstand zwischen den Streaming-Speicherorten, die mit dem Konto verknüpft sind.

**[!UICONTROL # AuthN OK]**

Die Anzahl der Anmeldungen, die Benutzer während des angegebenen Zeitraums mit diesem Konto vornehmen.

>[!NOTE]
>
> Einige D2C-Dienste sehen möglicherweise keine **[!UICONTROL # AuthN OK]** -Daten, da diese möglicherweise nicht in den Daten ihres Unternehmens enthalten sind.

**[!UICONTROL # AuthZ OK]**

Die Häufigkeit, mit der ein MVPD einen Stream autorisiert oder den Zugriff auf Inhalte für dieses Konto gewährt hat.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** ist nicht für D2C-Dienste verfügbar.

>[!NOTE]
>
>Für TV-Sendungen überall wird **[!UICONTROL # AuthZ OK]** mit der Anzahl der **[#-Abspielanforderungen](/help/accountiq/product-concepts.md##play-requests-def)** korreliert. Der Wert ist immer kleiner als **[!UICONTROL # Play Requests]**, da Adobe die Berechtigungen von MVPDs normalerweise ca. 24 Stunden lang zwischenspeichert.


**[!UICONTROL # Play Requests]**

Die tatsächliche Anzahl der Streams in einem bestimmten Zeitraum.

>[!NOTE]
>
>Die Spalte [# Play Requests](/help/accountiq/product-concepts.md##play-requests-def) ist nicht in TV Anywhere MVPD-Version verfügbar.

**[!UICONTROL # Channels]**

Die Gesamtanzahl der Kanäle, die das Konto während eines bestimmten Zeitraums gesehen hat.

>[!NOTE]
>
> Bei D2C-Diensten entspricht **[!UICONTROL # Channels]** der Nummer **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>Für TV Anywhere enthalten sie die Kanäle, die möglicherweise nicht zum angemeldeten Programmierer gehören. Diese Kontonummer enthält Ihren Kanal und andere Kanäle, auf die während des angegebenen Zeitraums zugegriffen wurde.


**Nutzungsmuster**

Die Werte in diesen Spalten dienen als Bezeichner für eines der 14 Muster, die wir zur Kategorisierung aller Benutzerkonten verwenden.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">Nutzungsmuster</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Regulärer Benutzer</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Reisende oder Pendler</td>
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
         <td>Social-Gruppenfreigabe</td>
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
         <td>Zweites Heim </td>
      </tr>
      </tr>
         <td>14</td>
         <td>Anormale Verwendung</td>
      </tr>
    </tbody>
  </table>

*Nutzungsmuster-IDs in der exportierten .csv-Zuordnung mit Nutzungsmustern*

**Freigabewahrscheinlichkeit**

Die Wahrscheinlichkeit, dass ein bestimmtes Konto seine Anmeldeinformationen teilt.

>[!NOTE]
>
> Der Durchschnitt der Freigabewahrscheinlichkeit aller Konten im ausgewählten Segment wird verwendet, um die [Teilungsstufe](/help/accountiq/data-panels.md#sharing-level) des [durchschnittlichen Teilungswerts](/help/accountiq/data-panels.md#aggregated-sharing) zu berechnen.
