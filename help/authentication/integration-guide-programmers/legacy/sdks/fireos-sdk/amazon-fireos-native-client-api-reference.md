---
title: Amazon FireOS Native Client-API-Referenz
description: Amazon FireOS Native Client-API-Referenz
exl-id: 8ac9f976-fd6b-4b19-a80d-49bfe57134b5
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '3429'
ht-degree: 0%

---

# (Legacy) Amazon FireOS-API-Referenz zur nativen Client-API {#amazon-fireos-native-client-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>

## Einführung {#intro}

In diesem Dokument werden die Methoden und Callbacks beschrieben, die von der Amazon FireOS SDK für die Adobe Pass-Authentifizierung bereitgestellt werden, die von der Adobe Pass-Authentifizierung unterstützt wird. Die hier beschriebenen Methoden und Callback-Funktionen sind in den Header-Dateien AccessEnabler.h und EntitlementDelegate.h definiert.

Die neueste Amazon FireOS AccessEnabler-SDK finden Sie in der <https://tve.zendesk.com/hc/en-us/articles/115005561623-fire-TV-Native-AccessEnabler-Library> .

>[!NOTE]
>
>Das Adobe Pass-Authentifizierungsteam empfiehlt, nur Adobe Pass-Authentifizierungs-APIs *öffentliche* zu verwenden:

- Öffentliche APIs sind für *unterstützten Clienttypen verfügbar* und vollständig getestet). Für jede öffentliche Funktion stellen wir sicher, dass jeder Client-Typ über eine entsprechende Version der zugehörigen Methode(n) verfügt.
- Öffentliche APIs müssen so stabil wie möglich sein, um die Abwärtskompatibilität zu unterstützen und sicherzustellen, dass Partnerintegrationen nicht beschädigt werden. Bei nicht *APIs behalten* sich jedoch das Recht vor, ihre Signatur in Zukunft zu ändern. Wenn Sie auf einen bestimmten Fluss stoßen, der nicht durch eine Kombination der aktuellen öffentlichen Adobe Pass-Authentifizierungs-API-Aufrufe unterstützt werden kann, sollten Sie uns dies mitteilen. Unter Berücksichtigung Ihrer Anforderungen können wir die öffentlichen APIs ändern und eine stabile Lösung für die Zukunft bereitstellen.

## Amazon FireOS SDK-API {#api}

- [getInstance](#getInstance)
- [setOptions](#fire_setOption)
- [setRequestor](#setRequestor)
- [setRequestComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- [checkPreauthorizedResources](#checkPreauth)
- [preauthorizedResources](#preauthResources)
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [Abmelden](#logout)
- [getSelectedProvider](#getSelectedProvider)
- [selectedProvider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setMetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

</br>

### Factory.getInstance {#getInstance}

**Beschreibung:** instanziiert das Access Enabler-Objekt. Pro Anwendungsinstanz sollte eine einzige Access Enabler-Instanz vorhanden sein.

| API-Aufruf: Konstruktor |
| --- |
| ```public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException```<br><br> |
| ```public static AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl) throws AccessEnablerException``` |

**Verfügbarkeit:** v3.0+


**Parameter:**

- *appContext*: Anwendungskontext des Amazon Fire OS.
- softwareStatement
- redirectUrl : Im Fall von FireOS wird der Parameterwert ignoriert und auf Standard gesetzt: adobepass://android.app
- env_url: Für Tests mit der Adobe-Staging-Umgebung kann env\_url auf „sp.auth-staging.adobe.com“ festgelegt werden

**Veraltet:**

```java
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```


### setRequestor {#setRequestor}

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung bei Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Diese Einstellung sollte nur einmal während des Lebenszyklus der Anwendung ausgeführt werden.

Die Antwort des Servers enthält eine Liste der MVPDs zusammen mit Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Server-Antwort wird intern vom Access Enabler-Code verwendet. Nur der Status des Vorgangs (d. h. ERFOLG/FEHLGESCHLAGEN) wird Ihrer Anwendung über den Callback setRequestorComplete() angezeigt.

Wenn der Parameter *urls* nicht verwendet wird, zielt der resultierende Netzwerkaufruf auf die Standard-Service-Provider-URL ab: die Adobe-Release-/Produktionsumgebung.

Wenn ein Wert für den Parameter *urls* angegeben wird, zielt der resultierende Netzwerkaufruf auf alle URLs ab, die im Parameter *urls* angegeben sind. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Bei der Kompilierung der Liste der MVPDs hat die erste Antwort Vorrang. Für jede MVPD in der Liste speichert Access Enabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die mit dem Dienstleister verknüpft ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Anfordererkonfiguration |
| --- |
| ```public void setRequestor(String requestorId)``` |


**Verfügbarkeit:** v3.0+


| API-Aufruf: Anfordererkonfiguration |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Verfügbarkeit:** v3.0+


**Parameter:**

- *RequestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie die eindeutige ID, die von Adobe an Ihre Site zugewiesen wurde, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
- *urls*: Optionaler Parameter; standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zu Debugging-Zwecken verwendet werden). Damit können Sie mehrere Instanzen des Authentifizierungs-Service von Adobe Pass angeben. Dabei besteht die MVPD-Liste aus den Endpunkten aller Dienstleister. Jede MVPD ist mit dem schnellsten Dienstleister verknüpft, d. h. dem Anbieter, der zuerst geantwortet hat und diese MVPD unterstützt.

**Ausgelöste Callbacks:** `setRequestorComplete()`



**Veraltet:**

```
    public void setRequestor(String requestorId, String signedRequestorId)

    public void setRequestor(String requestorId, String signedRequestorId, ArrayList<String> urls)
```

</br>


### setRequestComplete {#setRequestorComplete}

**Beschreibung:** Callback, ausgelöst durch den Access Enabler, der Ihrer Anwendung mitteilt, dass die Konfigurationsphase abgeschlossen ist. Dies ist ein Signal, dass die App mit der Ausstellung von Berechtigungsanfragen beginnen kann. Solange die Konfigurationsphase noch nicht abgeschlossen ist, kann die Anwendung keine Berechtigungsanfragen stellen.

| Callback: Anfordererkonfiguration abgeschlossen |
| --- |
| ```public void setRequestorComplete(int status)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - Konfiguration
Phase wurde erfolgreich abgeschlossen
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - Konfiguration
Phase fehlgeschlagen

**Ausgelöst von:** `setRequestor()`

</br>


### setOptions {#fire_setOption}

**Beschreibung:** Konfiguriert globale SDK-Optionen. Als Argument wird ein **Map\&lt;String, String\>** akzeptiert. Die Werte aus der Zuordnung werden zusammen mit jedem Netzwerkaufruf der SDK an den Server übergeben.

Die Werte werden unabhängig vom aktuellen Fluss (Authentifizierung/Autorisierung) an den Server übergeben. Wenn Sie die Werte ändern möchten, können Sie diese Methode jederzeit aufrufen.



| API-Aufruf: setOptions |
| --- |
| ```public void setOptions(HashMap<String,String> options)``` |

**Verfügbarkeit:** v3.0+

**Parameter:**

- *options*: Eine Zuordnung\&lt;String, String\> mit globalen SDK-Optionen. Derzeit stehen die folgenden Optionen zur Verfügung:
   - **applicationProfile** - Kann verwendet werden, um Server-Konfigurationen basierend auf diesem Wert vorzunehmen.
   - **ap\_vi** - Der Experience Cloud-ID-Dienst. Dieser Wert kann später für erweiterte Analyseberichte verwendet werden.
   - **device\_info** - Geräteinformationen, wie in **Cookbook für Geräteinformationen übergeben**

</br>

### checkAuthentication {#checkAuthN}

**Beschreibung:** Prüft den Authentifizierungsstatus. Dies erfolgt, indem im lokalen Token-Speicher nach einem gültigen Authentifizierungstoken gesucht wird. Beim Aufrufen dieser Methode werden keine Netzwerkaufrufe ausgeführt. Sie wird von der Anwendung verwendet, um den Authentifizierungsstatus des Benutzers abzufragen und die Benutzeroberfläche entsprechend zu aktualisieren (d. h. die Benutzeroberfläche für Anmeldung/Abmeldung zu aktualisieren). Der Authentifizierungsstatus wird der Anwendung über den Callback [*setAuthenticationStatus()*](#setAuthNStatus) mitgeteilt.

Wenn eine MVPD die Funktion „Authentifizierung pro Anfordernder“ unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden.

| API-Aufruf: Authentifizierungsstatus überprüfen |
| --- |
| ```public void checkAuthentication()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** `setAuthenticationStatus()`

</br>

### getAuthentication {#getAuthN}

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Sie beginnt mit der Prüfung des Authentifizierungsstatus. Wenn er nicht bereits authentifiziert ist, wird der Zustandscomputer des Authentifizierungsflusses gestartet:

- Wenn der letzte Authentifizierungsversuch erfolgreich war, wird die MVPD-Auswahlphase übersprungen und ein WebView-Steuerelement zeigt dem Benutzer die Anmeldeseite von MVPD an.
- Wenn der letzte Authentifizierungsversuch erfolglos war oder der Benutzer sich explizit abgemeldet hat, wird der [*displayProviderDialog()*](#displayProviderDialog) Callback ausgelöst. Ihre Anwendung verwendet diesen Callback, um die MVPD-Auswahlbenutzeroberfläche anzuzeigen. Außerdem muss die App den Authentifizierungsfluss fortsetzen, indem sie die Access Enabler-Bibliothek über die MVPD-Auswahl des Benutzers mithilfe der Methode [setSelectedProvider()](#setSelectedProvider) informiert.

Wenn eine MVPD die Funktion „Authentifizierung pro Anfordernder“ unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden (eines pro Programmierer).

Schließlich wird der Authentifizierungsstatus über den Rückruf *setAuthenticationStatus()* an die Anwendung übermittelt.

| API-Aufruf: Startet den Authentifizierungsfluss |
| --- |
| ```public void getAuthentication()``` |

**Verfügbarkeit:** v1.0+

| API-Aufruf: Startet den Authentifizierungsfluss |
| --- |
| ```public void getAuthentication(boolean forceAuthN, Map<String, Object> genericData)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *forceAuth*: Ein Flag, das angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Service gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:** `setAuthenticationStatus(), displayProviderDialog(), sendTrackingData()`

</br>

### displayProviderDialog {#displayProviderDialog}

**Beschreibung** Durch den Access Enabler ausgelöster Callback, um die Anwendung darüber zu informieren, dass die entsprechenden Benutzeroberflächenelemente instanziiert werden müssen, damit der Benutzer die gewünschte MVPD auswählen kann. Der Callback bietet eine Liste von MVPD-Objekten mit zusätzlichen Informationen, die dazu beitragen können, das Auswahlbenutzeroberflächenbedienfeld korrekt zu erstellen (z. B. die URL, die auf das MVPD-Logo verweist, den Anzeigenamen usw.)

Nachdem der Benutzer die gewünschte MVPD ausgewählt hat, muss die Upper-Layer-Anwendung den Authentifizierungsfluss fortsetzen, indem sie *setSelectedProvider()* aufruft und ihr die ID der MVPD übergibt, die der Auswahl des Benutzers entspricht.


| **Callback: Zeigt die MVPD-Auswahlbenutzeroberfläche an** |
| --- |
| ```public void displayProviderDialog(ArrayList<Mvpd> mvpds)``` |

**Verfügbarkeit:** v1.0+

**Parameter**:

- *mvpds*: Liste der MVPD-Objekte, die MVPD-bezogene Informationen enthalten, mit denen die Anwendung die MVPD-Auswahlbenutzeroberflächenelemente erstellen kann.

**Ausgelöst von:** `getAuthentication(), getAuthorization()`

</br>

### setSelectedProvider {#setSelectedProvider}

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Access Enabler über die MVPD-Auswahl des Benutzers zu informieren. Wenn *null* als Parameter übergeben wird, setzt Access Enabler die aktuelle MVPD auf einen Nullwert zurück.

| **API-Aufruf: Legen Sie den aktuell ausgewählten Anbieter fest** |
| --- |
| ```public void setSelectedProvider(String mvpdId)``` |


**Verfügbarkeit:**v 1.0+

**Parameter:** none

**Ausgelöste Callbacks:** `setAuthenticationStatus(), sendTrackingData()`
</br>

### navigateToUrl {#navigagteToUrl}

**Beschreibung:** Callback, ausgelöst durch den Access Enabler auf Android SDK. Sie sollte in Amazon FireOS SDK ignoriert werden.

| **Callback: MVPD-Anmeldeseite anzeigen** |
| --- |
| ```public void navigateToUrl(String url)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *url*: Die URL, die auf die Anmeldeseite von MVPD verweist

**Ausgelöst von:** `getAuthentication(), setSelectedProvider()`

</br>

### getAuthenticationToken {#getAuthNToken}

**Beschreibung:** Schließt den Authentifizierungsfluss ab, indem das Authentifizierungstoken vom Backend-Server angefordert wird.

| **API-Aufruf: Abrufen des Authentifizierungstokens** |
| --- |
| ```public void getAuthenticationToken(String cookies)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *Cookies*: Cookies, die in der Ziel-Domain gesetzt werden (eine Referenzimplementierung finden Sie im Demoprogramm in SDK).

**Ausgelöste Callbacks:** `setAuthenticationStatus(), sendTrackingData()`

</br>

### setAuthenticationStatus {#setAuthNStatus}

**Beschreibung:** Callback, ausgelöst durch den Access Enabler, der die Anwendung über den Status der Authentifizierung informiert. Es gibt viele Stellen, an denen der Authentifizierungsfluss fehlschlagen kann, entweder aufgrund der Interaktion des Benutzers oder aufgrund anderer unvorhergesehener Szenarien (z. B. Probleme mit der Netzwerkverbindung usw.). Dieser Rückruf informiert die Anwendung über den Erfolgs-/Fehlerstatus der Authentifizierung und liefert bei Bedarf zusätzliche Informationen zur Fehlerursache.

Dieser Rückruf signalisiert auch, wenn der Abmeldefluss abgeschlossen ist.

| **Callback: Meldet den Status des Authentifizierungsflusses** |
| --- |
| ```public void setAuthenticationStatus(int status, String errorCode)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - Authentifizierungsfluss wurde erfolgreich abgeschlossen
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - Authentifizierungsfluss fehlgeschlagen
   - `AccessEnabler.ACCESS_ENABLER_STATUS_LOGOUT` - Abmelden
- *code*: Grund für den präsentierten Status. Wenn *status* den Wert `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` hat, ist *code* eine leere Zeichenfolge (d. h. definiert durch die `AccessEnabler.USER_AUTHENTICATED`). Wenn dieser Parameter nicht authentifiziert ist, kann er einen der folgenden Werte annehmen:
   - `AccessEnabler.USER_NOT_AUTHENTICATED_ERROR` - Der Benutzer ist nicht authentifiziert. Als Antwort auf den Aufruf der *checkAuthentication()*-Methode, wenn im Cache des lokalen Tokens kein gültiges Authentifizierungstoken vorhanden ist.
   - `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler hat den Authentifizierungsstatus-Computer zurückgesetzt, nachdem die Upper-Layer-Anwendung (*)* wurde, um den Authentifizierungsfluss abzubrechen`setSelectedProvider()`.  Vermutlich hat der Benutzer den Authentifizierungsfluss abgebrochen (d.h. die Schaltfläche „Zurück“ gedrückt).
   - `AccessEnabler.GENERIC_AUTHENTICATION_ERROR` - Der Authentifizierungsfluss ist aus Gründen wie der Nichtverfügbarkeit des Netzwerks oder dem expliziten Abbruch des Authentifizierungsflusses fehlgeschlagen.
   - `AccessEnabler.LOGOUT` - Der Benutzer wird aufgrund einer Abmeldeaktion nicht authentifiziert.

**Ausgelöst von:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

</br>

### checkPreauthorizedResources {#checkPreauth}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der primäre Zweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden können (z. B. die Angabe des Zugriffsstatus mit Sperren- und Entsperren-Symbolen).

| **API-Aufruf: Legen Sie den aktuell ausgewählten Anbieter fest** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Verfügbarkeit:** v1.0+

**&lt;Parameters:** Der `resources` Parameter ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()`-Aufruf, d. h. sie sollte ein vereinbarter Wert sein, der zwischen dem Programmierer und dem MVPD oder einem RSS-Medienfragment festgelegt wird.

**Rückruf ausgelöst:** `preauthorizedResources()`

</br>

### preauthorizedResources {#preauthResources}

**Beschreibung:** Callback, ausgelöst durch checkPreauthorizedResources(). Stellt eine Liste der Ressourcen bereit, die der Benutzer bereits anzeigen darf.

| **API-Aufruf: Legen Sie den aktuell ausgewählten Anbieter fest** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Verfügbarkeit:**v 1.0+

**Parameter:** Der `resources` ist ein Array von Ressourcen, für die der Benutzer bereits berechtigt ist, sie anzuzeigen.

**Ausgelöst von:** `checkPreauthorizedResources()`

<br>

### checkAuthorization {#checkAuthZ}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn die Methode nicht authentifiziert ist *wird der Rückruf* setTokenRequestFailed() ausgelöst und die Methode beendet. Wenn der Benutzer authentifiziert ist, führt dies auch zu einem Trigger des Autorisierungsflusses. Siehe Details zur Methode *getAuthorization(*.

| **API-Aufruf: Autorisierungsstatus überprüfen** |
| --- |
| ```public void checkAuthorization(String resourceId)``` |

**Verfügbarkeit:** v1.0+

| **API-Aufruf: Autorisierungsstatus überprüfen** |
| --- |
| ```public void checkAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die ID der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Service gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:** `tokenRequestFailed(), setToken(), sendTrackingData(), setAuthenticationStatus()`

</br>

### getAuthorization {#getAuthZ}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsfluss zu starten. Wenn der Benutzer noch nicht authentifiziert ist, wird auch der Authentifizierungsfluss initiiert. Wenn der Benutzer authentifiziert wird, fährt der Access Enabler fort, Anfragen für das Autorisierungs-Token (wenn kein gültiges Autorisierungs-Token im lokalen Token-Cache vorhanden ist) und für das kurzlebige Medien-Token zu stellen. Sobald das kurze Medien-Token abgerufen wurde, wird der Autorisierungsfluss als abgeschlossen betrachtet. Der *setToken()*-Rückruf wird ausgelöst und das kurze Medien-Token wird als Parameter an die Anwendung gesendet. Wenn die Autorisierung aus irgendeinem Grund fehlschlägt, wird der Callback *tokenRequestFailed()* ausgelöst und der Fehlercode und die Details werden bereitgestellt.

| **API-Aufruf: Initiieren des Autorisierungsflusses** |
| --- |
| ```public void getAuthorization(String resourceId)``` |

**Verfügbarkeit:** v1.0+

| **API-Aufruf: Initiieren des Autorisierungsflusses** |
| --- |
| ```public void getAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die ID der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Service gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:** `tokenRequestFailed(), setToken(), sendTrackingData()`

|     |     |
| --- | --- |
| ![](http://learn.adobe.com/wiki/images/icons/emoticons/warning.gif) | **Zusätzliche Callbacks ausgelöst** <br>Diese Methode kann auch einen Trigger der folgenden Callbacks ausführen (wenn der Authentifizierungsfluss ebenfalls initiiert wird): _setAuthenticationStatus()_, _displayProviderDialog()_ |

**HINWEIS: Verwenden Sie nach Möglichkeit checkAuthorization() anstelle von getAuthorization(). Die Methode getAuthorization() startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf Seiten des Programmierers führen kann.**

</br>

### setToken {#setToken}

**Beschreibung:** Callback, ausgelöst durch den Access Enabler, der Ihrer Anwendung mitteilt, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde. Das kurzlebige Medien-Token wird ebenfalls als Parameter bereitgestellt.

| **Callback: Autorisierungsfluss erfolgreich abgeschlossen** |
| --- |
| ```public void setToken(String token, String resourceId)``` |

**Verfügbarkeit:**v 1.0+

**Parameter:**

- *token*: Das kurzlebige Medien-Token
- *resourceId*: Die Ressource, für die die Autorisierung abgerufen wurde

**Ausgelöst von:** `checkAuthorization(), getAuthorization()`

<br>

### tokenRequestFailed {#tokenRequestFailed}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird, der die Anwendung der oberen Ebene darüber informiert, dass der Autorisierungsfluss fehlgeschlagen ist.

| **Callback: Autorisierungsfluss fehlgeschlagen** |
| --- |
| ```public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die Ressource, für die die Autorisierung abgerufen wurde
- *errorCode*: Fehlercode, der mit dem Fehlerszenario verknüpft ist. Mögliche Werte:
   - `AccessEnabler.USER_NOT_AUTHORIZED_ERROR` - Der Benutzer konnte für die angegebene Ressource keine Autorisierung vornehmen.
- *errorDescription*: Zusätzliche Details zum Fehlerszenario. Wenn diese beschreibende Zeichenfolge aus irgendeinem Grund nicht verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge >**(“„)**.  Diese Zeichenfolge kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder verkaufsbezogene Meldungen zu übergeben. Wenn beispielsweise einem Abonnenten die Autorisierung für eine Ressource verweigert wird, kann der MVPD eine Nachricht senden, die wie folgt lautet: „Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie hier.“ Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Callback an den Programmierer weitergeleitet, der die Möglichkeit hat, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann diesen Parameter auch verwenden, um eine Benachrichtigung über die Bedingung bereitzustellen, die zu einem Fehler geführt haben könnte. Beispiel: „Bei der Kommunikation mit dem Autorisierungsdienst des Anbieters ist ein Netzwerkfehler aufgetreten.“

**Ausgelöst von:** `checkAuthorization(), getAuthorization()`

</br>

### Abmelden {#logout}

**Beschreibung:** Verwenden Sie diese Methode, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Umleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss.

| **API-Aufruf: Initiieren des Abmeldeflusses** |
| --- |
| ```public void logout()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** keine

</br>

### getSelectedProvider {#getSelectedProvider}

**Beschreibung:** Verwenden Sie diese Methode, um den aktuell ausgewählten Anbieter zu bestimmen.

| **API-Aufruf: Bestimmen Sie die aktuell ausgewählte MVPD** |
| --- |
| ```public void getSelectedProvider()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** `selectedProvider()`

</br>

### selectedProvider {#selectedProvider}

**Beschreibung:** Callback, der durch den Access Enabler ausgelöst wird, der Informationen über die aktuell ausgewählte MVPD an die Anwendung liefert.

| **Callback: Informationen zum aktuell ausgewählten MVPD** |
| --- |
| ```public void selectedProvider(Mvpd mvpd)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *mvpd*: Objekt, das Informationen zum aktuell ausgewählten MVPD enthält

**Ausgelöst von:** `getSelectedProvider()`

</br>

### getMetadata {#getMetadata}

**Beschreibung:** Verwenden Sie diese Methode, um Informationen abzurufen, die von der Access Enabler-Bibliothek als Metadaten bereitgestellt werden. Die Anwendung kann auf diese Informationen zugreifen, indem sie ein zusammengesetztes MetadataKey-Objekt bereitstellt.

| **API-Aufruf: Abfragen des AccessEnabler nach Metadaten** |
| --- |
| ```public void getMetadata(MetadataKey metadataKey)``` |

**Verfügbarkeit:** v1.0+

Programmierern stehen zwei Arten von Metadaten zur Verfügung:

- Statische Metadaten (Authentifizierungstoken-TTL, Autorisierungstoken-TTL und Geräte-ID)
- Benutzermetadaten (benutzerspezifische Informationen wie Benutzer-ID und Postleitzahl, die während der Authentifizierung und/oder des Autorisierungsflusses von einer MVPD an das Gerät eines Benutzers übergeben werden)

**Parameter:**

- *metadataKey*: Eine Datenstruktur, die eine Schlüssel- und eine args-Variable mit der folgenden Bedeutung einkapselt:
   - Wenn der Schlüssel `METADATA_KEY_TTL_AUTHN` ist, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.
   - Wenn key `METADATA_KEY_TTL_AUTHZ` ist und args ein SerializableNameValuePair-Objekt mit name = `METADATA_ARG_RESOURCE_ID` und value = `[resource_id]` enthält, wird die Abfrage ausgeführt, um die Ablaufzeit des Autorisierungstokens abzurufen, das der angegebenen Ressource zugeordnet ist.
   - Wenn der Schlüssel `METADATA_KEY_DEVICE_ID` ist, wird die Abfrage durchgeführt, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen zu Aktivierung und Gebühren zu erhalten.
   - Wenn key `METADATA_KEY_USER_META` ist und args ein SerializableNameValuePair-Objekt mit name = `METADATA_KEY_USER_META` und value = `[metadata_name]` enthält, wird die Abfrage für Benutzermetadaten durchgeführt. Die aktuelle Liste der verfügbaren Benutzer-Metadatentypen:
      - `zip` - Postleitzahl
      - `householdID` - Haushaltskennung. Wenn eine MVPD keine Unterkonten unterstützt, ist dies mit `userID` identisch.
      - `maxRating` - Maximale Bewertung der Eltern für den Benutzer
      - `userID` - Die Benutzerkennung. Wenn eine MVPD Unterkonten unterstützt und der Benutzer nicht das Hauptkonto ist,
      - `channelID` : Eine Liste der Kanäle, die der Benutzer anzeigen darf

Welche Benutzermetadaten einem Programmierer tatsächlich zur Verfügung stehen, hängt davon ab, was MVPD zur Verfügung stellt.  Diese Liste wird weiter erweitert, sobald neue Metadaten verfügbar gemacht und zum Adobe Pass-Authentifizierungssystem hinzugefügt werden.

**Ausgelöste Callbacks:** [`setMetadataStatus()`](#setMetadaStatus)

**Weitere Informationen:** [Benutzermetadaten](#setmetadatastatus)

</br>

### setMetadataStatus {#setMetadaStatus}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird, der die über einen Aufruf von *getMetadata()* angeforderten Metadaten bereitstellt.

| **Callback: Ergebnis einer Anfrage zum Abrufen von Metadaten** |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *key*: Das MetadataKey-Objekt, das den Schlüssel, für den der Metadatenwert angefordert wird, und die zugehörigen Parameter enthält (eine Referenzimplementierung finden Sie unter Demoanwendung ).
- *result*: Ein zusammengesetztes Objekt, das die angeforderten Metadaten enthält. Das -Objekt hat die folgenden Felder:
   - *simpleResult*: eine Zeichenfolge, die den Metadatenwert darstellt, als die Anfrage für die Authentifizierungs-TTL, Autorisierungs-TTL oder Geräte-ID gestellt wurde. Dieser Wert ist null, wenn die Anfrage für Benutzermetadaten durchgeführt wurde.

   - *userMetadataResult*: Ein Objekt, das die Java-Darstellung einer JSON-Benutzermetadaten-Payload enthält. Beispiel:

     ```json
     {
     "street": "Main Avenue",
     "buildings": ["150", "320"]
     }
     ```

     wird wie folgt in Java übersetzt:

     ```java
     Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
     ```

     **Die tatsächliche Struktur von Benutzermetadatenobjekten ähnelt der folgenden:**

     ```json
     {
         updated: 1334243471,
         encrypted: ["encryptedProp"],
         data: {
             zip: ["12345", "34567"],
             maxRating: { 
                 "MPAA": "PG-13",
                 "VCHIP": "TV-Y", 
                 "URL": "http://exam.pl/e/manage/ratings"
             },
             householdID: "3456",
             userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
             channelID: ["channel-1", "channel-2"]
         }
     }
     ```


Dieser Wert ist null, wenn die Anfrage für einfache Metadaten (Authentifizierungs-TTL, Autorisierungs-TTL oder Geräte-ID) gestellt wurde.

- *Encrypted*: Boolescher Wert, der angibt, ob die abgerufenen Metadaten verschlüsselt sind oder nicht. Dieser Parameter ist nur für Benutzermetadaten-Anfragen von Bedeutung. Er hat keine Bedeutung für statische Metadaten (z. B. Authentifizierungs-TTL), die immer unverschlüsselt empfangen werden. Wenn dieser Parameter auf „True“ gesetzt ist, liegt es am Programmierer, den unverschlüsselten Wert der Benutzermetadaten durch eine RSA-Entschlüsselung mit dem privaten Schlüssel für die Whitelist abzurufen (derselbe private Schlüssel, der für die Unterzeichnung der Anforderer-ID im [`setRequestor`](#setRequestor)-Aufruf verwendet wird).

**Ausgelöst von:** [`getMetadata()`](#getMetadata)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

</br>

## getVersion {#getVersion}

**Beschreibung:** Verwenden Sie diese Methode, um die aktuelle Version von AccessEnabler abzurufen

| **API-Aufruf: Version von AccessEnabler abrufen** |
| --- |
| ```public static String getVersion()``` |

## Tracking von Ereignissen {#tracking}

Der Access Enabler-Trigger führt einen zusätzlichen Callback aus, der nicht unbedingt mit den Berechtigungsflüssen in Zusammenhang steht. Die Implementierung der Rückruffunktion zur Ereignisverfolgung mit dem Namen *sendTrackingData()* ist optional, ermöglicht es der Anwendung jedoch, bestimmte Ereignisse zu verfolgen und Statistiken wie die Anzahl erfolgreicher/fehlgeschlagener Authentifizierungs-/Autorisierungsversuche zu erstellen. Nachfolgend finden Sie die Spezifikation für den Callback *sendTrackingData()*:

### sendTrackingData {#sendTrackingData}

**Beschreibung:** Vom Access Enabler ausgelöster Callback, der der Anwendung das Auftreten verschiedener Ereignisse wie Abschluss/Fehler von Authentifizierungs-/Autorisierungsflüssen signalisiert. Der Gerätetyp, der Access Enabler-Clienttyp und das Betriebssystem werden ebenfalls von sendTrackingData() gemeldet.

>[!WARNING]
>
> Der Gerätetyp und das Betriebssystem werden mithilfe einer öffentlichen Java-Bibliothek (http://java.net/projects/user-agent-utils) und der Benutzeragenten-Zeichenfolge abgeleitet. Beachten Sie, dass diese Informationen nur als grobe Methode zur Unterteilung der Betriebsmetriken in Gerätekategorien bereitgestellt werden, aber dass Adobe keine Verantwortung für falsche Ergebnisse übernehmen kann. Bitte verwenden Sie die neue Funktion entsprechend.

- Mögliche Werte für den Gerätetyp:
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`

- Mögliche Werte für den Client-Typ von Access Enabler:
   - `flash`
   - `html5`
   - `ios`
   - `tvos`
   - `android`
   - `firetv`

| Callback: Ereignisse verfolgen |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *event*: das verfolgte Ereignis. Es gibt drei mögliche Typen von Tracking-Ereignissen:
   - **authorizationDetection:** jedes Mal, wenn eine Autorisierungs-Token-Anfrage zurückgibt (Ereignistyp `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** jedes Mal, wenn eine Authentifizierungsprüfung erfolgt (Ereignistyp ist `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** Wenn Benutzende eine MVPD im MVPD-Auswahlformular auswählen (Ereignistyp ist `EVENT_MVPD_SELECTION`)
- *data*: Zusätzliche Daten, die mit dem gemeldeten Ereignis verknüpft sind. Diese Daten werden in Form einer Werteliste angezeigt.

Im Folgenden finden Sie Anweisungen zum Interpretieren der Werte im *data*-Array:

- Für Ereignistyp *`EVENT_AUTHN_DETECTION`:*
   - **0** - Ob die Token-Anfrage erfolgreich war (true/false) und ob das oben Genannte wahr ist:
   - **1** - MVPD-ID-Zeichenfolge
   - **2** - GUID (MD5-Hash)
   - **3** - Token bereits im Cache (true/false)
   - **4** - Gerätetyp
   - **5** - Access Enabler-Client-Typ
   - **6** - Betriebssystemtyp

- Für Ereignistyp `EVENT_AUTHZ_DETECTION`
   - **0** - Ob die Token-Anfrage erfolgreich war (true/false) und ob sie erfolgreich war:
   - **1** - MVPD ID
   - **2** - GUID (MD5-Hash)
   - **3** - Token bereits im Cache (true/false)
   - **4** - Fehler
   - **5** - Details
   - **6** - Gerätetyp
   - **7** - Zugriffs-Enabler-Client-Typ
   - **8** - Betriebssystemtyp

- Für Ereignistyp `EVENT_MVPD_SELECTION`
   - **0** - ID der aktuell ausgewählten MVPD
   - **1** - Gerätetyp
   - **2** - Zugriffs-Enabler-Client-Typ
   - **3** - Betriebssystemtyp

**Ausgelöst von:** `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`
