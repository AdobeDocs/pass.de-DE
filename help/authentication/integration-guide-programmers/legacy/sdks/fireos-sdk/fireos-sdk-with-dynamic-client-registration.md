---
title: Amazon FireOS SDK mit dynamischer Client-Registrierung
description: Amazon FireOS SDK mit dynamischer Client-Registrierung
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 0%

---


# (Legacy) Amazon FireOS SDK mit dynamischer Client-Registrierung {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>

## <span id=""></span>Einführung {#Intro}

FireOS AccessEnabler SDK for FireTV wurde geändert, um die Authentifizierung ohne Verwendung von Sitzungs-Cookies zu ermöglichen. Da immer mehr Browser den Zugriff auf Cookies einschränken, wurde eine andere Methode benötigt, um die Authentifizierung zu ermöglichen.

**FireOS SDK 3.0.4** ersetzt den aktuellen Mechanismus zur App-Registrierung basierend auf der signierten Anforderer-ID und der Authentifizierung über Sitzungs-Cookies durch [Übersicht über die Dynamic Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


## API-Änderungen {#API}

### Factory.getInstance

**Beschreibung:** instanziiert das Access Enabler-Objekt. Pro Anwendungsinstanz sollte eine einzige Access Enabler-Instanz vorhanden sein.

| API-Aufruf: Konstruktor |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException |

**Verfügbarkeit:** v3.0+

**Parameter:**

- *appContext*: Anwendungskontext von Android
- *softwareStatement*: Wert, der vom TVE-Dashboard abgerufen wird oder *null*, wenn „software\_statement“ in strings.xml festgelegt ist
- *redirectUrl* : Für FireTV-Implementierungen sollte dieser Parameter null sein. Alle Einstellungen für dieses Attribut werden ignoriert.

**Hinweise**

- Ungültige Software-Anweisung führt dazu, dass die Anwendung AccessEnabler nicht initialisiert oder die Anwendung für die Authentifizierung und Autorisierung von Adobe Pass registriert
- Der Parameter redirectUrl für FireTV wird von der SDK auf adobepass://android.app festgelegt, da die Authentifizierung von einer eindeutigen AccessEnabler-Instanz verarbeitet wird.

### setRequestor

**Beschreibung:** Legt die Identität des Kanals fest. Jedem Kanal wird bei der Registrierung bei Adobe für das Adobe Pass-Authentifizierungssystem eine eindeutige ID zugewiesen. Bei SSO- und Remote-Token kann sich der Authentifizierungsstatus ändern, wenn sich die Anwendung im Hintergrund befindet. setRequestor kann erneut aufgerufen werden, wenn die Anwendung in den Vordergrund gebracht wird, um mit dem Systemstatus zu synchronisieren (rufen Sie ein Remote-Token ab, wenn SSO aktiviert ist, oder löschen Sie das lokale Token, wenn eine Abmeldung zwischenzeitlich stattgefunden hat).

Die Antwort des Servers enthält eine Liste der MVPDs zusammen mit Konfigurationsinformationen, die an die Identität des Kanals angehängt sind. Die Server-Antwort wird intern vom Access Enabler-Code verwendet. Nur der Status des Vorgangs (d. h. ERFOLG/FEHLGESCHLAGEN) wird Ihrer Anwendung über den Callback setRequestorComplete() angezeigt.

Wenn der Parameter *urls* nicht verwendet wird, zielt der resultierende Netzwerkaufruf auf die Standard-Service-Provider-URL ab: die Produktionsumgebung der Adobe-Version.

Wenn ein Wert für den Parameter *urls* angegeben wird, zielt der resultierende Netzwerkaufruf auf alle URLs ab, die im Parameter *urls* angegeben sind. Alle Konfigurationsanfragen werden gleichzeitig in separaten Threads ausgelöst. Bei der Kompilierung der Liste der MVPDs hat die erste Antwort Vorrang. Für jede MVPD in der Liste speichert Access Enabler die URL des zugehörigen Dienstleisters. Alle nachfolgenden Berechtigungsanfragen werden an die URL weitergeleitet, die mit dem Dienstleister verknüpft ist, der während der Konfigurationsphase mit dem Ziel-MVPD gepaart wurde.

| API-Aufruf: Anfordererkonfiguration |
| --- |
| public void setRequestor(String RequestorId) |

**Verfügbarkeit:** v3.0+

| API-Aufruf: Anfordererkonfiguration |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Verfügbarkeit:** v3.0+

**Parameter:**

- *RequestorID*: Die eindeutige ID, die dem Kanal zugeordnet ist. Übergeben Sie die eindeutige ID, die Adobe Ihrer Site zugewiesen hat, wenn Sie sich zum ersten Mal beim Adobe Pass-Authentifizierungs-Service registrieren.
- *urls*: Optionaler Parameter; standardmäßig wird der Adobe-Dienstleister verwendet (http://sp.auth.adobe.com/). Mit diesem Array können Sie Endpunkte für von Adobe bereitgestellte Authentifizierungs- und Autorisierungsdienste angeben (verschiedene Instanzen können zu Debugging-Zwecken verwendet werden). Damit können Sie mehrere Instanzen des Authentifizierungs-Service von Adobe Pass angeben. Dabei besteht die MVPD-Liste aus den Endpunkten aller Dienstleister. Jede MVPD ist mit dem schnellsten Dienstleister verknüpft, d. h. dem Anbieter, der zuerst geantwortet hat und diese MVPD unterstützt.

Veraltet:

- *signedRequestorID*: Eine Kopie der Anforderer-ID, die mit Ihrem privaten Schlüssel digital signiert ist. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Ausgelöste Callbacks:** `setRequestorComplete()`

</br>

### Abmelden

**Beschreibung:** Verwenden Sie diese Methode, um den Abmeldefluss zu initiieren. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Umleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Daher wird bei diesem Fluss ein ChromeCustomTab-Fenster geöffnet, in dem die Abmeldung ausgeführt wird.

| API-Aufruf: Initiieren des Abmeldevorgangs |
| --- |
| public void logout() |

**Verfügbarkeit:** v3.0+

**Parameter:** none

**Ausgelöste Callbacks:** `setAuthenticationStatus()`

## Implementierungsfluss für Programmierer {#Progr}

### **1. Registrierungsantrag**

1. Software\_statement von Adobe Pass abrufen ( TVE Dashboard )
1. Es gibt zwei Optionen, um diese Werte an Adobe Pass SDK zu übergeben:
   - Fügen Sie in strings.xml hinzu:

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - AccessEnabler.getInstance(appContext,softwareStatement, null) aufrufen



### **2. Programm konfigurieren**

- a. setRequestor(Requestor\_id)

  SDK führt die folgenden Vorgänge aus:

   - Registrierungsanwendung: Mit **Software\_Statement** erhält die SDK eine **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. Diese Informationen werden im internen Speicher der Anwendung gespeichert.
   - Rufen Sie ein **access\_token** mithilfe von client\_id, client\_secret und grant\_type=„client\_credentials“ ab. Dieses Zugriffs-Token wird bei jedem Aufruf von SDK an Adobe Pass-Server verwendet.

| Token-Fehlerantworten : |  |  |
|--- | --- | --- |
| HTTP 400 (Fehlerhafte Anfrage) | {„error“: „invalid\_request“} | Bei der Anfrage fehlt ein erforderlicher Parameter, sie enthält einen nicht unterstützten Parameterwert (außer Gewährungstyp), sie wiederholt einen Parameter, sie enthält mehrere Anmeldeinformationen, sie verwendet mehr als einen Mechanismus zur Authentifizierung des Clients oder sie ist anderweitig fehlerhaft. |
| HTTP 400 (Fehlerhafte Anfrage) | {„error“: „invalid\_client“} | Die Client-Authentifizierung ist fehlgeschlagen, da der Client unbekannt war. Die SDK *MUSS* erneut beim Autorisierungsserver registriert werden. |
| HTTP 400 (Fehlerhafte Anfrage) | {„error“: „unauthorized\_client“} | Der authentifizierte Client ist nicht berechtigt, diesen Autorisierungs-Gewährungstyp zu verwenden. |

- Wenn eine MVPD eine passive Authentifizierung erfordert, wird eine WebView geöffnet, um sie passiv mit dieser MVPD auszuführen, und nach Abschluss geschlossen

- b. checkAuthentication()

   - *true* : zur Autorisierung wechseln
   - *false* : Wechseln Sie zu MVPD auswählen

- c. getAuthentication : Die SDK nimmt **access_token) in** auf

   - mvpd gespeichert : Gehen Sie zu setSelectedProvider(mvpd\_id)
   - mvpd nicht ausgewählt : displayProviderDialog
   - mvpd ausgewählt : Gehen Sie zu setSelectedProvider(mvpd\_id)

- d. setSelectedProvider

   - Die Authentifizierungs-URL mvpd\_id wird in ChromeCustomTabs geladen
   - Anmeldung erfolgreich: delegate.setAuthenticationStatus ( SUCCESS )
   - Anmeldung abgebrochen : MVPD-Auswahl zurücksetzen
   - Das URL-Schema wird als &quot;adobepass://android.app&quot; festgelegt, um zu erfassen, wann die Authentifizierung abgeschlossen ist

- e. get/checkAuthorization : SDK fügt den Header **access\_token** als Autorisierung hinzu: Bearer **access\_token**

- Wenn die Autorisierung erfolgreich ist, wird ein Aufruf zum Abrufen des Medien-Tokens durchgeführt

- F. Abmelden :

   - SDK löscht ein gültiges Token für den aktuellen Anforderer (Authentifizierungen, die von anderen Programmen und nicht über SSO erhalten wurden, bleiben gültig)
   - SDK öffnet benutzerdefinierte Chrome-Registerkarten, um den Abmeldeendpunkt mvpd\_id zu erreichen. Nach Abschluss des Vorgangs werden die benutzerdefinierten Registerkarten von Chrome geschlossen
   - Das URL-Schema wird als &quot;adobepass://logout&quot; festgelegt, um den Zeitpunkt des Abmeldevorgangs zu erfassen
   - Bei der Abmeldung werden SendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) und ein Callback : setAuthenticationStatus(0,„Logout„) Trigger



**Hinweis** Da jeder Aufruf ein **access_token** erfordert, werden die unten aufgeführten möglichen Fehlercodes in der SDK verarbeitet.

| Fehlerantworten |  |  |
|--- | --- | --- |
| Invalid_request | 400 | Die Anfrage ist fehlerhaft. Die SDK sollte keine Aufrufe mehr an den Server ausführen. |
| invalid_client | 403 | Die Client-ID darf keine Anfragen mehr ausführen. Das SDK MUSS die Client-Registrierung erneut durchführen. |
| access_denied | 401 | Das Zugriffstoken ist ungültig. Das SDK MUSS ein neues Zugriffs-Token anfordern. |
