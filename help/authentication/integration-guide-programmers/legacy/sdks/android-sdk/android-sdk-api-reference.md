---
title: Android SDK API-Referenz
description: Android SDK API-Referenz
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '4560'
ht-degree: 0%

---

# (Veraltete) Android SDK API-Referenz {#android-sdk-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung {#intro}

In diesem Dokument werden die Methoden und Callbacks beschrieben, die von der Android SDK für die Adobe Pass-Authentifizierung bereitgestellt werden, welche von der Adobe Pass-Authentifizierungsversion 1.7 und höher unterstützt wird. Die hier beschriebenen Methoden und Callback-Funktionen sind in den Header-Dateien AccessEnabler.h und EntitlementDelegate.h definiert.

Die neueste Android AccessEnabler-SDK finden Sie [&#x200B; &#x200B;](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library)https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library.


**Hinweis:** Das Adobe Pass-Authentifizierungsteam empfiehlt die Verwendung nur von (öffentlichen *Adobe Pass-*:

- Öffentliche APIs sind für *unterstützten Clienttypen verfügbar* und vollständig getestet). Für jede öffentliche Funktion stellen wir sicher, dass jeder Client-Typ über eine entsprechende Version der zugehörigen Methode(n) verfügt.</span>
- Öffentliche APIs müssen so stabil wie möglich sein, um die Abwärtskompatibilität zu unterstützen und sicherzustellen, dass Partnerintegrationen nicht beschädigt werden. Bei nicht *APIs behalten* sich jedoch das Recht vor, ihre Signatur in Zukunft zu ändern. Wenn Sie auf einen bestimmten Fluss stoßen, der nicht durch eine Kombination der aktuellen öffentlichen Adobe Pass-Authentifizierungs-API-Aufrufe unterstützt werden kann, sollten Sie uns dies mitteilen. Unter Berücksichtigung Ihrer Anforderungen können wir die öffentlichen APIs ändern und eine stabile Lösung für die Zukunft bereitstellen.

## Android-API {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- vorautorisieren
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

**Beschreibung:** instanziiert das Access Enabler-Objekt. Pro Anwendungsinstanz sollte eine einzige Access Enabler-Instanz vorhanden sein.

| API-Aufruf: Konstruktor |
| --- |
| **public static** AccessEnabler **getInstance**(Context appContext, String softwareStatement, String redirectUrl)<br>        **throws** AccessEnablerException <br><br>**public static** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**throws** AccessEnablerException |

**Verfügbarkeit:** v3.1.2+

**Parameter:**

- *appContext*: Anwendungskontext von Android.
- env\_url: Für Tests mit der Adobe-Staging-Umgebung kann env\_url auf „sp.auth-staging.adobe.com“ festgelegt werden

**Veraltet:**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung bei Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gebracht wird, um mit dem Systemstatus zu synchronisieren (rufen Sie ein Remote-Token ab, wenn SSO aktiviert ist, oder löschen Sie das lokale Token, wenn eine Abmeldung zwischenzeitlich stattgefunden hat).

Die Antwort des Servers enthält eine Liste der MVPDs zusammen mit Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Server-Antwort wird intern vom Access Enabler-Code verwendet. Nur der Status des Vorgangs (d. h. ERFOLG/FEHLGESCHLAGEN) wird Ihrer Anwendung über den Callback setRequestorComplete() angezeigt.

Wenn der Parameter *urls* nicht verwendet wird, zielt der resultierende Netzwerkaufruf auf die Standard-Service-Provider-URL ab: die Adobe Release-/Produktionsumgebung.

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

- *RequestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie die eindeutige ID, die Adobe Ihrer Site zugewiesen hat, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungs-Service registriert haben.

- *signedRequestorID*: Eine Kopie der Anforderer-ID, die mit Ihrem privaten Schlüssel digital signiert ist. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *urls*: Optionaler Parameter; standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für von Adobe bereitgestellte Authentifizierungs- und Autorisierungsdienste angeben (verschiedene Instanzen können zu Debugging-Zwecken verwendet werden). Damit können Sie mehrere Instanzen des Authentifizierungs-Service von Adobe Pass angeben. Dabei besteht die MVPD-Liste aus den Endpunkten aller Dienstleister. Jede MVPD ist mit dem schnellsten Dienstleister verknüpft, d. h. dem Anbieter, der zuerst geantwortet hat und diese MVPD unterstützt.

**Ausgelöste Callbacks:** `setRequestorComplete()`

Veraltet:

    public void setRequestor(String RequestorId, String signedRequestorId)
    
    public void setRequestor (String RequestorId, String signedRequestorId, ArrayList&lt;String> urls)

[Zurück zur Android-API…](#api)

### setRequestComplete {#setRequestorComplete}

**Beschreibung:** Callback, ausgelöst durch den Access Enabler, der Ihrer Anwendung mitteilt, dass die Konfigurationsphase abgeschlossen ist. Dies ist ein Signal, dass die App mit der Ausstellung von Berechtigungsanfragen beginnen kann. Solange die Konfigurationsphase noch nicht abgeschlossen ist, kann die Anwendung keine Berechtigungsanfragen stellen.

| Callback: Anfordererkonfiguration abgeschlossen |
| --- |
| java public void setRequestorComplete(int status) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - Konfigurationsphase wurde erfolgreich abgeschlossen
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - Konfigurationsphase ist fehlgeschlagen
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - Konfigurationsphase wurde erfolgreich abgeschlossen
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - Konfigurationsphase ist fehlgeschlagen

**Ausgelöst von:** `setRequestor()`

[Zurück zur Android-API…](#api)

### setOptions {#setOptions}

**Beschreibung:** Konfiguriert globale SDK-Optionen. Als Argument wird ein **Map\&lt;String, String\>** akzeptiert. Die Werte aus der Zuordnung werden zusammen mit jedem Netzwerkaufruf der SDK an den Server übergeben.

Die Werte werden unabhängig vom aktuellen Fluss (Authentifizierung/Autorisierung) an den Server übergeben. Wenn Sie die Werte ändern möchten, können Sie diese Methode jederzeit aufrufen.

| API-Aufruf: setOptions |
| --- |
| public void setOptions(HashMap&lt;String, String> options) |

**Verfügbarkeit:** v1.9.2+

**Parameter:**

- *options*: Eine Zuordnung&lt;String, String> mit globalen SDK-Optionen. Derzeit sind die folgenden Optionen verfügbar:
   - **applicationProfile** - Kann verwendet werden, um Server-Konfigurationen basierend auf diesem Wert vorzunehmen.
   - **ap_vi** - Die Experience Cloud-ID (visitorID). Dieser Wert kann später für erweiterte Analyseberichte verwendet werden.
   - **ap_ai** - Die Advertising-ID
   - **device_info** - Client-Informationen wie hier beschrieben: [Übergeben von Client-Informationen, Geräteverbindung und Anwendung](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

[Nach oben…](#apis)


### checkAuthentication {#checkAuthN}

**Beschreibung:** Prüft den Authentifizierungsstatus. Dies erfolgt, indem im lokalen Token-Speicher nach einem gültigen Authentifizierungstoken gesucht wird. Diese Methode führt keine Netzwerkaufrufe aus, und es wird empfohlen, sie im Haupt-Thread aufzurufen. Sie wird von der Anwendung verwendet, um den Authentifizierungsstatus des Benutzers abzufragen und die Benutzeroberfläche entsprechend zu aktualisieren (d. h. die Benutzeroberfläche für Anmeldung/Abmeldung zu aktualisieren). Der Authentifizierungsstatus wird der Anwendung über den Callback [*setAuthenticationStatus()*](#setAuthNStatus) mitgeteilt.

Wenn eine MVPD die Funktion „Authentifizierung pro Anfordernder“ unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden.  Weitere Informationen zu dieser Funktion finden Sie im Abschnitt [Caching-Richtlinien](#$caching) in der technischen Übersicht zu Android.

| API-Aufruf: Authentifizierungsstatus überprüfen |
| --- |
| public void checkAuthentication() |

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** `setAuthenticationStatus()`

[Zurück zur Android-API…](#api)


### getAuthentication {#getAuthN}

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Sie beginnt mit der Prüfung des Authentifizierungsstatus. Wenn er nicht bereits authentifiziert ist, wird der Zustandscomputer des Authentifizierungsflusses gestartet:

- Wenn der letzte Authentifizierungsversuch erfolgreich war, wird die MVPD-Auswahlphase übersprungen und der Rückruf [*navigateToUrl()*](#navigagteToUrl) ausgelöst. Die Anwendung verwendet diesen Rückruf, um das WebView-Steuerelement zu instanziieren, das dem Benutzer die Anmeldeseite der MVPD anzeigt.
- Wenn der letzte Authentifizierungsversuch erfolglos war oder der Benutzer sich explizit abgemeldet hat, wird der [*displayProviderDialog()*](#displayProviderDialog) Callback ausgelöst. Ihre Anwendung verwendet diesen Callback, um die MVPD-Auswahlbenutzeroberfläche anzuzeigen. Außerdem muss die App den Authentifizierungsfluss fortsetzen, indem sie die Access Enabler-Bibliothek über die MVPD-Auswahl des Benutzers mithilfe der Methode [setSelectedProvider()](#setSelectedProvider) informiert.

Da die Anmeldeinformationen der Benutzenden auf der Anmeldeseite von MVPD überprüft werden, muss die Anwendung die verschiedenen Umleitungsvorgänge überwachen, die stattfinden, während sich die Benutzenden auf der Anmeldeseite von MVPD authentifizieren. Wenn die korrekten Anmeldeinformationen eingegeben werden, wird das WebView-Steuerelement an eine benutzerdefinierte URL umgeleitet, die durch die Konstante *AccessEnabler.ADOBEPASS\_REDIRECT\_URL* definiert wird. Diese URL ist nicht zum Laden durch die WebView vorgesehen. Die Anwendung muss diese URL abfangen und dieses Ereignis als Signal interpretieren, dass die Anmeldephase abgeschlossen ist. Anschließend sollte die Steuerung an den Access Enabler übergeben werden, um den Authentifizierungsfluss abzuschließen (durch Aufruf der Methode *getAuthenticationToken()*.

Wenn eine MVPD die Funktion „Authentifizierung pro Anfordernder“ unterstützt, können mehrere Authentifizierungstoken auf einem Gerät gespeichert werden (eines pro Programmierer).  Weitere Informationen zu dieser Funktion finden Sie im Abschnitt [Caching-Richtlinien](#$caching) in der technischen Übersicht zu Android.

Schließlich wird der Authentifizierungsstatus über den Rückruf *setAuthenticationStatus()* an die Anwendung übermittelt.



| API-Aufruf: Startet den Authentifizierungsfluss |
| --- |
| public void getAuthentication() |

**Verfügbarkeit:** v1.0+

| API-Aufruf: Startet den Authentifizierungsfluss |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;String, Object> genericData) |

**Verfügbarkeit:** v1.8+

**Parameter:**

- *forceAuth*: Ein Flag, das angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Service gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Zurück zur Android-API…](#api)

### displayProviderDialog {#displayProviderDialog}

**Beschreibung** Durch den Access Enabler ausgelöster Callback, um die Anwendung darüber zu informieren, dass die entsprechenden Benutzeroberflächenelemente instanziiert werden müssen, damit der Benutzer die gewünschte MVPD auswählen kann. Der Callback bietet eine Liste von MVPD-Objekten mit zusätzlichen Informationen, die dazu beitragen können, das Auswahlbenutzeroberflächenbedienfeld korrekt zu erstellen (z. B. die URL, die auf das MVPD-Logo verweist, den Anzeigenamen usw.)

Nachdem der Benutzer die gewünschte MVPD ausgewählt hat, muss die Upper-Layer-Anwendung den Authentifizierungsfluss fortsetzen, indem sie *setSelectedProvider()* aufruft und ihr die ID der MVPD übergibt, die der Auswahl des Benutzers entspricht.

>[!NOTE]
>
> Abbrechen des Authentifizierungsflusses
> </br></br>
> Bitte beachten Sie, dass dies ein Punkt ist, an dem der Benutzer die Möglichkeit hat, die Schaltfläche „Zurück“ zu drücken, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss die Anwendung die `setSelectedProvider()` aufrufen und *null* als Parameter übergeben, um dem Access Enabler die Möglichkeit zu geben, seinen Authentifizierungszustand auf seinem Computer zurückzusetzen.

| Callback: Anzeigen der MVPD-Auswahlbenutzeroberfläche |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Verfügbarkeit:** v1.0+

**Parameter**:

- *mvpds*: Liste der MVPD-Objekte, die MVPD-bezogene Informationen enthalten, mit denen die Anwendung die MVPD-Auswahlbenutzeroberflächenelemente erstellen kann.

**Ausgelöst von:** `getAuthentication(), getAuthorization()`

[Zurück zur Android-API…](#api)


### setSelectedProvider {#setSelectedProvider}

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Access Enabler über die MVPD-Auswahl des Benutzers zu informieren. Die Anwendung kann diese Methode verwenden, um den für die Authentifizierung verwendeten Dienstleister auszuwählen oder zu ändern.

Wenn die ausgewählte MVPD eine TempPass-MVPD ist, wird sie automatisch mit dieser MVPD authentifiziert, ohne dass getAuthentication() später aufgerufen werden muss.

Beachten Sie, dass dies für den temporären Pass zu Werbezwecken nicht möglich ist, wenn zusätzliche Parameter für die getAuthentication()-Methode angegeben werden.

Wenn *null* als Parameter übergeben wird, geht Access Enabler davon aus, dass der Benutzer den Authentifizierungsfluss abgebrochen hat (d. h. die Schaltfläche „Zurück“ gedrückt hat), und antwortet, indem er den Authentifizierungsstatus-Computer zurücksetzt und den Callback *setAuthenticationStatus()* mit dem `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` Fehlercode aufruft.

| API-Aufruf: Festlegen des aktuell ausgewählten Anbieters |
| --- |
| public void setSelectedProvider(String mvpdId) |

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Zurück zur Android-API…](#api)


### navigateToUrl {#navigagteToUrl}

**Veraltet:** ab Android SDK 3.0 wird navigateToUrl nur verwendet, wenn die benutzerdefinierte Registerkarte &quot;Chrome&quot; nicht auf dem Gerät vorhanden ist

**Beschreibung:** Callback, der durch den Access Enabler ausgelöst wird, der die Anwendung darüber informiert, dass die Anmeldeseite von MVPD angezeigt werden muss, um seine Anmeldeinformationen einzugeben. Der Access Enabler gibt als Parameter die URL der MVPD-Anmeldeseite weiter. Die Anwendung muss ein WebView-Steuerelement instanziieren und an diese URL weiterleiten. Außerdem muss die Anwendung die vom WebView-Steuerelement geladenen URLs überwachen und den Weiterleitungsvorgang abfangen, der auf die von der `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` definierte benutzerdefinierte URL abzielt. Bei diesem Ereignis muss die Anwendung das WebView-Steuerelement entweder schließen oder ausblenden und das Steuerelement durch Aufruf der Methode *getAuthenticationToken() an die Access Enabler-Bibliothek*. Access Enabler schließt den Authentifizierungsfluss ab, indem es das Authentifizierungstoken vom Back-End-Server abruft und lokal im Token-Speicher speichert.

>[!WARNING]
>
> **Abbruch des Authentifizierungsflusses** <br>Beachten Sie, dass dies ein Punkt ist, an dem der Benutzer die Schaltfläche „Zurück“ drücken kann, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss die Anwendung die Methode _setSelectedProvider()_ aufrufen, die _null_ als Parameter übergibt und dem Access Enabler die Möglichkeit gibt, seinen Authentifizierungszustand auf seinem Computer zurückzusetzen.

| Callback: MVPD-Anmeldeseite anzeigen |
| --- |
| public void navigateToUrl(String url) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *url*: Die URL, die auf die Anmeldeseite von MVPD verweist

**Ausgelöst von:** `getAuthentication(), setSelectedProvider()`

[Zurück zur Android-API…](#api)


### getAuthenticationToken {#getAuthNToken}

**Veraltet:** Ab Android SDK 3.0 wird diese Methode nicht mehr von der Anwendung aus verwendet, da die benutzerdefinierte Registerkarte Chrome für die Authentifizierung verwendet wird.

**Beschreibung:** Schließt den Authentifizierungsfluss ab, indem das Authentifizierungstoken vom Backend-Server angefordert wird. Diese Methode sollte von der Anwendung nur als Antwort auf ein Ereignis aufgerufen werden, bei dem das WebView-Steuerelement, das die MVPD-Anmeldeseite hostet, an die von der `AccessEnabler.ADOBEPASS_REDIRECT_URL` definierte benutzerdefinierte URL umgeleitet wird.

| API-Aufruf: Authentifizierungs-Token abrufen |
| --- |
| public void getAuthenticationToken() |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *Cookies*: Cookies, die in der Ziel-Domain gesetzt werden (eine Referenzimplementierung finden Sie im Demoprogramm in SDK).

**Ausgelöste Callbacks:** `setAuthenticationStatus()`, `sendTrackingData()`

[Zurück zur Android-API…](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Beschreibung:** Callback, ausgelöst durch den Access Enabler, der informiert
Die Anwendung des Status des Authentifizierungsflusses. Es gibt viele
Orte, an denen dieser Fluss fehlschlagen kann, entweder aufgrund der
Interaktion oder aufgrund anderer unvorhergesehener Szenarien (z. B. Netzwerk
Verbindungsprobleme usw.). Dieser Rückruf informiert die Anwendung über
den Status Erfolg/Fehler des Authentifizierungsflusses sowie
bei Bedarf zusätzliche Informationen zur Fehlerursache angeben.

| Callback: Meldet den Status des Authentifizierungsflusses |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *status*: Kann einen der folgenden Werte annehmen:
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - Authentifizierungsfluss wurde erfolgreich abgeschlossen
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - Authentifizierungsfluss fehlgeschlagen
- *code*: Fehlerursache. Wenn *status* den Wert `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` hat, ist *code* eine leere Zeichenfolge (d. h. definiert durch die `AccessEnablerConstants.USER_AUTHENTICATED`). Im Falle eines Fehlers kann dieser Parameter einen der folgenden Werte annehmen:
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - Der Benutzer ist nicht authentifiziert. Als Antwort auf den Aufruf der *checkAuthentication()*-Methode, wenn im Cache des lokalen Tokens kein gültiges Authentifizierungstoken vorhanden ist.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler hat den Authentifizierungsstatus-Computer zurückgesetzt, nachdem die Upper-Layer-Anwendung (*)* wurde, um den Authentifizierungsfluss abzubrechen`setSelectedProvider()`.  Vermutlich hat der Benutzer den Authentifizierungsfluss abgebrochen (d.h. die Schaltfläche „Zurück“ gedrückt).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - Der Authentifizierungsfluss ist aus Gründen wie der Nichtverfügbarkeit des Netzwerks oder dem expliziten Abbruch des Authentifizierungsflusses fehlgeschlagen.

**Ausgelöst von:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Zurück zur Android-API…](#api)


### checkPreauthorizedResources {#checkPreauth}

>**Veraltet:** Ab Android SDK 3.6 ersetzt die vorautorisierte API checkPreauthorizedResources und stellt erweiterte Fehlercodes bereit.

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der primäre Zweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden können (z. B. die Angabe des Zugriffsstatus mit Sperren- und Entsperren-Symbolen).

| API-Aufruf: Festlegen des aktuell ausgewählten Anbieters |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Verfügbarkeit:** v1.3+

**Parameter:** Der `resources` ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()`-Aufruf, d. h. sie sollte ein vereinbarter Wert sein, der zwischen dem Programmierer und dem MVPD oder einem RSS-Medienfragment festgelegt wird.

**Rückruf ausgelöst:** `preauthorizedResources()`

[Zurück zur Android-API…](#api)


### checkPreauthorizedResources {#checkPreauth2}

**Veraltet:** Ab Android SDK 3.6 ersetzt die vorautorisierte API checkPreauthorizedResources und stellt erweiterte Fehlercodes bereit.

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der primäre Zweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden können (z. B. die Angabe des Zugriffsstatus mit Sperren- und Entsperren-Symbolen).

| API-Aufruf: Festlegen des aktuell ausgewählten Anbieters |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Verfügbarkeit:** v3.1+

**Parameter:** Der `resources` ist ein Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization()`-Aufruf, d. h. sie sollte ein vereinbarter Wert sein, der zwischen dem Programmierer und dem MVPD oder einem RSS-Medienfragment festgelegt wird.

Der Parameter `cache` gibt an, ob die zwischengespeicherte Antwort vor der Autorisierung verwendet werden kann oder nicht. Standardmäßig ist der Zwischenspeicher „true“, dann gibt die SDK eine zuvor zwischengespeicherte Antwort zurück, sofern verfügbar.

**Rückruf ausgelöst:** `preauthorizedResources()`

[Zurück zur Android-API…](#api)

### preauthorizedResources {#preauthResources}

**Veraltet:** Ab Android SDK 3.6 ersetzt die vorautorisierte API checkPreauthorizedResources und stellt erweiterte Fehlercodes bereit. preauthorizedResources-Callback wird in der neuen API nicht aufgerufen.


**Beschreibung:** Callback, ausgelöst durch checkPreauthorizedResources(). Stellt eine Liste der Ressourcen bereit, die der Benutzer bereits anzeigen darf.

| API-Aufruf: Festlegen des aktuell ausgewählten Anbieters |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Verfügbarkeit:** v1.3+

**Parameter:** Der `resources` ist ein Array von Ressourcen, für die der Benutzer bereits berechtigt ist, sie anzuzeigen.

**Ausgelöst von:** `checkPreauthorizedResources()`

[Zurück zur Android-API…](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn die Methode nicht authentifiziert ist *wird der Rückruf* setTokenRequestFailed() ausgelöst und die Methode beendet. Wenn der Benutzer authentifiziert ist, führt dies auch zu einem Trigger des Autorisierungsflusses. Siehe Details zur Methode *getAuthorization(*.

| API-Aufruf: Autorisierungsstatus überprüfen |
| --- |
| public void checkAuthorization(String resourceId) |

**Verfügbarkeit:** v1.0+

| API-Aufruf: Autorisierungsstatus überprüfen |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Verfügbarkeit:** v1.8+

**Parameter:**

- *resourceId*: Die ID der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Service gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Zurück zur Android-API…](#api)


### <span id="getAuthZ"></span>getAuthorization

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsfluss zu starten. Wenn der Benutzer noch nicht authentifiziert ist, wird auch der Authentifizierungsfluss initiiert. Wenn der Benutzer authentifiziert wird, fährt der Access Enabler fort, Anfragen für das Autorisierungs-Token (wenn kein gültiges Autorisierungs-Token im lokalen Token-Cache vorhanden ist) und für das kurzlebige Medien-Token zu stellen. Sobald das kurze Medien-Token abgerufen wurde, wird der Autorisierungsfluss als abgeschlossen betrachtet. Der *setToken()*-Rückruf wird ausgelöst und das kurze Medien-Token wird als Parameter an die Anwendung gesendet. Wenn die Autorisierung aus irgendeinem Grund fehlschlägt, wird der Callback *tokenRequestFailed()* ausgelöst und der Fehlercode und die Details werden bereitgestellt.

| API-Aufruf: Initiieren des Autorisierungsflusses |
| --- |
| public void getAuthorization(String resourceId) |

**Verfügbarkeit:** v1.0+

| API-Aufruf: Initiieren des Autorisierungsflusses |
| --- |
| public void getAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Verfügbarkeit:** v1.8+

**Parameter:**

- *resourceId*: Die ID der Ressource, für die der Benutzer eine Autorisierung anfordert.
- *data*: Eine Karte, die aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Service gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Zusätzliche Callbacks ausgelöst** <br> Diese Methode kann auch die folgenden Callbacks mit Trigger versehen (wenn der Authentifizierungsfluss ebenfalls initiiert wird): *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**HINWEIS: Verwenden Sie nach Möglichkeit checkAuthorization() anstelle von getAuthorization(). Die Methode getAuthorization() startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf Seiten des Programmierers führen kann.**

[Zurück zur Android-API…](#api)


### setToken {#setToken}

**Beschreibung:** Callback, ausgelöst durch den Access Enabler, der Ihrer Anwendung mitteilt, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde. Das kurzlebige Medien-Token wird ebenfalls als Parameter bereitgestellt.

| Callback: Autorisierungsfluss erfolgreich abgeschlossen |
| --- |
| public void setToken(String token, String resourceId) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *token*: Das kurzlebige Medien-Token
- *resourceId*: Die Ressource, für die die Autorisierung abgerufen wurde

**Ausgelöst durch:** `checkAuthorization()`, `getAuthorization()`


[Zurück zur Android-API…](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird, der die Anwendung der oberen Ebene darüber informiert, dass der Autorisierungsfluss fehlgeschlagen ist.

| Callback: Autorisierungsfluss fehlgeschlagen |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription) |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *resourceId*: Die Ressource, für die die Autorisierung abgerufen wurde
- *errorCode*: Fehlercode, der mit dem Fehlerszenario verknüpft ist. Mögliche Werte:
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - Der Benutzer konnte für die angegebene Ressource keine Autorisierung vornehmen.
- *errorDescription*: Zusätzliche Details zum Fehlerszenario. Wenn diese beschreibende Zeichenfolge aus irgendeinem Grund nicht verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge **(“„)**.

  Diese Zeichenfolge kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder verkaufsbezogene Meldungen zu übergeben. Wenn beispielsweise einem Abonnenten die Autorisierung für eine Ressource verweigert wird, kann der MVPD eine Nachricht senden, die wie folgt lautet: „Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie hier.“ Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Callback an den Programmierer weitergeleitet, der die Möglichkeit hat, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann diesen Parameter auch verwenden, um eine Benachrichtigung über die Bedingung bereitzustellen, die zu einem Fehler geführt haben könnte. Beispiel: „Bei der Kommunikation mit dem Autorisierungsdienst des Anbieters ist ein Netzwerkfehler aufgetreten.“

**Ausgelöst von:** `checkAuthorization(), getAuthorization()`

[Zurück zur Android-API…](#api)

### Abmelden {#logout}

**Beschreibung:** Verwenden Sie diese Methode, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Umleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Daher kann dieser Fluss nicht mit einer einfachen HTTP-Anfrage abgeschlossen werden, die von der Access Enabler-Bibliothek ausgegeben wird. Die SDK verwendet benutzerdefinierte Chrome-Registerkarten, um die HTTP-Umleitungsvorgänge auszuführen. Dieser Fluss ist für Benutzende sichtbar und wird nach Abschluss geschlossen

| API-Aufruf: Initiieren des Abmeldevorgangs |
| --- |
| public void logout() |

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:**

- `navigateToUrl()` für SDK vor 3.0
- `setAuthenticationStatus()` für SDK Version > 3.0


[Zurück zur Android-API…](#api)


### getSelectedProvider {#getSelectedProvider}

**Beschreibung:** Verwenden Sie diese Methode, um den aktuell ausgewählten Anbieter zu bestimmen.

| API-Aufruf: Bestimmen des aktuell ausgewählten MVPDS |
| --- |
| public void getSelectedProvider() |

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** `selectedProvider()`

[Zurück zur Android-API…](#api)


### <span id="selectedProvider"></span>selectedProvider

**Beschreibung:** Callback, der durch den Access Enabler ausgelöst wird, der Informationen über die aktuell ausgewählte MVPD an die Anwendung liefert.

| Callback: Informationen zur aktuell ausgewählten MVPD |
| --- |
| public void selectedProvider(Mvpd) |


**Verfügbarkeit:** v1.0+

**Parameter:**

- *mvpd*: Objekt, das Informationen zum aktuell ausgewählten MVPD enthält

**Ausgelöst von:** `getSelectedProvider()`

[Zurück zur Android-API…](#api)


### getMetadata {#getMetadata}

**Beschreibung:** Verwenden Sie diese Methode, um Informationen abzurufen, die von der Access Enabler-Bibliothek als Metadaten bereitgestellt werden. Die Anwendung kann auf diese Informationen zugreifen, indem sie ein zusammengesetztes MetadataKey-Objekt bereitstellt.

| API-Aufruf: Abfragen des AccessEnabler nach Metadaten |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Verfügbarkeit:** v1.0+

Programmierern stehen zwei Arten von Metadaten zur Verfügung:

- Statische Metadaten (Authentifizierungstoken-TTL, Autorisierungstoken-TTL und Geräte-ID)
- Benutzermetadaten (benutzerspezifische Informationen wie Benutzer-ID und Postleitzahl, die während der Authentifizierung und/oder des Autorisierungsflusses von einer MVPD an das Gerät eines Benutzers übergeben werden)

**Parameter:**

- *metadataKey*: Eine Datenstruktur, die eine Schlüssel- und eine args-Variable mit der folgenden Bedeutung einkapselt:
   - Wenn key `METADATA_KEY_USER_META` ist und args ein SerializableNameValuePair-Objekt mit name = `METADATA_ARG_USER_META` und value = `[metadata_name]` enthält, wird die Abfrage für Benutzermetadaten durchgeführt. Die aktuelle Liste der verfügbaren Benutzer-Metadatentypen:
      - `zip` - Postleitzahl

      - `householdID` - Haushaltskennung. Wenn eine MVPD keine Unterkonten unterstützt, ist dies mit `userID` identisch.

      - `maxRating` - Maximale Bewertung der Eltern für den Benutzer

      - `userID` - Die Benutzerkennung. Wenn eine MVPD Unterkonten unterstützt und der/die Benutzende nicht das Hauptkonto ist, unterscheidet sich `userID` von `householdID`.

      - `channelID` : Eine Liste der Kanäle, die der Benutzer anzeigen darf
   - Wenn der Schlüssel `METADATA_KEY_DEVICE_ID` ist, wird die Abfrage durchgeführt, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen zu Aktivierung und Gebühren zu erhalten.
   - Wenn key `METADATA_KEY_TTL_AUTHZ` ist und args ein SerializableNameValuePair-Objekt mit name = `METADATA_ARG_RESOURCE_ID` und value = `[resource_id]` enthält, wird die Abfrage ausgeführt, um die Ablaufzeit des Autorisierungstokens abzurufen, das der angegebenen Ressource zugeordnet ist.
   - Wenn der Schlüssel `METADATA_KEY_TTL_AUTHN` ist, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.



>[!NOTE]
>
>Für SDK 3.4.0 sind die Konstanten : `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` von com.adobe.adobepass.accessenabler.api.profile.UserProfileService verfügbar.



>[!NOTE]
>
>Welche Benutzermetadaten einem Programmierer tatsächlich zur Verfügung stehen, hängt davon ab, was MVPD zur Verfügung stellt.  Diese Liste wird weiter erweitert, sobald neue Metadaten verfügbar gemacht und zum Adobe Pass-Authentifizierungssystem hinzugefügt werden.

**Ausgelöste Callbacks:** [`setMetadataStatus()`](#setMetadaStatus)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

[Zurück zur Android-API…](#api)

### setMetadataStatus {#setMetadaStatus}

**Beschreibung:** Callback, der vom Access Enabler ausgelöst wird, der die über einen Aufruf von *getMetadata()* angeforderten Metadaten bereitstellt.

| Callback: Ergebnis der Anfrage zum Abrufen von Metadaten |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Verfügbarkeit:** v1.0+

**Parameter:**

- *key*: Das MetadataKey-Objekt, das den Schlüssel, für den der Metadatenwert angefordert wird, und die zugehörigen Parameter enthält (eine Referenzimplementierung finden Sie unter Demoanwendung ).
- *result*: Ein zusammengesetztes Objekt, das die angeforderten Metadaten enthält. Das -Objekt hat die folgenden Felder:
   - *simpleResult*: eine Zeichenfolge, die den Metadatenwert darstellt, als die Anfrage für die Authentifizierungs-TTL, Autorisierungs-TTL oder Geräte-ID gestellt wurde. Dieser Wert ist null, wenn die Anfrage für Benutzermetadaten durchgeführt wurde.

   - *userMetadataResult*: Ein Objekt, das die Java-Darstellung einer JSON-Benutzermetadaten-Payload enthält.\
     Beispiel:

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
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

- *Encrypted*: Boolescher Wert, der angibt, ob die abgerufenen Metadaten verschlüsselt sind oder nicht. Dieser Parameter ist nur für Benutzermetadaten-Anfragen von Bedeutung. Er hat keine Bedeutung für statische Metadaten (z. B.: Authentifizierung, TTL), die immer unverschlüsselt empfangen werden. Wenn dieser Parameter auf „True“ gesetzt ist, liegt es am Programmierer, den unverschlüsselten Wert der Benutzermetadaten durch eine RSA-Entschlüsselung mit dem privaten Schlüssel für die Whitelist abzurufen (derselbe private Schlüssel, der für die Unterzeichnung der Anforderer-ID im [`setRequestor`](#setRequestor)-Aufruf verwendet wird).

**Ausgelöst von:** [`getMetadata()`](#getMetadata)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)


[Zurück zur Android-API…](#api)


### getVersion {#getVersion}

**Beschreibung:** Mit dieser Methode kann die Version der AccessEnabler-Bibliothek abgerufen werden.

| API-Aufruf: AccessEnabler-Version abrufen |
| --- |
| ```public static String getVersion()``` |


[Zurück zur Android-API…](#api)

</br>

## Tracking von Ereignissen {#tracking}

Der Access Enabler-Trigger führt einen zusätzlichen Callback aus, der nicht unbedingt mit den Berechtigungsflüssen in Zusammenhang steht. Die Implementierung der Rückruffunktion zur Ereignisverfolgung mit dem Namen *sendTrackingData()* ist optional, ermöglicht es der Anwendung jedoch, bestimmte Ereignisse zu verfolgen und Statistiken wie die Anzahl erfolgreicher/fehlgeschlagener Authentifizierungs-/Autorisierungsversuche zu erstellen. Nachfolgend finden Sie die Spezifikation für den Callback *sendTrackingData()*:


### sendTrackingData {#sendTrackingData}

**Beschreibung:** Vom Access Enabler ausgelöster Callback, der der Anwendung das Auftreten verschiedener Ereignisse wie Abschluss/Fehler von Authentifizierungs-/Autorisierungsflüssen signalisiert. Der Gerätetyp, der Access Enabler-Clienttyp und das Betriebssystem werden ebenfalls von sendTrackingData() gemeldet.

>[!WARNING]
>
> Der Gerätetyp und das Betriebssystem werden mithilfe einer öffentlichen Java-Bibliothek ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) und der Benutzeragenten-Zeichenfolge abgeleitet. Beachten Sie, dass diese Informationen nur als grobe Möglichkeit bereitgestellt werden, Betriebsmetriken in Gerätekategorien aufzuschlüsseln, aber dass Adobe keine Verantwortung für falsche Ergebnisse übernehmen kann. Bitte verwenden Sie die neue Funktion entsprechend.


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
   - `android`

</br>

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

Im Folgenden finden Sie Anweisungen zur Interpretation der Werte in *data*
Array:

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

**Ausgelöst durch:** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Zurück zur Android-API…](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
