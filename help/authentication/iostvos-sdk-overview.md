---
title: Übersicht über das iOS/tvOS-SDK
description: Übersicht über das iOS/tvOS-SDK
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '3704'
ht-degree: 0%

---

# Übersicht über das iOS/tvOS-SDK {#iostvos-sdk-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


</br>


## Einführung {#intro}

iOS AccessEnabler ist eine Objective-C-iOS-/tvOS-Bibliothek, die es Apps ermöglicht, die Adobe Pass-Authentifizierung für die Berechtigungsdienste von TV Anywhere zu verwenden. Die Implementierung besteht aus der *AccessEnabler* -Schnittstelle, die die Berechtigungs-API und die *entityDelegate* und *[EntitlementStatus](#ios%20entitlement%20status)* -Protokolle, die die von der Bibliothek Trigger Rückrufe beschreiben. Die Schnittstelle wird zusammen mit dem Protokoll unter einem gemeinsamen Namen genannt: die AccessEnabler-Bibliothek.

## Anforderungen für iOS und tvOS {#reqs}

Aktuelle technische Anforderungen bezüglich der iOS- und tvOS-Plattform und der Adobe Pass-Authentifizierung finden Sie unter [Anforderungen an Plattform/Gerät/Tool](#ios)und lesen Sie die Versionshinweise, die im SDK-Download enthalten sind. Im Rest dieser Seite finden Sie Abschnitte, in denen Sie Änderungen feststellen, die für bestimmte SDK-Versionen und höher gelten. Beispielsweise ist Folgendes ein legitimer Hinweis zum SDK 1.7.5:

## Grundlegendes zu nativen Client-Workflows {#flows}

Native Client-Workflows sind in der Regel mit denen von browserbasierten Adobe Pass-Authentifizierungs-Clients identisch oder sehr ähnlich. Es gibt jedoch einige Ausnahmen, wie unten beschrieben.

- [Nachinitialisierungs-Workflow](#post-init)
- [Allgemeiner Workflow für die anfängliche Authentifizierung](#generic)
- [Workflow abmelden](#logout)


### Nachinitialisierungs-Workflow {#post-init}

Alle vom AccessEnabler unterstützten Berechtigungs-Workflows setzen voraus, dass Sie zuvor [`setRequestor()`](#setReq) , um Ihre Identität festzustellen. Sie führen diesen Aufruf aus, um Ihre Anforderer-ID nur einmal bereitzustellen, normalerweise während der Initialisierungs-/Setup-Phase Ihrer Anwendung.


Mit einem nativen iOS-Client nach dem ersten Aufruf von [`setRequestor()`](#setReq), können Sie entscheiden, wie Sie vorgehen:

- Sie können sofort mit Berechtigungsaufrufen beginnen und diese bei Bedarf still in die Warteschlange stellen.

- Sie können eine Bestätigung des Erfolgs/Misserfolgs von [`setRequestor()`](#setReq) durch Umsetzung der [`setRequestorComplete()`](#setReqComplete) Callback.

- Sie können beide der oben genannten Schritte ausführen.

Sie können entweder Ihre App auf die Benachrichtigung über den Erfolg von warten lassen [`setRequestor()`](#setReq) oder auf den Aufruf-Warteschlangenmechanismus von AccessEnabler zurückgreifen. Da alle nachfolgenden Autorisierungs- und Authentifizierungsanfragen die Anforderer-ID und die zugehörigen Konfigurationsinformationen benötigen, muss die [`setRequestor()`](#setReq) -Methode blockiert effektiv alle Authentifizierungs- und Autorisierungs-API-Aufrufe, bis die Initialisierung abgeschlossen ist.



### Allgemeiner Workflow für die anfängliche Authentifizierung {#generic}

Dieser Workflow dient der Anmeldung eines Benutzers mit seinem MVPD. Nach erfolgreicher Anmeldung gibt der Backend-Server dem Benutzer ein Authentifizierungstoken aus. Während die Authentifizierung in der Regel im Rahmen des Autorisierungsprozesses erfolgt, wird im Folgenden beschrieben, wie die Authentifizierung isoliert funktioniert, und es sind keine Autorisierungsschritte enthalten.

Beachten Sie, dass sich dieser Workflow für native Clients vom typischen browserbasierten Authentifizierungs-Workflow unterscheidet, die Schritte 1 bis 5 jedoch für native Clients und browserbasierte Clients gleich sind.

1. Ihre Anwendung initiiert den Authentifizierungs-Workflow mit einem Aufruf von AccessEnabler&#39;s `getAuthentication() `API-Methode, die nach einem gültigen zwischengespeicherten Authentifizierungstoken sucht.
1. Wenn der Benutzer derzeit authentifiziert ist, ruft AccessEnabler Ihre [`setAuthenticationStatus()`](#setAuthNStatus) Callback-Funktion, die einen Authentifizierungsstatus übergibt, der auf Erfolg hinweist, und den Fluss beendet.
1. Wenn der Benutzer derzeit nicht authentifiziert ist, setzt der AccessEnabler den Authentifizierungsfluss fort, indem er bestimmt, ob der letzte Authentifizierungsversuch des Benutzers mit einem bestimmten MVPD erfolgreich war. Wenn eine MVPD-ID zwischengespeichert wird UND die `canAuthenticate` Flag ist wahr ODER ein MVPD wurde mit [`setSelectedProvider()`](#setSelProv), wird der Benutzer nicht mit dem MVPD-Auswahldialogfeld aufgefordert. Der Authentifizierungsfluss wird mit dem zwischengespeicherten Wert des MVPD fortgesetzt (d. h. mit dem MVPD, der bei der letzten erfolgreichen Authentifizierung verwendet wurde). Ein Netzwerkaufruf erfolgt an den Backend-Server und der Benutzer wird zur MVPD-Anmeldeseite weitergeleitet (Schritt 6 unten).
1. Wenn keine MVPD-ID zwischengespeichert wurde UND kein MVPD mit [`setSelectedProvider()`](#setSelProv) ODER `canAuthenticate` Flag auf &quot;false&quot;festgelegt ist, wird die [`displayProviderDialog()`](#dispProvDialog) Callback wird aufgerufen. Dieser Rückruf weist Ihre Anwendung an, die Benutzeroberfläche zu erstellen, über die dem Benutzer eine Liste von MVPDs zur Auswahl angezeigt wird. Es wird ein Array von MVPD-Objekten bereitgestellt, die die erforderlichen Informationen zum Erstellen des MVPD-Selektors enthalten. Jedes MVPD-Objekt beschreibt eine MVPD-Entität und enthält Informationen wie die Kennung des MVPD (z. B. XFINITY, AT\&amp;T usw.) und die URL, unter der das MVPD-Logo zu finden ist.
1. Nachdem ein bestimmter MVPD ausgewählt wurde, muss Ihre Anwendung den AccessEnabler über die Wahl des Benutzers informieren. Sobald der Benutzer den gewünschten MVPD auswählt, informieren Sie den AccessEnabler über einen Aufruf an die [`setSelectedProvider()`](#setSelProv) -Methode.
1. Der iOS AccessEnabler ruft Ihre `navigateToUrl:` Callback oder `navigateToUrl:useSVC:` Callback , um den Benutzer zur MVPD-Anmeldeseite umzuleiten. Durch Auslösen einer der beiden fordert der AccessEnabler Ihre Anwendung auf, eine `UIWebView/WKWebView or SFSafariViewController` und zum Laden der im Callback bereitgestellten URL `url` -Parameter. Dies ist die URL des Authentifizierungsendpunkts auf dem Backend-Server. Für tvOS AccessEnabler muss die [status()](#status_callback_implementation) Callback wird mit einer `statusDictionary` -Parameter und die Abfrage für die zweite Bildschirmauthentifizierung wird sofort gestartet. Die `statusDictionary` enthält die `registration code` , die für die zweite Bildschirmauthentifizierung verwendet werden muss.
1. Im Fall von iOS AccessEnabler landet der Benutzer auf der Anmeldeseite des MVPD, um seine Anmeldeinformationen über die Anwendung einzugeben. `UIWebView/WKWebView or SFSafariViewController `Controller. Beachten Sie, dass während dieser Übertragung mehrere Umleitungsvorgänge stattfinden und Ihre Anwendung die URLs überwachen muss, die vom Controller während der verschiedenen Umleitungsvorgänge geladen werden.
1. Wenn der iOS AccessEnabler `UIWebView/WKWebView or SFSafariViewController` Controller lädt eine bestimmte benutzerdefinierte URL, die Ihre Anwendung schließen muss, und ruft den AccessEnabler `handleExternalURL:url `API-Methode. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Sie darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Authentifizierungsvorgang abgeschlossen ist und dass das Schließen der `UIWebView/WKWebView or SFSafariViewController` Controller. Falls Ihre Anwendung eine `SFSafariViewController `Controller, für den die spezifische benutzerdefinierte URL definiert wird, durch die `application's custom scheme` (z. B.: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die Variable `ADOBEPASS_REDIRECT_URL` Konstante (d. h. `adobepass://ios.app`).
1. Sobald die Anwendung geschlossen wurde `UIWebView/WKWebView or SFSafariViewController` Controller und Aufrufe von AccessEnabler `handleExternalURL:url `API-Methode verwendet, ruft der AccessEnabler das Authentifizierungstoken vom Backend-Server ab und informiert Ihre Anwendung darüber, dass der Authentifizierungsfluss abgeschlossen ist. AccessEnabler ruft die [`setAuthenticationStatus()`](#setAuthNStatus) Callback mit Status-Code 1, der auf Erfolg hinweist. Wenn bei der Ausführung dieser Schritte ein Fehler auftritt, wird die [`setAuthenticationStatus()`](#setAuthNStatus) -Callback wird mit dem Status-Code 0 ausgelöst, der auf einen Authentifizierungsfehler sowie einen entsprechenden Fehlercode hinweist.


>[!WARNING]
>
> Während der Schritte, in denen der AccessEnabler die Kontrolle an Ihre App übergibt (d. h. wenn das Dialogfeld zur Anbieterauswahl angezeigt wird oder wenn UIWebView/WKWebView oder SFSafariViewController angezeigt wird), hat der Benutzer die Möglichkeit, den Authentifizierungsfluss abzubrechen. In diesen Situationen ist Ihre App dafür verantwortlich, den AccessEnabler über dieses Ereignis zu informieren und die [`setSelectedProvider()`](#setSelProv) API-Methode, bei der null als Parameter übergeben wird. Dadurch erhält AccessEnabler die Möglichkeit, den internen Status zu bereinigen und den Authentifizierungsfluss zurückzusetzen. Unter tvOS können Sie die Authentifizierungsabfrage mit derselben Methode abbrechen.


### Workflow abmelden {#logout}

Bei nativen Clients wird die Abmeldung ähnlich wie der oben beschriebene Authentifizierungsprozess durchgeführt.

1. Ihre Anwendung initiiert den Abmelde-Workflow mit einem Aufruf von AccessEnabler&#39;s `logout() `API-Methode. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Weiterleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Da dieser Fluss nicht mit einer einfachen HTTP-Anforderung abgeschlossen werden kann, die von der AccessEnabler-Bibliothek, einer `UIWebView/WKWebView or SFSafariViewController` -Controller muss instanziiert werden, damit HTTP-Weiterleitungsvorgänge folgen können.

1. Es wird ein dem Authentifizierungsfluss ähnliches Muster verwendet. Die iOS AccessEnabler-Trigger `navigateToUrl:` oder `navigateToUrl:useSVC:` , um eine `UIWebView/WKWebView or SFSafariViewController` und zum Laden der im Callback bereitgestellten URL `url` -Parameter. Dies ist die URL des Abmelde-Endpunkts auf dem Backend-Server. Für den tvOS AccessEnabler weder die `navigateToUrl:` oder `navigateToUrl:useSVC:` Callback wird aufgerufen.

1. Da mehrere Umleitungen durchgeführt werden, muss Ihre Anwendung die Aktivität der `UIWebView/WKWebView or SFSafariViewController `und erkennen Sie den Moment, in dem eine bestimmte benutzerdefinierte URL geladen wird. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht für den Controller vorgesehen ist, sie tatsächlich zu laden. Es darf nur von Ihrer Anwendung als Signal interpretiert werden, dass der Abmeldefluss abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung den Controller schließen und den AccessEnabler `handleExternalURL:url `API-Methode. Falls Ihre Anwendung eine `SFSafariViewController `Controller, für den die spezifische benutzerdefinierte URL definiert wird, durch die `application's custom scheme` (z. B.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Andernfalls wird diese spezifische benutzerdefinierte URL durch die Variable ` ADOBEPASS_REDIRECT_URL  `Konstante (d. h. `adobepass://ios.app`).

1. Am Ende ruft AccessEnabler die [`setAuthenticationStatus()`](#setAuthNStatus) Callback mit Status-Code 0, der auf den Erfolg des Abmeldeflusses hinweist.

Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss insofern, als der Benutzer nicht mit der `UIWebView/WKWebView or SFSafariViewController`  Controller in irgendeiner Weise. Daher empfiehlt Adobe, das Steuerelement während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.

## Token {#tokens}

- [Definitionen und Verwendung](#definitions)
- [Caching-Richtlinien](#caching)
- [Persistenz](#persistence)
- [Format](#format)
- [Gerätebindung](#device_binding)


### Definitionen und Verwendung {#definitions}

Die Berechtigungslösung für die Adobe Pass-Authentifizierung umfasst die Generierung bestimmter Datenteile (Token), die die Adobe Pass-Authentifizierung nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungsarbeitsabläufe generiert. Diese Token werden lokal auf dem iOS-Gerät des Clients gespeichert.



Token haben eine begrenzte Lebensdauer. Nach Ablauf der Gültigkeit müssen Token erneut ausgestellt werden, indem die Authentifizierungs- und/oder Autorisierungs-Workflows neu gestartet werden.



Während der Berechtigungs-Workflows werden drei Typen von Token ausgegeben:

- **Authentifizierungstoken:** Das Endergebnis des Workflows zur Benutzerauthentifizierung ist eine Authentifizierungs-GUID, die der AccessEnabler verwenden kann, um Autorisierungsabfragen im Namen des Benutzers durchzuführen. Diese Authentifizierungs-GUID weist einen zugehörigen TTL-Wert (Time-to-Live) auf, der sich von der Authentifizierungssitzung des Benutzers selbst unterscheiden kann. Ein Authentifizierungstoken wird durch Binden der Authentifizierungs-GUID an das Gerät generiert, das die Authentifizierungsanforderungen initiiert.
- **Autorisierungstoken:** Gewährt Zugriff auf eine bestimmte geschützte Ressource, die durch eine eindeutige resourceID identifiziert wird. Es handelt sich dabei um eine von der autorisierenden Partei ausgestellte Ermächtigung zusammen mit der ursprünglichen resourceID. Diese Informationen sind an das Gerät gebunden, das die Anfrage initiiert.
- **Kurzlebiges Medien-Token:** Der AccessEnabler gewährt Zugriff auf die Hosting-Anwendung für eine bestimmte Ressource, indem er ein kurzlebiges Medien-Token zurückgibt. Dieses Token wird basierend auf dem Autorisierungstoken generiert, das zuvor für diese bestimmte Ressource erworben wurde. Außerdem ist dieses Token nicht an das Gerät gebunden und die zugehörige Lebensdauer ist deutlich kürzer (Standard: 5 Minuten).

Nach erfolgreicher Authentifizierung und Autorisierung stellt die Adobe Pass-Authentifizierung Authentifizierungs-, Autorisierungs- und kurzlebige Medien-Token aus. Diese Token sollten auf dem Gerät des Benutzers zwischengespeichert und für die Dauer der zugehörigen Lebensdauer verwendet werden.



### Caching-Richtlinien {#caching}

- Authentifizierungstoken
- Autorisierungstoken
- Kurzlebige Medien-Token


#### Authentifizierungstoken

- **AccessEnabler 1.7:** Dieses SDK führt eine neue Methode zur Token-Speicherung ein, die mehrere Programmer-MVPD-Buckets und damit mehrere Authentifizierungstoken ermöglicht. Jetzt wird dasselbe Speicherlayout sowohl für das Szenario &quot;Authentifizierung pro Anforderer&quot;als auch für den normalen Authentifizierungsfluss verwendet. Der einzige Unterschied zwischen den beiden besteht in der Art und Weise, wie die Authentifizierung durchgeführt wird: &quot;Authentifizierung pro Anforderer&quot;enthält eine neue Verbesserung (passive Authentifizierung), die es dem AccessEnabler ermöglicht, eine Back-Channel-Authentifizierung durchzuführen, basierend auf der Existenz eines Authentifizierungstokens im Speicher (für einen anderen Programmierer). Der Benutzer muss sich nur einmal authentifizieren. Diese Sitzung wird verwendet, um Authentifizierungstoken in zusätzlichen Apps zu erhalten. Dieser Rückkanal-Fluss findet während der [`setRequestor()`](#setReq) und ist größtenteils für den Programmierer transparent. **Es gibt jedoch eine wichtige Anforderung: Der Programmierer MUSS setRequestor() aus dem Haupt-UI-Thread aufrufen.**
- **AccessEnabler 1.6 und älter:** Wie Authentifizierungstoken auf dem Gerät zwischengespeichert werden, hängt von der **Authentifizierung pro Anforderer&quot;** Markierung, die mit dem aktuellen MVPD verknüpft ist:

<!-- end list -->

1. Wenn die Funktion &quot;Authentifizierung pro Anforderer&quot;deaktiviert ist, wird ein einzelnes Authentifizierungstoken lokal in der globalen Zwischenablage gespeichert. Dieses Token wird für alle Anwendungen freigegeben, die mit dem aktuellen MVPD integriert sind.
1. Wenn die Funktion &quot;Authentifizierung pro Anforderer&quot;aktiviert ist, wird ein Token explizit dem Programmierer zugeordnet, der den Authentifizierungsfluss durchgeführt hat (das Token wird nicht in der globalen Zwischenablage, sondern in einer privaten Datei gespeichert, die nur der Anwendung dieses Programmierers angezeigt wird). Genauer gesagt wird Single Sign-On (SSO) zwischen verschiedenen Anwendungen deaktiviert. Der Benutzer muss den Authentifizierungsfluss beim Wechsel zu einer neuen App explizit durchführen (vorausgesetzt, der Programmierer der zweiten App ist mit dem aktuellen MVPD integriert und für diesen Programmierer gibt es kein Authentifizierungstoken im lokalen Cache).



#### Autorisierungstoken

Es wird zu jedem Zeitpunkt nur ein Autorisierungstoken pro RESOURCE vom AccessEnabler zwischengespeichert. Es können mehrere Autorisierungstoken zwischengespeichert werden, sie sind jedoch mit verschiedenen Ressourcen verknüpft. Wenn ein neues Autorisierungstoken ausgestellt wird und bereits ein altes für *dieselbe Ressource*, überschreibt das neue Token den vorhandenen zwischengespeicherten Wert.



#### Medien-Token

Das Token für kurzlebige Medien sollte überhaupt NICHT zwischengespeichert werden. Das Medien-Token sollte jedes Mal, wenn eine Autorisierungs-API aufgerufen wird, vom Server abgerufen werden, da es auf die einmalige Verwendung beschränkt ist.



### Persistenz {#persistence}

Token müssen in aufeinander folgenden Ausführungen derselben Anwendung persistent sein. Das bedeutet, dass nach dem Erwerb der Authentifizierungs- und Autorisierungstoken und dem Schließen der Anwendung durch den Benutzer dieselben Token für die Anwendung verfügbar sind, wenn der Benutzer die Anwendung erneut öffnet. Darüber hinaus ist es wünschenswert, dass diese Token über mehrere Anwendungen hinweg persistent sind. Anders ausgedrückt: Wenn ein Benutzer eine Anwendung verwendet, um sich bei einem bestimmten Identitätsanbieter anzumelden (um Authentifizierungs- und Autorisierungstoken erfolgreich zu erhalten), können dieselben Token über eine andere Anwendung verwendet werden. Der Benutzer wird bei der Anmeldung über denselben Identitätsanbieter nicht mehr zur Eingabe von Anmeldeinformationen aufgefordert. Dieser nahtlose Authentifizierungs-/Autorisierungs-Workflow macht die Adobe Pass-Authentifizierungslösung zu einer echten TV-Überall-Implementierung.



## iOS

Die iOS AccessEnabler-Bibliothek umgeht die Probleme der anwendungsübergreifenden Datenfreigabe, indem die Token-Daten in einer &quot;clipboard-ähnlichen&quot;Datenstruktur gespeichert werden, die als *Pinnwand einfügen*. Diese freigegebene Ressource auf Systemebene bietet die wichtigsten Komponenten, die die Implementierung des gewünschten Anwendungsfalls für beständige Token ermöglichen:

- **Unterstützung für strukturierten Speicher** - Die Einfügemarke ist nicht nur eine einfache, lineare, pufferähnliche Speicherstruktur. Es bietet einen wörterbuchähnlichen Speichermechanismus, der eine Datenindizierung basierend auf benutzerdefinierten Schlüsselwerten ermöglicht.
- **Unterstützung der Datenpersistenz mithilfe des zugrunde liegenden Dateisystems** - Der Inhalt der Pinnwand-Struktur kann als persistent markiert werden. In diesem Fall werden die Daten im internen Speicher des Geräts gespeichert.



Sobald ein bestimmtes Token im Token-Cache platziert wurde, wird seine Gültigkeit von der AccessEnabler-Bibliothek zu unterschiedlichen Zeitpunkten überprüft.  Ein gültiges Token wird wie folgt definiert:

- Die TTL des Tokens ist nicht abgelaufen.
- Der Herausgeber des Tokens ist in der Liste der zulässigen Identitätsanbieter enthalten.



## tvOS

Da auf tvOS die Zwischenablage nicht verfügbar ist, verwendet die tvOS AccessEnabler-Bibliothek die NsUserDefaults als Speicheroption. Dadurch wird das Problem behoben, dass Authentifizierungs- und Autorisierungstoken beibehalten werden, die gespeicherten Informationen können jedoch nicht von verschiedenen Anwendungen gemeinsam genutzt werden.



**Änderungen an der Zwischenablage in iOS 10 -** Beim Upgrade von einer früheren Version von iOS wird die Zwischenablage gelöscht. Das bedeutet, dass alle Anwendungen erneut authentifiziert werden müssen.



**Änderungen an der iOS 7-Zwischenablage -** Aufgrund von Änderungen in der Funktionsweise von Pasteboards in iOS 7 ist die Cross-SSO zwischen Anwendungen, die auf iOS 7 ausgeführt werden, eingeschränkt. Anwendungen, die dieselbe `<Bundle Seed ID>`(auch bekannt als `<Team ID>`) Token gemeinsam verwenden, d. h., dass Apps A1 und A2 aus demselben Programmierer X Token gemeinsam nutzen, während App A1 (Programmierer X) und App A3 (Programmierer Y) Token nicht gemeinsam nutzen.

- Die Bundle-Seed-ID/Team-ID ist zwischen zwei Apps identisch, wenn sie von demselben Provisioning-Profil generiert werden. Weitere Informationen finden Sie unter diesem Link:
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Diese &quot;Cross-SSO&quot;-Beschränkung ist in iOS 7 unabhängig vom verwendeten Adobe Pass Authentication SDK vorhanden.

Weitere Informationen zum Konfigurieren von SSO auf iOS 7 und höher finden Sie in diesem technischen Hinweis (der Technote gilt für Access Enabler v1.8 und höher): <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Token-Speicher (AccessEnabler 1.7)

Ab AccessEnabler 1.7 kann der Token-Speicher mehrere Programmer-MVPD-Kombinationen unterstützen, die auf einer mehrstufigen verschachtelten Kartenstruktur basieren, die mehrere Authentifizierungstoken enthalten kann. Dieser neue Speicher wirkt sich in keiner Weise auf die öffentliche AccessEnabler-API aus und erfordert keine Änderungen auf der Seite des Programmierers. Im Folgenden finden Sie ein Beispiel, das diese neue Funktion veranschaulicht:

1. Öffnen Sie App1 (entwickelt von Programmer1).
1. Authentifizieren Sie sich mit MVPD1 (integriert in Programmer1).
1. Aussetzen/Schließen Sie die aktuelle Anwendung und öffnen Sie App2 (entwickelt von Programmer2).
1. Nehmen wir an, dass Programmer2 nicht in MVPD2 integriert ist. Daher wird der Benutzer NICHT in App2 authentifiziert.
1. Authentifizieren Sie sich mit MVPD2 (das mit Programmer2 integriert ist) in App2.
1. Wechseln Sie zurück zu App1. Der Benutzer wird weiterhin mit Programmer1 authentifiziert.

In älteren Versionen von AccessEnabler würde Schritt 6 den Benutzer als nicht authentifiziert rendern, da der Token-Speicher zuvor nur ein Authentifizierungstoken unterstützte.



Durch Abmelden von einer Programmer-/MVPD-Sitzung wird der gesamte zugrunde liegende Speicher gelöscht, einschließlich aller anderen Programmierer-/MVPD-Authentifizierungstoken auf dem Gerät. Auf der anderen Seite Abbrechen des Authentifizierungsflusses (Aufrufen von [`setSelectedProvider(null)`](#setSelProv)) den zugrunde liegenden Speicher NICHT löschen, sondern nur den aktuellen Programmierer/MVPD-Authentifizierungsversuch beeinflussen (durch Löschen des MVPD für den aktuellen Programmierer).



### Token Importer (AccessEnabler 1.7)

Eine weitere speicherbezogene Funktion, die in AccessEnabler 1.7 enthalten ist, ermöglicht den Import von Authentifizierungstoken aus älteren Speicherbereichen. Dieser &quot;Token Importer&quot;trägt dazu bei, die Kompatibilität zwischen aufeinander folgenden AccessEnabler-Versionen zu erreichen, wodurch der SSO-Status auch bei Aktualisierung der Speicherversion beibehalten wird. Der Importeur wird während der [`setRequestor()`](#setReq) fließt und wird in den folgenden beiden Szenarien ausgeführt (vorausgesetzt, dass im aktuellen Speicher kein gültiges Authentifizierungstoken für den aktuellen Programmierer vorhanden ist):

- Die erste Installation einer 1.7-App, die von einem bestimmten Programmierer entwickelt wurde
- Aktualisierungspfad zu einem zukünftigen AccessEnabler, der einen neuen Speicher verwendet

Der Importvorgang ist für den Programmierer transparent und erfordert keine Codeänderungen in der Clientanwendung.



### Token Sanitizer (AccessEnabler 1.7.5)

Ab AccessEnabler 1.7.5 läuft dieser Dienst auf [`setRequestor()`](#setReq)`. `Sie wurde durch den Wechsel von der WiFi MAC-Adresse zur IDFA-Verfolgung von iOS 7 entwickelt. Der Bereiniger stellt sicher, dass der aktuelle Speicher nur gültige Authentifizierungstoken enthält (gültig für die Geräte-ID, die zuvor unter Verwendung der MAC-Adresse vor iOS7 berechnet wurde). Der Token-Bereiniger entfernt alle ungültigen Token.



Der Token Sanitizer-Dienst ist am nützlichsten, wenn eine AccessEnabler 1.7.5-Anwendung auf iOS 6 verwendet wird und der Benutzer dann auf iOS 7 aktualisiert. In diesem Fall sind alle Authentifizierungstoken, die in iOS 6 abgerufen wurden, ungültig (aufgrund des Geräte-ID-Algorithmus ändert sich die Verwendung der MAC-Adresse in den IDFA). Der Bereiniger löscht alle ungültigen Token, und der Benutzer wird dann nicht authentifiziert.



Ohne das Entfernen ungültiger Token durch den Token-Berater würde AccessEnabler aufgrund ungültiger Authentifizierungstoken keine Berechtigungen erlangen. Dies wäre für den Endbenutzer schwierig zu debuggen, da Autorisierungsfehlermeldungen verschlüsselt sein können und nicht allzu hilfreich bei der Ermittlung der Ursache des Problems sein können.



### Format {#format}

- [AuthN-Token](#authn_token)
- [AuthZ-Token](#authz_token)
- [Short Media Token](#short_token)
- [Gerätebindung](#device_binding)


Beachten Sie, dass das Format der AuthN- und AuthZ-Token hier nur für Hintergrundinformationen enthalten ist. Die Struktur dieser Token kann von der Adobe Pass-Authentifizierung jederzeit geändert werden. Programmierer müssen nicht die genaue Struktur der AuthN- und AuthZ-Token kennen, um ihre Apps zu implementieren, da die AuthN- und AuthZ-Token nicht auf dem lokalen Gerät verfügbar gemacht werden. Das Short Media-Token *is* der Anwendung des Programmierers ausgesetzt sind.



#### Authentifizierungstoken {#authn_token}

Die folgende Liste zeigt das Format des Authentifizierungstokens:

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthenticationToken>
      <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
      <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>   
  </simpleAuthenticationToken>
```


#### Autorisierungstoken {#authz_token}

Die folgende Liste zeigt das Format des Autorisierungstokens:

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthorizationToken>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
      <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>
  </simpleAuthorizationToken>
```


#### Short Media Token {#short_token}

Die folgende Liste zeigt das Format des Short-Media-Tokens. Dieses Token wird der Anwendung des Programmierers zur Verfügung gestellt. Er wird am Ende eines erfolgreichen Berechtigungsprozesses an die Anwendung des Programmierers weitergeleitet:

```
  <signatureInfo>signature<signatureInfo>
  <shortAuthorizationToken>
    <sessionGUID>session_guid</sessionGUID>
    <requestorID>requestor_id</requestorID>
    <resourceID>resource_id</resourceID>
    <ttl>ttl_in_ms</ttl>
    <issueTime>issue_time</issueTime>
    <mvpdId>mvpd_id</mvpdId>
    <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
  </shortAuthorizationToken>
```


### Gerätebindung {#device_binding}

Beachten Sie in den oben aufgeführten XML-Listen das Tag mit dem Namen `simpleTokenFingerprint`. Der Zweck dieses Tags besteht darin, native ID-Individualisierungsinformationen für Geräte zu speichern. Die AccessEnabler-Bibliothek kann diese Individualisierungsinformationen abrufen und den Adobe Pass-Authentifizierungsdiensten während der Berechtigungsaufrufe zur Verfügung stellen. Der Dienst verwendet diese Informationen und bettet sie in die tatsächlichen Token ein, wodurch die Token effektiv an ein bestimmtes Gerät gebunden werden. Das Endziel besteht darin, die Token geräteübergreifend nicht übertragbar zu machen.



Da dies offensichtlich eine sicherheitsrelevante Funktion ist, ist diese Information von Natur aus &quot;sensibel&quot; aus sicherheitspolitischer Sicht. Daher müssen diese Informationen sowohl vor Manipulation als auch vor Abhören geschützt werden. Das Abhörproblem wird gelöst, indem die Authentifizierungs-/Autorisierungsanfragen über das HTTPS-Protokoll gesendet werden. Der Manipulationsschutz wird durch digitale Signatur der Identifizierungsinformationen des Geräts erreicht. Die AccessEnabler-Bibliothek berechnet eine Geräte-ID aus den vom Gerät bereitgestellten Informationen und sendet dann die Geräte-ID &quot;in the clear&quot;als Anfrageparameter an die Adobe Pass-Authentifizierungsserver. Die Adobe Pass-Authentifizierungsserver signieren die Geräte-ID digital mit dem privaten Adobe-Schlüssel und fügen sie dem Authentifizierungstoken hinzu, das an den AccessEnabler zurückgegeben wird. Daher ist die Geräte-ID mit dem Authentifizierungstoken verknüpft. Während des Autorisierungsflusses sendet der AccessEnabler erneut die Geräte-ID in der -Datei zusammen mit dem Authentifizierungstoken. Wenn der Validierungsprozess fehlschlägt, schlagen die Authentifizierungs-/Autorisierungs-Workflows automatisch fehl. Die Adobe Pass-Authentifizierungsserver wenden den privaten Schlüssel auf die Geräte-ID an und vergleichen ihn mit dem Wert im Authentifizierungstoken. Wenn sie nicht übereinstimmen, schlägt dieser Berechtigungsfluss fehl.



**Hinweis zur Gerätebindung in AccessEnabler 1.7.5:** Ab AccessEnabler 1.7.5 ändert sich die Berechnung der Geräte-ID, um Individualisierungsinformationen für ein iOS-Gerät bereitzustellen. Diese Änderung spiegelt eine Änderung in iOS 7 wider: Ab iOS 7 stellt Apple die WLAN-MAC-Adresse nicht mehr als Tracking-Option zugunsten der ID für Advertiser (IDFA) bereit. Da die Individualisierungsinformationen für eine App, die auf iOS 7 ausgeführt wird, auf dem IDFA basieren und diese Informationen in die Berechtigungsfluss-Token eingebettet sind, bedeutet dies, dass diese Änderung eine Reihe verschiedener möglicher Auswirkungen auf das Benutzererlebnis hat. Die verschiedenen möglichen Auswirkungen basieren darauf, von welcher Version von iOS der Benutzer aktualisiert, kombiniert mit welcher Version von AccessEnabler, von der der Programmierer ein Upgrade durchführt. Weitere Informationen zu dieser Änderung finden Sie in den Versionshinweisen des AccessEnabler SDK 1.7.5.

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
