---
title: Flugvorbereitung
description: Flugvorbereitung
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# Flugvorbereitung {#preflight-authorization}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>

## Übersicht {#overview}

Diese Funktion bietet eine einfache Autorisierungsprüfung für mehrere Ressourcen. Der Zweck dieser einfachen Prüfung besteht darin, die Benutzeroberfläche zu dekorieren (z. B. das Anzeigen des Zugriffsstatus mit Schloss- und Entsperrsymbol). Die Preflight-Autorisierung ist so einfach und effizient wie möglich, sodass ein einzelner API-Aufruf den Autorisierungsstatus für eine Liste von Ressourcen ergibt. Beachten Sie, dass diese Funktion in Bezug auf die Autorisierung einer Ressource nicht autoritär ist.

Ein `getAuthorization(resource)`- oder `checkAuthorization(resource)`-Aufruf MUSS noch erfolgen, bevor die Wiedergabe zugelassen wird.

Die Preflight-Autorisierung bietet auch Unterstützung für einen anderen Anwendungsfall, in dem der Programmierer die Autorisierung für mehrere Ressourcen-IDs anfordern muss, um die Wiedergabe eines Medieninhalts zu ermöglichen. Der Programmierer kann eine erste Preflight-Prüfung der erforderlichen Ressourcen durchführen und kann je nach Antwort vorzeitig fehlschlagen, wenn die Geschäftsbedingungen nicht erfüllt sind.

Eine Liste der MVPDs, die die Preflight-Autorisierung unterstützen, finden Sie auf der Seite [MVPD Preflight-Autorisierung](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#preflight_support_list) .

>[!NOTE]
>
> Bitte beachten Sie, dass die Nutzung dieser Funktion für MVPDs, die keine vollständige Unterstützung für die Preflight-Autorisierung haben, im Voraus mit Adobe und MVPDs vereinbart werden muss. Die Verwendung der PreFlight-Autorisierung auf diesen MVPDs fällt in das „Worst-Case-Szenario“, das [hier](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#intro) beschrieben wird, und kann zu Leistungsproblemen und langsamer Reaktionszeit führen. </br>
> Bitte beachten Sie auch, dass die Nutzung der Preflight-Autorisierung mit **mehr als 5 Ressourcen explizit durch Adobe vereinbart werden muss**.

## AccessEnabler Preflight-API {#AE_pre_api}

AccessEnabler stellt ein API-/Callback-Funktionspaar zur Implementierung der Preflight-Autorisierung bereit. Der API-Aufruf verwendet einen einzelnen Parameter, der aus einer Liste von Ressourcen besteht. Die Rückruffunktion verwendet einen einzelnen Parameter, der die tatsächlichen autorisierten Ressourcen darstellt. Die folgenden Beispiele sind im ActionScript enthalten, aber die Aufrufe sind in allen Varianten des AccessEnabler verfügbar.

### checkPreauthorizedResources(Array:resources):void {#checkPreauthRes}

Rufen Sie diese Funktion auf dem AccessEnabler-Objekt auf, um den Autorisierungsstatus für eine Liste von Ressourcen anzufordern.

Der Parameter resources ist die Liste der Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()`-Aufruf, d. h. sie wird auf einen zwischen dem Programmierer und dem MVPD festgelegten Wert oder auf ein RSS-Medienfragment festgelegt. Beachten Sie, dass die Adobe Pass-Authentifizierung Ressourcen in keiner Weise verwaltet, mit Ausnahme einer dünnen Vermittlungsschicht, die Ressourcenformate je nachdem, was MVPD tatsächlich unterstützt, transformieren kann.

### preauthorizedResources(Array:authorizedResources) {#preauthRes}

Dies ist eine Rückruffunktion, die in der Applikation auf der oberen Ebene des Programmierers implementiert werden muss. Der AccessEnabler ruft diese Funktion nach der Berechnung der Liste der autorisierten Ressourcen auf.


### JS-Beispiel

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## Implementierungsinformationen {#details}

- [PreFlight mit Kanal-ID](#preflight_using_channelID)
- [POST/Vorabautorisierung](#post)
- [Speicherung](#storage)

Der API-Aufruf versucht, eine zwischengespeicherte Liste von autorisierten Ressourcen für den aktuellen Benutzer im lokalen Speicher des Clients zu finden. Wenn keine zwischengespeicherte Liste vorhanden ist, wird ein HTTPS-Aufruf an die AdobePass-Server durchgeführt, um die Liste abzurufen.

Der Caching-Mechanismus verbessert die Leistungszeiten bei nachfolgenden Aufrufen, indem der Netzwerkaufruf vollständig übersprungen wird. Außerdem kann die zwischengespeicherte Liste im Rahmen des Authentifizierungsprozesses vorab ausgefüllt werden.  (Informationen zum Einrichten dieses Szenarios finden Sie [Preflight-Autorisierungsintegration](/help/authentication/integration-guide-mvpds/authz-usecase.md#preflight_authz_int) im Abschnitt „Autorisierung“ des MVPD-Integrationshandbuchs).

Darüber hinaus kann die zwischengespeicherte Liste von Ressourcen potenziell zur Optimierung des Autorisierungsflusses verwendet werden, in dem Sinne, dass `checkAuthorization()`, wenn eine zwischengespeicherte Liste von Ressourcen vorhanden ist, diese vor einem Netzwerkaufruf überprüfen können. Wenn die Ressource nicht in der Liste der vorab autorisierten Ressourcen enthalten ist, kann die Prüfung fehlschlagen, ohne dass die Adobe Pass-Authentifizierungsserver aufgerufen werden müssen.


### PreFlight mit Kanal-ID {#preflight_using_channelID}

Ab Version 2.4.1 der Adobe Pass-Authentifizierung funktioniert der Preflight-Fluss wie folgt:

1. Während der Authentifizierung liest die Adobe Pass-Authentifizierung das `channelIID` aus der SAML-Antwort von MVPD und verwendet diesen Wert, um das `authorizedResources` im Authentifizierungstoken festzulegen.
1. Innerhalb der `checkPreauthorizedResources()`-API-Funktion wird bei der Adobe Pass-Authentifizierung überprüft, ob das `authorizedResources` festgelegt ist.
1. Wenn das `authorizedResources` festgelegt ist, liest die Adobe Pass-Authentifizierung diesen Wert und überschneidet die Ressourcenliste aus dem `authorizedResources` Element mit der Liste der Ressourcen, die von `checkPreauthorizedResources()` Parameter empfangen wurden.  Das Ergebnis dieser Schnittmenge ist die endgültige Liste der vorab autorisierten Ressourcen.
1. Wenn das `authorizedResources` nicht festgelegt ist, führen Sie den zuvor implementierten Fluss aus, in dem die Liste der von `checkPreauthorizedResources()` Parameter empfangenen Ressourcen an das PreAuthorizationServlet übergeben wird. Dieses Servlet führt die Autorisierungsaufrufe an die MVPD-Endpunkte aus und gibt die Liste der vorab autorisierten Ressourcen zurück.

### Beispiel für Preflight mit ChannelID

Das folgende Beispiel zeigt ein Beispiel für eine Kanalaufstellung. Beachten Sie, dass der Name des Attributs, das zum Senden der Kanalliste verwendet wird, von MVPD zu unterschiedlich sein kann:

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


Das `authorizedResources` aus dem Authentifizierungselement wird wie folgt angezeigt:

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

Der Programmierer führt den `checkPreauthorizedResources()`-API-Aufruf aus und übergibt die folgende Liste von Parametern:</span>

- „MSNBC“
- „FBN“
- „TruTV“
- „FBC-FOX“

Die aktuelle Preflight-Implementierung führt die Schnittmenge mit der Liste der Ressourcen aus dem `authorizedResources`-Element aus und gibt diese Liste zurück:

- „MSNBC“
- „FBN“
- „TruTV“



**Hinweis:** Beachten Sie, dass bei der Schnittmenge nicht zwischen Groß- und Kleinschreibung unterschieden wird.



### POST/Vorabautorisierung {#post}

Dieser Aufruf wird automatisch vom AccessEnabler ausgeführt, wenn keine zwischengespeicherte, autorisierte Ressourcenliste vorhanden ist.


#### Anfrage {#req}

| Parameter | Typ | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| `authentication_token` | Zeichenfolge | JA | Das Authentifizierungs-Token. |
| `resource_id` | Zeichenfolge | JA | Eine einzelne Ressource. Dies kann mehrmals angegeben werden, und zwar einmal für jedes Element des Ressourcen-Arrays, das im `checkPreauthorizedResources()`-API-Aufruf bereitgestellt wird. |


**Hinweis:** Die maximale Anzahl der angeforderten Ressourcen kann konfiguriert werden.
**Der maximale Standardwert ist 5.**


#### Antwort {#resp}

Die Antwort, die das vorautorisierte Servlet zurücksendet, hat das folgende Format:

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### Speicherung {#storage}

Eine Liste der vorab autorisierten Ressourcen, die der AccessEnabler vom Service Provider erhält. Diese Liste von Ressourcen:

- Wird zusammen mit dem AuthN- und dem AuthZ-Token gespeichert
- Ist gültig, solange sich der Benutzer auf derselben Website befindet, oder bis
Authentifizierungs-Token läuft ab
- Wird jedes Mal neu abgerufen, wenn der Benutzer auf einer neuen Adobe Pass landet
Integrierte Authentifizierungs-Website

Jeder Eintrag enthält die Ressourcen-ID, für die der Benutzer vorab autorisiert ist.

Beispiel:


| Ressourcen-ID | Zugelassen |
| ----------- | ---------- |
| CNN | wahr |
| TNT | falsch |
| … | … |


Diese Liste wird als „Vorautorisierungs-Cache“ bezeichnet.

#### Fluss {#flow}

1. Programmierer-App / Site führt einen `checkPreauthorizedResources(resourceList)` Aufruf aus.
1. AccessEnabler überprüft das Authentifizierungstoken für autorisierte Ressourcen:
   1. Wenn das Authentifizierungs-Token autorisierte Ressourcen enthält - diese Liste ist autorisierend und es sollte kein Aufruf erfolgen, um diese Informationen zu erhalten. Ressourcen aus resourceList werden in der Liste der autorisierten Ressourcen im Authentifizierungstoken durchsucht und nur die gefundenen Ressourcen werden vom `preauthorizedResources()`-Callback zurückgegeben.
   1. Wenn das Authentifizierungs-Token KEINE autorisierten Ressourcen enthält, wird `resourceList` mit der Liste der Ressourcen im Vorautorisierungs-Cache verglichen.
      1. Wenn die Liste dieselben Ressourcen enthält - bedeutet dies, dass bereits ein Aufruf an den Server erfolgt ist und die Antwort bereits im Vorautorisierungs-Cache ist. Nur autorisierte Ressourcen werden vom `preauthorizedResources()`-Callback zurückgegeben.
      1. Wenn die Liste NICHT dieselben Ressourcen enthält, muss der Client den Server aufrufen, um den Autorisierungsstatus für die Ressourcen in resourceList abzurufen. Die Antwort wird abgerufen und im Vorautorisierungs-Cache gespeichert, wobei die alten Ressourcen vollständig ersetzt werden. Nur autorisierte Ressourcen werden vom `preauthorizedResources()`-Callback zurückgegeben.


#### Listenabruf {#listRetrieve}

Bei jedem Aufruf eines `checkPreauthorizedResources()` wird die Liste der Ressourcen, die auf Autorisierung überprüft werden sollen, mit dem Vorautorisierungscache verglichen. Wenn die Liste denselben Satz von Ressourcen enthält, wird kein Aufruf an den Dienstleister durchgeführt, da sich alle Ressourcen, die für das Auslösen des `preauthorizedResources()`-Callbacks benötigt werden, bereits im Cache befinden.


#### logout() {#logout}

Der Vorautorisierungscache wird beim Abmelden geleert.


## Abhängigkeiten {#depends}

Die Leistung der Preflight-API hängt von bestimmten MVPD-Implementierungen ab.  Implementierungsoptionen finden Sie unter [Preflight-Autorisierungsintegration](/help/authentication/integration-guide-mvpds/authz-usecase.md#preflight_authz_int) im Abschnitt „Autorisierung“ des MVPD-Integrationshandbuchs.


## Sicherheit {#security}

Die Client-APIs stehen allen Programmierern zur Verfügung.

Die Implementierung verwendet HTTPS als Transport, aber für einen leichteren Aufruf werden keine zusätzlichen Sicherheitsmaßnahmen angewendet (keine Signierung, keine FAXs).

**Achtung:** Verwenden Sie diese API NICHT mit Autorität, um zu bestimmen, ob einem Benutzer Zugriff auf eine geschützte Ressource gewährt werden soll. Der Zweck dieser API ist die Dekoration der Benutzeroberfläche und/oder die Vorbereitung geschäftlicher Entscheidungen. Die `getAuthorization()`- und `checkAuthorization()`-Aufrufe sollten immer erfolgen, bevor die Wiedergabe zugelassen wird.


## Kompatibilität {#compat}

Diese Funktion wird in allen Varianten von AccessEnabler unterstützt: AS, JS, AIR, iOS, Android, Xbox (im AuthN-Fluss im 2. Bildschirm).

Die PreFlight-Autorisierung unterstützt keine Pre-Authorization von Ressourcen, die CDATA-Abschnitte enthalten. Der Schwerpunkt des aktuellen Preflight-Systems liegt auf der Unterstützung der Filterung auf Kanalebene. Ressourcen mit CDATA-Abschnitten sind wahrscheinlich Ressourcen auf Asset-Ebene. Preflight unterstützt einfache `mrss` Ressourcen für die Vorabautorisierung auf Kanalebene, sofern sie keine CDATA enthalten.

## Integration mit anderen Funktionen {#integ_w_other_features}

Eine mögliche Optimierung des Autorisierungsflusses kann auftreten, um schnell zu fehlschlagen, wenn die Ressource nicht in der Liste der vorab autorisierten Ressourcen enthalten ist.

Bei dieser Optimierung ist der Preflight-Endpunkt des Servers mit dem Degradation-Mechanismus integriert.

Wenn für MVPD und Requestor eine Regel vom Typ „AuthN für alle“ vorhanden ist, spiegelt der Preflight-Endpunkt einfach alle in der Anfrage empfangenen Ressourcen wider.

Dasselbe gilt, wenn „AuthZ All“ für mindestens eine der in der Anfrage vorhandenen Ressourcen aktiviert ist.

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
