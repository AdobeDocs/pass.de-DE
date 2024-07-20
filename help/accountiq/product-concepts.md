---
title: Glossar zu Account IQ
description: Ein Glossar der Produktterminologie.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# Produktkonzepte und Glossare {#glossary}

## Häufige Begriffe in D2C und TV überall

Die folgenden Produktbegriffe und ihre Definitionen gelten für alle [Versionen von Account IQ](versions-aiq.md).

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Ein Dashboard-Bedienfeld mit Diagrammen, in dem die Teilungswerte des aktuellen Segments in Kategorien für Teilungsbereiche (&quot;Sehr niedrig&quot;, &quot;Niedrig&quot;, &quot;Moderat&quot;, &quot;Hoch&quot;und &quot;Sehr hoch&quot;) unterteilt werden.

### [!UICONTROL Action] {#action-def}

Ein direktes oder indirektes Ereignis, das mit einem [Vorgang](#operation-def) verknüpft ist, der sich auf die Eigenschaften (z. B. Teilungsbewertung oder Anzahl der verwendeten Geräte) eines zugehörigen Vorgangssegments auswirkt.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Ein Dashboard-Bedienfeld mit Diagrammen, die die Teilungsbewertungen des aktuellen Segments in Kategorien für den Freigabebereich von Sehr niedrig, Niedrig, Moderat, Hoch und Sehr Hoch unterteilen, zusammen mit jedem Kategorieprozentsatz der gesamten Streaming-Menge für das Segment.

### [!UICONTROL AuthN] {#authn-def}

Die Anzahl der Authentifizierungsversuche. Ein Authentifizierungsversuch ist der Prozess, bei dem ein Benutzer versucht, sich mit dem D2C-Service oder MVPD anzumelden. Für TV Anywhere-Benutzer wird der Benutzer zu seinem gewählten MVPD weitergeleitet, wo er sich zum MVPD identifiziert - normalerweise mit einem Benutzernamen und einem Passwort.

### [!UICONTROL AuthN OK] {#authn-ok-def}

Die Anzahl erfolgreicher Authentifizierungen. Eine erfolgreiche Authentifizierung erfolgt, wenn die Identifizierung eines Benutzers durch einen D2C-Dienst oder MVPD bestätigt wird. Für TV-Nutzer überall führt dies dazu, dass der Benutzer zurück zur Programmierer-App oder -Site geleitet wird.

### [!UICONTROL Cluster] {#cluster-def}

Ein Cluster ist eine Sammlung von Standorten und Geräten. Die Cluster werden erstellt, indem zwischen Geräten gemeinsame Speicherorte gefunden werden. Die Geräte, die sich an einem gemeinsamen Standort befinden, werden als Teil desselben Clusters betrachtet. Zwei Geräte können sich im selben Cluster befinden, auch wenn sie keine gemeinsamen Standorte haben, aber über die Standorte anderer Geräte verbunden werden können.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Ein Cluster ohne statische Geräte.

#### [!UICONTROL Static cluster] {#static-cluster-def}

Ein Cluster mit mindestens einem statischen Gerät.

### [!UICONTROL Concurrency] {#consurrency-def}

Die gleichzeitige Wiedergabe wird durch zwei (oder mehr) gleichzeitig oder sehr nahe gespielte Streams definiert, sodass das Intervall zwischen ihnen nicht durch eine normale Geschwindigkeit gerechtfertigt werden kann.
Die gleichzeitige Nutzung wird anhand der maximalen Geschwindigkeit (Meilen/Stunde) zwischen 2 verschiedenen Clustern berechnet. Ein Benutzer gilt als Benutzer mit gleichzeitiger Nutzung, wenn er eine Geschwindigkeit von mehr als 124 m/h über eine Entfernung von weniger als 124 Meilen hat oder wenn er eine Geschwindigkeit von mehr als 400 m/h über eine Entfernung von mehr als 124 Meilen hat. Der Abstand wird zwischen Orten aus verschiedenen Clustern berechnet. Die gleichzeitige Nutzung ist im selben Cluster zulässig.

### [!UICONTROL Device] {#device-def}

Ein Hardware-Produkt für digitale Videos, mit dem Streaming-Inhalte wiedergegeben werden können. Zum Beispiel Smartphones, Laptops und Desktop-Computer, Spielekonsolen und intelligente Fernseher.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

Der Auswertungszeitraum ist die Zeit vom Beginn der Aktion, die mit dem Vorgang verknüpft ist, bis zum Ende der Aktion oder der Messung.

### [!UICONTROL Geographical Span] {#geographical-span-def}

Der Abstand zwischen den am weitesten gelegenen Punkten einer Reihe von Orten.

### [!UICONTROL Granularity] {#granularity-def}

In Bezug auf das Zeitintervall die Größe des Zeitraums, z. B. eine Woche oder einen Monat.

### [!UICONTROL IP] {#ip-def}

Die Internet-Protokolladresse, die einem Gerät von einem Internet Service Provider zugewiesen wurde. Beispiel: Kabeldienstanbieter und Zellendienstanbieter.

### [!UICONTROL Location] {#location-def}

Ein einzigartiger Punkt auf der Erde. Es wird auch als Geolocation für eine bestimmte Wiedergabe-Anfrage mit einer Genauigkeit von 1000m x 1000m (ein Quadratkilometer) bezeichnet.

### [!UICONTROL Media Company] {#media-company-def}

Media Company ist ein Unternehmen, das Eigentümer einer Gruppe von Mediennetzwerken ist.

### [!UICONTROL Metric] {#metric}

Metrik ist ein Attribut des Abonnentenkontos (z. B. sein MVPD, die Programmierer und Kanäle des Inhalts, den sie streamen, und die Anzahl der von ihnen verwendeten Geräte).

### [!UICONTROL Mobile device] {#mobile-device-def}

Ein Gerät mit hoher Mobilität. Zum Beispiel Mobiltelefon und Tablet.

### Vorgang {#operation-def}

Der Vorgang ist ein Datensatz, der erstellt wurde, um die Auswirkungen einer bestimmten [Aktion](#action-def) auf ein verknüpftes Segment zu verfolgen. Ein Beispiel für eine Aktion kann eine Begrenzung der Anzahl gleichzeitiger Streams sein, die für die vom Segment identifizierten Konten zulässig sind.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

Ein Wert, der Benutzern dabei hilft, das Ausmaß der Kennwortfreigabe zu verstehen, und ihnen ein Gefühl der Dringlichkeit vermittelt, darauf zu reagieren.

### [!UICONTROL Play Request] {#play-requests-def}

Entspricht einem Stream-Start. Dieses Ereignis markiert den Anfang eines Benutzer-Streaming-Inhalts.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

Dieser Wert, auch Verwendung von freigegebenen Konten genannt, wird auf der Grundlage der Anzahl der Wiedergabeanforderungen berechnet, die von dem jeweiligen Konto erstellt werden, gewichtet mit der Freigabewahrscheinlichkeit jedes Kontos. Sie wird auch als &quot;Nutzung durch den Risikoindex für freigegebene Konten&quot;bezeichnet.

### [!UICONTROL Segment] {#segmet-def}

Segment ist ein Satz von Konten, die die von den ausgewählten Metriken angegebenen benutzerdefinierten Bedingungen erfüllen. Beispiel: &quot;Benutzer in den Regionen A, B, C, D oder E, die mehr als drei Geräte haben&quot;.

### [!UICONTROL Sharing level] {#sharing-level-def}

Dieser Wert wird auch als Risikoindex - Accounts (Risikoindex) oder Shared Accounts Risk Index (Risikoindex für geteilte Konten) bezeichnet und ist ein Wert, der auf der Grundlage der durchschnittlichen Freigabewahrscheinlichkeit berechnet wird, die für jedes Konto im aktuellen Segment berechnet wird, das im ausgewählten Zeitintervall mindestens einmal gestreamt hat.

### [!UICONTROL Static device] {#static-device-def}

Ein Gerät mit sehr geringer Mobilität. Beispielsweise Spielkonsole, Set-Top-Box und Fernseher.

### [!UICONTROL Time interval] {#time-interval-def}

Dieser Zeitraum wird auch als Zeitraum bezeichnet. Er enthält die Wiedergabe-Anfragenaktivität, die in der Benutzeroberfläche dargestellt wird, sowie Tabellen von Anfang bis Ende.

### [!UICONTROL Trend] {#trend-def}

Die prozentuale Differenz in der zugehörigen Metrik (z. B. Prozentsatz der gesamten Wiedergabeanforderungen) zwischen dem aktuellen und vorherigen Zeitraum.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Die Anzahl der eindeutigen Konten, die in einem bestimmten Zeitraum mindestens einmal gestreamt haben.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

Mehr als 37 Wiedergabeanforderungen pro Monat.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

Weniger als 9 Wiedergabeanforderungen pro Monat.

#### [!UICONTROL Regular user] {#regular-user-def}

Von 9 bis 37 Wiedergabeanforderungen pro Monat.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

Eine endliche Reihe von Kategorietiketten, die auf ein Konto angewendet werden, das die Benutzer des Kontos am besten in Bezug auf soziale Gruppen oder Verhaltensweisen (z. B. eine kleine Familie, einen Reisenden oder Pendler, soziale Freigabe usw.) charakterisiert.

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

Eine Videokategorie ist eine spezifische Bezeichnung, die die Art des Videos qualifiziert. Zum Beispiel Region der Quelle, Inhaltstypen wie VOD oder Live, Ereignis oder spezifische Beschriftungen wie Team.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

Eine Videokategorie wird durch die Kombination aus Programmierer, Kanal und MVPD definiert, die mit dem Stream verknüpft ist.

### [!UICONTROL Zip Code] {#zip-code-def}

Die US-Postleitzahl, die Orten in den USA zugeordnet ist.
<!--calculated metrics-->


## Überall spezifische Begriffe fernsehen

### [!UICONTROL AuthZ] {#authz-def}

Autorisierung oder die Anzahl der Genehmigungsanträge. Eine Autorisierungsanfrage ist der Prozess, bei dem ein Programmierer von einem MVPD über Adobe die Erlaubnis anfordert, mit dem Streaming des angeforderten Inhalts eines Benutzers zu beginnen. Der MVPD gewährt die Anfrage in der Regel basierend auf den Inhaltsrechten, die mit dem MVPD-Abonnement des Benutzers verknüpft sind (z. B. ob sich der mit dem Inhalt verknüpfte Kanal im Abonnement des Benutzers befindet). Einige Autorisierungsanfrageantworten werden von Adobe zwischengespeichert, was es Adobe ermöglicht, sofort zu antworten, ohne die Anfrage an den MVPD weiterzuleiten.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

Die Anzahl der erfolgreichen Berechtigungen.

### [!UICONTROL Channel] {#channel-def}

Der Kanal, auch als Eigenschaft bezeichnet, ist eine themenverwandte Quelle für Videoinhalte. Traditionell ein bestimmter, numerisch adressierbarer kontinuierlicher Video-Feed aus einem MVPD darstellt. Der Kanal wird direkt einem zugänglichen Inhaltskanal zugeordnet, der den Abonnenten über ihre Set Top Box (STB) zur Verfügung steht.

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Ein Wert, der für jeden der Risikoindizes (Konten, Nutzung, Allgemein) über alle Programmierer und MVPDs im ausgewählten Zeitintervall berechnet wird.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

Eine Art von Freigabeanalyse, bei der die Auswertung eines Kontos auf Ereignisse beschränkt ist, die direkt bei den Programmierern im ausgewählten Segment aufgetreten sind.  Normalerweise werden alle Kontoereignisse ausgewertet, was eine sehr viel genauere Schätzung der Freigabe bietet.  Einige MVPD-Daten sind so strukturiert, dass nur eine Isolationsmodus-Analyse möglich ist.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD, auch als Distributor bekannt, ist ein Aggregator, Wiederverkäufer und Distributor von Media Company-Videoinhalten.

### [!UICONTROL Programmer] {#programmer-def}

Programmierer, auch bekannt als Network, ist ein Unternehmen, das Tochterunternehmen eines größeren Unternehmens (Unternehmens) ist, das einen oder mehrere Kanäle besitzt und verwaltet.

### [!UICONTROL requestorID] {#requestorid-def}

Die ID, die ein Medienunternehmen verwendet, um sich oder eine Tochtergesellschaft eines MVPD zu identifizieren.  Abhängig von der Programmimplementierung kann dies einem Medienunternehmen, Programmierer oder Kanal zugeordnet werden. Traditionell wird dies einem Kanal zugeordnet.  Mit der Erstellung von Pseudo-Kanälen wie MML (März Madness Live) und technisch gesteuerten Schritten zur Behebung von MVPD-gesteuerten Datenbeschränkungen wird requestorID zunehmend mit dem Media Company verknüpft.

### [!UICONTROL resourceID] {#resource-id-def}

Der vom Endbenutzer angeforderte Inhalt. Traditionell wurde dadurch der Kanal identifiziert, der mit dem vom Benutzer angeforderten Inhalt verknüpft ist.  Systemverbesserungen ermöglichen es, dass diese ID bestimmte Programme (z. B. mit bestimmten Bewertungen) darstellt. Die ID identifiziert weiterhin den zugehörigen Kanal.


