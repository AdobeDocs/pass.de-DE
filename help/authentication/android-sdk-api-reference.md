---
title: Android SDK-API-Referenz
description: Android SDK-API-Referenz
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: 854698397d9d14c1bfddcc10eecc61c7e3c32b71
workflow-type: tm+mt
source-wordcount: '4537'
ht-degree: 0%

---

# Android SDK-API-Referenz {#android-sdk-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#intro}

In diesem Dokument werden die Methoden und Rückrufe beschrieben, die vom Android SDK für die Adobe Pass-Authentifizierung bereitgestellt werden, das mit Adobe Pass-Authentifizierungsversionen 1.7 und höher unterstützt wird. Die hier beschriebenen Methoden und Callback-Funktionen sind in den Kopfzeilendateien AccessEnabler.h und EntitlementDelegate.h definiert.

Das neueste Android AccessEnabler SDK finden Sie unter [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) .


**Hinweis:** Das Adobe Pass-Authentifizierungsteam empfiehlt Ihnen, nur Adobe Pass-Authentifizierungs-*öffentlichen* -APIs zu verwenden:

- Öffentliche APIs sind für alle unterstützten Client-Typen *verfügbar und vollständig getestet*. Für jede öffentliche Funktion stellen wir sicher, dass jeder Client-Typ über eine entsprechende Version der zugehörigen Methode(en) verfügt.</span>
- Öffentliche APIs müssen so stabil wie möglich sein, um die Abwärtskompatibilität zu unterstützen und sicherzustellen, dass Partnerintegrationen nicht beschädigt werden. Bei *nicht* öffentlichen APIs behalten wir uns jedoch das Recht vor, ihre Signatur zu jedem späteren Zeitpunkt zu ändern. Wenn Sie auf einen bestimmten Fluss stoßen, der nicht durch eine Kombination der aktuellen öffentlichen Adobe Pass-Authentifizierungs-API-Aufrufe unterstützt werden kann, sollten Sie uns am besten Bescheid geben. Unter Berücksichtigung Ihrer Anforderungen können wir die öffentlichen APIs ändern und eine stabile Lösung für künftige Aufgaben bereitstellen.

## Android-API {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequest](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- preauthorize
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

### Factory.getInstance {#getInstance}

**Beschreibung:** Instanziiert das Objekt Access Enabler . Pro Anwendungsinstanz sollte eine einzige Access Enabler -Instanz vorhanden sein.

| API-Aufruf: Konstruktor |
| --- |
| **public static** AccessEnabler **getInstance**(Context appContext, String softwareStatement, String redirectUrl)<br>        **throws** AccessEnablerException <br><br>**public static** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**throws** AccessEnablerException |

**Verfügbarkeit:** v3.1.2+

**Parameter:**

- *appContext*: Android-Anwendungskontext.
- env\_url: Zum Testen mit der Adobe-Staging-Umgebung kann env\_url auf &quot;sp.auth-staging.adobe.com&quot;festgelegt werden.

**Veraltet:**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequest {#setRequestor}

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gestellt wird, um mit dem Systemstatus zu synchronisieren (Abrufen eines Remote-Tokens, wenn SSO aktiviert ist, oder Löschen des lokalen Tokens, wenn in der Zwischenzeit ein Abmelden stattgefunden hat).

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

- *signedRequestorID*: Eine Kopie der Anforderer-ID, die digital mit Ihrem privaten Schlüssel signiert ist. 2.<!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->

- *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.

**Ausgelöste Rückrufe:** `setRequestorComplete()`

Veraltet:

    public void setRequestor(String requestorId, String signedRequestorId)
    
    public void setRequestor (String requestorId, String signedRequestorId, ArrayList&lt;String> urls)

[Zurück zur Android-API ...](#api)

### setRequestorComplete {#setRequestorComplete}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der Ihre Anwendung darüber informiert, dass die Konfigurationsphase abgeschlossen ist. Dies ist ein Signal, dass die App mit der Ausgabe von Berechtigungsanfragen beginnen kann. Bis zum Abschluss der Konfigurationsphase kann die Anwendung keine Berechtigungsanfragen mehr stellen.

| Callback: Konfiguration des Anforderers abgeschlossen |
| --- |
| java public void setRequestComplete(int status) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - Konfigurationsphase wurde erfolgreich abgeschlossen
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - Konfigurationsphase fehlgeschlagen
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - Konfigurationsphase wurde erfolgreich abgeschlossen
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - Konfigurationsphase fehlgeschlagen

**ausgelöst von:** `setRequestor()`

[Zurück zur Android-API ...](#api)

### setOptions {#setOptions}

**Beschreibung:** Konfiguriert globale SDK-Optionen. Es akzeptiert eine **Zuordnung\&lt;String, String\>** als Argument. Die Werte aus der Zuordnung werden zusammen mit jedem Netzwerkaufruf des SDK an den Server übergeben.

Die Werte werden unabhängig vom aktuellen Ablauf (Authentifizierung/Autorisierung) an den Server übergeben. Wenn Sie die Werte ändern möchten, können Sie diese Methode jederzeit aufrufen.

| API-Aufruf: setOptions |
| --- |
| public void setOptions(HashMap&lt;String,String> options) |

**Verfügbarkeit:** v1.9.2+

**Parameter:**

- *options*: Eine Zuordnung&lt;String, String> mit globalen SDK-Optionen. Derzeit sind die folgenden Optionen verfügbar:
   - **applicationProfile** - Damit können Serverkonfigurationen auf Grundlage dieses Werts erstellt werden.
   - **ap_vi** - Die Experience Cloud-ID (visitorID). Dieser Wert kann später für erweiterte Analyseberichte verwendet werden.
   - **ap_ai** - Die Advertising ID
   - **device_info** - Client-Informationen, wie hier beschrieben: [Weitergeben der Geräteverbindung für Client-Informationen und der Anwendung](/help/authentication/passing-client-information-device-connection-and-application.md).

[Zurück nach oben...](#apis)


### checkAuthentication {#checkAuthN}

**Beschreibung:** Überprüft den Authentifizierungsstatus. Dazu wird nach einem gültigen Authentifizierungstoken im lokalen Token-Speicher gesucht. Diese Methode führt keine Netzwerkaufrufe durch. Wir empfehlen, sie im Haupt-Thread aufzurufen. Sie wird von der Anwendung verwendet, um den Authentifizierungsstatus des Benutzers abzufragen und die Benutzeroberfläche entsprechend zu aktualisieren (d. h. die Anmelde-/Abmelde-Benutzeroberfläche zu aktualisieren). Der Authentifizierungsstatus wird der Anwendung über den Rückruf [*setAuthenticationStatus()*](#setAuthNStatus) übermittelt.

Wenn ein MVPD die Funktion &quot;Authentifizierung pro Anforderer&quot;unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden.  Weitere Informationen zu dieser Funktion finden Sie im Abschnitt [Caching Guidelines](#$caching) in der technischen Übersicht zu Android.

| API-Aufruf: Authentifizierungsstatus überprüfen |
| --- |
| public void checkAuthentication() |

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `setAuthenticationStatus()`

[Zurück zur Android-API ...](#api)


### getAuthentication {#getAuthN}

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Zunächst wird der Authentifizierungsstatus überprüft. Falls noch nicht authentifiziert, wird der Zustandsmaschine für den Authentifizierungsfluss gestartet:

- Wenn der letzte Authentifizierungsversuch erfolgreich war, wird die MVPD-Auswahlphase übersprungen und der Callback [*navigateToUrl()*](#navigagteToUrl) wird ausgelöst. Die Anwendung verwendet diesen Rückruf, um das WebView-Steuerelement zu instanziieren, das dem Benutzer die Anmeldeseite des MVPD anzeigt.
- Wenn der letzte Authentifizierungsversuch nicht erfolgreich war oder der Benutzer sich explizit abgemeldet hat, wird der Rückruf [*displayProviderDialog()*](#displayProviderDialog) ausgelöst. Ihre Anwendung verwendet diesen Rückruf, um die MVPD-Auswahlbenutzeroberfläche anzuzeigen. Außerdem muss Ihre App den Authentifizierungsfluss fortsetzen, indem sie die Access Enabler-Bibliothek über die Methode [setSelectedProvider()](#setSelectedProvider) über die MVPD-Auswahl des Benutzers informiert.

Da die Anmeldeinformationen des Benutzers auf der MVPD-Anmeldeseite überprüft werden, muss Ihre Anwendung die verschiedenen Weiterleitungsvorgänge überwachen, die während der Authentifizierung des Benutzers auf der Anmeldeseite des MVPD stattfinden. Wenn die richtigen Anmeldeinformationen eingegeben werden, wird das WebView-Steuerelement zu einer benutzerdefinierten URL umgeleitet, die von der *AccessEnabler.ADOBEPASS\_REDIRECT\_URL* -Konstante definiert wird. Diese URL soll nicht von WebView geladen werden. Die Anwendung muss diese URL abfangen und dieses Ereignis als Signal interpretieren, dass die Anmeldungsphase abgeschlossen ist. Anschließend sollte die Steuerung an den Access Enabler übergeben werden, um den Authentifizierungsfluss abzuschließen (durch Aufruf der Methode *getAuthenticationToken()* ).

Wenn ein MVPD die Funktion &quot;Authentifizierung pro Anforderer&quot;unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden (einer pro Programmierer).  Weitere Informationen zu dieser Funktion finden Sie im Abschnitt [Caching Guidelines](#$caching) in der technischen Übersicht zu Android.

Schließlich wird der Authentifizierungsstatus über den Rückruf *setAuthenticationStatus()* an die Anwendung übermittelt.



| API-Aufruf: initiiert den Authentifizierungsfluss |
| --- |
| public void getAuthentication() |

**Verfügbarkeit:** v1.0+

| API-Aufruf: initiiert den Authentifizierungsfluss |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;String, Object> genericData) |

**Verfügbarkeit:** v1.8+

**Parameter:**

- *forceAuthn*: Eine Markierung, die angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
- *data*: Eine Zuordnung, die aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Zurück zur Android-API ...](#api)

### displayProviderDialog {#displayProviderDialog}

**Beschreibung** Durch den Access Enabler ausgelöster Rückruf, um die Anwendung darüber zu informieren, dass die entsprechenden Elemente der Benutzeroberfläche instanziiert werden müssen, damit der Benutzer das gewünschte MVPD auswählen kann. Der Rückruf bietet eine Liste von MVPD-Objekten mit zusätzlichen Informationen, die dazu beitragen können, das Auswahlbenutzeroberflächenbedienfeld korrekt zu erstellen (z. B. die URL, die auf das MVPD-Logo verweist, der Anzeigename usw.).

Nachdem der Benutzer den gewünschten MVPD ausgewählt hat, muss die Anwendung auf der obersten Ebene den Authentifizierungsfluss fortsetzen, indem *setSelectedProvider()* aufgerufen und die Kennung des MVPD entsprechend der Auswahl des Benutzers übergeben wird.

>[!NOTE]
>
> Abbruch des Authentifizierungsablaufs
> </br></br>
> Bitte beachten Sie, dass dies ein Punkt ist, an dem der Benutzer die Schaltfläche &quot;Zurück&quot;drücken kann, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss Ihre Anwendung die `setSelectedProvider()` -Methode aufrufen und *null* als Parameter übergeben, damit der Access Enabler die Möglichkeit erhält, seinen Authentifizierungsstatus-Computer zurückzusetzen.

| Callback: Anzeigen der MVPD-Auswahlbenutzeroberfläche |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Verfügbarkeit:** v1.0+

**Parameter**:

- *mvpds*: Liste der MVPD-Objekte mit MVPD-bezogenen Informationen, die die Anwendung zum Erstellen der Elemente der MVPD-Auswahlbenutzeroberfläche verwenden kann.

**ausgelöst von:** `getAuthentication(), getAuthorization()`

[Zurück zur Android-API ...](#api)


### setSelectedProvider {#setSelectedProvider}

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Access Enabler über die MVPD-Auswahl des Benutzers zu informieren. Die Anwendung kann diese Methode verwenden, um den für die Authentifizierung verwendeten Dienstleister auszuwählen oder zu ändern.

Wenn der ausgewählte MVPD ein TempPass MVPD ist, authentifiziert er sich automatisch mit diesem MVPD, ohne später getAuthentication() aufrufen zu müssen.

Bitte beachten Sie, dass dies bei Promotional Temp Pass nicht möglich ist, wenn zusätzliche Parameter in der getAuthentication() -Methode angegeben werden.

Wenn *null* als Parameter übergeben wird, geht der Access Enabler davon aus, dass der Benutzer den Authentifizierungsfluss abgebrochen hat (d. h. durch Drücken der &quot;Zurück&quot;-Schaltfläche), und reagiert, indem er den Authentifizierungsstatus-Computer zurücksetzt und den Rückruf *setAuthenticationStatus()* mit dem Fehler-Code `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` aufruft.

| API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest. |
| --- |
| public void setSelectedProvider(String mvpdId) |

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Zurück zur Android-API ...](#api)


### navigateToUrl {#navigagteToUrl}

**Veraltet:** Ab Android SDK 3.0 wird navigateToUrl nur verwendet, wenn die benutzerdefinierte Registerkarte von Chrome nicht auf dem Gerät vorhanden ist.

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der die Anwendung informiert, dass dem Benutzer die MVPD-Anmeldeseite angezeigt werden muss, damit er seine Anmeldeinformationen eingeben kann. Der Access Enabler übergibt als Parameter die URL der MVPD-Anmeldeseite. Ihre Anwendung muss ein WebView-Steuerelement instanziieren und an diese URL weiterleiten. Außerdem muss die Anwendung die vom WebView-Steuerelement geladenen URLs überwachen und den Weiterleitungsvorgang abfangen, der auf die von der `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` -Konstante definierte benutzerdefinierte URL abzielt. Bei diesem Ereignis muss die Anwendung das WebView-Steuerelement entweder schließen oder ausblenden und das Steuerelement durch Aufruf der Methode *getAuthenticationToken()* an die Access Enabler-Bibliothek zurückgeben. Der Access Enabler schließt den Authentifizierungsfluss ab, indem er das Authentifizierungstoken vom Backend-Server abruft und lokal im Token-Speicher speichert.

>[!WARNING]
>
> **Abbruch des Authentifizierungsflusses** <br>Bitte beachten Sie, dass dies ein Punkt ist, an dem der Benutzer die Schaltfläche &quot;Zurück&quot;drücken kann, was dem Abbrechen des Authentifizierungsflusses entspricht. In einem solchen Szenario muss Ihre Anwendung die Methode _setSelectedProvider()_ aufrufen, die _null_ als Parameter übergibt und dem Access Enabler die Möglichkeit gibt, seinen Authentifizierungsstatus-Computer zurückzusetzen.

| Callback: MVPD-Anmeldeseite anzeigen |
| --- |
| public void navigateToUrl(String url) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *url*: Die URL, die auf die Anmeldeseite des MVPD verweist

**ausgelöst von:** `getAuthentication(), setSelectedProvider()`

[Zurück zur Android-API ...](#api)


### getAuthenticationToken {#getAuthNToken}

**Veraltet:** Ab Android SDK 3.0 wird diese Methode nicht mehr von der Anwendung verwendet, da die benutzerdefinierte Registerkarte von Chrome für die Authentifizierung verwendet wird.

**Beschreibung:** Schließt den Authentifizierungsfluss ab, indem das Authentifizierungstoken vom Backend-Server angefordert wird. Diese Methode sollte von Ihrer Anwendung nur als Reaktion auf ein Ereignis aufgerufen werden, bei dem das WebView-Steuerelement, das die MVPD-Anmeldeseite hostet, an die von der `AccessEnabler.ADOBEPASS_REDIRECT_URL` -Konstante definierte benutzerdefinierte URL umgeleitet wird.

| API-Aufruf: Authentifizierungstoken abrufen |
| --- |
| public void getAuthenticationToken() |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *Cookies*: Cookies, die in der Zieldomäne gesetzt werden (eine Referenzimplementierung finden Sie in der Demoanwendung im SDK ).

**Ausgelöste Rückrufe:** `setAuthenticationStatus()`, `sendTrackingData()`

[Zurück zur Android-API ...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Beschreibung:** Callback, der durch den Access Enabler ausgelöst wird, der
die Anwendung des Status des Authentifizierungsflusses. Es gibt viele
Stellen, an denen dieser Fluss fehlschlagen kann, entweder als Ergebnis der
Interaktion oder aufgrund anderer unvorhergesehener Szenarien (z. B. Netzwerk
Verbindungsprobleme usw.). Dieser Rückruf informiert die Anwendung von
den Erfolgs-/Fehlerstatus des Authentifizierungsflusses sowie
bei Bedarf zusätzliche Informationen zum Fehlergrund bereitzustellen.

| Callback: Melden Sie den Status des Authentifizierungsflusses |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - Authentifizierungsfluss wurde erfolgreich abgeschlossen
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - Authentifizierungsfluss fehlgeschlagen
- *code*: Grund für Fehler. Wenn *status* `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` ist, ist *code* eine leere Zeichenfolge (d. h. durch die `AccessEnablerConstants.USER_AUTHENTICATED` -Konstante definiert). Im Falle eines Fehlers kann dieser Parameter einen der folgenden Werte annehmen:
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - Der Benutzer ist nicht authentifiziert. Als Antwort auf den Methodenaufruf *checkAuthentication()* , wenn im lokalen Token-Cache kein gültiges Authentifizierungstoken vorhanden ist.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - Der AccessEnabler hat den Authentifizierungsstatus-Computer zurückgesetzt, nachdem die Anwendung der oberen Ebene *null* an `setSelectedProvider()` übergeben hat, um den Authentifizierungsfluss abzubrechen.  Vermutlich hat der Benutzer den Authentifizierungsfluss abgebrochen (d. h. die &quot;Zurück&quot;-Schaltfläche gedrückt).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - Der Authentifizierungsfluss schlug aus Gründen wie z. B. Nichtverfügbarkeit des Netzwerks fehl oder der Benutzer hat den Authentifizierungsfluss explizit abgebrochen.

**ausgelöst von:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Zurück zur Android-API ...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**Veraltet:** Ab Android SDK 3.6 ersetzt die Vorabautorisierungs-API checkPreauthorizedResources und liefert erweiterte Fehlercodes.

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der Hauptzweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden (z. B. zur Angabe des Zugriffs mit Schloss- und Entsperrungssymbolen).

| API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest. |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Verfügbarkeit:** v1.3+

**Parameter:** Der Parameter `resources` ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()` -Aufruf, d. h. es sollte sich um einen vereinbarten Wert handeln, der zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment festgelegt wurde.

**Callback ausgelöst:** `preauthorizedResources()`

[Zurück zur Android-API ...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**Veraltet:** Ab Android SDK 3.6 ersetzt die Vorabautorisierungs-API checkPreauthorizedResources und liefert erweiterte Fehlercodes.

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der Hauptzweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden (z. B. zur Angabe des Zugriffs mit Schloss- und Entsperrungssymbolen).

| API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest. |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Verfügbarkeit:** v3.1+

**Parameter:** Der Parameter `resources` ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()` -Aufruf, d. h. es sollte sich um einen vereinbarten Wert handeln, der zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment festgelegt wurde.

Der Parameter `cache` gibt an, ob die zwischengespeicherte Vorabautorisierungsantwort verwendet werden kann oder nicht. Standardmäßig lautet der Cache true (wahr). Das SDK gibt eine zuvor zwischengespeicherte Antwort zurück, sofern verfügbar.

**Callback ausgelöst:** `preauthorizedResources()`

[Zurück zur Android-API ...](#api)

### preauthorizedResources {#preauthResources}

**Veraltet:** Ab Android SDK 3.6 ersetzt die Vorabautorisierungs-API checkPreauthorizedResources und liefert erweiterte Fehlercodes. Der Rückruf preauthorizedResources wird in der neuen API nicht aufgerufen.


**Beschreibung:** Callback, der von checkPreauthorizedResources() ausgelöst wird. Bietet eine Liste der Ressourcen, für die der Benutzer bereits autorisiert ist.

| API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest. |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Verfügbarkeit:** v1.3+

**Parameter:** Der Parameter `resources` ist ein Array von Ressourcen, für die der Benutzer bereits zur Anzeige berechtigt ist.

**ausgelöst von:** `checkPreauthorizedResources()`

[Zurück zur Android-API ...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn sie nicht authentifiziert ist, wird der Rückruf *setTokenRequestFailed()* ausgelöst und die Methode wird beendet. Wenn der Benutzer authentifiziert ist, wird auch der Autorisierungsfluss Trigger. Siehe Details zur Methode *getAuthorization()* .

| API-Aufruf: Überprüfen des Autorisierungsstatus |
| --- |
| public void checkAuthorization(String resourceId) |

**Verfügbarkeit:** v1.0+

| API-Aufruf: Überprüfen des Autorisierungsstatus |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Verfügbarkeit:** v1.8+

**Parameter:**

- *resourceId*: Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Zuordnung, die aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Zurück zur Android-API ...](#api)


### <span id="getAuthZ"></span>getAuthorization

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsfluss zu initiieren. Wenn der Benutzer noch nicht authentifiziert ist, wird auch der Authentifizierungsfluss initiiert. Wenn der Benutzer authentifiziert wird, stellt der Access Enabler weiter Anforderungen an das Autorisierungstoken (wenn im lokalen Token-Cache kein gültiges Autorisierungstoken vorhanden ist) und an das Token für kurzlebige Medien. Sobald das Short-Media-Token abgerufen wurde, wird der Autorisierungsfluss als vollständig betrachtet. Der Rückruf *setToken()* wird ausgelöst und das Short-Media-Token wird als Parameter an die Anwendung gesendet. Wenn die Autorisierung aus irgendeinem Grund fehlschlägt, wird der Rückruf *tokenRequestFailed()* ausgelöst und der Fehlercode und die Details werden bereitgestellt.

| API-Aufruf: Initiieren des Autorisierungsflusses |
| --- |
| public void getAuthorization(String resourceId) |

**Verfügbarkeit:** v1.0+

| API-Aufruf: Initiieren des Autorisierungsflusses |
| --- |
| public void getAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Verfügbarkeit:** v1.8+

**Parameter:**

- *resourceId*: Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Zuordnung, die aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Zusätzliche ausgelöste Rückrufe** <br> Diese Methode kann auch die folgenden Rückrufe Trigger (wenn der Authentifizierungsfluss ebenfalls initiiert wird): *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**HINWEIS: Verwenden Sie möglichst checkAuthorization() anstelle von getAuthorization() . Die Methode getAuthorization() startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf der Seite des Programmierers führen könnte.**

[Zurück zur Android-API ...](#api)


### setToken {#setToken}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der Ihre Anwendung darüber informiert, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde. Das kurzlebige Medien-Token wird auch als Parameter bereitgestellt.

| Callback: Autorisierungsfluss erfolgreich abgeschlossen |
| --- |
| public void setToken(String token, String resourceId) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *token*: Das kurzlebige Medien-Token
- *resourceId*: Die Ressource, für die die Autorisierung abgerufen wurde

**ausgelöst durch:** `checkAuthorization()`, `getAuthorization()`


[Zurück zur Android-API ...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der die Anwendung auf der obersten Ebene darüber informiert, dass der Autorisierungsfluss fehlgeschlagen ist.

| Callback: Autorisierungsfluss fehlgeschlagen |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die Ressource, für die die Autorisierung abgerufen wurde
- *errorCode*: Fehler-Code, der dem Fehlerszenario zugeordnet ist. Mögliche Werte:
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - Der Benutzer konnte für die jeweilige Ressource nicht autorisieren
- *errorDescription*: Zusätzliche Details zum Fehlerszenario. Wenn diese beschreibende Zeichenfolge aus irgendeinem Grund nicht verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge **(&quot;&quot;)**.

  Diese Zeichenfolge kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder umsatzbezogene Nachrichten zu übergeben. Wenn einem Abonnenten beispielsweise die Autorisierung für eine Ressource verweigert wird, könnte der MVPD eine Nachricht wie die folgende senden: &quot;Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie hier.&quot; Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Rückruf an den Programmierer übergeben, der die Möglichkeit hat, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann diesen Parameter auch verwenden, um eine Benachrichtigung über die Bedingung bereitzustellen, die möglicherweise zu einem Fehler geführt hat. Beispiel: &quot;Bei der Kommunikation mit dem Autorisierungsdienst des Providers ist ein Netzwerkfehler aufgetreten.&quot;

**ausgelöst von:** `checkAuthorization(), getAuthorization()`

[Zurück zur Android-API ...](#api)

### Abmelden {#logout}

**Beschreibung:** Verwenden Sie diese Methode, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Weiterleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Daher kann dieser Fluss nicht mit einer einfachen HTTP-Anforderung abgeschlossen werden, die von der Access Enabler-Bibliothek ausgegeben wird. Chrome Custom Tabs werden vom SDK verwendet, um HTTP-Weiterleitungsvorgänge auszuführen. Dieser Fluss wird für den Benutzer sichtbar und nach Abschluss geschlossen

| API-Aufruf: Logout-Fluss initiieren |
| --- |
| public void logout() |

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:**

- `navigateToUrl()` für SDK-Version vor 3.0
- `setAuthenticationStatus()` für SDK-Version > 3.0


[Zurück zur Android-API ...](#api)


### getSelectedProvider {#getSelectedProvider}

**Beschreibung:** Verwenden Sie diese Methode, um den derzeit ausgewählten Anbieter zu bestimmen.

| API-Aufruf: Bestimmen Sie den aktuell ausgewählten MVPD |
| --- |
| public void getSelectedProvider() |

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `selectedProvider()`

[Zurück zur Android-API ...](#api)


### <span id="selectedProvider"></span>selectedProvider

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird und Informationen über das aktuell ausgewählte MVPD für die Anwendung bereitstellt.

| Callback: Informationen zum aktuell ausgewählten MVPD |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**Verfügbarkeit:** v1.0+

**Parameter:**

- *mvpd*: Objekt mit Informationen zum derzeit ausgewählten MVPD

**ausgelöst von:** `getSelectedProvider()`

[Zurück zur Android-API ...](#api)


### getMetadata {#getMetadata}

**Beschreibung:** Verwenden Sie diese Methode, um Informationen abzurufen, die von der Access Enabler-Bibliothek als Metadaten bereitgestellt werden. Die Anwendung kann auf diese Informationen zugreifen, indem sie ein zusammengesetztes MetadataKey -Objekt bereitstellt.

| API-Aufruf: Abfrage des AccessEnabler für Metadaten |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Verfügbarkeit:** v1.0+

Programmierern stehen zwei Metadatentypen zur Verfügung:

- Statische Metadaten (Authentifizierungstoken TTL, Autorisierungstoken TTL und Geräte-ID)
- Benutzermetadaten (benutzerspezifische Informationen wie Benutzer-ID und Postleitzahl; von einem MVPD an das Gerät eines Benutzers während der Authentifizierungs- und/oder Autorisierungsflüsse übergeben)

**Parameter:**

- *metadataKey*: Eine Datenstruktur, die eine Schlüssel- und args-Variable mit der folgenden Bedeutung enthält:
   - Wenn der Schlüssel `METADATA_KEY_USER_META` ist und args ein SerializableNameValuePair -Objekt mit Name = `METADATA_ARG_USER_META` und Wert = `[metadata_name]` enthält, wird die Abfrage für Benutzermetadaten durchgeführt. Die aktuelle Liste der verfügbaren Benutzer-Metadatentypen:
      - `zip` - Postleitzahl

      - `householdID` - Kennung des Haushalts. Wenn ein MVPD keine Unterkonten unterstützt, ist dies identisch mit `userID`.

      - `maxRating` - Maximale elterliche Bewertung für den Benutzer

      - `userID` - Die Benutzer-ID. Wenn ein MVPD Subkonten unterstützt und der Benutzer nicht das Hauptkonto ist, unterscheidet sich `userID` von `householdID`.

      - `channelID` - Eine Liste der Kanäle, die der Benutzer anzeigen darf
   - Wenn der Schlüssel `METADATA_KEY_DEVICE_ID` ist, wird die Abfrage durchgeführt, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen über Aktivierung und Gebühren zu erhalten.
   - Wenn der Schlüssel `METADATA_KEY_TTL_AUTHZ` ist und args ein SerializableNameValuePair -Objekt mit Name = `METADATA_ARG_RESOURCE_ID` und Wert = `[resource_id]` enthält, wird die Abfrage durchgeführt, um die Ablaufzeit des Autorisierungstokens zu erhalten, das mit der angegebenen Ressource verknüpft ist.
   - Wenn der Schlüssel `METADATA_KEY_TTL_AUTHN` ist, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.



>[!NOTE]
>
>Für SDK 3.4.0 sind die Konstanten: `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` unter com.adobe.adobepass.access.api.profile.UserProfileService verfügbar.



>[!NOTE]
>
>Die tatsächlichen Benutzermetadaten, die einem Programmierer zur Verfügung stehen, hängen davon ab, was ein MVPD zur Verfügung stellt.  Diese Liste wird weiter erweitert, sobald neue Metadaten verfügbar gemacht und dem Adobe Pass-Authentifizierungssystem hinzugefügt werden.

**Ausgelöste Rückrufe:** [`setMetadataStatus()`](#setMetadaStatus)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/user-metadata-feature.md)

[Zurück zur Android-API ...](#api)

### setMetadataStatus {#setMetadaStatus}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird und die über einen *getMetadata()* -Aufruf angeforderten Metadaten liefert.

| Rückruf: Ergebnis der Metadaten-Abrufanforderung |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *key*: Das MetadataKey-Objekt, das den Schlüssel enthält, für den der Metadatenwert angefordert wird, und die zugehörigen Parameter (siehe Demoanwendung für eine Referenzimplementierung).
- *result*: Ein zusammengesetztes Objekt, das die angeforderten Metadaten enthält. Das Objekt weist die folgenden Felder auf:
   - *simpleResult*: ein String, der den Metadatenwert darstellt, wenn die Anfrage für die Authentifizierungs-TTL, Autorisierungs-TTL oder Geräte-ID gestellt wurde. Dieser Wert ist null, wenn die Anforderung für Benutzermetadaten ausgeführt wurde.

   - *userMetadataResult*: ein Objekt, das die Java-Darstellung einer JSON User Metadata-Payload enthält.\
     Beispiel:

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
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

- *verschlüsselt*: Boolescher Wert, der angibt, ob die abgerufenen Metadaten verschlüsselt sind. Dieser Parameter ist nur für Benutzer-Metadaten-Anfragen von Bedeutung. Er hat keine Bedeutung für statische Metadaten (z. B.: Authentifizierungs-TTL), die immer unverschlüsselt empfangen werden. Wenn dieser Parameter auf &quot;True&quot;gesetzt ist, muss der Programmierer den Wert für die unverschlüsselten Benutzer-Metadaten abrufen, indem er eine RSA-Entschlüsselung mit dem privaten Schlüssel auf der Whitelist durchführt (der private Schlüssel, der für die Signierung der Anforderer-ID im [`setRequestor`](#setRequestor) -Aufruf verwendet wird).

**ausgelöst von:** [`getMetadata()`](#getMetadata)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/user-metadata-feature.md)


[Zurück zur Android-API ...](#api)


### getVersion {#getVersion}

**Beschreibung:** Mit dieser Methode kann die Version der AccessEnabler-Bibliothek abgerufen werden.

| API-Aufruf: get AccessEnabler version |
| --- |
| ```public static String getVersion()``` |


[Zurück zur Android-API ...](#api)

</br>

## Ereignisse verfolgen {#tracking}

Der Access Enabler -Trigger verfügt über einen zusätzlichen Callback, der nicht unbedingt mit den Berechtigungsflüssen in Zusammenhang steht. Die Implementierung der Rückruffunktion &quot;event-tracking&quot; mit dem Namen *sendTrackingData()* ist optional. Sie ermöglicht es der Anwendung jedoch, bestimmte Ereignisse zu verfolgen und Statistiken wie die Anzahl erfolgreicher/fehlgeschlagener Authentifizierungs-/Autorisierungsversuche zu kompilieren. Nachstehend finden Sie die Spezifikation für den Rückruf *sendTrackingData()* :


### sendTrackingData {#sendTrackingData}

**Beschreibung:** Durch den Access Enabler ausgelöster Rückruf, der der Anwendung das Auftreten verschiedener Ereignisse wie dem Fertigstellen/Fehlschlagen von Authentifizierungs-/Autorisierungsflüssen signalisiert. Der Gerätetyp, der Zugriffs-Enabler-Client-Typ und das Betriebssystem werden ebenfalls von sendTrackingData() gemeldet.

>[!WARNING]
>
> Gerätetyp und Betriebssystem werden durch die Verwendung einer öffentlichen Java-Bibliothek ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) und der Benutzeragenten-Zeichenfolge abgeleitet. Beachten Sie, dass diese Informationen nur als grobe Methode zur Unterteilung von Betriebsmetriken in Gerätekategorien bereitgestellt werden, dass Adobe jedoch keine Verantwortung für fehlerhafte Ergebnisse übernehmen kann. Verwenden Sie die neue Funktion entsprechend.


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
   - `android`

</br>

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

Im Folgenden finden Sie Anweisungen zur Interpretation der Werte in den *data*
array:

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

**ausgelöst durch:** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Zurück zur Android-API ...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
