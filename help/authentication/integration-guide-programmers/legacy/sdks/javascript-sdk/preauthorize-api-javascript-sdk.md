---
title: vorautorisieren
description: JavaScript vorab autorisieren
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1466'
ht-degree: 0%

---

# (Legacy) Autorisieren vorab {#js-preauthorize}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#preauth-overview}

Die Methode der Vorabautorisierungs-API muss von Anwendungen verwendet werden, um Vorabautorisierungsentscheidungen für eine oder mehrere Ressourcen zu erhalten. Die Anfrage zur Vorabautorisierung der API sollte für Benutzeroberflächen-Hinweise und/oder die Inhaltsfilterung verwendet werden. Eine Autorisierungs-API-Anfrage muss tatsächlich erfolgen, bevor der Benutzerzugriff auf die angegebenen Ressourcen zugelassen wird.

Falls ein unerwarteter Fehler auftritt (z. B. ein Netzwerkproblem und ein MVPD-Autorisierungsendpunkt nicht verfügbar), wenn eine Vorabautorisierungs-API-Anfrage von den Adobe Pass-Authentifizierungs-Services verarbeitet wird, werden eine oder mehrere separate Fehlerinformationen für die betroffenen Ressourcen als Teil des Ergebnisses der Vorabautorisierungs-API eingefügt.

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Beschreibung:** Diese Methode muss von Anwendungen verwendet werden, um die (informativen) Entscheidungen authentifizierter Benutzer vom Adobe Pass-Authentifizierungs-Service zur Anzeige bestimmter geschützter Ressourcen abzurufen, wobei der Hauptzweck darin besteht, die Benutzeroberfläche der Anwendung zu dekorieren (z. B. den Zugriffsstatus mit Schloss- und Entsperrsymbol anzuzeigen).

**Verfügbarkeit:** v4.4.0+

**Parameter:**

* `PreauthorizeRequest`: Builder-Objekt, das zum Definieren der Anfrage verwendet wird
* `AccessEnablerCallback`: Callback, der zur Rückgabe der API-Antwort verwendet wird
* `PreauthorizeResponse`: Objekt, das den Inhalt der API-Antwort zurückgibt

### PreauthorizeRequestBuilder-Klasse {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* Legt die Liste der Ressourcen fest, für die Sie Entscheidungen vor der Autorisierung abrufen möchten.
* Sie müssen sie für die Verwendung der vorab autorisierten API festlegen.
* Jedes Element in der Liste muss eine Zeichenfolge sein, die entweder den Wert der Ressourcen-ID oder das RSS-Medienfragment darstellt, das mit der MVPD abgestimmt werden muss.
* Diese Methode legt die Informationen nur im Kontext der aktuellen `PreauthorizeRequestBuilder` fest, die der Empfänger dieses Methodenaufrufs ist.

* Um eine tatsächliche `PreauthorizeRequest` zu erstellen, können Sie sich die Methode von `PreauthorizeRequestBuilder` ansehen:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` Ressourcen. Die Liste der Ressourcen, für die Sie Entscheidungen vor der Autorisierung abrufen möchten.
* `@returns {PreauthorizeRequestBuilder}` Der Verweis auf dieselbe `PreauthorizeRequestBuilder`-Objektinstanz, die der Empfänger des Methodenaufrufs ist.
* Dies ermöglicht die Erstellung einer Methodenverkettung.

#### disableFeatures(…features: string[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Legt die Funktionen fest, die deaktiviert werden sollen, wenn Entscheidungen über die Vorabautorisierung eingeholt werden.
* Diese Funktion legt die Informationen nur im Kontext der aktuellen `PreauthorizeRequestBuilder` fest, die der Empfänger dieses Funktionsaufrufs ist.
* Um ein tatsächliches `PreauthorizeRequest` zu erstellen, können Sie sich die Funktion von `PreauthorizeRequestBuilder` ansehen:

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` Funktionen. Die Funktionen, die deaktiviert werden sollen.
* `@returns` Der Verweis auf dieselbe `PreauthorizeRequestBuilder`-Objektinstanz, die der Empfänger des Funktionsaufrufs ist.
* Dies geschieht, um die Erstellung einer Funktionsverkettung zu ermöglichen.

#### build(s): preauthorizeRequest {#preauth-req}

* Erstellt den Verweis einer neuen `PreauthorizeRequest`-Objektinstanz und ruft ihn ab.
* Diese Methode instanziiert jedes Mal ein neues `PreauthorizeRequest`-Objekt, wenn es aufgerufen wird.
* Diese Methode verwendet die im Voraus festgelegten Werte im Kontext der aktuellen `PreauthorizeRequestBuilder`-Objektinstanz, die der Empfänger dieses Methodenaufrufs ist.
* Beachten Sie, dass diese Methode keine Nebenwirkungen verursacht,
* Daher ändert es weder den Status der SDK noch den Status der `PreauthorizeRequestBuilder`-Objektinstanz, die den Empfänger dieses Methodenaufrufs empfängt.
* Dies bedeutet, dass aufeinander folgende Aufrufe dieser Methode für denselben Empfänger verschiedene neue `PreauthorizeRequest`-Objektinstanzen erstellen, jedoch dieselben Informationen aufweisen, falls die auf den `PreauthorizeRequestBuilder` festgelegten Werte zwischen den Aufrufen nicht geändert wurden.
* Wenn Sie keine der bereitgestellten Informationen (Ressourcen und Caching) aktualisieren müssen, können Sie die PreauthorizeRequest-Instanz für mehrere Verwendungen der Preauthorize-API wiederverwenden.
* `@returns {PreauthorizeRequest}`

### Schnittstelle AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(result: T); {#on-response-result}

* Antwort-Callback, der von SDK aufgerufen wird, wenn die Anfrage zur Vorabautorisierung der API erfüllt wurde.
* Das Ergebnis ist entweder ein erfolgreiches oder ein Fehlerergebnis mit einem Status.
* `@param {T} result`

#### onFailure(Ergebnis: T); {#on-failure-result}

* Fehlgeschlagener Callback, der von SDK aufgerufen wird, wenn die Vorabautorisierungs-API-Anfrage nicht verarbeitet werden konnte.
* Das Ergebnis ist ein Fehlerergebnis mit einem Status .
* `@param {T} result`

### PreauthorizeResponse-Klasse {#preauth-response-class}

#### Öffentlicher Status: Status; {#public-status}

* Rückgabe: Zusätzliche Statusinformationen (Status) im Falle eines Fehlers.
* Kann einen `null` Wert enthalten.

#### öffentliche Entscheidungen: []; {#public-decisions}

* Gibt zurück: Die Liste der Entscheidungen vor Autorisierung. Eine Entscheidung für jede Ressource.
* Im Falle eines Fehlers kann die Liste leer sein.

### Klassenstatus {#class-status}

#### Öffentlicher Status: Nummer; {#public-status-numbr}

* Der HTTP-Antwort-Status-Code wie in RFC 7231 dokumentiert.
* Kann 0 sein, wenn die `Status` von SDK statt von Adobe Pass-Authentifizierungs-Services stammt.

#### Öffentlicher Code: Nummer; {#public-code-numbr}

* Der standardmäßige Fehler-Code der Adobe Pass-Authentifizierungs-Services.
* Kann eine leere Zeichenfolge oder einen `null` enthalten.

#### public message: string; {#public-msg-string}

* Die detaillierte Meldung, die in einigen Fällen von den MVPD-Autorisierungsendpunkten oder von Programmierer-Degradationsregeln bereitgestellt wird.
* Kann eine leere Zeichenfolge oder einen `null` enthalten.

#### public details: String; {#public-details-strng}

* Enthält eine detaillierte Nachricht, die in einigen Fällen von den MVPD-Autorisierungsendpunkten oder von Programmierer-Degradationsregeln bereitgestellt wird.
* Kann eine leere Zeichenfolge oder einen `null` enthalten.


#### public helpUrl: string; {#public-help-url-string}

* Die URL, die auf weitere Informationen über den Grund für diesen Status/Fehler und mögliche Lösungen verweist.
* Kann eine leere Zeichenfolge oder einen `null` enthalten.

#### public trace: string; {#public-trace-string}

* Die eindeutige Kennung für diese Antwort, die bei der Kontaktaufnahme mit dem Support verwendet werden kann, um bestimmte Probleme in komplexeren Szenarien zu identifizieren.
* Kann eine leere Zeichenfolge oder einen `null` enthalten.

#### public action: string; {#public-action-string}

* Die empfohlenen Maßnahmen zur Behebung der Situation.
   * **none**: Leider gibt es keine vordefinierte Aktion, um dieses Problem zu beheben. Dies kann auf einen fehlerhaften Aufruf der öffentlichen API hinweisen
   * **Konfiguration**: Eine Konfigurationsänderung ist über das TVE-Dashboard oder durch Kontaktaufnahme mit dem Support erforderlich.
   * **application-registration**: Die Anwendung muss sich erneut registrieren.
   * **Authentifizierung**: Der Benutzer muss sich authentifizieren oder erneut authentifizieren.
   * **authorization**: Der Benutzer muss die Autorisierung für die betreffende Ressource einholen.
   * **Abbau**: Es sollte irgendeine Form des Abbaus angewendet werden.
   * **Wiederholen**: Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden
   * **Erneut versuchen**: Wenn Sie die Anfrage nach dem angegebenen Zeitraum wiederholen, kann das Problem möglicherweise behoben sein.
* Kann eine leere Zeichenfolge oder einen `null` enthalten.

### Entscheidungsklasse {#class-decision}

#### public id: string; {#public-id-string}

* Die Ressourcen-ID, für die die Entscheidung abgerufen wurde.

#### public authorized: boolean; {#public-auth-boolean}

* Der Wert des Flags, der angibt, ob die Entscheidung erfolgreich war oder nicht.

#### öffentlicher Fehler: Status; {#public-error-status}

* Zusätzliche Statusinformationen (Status) für den Fall, dass ein Fehler aufgetreten ist. Kann einen `null` Wert enthalten.

## Beispiel einer Client-Implementierung {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## Szenario-Beispiele {#scenario-examples}

### Szenario 1: Alle angeforderten Ressourcen wurden autorisiert {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Verbesserte Fehlercode-Markierung</th>
    <th>Antwort</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### Szenario 2: Einige angeforderte Ressourcen wurden autorisiert. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Verbesserte Fehlercode-Markierung</th>
    <th>Antwort</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

    &quot;JavaScript
    
    {
    „decisions“: [
    {
    „id“: „RES01“,
    „authorized“: true
    },
    {
    „id“: „RES02“,
    „authorized“: false
    },
    {
    „id“: „RES03“,
    „authorized“: true
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Aktiviert</td>
    <td>

    &quot;JavaScript
    {
    „decisions“: [
    {
    „id“: „RES01“,
    „authorized“: true
    },
    {
    „id“: „RES02“,
    „authorized“: false,
    „error“: {
    „status“: 403,
    „code“: „preauthorization_denied_by_mvpd“,
    „message“: „Die MVPD hat eine \„Deny\&quot;-Entscheidung zurückgegeben, wenn eine Vorabautorisierung für die angegebene Ressource angefordert wird.“,
    „helpUrl“: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    „action“: „none“
    }
    },
    {
    „id“: „RES03“,
    „authorized“: true
    }
    &quot;
     
    
     

</td>
  </tr>
</tbody>


### Szenario 3: Keine der angeforderten Ressourcen wurde autorisiert. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Verbesserte Fehlercode-Markierung</th>
    <th>Antwort</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

    &quot;JavaScript
    
    {
    „decisions“: [
    {
    „id“: „RES01“,
    „authorized“: false
    },
    {
    „id“: „RES02“,
    „authorized“: false
    },
    {
    „id“: „RES03“,
    „authorized“: false
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Aktiviert</td>
    <td>

    &quot;JavaScript
    
    {
    „decisions“: [
    {
    „id“: „RES01“,
    „authorized“: false,
    „error“: {
    „status“: 403,
    „code“: „preauthorization_denied_by_mvpd“,
    „message“: „Die MVPD hat eine \„deny\&quot;-Entscheidung zurückgegeben, wenn sie eine Vorabautorisierung für die angegebene Ressource anfordert.“,
    „helpUrl“: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    „action“: „none“
    }
    },
    {
    „id“: „RES02“,
    „authorized“: false,
    „error“: {
    „status“: 403,
    „code“: „preauthorization_denied_by_mvpd“,message“: „Die MVPD hat bei der Anforderung einer Vorabautorisierung für die angegebene Ressource eine \„deny\&quot;-Entscheidung zurückgegeben.“,HelpUrl“: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;,Aktion“: „none„id}},
    „code“:RES03,0„authorized“: „false,error“: {
    „status“: {403,
     
     
     
     
     
     
     
     
    „code“: „maximum_execution_time“: &quot;.“„überstiegen“ wurde nicht in der maximal zulässigen Zeit abgeschlossen. Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden.“,
    „helpUrl“: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    „action“: „retry“
    }
    }
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Szenario 4: Fehlerhafte Client-Anfrage - Keine Ressourcen angegeben. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Verbesserte Fehlercode-Markierung</th>
    <th>Antwort</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>deaktiviert/aktiviert</td>
    <td>

    &quot;JavaScript
    {
    „status“: {
    „status“: 400,
    „code“: „internal_error“,
    „message“: „Die Anfrage ist aufgrund eines internen Fehlers fehlgeschlagen.“,
    „details“: „Erforderlicher String[]-Parameter „resource“ ist nicht vorhanden“,
    „helpUrl“: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    „action“: „none“
    },
    „decisions“: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Szenario 5: Fehlerhafte Client-Anfrage - leere Ressourcen angegeben. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Verbesserte Fehlercode-Markierung</th>
    <th>Antwort</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>deaktiviert/aktiviert</td>
    <td>

    &quot;JavaScript
    {
    „status“: {
    „status“: 412,
    „code“: „missing_resource“,
    „message“: „Der Ressourcenparameter fehlt“,
    „helpUrl“: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    „action“: „none“
    },
    „decisions“: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Szenario 6: Netzwerkfehler. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>Verbesserte Fehlercode-Markierung</th>
    <th>Antwort</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Aktiviert</td>
    <td>

    &quot;JavaScript
    {
    „decisions“: [
    {
    „id“: „RES01“,
    „authorized“: false,
    „error“: {
    „status“: 403,
    „code“: „network_received_error“,
    „message“: „Beim Abrufen der Antwort vom zugehörigen Partnerdienst ist ein Lesefehler aufgetreten. Ein Wiederholen der Anfrage kann das Problem möglicherweise beheben.“,
    „helpUrl“: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    „action“: „retry“
    }
    },
    {
    „id“: „RES02“,
    „authorized“: false,
    „error“: {
    „status“: 403,
    „code“: „network_received_error“,
    „message“: „Beim Abrufen der Antwort vom zugehörigen Partner-Service ist ein Lesefehler aufgetreten. Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden.“,
    „helpUrl“: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    „action“: „retry“
    }
    }
    ]
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Szenario 7: Der vorautorisierte Fluss wurde ohne gültige AuthN-Sitzung aufgerufen.

<table>
<thead>
  <tr>
    <th>Verbesserte Fehlercode-Markierung</th>
    <th>Antwort</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>deaktiviert/aktiviert</td>
    <td>

    &quot;JavaScript
    {
    „status“: {
    „status“: 0,
    „code“: „authentication_session_missing“,
    „message“: „Die mit dieser Anfrage verknüpfte Authentifizierungssitzung konnte nicht abgerufen werden. Die Benutzerin bzw. der Benutzer muss sich erneut mit einer unterstützten MVPD authentifizieren, um fortfahren zu können.“,
    „Aktion“: „Authentifizierung“
    },
    „Entscheidungen“: []
    }
    
    &quot;

</td>
  </tr>
</tbody>
</table>



### Szenario 8: Fluss „Vorab autorisieren“ wurde aufgerufen, bevor der setRequestor-Aufruf abgeschlossen wurde

<table>
<thead>
  <tr>
    <th>Verbesserte Fehlercode-Markierung</th>
    <th>Antwort</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>deaktiviert/aktiviert</td>
    <td>

    &quot;JavaScript
    {
    „status“: {
    „status“: 0,
    „code“: „Requestor_not_configuring“,
    „message“: „Der Anforderer ist noch nicht konfiguriert. Dies ist eine Voraussetzung für die Verwendung einer anderen API als der setRequestor-API.“,
    „action“: „Wiederholen“
    },
    „decisions“: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>
