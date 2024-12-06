---
title: JavaScript SDK-API-Referenz
description: JavaScript SDK-API-Referenz
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2860'
ht-degree: 0%

---

# JavaScript SDK-API-Referenz {#javascript-sdk-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## API-Referenz {#api-reference}

Diese Funktionen lösen Interaktionsanfragen mit einem MVPD aus. Alle Aufrufe sind asynchron. Sie müssen [Callbacks](#callbacks) implementieren, um die Antworten zu verarbeiten:

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestorID, endpoints, options){#setrequestor(inRequestorID,endpoints,options)}

**Beschreibung:** Identifiziert die Site, von der die Anforderungen stammen.  Sie müssen diesen Aufruf vor allen anderen API-Aufrufen in einer Kommunikationssitzung durchführen.

**Parameter:**

- *inRequestorID* - Die eindeutige Kennung, die Adobe der ursprünglichen Site während der Registrierung zugewiesen hat.

- *endpoints* - Dieser Parameter ist optional. Es kann sich um einen der folgenden Werte handeln:

   - Ein Array, mit dem Sie Endpunkte für von Adobe bereitgestellte Authentifizierungs- und Autorisierungsdienste angeben können (verschiedene Instanzen können zum Debugging verwendet werden). Wenn mehrere URLs bereitgestellt werden, setzt sich die MVPD-Liste aus den Endpunkten aller Service Provider zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt. Standardmäßig (wenn kein Wert angegeben ist) wird der Adobe-Dienstleister verwendet (<http://sp.auth.adobe.com/>).

  Beispiel:
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options* - Ein JSON-Objekt, das den Wert für die Anwendungs-ID, die Werte für die Besucher-ID, die Einstellungen für &quot;refresh-less&quot;(Anmeldung im Hintergrund) und die MVPD-Einstellungen (iFrame) enthält. Alle Werte sind optional.
   1. Wenn angegeben, wird die Experience Cloud-visitorID bei allen von der Bibliothek durchgeführten Netzwerkaufrufen gemeldet. Der Wert kann später für erweiterte Analyseberichte verwendet werden.
   2. Wenn die eindeutige Kennung der Anwendung angegeben ist -`applicationId` -, wird der Wert zu allen nachfolgenden Aufrufen hinzugefügt, die von der Anwendung als Teil der HTTP-Kopfzeile &quot;X-Device-Info&quot;durchgeführt werden. Dieser Wert kann später mithilfe der richtigen Abfrage aus [ESM](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) -Berichten abgerufen werden.

  **Hinweis:** Bei allen JSON-Schlüsseln wird zwischen Groß- und Kleinschreibung unterschieden.

  Beispiel:

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- Der Programmierer kann die in der Adobe Pass-Authentifizierung konfigurierten MVPD-Einstellungen überschreiben, indem er angibt, ob ein iFrame für die Anmeldung erforderlich ist oder nicht (*iFrameRequired*-Schlüssel) und die iFrame-Dimensionen (*iFrameWidth* und *iFrameHeight*-Schlüssel). Das JSON-Objekt weist die folgende Vorlage auf:

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


Alle Schlüssel der obersten Ebene in der obigen Vorlage sind optional und weisen Standardwerte auf (*backgroundLogin*, *backgroundLogut* sind standardmäßig false und mvpdConfig ist null - d. h., dass keine MVPD-Einstellungen überschrieben werden).


- **Hinweis**: Das Angeben ungültiger Werte/Typen für die oben genannten Parameter führt zu undefiniertem Verhalten.



Hier finden Sie eine Beispielkonfiguration für das folgende Szenario: Aktivieren der Anmeldung ohne Aktualisierung und Abmeldung, Ändern von MVPD1 zu einer vollständigen Seitenumleitungsanmeldung (Nicht-iFrame) und MVPD2 zu iFrame-Anmeldung mit Breite=500 und Höhe=300:

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**Ausgelöste Rückrufe:** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[Zurück zum Anfang](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Beschreibung:** Fordert die Autorisierung für die angegebene Ressource an. Jedes Mal, wenn ein Kunde versucht, auf eine autorisierbare Ressource zuzugreifen, rufen Sie diese Funktion auf, um ein kurzlebiges Autorisierungstoken vom Access Enabler zu erhalten. Ressourcen-IDs werden mit dem MVPD vereinbart, der die Autorisierung erteilt.

Verwendet das zwischengespeicherte Authentifizierungstoken für den aktuellen Kunden. Wenn kein solches Token gefunden wird, initiiert zunächst den Authentifizierungsprozess und fährt dann mit der Autorisierung fort.

**Parameter:**

- `inResourceID` - Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
- `redirect_url` - Optional können Sie eine Umleitungs-URL angeben, sodass der Autorisierungsprozess des MVPD den Benutzer auf diese Seite und nicht auf die Seite zurückgibt, von der aus die Autorisierung initiiert wurde.


**Callbacks ausgelöst:** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) bei Erfolg, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) bei Fehler

>[!CAUTION]
>
>Verwenden Sie nach Möglichkeit checkAuthorization() anstelle von getAuthorization() . Die Methode getAuthorization() startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf der Seite des Programmierers führen kann.

</b>

[Zurück zum Anfang](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Beschreibung:** Fordert die Authentifizierung für den aktuellen Kunden an. Wird normalerweise als Reaktion auf einen Klick auf eine Anmelde-Schaltfläche aufgerufen. Sucht nach einem im Cache gespeicherten Authentifizierungstoken für den aktuellen Kunden. Wenn kein solches Token gefunden wird, initiiert den Authentifizierungsprozess. Dadurch wird das standardmäßige oder benutzerdefinierte Dialogfeld zur Anbieterauswahl aufgerufen und der ausgewählte Provider wird dann zur Anmeldungsschnittstelle des MVPD weitergeleitet.

Bei Erfolg erstellt und speichert ein Authentifizierungstoken für den Benutzer. Wenn die Authentifizierung fehlschlägt, gibt der Provider eine entsprechende Fehlermeldung an Ihren [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode) -Rückruf zurück.

**Parameter:**

- redirect_url - Geben Sie optional eine Umleitungs-URL an, sodass der Authentifizierungsprozess des MVPD den Benutzer auf diese Seite und nicht auf die Seite zurückgibt, von der aus die Authentifizierung initiiert wurde.

**Callbacks ausgelöst:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Zurück zum Anfang](#top)

</br>

## checkAuthN {#checkauthn}

**Beschreibung:** Überprüft den aktuellen Authentifizierungsstatus für den aktuellen Kunden.  Kein UI zugeordnet.

**Callbacks ausgelöst:** [setAuthentcationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Zurück zum Anfang](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus für den aktuellen Kunden und die angegebene Ressource zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn sie nicht authentifiziert ist, wird der Rückruf tokenRequestFailed() ausgelöst und die Methode wird beendet. Wenn der Benutzer authentifiziert ist, wird auch der Autorisierungsfluss Trigger. Siehe Details zur Methode [getAuthorization()](#getAuthZ .

>[!TIP]
>
> **Verwenden von Check-Status-Funktionen** Sie müssen den Status der Authentifizierung oder Autorisierung nicht überprüfen, bevor Sie eine Autorisierung anfordern. Sie können diese Funktionen aufrufen, um beispielsweise Ihre eigene Statusanzeige zu aktualisieren. Verwenden Sie sie nicht, wenn Sie eine Benutzerinteraktion benötigen.

**Parameter:**

- `inResourceID` - Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.


**Ausgelöste Rückrufe:**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**Beschreibung:** Fordert den Berechtigungsstatus &quot;preflight&quot;für eine Liste von
Ressourcen.

**Parameter:**

- *resources*: Der Ressourcenparameter ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()` -Aufruf, d. h. es handelt sich um einen vereinbarten Wert, der zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment festgelegt wurde.

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Diese API-Variante ist ab JS SDK-Version 4.0 verfügbar.


**Parameter:**

- *resources*: Der Ressourcenparameter ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()` -Aufruf, d. h. es handelt sich um einen vereinbarten Wert, der zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment festgelegt wurde.

- *cache*: Gibt an, ob der interne Cache beim Überprüfen auf vorab autorisierte Ressourcen verwendet werden soll. Dies ist ein optionaler Parameter, der standardmäßig auf **true** festgelegt ist. Wenn &quot;true&quot;, ist das Verhalten mit der oben genannten API identisch, d. h. nachfolgende Aufrufe dieser Funktion verwenden einen internen Cache, um eine vorab autorisierte Ressource aufzulösen. Durch Übergabe von **false** für diesen Parameter wird der interne Cache deaktiviert, was dazu führt, dass bei jedem Aufruf der API **checkPreauthorizedResources** ein Server-Aufruf erfolgt.

**Ausgelöste Rückrufe:** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[Zurück nach oben](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Beschreibung:** Ruft Informationen ab, die von der Access Enabler-Bibliothek als Metadaten bereitgestellt werden.

Es gibt zwei Arten von Metadaten:

- **Statisch** (Authentifizierungstoken TTL, Autorisierungstoken TTL und Geräte-ID)
- **Benutzermetadaten** (Dies umfasst benutzerspezifische Informationen, die vom MVPD an das Gerät des Benutzers während der Authentifizierungs- und/oder Autorisierungsflüsse weitergegeben werden)

**Weitere Informationen:** [Benutzermetadaten](#UserMetadata)

**Parameter:**

- *key*: Eine ID, die die angeforderten Metadaten angibt:
   - Wenn der Schlüssel `"TTL_AUTHN",` ist, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.

   - Wenn der Schlüssel &quot;`"TTL_AUTHZ"`&quot;lautet und &quot;params&quot;ein Array ist, das die Ressourcen-ID als Zeichenfolge enthält, wird die Abfrage ausgeführt, um die Ablaufzeit des Autorisierungstokens zu erhalten, das mit der angegebenen Ressource verknüpft ist.

   - Wenn der Schlüssel `"DEVICEID"` ist, wird die Abfrage durchgeführt, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen über Aktivierung und Gebühren zu erhalten.

   - Wenn der Schlüssel aus der folgenden Liste von Benutzermetadatypen stammt, wird ein JSON-Objekt, das die entsprechenden Benutzermetadaten enthält, an die Callback-Funktion [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) gesendet:

   - `"zip"` - Postleitzahl

   - `"encryptedZip"` - Verschlüsselte Postleitzahl

   - `"householdID"` - Kennung des Haushalts. Wenn ein MVPD keine Unterkonten unterstützt, ist dies mit der userID identisch.

   - `"maxRating"` - Maximale elterliche Bewertung für den Benutzer

   - `"userID"` - Die Benutzer-ID. Wenn ein MVPD Unterkonten unterstützt und der Benutzer nicht das Hauptkonto ist, unterscheidet sich die Benutzer-ID von der Haushalts-ID.

   - `"channelID"` - Die Liste der Kanäle, die der Benutzer anzeigen darf

   - `"is_hoh"` - Flag, das angibt, ob ein Benutzer Haushaltsleiter ist

   - `"encryptedZip"` - Verschlüsselte Postleitzahl

   - `"typeID"` - Flag, das angibt, ob das Benutzerkonto ein primäres/sekundäres Konto ist

   - `"primaryOID"` - Kennung des Haushalts

   - `"postalCode"` - Ähnlich wie Postleitzahl

   - `"acctID"` - Konto-ID

   - `"acctParentID"` - Übergeordnete Konto-ID

  **Hinweis**: Die tatsächlichen Benutzermetadaten, die einem Programmierer zur Verfügung stehen, hängen davon ab, was ein MVPD bereitstellt.  Die aktuelle Liste der verfügbaren Benutzermetadaten finden Sie unter [Benutzermetadaten](#UserMetadata) .


Beispiel:

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**Ausgelöste Rückrufe:** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[Zurück zum Anfang](#top)

</br>


## setSelectedProvider(providerId) {#setSelectedProvider}

**Beschreibung:** Rufen Sie diese Funktion auf, wenn der Benutzer ein MVPD aus der Benutzeroberfläche zur Anbieterauswahl ausgewählt hat, um die Anbieterauswahl an den Access Enabler zu senden, oder rufen Sie diese Funktion mit einem Null-Parameter auf, falls der Benutzer die Benutzeroberfläche zur Anbieterauswahl ohne Auswahl eines Anbieters verworfen hat.

**Callbacks
ausgelöst:**[ setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Zurück zum Anfang](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Beschreibung:** Ruft die Ergebnisse der Kundenauswahl im Dialogfeld zur Anbieterauswahl ab. Dies kann jederzeit nach der anfänglichen Authentifizierungsprüfung verwendet werden.

Diese Funktion ist asynchron und gibt ihr Ergebnis an Ihre `selectedProvider()` -Rückruffunktion zurück.

- **MVPD** Der derzeit ausgewählte MVPD oder null, wenn kein MVPD ausgewählt wurde.
- **AE_State** Das Ergebnis der Authentifizierung für den aktuellen Kunden lautet &quot;New User&quot;, &quot;User Not Authenticated&quot;oder &quot;User Authenticated&quot;.

**Ausgelöste Rückrufe:** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[Zurück zum Anfang](#top)

</br>

## Abmelden {#logout}

**Beschreibung:** Meldet den aktuellen Kunden ab und löscht alle Authentifizierungs- und Autorisierungsinformationen für diesen Benutzer. Löscht alle authN- und authZ-Token aus dem System des Kunden.

**Callbacks ausgelöst:** [setAuthentcationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[Zurück zum Anfang](#top)

</br>

## Callback-Definition {#calllback-definitions}

Sie müssen diese Rückrufe implementieren, um die Antworten auf Ihre asynchronen Anfrageaufrufe zu verarbeiten:

- [entitlementLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider()](#selectedproviderresult-selectedprovider)

</br>

## entitlementLoaded() {#entitlementLoaded}

**Beschreibung:** Wird ausgelöst, wenn der Access Enabler die Initialisierung abgeschlossen hat und für den Empfang von Anfragen bereit ist. Implementieren Sie diesen Rückruf, um zu erfahren, wann Sie die Kommunikation mit der Access Enabler-API starten können.
</br>

[Zurück zum Anfang](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Beschreibung:** Implementieren Sie diesen Rückruf, um die Konfigurationsinformationen und die MVPD-Liste zu erhalten.

**Parameter:**

- *configXML*: XML-Objekt, das die Konfiguration für den aktuellen ANFORDERER einschließlich der MVPD-Liste enthält.


**Ausgelöst von:** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Zurück zum Anfang](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**Beschreibung:** Implementieren Sie diesen Rückruf, um Ihre eigene benutzerdefinierte Benutzeroberfläche zur Anbieterauswahl aufzurufen. Ihr Dialogfeld sollte den Anzeigenamen (und das optionale Logo) verwenden, um die Auswahlmöglichkeiten des Kunden bereitzustellen. Wenn der Kunde eine Auswahl getroffen und das Dialogfeld verworfen hat, senden Sie die zugehörige ID für den ausgewählten Provider im Aufruf an *setSelectedProvider()*.

**Parameter:**

- *provider* - Ein Array von Objekten, die die angeforderten MVPDs darstellen:

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**Ausgelöst von:** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[Zurück nach oben](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Beschreibung:** Implementieren Sie diesen Rückruf, wenn der Benutzer ein MVPD ausgewählt hat, für das ein iFrame erforderlich ist, in dem die Benutzeroberfläche der Authentifizierungsanmeldeseite angezeigt werden soll.

**Ausgelöst von:**[ setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [Zurück nach oben](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Beschreibung:** Implementieren Sie diesen Rückruf, um den Authentifizierungsstatus (1=authentifiziert oder 0=nicht authentifiziert) zu erhalten, und eine beschreibende Fehlermeldung, wenn beim Versuch, den Authentifizierungsstatus zu ermitteln, ein Fehler aufgetreten ist (leere Zeichenfolge nach erfolgreichem Abschluss der Prüfung).

>[!NOTE]
> 
>Wenn Sie das aktuelle [Advance Error Reporting](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) -System verwenden, können Sie den an diese Funktion gesendeten errorCode-Parameter ignorieren.  Das isAuthenticated -Flaggschiff wird jedoch weiterhin zur Verfolgung des Authentifizierungsstatus eines Benutzers im Berechtigungsfluss verwendet


**Parameter:**

- *isAuthenticated* - Stellt den Authentifizierungsstatus bereit: 1 (authentifiziert) oder 0 (nicht authentifiziert).
- *errorCode* - Jeder Fehler, der beim Ermitteln des Authentifizierungsstatus aufgetreten ist. Eine leere Zeichenfolge, wenn keine.


**Ausgelöst von:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Zurück zum Anfang](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>Gerätetyp und Betriebssystem werden durch die Verwendung einer öffentlichen Java-Bibliothek (<http://java.net/projects/user-agent-utils>) und der Benutzeragenten-Zeichenfolge abgeleitet. Beachten Sie, dass diese Informationen nur als grobe Methode zur Unterteilung von Betriebsmetriken in Gerätekategorien bereitgestellt werden, dass Adobe jedoch keine Verantwortung für fehlerhafte Ergebnisse übernehmen kann. Verwenden Sie die neue Funktion entsprechend.

**Beschreibung:** Implementieren Sie diesen Rückruf, um Tracking-Daten zu empfangen, wenn bestimmte Ereignisse auftreten. Sie können dies beispielsweise verwenden, um zu verfolgen, wie viele Benutzer sich mit denselben Anmeldedaten angemeldet haben. Das Tracking ist derzeit nicht konfigurierbar. Mit Adobe Pass Authentication 1.6 meldet `sendTrackingData()` auch Informationen über das Gerät, den Access Enabler-Client und den Betriebssystemtyp. Der Rückruf `sendTrackingData()` bleibt abwärtskompatibel.

- Mögliche Werte für den Gerätetyp:
   - Computer
   - Tablet
   - mobile
   - Gameconsole
   - unknown

- Mögliche Werte für den Client-Typ Access Enabler :
   - html5
   - iOS
   - Android


Übergibt den Ereignistyp und ein Array der zugehörigen Informationen. Ereignistypen sind:

| mvpdSelection | Der Benutzer wählte ein MVPD in einem Anbieterauswahldialogfeld aus. |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | Eine Authentifizierungsprüfung ist abgeschlossen. |
| authorizationDetection | Eine Genehmigungsanfrage ist abgeschlossen. |

</br>
Daten sind für jeden Ereignistyp spezifisch:
</br>

| Ereignistyp (Zeichenfolge) | Daten (Array) |
|:--- | :--- |
| mvpdSelection | 0: Ausgewählter MVPD |
|  | 1: Gerätetyp |
|  | 2: Auf Enabler-Client-Typ zugreifen |
|  | 3: Betriebssystem |
| authenticationDetection | 0: Ob die Token-Anfrage erfolgreich war (true/false) |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: Token bereits im Cache (true/false) |
|  | 4: Gerätetyp |
|  | 5: Auf Enabler-Client-Typ zugreifen |
|  | 6: Betriebssystem |
| authorizationDetection | 0: Ob die Token-Anfrage erfolgreich war (true/false) |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: Token bereits im Cache (true/false) |
|  | 4: Fehler |
|  | 5: Details |
|  | 6: Gerätetyp |
|  | 7: Auf Enabler-Client-Typ zugreifen |
|  | 8: Betriebssystem |


**Ausgelöst von:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Zurück zum Anfang](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Beschreibung:** Implementieren Sie diesen Rückruf, um das kurzlebige Medien-Token (inToken) und die Kennung der Ressource (inRequestedResourceID) zu erhalten, für die eine Autorisierungsanfrage oder eine Anfrage zur Autorisierung durchgeführt wurde und die erfolgreich abgeschlossen wurde.

**Ausgelöst von:** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[Zurück zum Anfang](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**Beschreibung:** Implementieren Sie diesen Rückruf, um signalisiert zu werden, wenn eine Autorisierungs- oder Prüfautorisierungsanforderung fehlgeschlagen ist. Kann optional von einem MVPD verwendet werden, um eine benutzerdefinierte Nachricht bereitzustellen, die vom Programmierer angezeigt werden soll.

>[!IMPORTANT]
>
>Diese Rückruffunktion ist Teil des älteren, ursprünglichen Adobe Pass-Authentifizierungsfehlermeldesystems. Sie wird aus Gründen der Abwärtskompatibilität beibehalten. Es ist jedoch nicht erforderlich, diese Funktion überhaupt zu verwenden, wenn Sie Ihre eigenen Rückrufe mit dem aktuellen erweiterten Fehlermeldesystem implementiert haben. Das neuere Fehlermeldesystem enthält detailliertere Informationen darüber, warum eine Autorisierung (oder ein anderer Vorgang) fehlgeschlagen ist, sowie empfohlene Vorgehensweisen für jeden Fehler- oder Warnungstyp.

**Parameter:**

- *inRequestedResourceID* - Eine Zeichenfolge, die die Ressourcen-ID angibt, die für die Autorisierungsanforderung verwendet wurde.
- *inRequestErrorCode* - Eine Zeichenfolge, die den Fehlercode für die Adobe Pass-Authentifizierung anzeigt und den Grund für den Fehler angibt. Mögliche Werte sind &quot;User Not Authenticated Error&quot;und &quot;User Not Authorized Error&quot;. Weitere Informationen finden Sie unten unter &quot;Callback error codes&quot;.
- *inRequestDetailedErrorMessage* - Eine zusätzliche beschreibende Zeichenfolge, die für die Anzeige geeignet ist. Wenn diese beschreibende Zeichenfolge aus irgendeinem Grund nicht verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge **(&quot;&quot;)**.  Dies kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder umsatzbezogene Nachrichten zu übergeben. Wenn einem Abonnenten beispielsweise die Autorisierung für eine Ressource verweigert wird, kann der MVPD mit einem `*inRequestDetailedErrorMessage*` antworten, z. B.: **&quot;Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie auf \*hier\*.&quot;** Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Rückruf an die Website des Programmierers übergeben. Der Programmierer hat dann die Möglichkeit, ihn anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann auch `*inRequestDetailedErrorMessage*` verwenden, um den Programmierer über die Bedingung zu informieren, die möglicherweise zu einem Fehler geführt hat. Beispiel: **&quot;Bei der Kommunikation mit dem Autorisierungsdienst des Providers ist ein Netzwerkfehler aufgetreten&quot;.**



**Ausgelöst von:** [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Zurück zum Anfang](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird und die Liste der autorisierten Ressourcen bereitstellt, die nach einem Aufruf an `checkPreauthorizedResources()` zurückgegeben wurde.

**Parameter:**

- *authorizedResources*: Die Liste der autorisierten Ressourcen.

**Ausgelöst von:** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[Zurück zum Anfang](#top)

</br>

## setMetadataStatus(key, encrypted, data) {#setMetadataStatus(key,encrypted,data)}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird und die über einen `getMetadata()` -Aufruf angeforderten Metadaten liefert.

**Weitere Informationen:** [Benutzermetadaten](#userMetadata)

**Parameter:**

- *key (String)*: Der Schlüssel der Metadaten, für die die Anforderung ausgeführt wurde.
- *verschlüsselt (Boolesch)*: Eine Markierung, die angibt, ob der &quot;Wert&quot;verschlüsselt ist oder nicht. Wenn dies &quot;true&quot;ist, ist &quot;value&quot;tatsächlich eine JSON Web-verschlüsselte Darstellung des tatsächlichen Werts.
- *data (JSON-Objekt)*: Ein JSON-Objekt mit der Darstellung der Metadaten. Bei einfachen Anfragen (&quot;`TTL_AUTHN`&quot;, &quot;`TTL_AUTHZ`&quot;, &quot;`DEVICEID`&quot;) ist das Ergebnis eine Zeichenfolge (die die Authentifizierungs-TTL, Autorisierungs-TTL oder Geräte-ID darstellt). Bei einer User Metadata-Anfrage kann das Ergebnis ein Primitive- oder JSON-Objekt sein, das die Metadaten-Payload darstellt. Die tatsächliche Struktur der JSON-Benutzermetadatenobjekte ähnelt der folgenden:

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


Beispiel:

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**ausgelöst von:** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[Zurück nach oben](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**Beschreibung:** Implementieren Sie diesen Rückruf, um den aktuell ausgewählten MVPD und das Ergebnis der Authentifizierung des aktuellen Benutzers zu erhalten, der im Parameter `result` eingeschlossen ist. Der Parameter `result` ist ein Objekt mit den folgenden Eigenschaften:

- **MVPD** Der derzeit ausgewählte MVPD oder null, wenn kein MVPD ausgewählt wurde.
- **AE\_State** Das Ergebnis der Authentifizierung für den aktuellen Benutzer, einer von &quot;New User&quot;, &quot;User Not Authenticated&quot; oder &quot;User Authenticated&quot;

**Ausgelöst von:** [getSelectedProvider()](#getSelProv)

</br>

[Zurück zum Anfang](#top)

</br>

### Callback-Fehlercodes {#callback-error-codes}

| Allgemeine Fehler | |
|:--- | :--- | 
| Interner Fehler | Beim Versuch, eine Anfrage zu verarbeiten, ist ein Systemfehler aufgetreten. |
| Fehler bei Provider nicht ausgewählt | Tritt auf, wenn Kunden im Dialogfeld zur Anbieterauswahl abbrechen |
| Fehler bei Provider nicht verfügbar | Tritt auf, wenn keine Anbieter verfügbar sind. |

| Authentifizierungsfehler | |
|:--- | :--- | 
| Generischer Authentifizierungsfehler | Wird zurückgegeben, wenn der Grund nicht bekannt ist oder nicht veröffentlicht werden kann. |
| Interner Authentifizierungsfehler | Beim Authentifizierungsversuch ist ein Systemfehler aufgetreten. |
| Benutzer nicht authentifiziert Fehler | Der Benutzer ist nicht authentifiziert. |
| Fehler bei mehreren Authentifizierungsanforderungen | Zusätzliche Authentifizierungsanfragen wurden empfangen, bevor der erste abgeschlossen wurde. |

| Autorisierungsfehler | |
|:--- | :--- | 
| Allgemeiner Autorisierungsfehler | Wird zurückgegeben, wenn der Grund nicht bekannt ist oder nicht veröffentlicht werden kann. |
| Interner Autorisierungsfehler | Beim Autorisieren ist ein Systemfehler aufgetreten. |
| Benutzer nicht autorisiert Fehler | Der Kunde ist nicht berechtigt, den angeforderten Inhalt anzuzeigen. |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[Zurück zum Anfang](#top)
