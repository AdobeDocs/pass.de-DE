---
title: JavaScript SDK API-Referenz
description: JavaScript SDK API-Referenz
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2883'
ht-degree: 0%

---

# (Veraltete) JavaScript SDK API-Referenz {#javascript-sdk-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## API-Referenz {#api-reference}

Diese Funktionen initiieren Anfragen zur Interaktion mit einer MVPD. Alle Aufrufe sind asynchron. Sie müssen [Callbacks](#callbacks) implementieren, um die Antworten zu verarbeiten:

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

**Beschreibung:** Identifiziert die Site, von der die Anfragen stammen.  Sie müssen diesen Aufruf vor allen anderen API-Aufrufen in einer Kommunikationssitzung ausführen.

**Parameter:**

- *inRequestorID* - Die eindeutige Kennung, die Adobe der Ursprungs-Site während der Registrierung zugewiesen hat.

- *Endpunkte* - Dieser Parameter ist optional. Dies kann einer der folgenden Werte sein:

   - Ein Array, mit dem Sie Endpunkte für von Adobe bereitgestellte Authentifizierungs- und Autorisierungsdienste angeben können (verschiedene Instanzen können zu Debugging-Zwecken verwendet werden). Wenn mehrere URLs bereitgestellt werden, besteht die MVPD-Liste aus den Endpunkten aller Dienstleister. Jede MVPD ist mit dem schnellsten Dienstleister verknüpft, d. h. dem Anbieter, der zuerst geantwortet hat und diese MVPD unterstützt. Standardmäßig wird der Adobe-Dienstleister verwendet (<http://sp.auth.adobe.com/>), wenn kein Wert angegeben ist.

  Beispiel:
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options* - Ein JSON-Objekt, das den Wert der Anwendungs-ID, den Wert der Besucher-ID, die Einstellungen ohne Aktualisierung (Anmeldung im Hintergrund) und die Einstellungen für MVPD (iFrame) enthält. Alle Werte sind optional.
   1. Wenn angegeben, wird die Besucher-ID von Experience Cloud für alle Netzwerkaufrufe gemeldet, die von der Bibliothek ausgeführt werden. Der Wert kann später für erweiterte Analyseberichte verwendet werden.
   2. Wenn die eindeutige Kennung der Anwendung -`applicationId` angegeben ist, wird der Wert allen nachfolgenden Aufrufen hinzugefügt, die von der Anwendung als Teil der HTTP-Kopfzeile „X-Device-Info“ durchgeführt werden. Dieser Wert kann später mithilfe der richtigen Abfrage aus [ESM](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)-Berichten abgerufen werden.

  **Hinweis:** Bei allen JSON-Schlüsseln wird zwischen Groß- und Kleinschreibung unterschieden.

  Beispiel:

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- Der Programmierer kann die in der Adobe Pass-Authentifizierung konfigurierten MVPD-Einstellungen überschreiben, indem er angibt, ob ein iFrame für die Anmeldung erforderlich ist oder nicht (*iFrameRequired*-Schlüssel) und die iFrame-Dimensionen (*iFrameWidth*- und *iFrameHeight*-Schlüssel). Das JSON-Objekt verfügt über die folgende Vorlage:

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


Alle Schlüssel der obersten Ebene in der obigen Vorlage sind optional und haben Standardwerte (*backgroundLogin*, *backgroundLogut* sind standardmäßig false und mvpdConfig ist null - was bedeutet, dass keine MVPD-Einstellungen überschrieben werden).


- **Hinweis**: Die Angabe ungültiger Werte/Typen für die oben genannten Parameter führt zu undefiniertem Verhalten.



Im Folgenden finden Sie eine Beispielkonfiguration für das folgende Szenario: Aktivieren von „refresh-less“ für Anmelden und Abmelden, Ändern von MVPD1 in eine vollständige Seitenumleitungsanmeldung (nicht iFrame) und MVPD2 in eine iFrame-Anmeldung mit Breite=500 und Höhe=300:

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


**Ausgelöste Callbacks:** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[Zurück zum Anfang](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Beschreibung:** Fordert die Autorisierung für die angegebene Ressource an. Jedes Mal, wenn eine Kundin oder ein Kunde versucht, auf eine autorisierbare Ressource zuzugreifen, rufen Sie diese Funktion auf, um ein kurzlebiges Autorisierungs-Token vom Access Enabler abzurufen. Ressourcen-IDs werden mit dem MVPD vereinbart, der die Autorisierung bereitstellt.

Verwendet das zwischengespeicherte Authentifizierungstoken für den aktuellen Kunden. Wenn kein solches Token gefunden wird, initiiert zuerst den Authentifizierungsprozess und fährt dann mit der Autorisierung fort.

**Parameter:**

- `inResourceID` - Die ID der Ressource, für die der Benutzer eine Autorisierung anfordert.
- `redirect_url` - Geben Sie optional eine Umleitungs-URL an, damit der Autorisierungsprozess von MVPD den Benutzer zu dieser Seite zurückgibt und nicht zu der Seite, von der die Autorisierung initiiert wurde.


**Callbacks ausgelöst:** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) bei Erfolg, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) bei Fehler

>[!CAUTION]
>
>Verwenden Sie nach Möglichkeit checkAuthorization() anstelle von getAuthorization(). Die Methode getAuthorization() startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf Seiten des Programmierers führen kann.

</b>

[Zurück zum Anfang](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Beschreibung:** Fordert die Authentifizierung für den aktuellen Kunden an. Wird normalerweise als Reaktion auf einen Klick auf eine Anmeldeschaltfläche aufgerufen. Sucht nach einem zwischengespeicherten Authentifizierungstoken für den aktuellen Kunden. Wenn kein solches Token gefunden wird, initiiert den Authentifizierungsprozess. Dadurch wird das standardmäßige oder benutzerdefinierte Dialogfeld zur Anbieterauswahl aufgerufen und der ausgewählte Anbieter verwendet, um zur Anmeldeschnittstelle von MVPD weiterzuleiten.

Bei Erfolg erstellt und speichert ein Authentifizierungstoken für den Benutzer. Wenn die Authentifizierung fehlschlägt, gibt der Anbieter eine entsprechende Fehlermeldung an Ihren [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)Callback zurück.

**Parameter:**

- redirect_url - Geben Sie optional eine Umleitungs-URL an, damit der MVPD-Authentifizierungsprozess den Benutzer auf diese Seite zurückführt und nicht auf die Seite, von der die Authentifizierung initiiert wurde.

**Ausgelöste Callbacks:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Zurück zum Anfang](#top)

</br>

## checkAuthN {#checkauthn}

**Beschreibung:** Prüft den aktuellen Authentifizierungsstatus für den aktuellen Kunden.  Keine Benutzeroberfläche zugeordnet.

**Ausgelöste Callbacks:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Zurück zum Anfang](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus für den aktuellen Kunden und die angegebene Ressource zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn die Methode nicht authentifiziert ist, wird der Rückruf tokenRequestFailed() ausgelöst und die Methode beendet. Wenn der Benutzer authentifiziert ist, führt dies auch zu einem Trigger des Autorisierungsflusses. Siehe Details zur Methode [getAuthorization()]&#x200B;(#getAuthZ.

>[!TIP]
>
> **Mit Funktionen zum Überprüfen des** müssen Sie den Status der Authentifizierung oder Autorisierung nicht überprüfen, bevor Sie eine Autorisierung anfordern. Sie können diese Funktionen beispielsweise aufrufen, um Ihre eigene Statusanzeige zu aktualisieren. Verwenden Sie sie nicht, wenn Sie Benutzerinteraktionen benötigen.

**Parameter:**

- `inResourceID` - Die ID der Ressource, für die der Benutzer eine Autorisierung anfordert.


**Ausgelöste Callbacks:**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**Beschreibung:** Fordert den Autorisierungsstatus „preflight“ für eine Liste von an
Ressourcen.

**Parameter:**

- *resources*: Der Ressourcenparameter ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()`-Aufruf, d. h. sie ist ein vereinbarter Wert, der zwischen dem Programmierer und dem MVPD festgelegt wurde, oder ein RSS-Medienfragment.

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Diese API-Variante ist ab Version 4.0 von JS SDK verfügbar


**Parameter:**

- *resources*: Der Ressourcenparameter ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()`-Aufruf, d. h. sie ist ein vereinbarter Wert, der zwischen dem Programmierer und dem MVPD festgelegt wurde, oder ein RSS-Medienfragment.

- *cache*: Ob der interne Cache bei der Suche nach vorab autorisierten Ressourcen verwendet werden soll. Dies ist ein optionaler Parameter, der standardmäßig auf **true** festgelegt ist. Wenn „true“, ist das Verhalten mit dem der obigen API identisch, was bedeutet, dass nachfolgende Aufrufe dieser Funktion einen internen Cache verwenden, um vorautorisierte Ressourcen aufzulösen. Wenn Sie **false** für diesen Parameter übergeben, wird der interne Cache deaktiviert, was zu einem Server-Aufruf jedes Mal führt, wenn die API **checkPreauthorizedResources** aufgerufen wird.

**Ausgelöste Callbacks:** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[Zurück zum Anfang](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Beschreibung:** Ruft Informationen ab, die von der Access Enabler-Bibliothek als Metadaten bereitgestellt werden.

Es gibt zwei Arten von Metadaten:

- **Statisch** (Authentifizierungstoken-TTL, Autorisierungstoken-TTL und Geräte-ID)
- **Benutzermetadaten** (Dazu gehören benutzerspezifische Informationen, die während der Authentifizierung und/oder des Autorisierungsflusses von der MVPD an das Gerät der Benutzenden übergeben werden)

**Weitere Informationen:** [Benutzermetadaten](#UserMetadata)

**Parameter:**

- *key*: Eine ID, die die angeforderten Metadaten angibt:
   - Wenn der Schlüssel `"TTL_AUTHN",` ist, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.

   - Wenn key `"TTL_AUTHZ"` und params ein Array ist, das die Ressourcen-ID als Zeichenfolge enthält, wird die Abfrage durchgeführt, um die Ablaufzeit des Autorisierungs-Tokens abzurufen, das mit der angegebenen Ressource verknüpft ist.

   - Wenn der Schlüssel `"DEVICEID"` ist, wird die Abfrage durchgeführt, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen zu Aktivierung und Gebühren zu erhalten.

   - Wenn der Schlüssel aus der folgenden Liste von Benutzer-Metadatentypen stammt, wird ein JSON-Objekt, das die entsprechenden Benutzer-Metadaten enthält, an die [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)-Rückruffunktion gesendet:

   - `"zip"` - Postleitzahl

   - `"encryptedZip"` - Verschlüsselte Postleitzahl

   - `"householdID"` - Haushaltskennung. Falls eine MVPD keine Unterkonten unterstützt, ist dies identisch mit der Benutzer-ID.

   - `"maxRating"` - Maximale Bewertung der Eltern für den Benutzer

   - `"userID"` - Die Benutzerkennung. Falls eine MVPD Unterkonten unterstützt und der Benutzer nicht das Hauptkonto ist, unterscheidet sich die userID von der householdID.

   - `"channelID"` - Die Liste der Kanäle, die der Benutzer anzeigen darf

   - `"is_hoh"` - Markierung, die angibt, ob ein Benutzer Haushaltsleiter ist

   - `"encryptedZip"` - Verschlüsselte Postleitzahl

   - `"typeID"` - Markierung, die angibt, ob das Benutzerkonto ein primäres/sekundäres Konto ist.

   - `"primaryOID"` - Haushaltskennung

   - `"postalCode"` - Ähnlich wie Postleitzahl

   - `"acctID"` - Konto-ID

   - `"acctParentID"` - ID des übergeordneten Kontos

  **Hinweis**: Welche Benutzermetadaten einem Programmierer tatsächlich zur Verfügung stehen, hängt davon ab, was MVPD zur Verfügung stellt.  Siehe [Benutzermetadaten](#UserMetadata) für die aktuelle Liste der verfügbaren Benutzermetadaten.


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


**Ausgelöste Callbacks:** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[Zurück zum Anfang](#top)

</br>


## setSelectedProvider(providerId) {#setSelectedProvider}

**Beschreibung:** Rufen Sie diese Funktion auf, wenn Benutzende eine MVPD aus Ihrer Anbieterauswahl-Benutzeroberfläche ausgewählt haben, um die Anbieterauswahl an den Access Enabler zu senden, oder rufen Sie diese Funktion mit einem Null-Parameter auf, falls Benutzende Ihre Anbieterauswahl-Benutzeroberfläche verworfen haben, ohne einen Anbieter auszuwählen.

**Callbacks
trigger:**[&#x200B; setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Zurück zum Anfang](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Beschreibung:** Ruft die Ergebnisse der Kundenauswahl im Dialogfeld für die Anbieterauswahl ab. Dies kann jederzeit nach der ersten Authentifizierungsprüfung verwendet werden.

Diese Funktion ist asynchron und gibt ihr Ergebnis an Ihre `selectedProvider()` Callback-Funktion zurück.

- **MVPD** Die aktuell ausgewählte MVPD oder null, wenn keine MVPD ausgewählt wurde.
- **AE_**: Das Ergebnis der Authentifizierung für den aktuellen Kunden, entweder „Neuer Benutzer“, „Benutzer nicht authentifiziert“ oder „Benutzer authentifiziert“

**Ausgelöste Callbacks:** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[Zurück zum Anfang](#top)

</br>

## Abmelden {#logout}

**Beschreibung:** Meldet den aktuellen Kunden ab und löscht alle Authentifizierungs- und Autorisierungsinformationen für diesen Benutzer. Löscht alle authN- und authZ-Token aus dem System des Kunden.

**Ausgelöste Callbacks:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[Zurück zum Anfang](#top)

</br>

## Callback-Definition {#calllback-definitions}

Sie müssen diese Callbacks implementieren, um die Antworten auf Ihre asynchronen Anfrageaufrufe zu verarbeiten:

- [entityLoaded()](#entitlementloaded-entitlementloaded)
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

## entityLoaded() {#entitlementLoaded}

**Beschreibung:** Wird ausgelöst, wenn die Initialisierung des Access Enabler abgeschlossen ist und er für den Empfang von Anfragen bereit ist. Implementieren Sie diesen Callback, um zu erfahren, wann Sie die Kommunikation mit der Access Enabler-API starten können.
</br>

[Nach oben](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Beschreibung:** Implementieren Sie diesen Callback, um die Konfigurationsinformationen und die MVPD-Liste zu erhalten.

**Parameter:**

- *configXML*: XML-Objekt, das die Konfiguration für den aktuellen REQUESTOR enthält, einschließlich der MVPD-Liste.


**Ausgelöst von:** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Nach oben](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**Beschreibung:** Implementieren Sie diesen Callback, um Ihre eigene benutzerdefinierte Benutzeroberfläche für die Anbieterauswahl aufzurufen. Ihr Dialogfeld sollte den Anzeigenamen (und das optionale Logo) verwenden, um die Auswahl des Kunden bereitzustellen. Wenn der Kunde eine Entscheidung getroffen und das Dialogfeld verworfen hat, senden Sie die zugehörige ID für den ausgewählten Provider im Aufruf an *setSelectedProvider()*.

**Parameter:**

- *providers* - Ein Array von Objekten, die die angeforderten MVPDs darstellen:

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**Ausgelöst von:** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[Zurück zum Anfang](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Beschreibung:** Implementieren Sie diesen Callback, wenn der Benutzer einen MVPD ausgewählt hat, für den ein iFrame erforderlich ist, in dem die Benutzeroberfläche für die Authentifizierungs-Anmeldeseite angezeigt werden soll.

**Ausgelöst durch:**&#x200B;[&#x200B; setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [Zurück zum Anfang](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Beschreibung:** Implementieren Sie diesen Callback, um den Authentifizierungsstatus (1=authentifiziert oder 0=nicht authentifiziert) und eine beschreibende Fehlermeldung zu erhalten, wenn ein Fehler beim Versuch aufgetreten ist, den Authentifizierungsstatus zu ermitteln (leere Zeichenfolge bei erfolgreichem Abschluss der Prüfung).

>[!NOTE]
> 
>Wenn Sie das aktuelle System für [erweiterte Fehlerberichterstattung“ verwenden](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) können Sie den an diese Funktion gesendeten Parameter „errorCode“ ignorieren.  Das Flag isAuthenticated ist jedoch weiterhin für das Tracking des Authentifizierungsstatus eines Benutzers im Berechtigungsfluss von Nutzen


**Parameter:**

- *isAuthenticated* - Bietet einen Authentifizierungsstatus: 1 (authentifiziert) oder 0 (nicht authentifiziert).
- *errorCode* - Alle Fehler, die beim Bestimmen des Authentifizierungsstatus aufgetreten sind. Eine leere Zeichenfolge, falls keine vorhanden ist.


**Ausgelöst durch:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Nach oben](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>Der Gerätetyp und das Betriebssystem werden mithilfe einer öffentlichen Java-Bibliothek (<http://java.net/projects/user-agent-utils>) und der Benutzeragenten-Zeichenfolge abgeleitet. Beachten Sie, dass diese Informationen nur als grobe Möglichkeit bereitgestellt werden, Betriebsmetriken in Gerätekategorien aufzuschlüsseln, aber dass Adobe keine Verantwortung für falsche Ergebnisse übernehmen kann. Bitte verwenden Sie die neue Funktion entsprechend.

**Beschreibung:** Implementieren Sie diesen Callback, um Tracking-Daten zu erhalten, wenn bestimmte Ereignisse eintreten. Sie können dies beispielsweise verwenden, um zu verfolgen, wie viele Benutzer sich mit denselben Anmeldeinformationen angemeldet haben. Das Tracking ist derzeit nicht konfigurierbar. Bei der Adobe Pass-Authentifizierung 1.6 meldet `sendTrackingData()` auch Informationen zum Gerät, zum Access Enabler-Client und zum Betriebssystemtyp. Der `sendTrackingData()` Callback bleibt abwärtskompatibel.

- Mögliche Werte für den Gerätetyp:
   - Rechner
   - Tablet
   - Mobiltelefon
   - Gameconsole
   - Unbekannt

- Mögliche Werte für den Client-Typ von Access Enabler:
   - HTML5
   - iOS
   - Android


Übergibt den Ereignistyp und ein Array der zugehörigen Informationen. Ereignistypen sind:

| mvpdSelection | Der Benutzer hat eine MVPD in einem Dialogfeld zur Anbieterauswahl ausgewählt. |
| ----------------------- | --------------------------------------------------------- |
| Authentifizierungserkennung | Eine Authentifizierungsprüfung ist abgeschlossen. |
| authorizationDetection | Eine Autorisierungsanfrage ist abgeschlossen. |

</br>
Die Daten sind für jeden Ereignistyp spezifisch:
</br>

| Ereignistyp (Zeichenfolge) | Daten (Array) |
|:--- | :--- |
| mvpdSelection | 0: Ausgewählte MVPD |
|  | 1: Gerätetyp |
|  | 2: Zugriffs-Enabler-Client-Typ |
|  | 3: Betriebssystem |
| Authentifizierungserkennung | 0: Ob die Token-Anfrage erfolgreich war (true/false) |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: Token bereits im Cache (true/false) |
|  | 4: Gerätetyp |
|  | 5: Zugriffs-Enabler-Client-Typ |
|  | 6: Betriebssystem |
| authorizationDetection | 0: Ob die Token-Anfrage erfolgreich war (true/false) |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: Token bereits im Cache (true/false) |
|  | 4: Fehler |
|  | 5: Details |
|  | 6: Gerätetyp |
|  | 7: Zugriffs-Enabler-Client-Typ |
|  | 8: Betriebssystem |


**Ausgelöst durch:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Nach oben](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Beschreibung:** Implementieren Sie diesen Rückruf, um das kurzlebige Medien-Token (inToken) und die ID der Ressource (inRequestedResourceID) zu erhalten, für die eine Autorisierungsanfrage oder eine Anfrage zur Überprüfung der Autorisierung ausgeführt und erfolgreich abgeschlossen wurde.

**Ausgelöst von:** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[Nach oben](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**Beschreibung:** Implementieren Sie diesen Callback, der signalisiert werden soll, wenn eine Autorisierungs- oder eine Prüfautorisierungsanfrage fehlgeschlagen ist. Optional kann von einem MVPD verwendet werden, um eine benutzerdefinierte Nachricht bereitzustellen, die vom Programmierer angezeigt wird.

>[!IMPORTANT]
>
>Diese Rückruffunktion ist Teil des älteren, ursprünglichen Adobe Pass-Authentifizierungsfehler-Berichtssystems. Sie wird aus Gründen der Abwärtskompatibilität beibehalten, aber es ist überhaupt nicht erforderlich, diese Funktion zu verwenden, wenn Sie Ihre eigenen Callbacks mit dem aktuellen, erweiterten Fehlerberichterstattungssystem implementiert haben. Das neuere System zur Fehlerberichterstattung bietet detailliertere Informationen darüber, warum eine Autorisierung (oder ein anderer Vorgang) fehlgeschlagen ist, sowie empfohlene Vorgehensweisen für jede Art von Fehler oder Warnung.

**Parameter:**

- *inRequestedResourceID* - Eine Zeichenfolge, die die Ressourcen-ID angibt, die bei der Autorisierungsanfrage verwendet wurde.
- *inRequestErrorCode* - Eine Zeichenfolge, die den Adobe Pass-Authentifizierungsfehler-Code anzeigt und den Grund für den Fehler angibt. Mögliche Werte sind „User Not Authenticated Error“ und „User Not Authorized Error“; für weitere Details, siehe „Callback-Fehler-Codes“ unten.
- *inRequestDetailedErrorMessage* - Eine zusätzliche beschreibende Zeichenfolge, die für die Anzeige geeignet ist. Wenn diese beschreibende Zeichenfolge aus irgendeinem Grund nicht verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge **(“„)**.  Dies kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder verkaufsbezogene Nachrichten zu übergeben. Wenn beispielsweise einem Abonnenten die Autorisierung für eine Ressource verweigert wird, kann der MVPD mit einem `*inRequestDetailedErrorMessage*` antworten, z. B.: &quot;**Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie \*hier\*.“** Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Callback an die Website des Programmierers weitergeleitet. Der Programmierer hat dann die Möglichkeit, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann auch `*inRequestDetailedErrorMessage*` verwenden, um den Programmierer über die Bedingung zu informieren, die zu einem Fehler geführt haben könnte. Beispiel: **„Bei der Kommunikation mit dem Autorisierungsdienst des Anbieters ist ein Netzwerkfehler aufgetreten“.**



**Ausgelöst von:** [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Nach oben](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird, der die Liste der autorisierten Ressourcen bereitstellt, die nach einem Aufruf an `checkPreauthorizedResources()` zurückgegeben wird.

**Parameter:**

- *authorizedResources*: Die Liste der autorisierten Ressourcen.

**Ausgelöst von:** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[Nach oben](#top)

</br>

## setMetadataStatus(key, encrypted, data) {#setMetadataStatus(key,encrypted,data)}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird, der die über einen `getMetadata()`-Aufruf angeforderten Metadaten bereitstellt.

**Weitere Informationen:** [Benutzermetadaten](#userMetadata)

**Parameter:**

- *key (Zeichenfolge)*: Der Schlüssel der Metadaten, für die die Anfrage gestellt wurde.
- *encrypted (Boolescher Wert)*: Ein Flag, das angibt, ob der „Wert“ verschlüsselt ist oder nicht. Wenn dies „true“ ist, ist „value“ tatsächlich eine JSON-Web-verschlüsselte Darstellung des tatsächlichen Werts.
- *data (JSON-Objekt)*: Ein JSON-Objekt mit der Darstellung der Metadaten. Bei einfachen Anfragen (&#39;`TTL_AUTHN`&#39;, &#39;`TTL_AUTHZ`&#39;, &#39;`DEVICEID`&#39;) ist das Ergebnis eine Zeichenfolge (die die Authentifizierungs-TTL, Autorisierungs-TTL oder Geräte-ID darstellt). Bei einer Benutzermetadaten-Anfrage kann das Ergebnis ein primitives - oder JSON-Objekt sein, das die Metadaten-Payload darstellt. Die tatsächliche Struktur von JSON-Benutzer-Metadatenobjekten ähnelt der folgenden:

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


**Ausgelöst von:** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[Zurück zum Anfang](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**Beschreibung:** Implementieren Sie diesen Callback, um die aktuell ausgewählte MVPD und das Ergebnis der Authentifizierung der aktuellen Benutzerin oder des aktuellen Benutzers zu erhalten, die bzw. der im `result`-Parameter eingeschlossen ist. Der `result` ist ein -Objekt mit den folgenden Eigenschaften:

- **MVPD** Die aktuell ausgewählte MVPD oder null, wenn keine MVPD ausgewählt wurde.
- **AE\_**: Das Ergebnis der Authentifizierung für den aktuellen Benutzer, entweder „Neuer Benutzer“, „Benutzer nicht authentifiziert“ oder „Benutzer authentifiziert“

**Ausgelöst von:** [getSelectedProvider()](#getSelProv)

</br>

[Nach oben](#top)

</br>

### Callback-Fehlercodes {#callback-error-codes}

| Allgemeine Fehler | |
|:--- | :--- | 
| Interner Fehler | Bei der Verarbeitung der Anfrage ist ein Systemfehler aufgetreten. |
| Fehler „Anbieter nicht ausgewählt“ | Tritt auf, wenn der Kunde im Dialogfeld zur Anbieterauswahl abbricht |
| Fehler „Anbieter nicht verfügbar“ | Tritt auf, wenn keine Anbieter verfügbar sind. |

| Authentifizierungsfehler | |
|:--- | :--- | 
| Allgemeiner Authentifizierungsfehler | Wird zurückgegeben, wenn der Grund unbekannt ist oder nicht veröffentlicht werden kann. |
| Interner Authentifizierungsfehler | Beim Authentifizierungsversuch ist ein Systemfehler aufgetreten. |
| Fehler „Benutzer nicht authentifiziert“ | Benutzer ist nicht authentifiziert. |
| Fehler bei mehreren Authentifizierungsanfragen | Zusätzliche Authentifizierungsanfragen wurden empfangen, bevor die erste abgeschlossen wurde. |

| Autorisierungsfehler | |
|:--- | :--- | 
| Allgemeiner Autorisierungsfehler | Wird zurückgegeben, wenn der Grund unbekannt ist oder nicht veröffentlicht werden kann. |
| Interner Autorisierungsfehler | Beim Autorisierungsversuch ist ein Systemfehler aufgetreten. |
| Fehler „Benutzer nicht autorisiert“ | Der Kunde ist nicht berechtigt, die angeforderten Inhalte anzuzeigen. |

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
