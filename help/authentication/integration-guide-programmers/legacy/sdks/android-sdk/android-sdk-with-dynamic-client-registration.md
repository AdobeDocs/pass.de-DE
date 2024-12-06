---
title: Android SDK mit dynamischer Client-Registrierung
description: Android SDK mit dynamischer Client-Registrierung
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1278'
ht-degree: 0%

---

# Android SDK mit dynamischer Client-Registrierung {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#Intro}

Das Android AccessEnabler SDK für Android wurde geändert, um die Authentifizierung zu aktivieren, ohne Sitzungs-Cookies zu verwenden. Da immer mehr Browser den Zugriff auf Cookies einschränken, muss eine andere Methode verwendet werden, um die Authentifizierung zu ermöglichen.

Bei Android schränkt die Verwendung benutzerdefinierter Chrome-Registerkarten den Zugriff auf Cookies aus anderen Anwendungen ein.

>**Android SDK 3.0.0** führt Folgendes ein:

- Die dynamische Client-Registrierung ersetzt den aktuellen App-Registrierungsmechanismus basierend auf der signierten Anfrage-ID und der Authentifizierung des Sitzungs-Cookies.
- Benutzerdefinierte Chrome-Registerkarten für Authentifizierungsflüsse

>[!NOTE]
>
>Bei älteren Android-Versionen ohne Chrome Custom Tabs wird die WebView-Authentifizierung ähnlich wie bei älteren AccessEnabler SDK-Versionen verwendet.


## Dynamische Kundenregistrierung {#DCR}

Android SDK v3.0+ verwendet das in [Übersicht über die dynamische Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) definierte Verfahren zur dynamischen Client-Registrierung.


## Funktionsdemo {#Demo}

Sehen Sie sich [dieses Webinar](https://my.adobeconnect.com/pzkp8ujrigg1/) an, das mehr Kontext für die Funktion bietet und eine Demo dazu enthält, wie Sie die Softwareanweisungen mit dem TVE-Dashboard verwalten und die generierten mithilfe einer Demoanwendung testen können, die von Adobe als Teil des Android SDK bereitgestellt wird.

## API-Änderungen {#API}


### Factory.getInstance

**Beschreibung:** Instanziiert das Objekt Access Enabler . Pro Anwendungsinstanz sollte eine einzige Access Enabler -Instanz vorhanden sein.

| API-Aufruf: Konstruktor |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        gibt AccessEnablerException aus |


**Verfügbarkeit:** v3.0+

**Parameter:**

- *appContext*: Android-Anwendungskontext
- softwareStatement: Wert, der vom TVE Dashboard abgerufen wird, oder *null*, wenn &quot;software\_statement&quot;in strings.xml festgelegt ist
- redirectUrl : eindeutige URL, eine der Domänen in umgekehrter Reihenfolge, die explizit im TVE-Dashboard hinzugefügt wurde, oder *null* , wenn &quot;redirect\_uri&quot;in strings.xml festgelegt ist

Hinweis : Eine ungültige SoftwareStatement oder redirectUrl führt dazu, dass die Anwendung AccessEnabler nicht initialisiert oder die Anwendung für Adobe Pass-Authentifizierung und -Autorisierung registriert
</br>
Hinweis : Der Parameter redirectUrl oder redirect\_uri in strings.xml sollte der Wert der Domäne sein, die im TVE Dashboard für die Anwendung in umgekehrter Reihenfolge hinzugefügt wird ( z. B.: Für die Domäne &quot;adobe.com&quot;, die im TVE Dashboard hinzugefügt wird, sollte die redirectUrl &quot;com.adobe&quot;lauten.


### setRequest

**Beschreibung:** Legt die Identität des Kanals fest. Jedem Kanal wird bei der Registrierung mit Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gestellt wird, um mit dem Systemstatus zu synchronisieren (Abrufen eines Remote-Tokens, wenn SSO aktiviert ist, oder Löschen des lokalen Tokens, wenn in der Zwischenzeit ein Abmelden stattgefunden hat).

Die Server-Antwort enthält eine Liste von MVPDs sowie einige Konfigurationsinformationen, die an die Identität des Kanals angehängt sind. Die Server-Antwort wird intern vom Access Enabler-Code verwendet. Nur der Status des Vorgangs (d. h. SUCCESS/FAIL) wird Ihrer Anwendung über den Rückruf setRequestorComplete() angezeigt.

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

- *requestorID*: Die eindeutige ID, die dem Kanal zugeordnet ist. Übergeben Sie die eindeutige ID, die von Adobe zugewiesen wurde, an Ihre Site, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
- *urls*: Optionaler Parameter. Standardmäßig wird der Adobe-Dienstanbieter [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/) verwendet. Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zum Debugging verwendet werden). Sie können dies verwenden, um mehrere Adobe Pass Authentication Service Provider-Instanzen anzugeben. Dabei setzt sich die MVPD-Liste aus den Endpunkten aller Dienstleister zusammen. Jeder MVPD ist mit dem schnellsten Dienstleister verbunden, d. h. dem Provider, der zuerst reagiert hat und dieser MVPD unterstützt.

Veraltet:

- *signedRequestorID*: Eine Kopie der Anforderer-ID, die digital mit Ihrem privaten Schlüssel signiert ist. 2.<!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->

**Ausgelöste Rückrufe:** `setRequestorComplete()`

### Abmelden

**Beschreibung:** Verwenden Sie diese Methode, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Weiterleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Daher öffnet dieser Fluss ein ChromeCustomTab-Fenster, um die Abmeldung auszuführen.

| API-Aufruf: Logout-Fluss initiieren |
| --- |
| public void logout() |

**Verfügbarkeit:** v3.0+

**Parameter:** None

**Ausgelöste Rückrufe:** `setAuthenticationStatus()`
</br></br>

## Programmierer-Implementierungsfluss {#Progr}

### **1. Registrierungsanwendung**

a. Rufen Sie software\_statement ab und leiten Sie\_uri über Adobe Pass ( TVE-Dashboard ) weiter.

b. Es gibt zwei Optionen, um diese Werte an das Adobe Pass SDK zu übergeben:

Fügen Sie in strings.xml Folgendes hinzu:

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

Rufen Sie AccessEnabler.getInstance(appContext,softwareStatement,
redirectUrl)


### 2. Anwendung konfigurieren

a. setRequestor(requestor\_id)

SDK führt die folgenden Vorgänge aus:

- Registrierungsanwendung: Mit **software\_statement** erhält das SDK eine **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. Diese Informationen werden im internen Speicher der Anwendung gespeichert.

- Rufen Sie einen **access\_token** mit client\_id, client\_secret und grant\_type=&quot;client\_credentials&quot; ab. Dieses access\_token wird für jeden Aufruf verwendet, den das SDK an Adobe Pass-Server sendet.

**Token-Fehlerantworten :**

| Fehlerantworten | | |
| --- | --- | --- |
| HTTP 400 (ungültige Anforderung) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Der Anfrage fehlt ein erforderlicher Parameter, enthält einen nicht unterstützten Parameterwert (außer dem Grant-Typ), wiederholt einen Parameter, enthält mehrere Anmeldeinformationen, verwendet mehr als einen Mechanismus zur Authentifizierung des Clients oder ist anderweitig fehlerhaft. |
| HTTP 400 (ungültige Anforderung) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Die Client-Authentifizierung schlug fehl, da der Client unbekannt war. Das SDK MUSS sich erneut beim Autorisierungsserver registrieren. |
| HTTP 400 (ungültige Anforderung) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | Der authentifizierte Client ist nicht berechtigt, diesen Autorisierungstyp zu verwenden. |

- Wenn ein MVPD eine passive Authentifizierung erfordert, wird eine benutzerdefinierte Registerkarte für Chrome geöffnet, die passiv mit diesem MVPD ausgeführt werden kann, und nach Abschluss geschlossen.

b. checkAuthentication()

- true : zur Autorisierung wechseln
- false : Wechseln Sie zu Select MVPD

c. getAuthentication : Das SDK nimmt **access_token** in die Aufrufparameter auf

- mvpd merkte : go to setSelectedProvider(mvpd_id)
- mvpd not selected : displayProviderDialog
- mvpd selected : go to setSelectedProvider(mvpd_id)

d. setSelectedProvider

- mvpd\_id authentication url wird in ChromeCustomTabs geladen
- Anmeldung erfolgreich : delegate.setAuthenticationStatus ( SUCCESS )
- Anmeldung abgebrochen : MVPD-Auswahl zurücksetzen
- Das URL-Schema wird als &quot;adobepass://redirect_uri&quot;festgelegt, um zu erfassen, wenn die Authentifizierung abgeschlossen ist.

e. get/checkAuthorization : Das SDK enthält **access_token** in der Kopfzeile als Authorization: Bearer **access_token**

- Wenn die Autorisierung erfolgreich ist, wird ein Aufruf zum Erhalt der
Medien-Token

f. Abmeldung :

- SDK löscht gültiges Token für den aktuellen Anforderer (Authentifizierungen, die von anderen Anwendungen und nicht über SSO abgerufen wurden, bleiben gültig)
- Das SDK öffnet benutzerdefinierte Chrome-Registerkarten, um den mvpd_id-Abmelde-Endpunkt zu erreichen. Nach Abschluss werden die benutzerdefinierten Registerkarten von Chrome geschlossen
- Das URL-Schema wird als &quot;adobepass://logout&quot;festgelegt, um den Zeitpunkt zu erfassen, zu dem die Abmeldung abgeschlossen ist.
- Die Abmeldung Trigger einen sendTrackingData(new Event(EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR) und einen callback: setAuthenticationStatus(0,&quot;Logout&quot;)

**Hinweis:** Da jeder Aufruf ein **access_token erfordert, werden** mögliche Fehlercodes unten im SDK verarbeitet.


| Fehlerantworten | | |
| --- | ---|--- |
| invalid_request | 400 | Die Anfrage ist fehlerhaft. Das SDK sollte Aufrufe an den Server nicht mehr durchführen. |
| invalid_client | 403 | Die Client-ID darf keine Anfragen mehr ausführen. Das SDK MUSS die Kundenregistrierung erneut durchführen. |
| access_denied | 401 | Der access_token ist ungültig. Das SDK MUSS ein neues access_token anfordern. |
