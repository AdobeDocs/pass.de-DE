---
title: API-Übersicht
description: API-Übersicht
exl-id: 3fe6f6d8-5b2f-47e5-a8da-06fb18a5d46b
source-git-commit: 301825253b21746684df9b6372a239b03305d50e
workflow-type: tm+mt
source-wordcount: '2043'
ht-degree: 0%

---

# API zur Überwachung der gleichzeitigen Nutzung {#cmu-api-usage}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## API-Übersicht {#api-overview}

Die gleichzeitige Überwachung der Nutzung (CMU) wird als WOLAP-Projekt (Web-basierte [Online Analytical Processing](http://en.wikipedia.org/wiki/Online_analytical_processing)) implementiert. CMU ist eine generische Business-Reporting-Web-API, die von einem Data Warehouse unterstützt wird. Sie dient als HTTP-Abfragesprache, mit der typische OLAP-Vorgänge RESTful ausgeführt werden können.


>[!NOTE]
>
>Die CMU-API ist nicht allgemein verfügbar. Bei Fragen zur Verfügbarkeit wenden Sie sich an Ihren Adobe-Support.

Die CMU-API bietet eine hierarchische Ansicht der zugrunde liegenden OLAP-Cubes. Jede Ressource ([Dimension](/help/concurrency-monitoring/cm-usage-reports.md#dimensions-2-filter-metrics) in der Dimensionshierarchie, zugeordnet als URL-Pfadsegment) generiert Berichte mit (aggregierten) [Metriken](/help/concurrency-monitoring/cm-usage-reports.md#monitor-metrics) für die aktuelle Auswahl. Jede Ressource verweist auf ihre übergeordnete Ressource (für die Aufschlüsselung) und ihre Unterressourcen (für die Aufschlüsselung). Slicing und Dicing werden über Abfragezeichenfolgen-Parameter erreicht, die Dimensionen an bestimmte Werte oder Bereiche anheften.

Die REST-API stellt die verfügbaren Daten innerhalb eines in der Anfrage angegebenen Zeitintervalls (d. h. zurück auf die Standardwerte, wenn keine Werte angegeben wurden) bereit, und zwar entsprechend dem Dimensionspfad, den bereitgestellten Filtern und den ausgewählten Metriken. Der Zeitbereich wird nicht auf Berichte angewendet, die keine Zeitdimensionen enthalten (Jahr, Monat, Tag, Stunde, Minute, Sekunde).

Der Endpunkt-URL-Stammpfad gibt die gesamten aggregierten Metriken innerhalb eines einzigen Datensatzes zusammen mit den Links zu den verfügbaren Drilldown-Optionen zurück. Die API-Version wird als nachfolgendes Segment des Endpunkt-URI-Pfads zugeordnet. Beispielsweise bedeutet https://mgmt.auth.adobe.com/cmu/*v2*, dass die Clients auf WOLAP Version 2 zugreifen.

Die verfügbaren URL-Pfade sind über in der Antwort enthaltene Links auffindbar. Gültige URL-Pfade werden gespeichert, um einen Pfad innerhalb der zugrunde liegenden Drilldown-Struktur zuzuordnen, der (vor-)aggregierte Metriken enthält. Ein Pfad in der Form /dimension1/dimension2/dimension3 spiegelt eine Voraggregation dieser drei Dimensionen wider (das Äquivalent einer SQL-Klausel GROUP BY dimension1, dimension2, dimension3). Wenn eine solche Voraggregation nicht vorhanden ist und das System sie nicht spontan berechnen kann, gibt die API die Antwort „404 Nicht gefunden“ zurück.

### Drilldown-Baum {#drill-down-tree}

Die folgenden Drilldown-Bäume veranschaulichen die in CMU 2.0 verfügbaren Dimensionen (Ressourcen):

**Dimensionen für CM-Mandanten verfügbar**

![](assets/new_breakdown.png)

Ein `GET` an den `https://mgmt.auth.adobe.com/cmu/v2`-API-Endpunkt gibt eine Darstellung zurück, die Folgendes enthält:

* Links zu den verfügbaren Stamm-Drilldown-Pfaden:

  ```html
  <link rel="drill-down" href="/cmu/v2/dimensionA"/>
  <link rel="drill-down" href="/cmu/v2/dimensionB"/>
  ```

* Eine Zusammenfassung (aggregierte Werte) für alle Metriken (im Standardintervall, da keine Abfragezeichenfolgen-Parameter bereitgestellt werden, siehe unten).

Nach einem Drilldown-Pfad (Schritt für Schritt): /dimensionA/year/month/day/dimensionX ruft die folgende Antwort ab:

* Links zu den Drilldown-Optionen „dimensionY“ und „dimensionZ“
* Ein Bericht mit täglichen Aggregaten für jeden Wert der DimensionX

### **Filter**

Mit Ausnahme der Datums-/Uhrzeitdimensionen können alle für die aktuelle Projektion verfügbaren Dimensionen (Dimensionspfad) gefiltert werden, indem ihr Name als Abfragezeichenfolgenparameter verwendet wird.

Die folgenden Filteroptionen sind verfügbar:

* **Gleich**-Filter werden bereitgestellt, indem der Dimensionsname in der Abfragezeichenfolge auf einen bestimmten Wert festgelegt wird.
* **IN** Filter können angegeben werden, indem derselbe Dimensionsnamensparameter mehrmals mit unterschiedlichen Werten hinzugefügt wird: dimension=value1&amp;dimension=value2
* **Ungleich** Filter müssen &quot;!“ verwenden Symbol nach dem Dimensionsnamen, was zu &quot;!=&#39; „Operator“: Dimension!=Wert
* **NOT IN** Filter erfordern &quot;!=&#39; Operator muss mehrmals verwendet werden, und zwar einmal für jeden Wert im Satz: Dimension!=Wert1&amp;Dimension!=Wert2&amp; …


Es gibt auch eine besondere Verwendung für die Dimensionsnamen in der Abfragezeichenfolge: Wenn der Dimensionsname als Abfragezeichenfolgenparameter ohne Wert verwendet wird, wird die API angewiesen, eine Projektion zurückzugeben, die diese Dimension im Bericht enthält.

Beispiele für CMU-Abfragen:

| URL | SQL-Äquivalent |
|:---|:---|
| /dimension1/dimension2/dimension3?dimension1=value1 | SELECT * from projection WHERE dimension1 = &#39;value1&#39; GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1=value1&amp;dimension1=value2 | SELECT * from projection WHERE dimension1 IN (&#39;value1&#39;, &#39;value2&#39;) GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=Wert1 | SELECT * from projection WHERE dimension1 &lt;> &#39;value1&#39; GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=Wert1&amp;Dimension2!=Wert2 | SELECT * from projection WHERE dimension1 NOT IN (&#39;value1&#39;, &#39;value2&#39;) GROUP BY dimension1, dimension2, dimension3 |
| Angenommen, es gibt keinen direkten Pfad: /dimension1/dimension3, aber einen Pfad: /dimension1/dimension2/dimension3 </br></br> /dimension1?dimension3 | SELECT * from projection GROUP BY dimension1,dimension3 |

>[!NOTE]
>
>Keine dieser Filtertechniken funktioniert für Datums-/Uhrzeitdimensionen. Die einzige Möglichkeit, Datums-/Uhrzeitdimensionen zu filtern, besteht darin, die Abfragezeichenfolgenparameter für Start und Ende (siehe unten) auf die erforderlichen Werte festzulegen.

Die folgenden Abfragezeichenfolgenparameter haben reservierte Bedeutungen für die API (und können daher nicht als Dimensionsnamen verwendet werden, da ansonsten keine Filterung für eine solche Dimension möglich wäre).

CMU API Reservierte Abfragezeichenfolgen-Parameter:

| Parameter | optional | Beschreibung | Standardwert | Beispiel |
|:--------------|:--------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------|
| access_token | Ja | Wenn der IMS-OAuth-Schutz aktiviert ist, kann das IMS-Token entweder als standardmäßiges Autorisierungs-Bearer-Token oder als Abfragezeichenfolgenparameter übergeben werden. | Keine | access_token=XXXXXX |
| Dimensionsname | Ja | Beliebiger Dimensionsname - entweder im aktuellen URL-Pfad oder in einem gültigen Unterpfad enthalten. Der Wert wird als Filter „ist gleich“ behandelt. Wenn kein Wert angegeben wird, erzwingt dies die Aufnahme der angegebenen Dimension in die Ausgabe, selbst wenn sie nicht enthalten ist oder an den aktuellen Pfad angrenzt | Keine | someDimension=someValue&amp;someOtherDimension |
| Ende | Ja | Endzeit für den Bericht in Millionen | Aktuelle Zeit des Servers | Ende=30.07.2012 |
| Format | Ja | Wird für die Inhaltsaushandlung verwendet (mit demselben Effekt, aber niedrigerer Priorität als der Pfad „Erweiterung“ - siehe unten). | Keine: die Inhaltsaushandlung wird die anderen Strategien ausprobieren | format=json |
| Grenze | Ja | Maximale Anzahl an zurückzugebenden Zeilen | Vom Server im Self-Link gemeldeter Standardwert, wenn in der Anfrage kein Limit angegeben ist | Limit=1500 |
| Metriken | Ja | Kommagetrennte Liste der zurückzugebenden Metriknamen. Diese sollte sowohl zum Filtern einer Teilmenge der verfügbaren Metriken (um die Payload-Größe zu reduzieren) als auch zum Erzwingen verwendet werden, dass die API eine Projektion zurückgibt, die die angeforderten Metriken enthält (anstelle der standardmäßigen optimalen Projektion). | Alle für die aktuelle Projektion verfügbaren Metriken werden zurückgegeben, falls dieser Parameter nicht angegeben wird. | Metriken=m1,m2 |
| anfangen | Ja | Startzeit für den Bericht als ISO8601; der Server füllt den verbleibenden Teil aus, wenn nur ein Präfix angegeben wird: z. B. führt start=2012 zu start=2012-01-01:00:00:00 | Wird vom Server im Self-Link gemeldet. Der Server versucht, basierend auf der ausgewählten Zeitgranularität angemessene Standardwerte bereitzustellen | start=2012-07-15 |


Die einzige verfügbare HTTP-Methode ist derzeit GET. Unterstützung für OPTIONS-/HEAD-Methoden kann in zukünftigen Versionen bereitgestellt werden.



## CMU-API-Status-Codes {#cmu-api-status-codes}

| Statuscode | Reason | Beschreibung |
|:-----------|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200 | OK | Die Antwort enthält Links zum „Rollout“ und „Drilldown“ (falls zutreffend). Der Bericht wird als Attribut der Ressource gerendert: ein verschachteltes Element bzw. eine verschachtelte Eigenschaft „Bericht“. |
| 400 | Fehlerhafte Anfrage | Der Antworttext enthält eine Textnachricht, die erklärt, was mit der Anfrage nicht stimmt.  Zu einem Status „Fehlerhafte 400-Anfrage“ gehört ein erklärender Text im Antworttext (Nur-Text-/Medientyp), der nützliche Informationen zum Client-Fehler bereitstellt. Neben den trivialen Szenarien wie ungültigen Datumsformaten oder Filtern, die auf nicht vorhandene Dimensionen angewendet werden, weigert sich das System auch, auf Abfragen zu reagieren, für die eine enorme Datenmenge zurückgegeben oder spontan aggregiert werden muss. |
| 401 | Nicht autorisiert | Wird durch eine Anfrage verursacht, die nicht die richtigen OAuth-Header zum Authentifizieren des Benutzers enthält |
| 403 | Verboten | Gibt an, dass die Anfrage im aktuellen Sicherheitskontext nicht zulässig ist. Dies tritt auf, wenn der Benutzer authentifiziert ist, aber nicht auf die angeforderten Informationen zugreifen darf |
| 404 | Nicht gefunden | Tritt auf, wenn mit der Anfrage ein ungültiger URL-Pfad angegeben wird. Dies sollte nie vorkommen, wenn der Client den Links für „Drilldown“/„Rollout“ folgt, die mit 200 Antworten bereitgestellt werden |
| 405 | Methode nicht zulässig | Signalisiert, dass eine nicht unterstützte Methode in der Anfrage verwendet wurde. Obwohl derzeit nur die GET-Methode unterstützt wird, können zukünftige Versionen HEAD oder OPTIONS zulassen |
| 406 | Nicht akzeptabel | Signalisiert, dass ein nicht unterstützter Medientyp vom Client angefordert wurde |
| 500 | Interner Server-Fehler | „Das sollte nie passieren“ |
| 503 | Service nicht verfügbar | Signalisiert einen Fehler innerhalb der Anwendung oder ihrer Abhängigkeiten |

## Datenformate {#data-formats}

Die Daten sind in den folgenden Formaten verfügbar:

* JSON (Standard)
* XML
* CSV
* HTML (zu Demozwecken)


Die folgenden Strategien zur Inhaltsaushandlung können von Clients verwendet werden (der Vorrang wird durch die Position in der Liste gegeben - erste Dinge):

1. Eine „Dateierweiterung“, die an das letzte Segment des URL-Pfads angehängt wird: z. B. /cmu/v2/tenant/year/month/day.xml. Wenn die URL eine Abfragezeichenfolge enthält, muss die Erweiterung vor dem Fragezeichen stehen: `/cmu/v2/tenant/year/month/day.csv?mvpd=SomeMVPD`
1. Ein Format-Abfragezeichenfolgenparameter: z. B. `/cmu/report?format=json`
1. Der Standard-HTTP-Accept-Header: z. B. `Accept: application/xml`

Sowohl die Erweiterung als auch der Abfrageparameter unterstützen die folgenden Werte:

* XML
* JSON
* CSV
* HTML

Wenn in einer der Strategien kein Medientyp angegeben ist, erstellt die API standardmäßig JSON-Inhalte.

## Hypertext Application Language (HAL) {#hypertext-app-lang}

Für JSON und XML wird die Payload als HAL codiert, wie hier beschrieben: `http://stateless.co/hal_specification.html`.

Der eigentliche Bericht (ein verschachteltes Tag/eine verschachtelte Eigenschaft namens „Bericht„) besteht aus der tatsächlichen Liste von Datensätzen, die alle ausgewählten/anwendbaren Dimensionen und Metriken mit ihren Werten enthalten, und zwar wie folgt:

### JSON {#json}

```js
 "report": [
  {
    "dimension1": "d1",
    ...
    "metric1": "m1",
    ...
  }, {
    ...
  }
]
```

### XML {#xml}

```xml
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report>
```

Bei XML- und JSON-Formaten ist die Reihenfolge der Felder (Dimensionen und Metriken) in einem Datensatz nicht angegeben, aber konsistent (die Reihenfolge ist in allen Datensätzen gleich). Kunden sollten sich jedoch nicht auf eine bestimmte Reihenfolge der Felder innerhalb eines Datensatzes verlassen.

Der Ressourcen-Link (das „Selbst“-rel in JSON und das „href“-Ressourcenattribut in XML) enthält den aktuellen Pfad und die Abfragezeichenfolge, die für den Inline-Bericht verwendet wird. Die Abfragezeichenfolge enthüllt alle impliziten und expliziten Parameter, sodass die Payload explizit auf das verwendete Zeitintervall, die impliziten Filter (falls vorhanden) usw. hinweist. Der Rest der Links innerhalb der Ressource enthält alle verfügbaren Segmente, denen Sie folgen können, um eine Aufschlüsselung der aktuellen Daten durchzuführen. Ein Link für die Datenaggregation wird ebenfalls bereitgestellt und verweist auf den übergeordneten Pfad (falls vorhanden). Der `href` für die Drill-down-/Roll-up-Links enthält nur den URL-Pfad (er enthält nicht die Abfragezeichenfolge, daher muss dieser bei Bedarf vom Client angehängt werden). Beachten Sie, dass nicht alle der von der aktuellen Ressource verwendeten (oder implizierten) Abfragezeichenfolgenparameter für „Hochrechnungs“- oder „Drilldown“-Links gelten (die Filter können beispielsweise nicht auf Unter- oder Überressourcen angewendet werden).

Beispiel (unter der Annahme, dass wir eine einzelne Metrik namens Clients haben und es eine Voraggregation für `year/month/day/...` gibt):

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.xml`

  ```xml
  <resource href="/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21">
    <links>
      <link rel="roll-up" href="/cmu/v2/year"/>
      <link rel="drill-down" href="/cmu/v2/year/month/day"/>
    </links>
    <report>
      <record month="6" year="2012" clients="205"/>
      <record month="7" year="2012" clients="466"/>
    </report>
  </resource>
  ```

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.json`

  ```js
  {
    "_links" : {
      "self" : {
        "href" : "/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21"
      },
      "roll-up" : {
        "href" : "/cmu/v2/year"
      },
      "drill-down" : {
        "href" : "/cmu/v2/year/month/day"
      }
    },
    "report" : [ {
      "month" : "6",
      "year" : "2012",
      "clients" : "205"
    }, {
      "month" : "7",
      "year" : "2012",
      "clients" : "466"
    } ]
  }
  ```

### CSV {#csv}

Im CSV-Datenformat werden keine Links oder anderen Metadaten (mit Ausnahme der Kopfzeile) inline bereitgestellt. Stattdessen werden die Auswahl-Metadaten im Dateinamen bereitgestellt, der diesem Muster folgt:

```html
report__<start-date>_<end-date>_<filter-values,...>.csv
```

Die CSV enthält eine Kopfzeile und die Berichtsdaten als nachfolgende Zeilen. Die Kopfzeile enthält alle Dimensionen gefolgt von allen Metriken. Die Sortierreihenfolge der Berichtsdaten wird in der Reihenfolge der Dimensionen angezeigt. Wenn die Daten also nach D1 und dann nach D2 sortiert werden, sieht die CSV-Kopfzeile wie folgt aus: `D1, D2, ...metrics....`

Die Reihenfolge der Felder in der Kopfzeile entspricht der Sortierreihenfolge der Tabellendaten.

Beispiel: https://mgmt.auth.adobe.com/cmu/v2/year/month.csv erstellt eine Datei mit dem Namen ```report__2012-07-20_2012-08-20_1000.csv``` mit folgendem Inhalt:

| Jahr | Monat | Clients |
|:----:|:-----:|:-------:|
| 2012 | 6 | 580 |
| 2012 | 7 | 231 |

## Aktualität der Daten {#data-freshness}

Obwohl die Anfrage eine Kopfzeile „Zuletzt geändert“ enthält **gibt sie** Zeitpunkt an, zu dem der Bericht im Hauptteil zuletzt aktualisiert wurde. Die allgemeinen Berichte werden regelmäßig nach folgenden Regeln berechnet:

* Wenn die Zeitgranularität **Jahr** oder **Monat** ist, wird der Bericht alle 2 Tage aktualisiert
* Wenn die Zeitgranularität **Tag** beträgt, wird der Bericht alle 3 Stunden aktualisiert
* Wenn die Zeitgranularität &quot;**&quot;**, wird der Bericht stündlich aktualisiert
* Wenn die Zeitgranularität **Minute** beträgt, wird der Bericht jede Minute aktualisiert

Die Berichte **Aktivitätsebene** und **Parallelitätsebene** werden täglich aktualisiert, unabhängig von der Zeitgranularität.

## GZIP-Komprimierung {#gzip-compression}

Adobe empfiehlt dringend, die gzip-Unterstützung in Clients zu aktivieren, die CMU-Berichte abrufen. Dadurch wird die Größe der Antwort erheblich reduziert, was wiederum die Reaktionszeit verkürzt. (Das Komprimierungsverhältnis für CMU-Daten liegt im Bereich von 20-30.)

Um die gzip-Komprimierung in Ihrem Client zu aktivieren, legen Sie den Accept-Encoding: -Header wie folgt fest:

```
Accept-Encoding: gzip, deflate
```

## Ergänzende Informationen {#related-information}

* [Übersicht über die Kapitalmarktunion](/help/concurrency-monitoring/cm-usage-reports.md)
