---
title: iOS/tvOS-API-Referenz
description: iOS/tvOS-API-Referenz
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '6933'
ht-degree: 0%

---

# iOS/tvOS SDK-API-Referenz {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#intro}

Auf dieser Seite werden die Methoden und Callback-Funktionen beschrieben, die vom nativen iOS/tvOS-Client für die Adobe Pass-Authentifizierung verfügbar gemacht werden. Die hier beschriebenen Methoden und Callback-Funktionen sind in den Header-Dateien `AccessEnabler.h` und `EntitlementDelegate.h` definiert. Sie finden sie im iOS AccessEnabler SDK hier: `[SDK directory]/AccessEnabler/headers/api/`


Zugehörige Dokumentation:

* Eine Beschreibung des grundlegenden Berechtigungsflusses für die Adobe Pass-Authentifizierung finden Sie unter [Berechtigungsfluss](/help/authentication/entitlement-flow.md).
* Schrittweise Anleitung zur Implementierung der Adobe Pass
Berechtigungsfluss der Authentifizierung mit dieser API, siehe [iOS Integration Cookbook](/help/authentication/iostvos-sdk-cookbook.md).
* Die neueste Version des iOS AccessEnabler SDK finden Sie unter [iOS Native Access Enabler Library](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe empfiehlt Ihnen, nur die Adobe Pass-Authentifizierungs-*Public* -APIs zu verwenden:
>
>* Öffentliche APIs sind für alle unterstützten Client-Typen verfügbar und wurden vollständig getestet. Für jede öffentliche Funktion stellen wir sicher, dass jeder Client-Typ über eine entsprechende Version der zugehörigen Methode(en) verfügt.
>* Öffentliche APIs müssen so stabil wie möglich sein, um die Abwärtskompatibilität zu unterstützen und sicherzustellen, dass Partnerintegrationen nicht beschädigt werden. Für nicht öffentliche APIs behalten wir uns jedoch das Recht vor, ihre Signatur zu jedem späteren Zeitpunkt zu ändern. Wenn Sie auf einen bestimmten Fluss stoßen, der nicht durch eine Kombination der aktuellen öffentlichen Adobe Pass-Authentifizierungs-API-Aufrufe unterstützt werden kann, sollten Sie uns am besten Bescheid geben. Unter Berücksichtigung Ihrer Anforderungen können wir die öffentlichen APIs ändern und eine stabile Lösung für künftige Aufgaben bereitstellen.

</br>

## API-Referenz {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Instanziiert das AccessEnabler -Objekt.

* **[VERALTET]** [init](#init) - Instanziiert das AccessEnabler-Objekt.

* [`setOptions:options:`](#setOptions) - Konfiguriert globale SDK-Optionen wie profile oder visitorID.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Legt die Identität des Programmierers fest.

* **[VERALTET]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Legt die Identität des Programmierers fest.

* **[VERALTET]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Legt die Identität des Programmierers fest.

* [`setRequestorComplete:`](#setReqComplete) - Informiert Ihre Anwendung darüber, dass die Konfigurationsphase abgeschlossen ist.

* [`checkAuthentication`](#checkAuthN) - Überprüft den Authentifizierungsstatus des aktuellen Benutzers.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - Startet den vollständigen Authentifizierungs-Workflow.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[undFilter](#getAuthN_filter) - Startet den vollständigen Authentifizierungs-Workflow.

* [`displayProviderDialog:`](#dispProvDialog) - Informiert Ihre Anwendung, die entsprechenden Elemente der Benutzeroberfläche zu instanziieren, damit der Benutzer einen MVPD auswählen kann.

* [`setSelectedProvider:`](#setSelProv) - Informiert den AccessEnabler über die MVPD-Auswahl des Benutzers.

* [`navigateToUrl:`](#nav2url) - Informiert Ihre Anwendung, dass dem Benutzer die MVPD-Anmeldeseite angezeigt werden muss.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Informiert Ihre Anwendung, dass dem Benutzer die MVPD-Anmeldeseite angezeigt werden muss, indem SFSafariViewController verwendet wird.

* [`handleExternalURL:url`](#handleExternalURL) - Schließt den Authentifizierungs-/Abmeldefluss ab.

* **[VERALTET]** [`getAuthenticationToken`](#getAuthNToken) - Fordert das Authentifizierungstoken vom Backend-Server an.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - Informiert Ihre Anwendung über den Status des Authentifizierungsflusses.

* [`checkPreauthorizedResources:`](#checkPreauth) - Stellt fest, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Stellt fest, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen.

* [`preauthorizedResources:`](#preauthResources) - Bietet eine Liste der Ressourcen, die der Benutzer bereits anzeigen darf.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Überprüft den Autorisierungsstatus des aktuellen Benutzers.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Startet den Autorisierungsfluss.

* [`setToken:forResource:`](#setToken) - Informiert Ihre Anwendung darüber, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Informiert Ihre Anwendung darüber, dass der Autorisierungsfluss fehlgeschlagen ist.

* [`logout`](#logout) - Startet den Abmeldefluss.

* [`getSelectedProvider`](#getSelProv) - Bestimmt den derzeit ausgewählten Anbieter.

* [`selectedProvider:`](#selProv) - Übermittelt Informationen über den derzeit ausgewählten MVPD an Ihre Anwendung.

* [`getMetadata:`](#getMeta) - Ruft Informationen ab, die von der AccessEnabler-Bibliothek als Metadaten bereitgestellt werden.

* [`presentTvProviderDialog:`](#presentTvDialog) - Informiert Ihre Anwendung, das Apple SSO-Dialogfeld anzuzeigen.

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Informiert Ihre Anwendung, das Apple SSO-Dialogfeld auszublenden.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Stellt die Metadaten bereit, die durch einen [`getMetadata:`](#getMeta) -Aufruf angefordert werden.

* [`sendTrackingData:forEventType:`](#sendTracking) - Liefert Tracking-Dateninformationen.

* [`MVPD`](#mvpd) - Die MVPD-Klasse. [Enthält Informationen zum MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Instanziiert das AccessEnabler -Objekt. Pro Anwendungsinstanz sollte eine einzige AccessEnabler -Instanz vorhanden sein.

| **API-Aufruf: iOS AccessEnabler-Konstruktor** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Verfügbarkeit:** v3.0+

**Parameter:**

* **software-Anweisung:** Eine Zeichenfolge, die die Anwendung im Adobe-System identifiziert. Sehen Sie sich an, wie Sie eine Software-Anweisung erhalten.

[Zurück nach oben...](#apis)



### init - [VERALTET]{#init}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Instanziiert das AccessEnabler -Objekt. Pro Anwendungsinstanz sollte eine einzige AccessEnabler -Instanz vorhanden sein.

| API-Aufruf: iOS AccessEnabler-Konstruktor |
| --- |
| ```- (id) init;``` |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

**Parameter:** None

[Zurück nach oben...](#apis)

</br>

### setOptions:options {#setOptions}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Konfiguriert globale SDK-Optionen. Es akzeptiert ein NSDictionary als Argument. Die Werte aus dem Wörterbuch werden zusammen mit jedem Netzwerkaufruf des SDK an den Server übergeben.

**Hinweis:** Die Werte werden unabhängig vom aktuellen Fluss (Authentifizierung/Autorisierung) an den Server übergeben. Wenn Sie die Werte ändern möchten, können Sie diese Methode jederzeit aufrufen.

| API-Aufruf: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Verfügbarkeit:** v2.3.0+

**Parameter:**

* *options*: Ein NSDictionary mit globalen SDK-Optionen. Derzeit sind die folgenden Optionen verfügbar:
   * **applicationProfile** - Damit können Serverkonfigurationen auf Grundlage dieses Werts erstellt werden.
   * **visitorID** - der Experience Cloud ID-Dienst. Dieser Wert kann später für erweiterte Analyseberichte verwendet werden.
   * **handleSVC** - Boolescher Wert, der angibt, ob der Programmierer die SFSafariViewControllers verarbeitet. Weitere Informationen finden Sie unter [SFSafariViewController-Unterstützung für iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) .
      * Wenn der Wert auf **false,** festgelegt ist, präsentiert das SDK den Endbenutzer automatisch mit einem SFSafariViewController. Das SDK navigiert weiter zur URL der MVPDs-Anmeldeseite.
      * Wenn der Wert auf **true,** festgelegt ist, stellt das SDK **NOT** den Endbenutzer automatisch mit einem SFSafariViewController dar. Das SDK führt den Trigger **navigate(toUrl:{url}, useSVC:YES)** weiter aus.
* **device\_info** - Client-Informationen, wie in [Weitergeben von Client-Informationen](/help/authentication/passing-client-information-device-connection-and-application.md) beschrieben.

[Zurück nach oben...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Beim Umgang mit SSO und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gestellt wird, um mit dem Systemstatus zu synchronisieren (Abrufen eines Remote-Tokens, wenn SSO aktiviert ist, oder Löschen des lokalen Tokens, wenn in der Zwischenzeit ein Abmelden stattgefunden hat).

Die Server-Antwort enthält eine Liste von MVPDs zusammen mit einigen Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Serverantwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. SUCCESS/FAIL) wird Ihrer Anwendung über den Rückruf `setRequestorComplete:` angezeigt.

Wenn der Parameter `urls` nicht verwendet wird, wird der resultierende Netzwerkaufruf an die Standard-Service-Provider-URL gesendet: die Adobe-VERSION/Produktionsumgebung.


Wenn ein Wert für den Parameter `urls` angegeben wird, werden alle im Parameter `urls` angegebenen URLs vom resultierenden Netzwerkaufruf als Ziel ausgewählt. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Der erste Antwortsender hat beim Kompilieren der Liste der MVPDs Vorrang. Für jeden MVPD in der Liste speichert der AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die dem Dienstanbieter zugeordnet ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Verfügbarkeit:** v3.0+

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Verfügbarkeit:** v3.0+

**Parameter:**

* *requestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie die von Adobe zugewiesene eindeutige ID an Ihre Site, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registrieren.
* *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.

>[!NOTE]
>
>Wenn der Aufruf ohne den Parameter `serviceProviders` erfolgt, ruft die Bibliothek die Konfiguration vom Standarddienstleister ab (d. h. `https://sp.auth.adobe.com` für das Produktionsprofil oder `https://sp.auth-staging.adobe.com` für das Staging-Profil). Wenn der Parameter `serviceProviders` angegeben wird, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn in verschiedenen Antworten des Dienstleisters doppelte Informationen vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers behoben (d. h. der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Rückrufe:** [`setRequestorComplete:`](#setReqComplete)

[Zurück nach oben...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [VERALTET] {#setReq}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gestellt wird, um mit dem Systemstatus zu synchronisieren (Abrufen eines Remote-Tokens, wenn SSO aktiviert ist, oder Löschen des lokalen Tokens, wenn in der Zwischenzeit ein Abmelden stattgefunden hat).

Die Server-Antwort enthält eine Liste von MVPDs zusammen mit einigen Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Serverantwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. SUCCESS/FAIL) wird Ihrer Anwendung über den Rückruf `setRequestorComplete:` angezeigt.

Wenn der Parameter `urls` nicht verwendet wird, wird der resultierende Netzwerkaufruf an die Standard-Service-Provider-URL gesendet: die Adobe-VERSION/Produktionsumgebung.

Wenn ein Wert für den Parameter `urls` angegeben wird, werden alle im Parameter `urls` angegebenen URLs vom resultierenden Netzwerkaufruf als Ziel ausgewählt. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Der erste Antwortsender hat beim Kompilieren der Liste der MVPDs Vorrang. Für jeden MVPD in der Liste speichert der AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die dem Dienstanbieter zugeordnet ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

**Parameter:**

* *requestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie die eindeutige ID, die von Adobe zugewiesen wurde, an Ihre Site, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
* *signedRequestorID*: **Dieser Parameter ist in den iOS AccessEnabler-Versionen 1.2 und höher vorhanden.** Eine Kopie der Anforderer-ID, die digital mit Ihrem privaten Schlüssel signiert ist. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->
* *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.

**Hinweise:** Wenn diese Funktion ohne den Parameter `serviceProviders` aufgerufen wird, ruft die Bibliothek die Konfiguration vom standardmäßigen Dienstanbieter ab (d. h. `https://sp.auth.adobe.com` für das Produktionsprofil oder `https://sp.auth-staging.adobe.com` für das Staging-Profil). Wenn der Parameter `serviceProviders` angegeben wird, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn in verschiedenen Antworten des Dienstleisters doppelte Informationen vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers behoben (d. h. der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Rückrufe:** [`setRequestorComplete:`](#setReqComplete)


[Zurück nach oben...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [VERALTET] {#setReq_tvos}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Diese Einstellung sollte nur einmal während des Lebenszyklus der Anwendung vorgenommen werden.

Die Server-Antwort enthält eine Liste von MVPDs zusammen mit einigen Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Serverantwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. SUCCESS/FAIL) wird Ihrer Anwendung über den Rückruf `setRequestorComplete:` angezeigt.

Wenn der Parameter `urls` nicht verwendet wird, wird der resultierende Netzwerkaufruf an die Standard-Service-Provider-URL gesendet: die Adobe-VERSION/Produktionsumgebung.

Wenn ein Wert für den Parameter `urls` angegeben wird, werden alle im Parameter `urls` angegebenen URLs vom resultierenden Netzwerkaufruf als Ziel ausgewählt. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Der erste Antwortsender hat beim Kompilieren der Liste der MVPDs Vorrang. Für jeden MVPD in der Liste speichert der AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die dem Dienstanbieter zugeordnet ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Konfiguration des Anforderers</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**Verfügbarkeit:** v2.0+ **Bis:** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Konfiguration des Anforderers</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v2.0+ **Bis:** v3.0

**Parameter:**

* *requestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie beim ersten Mal die von Adobe zugewiesene eindeutige ID an Ihre Site.   beim Adobe Pass-Authentifizierungsdienst registriert ist.
* *signedRequestID*: **Dieser Parameter ist in iOS AccessEnabler vorhanden   Versionen 1.2 und höher.** Eine Kopie der Anforderer-ID, die digital mit Ihrem privaten Schlüssel signiert ist. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->
* *urls*: Optionaler Parameter; standardmäßig der Adobe-Dienstleister   wird verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.
* secret und publicKey: Der geheime und öffentliche Schlüssel, der zum Signieren der zweiten Bildschirmaufrufe verwendet wird. Weitere Informationen finden Sie in der [clienteless-Dokumentation](#create_dev).

Wenn sie ohne den Parameter `serviceProviders` aufgerufen wird, ruft die Bibliothek die Konfiguration vom Standarddienstleister ab (d. h. `https://sp.auth.adobe.com` für das Produktionsprofil oder https://sp.auth-staging.adobe.com für das Staging-Profil). Wenn der Parameter `serviceProviders` angegeben wird, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn in verschiedenen Antworten des Dienstleisters doppelte Informationen vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers behoben (d. h. der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Rückrufe:** [`setRequestorComplete:`](#setReqComplete)

[Zurück nach oben...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch den AccessEnabler ausgelöster Rückruf, der Ihre Anwendung darüber informiert, dass die Konfigurationsphase abgeschlossen ist. Dies ist ein Signal, dass die App mit der Ausgabe von Berechtigungsanfragen beginnen kann. Bis zum Abschluss der Konfigurationsphase kann die Anwendung keine Berechtigungsanfragen mehr stellen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Konfiguration des Anforderers abgeschlossen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**Verfügbarkeit:** v1.0+

**Parameter**:

* *status*: kann einen der folgenden Werte annehmen:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - Konfigurationsphase wurde erfolgreich abgeschlossen
   * `ACCESS_ENABLER_STATUS_ERROR` - Konfigurationsphase fehlgeschlagen

**ausgelöst von:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Zurück nach oben...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Überprüft den Authentifizierungsstatus des aktuellen Benutzers.
Dazu wird nach einem gültigen Authentifizierungstoken in der lokalen
Token-Speicherplatz. Diese Methode führt keine Netzwerkaufrufe durch. Wir empfehlen, sie im Haupt-Thread aufzurufen.
Sie wird von der Anwendung verwendet, um den Authentifizierungsstatus des Benutzers abzufragen und
Aktualisieren Sie die Benutzeroberfläche entsprechend (d. h. aktualisieren Sie die Anmelde-/Abmelde-Benutzeroberfläche). Die
Der Authentifizierungsstatus wird der Anwendung über
den Rückruf [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) .


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Authentifizierungsstatus überprüfen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Zurück nach oben...](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Zunächst wird der Authentifizierungsstatus überprüft. Falls noch nicht authentifiziert, wird der Zustandsmaschine für den Authentifizierungsfluss gestartet:

* wenn der letzte Authentifizierungsversuch erfolgreich war, der MVPD   Auswahlphase wird übersprungen und   Der Rückruf [`navigateToUrl:`](#nav2url) wird ausgelöst. Die   verwendet diesen Rückruf, um das WebView-Steuerelement zu instanziieren, das dem Benutzer die Anmeldeseite des MVPD anzeigt. **[HINWEIS: Ab Access Enabler 1.5 ist diese Funktion aufgrund einer Beschränkung im SDK nicht verfügbar.]**
* Wenn der letzte Authentifizierungsversuch fehlgeschlagen ist oder der Benutzer sich explizit abgemeldet hat, lautet der Rückruf [`displayProviderDialog:`](#dispProvDialog) .   ausgelöst. Ihre Anwendung verwendet diesen Rückruf, um die MVPD-Auswahlbenutzeroberfläche anzuzeigen. Außerdem muss Ihre App den Authentifizierungsfluss fortsetzen, indem sie die AccessEnabler-Bibliothek über die Methode [`setSelectedProvider:`](#setSelProv) über die MVPD-Auswahl des Benutzers informiert.

Da die Anmeldeinformationen des Benutzers auf der MVPD-Anmeldeseite überprüft werden, muss Ihre Anwendung die verschiedenen Weiterleitungsvorgänge überwachen, die während der Authentifizierung des Benutzers auf der Anmeldeseite des MVPD stattfinden. Wenn die richtigen Anmeldeinformationen eingegeben werden, wird das WebView-Steuerelement zu einer benutzerdefinierten URL umgeleitet, die von der `ADOBEPASS_REDIRECT_URL` -Konstante definiert wird. Diese URL soll nicht von WebView geladen werden. Die Anwendung muss diese URL abfangen und dieses Ereignis als Signal interpretieren, dass die Anmeldungsphase abgeschlossen ist. Anschließend sollte die Steuerung an den AccessEnabler übergeben werden, um den Authentifizierungsfluss abzuschließen (durch Aufruf der Methode [handleExternalURL](#handleExternalURL) ).

Schließlich wird der Authentifizierungsstatus über den Rückruf [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) an die Anwendung übermittelt.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: initiiert den Authentifizierungsfluss</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: initiiert den Authentifizierungsfluss</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Verfügbarkeit:** v1.9+

**Parameter:**

* *forceAuthn*: Eine Markierung, die angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Zurück nach oben...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Zunächst wird der Authentifizierungsstatus überprüft. Falls noch nicht authentifiziert, wird der Zustandsmaschine für den Authentifizierungsfluss gestartet:

* [presentTvProviderDialog()](#presentTvDialog) wird aufgerufen, wenn der aktuelle Anfragende über mindestens einen MVPD verfügt, der SSO unterstützt. Wenn kein MVPD SSO unterstützt, beginnt der klassische Authentifizierungsfluss und der Filterparameter wird ignoriert.
* Nachdem der Benutzer den Apple SSO-Fluss abgeschlossen hat, wird [`dismissTvProviderDialog()`](#dismissTvDialog) ausgelöst und der Authentifizierungsprozess wird abgeschlossen.

Schließlich wird der Authentifizierungsstatus über den Rückruf [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) an die Anwendung übermittelt.

**Verfügbarkeit:** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: initiiert den Authentifizierungsfluss</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: initiiert den Authentifizierungsfluss</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>
 </div></td>
</tr>
</tbody>
</table>



**Parameter:**

* *forceAuthn*: Eine Markierung, die angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.
* filter: Ein Wörterbuch mit zwei Listen von MVPD-IDs, die im Apple SSO-Dialogfeld angezeigt werden sollen. Alle MVPD, die SSO nicht unterstützen, werden ignoriert, aber die Reihenfolge wird eingehalten. Das Wörterbuch muss über zwei Schlüssel verfügen:
   * TV\_PROVIDERS: Eine Liste mit allen MVPDs, die in der Auswahl angezeigt werden sollen
   * FEATURED\_TV\_PROVIDERS: Eine Liste mit allen MVPDs, die in der Auswahl als dargestellt markiert werden sollen. MVPDs in dieser Liste müssen auch in der Liste TV\_PROVIDERS angegeben werden.

**Verfügbarkeit:** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: initiiert den Authentifizierungsfluss</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: initiiert den Authentifizierungsfluss</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Parameter:**

* *forceAuthn*: Eine Markierung, die angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.
* filter: Eine Liste der MVPD-IDs, die im Apple SSO-Dialogfeld angezeigt werden sollen. Alle MVPD, die SSO nicht unterstützen, werden ignoriert, aber die Reihenfolge wird eingehalten.

**Ausgelöste Rückrufe:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Zurück nach oben...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch den AccessEnabler ausgelöster Rückruf, um die Anwendung darüber zu informieren, dass die entsprechenden Elemente der Benutzeroberfläche instanziiert werden müssen, damit der Benutzer das gewünschte MVPD auswählen kann. Der Rückruf bietet eine Liste von MVPD-Objekten mit zusätzlichen Informationen, die dazu beitragen können, das Auswahlbenutzeroberflächenbedienfeld korrekt zu erstellen (z. B. die URL, die auf das MVPD-Logo verweist, der Anzeigename usw.).

Nachdem der Benutzer das gewünschte MVPD ausgewählt hat, muss die obere Ebene-Anwendung den Authentifizierungsfluss fortsetzen, indem sie `setSelectedProvider:` aufruft und die Kennung des MVPD übergibt, die der Auswahl des Benutzers entspricht.

**Abbruch des Authentifizierungsflusses** - Dies ist ein Punkt, an dem der Benutzer die Schaltfläche &quot;Zurück&quot;drücken kann, was dem Abbrechen des Authentifizierungsflusses entspricht. In diesem Szenario muss Ihre Anwendung die Methode [setSelectedProvider:](#setSelProv) aufrufen und dabei null als Parameter übergeben, damit der AccessEnabler die Möglichkeit erhält, seinen Authentifizierungsstatus-Computer zurückzusetzen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Anzeigen der MVPD-Auswahlbenutzeroberfläche</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**Verfügbarkeit:** v1.0+

**Parameter**:

* *mvpds*: Liste der MVPD-Objekte mit MVPD-bezogenen Informationen, die die Anwendung zum Erstellen der Elemente der MVPD-Auswahlbenutzeroberfläche verwenden kann.

**ausgelöst durch:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Zurück nach oben...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Access Enabler über die MVPD-Auswahl des Benutzers zu informieren. Die Anwendung kann diese Methode verwenden, um den für die Authentifizierung verwendeten Dienstleister auszuwählen oder zu ändern.

Wenn der ausgewählte MVPD ein TempPass MVPD ist, authentifiziert er sich automatisch mit diesem MVPD, ohne später getAuthentication() aufrufen zu müssen.

Bitte beachten Sie, dass dies bei Promotional Temp Pass nicht möglich ist, wenn zusätzliche Parameter in der getAuthentication() -Methode angegeben werden.

Wenn *null* als Parameter übergeben wird, geht der Access Enabler davon aus, dass der Benutzer den Authentifizierungsfluss abgebrochen hat (d. h. durch Drücken der &quot;Zurück&quot;-Schaltfläche), und reagiert, indem er den Authentifizierungsstatus-Computer zurücksetzt und den [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) -Rückruf mit dem Fehler-Code `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` aufruft.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `setAuthenticationStatus:errorCode:`, `sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Zurück nach oben...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback, der vom AccessEnabler ausgelöst wird, um Ihre Anwendung aufzufordern, einen UIWebView/WKWebView-Controller zu instanziieren und die URL zu laden, die im Parameter **`url`** des Callbacks angegeben ist. Der Rückruf übergibt den Parameter **`url`** , der die URL des Authentifizierungsendpunkts oder die URL des Abmelde-Endpunkts darstellt.

Da der Controller UIWebView/WKWebView` `mehrere Umleitungen durchläuft, muss Ihre Anwendung die Aktivität des Controllers überwachen und den Zeitpunkt erkennen, zu dem eine bestimmte benutzerdefinierte URL geladen wird, die von der `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`) definiert wird. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Authentifizierungs- oder Abmeldevorgang abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung die UIWebView/WKWebView schließen und die `handleExternalURL:url `API-Methode von AccessEnabler aufrufen.

**Hinweis:** Bitte beachten Sie, dass im Falle eines Authentifizierungsflusses dieser Punkt ist, an dem der Benutzer die Schaltfläche &quot;Zurück&quot;drücken kann, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss Ihre Anwendung die Methode [setSelectedProvider:](#setSelProv) aufrufen, die **`nil`** als Parameter übergibt und dem AccessEnabler die Möglichkeit gibt, seinen Authentifizierungsstatus-Computer zurückzusetzen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: MVPD-Anmeldeseite anzeigen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter**:

* *url*: die URL, die auf die Anmeldeseite des MVPD verweist

**Ausgelöst von:** [setSelectedProvider:](#setSelProv)



[Zurück nach oben...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback, der vom AccessEnabler anstelle des `navigateToUrl:` -Callbacks ausgelöst wird, falls Ihre Anwendung zuvor die manuelle Verarbeitung des Safari View Controller (SVC) über den Aufruf [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) aktiviert hat, und nur bei MVPDs, die einen Safari View Controller (SVC) erfordern. Bei allen anderen MVPDs wird der Rückruf `navigateToUrl:` aufgerufen. Weitere Informationen dazu, wie der Safari View Controller (SVC) verwaltet werden soll, finden Sie unter[SFSafariViewController-Unterstützung für iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) .

Ähnlich wie der `navigateToUrl:` -Rückruf wird der `navigateToUrl:useSVC:` vom AccessEnabler ausgelöst, um Ihre Anwendung aufzufordern, einen `SFSafariViewController` -Controller zu instanziieren und die URL zu laden, die im **`url`** -Parameter des Rückrufs bereitgestellt wird. Der Rückruf übergibt den Parameter **`url`** , der die URL des Authentifizierungsendpunkts oder die URL des Abmelde-Endpunkts darstellt, sowie den Parameter **`useSVC`** , der angibt, dass die Anwendung einen `SFSafariViewController` verwenden muss.

Da der `SFSafariViewController` -Controller mehrere Umleitungen durchläuft, muss Ihre Anwendung die Aktivität des Controllers überwachen und den Zeitpunkt erkennen, zu dem sie eine bestimmte benutzerdefinierte URL lädt, die von Ihrem `application's custom scheme` definiert wurde (z. B.** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Authentifizierungs- oder Abmeldevorgang abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung die `SFSafariViewController` schließen und die `handleExternalURL:url `API-Methode von AccessEnabler aufrufen.

**Hinweis:** Bitte beachten Sie, dass im Falle eines Authentifizierungsflusses dieser Punkt ist, an dem der Benutzer die Schaltfläche &quot;Zurück&quot;drücken kann, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss Ihre Anwendung die Methode [setSelectedProvider:](#setSelProv) aufrufen, die **`nil`** als Parameter übergibt und dem AccessEnabler die Möglichkeit gibt, seinen Authentifizierungsstatus-Computer zurückzusetzen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Anzeige der MVPD-Anmeldeseite in SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:**Version 3.2+

**Parameter**:

* *url:* die URL, die auf die Anmeldeseite des MVPD verweist
* *useSVC:* Angabe, ob die URL in SFSafariViewController geladen werden soll.

**Ausgelöst von:**[ setOptions:](#setOptions) vor [setSelectedProvider:](#setSelProv)

[Zurück nach oben...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Authentifizierungs- oder Abmeldefluss abzuschließen. Diese Methode sollte direkt aufgerufen werden, nachdem Ihre Anwendung erkennt, wann der `UIWebView/WKWebView or SFSafariViewController`-Controller an eine bestimmte benutzerdefinierte URL umgeleitet wird. Falls Ihre Anwendung einen `SFSafariViewController `Controller verwenden muss, wird die spezifische benutzerdefinierte URL durch Ihren `application's custom scheme` definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`) definiert.

Im Fall eines Authentifizierungsflusses schließt der AccessEnabler den Ablauf ab, indem er das Authentifizierungstoken vom Backend-Server abruft und lokal im Token-Speicher speichert. Der AccessEnabler informiert Ihre Anwendung darüber, dass der Authentifizierungsfluss abgeschlossen ist, indem er den `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> -Rückruf mit dem Status-Code 1 aufruft und auf Erfolg hinweist. Wenn bei der Ausführung dieser Schritte ein Fehler auftritt, wird der Rückruf `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> mit dem Statuscode 0 ausgelöst, was auf einen Authentifizierungsfehler sowie einen entsprechenden Fehlercode hinweist.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Authentifizierungs- oder Abmeldefluss abschließen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v3.0+

**Parameter:**

* *url*: Die abgefangene URL aus dem ` UIWebView/WKWebView or SFSafariViewController `-Steuerelement als Zeichenfolge.


**Ausgelöste Rückrufe:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Zurück nach oben...](#apis)

</br>

#### getAuthenticationToken - [VERALTET] {#getAuthNToken}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Schließt den Authentifizierungsfluss ab, indem das Authentifizierungstoken vom Backend-Server angefordert wird. Diese Methode sollte von Ihrer Anwendung nur als Reaktion auf ein Ereignis aufgerufen werden, bei dem das WebView-Steuerelement, das die MVPD-Anmeldeseite hostet, an die von der `ADOBEPASS_REDIRECT_URL` -Konstante definierte benutzerdefinierte URL umgeleitet wird.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Authentifizierungstoken abrufen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

**Parameter:** None

**Ausgelöste Rückrufe:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Zurück nach oben...](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch den AccessEnabler ausgelöster Rückruf, der die Anwendung über den Status des Authentifizierungsflusses informiert. Es gibt viele Stellen, an denen dieser Fluss fehlschlagen kann, entweder aufgrund der Interaktion des Benutzers oder aufgrund anderer unvorhergesehener Szenarien (z. B. Probleme bei der Netzwerkverbindung usw.). Dieser Rückruf informiert die Anwendung über den Erfolgs-/Fehlerstatus des Authentifizierungsflusses und liefert bei Bedarf zusätzliche Informationen zum Fehlergrund.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Melden Sie den Status des Authentifizierungsflusses</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**Verfügbarkeit:** v1.0+

**Parameter**:

* *status*: kann einen der folgenden Werte annehmen:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - Authentifizierungsfluss wurde erfolgreich abgeschlossen
   * `ACCESS_ENABLER_STATUS_ERROR` - Authentifizierungsfluss fehlgeschlagen
* *code*: Grund für den Fehler. Wenn *status* `ACCESS_ENABLER_STATUS_SUCCESS` ist, ist *code* eine leere Zeichenfolge (d. h. durch die `USER_AUTHENTICATED` -Konstante definiert). Im Falle eines Fehlers kann dieser Parameter einen der folgenden Werte annehmen:
   * `USER_NOT_AUTHENTICATED_ERROR` - Der Benutzer ist nicht authentifiziert. Als Antwort auf den Methodenaufruf [checkAuthentication:](#checkAuthN) , wenn im lokalen Token-Cache kein gültiges Authentifizierungstoken vorhanden ist.
   * `PROVIDER_NOT_SELECTED_ERROR` - Der AccessEnabler hat die       Authentifizierungsstatusmaschine nach der Anwendung der oberen Ebene       *null* an [`setSelectedProvider:`](#setSelProv) übergeben, um den Authentifizierungsfluss abzubrechen.  Vermutlich hat der Benutzer den Authentifizierungsfluss abgebrochen (d. h. die &quot;Zurück&quot;-Schaltfläche gedrückt).
   * `GENERIC_AUTHENTICATION_ERROR` - Der Authentifizierungsfluss schlug aus Gründen wie z. B. Nichtverfügbarkeit des Netzwerks fehl oder der Benutzer hat den Authentifizierungsfluss explizit abgebrochen.

**ausgelöst durch:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Zurück nach oben...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der Hauptzweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden, **(z. B. zur Angabe des Zugriffsstatus mit Schloss- und Entsperrungssymbolen).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.3+

**Parameter:**

* *resources:* Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die     Die Ressourcen-ID unterliegt den gleichen Einschränkungen wie die Ressourcen-ID im Aufruf, d. h. es sollte sich um einen vereinbarten Wert handeln, der zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment festgelegt wurde.

**Callback ausgelöst:** [`preauthorizedResources:`](#preauthResources)

[Zurück nach oben...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der Hauptzweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden (z. B. zur Angabe des Zugriffs mit Schloss- und Entsperrungssymbolen). Der Parameter **cache** steuert, ob der interne Cache zum Auflösen von Ressourcen verwendet wird.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**Verfügbarkeit:** v3.1+



**Parameter:**

* *resources:* Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization:` -Aufruf, d. h. es sollte sich um einen vereinbarten Wert handeln, der zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment festgelegt wurde.
* *cache:* Boolescher Wert, der angibt, ob der interne Cache zum Auflösen von Ressourcen verwendet werden soll. Bei &quot;false&quot;wird der Cache umgangen, was zu Server-Aufrufen bei jedem Aufruf dieser API führt.

**Callback ausgelöst:** [`preauthorizedResources:`](#preauthResources)

[Zurück nach oben...](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback, der durch `checkPreauthorizedResources:` ausgelöst wird. Bietet eine Liste der Ressourcen, für die der Benutzer bereits autorisiert ist.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Legen Sie den aktuell ausgewählten Provider fest.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.3+

**Parameter:**

* `resources`: Array von Ressourcen, für die der Benutzer bereits zur Anzeige berechtigt ist.

**ausgelöst von:** [`checkPreauthorizedResources:`](#checkPreauth)



[Zurück nach oben...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn sie nicht authentifiziert ist, wird der Rückruf [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) ausgelöst und die Methode wird beendet. Wenn der Benutzer authentifiziert ist, wird auch der Autorisierungsfluss Trigger. Siehe Details zur [`getAuthorization:`](#getAuthZ) -Methode.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Überprüfen des Autorisierungsstatus</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Überprüfen des Autorisierungsstatus</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.9+

**Parameter:**

* *resource*: Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed), `setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Zurück nach oben...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsfluss zu initiieren. Wenn der Benutzer noch nicht authentifiziert ist, wird auch der Authentifizierungsfluss initiiert. Wenn der Benutzer authentifiziert wird, stellt der AccessEnabler weiterhin Anforderungen an das Autorisierungstoken (wenn im lokalen Token-Cache kein gültiges Autorisierungstoken vorhanden ist) und an das Token für kurzlebige Medien. Sobald das Short-Media-Token abgerufen wurde, wird der Autorisierungsfluss als vollständig betrachtet. Der Rückruf [`setToken:forResource:`](#setToken) wird ausgelöst und das Short-Media-Token wird als Parameter an die Anwendung gesendet. Wenn die Autorisierung aus irgendeinem Grund fehlschlägt, wird der Rückruf [`tokenRequestFailed:forEventType:`](#tokenReqFailed) ausgelöst und der Fehlercode/die Details werden bereitgestellt.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Initiieren des Autorisierungsflusses</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Initiieren des Autorisierungsflusses</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**Verfügbarkeit:** v1.9+

**Parameter:**

* *resource*: Die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht und an den Pay-TV-Pass-Dienst gesendet wird. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Zusätzliche ausgelöste Rückrufe:**\
Diese Methode kann auch die folgenden Rückrufe Trigger werden (wenn der Authentifizierungsfluss ebenfalls initiiert wird): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Verwenden Sie nach Möglichkeit `checkAuthorization:` / `checkAuthorization:withData:` anstelle von `getAuthorization:` / `getAuthorization:withData:`. Die Methode `getAuthorization:` / `getAuthorization:withData:` startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf der Seite des Programmierers führen könnte.

[Zurück nach oben...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch den AccessEnabler ausgelöster Rückruf, der Ihre Anwendung darüber informiert, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde. Das kurzlebige Medien-Token wird auch als Parameter bereitgestellt.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Autorisierungsfluss erfolgreich abgeschlossen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter**:

* *token*: Das kurzlebige Medien-Token
* *resource*: die Ressource, für die die Autorisierung abgerufen wurde

**ausgelöst durch:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Zurück nach oben...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch den AccessEnabler ausgelöster Rückruf, der die Anwendung auf der obersten Ebene darüber informiert, dass der Autorisierungsfluss fehlgeschlagen ist.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Autorisierungsfluss fehlgeschlagen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter**:

* *resource*: Die Ressource, für die die Autorisierung abgerufen wurde.
* *code*: Der mit dem Fehlerszenario verbundene Fehlercode. Mögliche Werte:
   * `USER_NOT_AUTHORIZED_ERROR` - Der Benutzer konnte nicht autorisieren
für die jeweilige Ressource
* *description*: Zusätzliche Details zum Fehlerszenario. Wenn diese beschreibende Zeichenfolge aus irgendeinem Grund nicht verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge **(&quot;&quot;)**.\
  Diese Zeichenfolge kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder umsatzbezogene Nachrichten zu übergeben. Wenn einem Abonnenten beispielsweise die Autorisierung für eine Ressource verweigert wird, könnte der MVPD eine Nachricht wie die folgende senden: &quot;Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie auf **hier**.&quot; Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Rückruf an den Programmierer übergeben, der die Möglichkeit hat, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann diesen Parameter auch verwenden, um eine Benachrichtigung über die Bedingung bereitzustellen, die möglicherweise zu einem Fehler geführt hat. Beispiel: &quot;Bei der Kommunikation mit dem Autorisierungsdienst des Providers ist ein Netzwerkfehler aufgetreten&quot;.

**ausgelöst durch:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Zurück nach oben...](#apis)

</br>

### Abmelden {#logout}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Weiterleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Da dieser Fluss nicht mit einer einfachen HTTP-Anforderung abgeschlossen werden kann, die von der AccessEnabler-Bibliothek ausgegeben wird, muss ein `UIWebView/WKWebView or SFSafariViewController` -Controller instanziiert werden, um den HTTP-Weiterleitungsvorgängen folgen zu können.

Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss insofern, als der Benutzer in keiner Weise mit dem `UIWebView/WKWebView or SFSafariViewController`-Controller interagieren muss. Daher empfiehlt Adobe, das Steuerelement während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.

Es wird ein dem Authentifizierungsfluss ähnliches Muster verwendet. Der iOS AccessEnabler Trigger den Rückruf `navigateToUrl:` oder den `navigateToUrl:useSVC:`, um einen `UIWebView/WKWebView or SFSafariViewController` -Controller zu erstellen und die im `url` -Parameter des Rückrufs angegebene URL zu laden. Dies ist die URL des Abmelde-Endpunkts auf dem Backend-Server. Für tvOS AccessEnabler wird weder der Rückruf `navigateToUrl:` noch der Rückruf `navigateToUrl:useSVC:` aufgerufen.

Da mehrere Umleitungen ausgeführt werden, muss Ihre Anwendung die Aktivität des `UIWebView/WKWebView or SFSafariViewController `Controllers überwachen und den Zeitpunkt ermitteln, zu dem sie eine bestimmte benutzerdefinierte URL lädt. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Abmeldefluss abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung den Controller schließen und die `handleExternalURL:url `API-Methode von AccessEnabler aufrufen. Falls Ihre Anwendung einen `SFSafariViewController `Controller verwenden muss, wird die spezifische benutzerdefinierte URL durch Ihren `application's custom scheme` definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`) definiert.

Am Ende ruft der AccessEnabler den Rückruf [`setAuthenticationStatus()`](#setAuthNStatus) mit dem Status-Code 0 auf und zeigt an, dass der Abmeldefluss erfolgreich war.

**Hinweis:** Wenn der Benutzer über die Apple SSO angemeldet ist, wird der VSA203-Status ausgelöst. Ist dies der Fall, sollte der Benutzer angewiesen werden, sich auch aus den Systemeinstellungen abzumelden. Andernfalls wird eine erneute Authentifizierung durchgeführt, wenn die Anwendung neu gestartet wird.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Logout-Fluss initiieren</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Zurück nach oben...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Verwenden Sie diese Methode, um den derzeit ausgewählten Anbieter zu bestimmen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Bestimmen Sie den aktuell ausgewählten MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter:** None

**Ausgelöste Rückrufe:** [`selectedProvider:`](#selProv)

[Zurück nach oben...](#apis)

</br>

### selectedProvider {#selProv}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, der vom AccessEnabler ausgelöst wird und Informationen über den aktuell ausgewählten MVPD für die Anwendung bereitstellt.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Informationen zum aktuell ausgewählten MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter**:

* *mvpd*: Objekt mit Informationen zum aktuell ausgewählten MVPD

**ausgelöst von:** [`getSelectedProvider`](#getSelProv)

[Zurück nach oben...](#apis)

</br>

### getMetadata: {#getMeta}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Verwenden Sie diese Methode, um Informationen abzurufen, die von der AccessEnabler-Bibliothek als Metadaten bereitgestellt werden. Die Anwendung kann auf diese Daten zugreifen, indem sie einen wörterbuchbasierten Eingabeparameter *key* bereitstellt.

Programmierern stehen zwei Metadatentypen zur Verfügung:

* Statische Metadaten (Authentifizierungstoken TTL, Autorisierungstoken TTL und Geräte-ID)
* Benutzermetadaten (benutzerspezifische Informationen wie Benutzer-ID, Postleitzahl; können während der Authentifizierungs- und Autorisierungsflüsse von einem MVPD an das Gerät eines Benutzers übergeben werden)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Abfrage des AccessEnabler für Metadaten</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter:**

* *keyDictionary*: eine Wörterbuchdatenstruktur mit der folgenden
format:
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_AUTHENTICATION` ist, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_AUTHORIZATION` **und** ist\
     Schlüssel ist `METADATA_RESOURCE_ID_KEY` und Wert eine bestimmte Ressourcen-ID ist, wird die Abfrage ausgeführt, um die Ablaufzeit des Autorisierungstokens zu erhalten, das mit der angegebenen Ressource verknüpft ist.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_DEVICE_ID` ist, wird die Abfrage zum Abrufen der aktuellen Geräte-ID durchgeführt. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen über Aktivierung und Gebühren zu erhalten.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_USER_META` **und der Schlüssel** den Wert `METADATA_USER_META_KEY` aufweist und der Wert der Name der Metadaten ist, wird die Abfrage für Benutzermetadaten durchgeführt. Die Liste der verfügbaren Benutzer-Metadatentypen:
      * `zip` - Liste der Postleitzahlen
      * `householdID` - Kennung des Haushalts. Wenn ein MVPD keine Unterkonten unterstützt, ist dies identisch mit `userID`.
      * `maxRating` - Eine Sammlung von maximalen elterlichen Bewertungen für den Benutzer
      * `userID` - Die Benutzer-ID. Wenn ein MVPD Subkonten unterstützt und der Benutzer nicht das Hauptkonto ist, unterscheidet sich `userID` von `householdID.`
      * `channelID` - Eine Liste der Kanäle, die ein Benutzer anzeigen darf.

  >[!NOTE]
  >
  >Die tatsächlichen Benutzermetadaten, die einem Programmierer zur Verfügung stehen, hängen davon ab, was ein MVPD zur Verfügung stellt. Diese Liste wird erweitert, sobald neue Metadaten verfügbar gemacht und dem Adobe Pass-Authentifizierungssystem hinzugefügt werden.

**Ausgelöste Rückrufe:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/user-metadata.md)

[Zurück nach oben...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, der vom AccessEnabler nach dem Aufruf von[getAuthentication()](#getAuthN) ausgelöst wird, wenn der aktuelle Anforderer mindestens einen MVPD mit SSO-Unterstützung unterstützt.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Ergebnis der SSO-Flüsse</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v2.0+

**Parameter**:

* viewController: steht für das Apple SSO-Dialogfeld. Dieser viewController muss auf dem Bildschirm angezeigt werden.

**ausgelöst von:** [`getAuthentication`](#getAuthN)

**Weitere Informationen:** [iOS/tvOS Single Sign On](#presentTvDialog)

[Zurück nach oben...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, der vom AccessEnabler ausgelöst wird, nachdem der Benutzer das Apple SSO-Dialogfeld geschlossen hat.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Ergebnis der SSO-Flüsse</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v2.0+

**Parameter**:

* viewController: steht für das Apple SSO-Dialogfeld. Dieser viewController muss aus dem Bildschirm entfernt werden.

**Ausgelöst von:** Benutzeraktion

**Weitere Informationen:** [iOS/tvOS Single Sign On](#presentTvDialog)

[Zurück nach oben...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, der vom AccessEnabler ausgelöst wird und die über einen [`getMetadata:`](#getMeta) -Aufruf angeforderten Metadaten liefert.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Rückruf: Ergebnis der Metadaten-Abrufanforderung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter**:

* *metadata*: Die angeforderten Metadaten. Dieser Wert ist ein `NSString` bei statischen Metadaten (Authentifizierung TTL, Autorisierungs-TTL, Geräte-ID).  Es handelt sich um ein komplexes Objekt beim Anfordern benutzerspezifischer Metadaten. Dieses komplexe Objekt ist normalerweise die Objective-C-Darstellung einer JSON-Nutzlast (z. B. &quot;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [&quot;150&quot;, &quot;320&quot;]&#39; wird in Objective-C als NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot;-> NSArray(&quot;15) übersetzt. 0&quot;, &quot;320&quot;)).   Beispiel für Metadaten-JSON-Objekt:

```JSON
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
```

* *verschlüsselt*: Boolescher Wert, der angibt, ob die abgerufenen Metadaten verschlüsselt sind. Dieser Parameter ist nur für Benutzer-Metadaten-Anfragen von Bedeutung. Er hat keine Bedeutung für statische Metadaten (z. B. Authentifizierungs-TTL), die immer unverschlüsselt empfangen werden. Wenn dieser Parameter auf &quot;true&quot;gesetzt ist, liegt es am Programmierer, den unverschlüsselten Benutzer-Metadatenwert abzurufen, indem er eine RSA-Entschlüsselung mithilfe des auf die Whitelist gesetzten privaten Schlüssels durchführt (derselbe private Schlüssel, der für das Signieren der Anforderer-ID in den Aufrufen [`setRequestor:setSignedRequestorId:`](#setReq) und `setRequestor:setSignedRequestorId:serviceProviders: `verwendet wird).

* *key*: Der Schlüssel, der zum Formulieren der Metadaten-Abrufanforderung verwendet wird.

* *arguments*: Dasselbe Wörterbuch, das an den [`getMetadata:`](#getMeta) -Aufruf übergeben wurde. Dies wird bereitgestellt, damit die Anwendung die Anfragen mit den Antworten abgleichen kann.

**ausgelöst von:** [`getMetadata:`](#getMeta)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/user-metadata.md)


[Zurück nach oben...](#apis)

</br>

### MVPD {#mvpd}

**Datei:** AccessEnabler/headers/model/MVPD.h

**Beschreibung** Beschreibt das MVPD-Objekt. Kann verwendet werden, um Informationen über die Eigenschaften des MVPD zu erhalten.

**Verfügbarkeit:** v1.0+ [boardingStatus-Eigenschaft ist ab v2.2] verfügbar

**Eigenschaften**:

* (NSString) ID - Die MVPD-ID.
* (NSString) displayName - Der MVPD-Name. [Dies sollte verwendet werden, um in der Auswahl anzuzeigen]
* (NSString) logoURL - Die Adresse des MVPD-Logos.
* (BOOL) enablePlatformServices - Wenn &quot;true&quot;, unterstützt der MVPD SSO-Dienste wie [Apple SSO](#presentTvDialog).
* (NSString) boardingStatus - Kann drei Werte haben:
   * nil - Der MVPD unterstützt keine Apple SSO.
   * PICKER - Der MVPD kann in der Apple-Auswahl angezeigt werden, der Authentifizierungsfluss erfolgt jedoch über Adobe.
   * UNTERSTÜTZT - Der MVPD wird vollständig von Apple unterstützt und verwendet das SSO-Token von Apple.

[Zurück nach oben...](#apis)

</br>

## Ereignisse verfolgen {#tracking}

Der AccessEnabler -Trigger verfügt über einen zusätzlichen Callback, der nicht unbedingt mit den Berechtigungsflüssen in Zusammenhang steht. Die Implementierung der Rückruffunktion [`sendTrackingData()`](#sendTracking) ist optional, ermöglicht es der Anwendung jedoch, bestimmte Ereignisse zu verfolgen und Statistiken wie die Anzahl erfolgreicher/fehlgeschlagener Authentifizierungs-/Autorisierungsversuche zu kompilieren.

### sendTrackingData:forEventType: {#sendTracking}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch den AccessEnabler ausgelöster Rückruf, der der Anwendung das Auftreten verschiedener Ereignisse signalisiert, z. B. das Fertigstellen/Fehlschlagen von Authentifizierungs-/Autorisierungsflüssen. Mit Adobe Pass Authentication 1.6 werden der Gerätetyp, der AccessEnabler-Client-Typ und das Betriebssystem von [`sendTrackingData()`](#sendTracking) gemeldet. Der Rückruf [`sendTrackingData()`](#sendTracking) bleibt abwärtskompatibel.

**Callback: tracking events**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Verfügbarkeit:** v1.0+

**Hinweis:** Der Gerätetyp und das Betriebssystem werden durch die Verwendung einer öffentlichen Java-Bibliothek (<http://java.net/projects/user-agent-utils>) und der Benutzeragenten-Zeichenfolge abgeleitet. Beachten Sie, dass diese Informationen nur als grobe Methode zur Unterteilung von Betriebsmetriken in Gerätekategorien bereitgestellt werden, dass Adobe jedoch keine Verantwortung für fehlerhafte Ergebnisse übernehmen kann. Verwenden Sie die neue Funktion entsprechend.

* Mögliche Werte für den Gerätetyp:
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* Mögliche Werte für den Client-Typ AccessEnabler :
   * `flash`
   * `html5`
   * `ios`
   * `android`


**Parameter**:

* *event*: der Code des Ereignisses, das verfolgt wird. Es gibt drei mögliche Tracking-Ereignistypen:
   * **authorizationDetection:** jedes Mal, wenn eine Autorisierungstoken-Anfrage zurückgegeben wird (Ereignis ist `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** jedes Mal, wenn eine Authentifizierungsprüfung erfolgt (Ereignis ist `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:**, wenn der Benutzer im MVPD-Auswahlformular einen MVPD auswählt (Ereignis ist `TRACKING_GET_SELECTED_PROVIDER`)
* *data*: zusätzliche Daten, die mit dem gemeldeten Ereignis verknüpft sind. Diese Daten werden in Form einer Werteliste dargestellt.

**ausgelöst durch:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Anweisungen zur Interpretation der Werte im Array *data* :

* Für trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Gibt an, ob die Token-Anfrage erfolgreich war (true/false) und ob sie erfolgreich war:
   * **1** - MVPD-ID-Zeichenfolge
   * **2** - GUID (md5 gehasht)
   * **3** - Token, das sich bereits im Cache befindet (true/false)
   * **4** - Gerätetyp
   * **5** - AccessEnabler-Client-Typ
   * **6** - Betriebssystemtyp

* Für trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Gibt an, ob die Token-Anfrage erfolgreich war (true/false) und ob sie erfolgreich war:
   * **1** - MVPD-ID
   * **2** - GUID (md5 gehasht)
   * **3** - Token, das sich bereits im Cache befindet (true/false)
   * **4** - Fehler
   * **5** - Details
   * **6** - Gerätetyp
   * **7** - AccessEnabler-Client-Typ
   * **8** - Betriebssystemtyp
* Für trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID des derzeit ausgewählten MVPD
   * **1** - Gerätetyp
   * **2** - AccessEnabler-Client-Typ
   * **3** - Betriebssystemtyp

</br>
