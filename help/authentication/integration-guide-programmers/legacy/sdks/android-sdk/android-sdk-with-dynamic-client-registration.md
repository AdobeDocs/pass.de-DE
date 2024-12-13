---
title: Android SDK mit dynamischer Client-Registrierung
description: Android SDK mit dynamischer Client-Registrierung
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---

# (Legacy) Android SDK mit dynamischer Client-Registrierung {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung {#Intro}

Android AccessEnabler SDK für Android wurde geändert, um die Authentifizierung ohne Sitzungs-Cookies zu aktivieren. Da immer mehr Browser den Zugriff auf Cookies einschränken, muss eine andere Methode verwendet werden, um die Authentifizierung zu ermöglichen.

Bei Android beschränkt die Verwendung benutzerdefinierter Chrome-Registerkarten den Zugriff auf Cookies aus anderen Programmen.

>**Android SDK 3.0.0** führt ein:

- Die dynamische Client-Registrierung ersetzt den aktuellen Mechanismus zur App-Registrierung basierend auf der Authentifizierung mit signierter Anforderer-ID und Sitzungs-Cookie .
- Benutzerdefinierte Chrome-Registerkarten für Authentifizierungsabläufe

>[!NOTE]
>
>Bei älteren Android-Versionen ohne Unterstützung für benutzerdefinierte Chrome-Registerkarten wird die WebView-Authentifizierung ähnlich wie in älteren AccessEnabler SDK-Versionen verwendet.


## Dynamische Client-Registrierung {#DCR}

Android SDK v3.0+ verwendet das Verfahren Dynamic Client Registration , wie in [Übersicht über die dynamische Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) definiert.


## Funktionsdemo {#Demo}

Sehen Sie sich [dieses Webinar](https://my.adobeconnect.com/pzkp8ujrigg1/) an, das mehr Kontext der Funktion bietet und eine Demo dazu enthält, wie Sie die Software-Anweisungen mithilfe des TVE-Dashboards verwalten und wie Sie die generierten Anweisungen mit einer Demoanwendung testen können, die von Adobe im Rahmen von Android SDK bereitgestellt wird.

## API-Änderungen {#API}


### Factory.getInstance

**Beschreibung:** instanziiert das Access Enabler-Objekt. Pro Anwendungsinstanz sollte eine einzige Access Enabler-Instanz vorhanden sein.

| API-Aufruf: Konstruktor |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException |


**Verfügbarkeit:** v3.0+

**Parameter:**

- *appContext*: Anwendungskontext von Android
- softwareStatement: Wert, der vom TVE-Dashboard abgerufen wurde oder *null*, wenn „software\_statement“ in strings.xml festgelegt ist
- redirectUrl : eine eindeutige URL, eine der Domains in umgekehrter Reihenfolge, die explizit im TVE-Dashboard hinzugefügt wurde, oder *null* wenn „redirect\_uri“ in strings.xml festgelegt ist

Hinweis : Ungültige softwareStatement oder redirectUrl führt dazu, dass die Anwendung AccessEnabler nicht initialisiert oder die Anwendung für die Authentifizierung und Autorisierung von Adobe Pass registriert
</br>
Hinweis : Der Parameter „redirectUrl“ oder „redirect\_uri“ in „strings.xml“ sollte der Wert der im TVE-Dashboard hinzugefügten Domain für die Anwendung in umgekehrter Reihenfolge sein ( z. B. : Für die im TVE-Dashboard hinzugefügte Domain „adobe.com“ sollte die redirectUrl „com.adobe“ sein.


### setRequestor

**Beschreibung:** Legt die Identität des Kanals fest. Jedem Kanal wird bei der Registrierung bei der Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gebracht wird, um mit dem Systemstatus zu synchronisieren (rufen Sie ein Remote-Token ab, wenn SSO aktiviert ist, oder löschen Sie das lokale Token, wenn eine Abmeldung zwischenzeitlich stattgefunden hat).

Die Antwort des Servers enthält eine Liste der MVPDs zusammen mit Konfigurationsinformationen, die an die Identität des Kanals angehängt sind. Die Server-Antwort wird intern vom Access Enabler-Code verwendet. Nur der Status des Vorgangs (d. h. ERFOLG/FEHLGESCHLAGEN) wird Ihrer Anwendung über den Callback setRequestorComplete() angezeigt.

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

- *RequestorID*: Die eindeutige ID, die dem Kanal zugeordnet ist. Übergeben Sie die eindeutige ID, die von Adobe an Ihre Site zugewiesen wurde, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungsdienst registriert haben.
- *urls*: Optionaler Parameter; standardmäßig wird der Adobe-Dienstleister verwendet [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für Authentifizierungs- und Autorisierungsdienste angeben, die von Adobe bereitgestellt werden (verschiedene Instanzen können zu Debugging-Zwecken verwendet werden). Damit können Sie mehrere Instanzen des Authentifizierungs-Service von Adobe Pass angeben. Dabei besteht die MVPD-Liste aus den Endpunkten aller Dienstleister. Jede MVPD ist mit dem schnellsten Dienstleister verknüpft, d. h. dem Anbieter, der zuerst geantwortet hat und diese MVPD unterstützt.

Veraltet:

- *signedRequestorID*: Eine Kopie der Anforderer-ID, die mit Ihrem privaten Schlüssel digital signiert ist. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Ausgelöste Callbacks:** `setRequestorComplete()`

### Abmelden

**Beschreibung:** Verwenden Sie diese Methode, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Umleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Daher wird bei diesem Fluss ein ChromeCustomTab-Fenster geöffnet, in dem die Abmeldung ausgeführt wird.

| API-Aufruf: Initiieren des Abmeldevorgangs |
| --- |
| public void logout() |

**Verfügbarkeit:** v3.0+

**Parameter:** none

**Ausgelöste Callbacks:** `setAuthenticationStatus()`
</br></br>

## Implementierungsfluss für Programmierer {#Progr}

### **1. Registrierungsantrag**

a. Abrufen von software\_statement und redirect\_uri von Adobe Pass ( TVE Dashboard )

b. Es gibt zwei Optionen, um diese Werte an Adobe Pass SDK zu übergeben:

Fügen Sie in strings.xml hinzu:

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

Call AccessEnabler.getInstance(appContext,softwareStatement,
redirectUrl)


### 2. Programm konfigurieren

a. setRequestor(Requestor\_id)

SDK führt die folgenden Vorgänge aus:

- Registrierungsanwendung: Mit **SOFTWARE\_STATEMENT** erhält SDK eine **CLIENT\_ID, CLIENT\_SECRET, client\_id\_issued\_at, REDIRECT\_URIS, GRANT\_TYPES**. Diese Informationen werden im internen Speicher der Anwendung gespeichert.

- Rufen Sie ein **access\_token** mithilfe von client\_id, client\_secret und grant\_type=„client\_credentials“ ab. Dieses Zugriffs-Token wird bei jedem Aufruf von SDK an Adobe Pass-Server verwendet

**Token-Fehlerantworten :**

| Fehlerantworten | | |
| --- | --- | --- |
| HTTP 400 (Fehlerhafte Anfrage) | {„error“: „invalid\_request“} | Bei der Anfrage fehlt ein erforderlicher Parameter, sie enthält einen nicht unterstützten Parameterwert (außer Gewährungstyp), sie wiederholt einen Parameter, sie enthält mehrere Anmeldeinformationen, sie verwendet mehr als einen Mechanismus zur Authentifizierung des Clients oder sie ist anderweitig fehlerhaft. |
| HTTP 400 (Fehlerhafte Anfrage) | {„error“: „invalid\_client“} | Die Client-Authentifizierung ist fehlgeschlagen, da der Client unbekannt war. SDK MUSS sich erneut beim Autorisierungsserver registrieren. |
| HTTP 400 (Fehlerhafte Anfrage) | {„error“: „unauthorized\_client“} | Der authentifizierte Client ist nicht berechtigt, diesen Autorisierungs-Gewährungstyp zu verwenden. |

- Wenn eine MVPD eine passive Authentifizierung erfordert, wird eine benutzerdefinierte Chrome-Registerkarte geöffnet, die passiv mit dieser MVPD ausgeführt wird und nach Abschluss geschlossen wird

b. checkAuthentication()

- true : Wechseln zur Autorisierung
- false : Wechseln Sie zu MVPD auswählen

c. getAuthentication : SDK nimmt **access_token) in** auf

- mvpd gespeichert : Gehen Sie zu setSelectedProvider(mvpd_id)
- mvpd nicht ausgewählt : displayProviderDialog
- mvpd ausgewählt : Gehen Sie zu setSelectedProvider(mvpd_id)

d. setSelectedProvider

- Die Authentifizierungs-URL mvpd\_id wird in ChromeCustomTabs geladen
- Anmeldung erfolgreich: delegate.setAuthenticationStatus ( SUCCESS )
- Anmeldung abgebrochen : MVPD-Auswahl zurücksetzen
- Das URL-Schema wird als &quot;adobepass://redirect_uri&quot; festgelegt, um zu erfassen, wann die Authentifizierung abgeschlossen ist

e. get/checkAuthorization : SDK fügt **access_token) in** Kopfzeile als Autorisierung ein: Bearer **access_token**

- Wenn die Autorisierung erfolgreich ist, wird ein Aufruf ausgeführt, um die
Medien-Token

F. Abmelden :

- SDK löscht ein gültiges Token für den aktuellen Anforderer (Authentifizierungen, die von anderen Programmen und nicht über SSO erhalten wurden, bleiben gültig)
- SDK öffnet benutzerdefinierte Chrome-Registerkarten, um den Abmeldeendpunkt mvpd_id zu erreichen. Nach Abschluss des Vorgangs werden die benutzerdefinierten Registerkarten von Chrome geschlossen
- Das URL-Schema wird als &quot;adobepass://logout&quot; festgelegt, um den Zeitpunkt des Abmeldevorgangs zu erfassen
- Bei der Abmeldung werden sendTrackingData(new Event(EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR) und ein Callback : setAuthenticationStatus(0,„Logout„) in den Trigger gestellt

**Hinweis** Da jeder Aufruf ein **access_token erfordert, werden** unten aufgeführten möglichen Fehlercodes in der SDK verarbeitet.


| Fehlerantworten | | |
| --- | ---|--- |
| Invalid_request | 400 | Die Anfrage ist fehlerhaft. Die SDK sollte keine Aufrufe mehr an den Server ausführen. |
| invalid_client | 403 | Die Client-ID darf keine Anfragen mehr ausführen. Das SDK MUSS die Client-Registrierung erneut durchführen. |
| access_denied | 401 | Das Zugriffstoken ist ungültig. Das SDK MUSS ein neues Zugriffs-Token anfordern. |
