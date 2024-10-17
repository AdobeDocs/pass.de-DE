---
title: API zur Überwachung von Entitätsdiensten
description: API zur Überwachung von Entitätsdiensten
exl-id: a9572372-14a6-4caa-9ab6-4a6baababaa1
source-git-commit: 8fa1e63619f4e22794d701a218c77649f73d9f60
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 0%

---

# API zur Überwachung von Entitätsdiensten {#entitlement-service-monitoring-api}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Stellen Sie vor Verwendung der Abbauungs-API sicher, dass die folgenden Voraussetzungen erfüllt sind:
>
> * Rufen Sie die Client-Anmeldeinformationen ab, wie in der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) beschrieben.
> * Rufen Sie das Zugriffstoken ab, wie in der API-Dokumentation [Zugriffstoken abrufen](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben.
>
> Weitere Informationen zum Erstellen einer registrierten Anwendung und Herunterladen der Softwareanweisung finden Sie in der Dokumentation zur [Übersicht über die dynamische Client-Registrierung](./dcr-api/dynamic-client-registration-overview.md) .

## API-Übersicht {#api-overview}

Entitlement Service Monitoring (ESM) wird als WOLAP-Projekt (Web-based [Online Analytical Processing](https://en.wikipedia.org/wiki/Online_analytical_processing){target=_blank}) implementiert. ESM ist eine generische Web-API für Geschäftsberichte, die von einem Data Warehouse unterstützt wird. Es dient als HTTP-Abfragesprache, die die Ausführung typischer OLAP-Vorgänge in RESTfull ermöglicht.

>[!NOTE]
>
>Die ESM-API ist nicht allgemein verfügbar. Wenden Sie sich bei Fragen zur Verfügbarkeit an Ihren Adobe-Support-Mitarbeiter.

Die ESM-API bietet eine hierarchische Ansicht der zugrunde liegenden OLAP-Cubes. Jede Ressource ([Dimension](#esm_dimensions) in der Dimensionshierarchie, die als URL-Pfadsegment zugeordnet ist) generiert Berichte mit (aggregierten) [Metriken](#esm_metrics) für die aktuelle Auswahl. Jede Ressource verweist auf ihre übergeordnete Ressource (für Datenaggregationen) und ihre Unterressourcen (für Drilldown). Slicing und Dicing werden über Abfragezeichenfolgenparameter erreicht, die Dimensionen an bestimmte Werte oder Bereiche anhängen.

Die REST-API stellt die verfügbaren Daten innerhalb eines in der Anfrage angegebenen Zeitintervalls bereit (wobei auf die Standardwerte zurückgegriffen wird, wenn keine Werte angegeben sind). Dies hängt vom Dimensionspfad, den bereitgestellten Filtern und ausgewählten Metriken ab. Der Zeitraum wird nicht für Berichte angewendet, die keine Zeitdimensionen enthalten (Jahr, Monat, Tag, Stunde, Minute, Sekunde).

Der Stammpfad der Endpunkt-URL gibt die aggregierten Gesamtmetriken innerhalb eines einzelnen Datensatzes zusammen mit den Links zu den verfügbaren Drilldown-Optionen zurück. Die API-Version wird als nachstehendes Segment des Endpunkt-URI-Pfads zugeordnet. Beispiel: `https://mgmt.auth.adobe.com/esm/v3` bedeutet, dass die Clients auf WOLAP Version 3 zugreifen.

Die verfügbaren URL-Pfade können über die in der Antwort enthaltenen Links gefunden werden. Gültige URL-Pfade werden gespeichert, um einen Pfad innerhalb der zugrunde liegenden Drilldown-Struktur zuzuordnen, der aggregierte (vorab erstellte) Metriken enthält. Ein Pfad im Formular `/dimension1/dimension2/dimension3` spiegelt eine Voraggregation dieser drei Dimensionen wider (entspricht einer SQL `clause GROUP` BY `dimension1`, `dimension2`, `dimension3`). Wenn eine solche Voraggregation nicht vorhanden ist und das System sie nicht sofort berechnen kann, gibt die API eine Antwort &quot;404 Not Found&quot;zurück.

## Drilldown-Struktur {#drill-down-tree}

Die folgenden Drilldown-Bäume veranschaulichen die in ESM 3.0 verfügbaren Dimensionen (Ressourcen) für [Programmierer](#progr-dimensions) und [MVPDs](#mvpd-dimensions).


### Dimensionen für Programmierer {#progr-dimensions}

#### Tag

![](assets/esm-progr-dimensions-day.png)

#### Stunde

![](assets/esm-progr-dimensions-hour.png)

#### Minute

![](assets/esm-progr-dimensions-minute.png)

### Für MVPDs verfügbare Dimensionen {#mvpd-dimensions}

![](assets/esm-mvpd-dimensions.png)

Ein GET zum API-Endpunkt `https://mgmt.auth.adobe.com/esm/v3` gibt eine Darstellung zurück, die Folgendes enthält:

* Links zu den verfügbaren Root-Drilldown-Pfaden:

   * `<link rel="drill-down" href="/v3/dimensionA"/>`

   * `<link rel="drill-down" href="/v3/dimensionB"/>`

* Eine Zusammenfassung (aggregierte Werte) für alle Metriken (standardmäßig
-Intervall, da keine Abfragezeichenfolgenparameter angegeben sind, siehe unten).


Folgen Sie einem Drilldown-Pfad (Schritt für Schritt):
`/dimensionA/year/month/day/dimensionX` ruft Folgendes ab
Antwort:

* Links zu den Drilldown-Optionen &quot;`dimensionY`&quot;und &quot;`dimensionZ`&quot;

* Ein Bericht mit täglichen Aggregaten für jeden Wert von `dimensionX`


### Filter

Mit Ausnahme der Datums-/Uhrzeitdimensionen kann jede für die aktuelle Projektion (Dimensionspfad) verfügbare Dimension anhand ihres Namens als Abfragezeichenfolgenparameter gefiltert werden.

Die folgenden Filteroptionen sind verfügbar:

* **Entspricht** Filtern wird bereitgestellt, indem der Dimensionsname in der Abfragezeichenfolge auf einen bestimmten Wert gesetzt wird.

* **IN** -Filter können angegeben werden, indem der Parameter &quot;dimension-name&quot;mehrmals mit verschiedenen Werten hinzugefügt wird: dimension=value1\&amp;dimension=value2

* **Nicht gleich** Filter müssen den &#39;\!&#39; verwenden Symbol hinter dem Dimensionsnamen, das zu &quot;\!&quot;führt.=&#39; &quot;operator&quot;: dimension\!=value

* **NOT IN** -Filter erfordern den &#39;\!=&#39;&#39;-Operator, der mehrmals verwendet wird, einmal für jeden Wert im Satz: Dimension\!=value1\&amp;dimension\!=value2&amp;...

Außerdem werden die Dimensionsnamen in der Abfragezeichenfolge besonders verwendet: Wenn der Dimensionsname als Abfragezeichenfolgenparameter ohne Wert verwendet wird, weist dies die API an, eine Projektion zurückzugeben, die diese Dimension im Bericht enthält.

### Beispiel-ESM-Abfragen

| *URL* | *SQL-Äquivalent* |
|---|---|
| /dimension1/dimension2/dimension3?dimension1=value1 | SELECT * from projektion WHERE dimension1 = &#39;value1&#39; </br> GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1=value1&amp;dimension1=value2 | SELECT * from projektion WHERE dimension1 IN (&#39;value1&#39;, &#39;value2&#39;) </br> GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=value1 | SELECT * from projektion WHERE dimension1 &lt;> &#39;value1&#39; | </br> GRUPPE NACH Dimension1, Dimension2, Dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=value1&amp;dimension2!=value2 | SELECT * from projektion WHERE dimension1 NOT IN (&#39;value1&#39;, &#39;value2&#39;) | </br> GRUPPE NACH Dimension1, Dimension2, Dimension3 |
| Angenommen, es gibt keinen direkten Pfad: /dimension1/dimension3 </br>, aber es gibt einen Pfad: /dimension1/dimension2/dimension3 </br> </br> /dimension1?dimension3 | AUSWÄHLEN * AUS ProjektionsGRUPPE NACH Dimension1, Dimension3 |

>[!NOTE]
>
>Keine dieser Filtermethoden funktioniert für `date/time` -Dimensionen. Die einzige Möglichkeit, `date/time` -Dimensionen zu filtern, besteht darin, die Abfragezeichenfolgenparameter `start` und `end` (siehe unten) auf die erforderlichen Werte festzulegen.

Die folgenden Abfragezeichenfolgenparameter haben reservierte Bedeutungen für die API (und können daher nicht als Dimensionsnamen verwendet werden, sonst ist keine Filterung für eine solche Dimension möglich).

### ESM API-reservierte Abfragezeichenfolgenparameter

| Parameter | Optional | Beschreibung | Standardwert | Beispiel |
| --- | ---- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ---- | --- |
| access_token | Ja | Das DCR-Token kann als standardmäßiges Autorisierungs-Trägertoken übergeben werden. | Keines | access_token=XXXXXX |
| dimension-name | Ja | Jeder Dimensionsname - entweder im aktuellen URL-Pfad oder in einem gültigen Unterpfad enthalten; der Wert wird als gleich Filter behandelt. Wenn kein Wert angegeben wird, erzwingt dies, dass die angegebene Dimension in die Ausgabe aufgenommen wird, auch wenn sie nicht enthalten ist oder an den aktuellen Pfad angrenzt | Keines | someDimension=someValue&amp;someOtherDimension |
| end | Ja | Endzeit für den Bericht in Millisekunden | Aktuelle Zeit des Servers | end=2012-07-30 |
| format | Ja | Wird für die Inhaltsverhandlung verwendet (mit demselben Effekt, aber geringerer Priorität als der Pfad &quot;Erweiterung&quot;- siehe unten). | Keine: Bei der Inhaltsverhandlung werden die anderen Strategien getestet | format=json |
| limit | Ja | Maximale Anzahl an zurückzugebenden Zeilen | Der vom Server im Self-Link gemeldete Standardwert, wenn in der Anfrage keine Begrenzung angegeben ist | limit=1500 |
| Metriken | Ja | Kommagetrennte Liste der zurückzugebenden Metriknamen. Diese sollte zum Filtern einer Untergruppe der verfügbaren Metriken (um die Payload-Größe zu reduzieren) und auch zum Erzwingen der API verwendet werden, eine Projektion zurückzugeben, die die angeforderten Metriken enthält (und nicht die standardmäßige optimale Projektion). | Alle für die aktuelle Projektion verfügbaren Metriken werden zurückgegeben, falls dieser Parameter nicht angegeben wird. | metrics=m1,m2 |
| start | Ja | Startzeit für den Bericht als ISO8601; der Server füllt den verbleibenden Teil aus, wenn nur ein Präfix angegeben wird: Beispielsweise führt start=2012 zu start=2012-01-01:00:00:00. | Vom Server in der Selbstverknüpfung gemeldet; der Server versucht, basierend auf der ausgewählten Zeitgranularität angemessene Standardwerte bereitzustellen. | start=2012-07-15 |

Die einzige verfügbare HTTP-Methode ist derzeit GET.

## ESM-API-Statuscodes {#esm-api-status-codes}

| Status-Code | Reason Phrase | Beschreibung |
|---|---|---|
| 200 | OK | Die Antwort enthält Links für &quot;Datenaggregation&quot;und &quot;Drilldown&quot;(falls zutreffend). Der Bericht wird als Attribut der Ressource gerendert: als verschachteltes Element/Eigenschaft &quot;Bericht&quot;. |
| 400 | Ungültige Anfrage | Der Antworttext enthält eine Textmeldung, die erklärt, was mit der Anfrage nicht stimmt. </br> </br> Dem Status &quot;Ungültige Anfrage 400&quot;wird ein erläuternder Text im Antworttext (Nur-/Text-Medientyp) hinzugefügt, der nützliche Informationen zum Client-Fehler liefert. Neben trivialen Szenarien wie ungültigen Datumsformaten oder Filtern, die auf nicht vorhandene Dimensionen angewendet werden, weicht das System auch die Beantwortung von Abfragen ab, bei denen ein massives Datenvolumen sofort zurückgegeben oder aggregiert werden muss. |
| 401 | Unerlaubt | Wird durch eine Anfrage ausgelöst, die nicht die richtigen OAuth-Header enthält, um den Benutzer zu authentifizieren |
| 403 | Verboten | Gibt an, dass die Anfrage im aktuellen Sicherheitskontext nicht zulässig ist. Dies geschieht, wenn der Benutzer authentifiziert ist, aber nicht auf die angeforderten Informationen zugreifen darf |
| 404 | Nicht gefunden | Tritt auf, wenn die Anfrage einen ungültigen URL-Pfad enthält. Dies sollte niemals vorkommen, wenn der Client den Links &quot;Drilldown&quot;/&quot;Rollout&quot; folgt, die mit 200 Antworten bereitgestellt werden |
| 405 | Methode nicht zulässig | Signalisiert, dass in der Anfrage eine nicht unterstützte Methode verwendet wurde. Obwohl derzeit nur die GET-Methode unterstützt wird, können zukünftige Versionen HEAD oder OPTIONS zulassen |
| 406 | Nicht akzeptabel | Signalisiert, dass ein nicht unterstützter Medientyp vom Client angefordert wurde |
| 500 | Interner Server-Fehler | &quot;Das sollte niemals geschehen&quot; |
| 503 | Dienst nicht verfügbar | Signalisiert einen Fehler innerhalb der Anwendung oder deren Abhängigkeiten |

## Datenformate {#data-formats}

Die Daten sind in den folgenden Formaten verfügbar:

* JSON (Standard)
* XML
* CSV
* HTML (zu Demozwecken)

Die folgenden Strategien für die Inhaltsverhandlung können von Kunden verwendet werden (der Vorrang wird durch die Position in der Liste gegeben - das erste Mal):

1. Eine &quot;Dateierweiterung&quot;, die an das letzte Segment des URL-Pfads angehängt wird, z. B. `/esm/v3/media-company/year/month/day.xml`. Wenn die URL eine Abfragezeichenfolge enthält, muss die Erweiterung vor dem Fragezeichen stehen: `/esm/v3/media-company/year/month/day.csv?mvpd= SomeMVPD`
1. Ein Formatabfragezeichenfolgenparameter, z. B. `/esm/report?format=json`
1. Die standardmäßige HTTP-Accept-Kopfzeile, z. B. `Accept: application/xml`

Sowohl die &quot;Erweiterung&quot;als auch der Abfrageparameter unterstützen die folgenden Werte:

* xml
* json
* csv
* html

Wenn von keiner der Strategien ein Medientyp angegeben wird, erzeugt die API standardmäßig JSON-Inhalte.

## Hypertext Application Language {#hypertext-application-language}

Bei JSON und XML wird die Payload als HAL kodiert, wie hier beschrieben: <http://stateless.co/hal_specification.html>.

Der tatsächliche Bericht (ein verschachteltes Tag/eine verschachtelte Eigenschaft namens &quot;Bericht&quot;) besteht aus der tatsächlichen Liste der Datensätze, die alle ausgewählten/anwendbaren Dimensionen und Metriken mit ihren Werten enthalten, die wie folgt kodiert sind:

### JSON

```JSON
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

### XML

```XML
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report
```

Bei XML- und JSON-Formaten ist die Reihenfolge der Felder (Dimensionen und Metriken) in einem Datensatz nicht angegeben - aber konsistent (die Reihenfolge ist in allen Datensätzen identisch). Clients sollten sich jedoch nicht auf eine bestimmte Reihenfolge der Felder in einem Datensatz verlassen.

Der Ressourcenlink (das &quot;self&quot;-rel in JSON und das &quot;href&quot;-Ressourcenattribut in XML) enthält den aktuellen Pfad und die Abfragezeichenfolge, die für den Inline-Bericht verwendet werden. Die Abfragezeichenfolge zeigt alle impliziten und expliziten Parameter an, sodass die Payload explizit auf das verwendete Zeitintervall, die impliziten Filter (falls vorhanden) usw. verweist. Der Rest der Links innerhalb der Ressource enthält alle verfügbaren Segmente, die verfolgt werden können, um einen Drilldown in den aktuellen Daten durchzuführen. Es wird auch ein Link für die Datenaggregation bereitgestellt, der auf den übergeordneten Pfad verweist (falls vorhanden). Der Wert `href` für die Drilldown-/Rollup-Links enthält nur den URL-Pfad (er enthält nicht die Abfragezeichenfolge, daher muss dieser bei Bedarf vom Client angehängt werden). Beachten Sie, dass nicht alle von der aktuellen Ressource verwendeten (oder impliziten) Abfragezeichenfolgenparameter für &quot;Datenaggregations&quot;- oder &quot;Drilldown&quot;-Links gelten (z. B. gelten die Filter nicht für Unter- oder Super-Ressourcen).

Beispiel (vorausgesetzt, wir haben eine einzelne Metrik namens `clients` und es gibt eine Voraggregation für `year/month/day/...`):

* https://mgmt.auth.adobe.com/esm/v3/year/month.xml

```XML
   <resource href="/esm/v3/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21">
   <links>
   <link rel="roll-up" href="/esm/v3/year"/>
   <link rel="drill-down" href="/esm/v3/year/month/day"/>
   </links>
   <report>
   <record month="6" year="2012" clients="205"/>
   <record month="7" year="2012" clients="466"/>
   </report>
   </resource>
```

* https://mgmt.auth.adobe.com/esm/v3/year/month.json

  ```JSON
      {
        "_links" : {
          "self" : {
            "href" : "/esm/v3/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21"
          },
          "roll-up" : {
            "href" : "/esm/v3/year"
          },
          "drill-down" : {
            "href" : "/esm/v3/year/month/day"
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

### CSV

Im CSV-Datenformat werden keine Links oder anderen Metadaten (mit Ausnahme der Kopfzeile) inline bereitgestellt. Stattdessen werden die Auswahlmetadaten im Dateinamen bereitgestellt, der diesem Muster folgt:

```CSV
    esm__<start-date>_<end-date>_<filter-values,...>.csv
```

Die CSV-Datei enthält eine Kopfzeile und dann die Berichtsdaten als nachfolgende Zeilen. Die Kopfzeile enthält alle Dimensionen, gefolgt von allen Metriken. Die Sortierreihenfolge der Berichtsdaten wird in der Reihenfolge der Dimensionen angezeigt. Wenn Daten nach `D1` und dann nach `D2` sortiert werden, sieht der CSV-Header daher wie folgt aus: `D1, D2, ...metrics...`.

Die Reihenfolge der Felder in der Kopfzeile entspricht der Sortierreihenfolge der Tabellendaten.


Beispiel: https://mgmt.auth.adobe.com/esm/v3/year/month.csv erstellt eine Datei mit dem Namen `report__2012-07-20_2012-08-20_1000.csv` mit folgendem Inhalt:


| Jahr | Monat | Kunden |
| ---- | :---: | ------- |
| 2012 | 6 | 580 |
| 2012 | 7 | 231 |

## Datenfreude {#data-freshness}

Die erfolgreichen HTTP-Antworten enthalten einen `Last-Modified` -Header, der den Zeitpunkt angibt, zu dem der Bericht im Textkörper zuletzt aktualisiert wurde. Das Fehlen einer Kopfzeile vom Typ Letzte Änderung zeigt an, dass die Berichtsdaten in Echtzeit berechnet werden.

Normalerweise werden grobkörnige Daten weniger häufig aktualisiert als fein abgestufte Daten (z. B. Minuswerte oder Stundenwerte), die aktueller als die täglichen Werte sein können, insbesondere für Metriken, die nicht anhand kleinerer Granularitäten berechnet werden können (z. B. eindeutige Werte).

## GZIP-Komprimierung {#gzip-compression}

Adobe empfiehlt dringend, dass Sie die gzip-Unterstützung in Clients aktivieren, die ESM-Berichte abrufen. Dadurch wird die Größe der Antwort erheblich reduziert, was wiederum Ihre Reaktionszeit reduziert. (Das Komprimierungsverhältnis für ESM-Daten liegt im Bereich von 20-30.)

Um die gzip-Komprimierung in Ihrem Client zu aktivieren, legen Sie die Kopfzeile `Accept-Encoding:` wie folgt fest:

* Accept-Encoding: gzip, deflate


<!--
## Related Information {#related-information}

- [ESM Overview](/help/authentication/entitlement-service-monitoring-overview.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Understanding Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
