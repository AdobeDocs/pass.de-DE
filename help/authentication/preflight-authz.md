---
title: Vorabgenehmigung
description: Vorabgenehmigung
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 0%

---

# Vorabgenehmigung {#preflight-authorization}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Übersicht {#overview}

Diese Funktion bietet eine einfache Autorisierungsprüfung für mehrere Ressourcen. Diese einfache Prüfung dient dazu, die Benutzeroberfläche zu dekorieren (z. B. um den Zugriffsstatus mit den Symbolen &quot;Sperren&quot;und &quot;Entsperren&quot;anzugeben). Die Vorabgenehmigung ist so einfach und effizient wie möglich, sodass ein einzelner API-Aufruf den Autorisierungsstatus für eine Liste von Ressourcen liefert. Beachten Sie, dass diese Funktion für die Autorisierung einer Ressource nicht maßgeblich ist.

A `getAuthorization(resource)` oder `checkAuthorization(resource)` -Aufruf MUSS noch durchgeführt werden, bevor die Wiedergabe zugelassen wird.

Die Preflight-Autorisierung bietet außerdem Unterstützung für einen anderen Anwendungsfall, bei dem der Programmierer eine Autorisierung für mehrere Ressourcen-IDs anfordern muss, um die Wiedergabe eines Elements von Medieninhalten zu ermöglichen. Der Programmierer kann eine erste Preflight-Prüfung der erforderlichen Ressourcen durchführen und kann je nach Antwort früh scheitern, wenn die Geschäftsbedingungen nicht erfüllt sind.

Eine Liste der MVPDs, die die Vorabgenehmigung für den Flug unterstützen, finden Sie im [MVPD Preflight-Autorisierung](/help/authentication/mvpd-preflight-authz.md#preflight_support_list) Seite.

>[!NOTE]
>
> Bitte beachten Sie, dass die Nutzung dieser Funktion für MVPDs, die nicht über volle Unterstützung für die Preflight-Autorisierung verfügen, im Voraus mit Adobe &amp; MVPDs vereinbart werden muss. Die Verwendung der Vorabgenehmigung für diese MVPDs wird in das so genannte &quot;Worst Case&quot;-Szenario fallen [here](/help/authentication/mvpd-preflight-authz.md#intro) und kann zu Leistungsproblemen und einer langsamen Reaktionszeit führen. </br>
> Beachten Sie außerdem, dass die Verwendung der Vorabgenehmigung mit **mehr als 5 Ressourcen müssen explizit durch Adobe genehmigt werden**.

## AccessEnabler Preflight-API {#AE_pre_api}

AccessEnabler stellt ein API-/Callback-Funktionspaar zur Implementierung der Preflight-Autorisierung bereit. Der API-Aufruf akzeptiert einen einzigen Parameter, der aus einer Liste von Ressourcen besteht. Die Callback-Funktion akzeptiert einen einzigen Parameter, der die tatsächlichen autorisierten Ressourcen darstellt. Die folgenden Beispiele sind in ActionScript enthalten, aber die Aufrufe sind in allen Varianten von AccessEnabler verfügbar.

### checkPreauthorizedResources(Array:resources):void {#checkPreauthRes}

Rufen Sie diese Funktion im AccessEnabler -Objekt auf, um den Autorisierungsstatus für eine Liste von Ressourcen anzufordern.

Der Ressourcenparameter ist die Liste der Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt den gleichen Einschränkungen wie die Ressourcen-ID in der `getAuthorization()` -Aufruf, d. h., es wird zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment ein Wert vereinbart. Beachten Sie, dass die Adobe Pass-Authentifizierung keine Ressourcen in irgendeiner Weise verwaltet, mit Ausnahme einer dünnen Mediationsschicht, die Ressourcenformate je nach Unterstützung durch den MVPD transformieren kann.

### preauthorizedResources(Array:authorizedResources) {#preauthRes}

Dies ist eine Rückruffunktion, die in der oberen Ebene des Programmierers implementiert werden muss. Der AccessEnabler ruft diese Funktion nach der Berechnung der Liste der autorisierten Ressourcen auf.


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

- [Preflight mit ChannelID](#preflight_using_channelID)
- [POST/Vorabautorisierung](#post)
- [Speicherung](#storage)

Der API-Aufruf versucht, eine zwischengespeicherte Liste der autorisierten Ressourcen für den aktuellen Benutzer im lokalen Speicher des Clients zu finden. Wenn keine zwischengespeicherte Liste vorhanden ist, wird ein HTTPS-Aufruf an die AdobePass-Server gesendet, um die Liste abzurufen.

Der Caching-Mechanismus verbessert die Leistungszeiten bei nachfolgenden Aufrufen, indem der Netzwerkaufruf vollständig übersprungen wird. Außerdem kann die zwischengespeicherte Liste im Rahmen des Authentifizierungsprozesses vorab ausgefüllt werden.  (Informationen zum Einrichten dieses Szenarios finden Sie unter [Integration der Preflight-Autorisierung](/help/authentication/authz-usecase.md#preflight_authz_int) im Abschnitt &quot;Autorisierung&quot;des MVPD-Integrationsleitfadens).

Darüber hinaus kann die zwischengespeicherte Liste von Ressourcen potenziell zur Optimierung des Autorisierungsflusses verwendet werden, sofern eine zwischengespeicherte Liste von Ressourcen vorhanden ist. `checkAuthorization()` kann es vor einem Netzwerkaufruf überprüfen. Wenn die Ressource nicht in der Liste der vorab autorisierten Ressourcen enthalten ist, kann die Prüfung fehlschlagen, ohne die Adobe Pass-Authentifizierungsserver aufrufen zu müssen.


### Preflight mit ChannelID {#preflight_using_channelID}

Ab der Adobe Pass-Authentifizierungsversion 2.4.1 funktioniert der Preflight-Fluss wie folgt:

1. Während der Authentifizierung liest die Adobe Pass-Authentifizierung die `channelIID` -Element aus der SAML-Antwort des MVPD und setzt mithilfe dieses Werts die `authorizedResources` -Element im Authentifizierungstoken.
1. Innerhalb des `checkPreauthorizedResources()` API-Funktion, Adobe Pass-Authentifizierung überprüft, ob die `authorizedResources` -Element festgelegt ist.
1. Wenn die Variable `authorizedResources` -Element festgelegt ist, liest die Adobe Pass-Authentifizierung diesen Wert und führt eine Schnittmenge zwischen der Ressourcenliste aus der `authorizedResources` Element und Liste der von `checkPreauthorizedResources()` -Parameter.  Das Ergebnis dieser Schnittmenge ist die endgültige Liste der vorab autorisierten Ressourcen.
1. Wenn die Variable `authorizedResources` nicht festgelegt ist, führen Sie den zuvor implementierten Fluss aus, in dem die Liste der Ressourcen, die von empfangen wurden `checkPreauthorizedResources()` wird an das PreAuthorizationServlet übergeben. Dieses Servlet führt die Autorisierungsaufrufe an die MVPD-Endpunkte aus und gibt die Liste der vorab autorisierten Ressourcen zurück.

### Beispiel für Preflight mit ChannelID

Das folgende Beispiel zeigt ein Beispiel für eine Kanalverbindung. Beachten Sie, dass der Name des Attributs, das zum Senden der Kanalliste verwendet wird, von einem MVPD zum anderen unterschiedlich sein kann:

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


Die `authorizedResources` -Element des Authentifizierungselements wie folgt angezeigt:

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

Der Programmierer führt die `checkPreauthorizedResources()` API-Aufruf, der die folgende Parameterliste übergibt:</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;
- &quot;fbc-fox&quot;

Die aktuelle Preflight-Implementierung führt die Schnittmenge mit der Liste der Ressourcen aus der `authorizedResources` -Element und gibt diese Liste zurück:

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;



**Hinweis:** Beachten Sie bitte, dass bei der Schnittmenge nicht zwischen Groß- und Kleinschreibung unterschieden wird.



### POST/Vorabautorisierung {#post}

Dieser Aufruf wird automatisch vom AccessEnabler durchgeführt, wenn keine zwischengespeicherte, autorisierte Ressourcen-Liste existiert.


#### Anfrage {#req}

| Parameter | Typ | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| `authentication_token` | Zeichenfolge | JA | Das Authentifizierungstoken. |
| `resource_id` | Zeichenfolge | JA | Eine einzelne Ressource. Dies kann mehrmals angegeben werden, einmal für jedes Element des Ressourcen-Arrays, das im `checkPreauthorizedResources()` API-Aufruf. |


**Hinweis:** Die maximale Anzahl der angeforderten Ressourcen kann konfiguriert werden.
**Der maximale Standardwert ist 5.**


#### Reaktion {#resp}

Die Antwort, die das Servlet preauthorize zurücksendet, hat das folgende Format:

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

Eine Liste der bereits autorisierten Ressourcen, die der AccessEnabler vom Service Provider erhält. Diese Liste von Ressourcen:

- Wird zusammen mit den AuthN- und AuthZ-Token gespeichert
- gültig, solange sich der Benutzer auf derselben Website befindet oder bis das AuthN-Token abläuft
- Wird jedes Mal erneut abgerufen, wenn der Benutzer auf eine neue integrierte Website mit Adobe Pass-Authentifizierung gelangt

Jeder Eintrag enthält die Ressourcen-ID, für die der Benutzer vorautorisiert ist.

Beispiel:


| Ressourcen-ID | Autorisiert |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


Diese Liste heißt &quot;Cache für die Vorabautorisierung&quot;.

#### Fluss {#flow}

1. Die App/Site des Programmierers erstellt eine `checkPreauthorizedResources(resourceList)` aufrufen.
1. AccessEnabler überprüft das Authentifizierungstoken für autorisierte Ressourcen:
   1. Wenn das Authentifizierungstoken autorisierte Ressourcen enthält, ist diese Liste autoritär und es sollte kein Aufruf durchgeführt werden, um diese Informationen zu erhalten. Ressourcen aus resourceList werden in der Liste der autorisierten Ressourcen im Authentifizierungstoken gesucht und nur die gefundenen Ressourcen werden von der `preauthorizedResources()` Callback.
   1. Wenn das Authentifizierungstoken KEINE autorisierten Ressourcen enthält - `resourceList` wird mit der Liste der Ressourcen im Cache für die Vorabautorisierung verglichen.
      1. Wenn die Liste dieselben Ressourcen enthält, bedeutet dies, dass bereits ein Aufruf an den Server erfolgt ist und sich die Antwort bereits im Cache für die Vorabautorisierung befindet. Nur autorisierte Ressourcen werden von der `preauthorizedResources()` Callback.
      1. Wenn die Liste NICHT dieselben Ressourcen enthält, muss der Client einen Aufruf an den Server durchführen, um den Autorisierungsstatus für die Ressourcen in resourceList zu erhalten. Die Antwort wird abgerufen und im Cache für die Vorabautorisierung gespeichert, wobei die alten Ressourcen vollständig ersetzt werden. Nur autorisierte Ressourcen werden von der `preauthorizedResources()` Callback.


#### Listenabruf {#listRetrieve}

Immer `checkPreauthorizedResources()` aufgerufen wird, wird die Liste der Ressourcen, die für die Autorisierung überprüft werden sollen, mit dem Cache für die Vorabautorisierung verglichen. Wenn die Liste denselben Satz von Ressourcen enthält, wird der Dienstleister nicht aufgerufen, da alle Ressourcen zum Auslösen der `preauthorizedResources()` -Rückruf befindet sich bereits im Cache.


#### logout() {#logout}

Der Cache für die Vorabautorisierung wird beim Abmelden geleert.


## Abhängigkeiten {#depends}

Die Leistung der Preflight-API hängt von bestimmten MVPD-Implementierungen ab.  Informationen zu Implementierungsoptionen finden Sie unter [Integration der Preflight-Autorisierung](/help/authentication/authz-usecase.md#preflight_authz_int) im Abschnitt &quot;Autorisierung&quot;des MVPD-Integrationsleitfadens.


## Sicherheit {#security}

Die Client-APIs stehen allen Programmierern zur Verfügung.

Bei der Implementierung wird HTTPS als Transport verwendet. Um jedoch einen leichteren Aufruf zu erhalten, werden keine zusätzlichen Sicherheitsmaßnahmen angewendet (keine Signierung, kein FAXS).

**Achtung:** Verwenden Sie diese API NICHT auf eine maßgebliche Weise, um festzustellen, ob einem Benutzer Zugriff auf eine geschützte Ressource gewährt werden soll. Der Zweck dieser API ist die Benutzeroberflächen-Dekoration und / oder die Vorabkennzeichnung von Geschäftsentscheidungen. Die `getAuthorization()` und `checkAuthorization()` -Aufrufe sollten immer durchgeführt werden, bevor die Wiedergabe zugelassen wird.


## Kompatibilität {#compat}

Diese Funktion wird in allen Versionen von AccessEnabler unterstützt: AS, JS, AIR, iOS, Android, Xbox (im 2nd-Screen-AuthN-Fluss).

Die Preflight-Autorisierung unterstützt keine Ressourcen, die CDATA-Abschnitte enthalten. Der Schwerpunkt des aktuellen Preflight-Systems liegt auf der Unterstützung der Filterung auf Kanalebene. Ressourcen mit CDATA-Abschnitten sind wahrscheinlich Ressourcen auf Asset-Ebene. Preflight unterstützt einfache `mrss` Ressourcen für die Vorautorisierung auf Kanalebene, sofern sie kein CDATA enthalten.

## Integration mit anderen Funktionen {#integ_w_other_features}

Eine mögliche Optimierung des Autorisierungsflusses kann vorkommen, um schnell zu scheitern, wenn die Ressource nicht in der Liste der vorab autorisierten Ressourcen enthalten ist.

Bei dieser Optimierung wird der Preflight-Endpunkt des Servers in den Abbaumechanismus integriert.

Wenn eine &quot;AuthN All&quot;-Regel für MVPD und Requestor vorhanden ist, spiegelt der Preflight-Endpunkt einfach alle in der Anfrage empfangenen Ressourcen wieder.

Dasselbe gilt, wenn &quot;AuthZ All&quot;für mindestens eine der in der Anfrage vorhandenen Ressourcen aktiviert ist.

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
