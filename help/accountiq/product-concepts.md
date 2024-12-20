---
title: Account IQ-Glossar
description: Ein Glossar der Produktterminologie.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# Produktkonzepte und Glossar {#glossary}

## Gemeinsame Terminologie für D2C und TV Everywhere

Die folgenden Produktterminologien und ihre Definitionen sind in allen [Versionen von Account IQ](versions-aiq.md) enthalten.

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Ein Dashboard mit Diagrammen, die die Freigabewerte des aktuellen Segments in die Freigabebereichskategorien Sehr niedrig, Niedrig, Mittel, Hoch und Sehr Hoch unterteilen.

### [!UICONTROL Action] {#action-def}

Ein direktes oder indirektes Ereignis, das mit einem [Vorgang](#operation-def) verbunden ist und sich auf die Eigenschaften (z. B. die Freigabe eines Scores oder die Anzahl der verwendeten Geräte) eines zugehörigen Vorgangssegments auswirkt.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Ein Dashboard-Bedienfeld mit Diagrammen, das die Freigabewerte des aktuellen Segments in die Freigabebereichskategorien Sehr niedrig, Niedrig, Mittel, Hoch und Sehr Hoch unterteilt, zusammen mit jedem Kategorieprozentsatz des gesamten Streaming-Umfangs für das Segment.

### [!UICONTROL AuthN] {#authn-def}

Die Anzahl der Authentifizierungsversuche. Ein Authentifizierungsversuch ist der Prozess, bei dem ein Benutzer versucht, sich mit dem D2C-Service oder MVPD anzumelden. Bei Benutzern von TV Everywhere werden die Benutzenden zu ihrer ausgewählten MVPD weitergeleitet, wo sie sich bei der MVPD identifizieren - in der Regel mit einem Benutzernamen und einem Kennwort.

### [!UICONTROL AuthN OK] {#authn-ok-def}

Die Anzahl erfolgreicher Authentifizierungen. Eine erfolgreiche Authentifizierung erfolgt, wenn die Identität einer Benutzerin oder eines Benutzers durch einen D2C-Service oder MVPD bestätigt wird. Für TV Everywhere-Benutzer führt dies dazu, dass der Benutzer zurück zur Programmieranwendung oder Site weitergeleitet wird.

### [!UICONTROL Cluster] {#cluster-def}

Ein Cluster ist eine Sammlung von Standorten und Geräten. Die Cluster werden durch die Suche nach gemeinsamen Standorten zwischen Geräten erstellt. Die an einem gemeinsamen Speicherort angezeigten Geräte gehören zum selben Cluster. Zwei Geräte können sich im selben Cluster befinden, obwohl sie keine gemeinsamen Standorte haben. Sie können jedoch über die Standorte anderer Geräte verbunden werden.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Ein Cluster ohne statische Geräte.

#### [!UICONTROL Static cluster] {#static-cluster-def}

Ein Cluster mit mindestens einem statischen Gerät.

### [!UICONTROL Concurrency] {#consurrency-def}

Der gleichzeitige Stream wird durch zwei (oder mehr) Streams definiert, die gleichzeitig oder sehr zeitnah wiedergegeben werden, sodass das Intervall zwischen ihnen nicht durch eine Fahrt mit normaler Geschwindigkeit gerechtfertigt werden kann.
Die gleichzeitige Nutzung wird anhand der maximalen Geschwindigkeit (Meilen/Stunde) zwischen zwei verschiedenen Clustern berechnet. Eine gleichzeitige Nutzung gilt als gegeben, wenn der Benutzer eine Geschwindigkeit von mehr als 124 m/h auf einer Strecke von weniger als 124 Meilen oder eine Geschwindigkeit von mehr als 400 m/h auf einer Strecke von mehr als 124 Meilen hat. Der Abstand wird zwischen Standorten aus verschiedenen Clustern berechnet. Die gleichzeitige Verwendung ist im selben Cluster zulässig.

### [!UICONTROL Device] {#device-def}

Ein digitales Video-Hardware-Produkt, das in der Lage ist, Upstream-Inhalte wiederzugeben. Beispielsweise Smartphones, Laptops und Desktop-Computer, Spielekonsolen und Smart-TVs.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

Der Auswertungszeitraum ist der Zeitraum vom Beginn der Aktion, die mit dem Vorgang verknüpft ist, bis zum Ende der Aktion oder ihrer Messung.

### [!UICONTROL Geographical Span] {#geographical-span-def}

Der Abstand zwischen den am weitesten entfernten Punkten in einer Reihe von Positionen.

### [!UICONTROL Granularity] {#granularity-def}

In Bezug auf das Zeitintervall, die Größe des Zeitraums; z. B. eine Woche oder ein Monat.

### [!UICONTROL IP] {#ip-def}

Die Internetprotokolladresse, die einem Gerät von einem Internetdienstanbieter zugewiesen wurde. Beispielsweise den Kabeldienstleister und den Zellendienstleister.

### [!UICONTROL Location] {#location-def}

Ein einzigartiger Punkt auf der Erde. Es wird auch als Geolokalisierung für eine spezielle Spielanforderung mit einer Genauigkeit von 1000m x 1000m (ein Quadratkilometer) bezeichnet.

### [!UICONTROL Media Company] {#media-company-def}

Media Company ist ein Unternehmen, das eine Gruppe von Mediennetzwerken besitzt.

### [!UICONTROL Metric] {#metric}

Die Metrik ist ein Attribut des Abonnentenkontos (z. B. dessen MVPD, die Programmierer und Kanäle des gestreamten Inhalts und die Anzahl der von ihnen verwendeten Geräte).

### [!UICONTROL Mobile device] {#mobile-device-def}

Ein Gerät mit hoher Mobilität. Beispiel: Mobiltelefon und Tablet.

### Bedienung {#operation-def}

Operation ist ein Datensatz, der erstellt wird, um die Wirkung einer bestimmten [Aktion](#action-def) auf ein verknüpftes Segment zu verfolgen. Ein Beispiel für eine Aktion kann eine Begrenzung der Anzahl gleichzeitiger Streams sein, die für durch das Segment identifizierte Konten zulässig sind.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

Ein Wert, der Benutzenden hilft, das Ausmaß der Kennwortfreigabe zu verstehen, und ihnen ein Gefühl der Dringlichkeit vermittelt, entsprechend zu handeln.

### [!UICONTROL Play Request] {#play-requests-def}

Entspricht einem Stream-Start. Dieses Ereignis markiert den Beginn eines Benutzer-Streaming-Inhalts.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

Dieser Wert wird auch als Nutzung von freigegebenen Konten bezeichnet und basiert auf der Anzahl der Wiedergabeanfragen, die von jedem einzelnen Konto gesendet und mit der Freigabewahrscheinlichkeit jedes Kontos gewichtet werden. Er wird auch als Usage by Shared Accounts Risk Index bezeichnet.

### [!UICONTROL Segment] {#segmet-def}

Segment ist ein Satz von Konten, die die von den ausgewählten Metriken angegebenen benutzerdefinierten Bedingungen erfüllen. Beispiel: „Benutzende in den Regionen A, B, C, D oder E, die mehr als drei Geräte haben“.

### [!UICONTROL Sharing level] {#sharing-level-def}

Dieser Wert wird auch als Risikoindex - Risikoindex für Konten oder freigegebene Konten bezeichnet und basiert auf dem Durchschnitt der Freigabewahrscheinlichkeit, die für jedes Konto im aktuellen Segment berechnet wird, das im ausgewählten Zeitintervall mindestens einmal gestreamt hat.

### [!UICONTROL Static device] {#static-device-def}

Ein Gerät mit sehr geringer Beweglichkeit. Zum Beispiel Spielkonsole, Set-Top-Box und Fernseher.

### [!UICONTROL Time interval] {#time-interval-def}

Dieses Zeitfenster wird auch als Zeitraum bezeichnet und enthält die Wiedergabeanfrageaktivität, die in der Benutzeroberfläche dargestellt wird, sowie Tabellen von Anfang bis Ende.

### [!UICONTROL Trend] {#trend-def}

Der prozentuale Unterschied in der zugehörigen Metrik (z. B. Prozentsatz der gesamten Wiedergabeanforderungen) zwischen dem aktuellen und dem vorherigen Zeitraum.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Die Anzahl der eindeutigen Konten, die in einem bestimmten Zeitraum mindestens einmal gestreamt haben.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

Mehr als 37 Spielanfragen pro Monat.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

Weniger als 9 Wiedergabeanfragen pro Monat.

#### [!UICONTROL Regular user] {#regular-user-def}

Von 9 bis 37 Spielanfragen pro Monat.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

Eines einer endlichen Reihe von Kategoriekennzeichnungen, die auf ein Konto angewendet werden und die Benutzer des Kontos in Bezug auf soziale Gruppen oder Verhaltensweisen am besten charakterisieren (z. B. eine kleine Familie, ein Reisender oder Pendler, Social Sharing usw.).

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

Eine Videokategorie ist eine bestimmte Beschriftung, die die Art des Videos kennzeichnet. Beispielsweise die Region der Quelle, Inhaltstypen wie VOD oder Live, Ereignis oder bestimmte Kennzeichnungen wie Team.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

Eine Videokategorie wird durch die Kombination aus Programmierer, Kanal und MVPD definiert, die mit dem Stream verknüpft sind.

### [!UICONTROL Zip Code] {#zip-code-def}

Die US-Postleitzahl, die mit Standorten innerhalb der USA verknüpft ist.
<!--calculated metrics-->


## TV Everywhere - Spezifische Terminologien

### [!UICONTROL AuthZ] {#authz-def}

Autorisierung oder die Anzahl der Autorisierungsanfragen. Eine Autorisierungsanfrage ist der Prozess, bei dem ein Programmierer die Berechtigung von einer MVPD über Adobe anfordert, um mit dem Streaming der angeforderten Inhalte einer Benutzerin oder eines Benutzers zu beginnen. Die MVPD gewährt die Anfrage in der Regel anhand der Inhaltsrechte, die mit dem MVPD-Abonnement des Benutzers verknüpft sind (z. B. ob der mit dem Inhalt verknüpfte Kanal im Abonnement des Benutzers enthalten ist). Einige Antworten auf Autorisierungsanfragen werden per Adobe zwischengespeichert, sodass Adobe sofort reagieren kann, ohne die Anfrage an MVPD weiterzugeben.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

Die Anzahl der erfolgreichen Autorisierungen.

### [!UICONTROL Channel] {#channel-def}

Der Kanal, auch als Eigenschaft bezeichnet, ist eine thematisch verwandte Quelle von Videoinhalten. Herkömmliche Darstellung eines eindeutigen, numerisch adressierbaren kontinuierlichen Video-Feeds aus einem MVPD. Der Kanal ist direkt einem barrierefreien Kanal von Inhalten zugeordnet, die den Abonnenten über ihre Set Top Box (STB) zur Verfügung stehen.

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Ein für jeden Risikoindex (Konten, Nutzung, Gesamt) für alle Programmierer und MVPDs während des ausgewählten Zeitintervalls berechneter Wert.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

Eine Art der Freigabeanalyse, bei der die Auswertung eines Kontos auf Ereignisse beschränkt ist, die direkt bei den Programmierern im ausgewählten Segment aufgetreten sind.  Normalerweise werden alle Kontoereignisse ausgewertet, was eine viel genauere Schätzung der Freigabe ermöglicht.  Einige MVPD-Daten sind so strukturiert, dass nur der Isolationsmodus analysiert werden kann.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD, auch bekannt als Distributor, ist ein Aggregator, Reseller und Distributor von Media Company-Videoinhalten.

### [!UICONTROL Programmer] {#programmer-def}

Programmierer, auch bekannt als Network, ist ein Unternehmen, das Tochtergesellschaft eines größeren Unternehmens (Unternehmen) ist, das einen oder mehrere Kanäle besitzt und verwaltet.

### [!UICONTROL requestorID] {#requestorid-def}

Die ID, die ein Medienunternehmen verwendet, um sich selbst oder eine Tochtergesellschaft einer MVPD zu identifizieren.  Abhängig von der Implementierung des Programmierers kann dies einem Medienunternehmen, Programmierer oder Kanal zugeordnet werden. Üblicherweise wird dies einem Kanal zugeordnet.  Mit der Erstellung von Pseudo-Kanälen wie MML (March Madness Live) und technisch gesteuerten Schritten zur Behebung von MVPD-basierten Datenbeschränkungen wird die RequestorID mehr und mehr mit dem Medienunternehmen in Verbindung gebracht.

### [!UICONTROL resourceID] {#resource-id-def}

Der vom Endbenutzer angeforderte Inhalt. Herkömmlicherweise wird dadurch der Kanal identifiziert, der mit dem Inhalt verknüpft ist, den der Benutzer angefordert hat.  Systemverbesserungen ermöglichen es dieser ID, bestimmte Programme (z. B. mit bestimmten Bewertungen) darzustellen. Die ID identifiziert weiterhin den zugehörigen Kanal.


