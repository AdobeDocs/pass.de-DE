---
title: Amazon FireOS Native Client-API-Referenz
description: Amazon FireOS Native Client-API-Referenz
exl-id: 8ac9f976-fd6b-4b19-a80d-49bfe57134b5
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '3416'
ht-degree: 0%

---

# Amazon FireOS Native Client-API-Referenz {#amazon-fireos-native-client-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Einführung {#intro}

In diesem Dokument werden die Methoden und Rückrufe beschrieben, die vom Amazon FireOS-SDK für die Adobe Pass-Authentifizierung bereitgestellt werden, das mit der Adobe Pass-Authentifizierung unterstützt wird. Die hier beschriebenen Methoden und Callback-Funktionen sind in den Kopfzeilendateien AccessEnabler.h und EntitlementDelegate.h definiert.

Siehe <https://tve.zendesk.com/hc/en-us/articles/115005561623-fire-TV-Native-AccessEnabler-Library> für das neueste Amazon FireOS AccessEnabler SDK.

>[!NOTE]
>
>Das Adobe Pass-Authentifizierungsteam empfiehlt Ihnen die Verwendung von Adobe Pass-Authentifizierung *öffentlich* APIs:

- Öffentliche APIs sind verfügbar *und vollständig getestet* auf allen unterstützten Client-Typen. Für jede öffentliche Funktion stellen wir sicher, dass jeder Client-Typ über eine entsprechende Version der zugehörigen Methode(en) verfügt.
- Öffentliche APIs müssen so stabil wie möglich sein, um die Abwärtskompatibilität zu unterstützen und sicherzustellen, dass Partnerintegrationen nicht beschädigt werden. Allerdings gilt Folgendes: *non*-öffentlichen APIs, behalten wir uns das Recht vor, ihre Signatur jederzeit zu ändern. Wenn Sie auf einen bestimmten Fluss stoßen, der nicht durch eine Kombination der aktuellen öffentlichen Adobe Pass-Authentifizierungs-API-Aufrufe unterstützt werden kann, sollten Sie uns am besten Bescheid geben. Unter Berücksichtigung Ihrer Anforderungen können wir die öffentlichen APIs ändern und eine stabile Lösung für künftige Aufgaben bereitstellen.

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

Wenn die Variable *urls* nicht verwendet wird, zielt der resultierende Netzwerkaufruf auf die Standard-Service-Provider-URL ab: die Adobe-Release-/Produktionsumgebung.

Wenn ein Wert für *urls* festgelegt ist, werden alle in der Variablen *urls* -Parameter. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Der erste Antwortsender hat beim Kompilieren der Liste der MVPDs Vorrang. Für jeden MVPD in der Liste speichert der Access Enabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die dem Dienstanbieter zugeordnet ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| ```public void setRequestor(String requestorId)``` |


**Verfügbarkeit:** v3.0+


| API-Aufruf: Konfiguration des Anforderers |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Verfügbarkeit:** v3.0+


**Parameter:**

- *requestorID*: Die eindeutige ID, die mit dem Programmierer verknüpft ist. Übergeben Sie die eindeutige ID, die von Adobe zugewiesen wurde, an Ihre Site, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
- *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.

**Ausgelöste Rückrufe:** `setRequestorComplete()`



**Veraltet:**

```
    public void setRequestor(String requestorId, String signedRequestorId)

    public void setRequestor(String requestorId, String signedRequestorId, ArrayList<String> urls)
```

</br>


### setRequestorComplete {#setRequestorComplete}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der Ihre Anwendung über den Abschluss der Konfigurationsphase informiert. Dies ist ein Signal, dass die App mit der Ausgabe von Berechtigungsanfragen beginnen kann. Bis zum Abschluss der Konfigurationsphase kann die Anwendung keine Berechtigungsanfragen mehr stellen.

| Callback: Konfiguration des Anforderers abgeschlossen |
| --- |
| ```public void setRequestorComplete(int status)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - Die Konfigurationsphase wurde erfolgreich abgeschlossen.
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - Konfigurationsphase fehlgeschlagen

**Ausgelöst von:** `setRequestor()`

</br>


### setOptions {#fire_setOption}

**Beschreibung:** Konfiguriert globale SDK-Optionen. Es akzeptiert eine **Zuordnung\&lt;string string=&quot;&quot;>** als Argument. Die Werte aus der Zuordnung werden zusammen mit jedem Netzwerkaufruf des SDK an den Server übergeben.

Die Werte werden unabhängig vom aktuellen Ablauf (Authentifizierung/Autorisierung) an den Server übergeben. Wenn Sie die Werte ändern möchten, können Sie diese Methode jederzeit aufrufen.



| API-Aufruf: setOptions |
| --- |
| ```public void setOptions(HashMap<String,String> options)``` |

**Verfügbarkeit:** v3.0+

**Parameter:**

- *options*: Eine Zuordnung\&lt;string string=&quot;&quot;> enthält globale SDK-Optionen. Derzeit sind die folgenden Optionen verfügbar:
   - **applicationProfile** - Sie kann verwendet werden, um Serverkonfigurationen auf Grundlage dieses Werts vorzunehmen.
   - **ap\_vi** - Der Experience Cloud ID-Dienst. Dieser Wert kann später für erweiterte Analyseberichte verwendet werden.
   - **device\_info** - Geräteinformationen, wie unter **Weitergeben des Cookies für Geräteinformationen**

</br>

### checkAuthentication {#checkAuthN}

**Beschreibung:** Überprüft den Authentifizierungsstatus. Dazu wird nach einem gültigen Authentifizierungstoken im lokalen Token-Speicher gesucht. Der Aufruf dieser Methode führt keine Netzwerkaufrufe durch. Sie wird von der Anwendung verwendet, um den Authentifizierungsstatus des Benutzers abzufragen und die Benutzeroberfläche entsprechend zu aktualisieren (d. h. die Anmelde-/Abmelde-Benutzeroberfläche zu aktualisieren). Der Authentifizierungsstatus wird der Anwendung über das [*setAuthenticationStatus()*](#setAuthNStatus) Callback.

Wenn ein MVPD die Funktion &quot;Authentifizierung pro Anforderer&quot;unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden.

| API-Aufruf: Authentifizierungsstatus überprüfen |
| --- |
| ```public void checkAuthentication()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** Keines

**Ausgelöste Rückrufe:** `setAuthenticationStatus()`

</br>

### getAuthentication {#getAuthN}

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Zunächst wird der Authentifizierungsstatus überprüft. Falls noch nicht authentifiziert, wird der Zustandsmaschine für den Authentifizierungsfluss gestartet:

- Wenn der letzte Authentifizierungsversuch erfolgreich war, wird die MVPD-Auswahlphase übersprungen und ein WebView-Steuerelement zeigt dem Benutzer die Anmeldeseite des MVPD an.
- Wenn der letzte Authentifizierungsversuch nicht erfolgreich war oder der Benutzer sich explizit abgemeldet hat, wird die [*displayProviderDialog()*](#displayProviderDialog) Callback wird ausgelöst. Ihre Anwendung verwendet diesen Rückruf, um die MVPD-Auswahlbenutzeroberfläche anzuzeigen. Außerdem muss Ihre App den Authentifizierungsfluss fortsetzen, indem sie die Access Enabler-Bibliothek über die MVPD-Auswahl des Benutzers über die [setSelectedProvider()](#setSelectedProvider) -Methode.

Wenn ein MVPD die Funktion &quot;Authentifizierung pro Anforderer&quot;unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden (einer pro Programmierer).

Schließlich wird der Authentifizierungsstatus über die *setAuthenticationStatus()* Callback.

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
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Dienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `setAuthenticationStatus(), displayProviderDialog(), sendTrackingData()`

</br>

### displayProviderDialog {#displayProviderDialog}

**Beschreibung** Durch den Access Enabler ausgelöster Rückruf, um die Anwendung darüber zu informieren, dass die entsprechenden UI-Elemente instanziiert werden müssen, damit der Benutzer das gewünschte MVPD auswählen kann. Der Rückruf bietet eine Liste von MVPD-Objekten mit zusätzlichen Informationen, die dazu beitragen können, das Auswahlbenutzeroberflächenbedienfeld korrekt zu erstellen (z. B. die URL, die auf das MVPD-Logo verweist, der Anzeigename usw.).

Nachdem der Benutzer den gewünschten MVPD ausgewählt hat, muss die Anwendung auf der obersten Ebene den Authentifizierungsfluss durch Aufruf von *setSelectedProvider()* und übergeben die Kennung des MVPD entsprechend der Auswahl des Benutzers.


| **Callback: Anzeigen der MVPD-Auswahlbenutzeroberfläche** |
| --- |
| ```public void displayProviderDialog(ArrayList<Mvpd> mvpds)``` |

**Verfügbarkeit:** v1.0+

**Parameter**:

- *mvpds*: Liste der MVPD-Objekte, die MVPD-bezogene Informationen enthalten, die die Anwendung zum Erstellen der Elemente der MVPD-Auswahlbenutzeroberfläche verwenden kann.

**Ausgelöst von:** `getAuthentication(), getAuthorization()`

</br>

### setSelectedProvider {#setSelectedProvider}

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Access Enabler über die MVPD-Auswahl des Benutzers zu informieren. Beim Übergeben *null* als Parameter verwendet, setzt der Access Enabler den aktuellen MVPD auf einen Nullwert zurück.

| **API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest.** |
| --- |
| ```public void setSelectedProvider(String mvpdId)``` |


**Verfügbarkeit:**Version 1.0+

**Parameter:** Keines

**Ausgelöste Rückrufe:** `setAuthenticationStatus(), sendTrackingData()`
</br>

### navigateToUrl {#navigagteToUrl}

**Beschreibung:** Callback, der vom Access Enabler auf Android SDK ausgelöst wird. Sie sollte beim Amazon FireOS-SDK ignoriert werden.

| **Callback: MVPD-Anmeldeseite anzeigen** |
| --- |
| ```public void navigateToUrl(String url)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *url*: Die URL, die auf die Anmeldeseite des MVPD verweist

**Ausgelöst von:** `getAuthentication(), setSelectedProvider()`

</br>

### getAuthenticationToken {#getAuthNToken}

**Beschreibung:** Schließt den Authentifizierungsfluss durch Anfordern des Authentifizierungstokens vom Backend-Server ab.

| **API-Aufruf: Authentifizierungstoken abrufen** |
| --- |
| ```public void getAuthenticationToken(String cookies)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *Cookies*: Cookies, die in der Zieldomäne gesetzt sind (eine Referenzimplementierung finden Sie in der Demoanwendung im SDK ).

**Ausgelöste Rückrufe:** `setAuthenticationStatus(), sendTrackingData()`

</br>

### setAuthenticationStatus {#setAuthNStatus}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der die Anwendung über den Status der Authentifizierung informiert. Es gibt viele Stellen, an denen der Authentifizierungsfluss fehlschlagen kann, entweder aufgrund der Interaktion des Benutzers oder aufgrund anderer unvorhergesehener Szenarien (z. B. Probleme bei der Netzwerkverbindung usw.). Dieser Rückruf informiert die Anwendung über den Erfolgs-/Fehlerstatus der Authentifizierung und liefert bei Bedarf zusätzliche Informationen zum Fehlergrund.

Dieser Rückruf signalisiert auch, wenn der Abmeldefluss abgeschlossen ist.

| **Callback: Melden Sie den Status des Authentifizierungsflusses** |
| --- |
| ```public void setAuthenticationStatus(int status, String errorCode)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - Der Authentifizierungsfluss wurde erfolgreich abgeschlossen.
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - Authentifizierungsfluss fehlgeschlagen
   - `AccessEnabler.ACCESS_ENABLER_STATUS_LOGOUT` - Abmelden
- *code*: Grund für den angezeigten Status. Wenn *status* is `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS`, dann *code* ist eine leere Zeichenfolge (d. h., die durch die Variable `AccessEnabler.USER_AUTHENTICATED` -Konstante). Wenn dieser Parameter nicht authentifiziert ist, kann er einen der folgenden Werte annehmen:
   - `AccessEnabler.USER_NOT_AUTHENTICATED_ERROR` - Der Benutzer ist nicht authentifiziert. Als Antwort auf die *checkAuthentication()* Methodenaufruf, wenn kein gültiges Authentifizierungstoken im lokalen Token-Cache vorhanden ist.
   - `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` - Der AccessEnabler hat den Authentifizierungsstatus-Computer zurückgesetzt, nachdem die Anwendung der oberen Ebene übergeben wurde. *null* nach `setSelectedProvider()` , um den Authentifizierungsfluss abzubrechen.  Vermutlich hat der Benutzer den Authentifizierungsfluss abgebrochen (d. h. die &quot;Zurück&quot;-Schaltfläche gedrückt).
   - `AccessEnabler.GENERIC_AUTHENTICATION_ERROR` - Der Authentifizierungsfluss schlug aus Gründen wie z. B. Nichtverfügbarkeit des Netzwerks fehl oder der Benutzer hat den Authentifizierungsfluss explizit abgebrochen.
   - `AccessEnabler.LOGOUT` - Der Benutzer wird aufgrund einer Abmeldeaktion nicht authentifiziert.

**Ausgelöst von:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

</br>

### checkPreauthorizedResources {#checkPreauth}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der Hauptzweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden (z. B. zur Angabe des Zugriffs mit Schloss- und Entsperrungssymbolen).

| **API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest.** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Verfügbarkeit:** v1.0+

**&lt;parameters: span=&quot;&quot; id=&quot;1&quot; translate=&quot;no&quot; /> Die `resources` -Parameter ist ein Array von Ressourcen, für die die Autorisierung überprüft werden sollte.** Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt den gleichen Einschränkungen wie die Ressourcen-ID in der `getAuthorization()` -Aufruf, d. h., es sollte sich um einen zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment vereinbarten Wert handeln.

**Callback ausgelöst:** `preauthorizedResources()`

</br>

### preauthorizedResources {#preauthResources}

**Beschreibung:** Callback ausgelöst durch checkPreauthorizedResources(). Bietet eine Liste der Ressourcen, für die der Benutzer bereits autorisiert ist.

| **API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest.** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Verfügbarkeit:**Version 1.0+

**Parameter:** Die `resources` -Parameter ist ein Array von Ressourcen, für die der Benutzer bereits zur Anzeige berechtigt ist.

**Ausgelöst von:** `checkPreauthorizedResources()`

<br>

### checkAuthorization {#checkAuthZ}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn nicht authentifiziert, wird die *setTokenRequestFailed()* -Rückruf ausgelöst und die -Methode beendet. Wenn der Benutzer authentifiziert ist, wird auch der Autorisierungsfluss Trigger. Siehe Details zu den *getAuthorization()* -Methode.

| **API-Aufruf: Überprüfen des Autorisierungsstatus** |
| --- |
| ```public void checkAuthorization(String resourceId)``` |

**Verfügbarkeit:** v1.0+

| **API-Aufruf: Überprüfen des Autorisierungsstatus** |
| --- |
| ```public void checkAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Dienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `tokenRequestFailed(), setToken(), sendTrackingData(), setAuthenticationStatus()`

</br>

### getAuthorization {#getAuthZ}

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsfluss zu initiieren. Wenn der Benutzer noch nicht authentifiziert ist, wird auch der Authentifizierungsfluss initiiert. Wenn der Benutzer authentifiziert wird, stellt der Access Enabler weiter Anforderungen an das Autorisierungstoken (wenn im lokalen Token-Cache kein gültiges Autorisierungstoken vorhanden ist) und an das Token für kurzlebige Medien. Sobald das Short-Media-Token abgerufen wurde, wird der Autorisierungsfluss als vollständig betrachtet. Die *setToken()* Callback wird ausgelöst und das Short-Media-Token wird als Parameter an die Anwendung gesendet. Wenn die Genehmigung aus irgendeinem Grund fehlschlägt, wird die *tokenRequestFailed()* -Rückruf ausgelöst und der Fehlercode und die Details bereitgestellt werden.

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
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Dienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `tokenRequestFailed(), setToken(), sendTrackingData()`

|     |     |
| --- | --- |
| ![](http://learn.adobe.com/wiki/images/icons/emoticons/warning.gif) | **Zusätzliche ausgelöste Rückrufe**  <br>Diese Methode kann auch die folgenden Rückrufe Trigger werden (wenn der Authentifizierungsfluss ebenfalls initiiert wird): _setAuthenticationStatus()_, _displayProviderDialog()_ |

**HINWEIS: Verwenden Sie möglichst checkAuthorization() anstelle von getAuthorization() . Die Methode getAuthorization() startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf der Seite des Programmierers führen kann.**

</br>

### setToken {#setToken}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der Ihre Anwendung darüber informiert, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde. Das kurzlebige Medien-Token wird auch als Parameter bereitgestellt.

| **Callback: Autorisierungsfluss erfolgreich abgeschlossen** |
| --- |
| ```public void setToken(String token, String resourceId)``` |

**Verfügbarkeit:**Version 1.0+

**Parameter:**

- *token*: Das Medien-Token mit kurzer Lebensdauer
- *resourceId*: Die Ressource, für die die Autorisierung erfolgt ist

**Ausgelöst von:** `checkAuthorization(), getAuthorization()`

<br>

### tokenRequestFailed {#tokenRequestFailed}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der die Anwendung auf der oberen Ebene darüber informiert, dass der Autorisierungsfluss fehlgeschlagen ist.

| **Callback: Autorisierungsfluss fehlgeschlagen** |
| --- |
| ```public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die Ressource, für die die Autorisierung erfolgt ist
- *errorCode*: Fehler-Code, der mit dem Fehlerszenario verknüpft ist. Mögliche Werte:
   - `AccessEnabler.USER_NOT_AUTHORIZED_ERROR` - Der Benutzer konnte für die jeweilige Ressource nicht autorisieren
- *errorDescription*: Zusätzliche Details zum Fehlerszenario. Wenn diese beschreibende Zeichenfolge aus keinem Grund verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge >**(&quot;&quot;)**.  Diese Zeichenfolge kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder umsatzbezogene Nachrichten zu übergeben. Wenn einem Abonnenten beispielsweise die Autorisierung für eine Ressource verweigert wird, könnte der MVPD eine Nachricht wie die folgende senden: &quot;Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie hier.&quot; Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Rückruf an den Programmierer übergeben, der die Möglichkeit hat, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann diesen Parameter auch verwenden, um eine Benachrichtigung über die Bedingung bereitzustellen, die möglicherweise zu einem Fehler geführt hat. Beispiel: &quot;Bei der Kommunikation mit dem Autorisierungsdienst des Providers ist ein Netzwerkfehler aufgetreten.&quot;

**Ausgelöst von:** `checkAuthorization(), getAuthorization()`

</br>

### Abmelden {#logout}

**Beschreibung:** Verwenden Sie diese Methode, um den Abmeldefluss zu starten. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Weiterleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss.

| **API-Aufruf: Logout-Fluss initiieren** |
| --- |
| ```public void logout()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** Keines

**Ausgelöste Rückrufe:** Keines

</br>

### getSelectedProvider {#getSelectedProvider}

**Beschreibung:** Verwenden Sie diese Methode, um den aktuell ausgewählten Anbieter zu bestimmen.

| **API-Aufruf: Bestimmen Sie den aktuell ausgewählten MVPD** |
| --- |
| ```public void getSelectedProvider()``` |

**Verfügbarkeit:** v1.0+

**Parameter:** Keines

**Ausgelöste Rückrufe:** `selectedProvider()`

</br>

### selectedProvider {#selectedProvider}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der Informationen über das aktuell ausgewählte MVPD für die Anwendung bereitstellt.

| **Callback: Informationen zum aktuell ausgewählten MVPD** |
| --- |
| ```public void selectedProvider(Mvpd mvpd)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *mvpd*: Objekt mit Informationen zum aktuell ausgewählten MVPD

**Ausgelöst von:** `getSelectedProvider()`

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
   - Wenn der Schlüssel `METADATA_KEY_TTL_AUTHN` dann wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.
   - Wenn der Schlüssel `METADATA_KEY_TTL_AUTHZ` und args enthält ein SerializableNameValuePair -Objekt mit name = `METADATA_ARG_RESOURCE_ID` und Wert = `[resource_id]`, wird die Abfrage ausgeführt, um die Ablaufzeit des Autorisierungstokens zu erhalten, das mit der angegebenen Ressource verknüpft ist.
   - Wenn der Schlüssel `METADATA_KEY_DEVICE_ID` dann erfolgt die Abfrage, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen über Aktivierung und Gebühren zu erhalten.
   - Wenn der Schlüssel `METADATA_KEY_USER_META` und args enthält ein SerializableNameValuePair -Objekt mit name = `METADATA_KEY_USER_META` und Wert = `[metadata_name]`, wird die Abfrage für Benutzermetadaten durchgeführt. Die aktuelle Liste der verfügbaren Benutzer-Metadatentypen:
      - `zip` - Postleitzahl
      - `householdID` - Haushaltskennung. Wenn ein MVPD keine Unterkonten unterstützt, ist dies identisch mit `userID`.
      - `maxRating` - Maximale elterliche Bewertung für den Benutzer
      - `userID` - Die Benutzer-ID. Wenn ein MVPD Subkonten unterstützt und der Benutzer nicht das Hauptkonto ist,
      - `channelID` - Eine Liste der Kanäle, die der Benutzer anzeigen darf

Die tatsächlichen Benutzermetadaten, die einem Programmierer zur Verfügung stehen, hängen davon ab, was ein MVPD zur Verfügung stellt.  Diese Liste wird weiter erweitert, sobald neue Metadaten verfügbar gemacht und dem Adobe Pass-Authentifizierungssystem hinzugefügt werden.

**Ausgelöste Rückrufe:** [`setMetadataStatus()`](#setMetadaStatus)

**Weitere Informationen:** [Benutzermetadaten](#setmetadatastatus)

</br>

### setMetadataStatus {#setMetadaStatus}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der die über einen *getMetadata()* aufrufen.

| **Rückruf: Ergebnis der Metadaten-Abrufanforderung** |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *key*: Das MetadataKey -Objekt, das den Schlüssel enthält, für den ein Metadatenwert angefordert wird, und die zugehörigen Parameter (siehe Demoanwendung für eine Referenzimplementierung).
- *result*: Ein zusammengesetztes Objekt, das die angeforderten Metadaten enthält. Das Objekt weist die folgenden Felder auf:
   - *simpleResult*: ein String, der den Metadatenwert darstellt, wenn die Anfrage für die Authentifizierung TTL, Autorisierungs-TTL oder Geräte-ID gestellt wurde. Dieser Wert ist null, wenn die Anforderung für Benutzermetadaten ausgeführt wurde.

   - *userMetadataResult*: ein Objekt, das die Java-Darstellung einer JSON-Benutzer-Metadaten-Payload enthält. Beispiel:

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


Dieser Wert ist null, wenn die Anforderung für einfache Metadaten (Authentifizierung TTL, Autorisierungs-TTL oder Geräte-ID) ausgeführt wurde.

- *verschlüsselt*: Boolescher Wert, der angibt, ob die abgerufenen Metadaten verschlüsselt sind. Dieser Parameter ist nur für Benutzer-Metadaten-Anfragen von Bedeutung. Er hat keine Bedeutung für statische Metadaten (z. B. Authentifizierungs-TTL), die immer unverschlüsselt empfangen werden. Wenn dieser Parameter auf True festgelegt ist, liegt es am Programmierer, den unverschlüsselten Benutzer-Metadatenwert abzurufen, indem er eine RSA-Entschlüsselung mithilfe des auf die Whitelist gesetzten privaten Schlüssels durchführt (derselbe private Schlüssel, der zum Signieren der Anforderer-ID in der [`setRequestor`](#setRequestor) -Aufruf).

**Ausgelöst von:** [`getMetadata()`](#getMetadata)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/user-metadata.md)

</br>

## getVersion {#getVersion}

**Beschreibung:** Verwenden Sie diese Methode, um die aktuelle Version von AccessEnabler abzurufen

| **API-Aufruf: get AccessEnabler version** |
| --- |
| ```public static String getVersion()``` |

## Ereignisse verfolgen {#tracking}

Der Access Enabler -Trigger verfügt über einen zusätzlichen Callback, der nicht unbedingt mit den Berechtigungsflüssen in Zusammenhang steht. Implementieren der Rückruffunktion &quot;event-tracking&quot; mit dem Namen *sendTrackingData()* ist optional, ermöglicht es der Anwendung jedoch, bestimmte Ereignisse zu verfolgen und Statistiken wie die Anzahl erfolgreicher/fehlgeschlagener Authentifizierungs-/Autorisierungsversuche zu erstellen. Nachstehend finden Sie die Spezifikation für die *sendTrackingData()* callback:

### sendTrackingData {#sendTrackingData}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der der Anwendung das Auftreten verschiedener Ereignisse signalisiert, z. B. das Fertigstellen/Fehlschlagen von Authentifizierungs-/Autorisierungsflüssen. Der Gerätetyp, der Zugriffs-Enabler-Client-Typ und das Betriebssystem werden ebenfalls von sendTrackingData() gemeldet.

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

- *event*: das Ereignis, das verfolgt wird. Es gibt drei mögliche Tracking-Ereignistypen:
   - **authorizationDetection:** jedes Mal, wenn eine Autorisierungstoken-Anfrage zurückgegeben wird (Ereignistyp ist `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** jedes Mal, wenn eine Authentifizierungsprüfung erfolgt (Ereignistyp `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** wenn der Benutzer einen MVPD im MVPD-Auswahlformular auswählt (Ereignistyp ist `EVENT_MVPD_SELECTION`)
- *data*: zusätzliche Daten, die mit dem gemeldeten Ereignis verknüpft sind. Diese Daten werden in Form einer Werteliste dargestellt.

Im Folgenden finden Sie Anweisungen zur Interpretation der Werte in der *data* array:

- Für Ereignistyp *`EVENT_AUTHN_DETECTION`:*
   - **0** - Gibt an, ob die Token-Anfrage erfolgreich war (true/false) und ob die obige wahr ist:
   - **1** - MVPD ID-Zeichenfolge
   - **2** - GUID (md5-Hash)
   - **3** - Token bereits im Cache (true/false)
   - **4** - Gerätetyp
   - **5** - Access Enabler Client Type
   - **6** - Betriebssystemtyp

- Für Ereignistyp `EVENT_AUTHZ_DETECTION`
   - **0** - Ob die Token-Anfrage erfolgreich war (true/false) und ob erfolgreich:
   - **1** - MVPD ID
   - **2** - GUID (md5-Hash)
   - **3** - Token bereits im Cache (true/false)
   - **4** - Fehler
   - **5** - Details
   - **6** - Gerätetyp
   - **7** - Access Enabler Client Type
   - **8** - Betriebssystemtyp

- Für Ereignistyp `EVENT_MVPD_SELECTION`
   - **0** - Kennung des derzeit ausgewählten MVPD
   - **1** - Gerätetyp
   - **2** - Access Enabler Client Type
   - **3** - Betriebssystemtyp

**Ausgelöst von:** `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`
