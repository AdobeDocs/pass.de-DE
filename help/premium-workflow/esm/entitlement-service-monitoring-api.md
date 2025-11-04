---
title: Berechtigungs-Service-Überwachungs-API
description: Berechtigungs-Service-Überwachungs-API
exl-id: a9572372-14a6-4caa-9ab6-4a6baababaa1
source-git-commit: 2afe9ea2a814817757f1ab28484a84466da68d62
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 0%

---

# Berechtigungs-Service-Überwachungs-API {#entitlement-service-monitoring-api}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Stellen Sie vor der Verwendung der Degradation API sicher, dass die folgenden Voraussetzungen erfüllt sind:
>
> * Rufen Sie die Client-Anmeldeinformationen ab, wie in der API[Dokumentation zum Abrufen von Client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)Anmeldeinformationen beschrieben.
> * Rufen Sie das Zugriffstoken ab, wie in der API[Dokumentation zum Abrufen des Zugriffstokens &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Weitere Informationen zum Erstellen einer registrierten [&#x200B; und zum Herunterladen der Software-Anweisung finden &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) in der Dokumentation Übersicht zur Dynamic Client-Registrierung .

## API-Übersicht {#api-overview}

Die Überwachung von Berechtigungen für Services (Entitlement Service Monitoring, ESM) wird als WOLAP-Projekt (Web-basierte [Online-](https://en.wikipedia.org/wiki/Online_analytical_processing){target=_blank}) implementiert. ESM ist eine generische Business-Reporting-Web-API, die von einem Data Warehouse unterstützt wird. Sie dient als HTTP-Abfragesprache, mit der typische OLAP-Vorgänge RESTful ausgeführt werden können.

>[!NOTE]
>
>Die ESM-API ist nicht allgemein verfügbar. Bei Fragen zur Verfügbarkeit wenden Sie sich an den Adobe-Support.

Die ESM-API bietet eine hierarchische Ansicht der zugrunde liegenden OLAP-Cubes. Jede Ressource ([Dimension](#esm_dimensions) in der Dimensionshierarchie, zugeordnet als URL-Pfadsegment) generiert Berichte mit (aggregierten) [Metriken](#esm_metrics) für die aktuelle Auswahl. Jede Ressource verweist auf ihre übergeordnete Ressource (für die Aufschlüsselung) und ihre Unterressourcen (für die Aufschlüsselung). Slicing und Dicing werden über Abfragezeichenfolgen-Parameter erreicht, die Dimensionen an bestimmte Werte oder Bereiche anheften.

Die REST-API stellt die verfügbaren Daten innerhalb eines in der Anfrage angegebenen Zeitintervalls (d. h. zurück auf die Standardwerte, wenn keine Werte angegeben wurden) bereit, und zwar entsprechend dem Dimensionspfad, den bereitgestellten Filtern und den ausgewählten Metriken. Der Zeitbereich wird nicht auf Berichte angewendet, die keine Zeitdimensionen enthalten (Jahr, Monat, Tag, Stunde, Minute, Sekunde).

Der Endpunkt-URL-Stammpfad gibt die gesamten aggregierten Metriken innerhalb eines einzigen Datensatzes zusammen mit den Links zu den verfügbaren Drilldown-Optionen zurück. Die API-Version wird als nachfolgendes Segment des Endpunkt-URI-Pfads zugeordnet. `https://mgmt.auth.adobe.com/esm/v3` bedeutet beispielsweise, dass die Clients auf WOLAP Version 3 zugreifen.

Die verfügbaren URL-Pfade sind über in der Antwort enthaltene Links auffindbar. Gültige URL-Pfade werden gespeichert, um einen Pfad innerhalb der zugrunde liegenden Drilldown-Struktur zuzuordnen, der (vor-)aggregierte Metriken enthält. Ein Pfad in der `/dimension1/dimension2/dimension3` spiegelt eine Voraggregation dieser drei Dimensionen wider (das Äquivalent eines SQL-`clause GROUP` NACH `dimension1`, `dimension2`, `dimension3`). Wenn eine solche Voraggregation nicht vorhanden ist und das System sie nicht spontan berechnen kann, gibt die API die Antwort „404 Nicht gefunden“ zurück.

## Drilldown-Baum {#drill-down-tree}

Die folgenden Drill-down-Bäume veranschaulichen die in ESM 3.0 verfügbaren Dimensionen (Ressourcen) für [Programmierer](#progr-dimensions) und [MVPD](#mvpd-dimensions).


### Für Programmierer verfügbare Dimensionen {#progr-dimensions}

#### Tag

![](/help/authentication/assets/esm-progr-dimensions-day.png)

#### Stunde

![](/help/authentication/assets/esm-progr-dimensions-hour.png)

#### Minute

![](/help/authentication/assets/esm-progr-dimensions-minute.png)

### Für MVPDs verfügbare Dimensionen {#mvpd-dimensions}

![](/help/authentication/assets/esm-mvpd-dimensions.png)

Eine GET an den `https://mgmt.auth.adobe.com/esm/v3`-API-Endpunkt gibt eine Darstellung zurück, die Folgendes enthält:

* Links zu den verfügbaren Stamm-Drilldown-Pfaden:

   * `<link rel="drill-down" href="/v3/dimensionA"/>`

   * `<link rel="drill-down" href="/v3/dimensionB"/>`

* Eine Zusammenfassung (aggregierte Werte) für alle Metriken (standardmäßig )
Intervall, da keine Abfragezeichenfolgen-Parameter angegeben werden (siehe unten).


Folgen eines Drilldown-Pfads (Schritt für Schritt):
`/dimensionA/year/month/day/dimensionX` ruft Folgendes ab
Antwort:

* Links zu den Aufschlüsselungsoptionen &quot;`dimensionY`&quot; und &quot;`dimensionZ`&quot;

* Ein Bericht mit täglichen Aggregaten für jeden `dimensionX`


### Filter

Mit Ausnahme der Datums-/Uhrzeitdimensionen können alle für die aktuelle Projektion verfügbaren Dimensionen (Dimensionspfad) gefiltert werden, indem ihr Name als Abfragezeichenfolgenparameter verwendet wird.

Die folgenden Filteroptionen sind verfügbar:

* **Gleich**-Filter werden bereitgestellt, indem der Dimensionsname in der Abfragezeichenfolge auf einen bestimmten Wert festgelegt wird.

* **IN** Filter können angegeben werden, indem derselbe Dimensionsnamensparameter mehrmals mit unterschiedlichen Werten hinzugefügt wird: dimension=value1\&amp;dimension=value2

* **Ungleich** Filter müssen &#39;\!&#39; verwenden. nach dem Namen der Dimension erscheint, was zu &#39;\!=&#39; „Operator“: dimension\!=Wert

* **NOT IN** Filter erfordern &#39;\!=&#39; Operator muss mehrmals verwendet werden, und zwar einmal für jeden Wert im Satz: dimension\!=Wert1\&amp;Dimension\!=Wert2&amp; …

Es gibt auch eine besondere Verwendung für die Dimensionsnamen in der Abfragezeichenfolge: Wenn der Dimensionsname als Abfragezeichenfolgenparameter ohne Wert verwendet wird, wird die API angewiesen, eine Projektion zurückzugeben, die diese Dimension im Bericht enthält.

### Beispiele für ESM-Abfragen

| *URL* | *SQL-Äquivalent* |
|---|---|
| /dimension1/dimension2/dimension3?dimension1=value1 | SELECT * from projection WHERE dimension1 = &#39;value1&#39; </br> GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1=value1&amp;dimension1=value2 | SELECT * from projection WHERE dimension1 IN (&#39;value1&#39;, &#39;value2&#39;) </br> GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=Wert1 | SELECT * from projection WHERE dimension1 &lt;> &#39;value1&#39; | </br> GRUPPIEREN NACH Dimension1, Dimension2, Dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=Wert1&amp;Dimension2!=Wert2 | SELECT * from projection WHERE dimension1 NOT IN (&#39;value1&#39;, &#39;value2&#39;) | </br> GRUPPIEREN NACH Dimension1, Dimension2, Dimension3 |
| Angenommen, es gibt keinen direkten Pfad: /dimension1/dimension3 </br>, aber es gibt einen Pfad: /dimension1/dimension2/dimension3 </br> </br> /dimension1?dimension3 | SELECT * from projection GROUP BY dimension1, dimension3 |

>[!NOTE]
>
>Keine dieser Filtertechniken funktioniert für `date/time` Dimensionen. Die einzige Möglichkeit, `date/time` Dimensionen zu filtern, besteht darin, die `start` und `end` Abfragezeichenfolgenparameter (siehe unten) auf die erforderlichen Werte festzulegen.

Die folgenden Abfragezeichenfolgenparameter haben reservierte Bedeutungen für die API (und können daher nicht als Dimensionsnamen verwendet werden, da ansonsten keine Filterung für eine solche Dimension möglich wäre).

### ESM API reservierte Abfragezeichenfolgen-Parameter

| Parameter | optional | Beschreibung | Standardwert | Beispiel |
| --- | ---- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ---- | --- |
| access_token | Ja | Das DCR-Token kann als standardmäßiges Autorisierungs-Bearer-Token übergeben werden. | Keine | access_token=XXXXXX |
| Dimensionsname | Ja | Beliebiger Dimensionsname - entweder im aktuellen URL-Pfad oder in einem gültigen Unterpfad enthalten. Der Wert wird als Filter „ist gleich“ behandelt. Wenn kein Wert angegeben wird, erzwingt dies die Aufnahme der angegebenen Dimension in die Ausgabe, selbst wenn sie nicht enthalten ist oder an den aktuellen Pfad angrenzt | Keine | someDimension=someValue&amp;someOtherDimension |
| Ende | Ja | Endzeit für den Bericht in Millionen | Aktuelle Zeit des Servers | Ende=30.07.2024 |
| Format | Ja | Wird für die Inhaltsaushandlung verwendet (mit demselben Effekt, aber niedrigerer Priorität als der Pfad „Erweiterung“ - siehe unten). | Keine: die Inhaltsaushandlung wird die anderen Strategien ausprobieren | format=json |
| Grenze | Ja | Maximale Anzahl an zurückzugebenden Zeilen | Vom Server im Self-Link gemeldeter Standardwert, wenn in der Anfrage kein Limit angegeben ist | Limit=1500 |
| Metriken | Ja | Kommagetrennte Liste der zurückzugebenden Metriknamen. Diese sollte sowohl zum Filtern einer Teilmenge der verfügbaren Metriken (um die Payload-Größe zu reduzieren) als auch zum Erzwingen verwendet werden, dass die API eine Projektion zurückgibt, die die angeforderten Metriken enthält (anstelle der standardmäßigen optimalen Projektion). | Alle für die aktuelle Projektion verfügbaren Metriken werden zurückgegeben, falls dieser Parameter nicht angegeben wird. | Metriken=m1,m2 |
| anfangen | Ja | Startzeit für den Bericht als ISO8601; der Server füllt den verbleibenden Teil aus, wenn nur ein Präfix angegeben wird: z. B. führt start=2024 zu start=2024-01-01:00:00:00 | Wird vom Server im Self-Link gemeldet. Der Server versucht, basierend auf der ausgewählten Zeitgranularität angemessene Standardwerte bereitzustellen | start=2024-07-15 |

Die einzige derzeit verfügbare HTTP-Methode ist GET.

## ESM-API-Status-Codes {#esm-api-status-codes}

| Statuscode | Reason | Beschreibung |
|---|---|---|
| 200 | OK | Die Antwort enthält Links zum „Rollout“ und „Drilldown“ (falls zutreffend). Der Bericht wird als Attribut der Ressource gerendert: ein verschachteltes Element bzw. eine verschachtelte Eigenschaft „Bericht“. |
| 400 | Fehlerhafte Anfrage | Der Antworttext enthält eine Textnachricht, die erklärt, was mit der Anfrage nicht stimmt. </br> </br> Der Status „Fehlerhafte Anfrage“ des Typs „400“ wird im Antworttext von einem erläuternden Text (Medientyp „Nur Text„) begleitet, der nützliche Informationen zum Client-Fehler bereitstellt. Neben den trivialen Szenarien wie ungültigen Datumsformaten oder Filtern, die auf nicht vorhandene Dimensionen angewendet werden, weigert sich das System auch, auf Abfragen zu reagieren, für die eine enorme Datenmenge zurückgegeben oder spontan aggregiert werden muss. |
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

1. Eine „Dateierweiterung“, die an das letzte Segment des URL-Pfads angehängt wird: z. B. `/esm/v3/media-company/year/month/day.xml`. Wenn die URL eine Abfragezeichenfolge enthält, muss die Erweiterung vor dem Fragezeichen stehen: `/esm/v3/media-company/year/month/day.csv?mvpd= SomeMVPD`
1. Ein Format-Abfragezeichenfolgenparameter: z. B. `/esm/report?format=json`
1. Der Standard-HTTP-Accept-Header: z. B. `Accept: application/xml`

Sowohl die Erweiterung als auch der Abfrageparameter unterstützen die folgenden Werte:

* XML
* JSON
* CSV
* HTML

Wenn in einer der Strategien kein Medientyp angegeben ist, erstellt die API standardmäßig JSON-Inhalte.

## Hypertext-Anwendungssprache {#hypertext-application-language}

Für JSON und XML wird die Payload als HAL codiert, wie hier beschrieben: <http://stateless.co/hal_specification.html>.

Der eigentliche Bericht (ein verschachteltes Tag/eine verschachtelte Eigenschaft namens „Bericht„) besteht aus der tatsächlichen Liste von Datensätzen, die alle ausgewählten/anwendbaren Dimensionen und Metriken mit ihren Werten enthalten, und zwar wie folgt:

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

Bei XML- und JSON-Formaten ist die Reihenfolge der Felder (Dimensionen und Metriken) in einem Datensatz nicht angegeben, aber konsistent (die Reihenfolge ist in allen Datensätzen gleich). Kunden sollten sich jedoch nicht auf eine bestimmte Reihenfolge der Felder innerhalb eines Datensatzes verlassen.

Der Ressourcen-Link (das „Selbst“-rel in JSON und das „href“-Ressourcenattribut in XML) enthält den aktuellen Pfad und die Abfragezeichenfolge, die für den Inline-Bericht verwendet wird. Die Abfragezeichenfolge enthüllt alle impliziten und expliziten Parameter, sodass die Payload explizit auf das verwendete Zeitintervall, die impliziten Filter (falls vorhanden) usw. hinweist. Der Rest der Links innerhalb der Ressource enthält alle verfügbaren Segmente, denen Sie folgen können, um eine Aufschlüsselung der aktuellen Daten durchzuführen. Ein Link für die Datenaggregation wird ebenfalls bereitgestellt und verweist auf den übergeordneten Pfad (falls vorhanden). Der `href` für die Drill-down-/Roll-up-Links enthält nur den URL-Pfad (er enthält nicht die Abfragezeichenfolge, daher muss dieser bei Bedarf vom Client angehängt werden). Beachten Sie, dass nicht alle der von der aktuellen Ressource verwendeten (oder implizierten) Abfragezeichenfolgenparameter für „Hochrechnungs“- oder „Drilldown“-Links gelten (die Filter können beispielsweise nicht auf Unter- oder Überressourcen angewendet werden).

Beispiel (vorausgesetzt, wir haben eine einzelne Metrik namens `clients` und es gibt eine Voraggregation für `year/month/day/...`):

* https://mgmt.auth.adobe.com/esm/v3/year/month.xml

```XML
   <resource href="/esm/v3/year/month?start=2024-07-20T00:00:00&end=2024-08-20T14:35:21">
   <links>
   <link rel="roll-up" href="/esm/v3/year"/>
   <link rel="drill-down" href="/esm/v3/year/month/day"/>
   </links>
   <report>
   <record month="6" year="2024" clients="205"/>
   <record month="7" year="2024" clients="466"/>
   </report>
   </resource>
```

* https://mgmt.auth.adobe.com/esm/v3/year/month.json

  ```JSON
      {
        "_links" : {
          "self" : {
            "href" : "/esm/v3/year/month?start=2024-07-20T00:00:00&end=2024-08-20T14:35:21"
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
          "year" : "2024",
          "clients" : "205"
        }, {
          "month" : "7",
          "year" : "2024",
          "clients" : "466"
        } ]
      }
  ```

### CSV

Im CSV-Datenformat werden keine Links oder anderen Metadaten (mit Ausnahme der Kopfzeile) inline bereitgestellt. Stattdessen werden die Auswahl-Metadaten im Dateinamen bereitgestellt, der diesem Muster folgt:

```CSV
    esm__<start-date>_<end-date>_<filter-values,...>.csv
```

Die CSV enthält eine Kopfzeile und die Berichtsdaten als nachfolgende Zeilen. Die Kopfzeile enthält alle Dimensionen gefolgt von allen Metriken. Die Sortierreihenfolge der Berichtsdaten wird in der Reihenfolge der Dimensionen angezeigt. Wenn die Daten also nach `D1` und dann nach `D2` sortiert werden, sieht die CSV-Kopfzeile wie folgt aus: `D1, D2, ...metrics...`.

Die Reihenfolge der Felder in der Kopfzeile entspricht der Sortierreihenfolge der Tabellendaten.


Beispiel: https://mgmt.auth.adobe.com/esm/v3/year/month.csv erstellt eine Datei mit dem Namen `report__2024-07-20_2024-08-20_1000.csv` mit folgendem Inhalt:


| Jahr | Monat | Clients |
| ---- | :---: | ------- |
| 2024 | 6 | 580 |
| 2024 | 7 | 231 |

## Aktualität der Daten {#data-freshness}

Die erfolgreichen HTTP-Antworten enthalten eine `Last-Modified`-Kopfzeile, die den Zeitpunkt angibt, zu dem der Bericht im Hauptteil zuletzt aktualisiert wurde. Das Fehlen einer Kopfzeile „Zuletzt geändert“ zeigt an, dass die Berichtsdaten in Echtzeit berechnet werden.

Normalerweise werden grobkörnige Daten seltener aktualisiert als feinkörnige Daten (z. B. könnten minutengenaue Werte oder stündliche Werte aktueller sein als die täglichen Werte, insbesondere bei Metriken, die nicht auf Grundlage kleinerer Granularitäten berechnet werden können, z. B. eindeutige Zahlen).

## GZIP-Komprimierung {#gzip-compression}

Adobe empfiehlt dringend, die gzip-Unterstützung in Clients zu aktivieren, die ESM-Berichte abrufen. Dadurch wird die Größe der Antwort erheblich reduziert, was wiederum die Reaktionszeit verkürzt. (Das Komprimierungsverhältnis für ESM-Daten liegt im Bereich von 20-30.)

Um die Gzip-Komprimierung in Ihrem Client zu aktivieren, legen Sie die `Accept-Encoding:`-Kopfzeile wie folgt fest:

* Accept-Encoding: gzip, deflate
