---
title: iOS/tvOS-API-Referenz
description: iOS/tvOS-API-Referenz
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 854698397d9d14c1bfddcc10eecc61c7e3c32b71
workflow-type: tm+mt
source-wordcount: '7018'
ht-degree: 0%

---

# iOS/tvOS SDK-API-Referenz {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#intro}

Auf dieser Seite werden die Methoden und Callback-Funktionen beschrieben, die vom nativen iOS/tvOS-Client für die Adobe Pass-Authentifizierung verfügbar gemacht werden. Die hier beschriebenen Methoden und Rückruffunktionen werden im Abschnitt `AccessEnabler.h` und `EntitlementDelegate.h` -Kopfzeilendateien. Sie finden sie im iOS AccessEnabler SDK hier: `[SDK directory]/AccessEnabler/headers/api/`


Zugehörige Dokumentation:

* Eine Beschreibung des grundlegenden Berechtigungsflusses für die Adobe Pass-Authentifizierung finden Sie unter [Berechtigungsfluss](/help/authentication/entitlement-flow.md).
* Eine schrittweise Anleitung zur Implementierung des Berechtigungsflusses für die Adobe Pass-Authentifizierung mit dieser API finden Sie im Abschnitt [iOS-Integrations-Cookie](/help/authentication/iostvos-sdk-cookbook.md).
* Das neueste iOS AccessEnabler SDK finden Sie unter [iOS Native Access Enabler Library](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe empfiehlt Ihnen, nur die Adobe Pass-Authentifizierung zu verwenden *öffentlich* APIs:
>
>* Öffentliche APIs sind für alle unterstützten Client-Typen verfügbar und wurden vollständig getestet. Für jede öffentliche Funktion stellen wir sicher, dass jeder Client-Typ über eine entsprechende Version der zugehörigen Methode(en) verfügt.
>* Öffentliche APIs müssen so stabil wie möglich sein, um die Abwärtskompatibilität zu unterstützen und sicherzustellen, dass Partnerintegrationen nicht beschädigt werden. Für nicht öffentliche APIs behalten wir uns jedoch das Recht vor, ihre Signatur zu jedem späteren Zeitpunkt zu ändern. Wenn Sie auf einen bestimmten Fluss stoßen, der nicht durch eine Kombination der aktuellen öffentlichen Adobe Pass-Authentifizierungs-API-Aufrufe unterstützt werden kann, sollten Sie uns am besten Bescheid geben. Unter Berücksichtigung Ihrer Anforderungen können wir die öffentlichen APIs ändern und eine stabile Lösung für künftige Aufgaben bereitstellen.

</br>

## API-Referenz {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Instanziiert das AccessEnabler -Objekt.

* **[VERALTET]** [init](#init) - Instanziiert das AccessEnabler -Objekt.

* [setOptions:options:](#setOptions) - Konfiguriert globale SDK-Optionen wie profile oder visitorID.

* [setRequest:](#setReqV3)[requestorID](#setReqV3),[setRequest:requestorID:serviceProviders:](#setReqV3) - Legt die Identität des Programmierers fest.

* **[VERALTET]** [setRequest:signedRequestorId:](#setReq),[setRequest:signedRequestorId:serviceProviders:](#setReq) - Legt die Identität des Programmierers fest.

* **[VERALTET]** [setRequest:signedRequestorId:secret:publicKey](#setReq_tvos), [setRequest:signedRequestorId:serviceProviders:secret:publicKey](#setReq_tvos)-Legt die Identität des Programmierers fest.

* [setRequestorComplete:](#setReqComplete) - Informiert Ihre Anwendung darüber, dass die Konfigurationsphase abgeschlossen ist.

* [checkAuthentication](#checkAuthN) - Überprüft den Authentifizierungsstatus des aktuellen Benutzers.

* [getAuthentication](#getAuthN), [getAuthentication:withData:](#getAuthN) - Startet den vollständigen Authentifizierungs-Workflow.

* [getAuthentication:filter](#getAuthN_filter),[getAuthentication:withData:](#getAuthN)[andFilter](#getAuthN_filter) - Startet den vollständigen Authentifizierungs-Workflow.

* [displayProviderDialog:](#dispProvDialog) - Informiert Ihre Anwendung, die entsprechenden UI-Elemente zu instanziieren, damit der Benutzer einen MVPD auswählen kann.

* [setSelectedProvider:](#setSelProv) - Informiert den AccessEnabler über die MVPD-Auswahl des Benutzers.

* [navigateToUrl:](#nav2url) - Informiert Ihre Anwendung darüber, dass dem Benutzer die MVPD-Anmeldeseite angezeigt werden muss.

* [navigateToUrl:useSVC:](#nav2urlSVC) - Informiert Ihre Anwendung darüber, dass dem Benutzer die MVPD-Anmeldeseite angezeigt werden muss, indem SFSafariViewController verwendet wird.

* [handleExternalURL:url](#handleExternalURL) - Schließt den Authentifizierungs-/Abmeldefluss ab.

* **[VERALTET]** [getAuthenticationToken](#getAuthNToken) - Fordert das Authentifizierungstoken vom Backend-Server an.

* [setAuthenticationStatus:errorCode:](#setAuthNStatus) - Informiert Ihre Anwendung über den Status des Authentifizierungsflusses.

* [checkPreauthorizedResources:](#checkPreauth) - Bestimmt, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen.

* [checkPreauthorizedResources:cache:](#checkPreauthCache) - Bestimmt, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen.

* [preauthorizedResources:](#preauthResources) - Bietet eine Liste der Ressourcen, für die der Benutzer bereits autorisiert ist.

* [checkAuthorization:](#checkAuthZ), [checkAuthorization:withData:](#checkAuthZ) - Überprüft den Autorisierungsstatus des aktuellen Benutzers.

* [getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ) - Startet den Autorisierungsfluss.

* [setToken:forResource:](#setToken) - Informiert Ihre Anwendung darüber, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde.

* [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) - Informiert Ihre Anwendung darüber, dass der Autorisierungsfluss fehlgeschlagen ist.

* [Abmelden](#logout) - Startet den Abmeldefluss.

* [getSelectedProvider](#getSelProv) - Bestimmt den derzeit ausgewählten Anbieter.

* [selectedProvider:](#selProv) - Übermittelt Informationen über den aktuell ausgewählten MVPD an Ihre Anwendung.

* [getMetadata:](#getMeta) - Ruft Informationen ab, die von der AccessEnabler-Bibliothek als Metadaten bereitgestellt werden.

* [presentTvProviderDialog:](#presentTvDialog) - Informiert Ihre Anwendung zur Anzeige des Apple SSO-Dialogfelds.

* [dismissTvProviderDialog:](#dismissTvDialog) - Informiert Ihre Anwendung, um das Apple SSO-Dialogfeld auszublenden.

* [setMetadataStatus:encrypted:forKey:andArguments:](#setMetaStatus) - Stellt die Metadaten bereit, die von einer [getMetadata:](#getMeta) aufrufen.

* [sendTrackingData:forEventType:](#sendTracking) - Stellt Tracking-Daten bereit.

* [MVPD](#mvpd) - Die MVPD-Klasse. [Enthält Informationen zum MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Instanziiert das AccessEnabler -Objekt. Pro Anwendungsinstanz sollte eine einzige AccessEnabler -Instanz vorhanden sein.

| **API-Aufruf: iOS AccessEnabler-Konstruktor** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Verfügbarkeit:** v3.0+

**Parameter:**

* **softwareStatement:** Eine Zeichenfolge, die die Anwendung im Adobe-System angibt. Sehen Sie sich an, wie Sie eine Software-Anweisung erhalten.

[Zurück nach oben...](#apis)



### init - [VERALTET]{#init}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Instanziiert das AccessEnabler -Objekt. Pro Anwendungsinstanz sollte eine einzige AccessEnabler -Instanz vorhanden sein.

| API-Aufruf: iOS AccessEnabler-Konstruktor |
| --- |
| ```- (id) init;``` |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

**Parameter:** Keines

[Zurück nach oben...](#apis)

</br>

### setOptions:options {#setOptions}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Konfiguriert globale SDK-Optionen. Es akzeptiert ein NSDictionary als Argument. Die Werte aus dem Wörterbuch werden zusammen mit jedem Netzwerkaufruf des SDK an den Server übergeben.

**Hinweis:** Die Werte werden unabhängig vom aktuellen Ablauf (Authentifizierung/Autorisierung) an den Server übergeben. Wenn Sie die Werte ändern möchten, können Sie diese Methode jederzeit aufrufen.

| API-Aufruf: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Verfügbarkeit:** v2.3.0+

**Parameter:**

* *options*: Ein NSDictionary, das globale SDK-Optionen enthält. Derzeit sind die folgenden Optionen verfügbar:
   * **applicationProfile** - Sie kann verwendet werden, um Serverkonfigurationen auf Grundlage dieses Werts vorzunehmen.
   * **visitorID** - Der Experience Cloud ID-Dienst. Dieser Wert kann später für erweiterte Analyseberichte verwendet werden.
   * **handleSVC** - Boolescher Wert, der angibt, ob der Programmierer die SFSafariViewControllers verarbeiten wird. Siehe [SFSafariViewController-Unterstützung für iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) für weitere Details.
      * Wenn auf **false,** das SDK stellt dem Endbenutzer automatisch einen SFSafariViewController zur Verfügung. Das SDK navigiert weiter zur URL der MVPDs-Anmeldeseite.
      * Wenn auf **true,** das SDK **NOT** stellt dem Endbenutzer automatisch einen SFSafariViewController zur Verfügung. Das SDK wird den Trigger weiter vorantreiben **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Client-Informationen, wie beschrieben in [Weitergeben von Client-Informationen](/help/authentication/passing-client-information-device-connection-and-application.md).

[Zurück nach oben...](#apis)


### setRequestor:requestorID, setRequestor:requestorID:serviceProviders: {#setReqV3}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Beim Umgang mit SSO und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gestellt wird, um mit dem Systemstatus zu synchronisieren (Abrufen eines Remote-Tokens, wenn SSO aktiviert ist, oder Löschen des lokalen Tokens, wenn in der Zwischenzeit ein Abmelden stattgefunden hat).

Die Server-Antwort enthält eine Liste von MVPDs zusammen mit einigen Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Serverantwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. SUCCESS/FAIL) wird Ihrer Anwendung über das `setRequestorComplete:` Callback.

Wenn die Variable `urls` nicht verwendet wird, zielt der resultierende Netzwerkaufruf auf die Standard-Service-Provider-URL ab: die Adobe RELEASE-/Produktionsumgebung.


Wenn ein Wert für `urls` -Parameter, werden alle in der `urls` -Parameter. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Der erste Antwortsender hat beim Kompilieren der Liste der MVPDs Vorrang. Für jeden MVPD in der Liste speichert der AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die dem Dienstanbieter zugeordnet ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Verfügbarkeit:** v3.0+

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Verfügbarkeit:** v3.0+

**Parameter:**

* *requestorID*: Die eindeutige ID, die mit dem Programmierer verknüpft ist. Übergeben Sie die von Adobe zugewiesene eindeutige ID an Ihre Site, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registrieren.
* *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.

>[!NOTE]
>
>Wenn ohne `serviceProviders` -Parameter, ruft die Bibliothek die Konfiguration vom standardmäßigen Service Provider ab (d. h. `https://sp.auth.adobe.com` für das Produktionsprofil oder `https://sp.auth-staging.adobe.com` für das Staging-Profil). Wenn die Variable `serviceProviders` angegeben ist, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn in verschiedenen Antworten des Dienstleisters doppelte Informationen vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers behoben (d. h. der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Rückrufe:** [`setRequestorComplete:`](#setReqComplete)

[Zurück nach oben...](#apis)

</br>

### setRequest:setSignedRequestorId:, setRequestor:setSignedRequestorId:serviceProviders: - [VERALTET] {#setReq}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gestellt wird, um mit dem Systemstatus zu synchronisieren (Abrufen eines Remote-Tokens, wenn SSO aktiviert ist, oder Löschen des lokalen Tokens, wenn in der Zwischenzeit ein Abmelden stattgefunden hat).

Die Server-Antwort enthält eine Liste von MVPDs zusammen mit einigen Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Serverantwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. SUCCESS/FAIL) wird Ihrer Anwendung über das `setRequestorComplete:` Callback.

Wenn die Variable `urls` nicht verwendet wird, zielt der resultierende Netzwerkaufruf auf die Standard-Service-Provider-URL ab: die Adobe RELEASE-/Produktionsumgebung.

Wenn ein Wert für `urls` -Parameter, werden alle in der `urls` -Parameter. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Der erste Antwortsender hat beim Kompilieren der Liste der MVPDs Vorrang. Für jeden MVPD in der Liste speichert der AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die dem Dienstanbieter zugeordnet ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

| API-Aufruf: Konfiguration des Anforderers |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

**Parameter:**

* *requestorID*: Die eindeutige ID, die mit dem Programmierer verknüpft ist. Übergeben Sie die eindeutige ID, die von Adobe zugewiesen wurde, an Ihre Site, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
* *signedRequestorID*: **Dieser Parameter ist in den iOS AccessEnabler-Versionen 1.2 und höher vorhanden.** Eine Kopie der Anforderer-ID, die digital mit Ihrem privaten Schlüssel signiert ist. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.

**Hinweise:** Wenn ohne `serviceProviders` -Parameter, ruft die Bibliothek die Konfiguration vom standardmäßigen Service Provider ab (d. h.`https://sp.auth.adobe.com` für das Produktionsprofil oder `https://sp.auth-staging.adobe.com` für das Staging-Profil). Wenn die Variable `serviceProviders` angegeben ist, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn in verschiedenen Antworten des Dienstleisters doppelte Informationen vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers behoben (d. h. der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Rückrufe:** [`setRequestorComplete:`](#setReqComplete)


[Zurück nach oben...](#apis)

### setRequest:setSignedRequestorId:secret:publicKey, setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey - [VERALTET] {#setReq_tvos}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Diese Einstellung sollte nur einmal während des Lebenszyklus der Anwendung vorgenommen werden.

Die Server-Antwort enthält eine Liste von MVPDs zusammen mit einigen Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Serverantwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. SUCCESS/FAIL) wird Ihrer Anwendung über das `setRequestorComplete:` Callback.

Wenn die Variable `urls` nicht verwendet wird, zielt der resultierende Netzwerkaufruf auf die Standard-Service-Provider-URL ab: die Adobe RELEASE-/Produktionsumgebung.

Wenn ein Wert für `urls` -Parameter, werden alle in der `urls` -Parameter. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Der erste Antwortsender hat beim Kompilieren der Liste der MVPDs Vorrang. Für jeden MVPD in der Liste speichert der AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die dem Dienstanbieter zugeordnet ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.



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

* *requestorID*: Die eindeutige ID, die mit dem Programmierer verknüpft ist. Übergeben Sie die eindeutige ID, die von Adobe zugewiesen wurde, an Ihre Site, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
* *signedRequestorID*: **Dieser Parameter ist in den iOS AccessEnabler-Versionen 1.2 und höher vorhanden.** Eine Kopie der Anforderer-ID, die digital mit Ihrem privaten Schlüssel signiert ist. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.
* secret und publicKey: Der geheime und öffentliche Schlüssel, der zum Signieren der zweiten Bildschirmaufrufe verwendet wird. Weitere Informationen finden Sie unter [Clientlose Dokumentation](#create_dev).

Wenn ohne `serviceProviders` -Parameter, ruft die Bibliothek die Konfiguration vom Standarddienstleister ab (d. h. `https://sp.auth.adobe.com` für das Produktionsprofil oder https://sp.auth-staging.adobe.com für das Staging-Profil). Wenn die Variable `serviceProviders` angegeben ist, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn in verschiedenen Antworten des Dienstleisters doppelte Informationen vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers behoben (d. h. der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Rückrufe:** [`setRequestorComplete:`](#setReqComplete)

[Zurück nach oben...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch den AccessEnabler ausgelöster Rückruf, der Ihre Anwendung über den Abschluss der Konfigurationsphase informiert. Dies ist ein Signal, dass die App mit der Ausgabe von Berechtigungsanfragen beginnen kann. Bis zum Abschluss der Konfigurationsphase kann die Anwendung keine Berechtigungsanfragen mehr stellen.

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
   * `ACCESS_ENABLER_STATUS_SUCCESS` - Die Konfigurationsphase wurde erfolgreich abgeschlossen.
   * `ACCESS_ENABLER_STATUS_ERROR` - Konfigurationsphase fehlgeschlagen

**Ausgelöst von:**
`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Zurück nach oben...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Überprüft den Authentifizierungsstatus des aktuellen Benutzers.
Dazu wird nach einem gültigen Authentifizierungstoken im lokalen Token-Speicher gesucht. Diese Methode führt keine Netzwerkaufrufe durch. Wir empfehlen, sie im Haupt-Thread aufzurufen.
Sie wird von der Anwendung verwendet, um den Authentifizierungsstatus des Benutzers abzufragen und die Benutzeroberfläche entsprechend zu aktualisieren (d. h. die Anmelde-/Abmelde-Benutzeroberfläche zu aktualisieren). Der Authentifizierungsstatus wird der Anwendung über das [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) Callback.


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

**Parameter:** Keines

**Ausgelöste Rückrufe:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Zurück nach oben...](#apis)

</br>

### getAuthentication, getAuthentication:withData: {#getAuthN}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Zunächst wird der Authentifizierungsstatus überprüft. Falls noch nicht authentifiziert, wird der Zustandsmaschine für den Authentifizierungsfluss gestartet:

* Wenn der letzte Authentifizierungsversuch erfolgreich war, wird die MVPD-Auswahlphase übersprungen und die [`navigateToUrl:`](#nav2url) Callback wird ausgelöst. Die Anwendung verwendet diesen Rückruf, um das WebView-Steuerelement zu instanziieren, das dem Benutzer die Anmeldeseite des MVPD anzeigt. **[HINWEIS: Ab Access Enabler 1.5 ist diese Funktion aufgrund einer Beschränkung im SDK nicht mehr verfügbar].**
* wenn der letzte Authentifizierungsversuch fehlgeschlagen ist oder der Benutzer sich explizit abgemeldet hat, wird die [`displayProviderDialog:`](#dispProvDialog) Callback wird ausgelöst. Ihre Anwendung verwendet diesen Rückruf, um die MVPD-Auswahlbenutzeroberfläche anzuzeigen. Außerdem muss Ihre App den Authentifizierungsfluss fortsetzen, indem sie die AccessEnabler-Bibliothek über die MVPD-Auswahl des Benutzers über die [`setSelectedProvider:`](#setSelProv) -Methode.

Da die Anmeldeinformationen des Benutzers auf der MVPD-Anmeldeseite überprüft werden, muss Ihre Anwendung die verschiedenen Weiterleitungsvorgänge überwachen, die während der Authentifizierung des Benutzers auf der Anmeldeseite des MVPD stattfinden. Wenn die richtigen Anmeldeinformationen eingegeben werden, wird das WebView-Steuerelement zu einer benutzerdefinierten URL umgeleitet, die von der `ADOBEPASS_REDIRECT_URL` Konstante. Diese URL soll nicht von WebView geladen werden. Die Anwendung muss diese URL abfangen und dieses Ereignis als Signal interpretieren, dass die Anmeldungsphase abgeschlossen ist. Anschließend sollte die Steuerung an den AccessEnabler übergeben werden, um den Authentifizierungsfluss abzuschließen (durch Aufruf der [handleExternalURL](#handleExternalURL) -Methode).

Schließlich wird der Authentifizierungsstatus über die [setAuthenticationStatus:errorCode:](#setAuthNStatus) Callback.

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
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Dienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** ` setAuthenticationStatus:errorCode:, `[`displayProviderDialog:`](#dispProvDialog)`,`` sendTrackingData:forEventType:`


[Zurück nach oben...](#apis)

</br>

### getAuthentication:filter, getAuthentication:withData:andFilter {#getAuthN_filter}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Zunächst wird der Authentifizierungsstatus überprüft. Falls noch nicht authentifiziert, wird der Zustandsmaschine für den Authentifizierungsfluss gestartet:

* [presentTvProviderDialog()](#presentTvDialog) wird aufgerufen, wenn der aktuelle Anfragende über mindestens einen MVPD verfügt, der SSO unterstützt. Wenn kein MVPD SSO unterstützt, beginnt der klassische Authentifizierungsfluss und der Filterparameter wird ignoriert.
* Nachdem der Benutzer den Apple SSO-Fluss abgeschlossen hat [dismissTvProviderDialog()](#dismissTvDialog) ausgelöst und der Authentifizierungsprozess abgeschlossen wird.

Schließlich wird der Authentifizierungsstatus über die [setAuthenticationStatus:errorCode:](#setAuthNStatus) Callback.

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
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Dienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.
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
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Dienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.
* filter: Eine Liste der MVPD-IDs, die im Apple SSO-Dialogfeld angezeigt werden sollen. Alle MVPD, die SSO nicht unterstützen, werden ignoriert, aber die Reihenfolge wird eingehalten.

**Ausgelöste Rückrufe:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Zurück nach oben...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch AccessEnabler ausgelöster Rückruf, um die Anwendung darüber zu informieren, dass die entsprechenden UI-Elemente instanziiert werden müssen, damit der Benutzer den gewünschten MVPD auswählen kann. Der Rückruf bietet eine Liste von MVPD-Objekten mit zusätzlichen Informationen, die dazu beitragen können, das Auswahlbenutzeroberflächenbedienfeld korrekt zu erstellen (z. B. die URL, die auf das MVPD-Logo verweist, der Anzeigename usw.).

Nachdem der Benutzer den gewünschten MVPD ausgewählt hat, muss die Anwendung auf der obersten Ebene den Authentifizierungsfluss durch Aufruf von `setSelectedProvider:` und übergeben die Kennung des MVPD entsprechend der Auswahl des Benutzers.

**Abbruch des Authentifizierungsablaufs** - Dies ist ein Punkt, an dem der Benutzer die Möglichkeit hat, die &quot;Zurück&quot;-Taste zu drücken, was dem Abbrechen des Authentifizierungsflusses entspricht. In diesem Szenario muss Ihre Anwendung die [setSelectedProvider:](#setSelProv) -Methode, wobei null als Parameter übergeben wird, um dem AccessEnabler die Möglichkeit zu geben, seinen Authentifizierungsstatus-Computer zurückzusetzen.

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

* *mvpds*: Liste der MVPD-Objekte, die MVPD-bezogene Informationen enthalten, die die Anwendung zum Erstellen der MVPD-Auswahl-UI-Elemente verwenden kann.

**Ausgelöst von:** ` getAuthentication, `[getAuthentication:withData:](#getAuthN),` getAuthorization:, `[getAuthorization:withData:](#getAuthZ)


[Zurück nach oben...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Access Enabler über die MVPD-Auswahl des Benutzers zu informieren. Die Anwendung kann diese Methode verwenden, um den für die Authentifizierung verwendeten Dienstleister auszuwählen oder zu ändern.

Wenn der ausgewählte MVPD ein TempPass MVPD ist, authentifiziert er sich automatisch mit diesem MVPD, ohne später getAuthentication() aufrufen zu müssen.

Bitte beachten Sie, dass dies bei Promotional Temp Pass nicht möglich ist, wenn zusätzliche Parameter in der getAuthentication() -Methode angegeben werden.

Beim Übergeben *null* als Parameter verwendet wird, geht der Access Enabler davon aus, dass der Benutzer den Authentifizierungsfluss abgebrochen hat (d. h. durch Drücken der &quot;Zurück&quot;-Schaltfläche), und antwortet, indem er den Authentifizierungsstatus-Computer zurücksetzt und die [setAuthenticationStatus:errorCode:](#setAuthNStatus) Callback mit `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` Fehlercode.

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

**Parameter:** Keines

**Ausgelöste Rückrufe:** ` setAuthenticationStatus:errorCode:,sendTrackingData:forEventType:,  `[`navigateToUrl:`](#nav2url)

[Zurück nach oben...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback, der vom AccessEnabler ausgelöst wurde, um Ihre Anwendung aufzufordern, einen UIWebView/WKWebView-Controller zu instanziieren und die im Callback bereitgestellte URL zu laden **`url`** -Parameter. Der Rückruf übergibt die **`url`** -Parameter, der die URL des Authentifizierungsendpunkts oder die URL des Abmelde-Endpunkts darstellt.

Als UIWebView/WKWebView` `Controller durchläuft mehrere Umleitungen. Ihre Anwendung muss die Aktivität des Controllers überwachen und den Zeitpunkt erkennen, zu dem sie eine bestimmte benutzerdefinierte URL lädt, die von der `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`). Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Authentifizierungs- oder Abmeldevorgang abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung die UIWebView/WKWebView schließen und den `handleExternalURL:url `API-Methode.

**Hinweis:** Bitte beachten Sie, dass im Fall eines Authentifizierungsflusses dies ein Punkt ist, an dem der Benutzer die Schaltfläche &quot;Zurück&quot; drücken kann, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss Ihre Anwendung die [setSelectedProvider:](#setSelProv) Methodenübergabe **`nil`** als Parameter angeben und dem AccessEnabler die Möglichkeit geben, seinen Authentifizierungsstatus-Computer zurückzusetzen.

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

* *url*: die URL, die auf die Anmeldeseite des MVPD verweist.

**Ausgelöst von:** [setSelectedProvider:](#setSelProv)



[Zurück nach oben...](#apis)

</br>

#### navigateToUrl:useSVC: {#nav2urlSVC}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback, der vom AccessEnabler anstelle der `navigateToUrl:` Callback, falls Ihre Anwendung zuvor die manuelle Verarbeitung des Safari View Controller (SVC) über die [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) und nur bei MVPDs, die einen Safari View Controller (SVC) erfordern. Für alle anderen MVPDs wird die `navigateToUrl:` Callback wird aufgerufen. Siehe[SFSafariViewController-Unterstützung für iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) für Informationen zur Verwaltung des Safari View Controller (SVC).

Ähnlich wie bei `navigateToUrl:` Callback `navigateToUrl:useSVC:` wird vom AccessEnabler ausgelöst, um Ihre Anwendung zur Instanziierung einer `SFSafariViewController` und zum Laden der im Callback bereitgestellten URL **`url`** -Parameter. Der Rückruf übergibt die **`url`** -Parameter, der die URL des Authentifizierungsendpunkts oder die URL des Abmelde-Endpunkts darstellt, und **`useSVC`** -Parameter, der angibt, dass die Anwendung einen `SFSafariViewController`.

Als `SFSafariViewController` Controller durchläuft mehrere Umleitungen. Ihre Anwendung muss die Aktivität des Controllers überwachen und den Zeitpunkt erkennen, zu dem sie eine bestimmte benutzerdefinierte URL lädt, die von Ihrer `application's custom scheme` (z. B.** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Authentifizierungs- oder Abmeldevorgang abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung die `SFSafariViewController` und rufen Sie AccessEnabler `handleExternalURL:url `API-Methode.

**Hinweis:** Bitte beachten Sie, dass im Fall eines Authentifizierungsflusses dies ein Punkt ist, an dem der Benutzer die Schaltfläche &quot;Zurück&quot; drücken kann, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss Ihre Anwendung die [setSelectedProvider:](#setSelProv) Methodenübergabe **`nil`** als Parameter angeben und dem AccessEnabler die Möglichkeit geben, seinen Authentifizierungsstatus-Computer zurückzusetzen.

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
* *useSVC:* ob die URL in SFSafariViewController geladen werden soll.

**Ausgelöst von:**[ setOptions:](#setOptions) before [setSelectedProvider:](#setSelProv)

[Zurück nach oben...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Authentifizierungs- oder Abmeldevorgang abzuschließen. Diese Methode sollte direkt aufgerufen werden, nachdem Ihre Anwendung erkennt, wann die `UIWebView/WKWebView or SFSafariViewController` -Controller zu einer bestimmten benutzerdefinierten URL umgeleitet. Falls Ihre Anwendung eine `SFSafariViewController `Controller, für den die spezifische benutzerdefinierte URL von Ihrem `application's custom scheme` (z. B.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die Variable `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`).

Im Fall eines Authentifizierungsflusses schließt der AccessEnabler den Ablauf ab, indem er das Authentifizierungstoken vom Backend-Server abruft und lokal im Token-Speicher speichert. Der AccessEnabler informiert Ihre Anwendung über den Abschluss des Authentifizierungsvorgangs, indem er die `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> Callback mit Status-Code 1, der auf Erfolg hinweist. Wenn bei der Ausführung dieser Schritte ein Fehler auftritt, wird die `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> -Callback wird mit dem Status-Code 0 ausgelöst, der auf einen Authentifizierungsfehler sowie einen entsprechenden Fehlercode hinweist.

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

* *url*: Die abgefangene URL aus der ` UIWebView/WKWebView or SFSafariViewController ` als Zeichenfolge.


**Ausgelöste Rückrufe:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Zurück nach oben...](#apis)

</br>

#### getAuthenticationToken - [VERALTET] {#getAuthNToken}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Schließt den Authentifizierungsfluss durch Anfordern des Authentifizierungstokens vom Backend-Server ab. Diese Methode sollte von Ihrer Anwendung nur als Reaktion auf ein Ereignis aufgerufen werden, bei dem das WebView-Steuerelement, das die MVPD-Anmeldeseite hostet, an die benutzerdefinierte URL weitergeleitet wird, die von der `ADOBEPASS_REDIRECT_URL` Konstante.


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

**Parameter:** Keines

**Ausgelöste Rückrufe:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Zurück nach oben...](#apis)

&lt;/br

#### setAuthenticationStatus:errorCode: {#setAuthNStatus}

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
   * `ACCESS_ENABLER_STATUS_SUCCESS` - Der Authentifizierungsfluss wurde erfolgreich abgeschlossen.
   * `ACCESS_ENABLER_STATUS_ERROR` - Authentifizierungsfluss fehlgeschlagen
* *code*: Grund für Fehler. Wenn *status* is `ACCESS_ENABLER_STATUS_SUCCESS`, dann *code* ist eine leere Zeichenfolge (d. h., die durch die Variable `USER_AUTHENTICATED` -Konstante). Im Falle eines Fehlers kann dieser Parameter einen der folgenden Werte annehmen:
   * `USER_NOT_AUTHENTICATED_ERROR` - Der Benutzer ist nicht authentifiziert. Als Antwort auf die [checkAuthentication:](#checkAuthN) Methodenaufruf, wenn kein gültiges Authentifizierungstoken im lokalen Token-Cache vorhanden ist.
   * `PROVIDER_NOT_SELECTED_ERROR` - Der AccessEnabler hat den Authentifizierungsstatus-Computer zurückgesetzt, nachdem die Anwendung der oberen Ebene übergeben wurde. *null* nach [`setSelectedProvider:`](#setSelProv) , um den Authentifizierungsfluss abzubrechen.  Vermutlich hat der Benutzer den Authentifizierungsfluss abgebrochen (d. h. die &quot;Zurück&quot;-Schaltfläche gedrückt).
   * `GENERIC_AUTHENTICATION_ERROR` - Der Authentifizierungsfluss schlug aus Gründen wie z. B. Nichtverfügbarkeit des Netzwerks fehl oder der Benutzer hat den Authentifizierungsfluss explizit abgebrochen.

**Ausgelöst von:** ` checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN),` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)

[Zurück nach oben...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der Hauptzweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden können **(z. B. Angabe des Zugriffstatus mit den Symbolen &quot;Sperren&quot;und &quot;Entsperren&quot;).**

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

* *Ressourcen:* Array von Ressourcen, für die die Autorisierung überprüft werden sollte. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt den gleichen Einschränkungen wie die Ressourcen-ID im Aufruf, d. h. es sollte sich um einen vereinbarten Wert handeln, der zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment festgelegt wurde.

**Callback ausgelöst:** [`preauthorizedResources:`](#preauthResources)

[Zurück nach oben...](#apis)

</br>

### checkPreauthorizedResources:cache: {#checkPreauthCache}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der Hauptzweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden (z. B. zur Angabe des Zugriffs mit Schloss- und Entsperrungssymbolen). Die **cache** -Parameter steuert, ob der interne Cache zum Auflösen von Ressourcen verwendet wird.

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

* *Ressourcen:* Array von Ressourcen, für die die Autorisierung überprüft werden sollte. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt den gleichen Einschränkungen wie die Ressourcen-ID in der `getAuthorization:` -Aufruf, d. h., es sollte sich um einen zwischen dem Programmierer und dem MVPD oder einem Medien-RSS-Fragment vereinbarten Wert handeln.
* *cache:* Boolescher Wert, der angibt, ob der interne Cache zum Auflösen von Ressourcen verwendet werden soll. Bei &quot;false&quot;wird der Cache umgangen, was zu Server-Aufrufen bei jedem Aufruf dieser API führt.

**Callback ausgelöst:** [`preauthorizedResources:`](#preauthResources)

[Zurück nach oben...](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback ausgelöst durch `checkPreauthorizedResources:`. Bietet eine Liste der Ressourcen, für die der Benutzer bereits autorisiert ist.

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

**Ausgelöst von:** [`checkPreauthorizedResources:`](#checkPreauth)



[Zurück nach oben...](#apis)

</br>

### checkAuthorization:, checkAuthorization:withData: {#checkAuthZ}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn nicht authentifiziert, wird die [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) -Rückruf ausgelöst und die -Methode beendet. Wenn der Benutzer authentifiziert ist, wird auch der Autorisierungsfluss Trigger. Siehe Details zu den [`getAuthorization:`](#getAuthZ) -Methode.


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

* *resource*: die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Dienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:**
[tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed)`,setToken:forResource:, sendTrackingData:forEventType:, setAuthenticationStatus:errorCode:`

[Zurück nach oben...](#apis)

</br>

### getAuthorization:, getAuthorization:withData: {#getAuthZ}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsfluss zu initiieren. Wenn der Benutzer noch nicht authentifiziert ist, wird auch der Authentifizierungsfluss initiiert. Wenn der Benutzer authentifiziert wird, stellt der AccessEnabler weiterhin Anforderungen an das Autorisierungstoken (wenn im lokalen Token-Cache kein gültiges Autorisierungstoken vorhanden ist) und an das Token für kurzlebige Medien. Sobald das Short-Media-Token abgerufen wurde, wird der Autorisierungsfluss als vollständig betrachtet. Die [setToken:forResource:](#setToken) Callback wird ausgelöst und das Short-Media-Token wird als Parameter an die Anwendung gesendet. Wenn die Genehmigung aus irgendeinem Grund fehlschlägt, wird die [tokenRequestFailed:forEventType:](#tokenReqFailed) -Rückruf ausgelöst und der Fehlercode/die Details bereitgestellt werden.

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

* *resource*: die Kennung der Ressource, für die der Benutzer eine Autorisierung anfordert.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Pass-Dienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne das SDK zu ändern.

**Ausgelöste Rückrufe:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Zusätzliche ausgelöste Rückrufe:**\
Diese Methode kann auch die folgenden Rückrufe Trigger werden (wenn der Authentifizierungsfluss ebenfalls initiiert wird): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

**HINWEIS: Bitte verwenden Sie checkAuthorization: / checkAuthorization:withData: anstatt getAuthorization: / getAuthorization:withData: wann immer möglich. Die getAuthorization: / getAuthorization:withData: -Methode startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf der Seite des Programmierers führen könnte.**

[Zurück nach oben...](#apis)

</br>

### setToken:forResource: {#setToken}

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

* *token*: das kurzlebige Medien-Token
* *resource*: die Ressource, für die die Genehmigung erteilt wurde

**Ausgelöst von:** [checkAuthorization:](#checkAuthZ)` , `[checkAuthorization:withData:](#checkAuthZ),` `[getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ)

[Zurück nach oben...](#apis)

</br>

### tokenRequestFailed:errorCode:errorDescription: {#tokenReqFailed}

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
* *code*: Der dem Fehlerszenario zugeordnete Fehlercode. Mögliche Werte:
   * `USER_NOT_AUTHORIZED_ERROR` - der Benutzer konnte für die jeweilige Ressource nicht autorisieren
* *description*: Zusätzliche Details zum Fehlerszenario. Wenn diese beschreibende Zeichenfolge aus keinem Grund verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge **(&quot;&quot;)**.\
  Diese Zeichenfolge kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder umsatzbezogene Nachrichten zu übergeben. Wenn einem Abonnenten beispielsweise die Autorisierung für eine Ressource verweigert wird, könnte der MVPD eine Nachricht wie die folgende senden: &quot;Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie auf **here**.&quot; Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Rückruf an den Programmierer übergeben, der die Möglichkeit hat, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann diesen Parameter auch verwenden, um eine Benachrichtigung über die Bedingung bereitzustellen, die möglicherweise zu einem Fehler geführt hat. Beispiel: &quot;Bei der Kommunikation mit dem Autorisierungsdienst des Providers ist ein Netzwerkfehler aufgetreten&quot;.

**Ausgelöst von:** ` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ)

[Zurück nach oben...](#apis)

</br>

### Abmelden {#logout}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Weiterleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Da dieser Fluss nicht mit einer einfachen HTTP-Anforderung abgeschlossen werden kann, die von der AccessEnabler-Bibliothek, einer `UIWebView/WKWebView or SFSafariViewController` -Controller muss instanziiert werden, damit HTTP-Weiterleitungsvorgänge folgen können.

Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss insofern, als der Benutzer nicht mit der `UIWebView/WKWebView or SFSafariViewController`  Controller in irgendeiner Weise. Daher empfiehlt Adobe, das Steuerelement während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.

Es wird ein dem Authentifizierungsfluss ähnliches Muster verwendet. Die iOS AccessEnabler-Trigger `navigateToUrl:` oder `navigateToUrl:useSVC:` , um eine `UIWebView/WKWebView or SFSafariViewController` und zum Laden der im Callback bereitgestellten URL `url` -Parameter. Dies ist die URL des Abmelde-Endpunkts auf dem Backend-Server. Für den tvOS AccessEnabler weder die `navigateToUrl:` oder `navigateToUrl:useSVC:` Callback wird aufgerufen.

Da mehrere Umleitungen durchgeführt werden, muss Ihre Anwendung die Aktivität der `UIWebView/WKWebView or SFSafariViewController `und erkennen Sie den Moment, in dem eine bestimmte benutzerdefinierte URL geladen wird. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Abmeldefluss abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung den Controller schließen und den AccessEnabler `handleExternalURL:url `API-Methode. Falls Ihre Anwendung eine `SFSafariViewController `Controller, für den die spezifische benutzerdefinierte URL von Ihrem `application's custom scheme` (z. B.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die Variable `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`).

Am Ende ruft AccessEnabler die [`setAuthenticationStatus()`](#setAuthNStatus) Callback mit Status-Code 0, der auf den Erfolg des Abmeldeflusses hinweist.

**Hinweis:** Wenn der Benutzer mit Apple SSO angemeldet ist, wird der VSA203-Status ausgelöst. Ist dies der Fall, sollte der Benutzer angewiesen werden, sich auch aus den Systemeinstellungen abzumelden. Andernfalls wird eine erneute Authentifizierung durchgeführt, wenn die Anwendung neu gestartet wird.


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

**Parameter:** Keines

**Ausgelöste Rückrufe:** `navigateToUrl:, `[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Zurück nach oben...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Verwenden Sie diese Methode, um den aktuell ausgewählten Anbieter zu bestimmen.

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

**Parameter:** Keines

**Ausgelöste Rückrufe:** [`selectedProvider:`](#selProv)

[Zurück nach oben...](#apis)

</br>

### selectedProvider {#selProv}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, der vom AccessEnabler ausgelöst wird und Informationen über das aktuell ausgewählte MVPD für die Anwendung bereitstellt.

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

* *mvpd*: Objekt, das Informationen zum aktuell ausgewählten MVPD enthält

**Ausgelöst von:** [`getSelectedProvider`](#getSelProv)

[Zurück nach oben...](#apis)

</br>

### getMetadata: {#getMeta}

**Datei:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Verwenden Sie diese Methode, um Informationen abzurufen, die von der AccessEnabler-Bibliothek als Metadaten bereitgestellt werden. Die Anwendung kann auf diese Daten zugreifen, indem sie ein wörterbuchbasiertes *key* Eingabeparameter.

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

* *keyDictionary*: eine Wörterbuchdatenstruktur im folgenden Format:
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_AUTHENTICATION`, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_AUTHORIZATION` **und**\
     Schlüssel ist `METADATA_RESOURCE_ID_KEY` und -Wert eine bestimmte Ressourcen-ID ist, wird die Abfrage ausgeführt, um die Ablaufzeit des Autorisierungstokens zu erhalten, das mit der angegebenen Ressource verknüpft ist.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_DEVICE_ID`, wird die Abfrage durchgeführt, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen über Aktivierung und Gebühren zu erhalten.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_USER_META` **und** Schlüssel ist `METADATA_USER_META_KEY` und value der Name der Metadaten ist, wird die Abfrage für Benutzermetadaten durchgeführt. Die Liste der verfügbaren Benutzer-Metadatentypen:
      * `zip` - Liste der Postleitzahlen
      * `householdID` - Haushaltskennung. Wenn ein MVPD keine Unterkonten unterstützt, ist dies identisch mit `userID`.
      * `maxRating` - Eine Sammlung von elterlichen Höchstwerten für den Benutzer
      * `userID` - Die Benutzer-ID. Wenn ein MVPD Subkonten unterstützt und der Benutzer nicht das Hauptkonto ist, `userID` unterscheidet sich `householdID.`
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

**Beschreibung** Callback, der vom AccessEnabler nach dem Aufruf ausgelöst wurde[getAuthentication()](#getAuthN) wenn der aktuelle Anforderer mindestens einen MVPD mit SSO-Unterstützung unterstützt.

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

**Ausgelöst von:** [`getAuthentication`](#getAuthN)

**Weitere Informationen:** [iOS/tvOS-Single-Sign-On](#presentTvDialog)

[Zurück nach oben...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Rückruf, der vom AccessEnabler ausgelöst wird, nachdem der Benutzer das Apple SSO-Dialogfeld geschlossen hat.

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

**Weitere Informationen:** [iOS/tvOS-Single-Sign-On](#presentTvDialog)

[Zurück nach oben...](#apis)

</br>

### setMetadataStatus:encrypted:forKey:andArguments: {#setMetaStatus}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, der vom AccessEnabler ausgelöst wurde und die über einen [`getMetadata:`](#getMeta) aufrufen.

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

* *Metadaten*: Die angeforderten Metadaten. Dieser Wert ist ein `NSString` im Falle statischer Metadaten (Authentifizierung TTL, Autorisierungs-TTL, Geräte-ID).  Es handelt sich um ein komplexes Objekt beim Anfordern benutzerspezifischer Metadaten. Dieses komplexe Objekt ist normalerweise die Objective-C-Darstellung einer JSON-Nutzlast (z. B. &quot;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [&quot;150&quot;, &quot;320&quot;]}&#39; wird in Objective-C als NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;) übersetzt.   Beispiel für Metadaten-JSON-Objekt:

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

* *verschlüsselt*: Boolescher Wert, der angibt, ob die abgerufenen Metadaten verschlüsselt sind. Dieser Parameter ist nur für Benutzer-Metadaten-Anfragen von Bedeutung. Er hat keine Bedeutung für statische Metadaten (z. B. Authentifizierungs-TTL), die immer unverschlüsselt empfangen werden. Wenn dieser Parameter auf &quot;true&quot;gesetzt ist, liegt es am Programmierer, den unverschlüsselten Benutzer-Metadatenwert abzurufen, indem er eine RSA-Entschlüsselung mithilfe des auf die Whitelist gesetzten privaten Schlüssels durchführt (derselbe private Schlüssel, der zum Signieren der Anforderer-ID in der Variablen [`setRequestor:setSignedRequestorId:`](#setReq) und `setRequestor:setSignedRequestorId:serviceProviders: `-Aufrufe).

* *key*: Der Schlüssel, der zum Formulieren der Metadaten-Abrufanforderung verwendet wird.

* *Arguments*: Das gleiche Wörterbuch, das an die [`getMetadata:`](#getMeta) aufrufen. Dies wird bereitgestellt, damit die Anwendung die Anfragen mit den Antworten abgleichen kann.

**Ausgelöst von:** [`getMetadata:`](#getMeta)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/user-metadata.md)


[Zurück nach oben...](#apis)

</br>

### MVPD {#mvpd}

**Datei:** AccessEnabler/headers/model/MVPD.h

**Beschreibung** Beschreibt das MVPD-Objekt. Kann verwendet werden, um Informationen über die Eigenschaften des MVPD zu erhalten.

**Verfügbarkeit:** v1.0+ [boardingStatus-Eigenschaft ist in v2.2 verfügbar]

**Eigenschaften**:

* (NSString) ID - Die MVPD-ID.
* (NSString) displayName - Der MVPD-Name. [Dies sollte verwendet werden, um in der Auswahl angezeigt zu werden]
* (NSString) logoURL - Die Adresse des MVPD-Logos.
* (BOOL) enablePlatformServices - Wenn &quot;true&quot;, unterstützt der MVPD SSO-Dienste wie [APPLE SSO](#presentTvDialog).
* (NSString) boardingStatus - Kann drei Werte haben:
   * nil - Der MVPD unterstützt keine Apple SSO.
   * PICKER - Der MVPD kann in der Apple-Auswahl angezeigt werden, der Authentifizierungsfluss erfolgt jedoch über Adobe.
   * UNTERSTÜTZT - Der MVPD wird vollständig von Apple unterstützt und verwendet das SSO-Token von Apple.

[Zurück nach oben...](#apis)

</br>

## Ereignisse verfolgen {#tracking}

Der AccessEnabler -Trigger verfügt über einen zusätzlichen Callback, der nicht unbedingt mit den Berechtigungsflüssen in Zusammenhang steht. Implementieren der [`sendTrackingData()`](#sendTracking) Die Callback-Funktion ist optional, ermöglicht es der Anwendung jedoch, bestimmte Ereignisse zu verfolgen und Statistiken zu kompilieren, z. B. die Anzahl erfolgreicher/fehlgeschlagener Authentifizierungs-/Autorisierungsversuche.

### sendTrackingData:forEventType: {#sendTracking}

**Datei:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Durch AccessEnabler ausgelöster Rückruf, der der Anwendung das Auftreten verschiedener Ereignisse signalisiert, z. B. das Fertigstellen/Fehlschlagen von Authentifizierungs-/Autorisierungsflüssen. Mit Adobe Pass Authentication 1.6 werden der Gerätetyp, der AccessEnabler-Client-Typ und das Betriebssystem von [`sendTrackingData()`](#sendTracking). Die [`sendTrackingData()`](#sendTracking) callback bleibt abwärtskompatibel.

**Callback: Tracking-Ereignisse**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Verfügbarkeit:** v1.0+

**Hinweis:** Gerätetyp und Betriebssystem werden durch die Verwendung einer öffentlichen Java-Bibliothek (<http://java.net/projects/user-agent-utils>) und die Benutzeragenten-Zeichenfolge. Beachten Sie, dass diese Informationen nur als grobe Methode zur Unterteilung von Betriebsmetriken in Gerätekategorien bereitgestellt werden, dass Adobe jedoch keine Verantwortung für fehlerhafte Ergebnisse übernehmen kann. Verwenden Sie die neue Funktion entsprechend.

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
   * **authenticationDetection:** jedes Mal, wenn eine Authentifizierungsprüfung erfolgt (Ereignis `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** wenn der Benutzer einen MVPD im MVPD-Auswahlformular auswählt (Ereignis ist `TRACKING_GET_SELECTED_PROVIDER`)
* *data*: zusätzliche Daten, die mit dem gemeldeten Ereignis verknüpft sind. Diese Daten werden in Form einer Werteliste dargestellt.

**Ausgelöst von:** `checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN), `checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ), `setSelectedProvider:`

Anweisungen zur Interpretation der Werte im *data* array:

* Für trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Ob die Token-Anfrage erfolgreich war (true/false) und ob erfolgreich:
   * **1** - MVPD ID-Zeichenfolge
   * **2** - GUID (md5-Hash)
   * **3** - Token bereits im Cache (true/false)
   * **4** - Gerätetyp
   * **5** - AccessEnabler-Client-Typ
   * **6** - Betriebssystemtyp

* Für trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Ob die Token-Anfrage erfolgreich war (true/false) und ob erfolgreich:
   * **1** - MVPD ID
   * **2** - GUID (md5-Hash)
   * **3** - Token bereits im Cache (true/false)
   * **4** - Fehler
   * **5** - Details
   * **6** - Gerätetyp
   * **7** - AccessEnabler-Client-Typ
   * **8** - Betriebssystemtyp
* Für trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - Kennung des derzeit ausgewählten MVPD
   * **1** - Gerätetyp
   * **2** - AccessEnabler-Client-Typ
   * **3** - Betriebssystemtyp

</br>
