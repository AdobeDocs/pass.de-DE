---
title: Glossar zu Konto-IQ
description: Ein Glossar der Produktterminologie.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1383'
ht-degree: 0%

---

# Produktkonzepte und Glossare {#glossary}

Siehe die folgenden Produktterminologie und ihre Definitionen.

## [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Ein Dashboard-Bedienfeld mit Diagrammen, in dem die aktuellen Segmente, die Bewertungen teilen, in Kategorien für den Freigabebereich unterteilt sind: Sehr niedrig, Niedrig, Moderat, Hoch und Sehr Hoch.

## [!UICONTROL Action] {#action-def}

Ein direktes oder indirektes Ereignis, das mit einem [Vorgang](#operation-def) , die sich auf die Eigenschaften (z. B. Teilungswert oder Anzahl der verwendeten Geräte) eines zugehörigen Vorgangssegments (oder Kohorte) auswirkt.

## [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Ein Dashboard-Bedienfeld mit Diagrammen, in dem die aktuellen Segmente, die Bewertungen teilen, in Kategorien für den Freigabebereich von Sehr niedrig, Niedrig, Moderat, Hoch und Sehr Hoch unterteilt werden, zusammen mit jedem Kategorieprozentsatz der gesamten Streaming-Menge für das Segment.

## [!UICONTROL AuthN] {#authn-def}

Authentifizierung oder die Anzahl der Authentifizierungsversuche. Ein Authentifizierungsversuch ist der Prozess, bei dem ein Benutzer ohne einen derzeit gültigen Authentifizierungsstatus zu seinem gewählten MVPD weitergeleitet wird, wo er sich zum MVPD identifiziert - normalerweise mit einem Benutzernamen und einem Kennwort.

## [!UICONTROL AuthN OK] {#authn-ok-def}

Die Anzahl erfolgreicher Authentifizierungen. Eine erfolgreiche Authentifizierung tritt auf, wenn eine Benutzer-Identifizierung durch einen MVPD bestätigt wird und der Benutzer zur Programmierer-App oder -Site zurückgeleitet wird.

## [!UICONTROL AuthZ] {#authz-def}

Autorisierung oder die Anzahl der Genehmigungsanträge. Eine Autorisierungsanfrage ist der Prozess, bei dem ein Programmierer von einem MVPD über Adobe die Erlaubnis anfordert, mit dem Streaming des angeforderten Inhalts eines Benutzers zu beginnen. Der MVPD gewährt die Anfrage in der Regel basierend auf den Inhaltsrechten, die mit dem MVPD-Abonnement des Benutzers verknüpft sind (z. B. ob sich der mit dem Inhalt verknüpfte Kanal im Abonnement des Benutzers befindet). Einige Autorisierungsanfrageantworten werden von Adobe zwischengespeichert, was es Adobe ermöglicht, sofort zu antworten, ohne die Anfrage an den MVPD weiterzuleiten.

## [!UICONTROL AuthZ OK] {#authz-ok-def}

Die Anzahl der erfolgreichen Berechtigungen.

## [!UICONTROL Channel] {#channel-def}

Der Kanal, auch als Eigenschaft bezeichnet, ist eine thematisch verwandte Quelle für Videoinhalte. Traditionell ein bestimmter, numerisch adressierbarer kontinuierlicher Video-Feed aus einem MVPD darstellt. Der Kanal wird direkt einem zugänglichen Inhaltskanal zugeordnet, der den Abonnenten über ihre Set Top Box (STB) zur Verfügung steht.

## [!UICONTROL Cluster] {#cluster-def}

Ein Cluster ist eine Sammlung von Standorten und Geräten. Die Cluster werden erstellt, indem zwischen Geräten gemeinsame Speicherorte gefunden werden. Die Geräte, die sich an einem gemeinsamen Standort befinden, werden als Teil desselben Clusters betrachtet. Zwei Geräte können sich im selben Cluster befinden, auch wenn sie keine gemeinsamen Standorte haben, aber über die Standorte anderer Geräte verbunden werden können.

### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Ein Cluster ohne statische Geräte.

### [!UICONTROL Static cluster] {#static-cluster-def}

Ein Cluster mit mindestens einem statischen Gerät.

## [!UICONTROL Concurrency] {#consurrency-def}

Die gleichzeitige Wiedergabe wird durch zwei (oder mehr) gleichzeitig oder sehr nahe gespielte Streams definiert, sodass das Intervall zwischen ihnen nicht durch eine normale Geschwindigkeit gerechtfertigt werden kann.
Die gleichzeitige Nutzung wird anhand der maximalen Geschwindigkeit (Meilen/Stunde) zwischen 2 verschiedenen Clustern berechnet. Ein Benutzer gilt als Benutzer mit gleichzeitiger Nutzung, wenn er eine Geschwindigkeit von mehr als 124 m/h über eine Entfernung von weniger als 124 Meilen hat oder wenn er eine Geschwindigkeit von mehr als 400 m/h über eine Entfernung von mehr als 124 Meilen hat. Der Abstand wird zwischen Orten aus verschiedenen Clustern berechnet. Die gleichzeitige Nutzung ist im selben Cluster zulässig.

## [!UICONTROL Device] {#device-def}

Ein Hardware-Produkt für digitale Videos, das überall TV-Inhalte wiedergibt und von Adobe Pass unterstützt wird. Zum Beispiel Smartphones, Laptops und Desktop-Computer, Spielekonsolen und intelligente Fernseher.

## [!UICONTROL Geographical Span] {#geographical-span-def}

Der Abstand zwischen den am weitesten gelegenen Punkten einer Reihe von Orten.

## [!UICONTROL Granularity] {#granularity-def}

In Bezug auf den Zeitraum die Größe des Zeitraums, z. B. eine Woche oder einen Monat.

## [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Ein für jeden Risikoindex (Konten, Nutzung, Allgemein) berechneter Wert für alle Programmierer und MVPDs während des ausgewählten Zeitraums.

## [!UICONTROL IP] {#ip-def}

Die Internet-Protokolladresse, die einem Gerät von einem Internet Service Provider zugewiesen wurde. Beispiel: Kabeldienstanbieter und Zellendienstanbieter.

## [!UICONTROL Isolation Mode] {#isolation-mode-def}

Eine Art von Freigabeanalyse, bei der die Auswertung eines Kontos auf Ereignisse beschränkt ist, die direkt bei den Programmierern im ausgewählten Segment aufgetreten sind.  Normalerweise werden alle Kontoereignisse ausgewertet, was eine sehr viel genauere Schätzung der Freigabe bietet.  Einige MVPD-Daten sind so strukturiert, dass nur eine Isolationsmodus-Analyse möglich ist.

## [!UICONTROL Location] {#location-def}

Ein einzigartiger Punkt auf der Erde. Sie wird auch als Geolocation für eine bestimmte Wiedergabeanforderung bezeichnet, die mithilfe der Pass-Daten erkannt wird und eine Genauigkeit von 1000mx1000m (eine Quadratkilometer) aufweist.

<!-- ### Home location {#home-location-def}

the precision is 0.01 ~ 2000mx2000m (4 square km) - at this moment the home location is detected using an ML algo, based on data provided by two mvpds. The probability is ~ 80%. We are not using the zip code for the majority of the users. Currently, this information is not used in assessing the sharing probability. -->

## [!UICONTROL Media Company] {#media-company-def}

Media Company ist ein Unternehmen, das Eigentümer einer Gruppe von Mediennetzwerken ist.

## [!UICONTROL Metric] {#metric}

Metrik ist ein Attribut des Abonnentenkontos (z. B. ihr MVPD, die Programmierer und Kanäle des Inhalts, den sie streamen, die Anzahl der von ihnen verwendeten Geräte).

## [!UICONTROL Mobile device] {#mobile-device-def}

Ein Gerät mit hoher Mobilität. Zum Beispiel Mobiltelefon und Tablet.

## [!UICONTROL MVPD] {#mvpd-def}

MVPD, auch als Distributor bekannt, ist Aggregator, Wiederverkäufer und Distributor von Media Company-Videoinhalten.

## Vorgang {#operation-def}

Vorgang ist ein Datensatz, der erstellt wurde, um die Wirkung eines bestimmten [action](#action-def) in einem zugehörigen Segment. Ein Beispiel für eine Aktion kann eine Begrenzung der Anzahl gleichzeitiger Streams sein, die für die vom Segment identifizierten Konten zulässig sind.

## [!UICONTROL Overall sharing score] {#overall-sharing-score}

Ein Wert, der Benutzern dabei hilft, das Ausmaß der Kennwortfreigabe in Programmierereigenschaften oder von MVPD-Abonnenten zu verstehen und ihnen ein Gefühl der Dringlichkeit zu geben, entsprechend zu handeln.

<!--**Aggregated Risk Index**
Also known as Risk Index and Sharing Risk Index, it is a value that helps users understand the magnitude of password sharing on Programmer properties or by MVPD subscribers and provides them a sense of urgency to act upon it.-->

<!--**Risk Index - Overall**
A value computed as an average of "Risk Index - Accounts" and the "Risk Index - Usage". Overall Sharing Risk Index-->

## [!UICONTROL Play Request] {#play-requests-def}

Eine Anfrage von einer Client-App oder -Site an Adobe, um ein Medien-Token zur Aufzeichnung und Sicherung eines Stream-Starts anzufordern.

## [!UICONTROL Programmer] {#programmer-def}

Programmierer, auch bekannt als Network, ist ein Unternehmen, das Tochterunternehmen eines größeren Unternehmens (Unternehmens) ist, das einen oder mehrere Kanäle besitzt und verwaltet.

## [!UICONTROL requestorID] {#requestorid-def}

Die ID, die ein Medienunternehmen verwendet, um sich oder eine Tochtergesellschaft eines MVPD zu identifizieren.  Abhängig von der Programmimplementierung kann dies einem Medienunternehmen, Programmierer oder Kanal zugeordnet werden.  Die am häufigsten verwendete ID Die ID, die ein Medienunternehmen verwendet, um sich selbst oder eine Tochtergesellschaft eines MVPD zu identifizieren.  Abhängig von der Programmimplementierung kann dies einem Medienunternehmen, Programmierer oder Kanal zugeordnet werden.  Dies ist traditionell einem Kanal zugeordnet.  Mit der Erstellung von Pseudo-Kanälen wie MML (März Madness Live) und technisch gesteuerten Schritten zur Behebung von MVPD-gesteuerten Datenbeschränkungen wird requestorID zunehmend mit dem Media Company verknüpft.

## [!UICONTROL resourceID] {#resource-id-def}

Der vom Endbenutzer angeforderte Inhalt.  Traditionell wurde dadurch der Kanal identifiziert, der mit dem vom Benutzer angeforderten Inhalt verknüpft ist.  Systemverbesserungen ermöglichen es, dass diese ID bestimmte Programme (z. B. mit bestimmten Bewertungen) darstellt. Die ID identifiziert weiterhin den zugehörigen Kanal.

## [!UICONTROL Risk Index - Usage] {#risk-index-usage}

Dieser Wert, auch Verwendung von freigegebenen Konten genannt, wird auf der Grundlage der Anzahl der Wiedergabeanforderungen berechnet, die von dem jeweiligen Konto erstellt werden, gewichtet mit der Freigabewahrscheinlichkeit jedes Kontos. Sie wird auch als &quot;Nutzung durch den Risikoindex für freigegebene Konten&quot;bezeichnet.

## [!UICONTROL Segment] {#segmet-def}

Segment ist ein Satz von Konten, die die von den ausgewählten Metriken definierten benutzerspezifischen Bedingungen erfüllen (z. B. &quot;Benutzer von MVPD A, B, C, D oder E, die die Kanäle X, Y oder Z angesehen haben&quot;).

## [!UICONTROL Sharing level] {#sharing-level-def}

Dieser Wert wird auch als Risikoindex - Accounts or Shared Accounts Risk Index (Risikoindex für Konten oder gemeinsame Konten) bezeichnet und basiert auf einem Durchschnitt der Freigabewahrscheinlichkeit, die für jedes Konto in der Gruppe ausgewählter MVPDs berechnet wird, das während des ausgewählten Zeitraums von einem der ausgewählten Programmierkanäle gestreamt wurde.

## [!UICONTROL Static device] {#static-device-def}

Ein Gerät mit sehr geringer Mobilität. Beispielsweise Spielkonsole, Set-Top-Box und Fernseher.

## [!UICONTROL Time frame] {#time-frame-def}

Das Zeitfenster, das auch als Zeitraum oder Zeitfenster bezeichnet wird, enthält die Wiedergabe-Anforderungsaktivität, die in der Benutzeroberfläche dargestellt wird, und Tabellen von Anfang bis Ende.

## [!UICONTROL Top MVPDs in segment] {#top-mvpds-def}

Die obersten (höchstens 10) MVPDs im ausgewählten Segment als Maß für die Teilungsebene, Nutzung aus freigegebenen Konten oder Gesamtwert für die Freigabe.

## [!UICONTROL Trend] {#trend-def}

Die prozentuale Differenz in der zugehörigen Metrik (z. B. Prozentsatz der gesamten Wiedergabeanforderungen) zwischen dem aktuellen und vorherigen Zeitraum.

## [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Die Anzahl der eindeutigen MVPD-Konten für einen bestimmten Zeitraum, die in einem bestimmten Zeitraum mit Programmierer TV-Programmen oder -Sites, an denen Adobe Pass beteiligt war, interagiert haben.  Diese Interaktion umfasst alle Aktivitäten in der Programmieranwendung oder Site, die zu einem Aufruf an einen Adobe Pass-Dienst führen. Überprüfen Sie beispielsweise den Status authN oder authZ, authentifizieren und autorisieren. Die Gesamtanzahl der Unique Abonnenten umfasst auch die Anzahl der individuellen Geräte, wenn die Nutzung des Adobe TempPass durch Programmierer (d. h. die kostenlose Vorschau) Teil des Segments ist.

## [!UICONTROL Usage] {#usage-defs}

### [!UICONTROL Avid user] {#avid-user-def}

Mehr als 37 Wiedergabeanforderungen pro Monat.

### [!UICONTROL Infrequent user] {#infrequent-users-def}

Weniger als 9 Wiedergabeanforderungen pro Monat.

### [!UICONTROL Regular user] {#regular-user-def}

Von 9 bis 37 Wiedergabeanforderungen pro Monat.

## [!UICONTROL Usage Pattern] {#usage-patern-def}

Eine endliche Reihe von Kategorietiketten, die auf ein Konto angewendet werden, das die Benutzer des Kontos am besten in Bezug auf soziale Gruppen oder Verhaltensweisen (z. B. eine kleine Familie, einen Reisenden oder Pendler, soziale Freigabe usw.) charakterisiert.

## [!UICONTROL Zip Code] {#zip-code-def}

Die US-Postleitzahl, die Orten in den USA zugeordnet ist.
