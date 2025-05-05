---
title: iOS/tvOS-API-Referenz
description: iOS/tvOS-API-Referenz
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '6942'
ht-degree: 0%

---

# (Veraltete) iOS/tvOS SDK API-Referenz {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung {#intro}

Auf dieser Seite werden die Methoden und Rückruffunktionen beschrieben, die vom nativen iOS/tvOS-Client für die Adobe Pass-Authentifizierung bereitgestellt werden. Die hier beschriebenen Methoden und Callback-Funktionen sind in den Header-Dateien `AccessEnabler.h` und `EntitlementDelegate.h` definiert. Sie finden sie in der iOS AccessEnabler-SDK hier: `[SDK directory]/AccessEnabler/headers/api/`


Zugehörige Dokumentation:

* Eine schrittweise Anleitung zur Implementierung von Adobe Pass
Informationen zum Ablauf der Authentifizierungsberechtigungen mithilfe dieser API finden Sie im [iOS-Integrations-Cookbook](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md).
* Die neueste iOS AccessEnabler-SDK finden Sie unter [iOS Native Access Enabler-Bibliothek](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe empfiehlt, nur die Adobe Pass-Authentifizierungs-APIs *Public* zu verwenden:
>
>* Öffentliche APIs sind für alle unterstützten Clienttypen verfügbar und wurden vollständig getestet. Für jede öffentliche Funktion stellen wir sicher, dass jeder Client-Typ über eine entsprechende Version der zugehörigen Methode(n) verfügt.
>* Öffentliche APIs müssen so stabil wie möglich sein, um die Abwärtskompatibilität zu unterstützen und sicherzustellen, dass Partnerintegrationen nicht beschädigt werden. Für nicht-öffentliche APIs behalten wir uns jedoch das Recht vor, ihre Signatur in Zukunft zu ändern. Wenn Sie auf einen bestimmten Fluss stoßen, der nicht durch eine Kombination der aktuellen öffentlichen Adobe Pass-Authentifizierungs-API-Aufrufe unterstützt werden kann, sollten Sie uns dies mitteilen. Unter Berücksichtigung Ihrer Anforderungen können wir die öffentlichen APIs ändern und eine stabile Lösung für die Zukunft bereitstellen.

</br>

## API-Referenz {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Instanziiert das AccessEnabler-Objekt.

* **[VERALTET]** [init](#init) - instanziiert das AccessEnabler-Objekt.

* [`setOptions:options:`](#setOptions) - Konfiguriert globale SDK-Optionen wie profile oder visitorID.

* [`setRequestor:`](#setReqV3) [`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Legt die Identität des Programmierers fest.

* **[VERALTET]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Legt die Identität des Programmierers fest.

* **[VERALTET]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Legt die Identität des Programmierers fest.

* [`setRequestorComplete:`](#setReqComplete) : Informiert Ihr Programm, dass die Konfigurationsphase abgeschlossen ist.

* [`checkAuthentication`](#checkAuthN) - Überprüft den Authentifizierungsstatus des aktuellen Benutzers.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - Startet den vollständigen Authentifizierungs-Workflow.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN) [andFilter](#getAuthN_filter) - Startet den vollständigen Authentifizierungs-Workflow.

* [`displayProviderDialog:`](#dispProvDialog) : Informiert Ihr Programm, die entsprechenden Benutzeroberflächenelemente zu instanziieren, damit die Benutzenden eine MVPD auswählen können.

* [`setSelectedProvider:`](#setSelProv) : Informiert den AccessEnabler über die MVPD-Auswahl des Benutzers.

* [`navigateToUrl:`](#nav2url) : Informiert Ihre Anwendung, dass die Anmeldeseite von MVPD angezeigt werden muss.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) : Informiert Ihre Anwendung, dass der Benutzer mithilfe von SFSafariViewController die Anmeldeseite von MVPD sehen muss.

* [`handleExternalURL:url`](#handleExternalURL) - Schließt den Authentifizierungs-/Abmeldevorgang ab.

* **[VERALTET]** [`getAuthenticationToken`](#getAuthNToken) - Fordert das Authentifizierungstoken vom Back-End-Server an.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) : Informiert Ihre Anwendung über den Status des Authentifizierungsflusses.

* [`checkPreauthorizedResources:`](#checkPreauth) - Bestimmt, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Bestimmt, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen.

* [`preauthorizedResources:`](#preauthResources) - Stellt eine Liste der Ressourcen bereit, die der Benutzer bereits anzeigen darf.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Prüft den Autorisierungsstatus des aktuellen Benutzers.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Startet den Autorisierungsfluss.

* [`setToken:forResource:`](#setToken) : Informiert Ihre Anwendung, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) : Informiert Ihre Anwendung, dass der Autorisierungsfluss fehlgeschlagen ist.

* [`logout`](#logout) - Startet den Abmeldefluss.

* [`getSelectedProvider`](#getSelProv) - Bestimmt den aktuell ausgewählten Anbieter.

* [`selectedProvider:`](#selProv) : Liefert Informationen zum derzeit ausgewählten MVPD für Ihr Programm.

* [`getMetadata:`](#getMeta) - Ruft Informationen ab, die von der AccessEnabler-Bibliothek als Metadaten bereitgestellt werden.

* [`presentTvProviderDialog:`](#presentTvDialog) : Informiert Ihr Programm, das Apple-SSO-Dialogfeld anzuzeigen.

* [`dismissTvProviderDialog:`](#dismissTvDialog) : Informiert Ihr Programm, das Apple-SSO-Dialogfeld auszublenden.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus): Stellt die von einem [`getMetadata:`](#getMeta)-Aufruf angeforderten Metadaten bereit.

* [`sendTrackingData:forEventType:`](#sendTracking): Liefert Informationen zu Tracking-Daten.

* [`MVPD`](#mvpd) - Die MVPD-Klasse. [Enthält Informationen zum MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** instanziiert das AccessEnabler-Objekt. Pro Anwendungsinstanz sollte eine einzige AccessEnabler-Instanz vorhanden sein.

| **API-Aufruf: iOS AccessEnabler-Konstruktor** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Verfügbarkeit:** v3.0+

**Parameter:**

* **softwareStatement:** Eine Zeichenfolge, die die Anwendung im Adobe-System identifiziert. Sehen Sie sich an, wie Sie eine Software-Erklärung erhalten.

[Nach oben…](#apis)



### init - [DEPRECATED]{#init}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** instanziiert das AccessEnabler-Objekt. Pro Anwendungsinstanz sollte eine einzige AccessEnabler-Instanz vorhanden sein.

| API-Aufruf: iOS AccessEnabler-Konstruktor |
| --- |
| ```- (id) init;``` |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

**Parameter:** none

[Nach oben…](#apis)

</br>

### setOptions:options {#setOptions}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Konfiguriert globale SDK-Optionen. Sie akzeptiert ein NSDictionary als Argument. Die Werte aus dem Wörterbuch werden zusammen mit jedem Netzwerkaufruf, den SDK tätigt, an den Server übergeben.

**Hinweis:** Die Werte werden unabhängig vom aktuellen Fluss (Authentifizierung/Autorisierung) an den Server übergeben. Wenn Sie die Werte ändern möchten, können Sie diese Methode jederzeit aufrufen.

| API-Aufruf: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Verfügbarkeit:** v2.3.0+

**Parameter:**

* *options*: Ein NSDictionary, das globale SDK-Optionen enthält. Derzeit sind die folgenden Optionen verfügbar:
   * **applicationProfile** - Kann verwendet werden, um Server-Konfigurationen basierend auf diesem Wert vorzunehmen.
   * **visitorID** - Der Experience Cloud-ID-Service. Dieser Wert kann später für erweiterte Analyseberichte verwendet werden.
   * **handleSVC** - Boolescher Wert, der angibt, ob der Programmierer die SFSafariViewControllers verarbeiten wird. Weitere Informationen finden Sie [SFSafariViewController-Unterstützung auf iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md).
      * Bei Festlegung auf **false** stellt die SDK dem Endbenutzer automatisch einen SFSafariViewController bereit. Die SDK navigiert außerdem zur MVPDs-Anmeldeseiten-URL.
      * Wenn dies auf **true“ festgelegt ist** stellt **SDK dem Endbenutzer NOT** automatisch einen SFSafariViewController bereit. Die SDK führt weitere Trigger **navigieren(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Client-Informationen, wie in [Übergeben von Client-Informationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md) beschrieben.

[Nach oben…](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung bei Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gebracht wird, um mit dem Systemstatus zu synchronisieren (ein Remote-Token abrufen, wenn SSO aktiviert ist, oder das lokale Token löschen, wenn zwischenzeitlich eine Abmeldung stattgefunden hat).

Die Antwort des Servers enthält eine Liste der MVPDs zusammen mit Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Server-Antwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. ERFOLG/FEHLSCHLAGEN) wird Ihrer Anwendung über den `setRequestorComplete:`-Callback angezeigt.

Wenn der `urls` nicht verwendet wird, richtet sich der resultierende Netzwerkaufruf an die standardmäßige Service-Provider-URL: die Adobe-RELEASE-/Produktionsumgebung.


Wenn ein Wert für den `urls`-Parameter angegeben wird, zielt der resultierende Netzwerkaufruf auf alle im `urls`-Parameter angegebenen URLs ab. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Bei der Kompilierung der Liste der MVPDs hat die erste Antwort Vorrang. Für jede MVPD in der Liste speichert AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die mit dem Dienstleister verknüpft ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Anfordererkonfiguration |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Verfügbarkeit:** v3.0+

| API-Aufruf: Anfordererkonfiguration |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Verfügbarkeit:** v3.0+

**Parameter:**

* *RequestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie bei der ersten Registrierung beim Adobe Pass-Authentifizierungs-Service die eindeutige ID, die Sie per Adobe Ihrer Site zugewiesen haben.
* *urls*: Optionaler Parameter; standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zu Debugging-Zwecken verwendet werden). Damit können Sie mehrere Instanzen des Authentifizierungs-Service von Adobe Pass angeben. Dabei besteht die MVPD-Liste aus den Endpunkten aller Dienstleister. Jede MVPD ist mit dem schnellsten Dienstleister verknüpft, d. h. dem Anbieter, der zuerst geantwortet hat und diese MVPD unterstützt.

>[!NOTE]
>
>Wenn die Bibliothek ohne den `serviceProviders` aufgerufen wird, ruft sie die Konfiguration vom standardmäßigen Service Provider ab (d. h. `https://sp.auth.adobe.com` für das Produktionsprofil oder `https://sp.auth-staging.adobe.com` für das Staging-Profil). Wenn der `serviceProviders` angegeben wird, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn doppelte Informationen in verschiedenen Antworten des Dienstleisters vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers gelöst (d. h., der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Callbacks:** [`setRequestorComplete:`](#setReqComplete)

[Nach oben…](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [VERALTET] {#setReq}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung bei Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gebracht wird, um mit dem Systemstatus zu synchronisieren (rufen Sie ein Remote-Token ab, wenn SSO aktiviert ist, oder löschen Sie das lokale Token, wenn eine Abmeldung zwischenzeitlich stattgefunden hat).

Die Antwort des Servers enthält eine Liste der MVPDs zusammen mit Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Server-Antwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. ERFOLG/FEHLSCHLAGEN) wird Ihrer Anwendung über den `setRequestorComplete:`-Callback angezeigt.

Wenn der `urls` nicht verwendet wird, richtet sich der resultierende Netzwerkaufruf an die standardmäßige Service-Provider-URL: die Adobe-RELEASE-/Produktionsumgebung.

Wenn ein Wert für den `urls`-Parameter angegeben wird, zielt der resultierende Netzwerkaufruf auf alle im `urls`-Parameter angegebenen URLs ab. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Bei der Kompilierung der Liste der MVPDs hat die erste Antwort Vorrang. Für jede MVPD in der Liste speichert AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die mit dem Dienstleister verknüpft ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Anfordererkonfiguration |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

| API-Aufruf: Anfordererkonfiguration |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

**Parameter:**

* *RequestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie die eindeutige ID, die von Adobe an Ihre Site zugewiesen wurde, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
* *signedRequestorID*: **Dieser Parameter ist in iOS AccessEnabler der Versionen 1.2 und höher vorhanden.** Eine Kopie der Anforderer-ID , die mit Ihrem privaten Schlüssel digital signiert ist. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Optionaler Parameter; standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zu Debugging-Zwecken verwendet werden). Damit können Sie mehrere Instanzen des Authentifizierungs-Service von Adobe Pass angeben. Dabei besteht die MVPD-Liste aus den Endpunkten aller Dienstleister. Jede MVPD ist mit dem schnellsten Dienstleister verknüpft, d. h. dem Anbieter, der zuerst geantwortet hat und diese MVPD unterstützt.

**Hinweise:** Wird die Bibliothek ohne den Parameter &quot;`serviceProviders`&quot; aufgerufen, ruft sie die Konfiguration vom standardmäßigen Service Provider ab (d. h. `https://sp.auth.adobe.com` für das Produktionsprofil oder `https://sp.auth-staging.adobe.com` für das Staging-Profil). Wenn der `serviceProviders` angegeben wird, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn doppelte Informationen in verschiedenen Antworten des Dienstleisters vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers gelöst (d. h., der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Callbacks:** [`setRequestorComplete:`](#setReqComplete)


[Nach oben…](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [VERALTET] {#setReq_tvos}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Legt die Identität des Programmierers fest. Jedem Programmierer wird bei der Registrierung bei Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Diese Einstellung sollte nur einmal während des Lebenszyklus der Anwendung ausgeführt werden.

Die Antwort des Servers enthält eine Liste der MVPDs zusammen mit Konfigurationsinformationen, die an die Identität des Programmierers angehängt sind. Die Server-Antwort wird intern vom AccessEnabler-Code verwendet. Nur der Status des Vorgangs (d. h. ERFOLG/FEHLSCHLAGEN) wird Ihrer Anwendung über den `setRequestorComplete:`-Callback angezeigt.

Wenn der `urls` nicht verwendet wird, richtet sich der resultierende Netzwerkaufruf an die standardmäßige Service-Provider-URL: die Adobe-RELEASE-/Produktionsumgebung.

Wenn ein Wert für den `urls`-Parameter angegeben wird, zielt der resultierende Netzwerkaufruf auf alle im `urls`-Parameter angegebenen URLs ab. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Bei der Kompilierung der Liste der MVPDs hat die erste Antwort Vorrang. Für jede MVPD in der Liste speichert AccessEnabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die mit dem Dienstleister verknüpft ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Anfordererkonfiguration</th>
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
<th>API-Aufruf: Anfordererkonfiguration</th>
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

* *RequestorID*: Die eindeutige ID, die dem Programmierer zugeordnet ist. Übergeben Sie die eindeutige ID, die von Adobe bei der ersten Verwendung Ihrer Site zugewiesen wurde   beim Adobe Pass-Authentifizierungsdienst registriert.
* *signedRequestorID*: **Dieser Parameter ist in iOS AccessEnabler vorhanden   Versionen 1.2 und höher.** Eine Kopie der Anforderer-ID , die mit Ihrem privaten Schlüssel digital signiert ist. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Optionaler Parameter; standardmäßig der Adobe-Dienstleister   wird verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zu Debugging-Zwecken verwendet werden). Damit können Sie mehrere Instanzen des Authentifizierungs-Service von Adobe Pass angeben. Dabei besteht die MVPD-Liste aus den Endpunkten aller Dienstleister. Jede MVPD ist mit dem schnellsten Dienstleister verknüpft, d. h. dem Anbieter, der zuerst geantwortet hat und diese MVPD unterstützt.
* secret und publicKey: Der geheime und öffentliche Schlüssel, die zum Signieren der zweiten Bildschirmaufrufe verwendet werden. Weitere Informationen finden Sie in der [Client-losen Dokumentation](#create_dev).

Wenn die Bibliothek ohne den `serviceProviders`-Parameter aufgerufen wird, ruft sie die Konfiguration vom standardmäßigen Service-Provider ab (d. h. `https://sp.auth.adobe.com` für das Produktionsprofil oder https://sp.auth-staging.adobe.com für das Staging-Profil). Wenn der `serviceProviders` angegeben wird, muss es sich um ein Array von URLs handeln. Die Konfigurationsinformationen werden von allen angegebenen Endpunkten abgerufen und zusammengeführt. Wenn doppelte Informationen in verschiedenen Antworten des Dienstleisters vorhanden sind, wird der Konflikt zugunsten des am schnellsten reagierenden Servers gelöst (d. h., der Server mit der kürzesten Antwortzeit hat Vorrang).

**Ausgelöste Callbacks:** [`setRequestorComplete:`](#setReqComplete)

[Nach oben…](#apis)

</br>

### setRequestComplete: {#setReqComplete}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, ausgelöst durch den AccessEnabler, der Ihrer Anwendung mitteilt, dass die Konfigurationsphase abgeschlossen ist. Dies ist ein Signal, dass die App mit der Ausstellung von Berechtigungsanfragen beginnen kann. Solange die Konfigurationsphase noch nicht abgeschlossen ist, kann die Anwendung keine Berechtigungsanfragen stellen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Anfordererkonfiguration abgeschlossen</th>
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
   * `ACCESS_ENABLER_STATUS_ERROR` - Konfigurationsphase ist fehlgeschlagen

**Ausgelöst von:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Nach oben…](#apis)

</br>

### checkAuthentication {#checkAuthN}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Prüft den Authentifizierungsstatus des aktuellen Benutzers.
Dies erfolgt, indem im lokalen nach einem gültigen Authentifizierungstoken gesucht wird
Token-Speicherplatz. Diese Methode führt keine Netzwerkaufrufe aus, und es wird empfohlen, sie im Haupt-Thread aufzurufen.
Sie wird von der Anwendung verwendet, um den Authentifizierungsstatus des Benutzers abzufragen und
Aktualisieren Sie die Benutzeroberfläche entsprechend (d. h., aktualisieren Sie die Benutzeroberfläche zum Anmelden/Abmelden). Die
Der Authentifizierungsstatus wird über an die Anwendung übermittelt.
Der [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) Callback.


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

**Parameter:** none

**Ausgelöste Callbacks:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Nach oben…](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Sie beginnt mit der Prüfung des Authentifizierungsstatus. Wenn er nicht bereits authentifiziert ist, wird der Zustandscomputer des Authentifizierungsflusses gestartet:

* Wenn der letzte Authentifizierungsversuch erfolgreich war, wird die MVPD   Die Auswahlphase wird übersprungen und   Der [`navigateToUrl:`](#nav2url)-Callback wird ausgelöst. Die   Die Anwendung verwendet diesen Rückruf, um das WebView-Steuerelement zu instanziieren, das dem Benutzer die Anmeldeseite der MVPD anzeigt. **[HINWEIS: Ab Access Enabler 1.5 ist diese Funktion aufgrund einer Einschränkung in der SDK nicht mehr verfügbar].**
* Wenn der letzte Authentifizierungsversuch nicht erfolgreich war oder sich der Benutzer explizit abgemeldet hat, lautet der [`displayProviderDialog:`](#dispProvDialog)-Rückruf   Ausgelöst. Ihre Anwendung verwendet diesen Callback, um die MVPD-Auswahlbenutzeroberfläche anzuzeigen. Außerdem muss die App den Authentifizierungsfluss fortsetzen, indem sie die AccessEnabler-Bibliothek über die MVPD-Auswahl des Benutzers mithilfe der [`setSelectedProvider:`](#setSelProv) informiert.

Da die Anmeldeinformationen der Benutzenden auf der Anmeldeseite von MVPD überprüft werden, muss die Anwendung die verschiedenen Umleitungsvorgänge überwachen, die stattfinden, während sich die Benutzenden auf der Anmeldeseite von MVPD authentifizieren. Wenn die korrekten Anmeldeinformationen eingegeben werden, wird das WebView-Steuerelement an eine benutzerdefinierte URL umgeleitet, die durch die `ADOBEPASS_REDIRECT_URL`-Konstante definiert wird. Diese URL ist nicht zum Laden durch die WebView vorgesehen. Die Anwendung muss diese URL abfangen und dieses Ereignis als Signal interpretieren, dass die Anmeldephase abgeschlossen ist. Anschließend sollte die Steuerung an AccessEnabler übergeben werden, um den Authentifizierungsfluss abzuschließen (durch Aufruf der Methode [handleExternalURL](#handleExternalURL)).

Schließlich wird der Authentifizierungsstatus über den [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)-Callback an die Anwendung übermittelt.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Startet den Authentifizierungsfluss</th>
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
<th>API-Aufruf: Startet den Authentifizierungsfluss</th>
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

* *forceAuth*: Ein Flag, das angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Passdienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Nach oben…](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Startet den vollständigen Authentifizierungs-Workflow. Sie beginnt mit der Prüfung des Authentifizierungsstatus. Wenn er nicht bereits authentifiziert ist, wird der Zustandscomputer des Authentifizierungsflusses gestartet:

* [presentTvProviderDialog()](#presentTvDialog) wird aufgerufen, wenn der aktuelle Anforderer über mindestens einen MVPD verfügt, der SSO unterstützt. Wenn MVPD SSO nicht unterstützt, beginnt der klassische Authentifizierungsfluss und der Filterparameter wird ignoriert.
* Nachdem der Benutzer den Apple SSO-Fluss abgeschlossen hat, wird [`dismissTvProviderDialog()`](#dismissTvDialog) ausgelöst und der Authentifizierungsprozess abgeschlossen.

Schließlich wird der Authentifizierungsstatus über den [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)-Callback an die Anwendung übermittelt.

**Verfügbarkeit:** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Startet den Authentifizierungsfluss</th>
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
<th>API-Aufruf: Startet den Authentifizierungsfluss</th>
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

* *forceAuth*: Ein Flag, das angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Passdienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.
* filter: Ein Wörterbuch mit zwei Listen von MVPD-IDs, die im Apple-SSO-Dialogfeld angezeigt werden sollen. Alle MVPD, die SSO nicht unterstützen, werden ignoriert, aber die Reihenfolge wird eingehalten. Das Wörterbuch muss zwei Schlüssel haben:
   * TV\_PROVIDERS: Eine Liste mit allen MVPDs, die in der Auswahl angezeigt werden sollen
   * FEATURED\_TV\_PROVIDERS: Eine Liste mit allen MVPDs, die als in der Auswahl enthalten markiert werden sollen. MVPDs in dieser Liste müssen auch in der Liste der TV\_PROVIDERS angegeben werden.

**Verfügbarkeit:** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Startet den Authentifizierungsfluss</th>
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
<th>API-Aufruf: Startet den Authentifizierungsfluss</th>
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

* *forceAuth*: Ein Flag, das angibt, ob der Authentifizierungsfluss gestartet werden soll, unabhängig davon, ob der Benutzer bereits authentifiziert ist oder nicht.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Passdienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.
* filter: Eine Liste der MVPD-IDs, die im Apple-SSO-Dialogfeld angezeigt werden sollen. Alle MVPD, die SSO nicht unterstützen, werden ignoriert, aber die Reihenfolge wird eingehalten.

**Ausgelöste Callbacks:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Nach oben…](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Vom AccessEnabler ausgelöster Callback, um die Anwendung darüber zu informieren, dass die entsprechenden Benutzeroberflächenelemente instanziiert werden müssen, damit der Benutzer die gewünschte MVPD auswählen kann. Der Callback bietet eine Liste von MVPD-Objekten mit zusätzlichen Informationen, die dazu beitragen können, das Auswahlbenutzeroberflächenbedienfeld korrekt zu erstellen (z. B. die URL, die auf das MVPD-Logo verweist, den Anzeigenamen usw.)

Nachdem der Benutzer die gewünschte MVPD ausgewählt hat, muss die Anwendung der oberen Ebene den Authentifizierungsfluss fortsetzen, indem `setSelectedProvider:` aufgerufen und ihr die ID der MVPD übergeben wird, die der Auswahl des Benutzers entspricht.

**Abbruch des Authentifizierungsflusses** - Dies ist ein Punkt, an dem der Benutzer die Schaltfläche „Zurück“ drücken kann, was einem Abbruch des Authentifizierungsflusses entspricht. In diesem Szenario muss die Anwendung die Methode [setSelectedProvider:](#setSelProv) aufrufen, wobei null als Parameter übergeben wird, um AccessEnabler die Möglichkeit zu geben, seinen Authentifizierungszustand auf seinem Computer zurückzusetzen.

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

* *mvpds*: Liste der MVPD-Objekte, die MVPD-bezogene Informationen enthalten, mit denen die Anwendung die MVPD-Auswahlbenutzeroberflächenelemente erstellen kann.

**Ausgelöst durch:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Nach oben…](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Access Enabler über die MVPD-Auswahl des Benutzers zu informieren. Die Anwendung kann diese Methode verwenden, um den für die Authentifizierung verwendeten Dienstleister auszuwählen oder zu ändern.

Wenn die ausgewählte MVPD eine TempPass-MVPD ist, wird sie automatisch mit dieser MVPD authentifiziert, ohne dass getAuthentication() später aufgerufen werden muss.

Beachten Sie, dass dies für den temporären Pass zu Werbezwecken nicht möglich ist, wenn zusätzliche Parameter für die getAuthentication()-Methode angegeben werden.

Wenn *null* als Parameter übergeben wird, geht Access Enabler davon aus, dass der Benutzer den Authentifizierungsfluss abgebrochen hat (d. h. die Schaltfläche „Zurück“ gedrückt hat), und antwortet, indem er den Authentifizierungsstatus-Computer zurücksetzt und den [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)-Callback mit dem `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` Fehlercode aufruft.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Festlegen des aktuell ausgewählten Anbieters</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Nach oben…](#apis)

</br>

#### navigateToUrl: {#nav2url}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback, der vom AccessEnabler ausgelöst wird, um Ihre Anwendung aufzufordern, einen UIWebView/WKWebView-Controller zu instanziieren und die im **`url`** des Callbacks angegebene URL zu laden. Der Callback übergibt den **`url`**, der die URL des Authentifizierungsendpunkts oder die URL des Abmeldeendpunkts darstellt.

Während der UIWebView/WKWebView` `Controller mehrere Weiterleitungen durchläuft, muss die Anwendung die Aktivität des Controllers überwachen und den Zeitpunkt erkennen, zu dem eine bestimmte benutzerdefinierte URL geladen wird, die durch die `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`) definiert ist. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht vom Controller geladen werden soll. Sie darf von Ihrer Anwendung nur als Signal interpretiert werden, dass der Authentifizierungs- oder Abmeldefluss abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss die Anwendung die UIWebView/WKWebView schließen und die Methode `handleExternalURL:url `API von AccessEnabler aufrufen.

**Hinweis:** Bitte beachten Sie, dass im Falle des Authentifizierungsflusses dies ein Punkt ist, an dem der Benutzer die Schaltfläche „Zurück“ drücken kann, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss die Anwendung die Methode [setSelectedProvider:](#setSelProv) aufrufen, die **`nil`** als Parameter übergibt und dem AccessEnabler die Möglichkeit gibt, seinen Authentifizierungszustandsrechner zurückzusetzen.

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

* *url*: Die URL, die auf die Anmeldeseite von MVPD verweist

**Ausgelöst von:** [setSelectedProvider:](#setSelProv)



[Nach oben…](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback, der durch den AccessEnabler anstelle des `navigateToUrl:`-Callbacks ausgelöst wird, falls Ihre Anwendung zuvor die manuelle Verarbeitung des Safari View Controller (SVC) über den Aufruf [setOptions(\[„handleSVC“:true“\]) aktiviert hat](#setOptions) und nur für MVPDs, für die Safari View Controller (SVC) erforderlich ist. Für alle anderen MVPDs wird der `navigateToUrl:` Callback aufgerufen. Bitte lesen Sie [SFSafariViewController-Unterstützung auf iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md) für Details, wie Safari View Controller (SVC) verwaltet werden sollte.

Ähnlich wie beim `navigateToUrl:`-Callback wird der `navigateToUrl:useSVC:` vom AccessEnabler ausgelöst, um die Anwendung aufzufordern, einen `SFSafariViewController`-Controller zu instanziieren und die im **`url`** des Callbacks angegebene URL zu laden. Der Callback übergibt den **`url`**, der die URL des Authentifizierungsendpunkts oder die URL des Abmeldeendpunkts darstellt, und den **`useSVC`**, der angibt, dass die Anwendung einen `SFSafariViewController` verwenden muss.

Während der `SFSafariViewController` mehrere Weiterleitungen durchläuft, muss die Anwendung die Aktivität des Controllers überwachen und den Zeitpunkt erkennen, zu dem eine bestimmte, von Ihrer `application's custom scheme` definierte URL geladen wird (z. B. **&#x200B; **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht vom Controller geladen werden soll. Sie darf von Ihrer Anwendung nur als Signal interpretiert werden, dass der Authentifizierungs- oder Abmeldefluss abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss die Anwendung die `SFSafariViewController` schließen und die Methode &quot;`handleExternalURL:url `&quot; von AccessEnabler aufrufen.

**Hinweis:** Bitte beachten Sie, dass im Falle des Authentifizierungsflusses dies ein Punkt ist, an dem der Benutzer die Schaltfläche „Zurück“ drücken kann, was dem Abbruch des Authentifizierungsflusses entspricht. In einem solchen Szenario muss die Anwendung die Methode [setSelectedProvider:](#setSelProv) aufrufen, die **`nil`** als Parameter übergibt und dem AccessEnabler die Möglichkeit gibt, seinen Authentifizierungszustandsrechner zurückzusetzen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: MVPD-Anmeldeseite in SFSafariViewController anzeigen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
&#x200B;- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:**&#x200B;v 3.2+

**Parameter**:

* *url:* die URL, die auf die Anmeldeseite von MVPD verweist
* *useSVC:*, ob die URL in SFSafariViewController geladen werden soll.

**Ausgelöst von:**&#x200B;[ setOptions:](#setOptions) vor [setSelectedProvider:](#setSelProv)

[Nach oben…](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Authentifizierungs- oder Abmeldefluss abzuschließen. Diese Methode sollte direkt aufgerufen werden, nachdem die Anwendung den Zeitpunkt erkannt hat, zu dem der `UIWebView/WKWebView or SFSafariViewController`-Controller zu einer bestimmten benutzerdefinierten URL umgeleitet wird. Wenn Ihre Anwendung einen Controller verwenden muss `SFSafariViewController `die spezifische benutzerdefinierte URL wird durch Ihre `application's custom scheme` definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), andernfalls wird diese spezifische benutzerdefinierte URL durch die `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`) definiert.

Im Falle des Authentifizierungsflusses vervollständigt AccessEnabler den Fluss, indem es das Authentifizierungstoken vom Back-End-Server abruft und lokal im Token-Speicher speichert. Der AccessEnabler informiert Ihre Anwendung über den Abschluss des Authentifizierungsflusses, indem er den `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)-->-Callback mit dem Status-Code 1 aufruft, was auf Erfolg hinweist. Wenn während der Ausführung dieser Schritte ein Fehler auftritt, wird der `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)-->-Callback mit dem Status-Code 0 ausgelöst, was auf einen Authentifizierungsfehler hinweist, sowie mit einem entsprechenden Fehler-Code.

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

* *url*: Die abgefangene URL aus dem ` UIWebView/WKWebView or SFSafariViewController ` als Zeichenfolge.


**Ausgelöste Callbacks:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Nach oben…](#apis)

</br>

#### getAuthenticationToken - [VERALTET] {#getAuthNToken}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Schließt den Authentifizierungsfluss ab, indem das Authentifizierungstoken vom Backend-Server angefordert wird. Diese Methode sollte von der Anwendung nur als Antwort auf ein Ereignis aufgerufen werden, bei dem das WebView-Steuerelement, das die MVPD-Anmeldeseite hostet, an die von der `ADOBEPASS_REDIRECT_URL` definierte benutzerdefinierte URL umgeleitet wird.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Authentifizierungs-Token abrufen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+ **Bis:** v3.0

**Parameter:** none

**Ausgelöste Callbacks:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Nach oben…](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, ausgelöst durch den AccessEnabler, der die Anwendung über den Status des Authentifizierungsflusses informiert. Es gibt viele Stellen, an denen dieser Fluss fehlschlagen kann, entweder aufgrund der Interaktion des Benutzers oder aufgrund anderer unvorhergesehener Szenarien (z. B. Probleme mit der Netzwerkverbindung usw.). Dieser Rückruf informiert die Anwendung über den Erfolgs-/Fehlerstatus des Authentifizierungsflusses und liefert bei Bedarf zusätzliche Informationen zur Fehlerursache.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Meldet den Status des Authentifizierungsflusses</th>
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
* *code*: Fehlerursache. Wenn *status* den Wert `ACCESS_ENABLER_STATUS_SUCCESS` hat, ist *code* eine leere Zeichenfolge (d. h. definiert durch die `USER_AUTHENTICATED`). Im Falle eines Fehlers kann dieser Parameter einen der folgenden Werte annehmen:
   * `USER_NOT_AUTHENTICATED_ERROR` - Der Benutzer ist nicht authentifiziert. Als Antwort auf den Aufruf [checkAuthentication:](#checkAuthN)-Methode, wenn im Cache des lokalen Tokens kein gültiges Authentifizierungstoken vorhanden ist.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler hat das       Authentifizierungszustandsmaschine nach der Upper-Layer-Anwendung       hat *null* an [`setSelectedProvider:`](#setSelProv) übergeben, um den Authentifizierungsfluss abzubrechen.  Vermutlich hat der Benutzer den Authentifizierungsfluss abgebrochen (d.h. die Schaltfläche „Zurück“ gedrückt).
   * `GENERIC_AUTHENTICATION_ERROR` - Der Authentifizierungsfluss ist aus Gründen wie der Nichtverfügbarkeit des Netzwerks oder dem expliziten Abbruch des Authentifizierungsflusses fehlgeschlagen.

**Ausgelöst durch:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Nach oben…](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der primäre Zweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der **der Benutzeroberfläche verwendet werden können (z. B. die Angabe des Zugriffsstatus mit Sperren- und Entsperren-Symbolen).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Festlegen des aktuell ausgewählten Anbieters</th>
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

* *resources:* Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die     Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im Aufruf, d. h. sie sollte ein vereinbarter Wert sein, der zwischen dem Programmierer und dem MVPD oder einem RSS-Medienfragment festgelegt wird.

**Rückruf ausgelöst:** [`preauthorizedResources:`](#preauthResources)

[Nach oben…](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um festzustellen, ob der Benutzer bereits berechtigt ist, bestimmte geschützte Ressourcen anzuzeigen. Der primäre Zweck dieser Methode besteht darin, Informationen abzurufen, die zum Dekorieren der Benutzeroberfläche verwendet werden können (z. B. die Angabe des Zugriffsstatus mit Sperren- und Entsperren-Symbolen). Der **cache**-Parameter steuert, ob der interne Cache zum Auflösen von Ressourcen verwendet wird.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Festlegen des aktuell ausgewählten Anbieters</th>
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

* *resources:* Array von Ressourcen, für die die Autorisierung überprüft werden soll. Jedes Element in der Liste sollte eine Zeichenfolge sein, die die Ressourcen-ID darstellt. Die Ressourcen-ID unterliegt denselben Einschränkungen wie die Ressourcen-ID im `getAuthorization:`-Aufruf, d. h. sie sollte ein vereinbarter Wert sein, der zwischen dem Programmierer und dem MVPD oder einem RSS-Medienfragment festgelegt wird.
* *cache:* Boolescher Wert, der angibt, ob der interne Cache zum Auflösen von Ressourcen verwendet werden soll. Bei „false“ wird der Cache umgangen, was bei jedem Aufruf dieser API zu Server-Aufrufen führt.

**Rückruf ausgelöst:** [`preauthorizedResources:`](#preauthResources)

[Nach oben…](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung:** Callback, ausgelöst durch `checkPreauthorizedResources:`. Stellt eine Liste der Ressourcen bereit, die der Benutzer bereits anzeigen darf.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Festlegen des aktuell ausgewählten Anbieters</th>
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

* `resources`: Array von Ressourcen, die der Benutzer bereits anzeigen darf.

**Ausgelöst von:** [`checkPreauthorizedResources:`](#checkPreauth)



[Nach oben…](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsstatus zu überprüfen. Zunächst wird der Authentifizierungsstatus überprüft. Wenn er nicht authentifiziert ist, wird der [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed)-Rückruf ausgelöst und die Methode beendet. Wenn der Benutzer authentifiziert ist, führt dies auch zu einem Trigger des Autorisierungsflusses. Siehe Details zur [`getAuthorization:`](#getAuthZ).


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Autorisierungsstatus überprüfen</th>
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
<th>API-Aufruf: Autorisierungsstatus überprüfen</th>
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

* *resource*: Die ID der Ressource, für die der Benutzer eine Autorisierung anfordert.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Passdienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Nach oben…](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von der Anwendung verwendet, um den Autorisierungsfluss zu starten. Wenn der Benutzer noch nicht authentifiziert ist, wird auch der Authentifizierungsfluss initiiert. Wenn der Benutzer authentifiziert wird, fährt AccessEnabler mit der Ausgabe von Anfragen für das Autorisierungs-Token (wenn im lokalen Token-Cache kein gültiges Autorisierungs-Token vorhanden ist) und für das kurzlebige Medien-Token fort. Sobald das kurze Medien-Token abgerufen wurde, wird der Autorisierungsfluss als abgeschlossen betrachtet. Der [`setToken:forResource:`](#setToken)-Callback wird ausgelöst und das Kurz-Medien-Token wird als Parameter an die Anwendung gesendet. Wenn die Autorisierung aus irgendeinem Grund fehlschlägt, wird der [`tokenRequestFailed:forEventType:`](#tokenReqFailed)-Callback ausgelöst und der Fehler-Code/die Details werden angegeben.

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

* *resource*: Die ID der Ressource, für die der Benutzer eine Autorisierung anfordert.
* *data*: Ein Wörterbuch, das aus Schlüssel-Wert-Paaren besteht, die an den Pay-TV-Passdienst gesendet werden. Adobe kann diese Daten verwenden, um zukünftige Funktionen zu aktivieren, ohne die SDK zu ändern.

**Ausgelöste Callbacks:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Zusätzliche ausgelöste Callbacks:**\
Diese Methode kann auch die folgenden Callbacks mit Trigger versehen (wenn der Authentifizierungsfluss ebenfalls initiiert wird): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Verwenden Sie nach Möglichkeit `checkAuthorization:` / `checkAuthorization:withData:` anstelle von `getAuthorization:` / `getAuthorization:withData:`. Die `getAuthorization:`/`getAuthorization:withData:`-Methode startet einen vollständigen Authentifizierungsfluss (wenn der Benutzer nicht authentifiziert ist), was zu einer komplizierten Implementierung auf Seiten des Programmierers führen kann.

[Nach oben…](#apis)

</br>

### `setToken:forResource:` {#setToken}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, ausgelöst durch den AccessEnabler, der Ihrer Anwendung mitteilt, dass der Autorisierungsfluss erfolgreich abgeschlossen wurde. Das kurzlebige Medien-Token wird ebenfalls als Parameter bereitgestellt.


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

* *Token*: Das kurzlebige Medien-Token
* *resource*: Die Ressource, für die die Autorisierung abgerufen wurde

**Ausgelöst durch:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Nach oben…](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Vom AccessEnabler ausgelöster Callback, der die Anwendung der oberen Ebene darüber informiert, dass der Autorisierungsfluss fehlgeschlagen ist.

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
* *code*: Der mit dem Fehlerszenario verknüpfte Fehler-Code. Mögliche Werte:
   * `USER_NOT_AUTHORIZED_ERROR` - Der Benutzer konnte Folgendes nicht autorisieren
für die angegebene Ressource
* *description*: Zusätzliche Details zum Fehlerszenario. Wenn diese beschreibende Zeichenfolge aus irgendeinem Grund nicht verfügbar ist, sendet die Adobe Pass-Authentifizierung eine leere Zeichenfolge **(“„)**.\
  Diese Zeichenfolge kann von einem MVPD verwendet werden, um benutzerdefinierte Fehlermeldungen oder verkaufsbezogene Meldungen zu übergeben. Wenn beispielsweise einem Abonnenten die Autorisierung für eine Ressource verweigert wird, kann der MVPD eine Nachricht senden, die wie folgt lautet: „Sie haben derzeit keinen Zugriff auf diesen Kanal in Ihrem Paket. Wenn Sie Ihr Paket aktualisieren möchten, klicken Sie **hier**. Die Nachricht wird von der Adobe Pass-Authentifizierung über diesen Callback an den Programmierer weitergeleitet, der die Möglichkeit hat, sie anzuzeigen oder zu ignorieren. Die Adobe Pass-Authentifizierung kann diesen Parameter auch verwenden, um eine Benachrichtigung über die Bedingung bereitzustellen, die zu einem Fehler geführt haben könnte. Beispiel: „Bei der Kommunikation mit dem Autorisierungsdienst des Anbieters ist ein Netzwerkfehler aufgetreten“.

**Ausgelöst durch:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Nach oben…](#apis)

</br>

### Abmelden {#logout}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Diese Methode wird von Ihrer Anwendung aufgerufen, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Umleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Da dieser Fluss nicht mit einer einfachen HTTP-Anfrage abgeschlossen werden kann, die von der AccessEnabler-Bibliothek ausgegeben wird, muss ein `UIWebView/WKWebView or SFSafariViewController`-Controller instanziiert werden, um die HTTP-Umleitungsvorgänge verfolgen zu können.

Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss dadurch, dass der Benutzer in keiner Weise mit dem `UIWebView/WKWebView or SFSafariViewController`-Controller interagieren muss. Daher empfiehlt Adobe, das Steuerelement während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.

Es wird ein ähnliches Muster wie beim Authentifizierungsfluss verwendet. Der iOS AccessEnabler-Trigger verwendet den `navigateToUrl:`-Callback oder den `navigateToUrl:useSVC:`, um einen `UIWebView/WKWebView or SFSafariViewController` Controller zu erstellen und die im `url` des Callbacks angegebene URL zu laden. Dies ist die URL des Abmeldeendpunkts auf dem Backend-Server. Für den tvOS AccessEnabler wird weder der `navigateToUrl:` Callback noch der `navigateToUrl:useSVC:` Callback aufgerufen.

Während mehrere Weiterleitungen durchlaufen, muss die Anwendung die Aktivität des Controllers überwachen `UIWebView/WKWebView or SFSafariViewController ` den Zeitpunkt erkennen, zu dem eine bestimmte benutzerdefinierte URL geladen wird. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht vom Controller geladen werden soll. Sie darf von Ihrer Anwendung nur als Signal interpretiert werden, dass der Abmeldefluss abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss die Anwendung den Controller schließen und die Methode &quot;`handleExternalURL:url `&quot; von AccessEnabler aufrufen. Wenn Ihre Anwendung einen Controller verwenden muss `SFSafariViewController `die spezifische benutzerdefinierte URL wird durch Ihre `application's custom scheme` definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), andernfalls wird diese spezifische benutzerdefinierte URL durch die `ADOBEPASS_REDIRECT_URL `Konstante (d. h. `adobepass://ios.app`) definiert.

Am Ende ruft AccessEnabler den [`setAuthenticationStatus()`](#setAuthNStatus) Callback mit dem Status-Code 0 auf, was den Erfolg des Abmeldevorgangs anzeigt.

**Hinweis:** Wenn der Benutzer mit Apple SSO angemeldet ist, wird der VSA203-Status ausgelöst. In diesem Fall sollte der Benutzer angewiesen werden, sich auch von den Systemeinstellungen abzumelden. Andernfalls erfolgt eine erneute Authentifizierung, wenn die Anwendung neu gestartet wird.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Initiieren des Abmeldevorgangs</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Nach oben…](#apis)

</br>

### getSelectedProvider {#getSelProv}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Verwenden Sie diese Methode, um den aktuell ausgewählten Anbieter zu bestimmen.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Bestimmen des aktuell ausgewählten MVPDS</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**Verfügbarkeit:** v1.0+

**Parameter:** none

**Ausgelöste Callbacks:** [`selectedProvider:`](#selProv)

[Nach oben…](#apis)

</br>

### selectedProvider {#selProv}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Vom AccessEnabler ausgelöster Callback, der Informationen über die aktuell ausgewählte MVPD an die Anwendung liefert.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Informationen zur aktuell ausgewählten MVPD</th>
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

[Nach oben…](#apis)

</br>

### getMetadata: {#getMeta}

**file:** AccessEnabler/headers/AccessEnabler.h

**Beschreibung:** Verwenden Sie diese Methode, um Informationen abzurufen, die von der AccessEnabler-Bibliothek als Metadaten bereitgestellt werden. Die Anwendung kann auf diese Daten zugreifen, indem sie einen auf dem Wörterbuch basierenden Eingabeparameter *Schlüssel* bereitstellt.

Programmierern stehen zwei Arten von Metadaten zur Verfügung:

* Statische Metadaten (Authentifizierungstoken-TTL, Autorisierungstoken-TTL und Geräte-ID)
* Benutzermetadaten (benutzerspezifische Informationen wie Benutzer-ID, Postleitzahl; können während der Authentifizierungs- und Autorisierungsflüsse von einer MVPD an das Gerät einer Benutzerin oder eines Benutzers übergeben werden)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aufruf: Abfragen des AccessEnabler nach Metadaten</th>
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

* *keyDictionary*: eine Wörterbuchdatenstruktur mit folgender Konfiguration
Format:
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_AUTHENTICATION` ist, wird die Abfrage durchgeführt, um die Ablaufzeit des Authentifizierungstokens abzurufen.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_AUTHORIZATION` ist **und**\
     Schlüssel ist `METADATA_RESOURCE_ID_KEY` und Wert ist eine bestimmte Ressourcen-ID, dann wird die Abfrage durchgeführt, um die Ablaufzeit des Autorisierungs-Tokens abzurufen, das mit der angegebenen Ressource verknüpft ist.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_DEVICE_ID` ist, wird die Abfrage durchgeführt, um die aktuelle Geräte-ID abzurufen. Beachten Sie, dass diese Funktion standardmäßig deaktiviert ist und Programmierer sich an Adobe wenden sollten, um Informationen zu Aktivierung und Gebühren zu erhalten.
   * Wenn der Schlüssel `METADATA_OPCODE_KEY` und der Wert `METADATA_USER_META` ist **und** der Schlüssel `METADATA_USER_META_KEY` und der Wert der Name der Metadaten ist, wird die Abfrage für Benutzermetadaten durchgeführt. Die Liste der verfügbaren Benutzer-Metadatentypen:
      * `zip` - Liste der Postleitzahlen
      * `householdID` - Haushaltskennung. Wenn eine MVPD keine Unterkonten unterstützt, ist dies mit `userID` identisch.
      * `maxRating` - Eine Sammlung von maximalen elterlichen Bewertungen für den Benutzer
      * `userID` - Die Benutzerkennung. Wenn eine MVPD Unterkonten unterstützt und der/die Benutzende nicht das Hauptkonto ist, unterscheidet sich `userID` von `householdID.`
      * `channelID` : Eine Liste der Kanäle, die eine Benutzerin oder ein Benutzer anzeigen darf.

  >[!NOTE]
  >
  >Welche Benutzermetadaten einem Programmierer tatsächlich zur Verfügung stehen, hängt davon ab, was MVPD zur Verfügung stellt. Diese Liste wird erweitert, sobald neue Metadaten verfügbar gemacht und zum Adobe Pass-Authentifizierungssystem hinzugefügt werden.

**Ausgelöste Callbacks:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

[Nach oben…](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Vom AccessEnabler nach dem Aufruf von [getAuthentication() ausgelöster Callback](#getAuthN) wenn der aktuelle Anforderer mindestens eine MVPD mit SSO-Unterstützung unterstützt.

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

* viewController: Stellt das Apple-SSO-Dialogfeld dar. Dieser ViewController muss auf dem Bildschirm angezeigt werden.

**Ausgelöst von:** [`getAuthentication`](#getAuthN)

**Weitere Informationen:** [iOS/tvOS Single Sign-On](#presentTvDialog)

[Nach oben…](#apis)

</br>

### disableTVProviderDialog {#dismissTvDialog}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, der vom AccessEnabler ausgelöst wird, nachdem der Benutzer das Apple-SSO-Dialogfeld geschlossen hat.

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

* viewController: Stellt das Apple-SSO-Dialogfeld dar. Dieser ViewController muss vom Bildschirm entfernt werden.

**Ausgelöst durch:** Benutzeraktion

**Weitere Informationen:** [iOS/tvOS Single Sign-On](#presentTvDialog)

[Nach oben…](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Callback, ausgelöst durch den AccessEnabler, der die über einen [`getMetadata:`](#getMeta)-Aufruf angeforderten Metadaten bereitstellt.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: Ergebnis der Anfrage zum Abrufen von Metadaten</th>
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

* *metadata*: Die angeforderten Metadaten. Dieser Wert ist ein `NSString` bei statischen Metadaten (Authentifizierungs-TTL, Autorisierungs-TTL, Geräte-ID).  Dies ist ein komplexes Objekt bei der Anforderung benutzerspezifischer Metadaten. Dieses komplexe Objekt ist in der Regel die Objective-C-Darstellung einer JSON-Payload (z. B. &quot;{„street“: „Main Avenue“, „building“: [„150“, „320“]}&#39; wird in Objective-C als NSDictionary(„street“ -> „Main Avenue“, „building“ -> NSArray(„150“, „320„)) übersetzt.   JSON-Beispielobjekt für Metadaten:

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

* *Encrypted*: Boolescher Wert, der angibt, ob die abgerufenen Metadaten verschlüsselt sind oder nicht. Dieser Parameter ist nur für Benutzermetadaten-Anfragen von Bedeutung. Er hat keine Bedeutung für statische Metadaten (z. B. Authentifizierungs-TTL), die immer unverschlüsselt empfangen werden. Wenn dieser Parameter auf „true“ gesetzt ist, muss der Programmierer den unverschlüsselten Wert der Benutzermetadaten abrufen, indem er eine RSA-Entschlüsselung mit dem privaten Schlüssel für die Whitelist durchführt (derselbe private Schlüssel, der für das Signieren der Anforderer-ID in den [`setRequestor:setSignedRequestorId:`](#setReq)- und `setRequestor:setSignedRequestorId:serviceProviders: `Aufrufen verwendet wird).

* *key*: Der zur Formulierung der Metadaten-Abrufanfrage verwendete Schlüssel.

* *arguments*: Dasselbe Wörterbuch, das an den [`getMetadata:`](#getMeta)-Aufruf übergeben wurde. Dies wird bereitgestellt, damit die Anwendung die Anfragen den Antworten zuordnen kann.

**Ausgelöst von:** [`getMetadata:`](#getMeta)

**Weitere Informationen:** [Benutzermetadaten](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)


[Nach oben…](#apis)

</br>

### MVPD {#mvpd}

**file:** AccessEnabler/headers/model/MVPD.h

**Beschreibung** Beschreibt das MVPD-Objekt. Kann verwendet werden, um Informationen über die Eigenschaften der MVPD zu erhalten.

**Verfügbarkeit:** v1.0+ [boardingStatus -Eigenschaft ist ab v2.2 verfügbar]

**Eigenschaften**:

* (NSString) ID - Die MVPD-ID.
* (NSString) displayName : Der MVPD-Name. [Dies sollte verwendet werden, um in der Auswahl anzuzeigen]
* (NSString) logoURL : Die MVPD-Logo-Adresse.
* (BOOL) enablePlatformServices - Wenn „true“, unterstützt der MVPD SSO-Services wie [Apple SSO](#presentTvDialog).
* (NSString) boardingStatus - Kann 3 Werte haben:
   * null - MVPD unterstützt Apple SSO nicht.
   * AUSWAHL - Die MVPD kann in der Apple-Auswahl angezeigt werden, der Authentifizierungsfluss wird jedoch per Adobe ausgeführt.
   * UNTERSTÜTZT - MVPD wird von Apple vollständig unterstützt und verwendet das Apple-SSO-Token.

[Nach oben…](#apis)

</br>

## Tracking von Ereignissen {#tracking}

Der AccessEnabler-Trigger führt einen zusätzlichen Callback aus, der nicht unbedingt mit den Berechtigungsflüssen in Zusammenhang steht. Die Implementierung der Rückruffunktion &quot;[`sendTrackingData()`](#sendTracking)&quot; ist optional, aber sie ermöglicht es der Anwendung, bestimmte Ereignisse zu verfolgen und Statistiken wie die Anzahl erfolgreicher/fehlgeschlagener Authentifizierungs-/Autorisierungsversuche zu erstellen.

### sendTrackingData:forEventType: {#sendTracking}

**file:** AccessEnabler/headers/EntitlementDelegate.h

**Beschreibung** Vom AccessEnabler ausgelöster Callback, der der Anwendung das Auftreten verschiedener Ereignisse wie Abschluss/Fehler von Authentifizierungs-/Autorisierungsflüssen signalisiert. Bei der Adobe Pass-Authentifizierung 1.6 werden der Gerätetyp, der AccessEnabler-Client-Typ und das Betriebssystem von [`sendTrackingData()`](#sendTracking) gemeldet. Der [`sendTrackingData()`](#sendTracking) Callback bleibt abwärtskompatibel.

**Callback: Tracking von Ereignissen**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Verfügbarkeit:** v1.0+

**Hinweis:** Der Gerätetyp und das Betriebssystem werden mithilfe einer öffentlichen Java-Bibliothek (<http://java.net/projects/user-agent-utils>) und der Benutzeragenten-Zeichenfolge abgeleitet. Beachten Sie, dass diese Informationen nur als grobe Methode zur Unterteilung der Betriebsmetriken in Gerätekategorien bereitgestellt werden, aber dass Adobe keine Verantwortung für falsche Ergebnisse übernehmen kann. Bitte verwenden Sie die neue Funktion entsprechend.

* Mögliche Werte für den Gerätetyp:
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* Mögliche Werte für den AccessEnabler-Client-Typ:
   * `flash`
   * `html5`
   * `ios`
   * `android`


**Parameter**:

* *event*: der Code des Ereignisses, das verfolgt wird. Es gibt drei mögliche Typen von Tracking-Ereignissen:
   * **authorizationDetection:** jedes Mal, wenn eine Autorisierungs-Token-Anfrage zurückgibt (Ereignis ist `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** jedes Mal, wenn eine Authentifizierungsprüfung erfolgt (Ereignis wird `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** Wenn Benutzende eine MVPD im MVPD-Auswahlformular auswählen (Ereignis wird `TRACKING_GET_SELECTED_PROVIDER`)
* *data*: Zusätzliche Daten, die mit dem gemeldeten Ereignis verknüpft sind. Diese Daten werden in Form einer Werteliste angezeigt.

**Ausgelöst durch:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Anweisungen zum Interpretieren der Werte im *data*-Array:

* Für trackingEventType-`TRACKING_AUTHENTICATION:`
   * **0** - Ob die Token-Anfrage erfolgreich war (true/false) und ob sie erfolgreich war:
   * **1** - MVPD-ID-Zeichenfolge
   * **2** - GUID (MD5-Hash)
   * **3** - Token bereits im Cache (true/false)
   * **4** - Gerätetyp
   * **5** - AccessEnabler-Client-Typ
   * **6** - Betriebssystemtyp

* Für trackingEventType-`TRACKING_AUTHORIZATION:`
   * **0** - Ob die Token-Anfrage erfolgreich war (true/false) und ob sie erfolgreich war:
   * **1** - MVPD ID
   * **2** - GUID (MD5-Hash)
   * **3** - Token bereits im Cache (true/false)
   * **4** - Fehler
   * **5** - Details
   * **6** - Gerätetyp
   * **7** - AccessEnabler-Client-Typ
   * **8** - Betriebssystemtyp
* Für trackingEventType-`TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID der aktuell ausgewählten MVPD
   * **1** - Gerätetyp
   * **2** - AccessEnabler-Client-Typ
   * **3** - Betriebssystemtyp

</br>
