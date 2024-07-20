---
title: Amazon FireOS Native Client-API-Referenz
description: Amazon FireOS Native Client-API-Referenz
exl-id: 8ac9f976-fd6b-4b19-a80d-49bfe57134b5
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '3428'
ht-degree: 0%

---

# Amazon FireOS Native Client-API-Referenz {#amazon-fireos-native-client-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Einführung {#intro}

In diesem Dokument werden die Methoden und Rückrufe beschrieben, die vom Amazon FireOS-SDK für die Adobe Pass-Authentifizierung bereitgestellt werden, das mit der Adobe Pass-Authentifizierung unterstützt wird. Die hier beschriebenen Methoden und Callback-Funktionen sind in den Kopfzeilendateien AccessEnabler.h und EntitlementDelegate.h definiert.

Unter <https://tve.zendesk.com/hc/en-us/articles/115005561623-fire-TV-Native-AccessEnabler-Library> finden Sie das neueste Amazon FireOS AccessEnabler SDK.

>[!NOTE]
>
>Das Adobe Pass-Authentifizierungsteam empfiehlt Ihnen, nur Adobe Pass-Authentifizierungs-*öffentliche* APIs zu verwenden:

- Öffentliche APIs sind für alle unterstützten Client-Typen *verfügbar und vollständig getestet*. Für jede öffentliche Funktion stellen wir sicher, dass jeder Client-Typ über eine entsprechende Version der zugehörigen Methode(en) verfügt.
- Öffentliche APIs müssen so stabil wie möglich sein, um die Abwärtskompatibilität zu unterstützen und sicherzustellen, dass Partnerintegrationen nicht beschädigt werden. Bei *nicht* öffentlichen APIs behalten wir uns jedoch das Recht vor, ihre Signatur zu jedem späteren Zeitpunkt zu ändern. Wenn Sie auf einen bestimmten Fluss stoßen, der nicht durch eine Kombination der aktuellen öffentlichen Adobe Pass-Authentifizierungs-API-Aufrufe unterstützt werden kann, sollten Sie uns am besten Bescheid geben. Unter Berücksichtigung Ihrer Anforderungen können wir die öffentlichen APIs ändern und eine stabile Lösung für künftige Aufgaben bereitstellen.

## Amazon FireOS-SDK-API {#api}

- [getInstance](#getInstance)
- [setOptions](#fire_setOption)
- [setRequest](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
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

**Beschreibung:** Instanziiert das Objekt Access Enabler . Pro Anwendungsinstanz sollte eine einzige Access Enabler -Instanz vorhanden sein.

| API-Aufruf: Konstruktor |
| --- |
| ```public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException```<br><br> |
| ```public static AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl) throws AccessEnablerException``` |

**Verfügbarkeit:** v3.0+


**Parameter:**

- *appContext*: Amazon Fire OS-Anwendungskontext.
- softwareStatement
- redirectUrl : Bei FireOS wird der Parameterwert ignoriert und auf &quot;default&quot;festgelegt: adobepass://android.app
- env_url: Zum Testen mit der Adobe-Staging-Umgebung kann env\_url auf &quot;sp.auth-staging.adobe.com&quot;festgelegt werden.

**Veraltet:**

```java
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```


### setRequest {#setRequestor}

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Diese Einstellung sollte nur einmal während des Lebenszyklus der Anwendung vorgenommen werden.

Die Server-Antwort enthält eine Liste von MVPDs zusammen mit einigen Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Server-Antwort wird intern vom Access Enabler-Code verwendet. Nur der Status des Vorgangs (d. h. SUCCESS/FAIL) wird Ihrer Anwendung über den Rückruf setRequestorComplete() angezeigt.

Wenn der Parameter *urls* nicht verwendet wird, wird der resultierende Netzwerkaufruf auf die Standard-Service-Provider-URL ausgerichtet: die Adobe-Release-/Produktionsumgebung.

Wenn ein Wert für den Parameter *urls* angegeben wird, werden alle im Parameter *urls* angegebenen URLs vom resultierenden Netzwerkaufruf als Ziel ausgewählt. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Der erste Antwortsender hat beim Kompilieren der Liste der MVPDs Vorrang. Für jeden MVPD in der Liste speichert der Access Enabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die dem Dienstanbieter zugeordnet ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| ```public void setRequestor(String requestorId)``` |


**Verfügbarkeit:** v3.0+


| API-Aufruf: Konfiguration des Anforderers |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Verfügbarkeit:** v3.0+


**Parameter:**

- *requestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie die eindeutige ID, die von Adobe zugewiesen wurde, an Ihre Site, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
- *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.

**Ausgelöste Rückrufe:** `setRequestorComplete()`



**Veraltet:**

```
    public void setRequestor(String requestorId, String signedRequestorId)

    public void setRequestor(String requestorId, String signedRequestorId, ArrayList<String> urls)
```

</br>


### setRequestorComplete {#setRequestorComplete}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der Ihre Anwendung darüber informiert, dass die Konfigurationsphase abgeschlossen ist. Dies ist ein Signal, dass die App mit der Ausgabe von Berechtigungsanfragen beginnen kann. Bis zum Abschluss der Konfigurationsphase kann die Anwendung keine Berechtigungsanfragen mehr stellen.

| Callback: Konfiguration des Anforderers abgeschlossen |
| --- |
| ```public void setRequestorComplete(int status)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - Konfiguration
Phase erfolgreich abgeschlossen
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - Konfiguration
Phase fehlgeschlagen

**ausgelöst von:** `setRequestor()`

</br>


### setOptions {#fire_setOption}

**Beschreibung:** Konfiguriert globale SDK-Optionen. Es akzeptiert eine **Zuordnung\&lt;String, String\>** als Argument. Die Werte aus der Zuordnung werden zusammen mit jedem Netzwerkaufruf des SDK an den Server übergeben.

Die Werte werden unabhängig vom aktuellen Ablauf (Authentifizierung/Autorisierung) an den Server übergeben. Wenn Sie die Werte ändern möchten, können Sie diese Methode jederzeit aufrufen.



| API-Aufruf: setOptions |
| --- |
| ```public void setOptions(HashMap<String,String> options)``` |

**Verfügbarkeit:** v3.0+

**Parameter:**

- *options*: Eine Zuordnung\&lt;String, String\> mit globalen SDK-Optionen. Derzeit sind die folgenden Optionen verfügbar:
   - **applicationProfile** - Damit können Serverkonfigurationen auf Grundlage dieses Werts erstellt werden.
   - **ap\_vi** - Der Experience Cloud ID-Dienst. Dieser Wert kann später für erweiterte Analyseberichte verwendet werden.
   - **device\_info** - Geräteinformationen wie in **Weitergeben des Cookies für Geräteinformationen** beschrieben

</br>

### checkAuthentication {#checkAuthN}

**Beschreibung:** Überprüft den Authentifizierungsstatus. Dazu wird nach einem gültigen Authentifizierungstoken im lokalen Token-Speicher gesucht. Der Aufruf dieser Methode führt keine Netzwerkaufrufe durch. Sie wird von der Anwendung verwendet, um den Authentifizierungsstatus des Benutzers abzufragen und die Benutzeroberfläche entsprechend zu aktualisieren (d. h. die Anmelde-/Abmelde-Benutzeroberfläche zu aktualisieren). Der Authentifizierungsstatus wird der Anwendung über den Rückruf [*setAuthenticationStatus()*](#setAuthNStatus) übermittelt.

Wenn ein MVPD die Funktion &quot;Authentifizierung pro Anforderer&quot;unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden.

| API-Aufruf: Authentifizierungsstatus überprüfen |
| --- |
| ```public void checkAuthentication()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `setAuthenticationStatus()`

</br>

### getAuthentication {#getAuthN}

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Zunächst wird der Authentifizierungsstatus überprüft. Falls noch nicht authentifiziert, wird der Zustandsmaschine für den Authentifizierungsfluss gestartet:

- Wenn der letzte Authentifizierungsversuch erfolgreich war, wird die MVPD-Auswahlphase übersprungen und ein WebView-Steuerelement zeigt dem Benutzer die Anmeldeseite des MVPD an.
- Wenn der letzte Authentifizierungsversuch nicht erfolgreich war oder der Benutzer sich explizit abgemeldet hat, wird der Rückruf [*displayProviderDialog()*](#displayProviderDialog) ausgelöst. Ihre Anwendung verwendet diesen Rückruf, um die MVPD-Auswahlbenutzeroberfläche anzuzeigen. Außerdem muss Ihre App den Authentifizierungsfluss fortsetzen, indem sie die Access Enabler-Bibliothek über die Methode [setSelectedProvider()](#setSelectedProvider) über die MVPD-Auswahl des Benutzers informiert.

Wenn ein MVPD die Funktion &quot;Authentifizierung pro Anforderer&quot;unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden (einer pro Programmierer).

Schließlich wird der Authentifizierungsstatus über den Rückruf *setAuthenticationStatus()* an die Anwendung übermittelt.

| API-Aufruf: initiiert den Authentifizierungsfluss |
| --- |
| ```public void getAuthentication()``` |

**Verfügbarkeit:** v1.0+

| API-Aufruf: initiiert den Authentifizierungsfluss |
| --- |
| ```public void getAuthentication(boolean forceAuthN, Map<String, Object> genericData)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *forceAuthn*: Eine Markierung, die angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
- *data*: Eine Zuordnung, die aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `setAuthenticationStatus(), displayProviderDialog(), sendTrackingData()`

</br>

### displayProviderDialog {#displayProviderDialog}

**Beschreibung** Durch den Access Enabler ausgelöster Rückruf, um die Anwendung darüber zu informieren, dass die entsprechenden Elemente der Benutzeroberfläche instanziiert werden müssen, damit der Benutzer das gewünschte MVPD auswählen kann. Der Rückruf bietet eine Liste von MVPD-Objekten mit zusätzlichen Informationen, die dazu beitragen können, das Auswahlbenutzeroberflächenbedienfeld korrekt zu erstellen (z. B. die URL, die auf das MVPD-Logo verweist, der Anzeigename usw.).

Nachdem der Benutzer den gewünschten MVPD ausgewählt hat, muss die Anwendung auf der obersten Ebene den Authentifizierungsfluss fortsetzen, indem *setSelectedProvider()* aufgerufen und die Kennung des MVPD entsprechend der Auswahl des Benutzers übergeben wird.


| **Callback: Anzeigen der MVPD-Auswahlbenutzeroberfläche** |
| --- |
| ```public void displayProviderDialog(ArrayList<Mvpd> mvpds)``` |

**Verfügbarkeit:** v1.0+

**Parameter**:

- *mvpds*: Liste der MVPD-Objekte mit MVPD-bezogenen Informationen, die die Anwendung zum Erstellen der Elemente der MVPD-Auswahlbenutzeroberfläche verwenden kann.

**ausgelöst von:** `getAuthentication(), getAuthorization()`

</br>

### setSelectedProvider {#setSelectedProvider}

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Access Enabler über die MVPD-Auswahl des Benutzers zu informieren. Wenn *null* als Parameter übergeben wird, setzt der Access Enabler den aktuellen MVPD auf einen Nullwert zurück.

| **API-Aufruf: Legt den aktuell ausgewählten Provider fest** |
| --- |
| ```public void setSelectedProvider(String mvpdId)``` |


**Verfügbarkeit:**Version 1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `setAuthenticationStatus(), sendTrackingData()`
</br>

### navigateToUrl {#navigagteToUrl}

**Beschreibung:** Callback, der vom Access Enabler im Android SDK ausgelöst wird. Sie sollte beim Amazon FireOS-SDK ignoriert werden.

| **Callback: MVPD-Anmeldeseite anzeigen** |
| --- |
| ```public void navigateToUrl(String url)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *url*: Die URL, die auf die Anmeldeseite des MVPD verweist

**ausgelöst von:** `getAuthentication(), setSelectedProvider()`

</br>

### getAuthenticationToken {#getAuthNToken}

**Beschreibung:** Schließt den Authentifizierungsfluss ab, indem das Authentifizierungstoken vom Backend-Server angefordert wird.

| **API-Aufruf: Authentifizierungstoken abrufen** |
| --- |
| ```public void getAuthenticationToken(String cookies)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *Cookies*: Cookies, die in der Zieldomäne gesetzt werden (eine Referenzimplementierung finden Sie in der Demoanwendung im SDK ).

**Ausgelöste Rückrufe:** `setAuthenticationStatus(), sendTrackingData()`

</br>

### setAuthenticationStatus {#setAuthNStatus}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der die Anwendung über den Status der Authentifizierung informiert. Es gibt viele Stellen, an denen der Authentifizierungsfluss fehlschlagen kann, entweder aufgrund der Interaktion des Benutzers oder aufgrund anderer unvorhergesehener Szenarien (z. B. Probleme bei der Netzwerkverbindung usw.). Dieser Rückruf informiert die Anwendung über den Erfolgs-/Fehlerstatus der Authentifizierung und liefert bei Bedarf zusätzliche Informationen zum Fehlergrund.

Dieser Rückruf signalisiert auch, wenn der Abmeldefluss abgeschlossen ist.

| **Callback: Bericht zum Status des Authentifizierungsflusses** |
| --- |
| ```public void setAuthenticationStatus(int status, String errorCode)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - Authentifizierungsfluss wurde erfolgreich abgeschlossen
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - Authentifizierungsfluss fehlgeschlagen
   - `AccessEnabler.ACCESS_ENABLER_STATUS_LOGOUT` - logout
- *code*: Grund für den angezeigten Status. Wenn *status* `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` ist, ist *code* eine leere Zeichenfolge (d. h. durch die `AccessEnabler.USER_AUTHENTICATED` -Konstante definiert). Wenn dieser Parameter nicht authentifiziert ist, kann er einen der folgenden Werte annehmen:
   - `AccessEnabler.USER_NOT_AUTHENTICATED_ERROR` - Der Benutzer ist nicht authentifiziert. Als Antwort auf den Methodenaufruf *checkAuthentication()* , wenn im lokalen Token-Cache kein gültiges Authentifizierungstoken vorhanden ist.
   - `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` - Der AccessEnabler hat den Authentifizierungsstatus-Computer zurückgesetzt, nachdem die Anwendung der oberen Ebene *null* an `setSelectedProvider()` übergeben hat, um den Authentifizierungsfluss abzubrechen.  Vermutlich hat der Benutzer den Authentifizierungsfluss abgebrochen (d. h. die &quot;Zurück&quot;-Schaltfläche gedrückt).
   - `AccessEnabler.GENERIC_AUTHENTICATION_ERROR` - Der Authentifizierungsfluss schlug aus Gründen wie z. B. Nichtverfügbarkeit des Netzwerks fehl oder der Benutzer hat den Authentifizierungsfluss explizit abgebrochen.
   - `AccessEnabler.LOGOUT` - Der Benutzer wird aufgrund einer Abmeldeaktion nicht authentifiziert.

**ausgelöst von:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

</br>

### checkPreauthorizedResources {#checkPreauth}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der Hauptzweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden (z. B. zur Angabe des Zugriffs mit Schloss- und Entsperrungssymbolen).

| **API-Aufruf: Legt den aktuell ausgewählten Provider fest** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Verfügbarkeit:** v1.0+

**&lt;Parameter:** Der Parameter `resources` ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()` -Aufruf, d. h. es sollte sich um einen vereinbarten Wert handeln, der zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment festgelegt wurde.

**Callback ausgelöst:** `preauthorizedResources()`

</br>

### preauthorizedResources {#preauthResources}

**Beschreibung:** Callback, der von checkPreauthorizedResources() ausgelöst wird. Bietet eine Liste der Ressourcen, für die der Benutzer bereits autorisiert ist.

| **API-Aufruf: Legt den aktuell ausgewählten Provider fest** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Verfügbarkeit:**Version 1.0+

**Parameter:** Der Parameter `resources` ist ein Array von Ressourcen, für die der Benutzer bereits zur Anzeige berechtigt ist.

**ausgelöst von:** `checkPreauthorizedResources()`

<br>

### checkAuthorization {#checkAuthZ}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn sie nicht authentifiziert ist, wird der Rückruf *setTokenRequestFailed()* ausgelöst und die Methode wird beendet. Wenn der Benutzer authentifiziert ist, wird auch der Autorisierungsfluss Trigger. Siehe Details zur Methode *getAuthorization()* .

| **API-Aufruf: Autorisierungsstatus überprüfen** |
| --- |
| ```public void checkAuthorization(String resourceId)``` |

**Verfügbarkeit:** v1.0+

| **API-Aufruf: Autorisierungsstatus überprüfen** |
| --- |
| ```public void checkAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Zuordnung, die aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `tokenRequestFailed(), setToken(), sendTrackingData(), setAuthenticationStatus()`

</br>

### getAuthorization {#getAuthZ}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsfluss zu initiieren. Wenn der Benutzer noch nicht authentifiziert ist, wird auch der Authentifizierungsfluss initiiert. Wenn der Benutzer authentifiziert wird, stellt der Access Enabler weiter Anforderungen an das Autorisierungstoken (wenn im lokalen Token-Cache kein gültiges Autorisierungstoken vorhanden ist) und an das Token für kurzlebige Medien. Sobald das Short-Media-Token abgerufen wurde, wird der Autorisierungsfluss als vollständig betrachtet. Der Rückruf *setToken()* wird ausgelöst und das Short-Media-Token wird als Parameter an die Anwendung gesendet. Wenn die Autorisierung aus irgendeinem Grund fehlschlägt, wird der Rückruf *tokenRequestFailed()* ausgelöst und der Fehlercode und die Details werden bereitgestellt.

| **API-Aufruf: Initiieren des Autorisierungsflusses** |
| --- |
| ```public void getAuthorization(String resourceId)``` |

**Verfügbarkeit:** v1.0+

| **API-Aufruf: Initiieren des Autorisierungsflusses** |
| --- |
| ```public void getAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Zuordnung, die aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `tokenRequestFailed(), setToken(), sendTrackingData()`

|     |     |
| --- | --- |
| ![](http://learn.adobe.com/wiki/images/icons/emoticons/warning.gif) | **Zusätzliche ausgelöste Rückrufe** <br>Diese Methode kann auch die folgenden Rückrufe Trigger (wenn auch der Authentifizierungsfluss initiiert wird): _setAuthenticationStatus()_, _displayProviderDialog()_ |

**HINWEIS: Verwenden Sie möglichst checkAuthorization() anstelle von getAuthorization() . Die Methode getAuthorization() startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf der Seite des Programmierers führen könnte.**

</br>

### setToken {#setToken}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der Ihre Anwendung darüber informiert, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde. Das kurzlebige Medien-Token wird auch als Parameter bereitgestellt.

| **Callback: Autorisierungsfluss erfolgreich abgeschlossen** |
| --- |
| ```public void setToken(String token, String resourceId)``` |

**Verfügbarkeit:**Version 1.0+

**Parameter:**

- *token*: Das kurzlebige Medien-Token
- *resourceId*: Die Ressource, für die die Autorisierung abgerufen wurde

**ausgelöst von:** `checkAuthorization(), getAuthorization()`

<br>

### tokenRequestFailed {#tokenRequestFailed}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der die Anwendung auf der obersten Ebene darüber informiert, dass der Autorisierungsfluss fehlgeschlagen ist.

| **Callback: Autorisierungsfluss fehlgeschlagen** |
| --- |
| ```public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die Ressource, für die die Autorisierung abgerufen wurde
- *errorCode*: Fehler-Code, der dem Fehlerszenario zugeordnet ist. Mögliche Werte:
   - `AccessEnabler.USER_NOT_AUTHORIZED_ERROR` - Der Benutzer konnte für die jeweilige Ressource nicht autorisieren
- *errorDescription*: Zusätzliche Details zum Fehlerszenario. Wenn diese beschreibende Zeichenfolge aus irgendeinem Grund nicht verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge >**(&quot;&quot;)**.  Diese Zeichenfolge kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder umsatzbezogene Nachrichten zu übergeben. Wenn einem Abonnenten beispielsweise die Autorisierung für eine Ressource verweigert wird, könnte der MVPD eine Nachricht wie die folgende senden: &quot;Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie hier.&quot; Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Rückruf an den Programmierer übergeben, der die Möglichkeit hat, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann diesen Parameter auch verwenden, um eine Benachrichtigung über die Bedingung bereitzustellen, die möglicherweise zu einem Fehler geführt hat. Beispiel: &quot;Bei der Kommunikation mit dem Autorisierungsdienst des Providers ist ein Netzwerkfehler aufgetreten.&quot;

**ausgelöst von:** `checkAuthorization(), getAuthorization()`

</br>

### Abmelden {#logout}

**Beschreibung:** Verwenden Sie diese Methode, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Weiterleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss.

| **API-Aufruf: Logout-Fluss starten** |
| --- |
| ```public void logout()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** Keine

</br>

### getSelectedProvider {#getSelectedProvider}

**Beschreibung:** Verwenden Sie diese Methode, um den derzeit ausgewählten Anbieter zu bestimmen.

| **API-Aufruf: Bestimmen Sie den aktuell ausgewählten MVPD** |
| --- |
| ```public void getSelectedProvider()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `selectedProvider()`

</br>

### selectedProvider {#selectedProvider}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird und Informationen über das aktuell ausgewählte MVPD für die Anwendung bereitstellt.

| **Callback: Informationen zum derzeit ausgewählten MVPD** |
| --- |
| ```public void selectedProvider(Mvpd mvpd)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *mvpd*: Objekt mit Informationen zum derzeit ausgewählten MVPD

**ausgelöst von:** `getSelectedProvider()`

</br>

### getMetadata {#getMetadata}

**Beschreibung:** Verwenden Sie diese Methode, um Informationen abzurufen, die von der Access Enabler-Bibliothek als Metadaten bereitgestellt werden. Die Anwendung kann auf diese Informationen zugreifen, indem sie ein zusammengesetztes MetadataKey -Objekt bereitstellt.

| **API-Aufruf: Abfrage des AccessEnabler für Metadaten** |
| --- |
| ```public void getMetadata(MetadataKey metadataKey)``` |

**Verfügbarkeit:** v1.0+

Programmierern stehen zwei Metadatentypen zur Verfügung:

- Statische Metadaten (Authentifizierungstoken TTL, Autorisierungstoken TTL und Geräte-ID)
- Benutzermetadaten (benutzerspezifische Informationen wie Benutzer-ID und Postleitzahl; von einem MVPD an das Gerät eines Benutzers während der Authentifizierungs- und/oder Autorisierungsflüsse übergeben)

**Parameter:**

- *metadataKey*: Eine Datenstruktur, die eine Schlüssel- und args-Variable mit der folgenden Bedeutung enthält:
   - Wenn der Schlüssel `METADATA_KEY_TTL_AUTHN` ist, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.
   - Wenn der Schlüssel `METADATA_KEY_TTL_AUTHZ` ist und args ein SerializableNameValuePair -Objekt mit Name = `METADATA_ARG_RESOURCE_ID` und Wert = `[resource_id]` enthält, wird die Abfrage durchgeführt, um die Ablaufzeit des Autorisierungstokens zu erhalten, das mit der angegebenen Ressource verknüpft ist.
   - Wenn der Schlüssel `METADATA_KEY_DEVICE_ID` ist, wird die Abfrage durchgeführt, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen über Aktivierung und Gebühren zu erhalten.
   - Wenn der Schlüssel `METADATA_KEY_USER_META` ist und args ein SerializableNameValuePair -Objekt mit Name = `METADATA_KEY_USER_META` und Wert = `[metadata_name]` enthält, wird die Abfrage für Benutzermetadaten durchgeführt. Die aktuelle Liste der verfügbaren Benutzer-Metadatentypen:
      - `zip` - Postleitzahl
      - `householdID` - Kennung des Haushalts. Wenn ein MVPD keine Unterkonten unterstützt, ist dies identisch mit `userID`.
      - `maxRating` - Maximale elterliche Bewertung für den Benutzer
      - `userID` - Die Benutzer-ID. Wenn ein MVPD Subkonten unterstützt und der Benutzer nicht das Hauptkonto ist,
      - `channelID` - Eine Liste der Kanäle, die der Benutzer anzeigen darf

Die tatsächlichen Benutzermetadaten, die einem Programmierer zur Verfügung stehen, hängen davon ab, was ein MVPD zur Verfügung stellt.  Diese Liste wird weiter erweitert, sobald neue Metadaten verfügbar gemacht und dem Adobe Pass-Authentifizierungssystem hinzugefügt werden.

**Ausgelöste Rückrufe:** [`setMetadataStatus()`](#setMetadaStatus)

**Weitere Informationen:** [Benutzermetadaten](#setmetadatastatus)

</br>

### setMetadataStatus {#setMetadaStatus}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird und die über einen *getMetadata()* -Aufruf angeforderten Metadaten liefert.

| **Callback: Ergebnis der Metadaten-Abrufanforderung** |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *key*: Das MetadataKey-Objekt, das den Schlüssel enthält, für den der Metadatenwert angefordert wird, und die zugehörigen Parameter (siehe Demoanwendung für eine Referenzimplementierung).
- *result*: Ein zusammengesetztes Objekt, das die angeforderten Metadaten enthält. Das Objekt weist die folgenden Felder auf:
   - *simpleResult*: ein String, der den Metadatenwert darstellt, wenn die Anfrage für die Authentifizierungs-TTL, Autorisierungs-TTL oder Geräte-ID gestellt wurde. Dieser Wert ist null, wenn die Anforderung für Benutzermetadaten ausgeführt wurde.

   - *userMetadataResult*: ein Objekt, das die Java-Darstellung einer JSON User Metadata-Payload enthält. Beispiel:

     ```json
     {
     "street": "Main Avenue",
     "buildings": ["150", "320"]
     }
     ```

     wird in Java als übersetzt:

     ```java
     Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
     ```

     **Die tatsächliche Struktur der Benutzermetadatenobjekte ähnelt der folgenden:**

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


Dieser Wert ist null, wenn die Anforderung für einfache Metadaten (Authentifizierung TTL, Autorisierungs-TTL oder Geräte-ID) ausgeführt wurde.

- *verschlüsselt*: Boolescher Wert, der angibt, ob die abgerufenen Metadaten verschlüsselt sind. Dieser Parameter ist nur für Benutzer-Metadaten-Anfragen von Bedeutung. Er hat keine Bedeutung für statische Metadaten (z. B. Authentifizierungs-TTL), die immer unverschlüsselt empfangen werden. Wenn dieser Parameter auf &quot;True&quot;gesetzt ist, muss der Programmierer den Wert für die unverschlüsselten Benutzer-Metadaten abrufen, indem er eine RSA-Entschlüsselung mit dem privaten Schlüssel auf der Whitelist durchführt (der private Schlüssel, der für die Signierung der Anforderer-ID im [`setRequestor`](#setRequestor) -Aufruf verwendet wird).

**ausgelöst von:** [`getMetadata()`](#getMetadata)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/user-metadata.md)

</br>

## getVersion {#getVersion}

**Beschreibung:** Verwenden Sie diese Methode, um die aktuelle Version von AccessEnabler abzurufen.

| **API-Aufruf: get AccessEnabler version** |
| --- |
| ```public static String getVersion()``` |

## Ereignisse verfolgen {#tracking}

Der Access Enabler -Trigger verfügt über einen zusätzlichen Callback, der nicht unbedingt mit den Berechtigungsflüssen in Zusammenhang steht. Die Implementierung der Rückruffunktion &quot;event-tracking&quot; mit dem Namen *sendTrackingData()* ist optional. Sie ermöglicht es der Anwendung jedoch, bestimmte Ereignisse zu verfolgen und Statistiken wie die Anzahl erfolgreicher/fehlgeschlagener Authentifizierungs-/Autorisierungsversuche zu kompilieren. Nachstehend finden Sie die Spezifikation für den Rückruf *sendTrackingData()* :

### sendTrackingData {#sendTrackingData}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der der Anwendung das Auftreten verschiedener Ereignisse wie dem Fertigstellen/Fehlschlagen von Authentifizierungs-/Autorisierungsflüssen signalisiert. Der Gerätetyp, der Zugriffs-Enabler-Client-Typ und das Betriebssystem werden ebenfalls von sendTrackingData() gemeldet.

>[!WARNING]
>
> Gerätetyp und Betriebssystem werden durch die Verwendung einer öffentlichen Java-Bibliothek (http://java.net/projects/user-agent-utils) und der Benutzeragenten-Zeichenfolge abgeleitet. Beachten Sie, dass diese Informationen nur als grobe Methode zur Unterteilung von Betriebsmetriken in Gerätekategorien bereitgestellt werden, dass Adobe jedoch keine Verantwortung für fehlerhafte Ergebnisse übernehmen kann. Verwenden Sie die neue Funktion entsprechend.

- Mögliche Werte für den Gerätetyp:
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`

- Mögliche Werte für den Client-Typ Access Enabler :
   - `flash`
   - `html5`
   - `ios`
   - `tvos`
   - `android`
   - `firetv`

| Callback: Tracking-Ereignisse |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *event*: Das Ereignis, das verfolgt wird. Es gibt drei mögliche Tracking-Ereignistypen:
   - **authorizationDetection:** jedes Mal, wenn eine Autorisierungstoken-Anfrage zurückgegeben wird (Ereignistyp ist `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** jedes Mal, wenn eine Authentifizierungsprüfung erfolgt (Ereignistyp ist `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:**, wenn der Benutzer im MVPD-Auswahlformular einen MVPD auswählt (Ereignistyp ist `EVENT_MVPD_SELECTION`)
- *data*: zusätzliche Daten, die mit dem gemeldeten Ereignis verknüpft sind. Diese Daten werden in Form einer Werteliste dargestellt.

Im Folgenden finden Sie Anweisungen zur Interpretation der Werte im Array *data* :

- Für Ereignistyp *`EVENT_AUTHN_DETECTION`:*
   - **0** - Gibt an, ob die Token-Anfrage erfolgreich war (true/false) und ob die obige wahr ist:
   - **1** - MVPD-ID-Zeichenfolge
   - **2** - GUID (md5 gehasht)
   - **3** - Token, das sich bereits im Cache befindet (true/false)
   - **4** - Gerätetyp
   - **5** - Zugriffs-Enabler-Client-Typ
   - **6** - Betriebssystemtyp

- Für Ereignistyp `EVENT_AUTHZ_DETECTION`
   - **0** - Gibt an, ob die Token-Anfrage erfolgreich war (true/false) und ob sie erfolgreich war:
   - **1** - MVPD-ID
   - **2** - GUID (md5 gehasht)
   - **3** - Token, das sich bereits im Cache befindet (true/false)
   - **4** - Fehler
   - **5** - Details
   - **6** - Gerätetyp
   - **7** - Zugriffs-Enabler-Client-Typ
   - **8** - Betriebssystemtyp

- Für Ereignistyp `EVENT_MVPD_SELECTION`
   - **0** - ID des derzeit ausgewählten MVPD
   - **1** - Gerätetyp
   - **2** - Zugriffs-Enabler-Client-Typ
   - **3** - Betriebssystemtyp

**ausgelöst von:** `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`
