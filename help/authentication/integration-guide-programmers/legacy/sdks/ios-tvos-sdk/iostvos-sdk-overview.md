---
title: Übersicht über iOS/tvOS SDK
description: Übersicht über iOS/tvOS SDK
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3754'
ht-degree: 0%

---

# (Veraltete) Übersicht über iOS/tvOS SDK {#iostvos-sdk-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>


## Einführung {#intro}

iOS AccessEnabler ist eine Objective C iOS/tvOS-Bibliothek, die es Apps ermöglicht, die Adobe Pass-Authentifizierung für die Berechtigungsdienste von TV Everywhere zu verwenden. Die Implementierung besteht aus der *AccessEnabler*-Schnittstelle, die die Berechtigungs-API definiert, und den *EntitlementDelegate*- und *[EntitlementStatus](#ios%20entitlement%20status)*-Protokollen, die die Callbacks beschreiben, die von der Bibliothek Trigger werden. Die Schnittstelle zusammen mit dem Protokoll wird unter einem gemeinsamen Namen genannt: die AccessEnabler-Bibliothek.

## Anforderungen an iOS und tvOS {#reqs}

Aktuelle technische Anforderungen für die iOS- und tvOS-Plattform und die Adobe Pass-Authentifizierung finden Sie unter [Plattform-/Geräte-/Tool-Anforderungen](#ios) und in den Versionshinweisen zum SDK-Download. Im weiteren Verlauf dieser Seite werden Abschnitte angezeigt, in denen Änderungen vermerkt werden, die für bestimmte SDK-Versionen und höher gelten. Im Folgenden finden Sie beispielsweise einen legitimen Hinweis zu SDK 1.7.5:

## Grundlegendes zu nativen Client-Workflows {#flows}

Native Client-Workflows sind in der Regel mit denen browserbasierter Adobe Pass-Authentifizierungs-Clients identisch oder diesen sehr ähnlich. Es gibt jedoch einige Ausnahmen, wie unten beschrieben.

- [Workflow nach der Initialisierung](#post-init)
- [Allgemeiner anfänglicher Authentifizierungs-Workflow](#generic)
- [Abmelde-Workflow](#logout)


### Workflow nach der Initialisierung {#post-init}

Bei allen vom AccessEnabler unterstützten Berechtigungs-Workflows wird davon ausgegangen, dass Sie [`setRequestor()`](#setReq) zuvor aufgerufen haben, um Ihre Identität festzulegen. Sie führen diesen Aufruf aus, um Ihre Anforderer-ID nur einmal anzugeben, normalerweise während der Initialisierungs-/Einrichtungsphase Ihrer Anwendung.


Bei einem nativen iOS-Client haben Sie nach Ihrem ersten Aufruf an [`setRequestor()`](#setReq) die Wahl, wie Sie fortfahren:

- Sie können sofort mit Berechtigungsaufrufen beginnen und bei Bedarf zulassen, dass sie ohne Nachfrage in die Warteschlange gestellt werden.

- Sie können eine Bestätigung über den Erfolg/Misserfolg von [`setRequestor()`](#setReq) erhalten, indem Sie den [`setRequestorComplete()`](#setReqComplete) Callback implementieren.

- Sie können beide oben genannten Aktionen durchführen.

Sie haben die Wahl, Ihre App entweder auf die Benachrichtigung über den Erfolg von [`setRequestor()`](#setReq) warten zu lassen oder sie auf den Anrufwarteschlangenmechanismus von AccessEnabler zu verlassen. Da alle nachfolgenden Autorisierungs- und Authentifizierungsanfragen die Anforderer-ID und die zugehörigen Konfigurationsinformationen benötigen, blockiert die [`setRequestor()`](#setReq)-Methode effektiv alle Authentifizierungs- und Autorisierungs-API-Aufrufe, bis die Initialisierung abgeschlossen ist.



### Allgemeiner anfänglicher Authentifizierungs-Workflow {#generic}

Dieser Workflow dient der Anmeldung von Benutzenden mit ihrer MVPD. Nach erfolgreicher Anmeldung gibt der Backend-Server dem Benutzer ein Authentifizierungstoken aus. Während die Authentifizierung in der Regel als Teil des Autorisierungsprozesses erfolgt, wird im Folgenden beschrieben, wie die Authentifizierung isoliert funktionieren kann und keine Autorisierungsschritte enthält.

Beachten Sie, dass dieser Workflow sich zwar für native Clients vom typischen browserbasierten Authentifizierungs-Workflow unterscheidet, die Schritte 1 bis 5 jedoch für native Clients und browserbasierte Clients gleich sind.

1. Ihre Anwendung initiiert den Authentifizierungs-Workflow mit einem Aufruf der -API`getAuthentication() `Methode von AccessEnabler, die nach einem gültigen zwischengespeicherten Authentifizierungstoken sucht.
1. Wenn der Benutzer derzeit authentifiziert ist, ruft AccessEnabler Ihre [`setAuthenticationStatus()`](#setAuthNStatus)-Rückruffunktion auf, übergibt einen Authentifizierungsstatus, der auf Erfolg hinweist, und beendet den Fluss.
1. Wenn der Benutzer derzeit nicht authentifiziert ist, setzt AccessEnabler den Authentifizierungsfluss fort, indem festgestellt wird, ob der letzte Authentifizierungsversuch des Benutzers mit einer bestimmten MVPD erfolgreich war. Wenn eine MVPD-ID zwischengespeichert wird UND das `canAuthenticate`-Flag „true“ lautet ODER eine MVPD mithilfe von [`setSelectedProvider()`](#setSelProv) ausgewählt wurde, wird der/die Benutzende nicht über das MVPD-Auswahldialogfeld aufgefordert. Der Authentifizierungsfluss verwendet weiterhin den zwischengespeicherten Wert der MVPD (d. h. dieselbe MVPD, die bei der letzten erfolgreichen Authentifizierung verwendet wurde). Ein Netzwerkaufruf erfolgt an den Backend-Server und der Anwender wird zur Anmeldeseite von MVPD weitergeleitet (Schritt 6 unten).
1. Wenn keine MVPD-ID zwischengespeichert ist UND keine MVPD mithilfe von [`setSelectedProvider()`](#setSelProv) ausgewählt wurde ODER das `canAuthenticate`-Flag auf „false“ gesetzt ist, wird der [`displayProviderDialog()`](#dispProvDialog)-Callback aufgerufen. Dieser Rückruf weist Ihre Anwendung an, die Benutzeroberfläche zu erstellen, die dem Benutzer eine Liste von MVPDs zur Auswahl bietet. Es wird ein Array von MVPD-Objekten bereitgestellt, das die erforderlichen Informationen zum Erstellen des MVPD-Selektors enthält. Jedes MVPD-Objekt beschreibt eine MVPD-Entität und enthält Informationen wie die ID der MVPD (z. B. XFINITY, AT\&amp;T usw.) und die URL, unter der sich das MVPD-Logo befindet.
1. Nachdem eine bestimmte MVPD ausgewählt wurde, muss die Anwendung den AccessEnabler über die Auswahl des Benutzers informieren. Sobald der Anwender die gewünschte MVPD auswählt, informiert der AccessEnabler über die Benutzerauswahl über einen Aufruf der [`setSelectedProvider()`](#setSelProv).
1. IOS AccessEnabler ruft Ihren `navigateToUrl:`- oder `navigateToUrl:useSVC:`-Callback auf, um den Benutzer zur Anmeldeseite von MVPD weiterzuleiten. Durch Auslösen einer der beiden Optionen fordert AccessEnabler Ihre Anwendung auf, einen `UIWebView/WKWebView or SFSafariViewController` Controller zu erstellen und die im `url` des Callbacks bereitgestellte URL zu laden. Dies ist die URL des Authentifizierungsendpunkts auf dem Backend-Server. Bei tvOS AccessEnabler wird der [status()](#status_callback_implementation) Callback mit einem `statusDictionary` aufgerufen und der Abruf für die zweite Bildschirmauthentifizierung wird sofort gestartet. Die `statusDictionary` enthält die `registration code`, die für die Authentifizierung auf dem zweiten Bildschirm verwendet werden muss.
1. Im Fall von iOS AccessEnabler landet der Benutzer auf der Anmeldeseite von MVPD, um seine Anmeldeinformationen über das Medium Ihres Programm-Controllers `UIWebView/WKWebView or SFSafariViewController `. Beachten Sie, dass während dieser Übertragung mehrere Umleitungsvorgänge stattfinden und Ihre Anwendung die URLs überwachen muss, die vom Controller während der mehreren Umleitungsvorgänge geladen werden.
1. Beim Laden einer bestimmten benutzerdefinierten URL durch den `UIWebView/WKWebView or SFSafariViewController`-Controller muss die Anwendung im Fall von iOS AccessEnabler den Controller schließen und die `handleExternalURL:url `API-Methode von AccessEnabler aufrufen. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht vom Controller geladen werden soll. Sie darf von Ihrer Anwendung nur als Signal interpretiert werden, dass der Authentifizierungsfluss abgeschlossen ist und dass es sicher ist, den `UIWebView/WKWebView or SFSafariViewController`-Controller zu schließen. Falls Ihre Anwendung einen Controller verwenden muss `SFSafariViewController `die spezifische benutzerdefinierte URL wird durch die `application's custom scheme` definiert (z. B.: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), andernfalls wird diese spezifische benutzerdefinierte URL durch die `ADOBEPASS_REDIRECT_URL`-Konstante definiert (d. h. `adobepass://ios.app`).
1. Sobald Ihre Anwendung den `UIWebView/WKWebView or SFSafariViewController`-Controller schließt und die API-Methode von AccessEnabler aufruft`handleExternalURL:url ` ruft AccessEnabler das Authentifizierungstoken vom Backend-Server ab und informiert Ihre Anwendung darüber, dass der Authentifizierungsfluss abgeschlossen ist. Der AccessEnabler ruft den [`setAuthenticationStatus()`](#setAuthNStatus)-Callback mit dem Status-Code 1 auf und zeigt damit an, dass er erfolgreich war. Wenn während der Ausführung dieser Schritte ein Fehler auftritt, wird der [`setAuthenticationStatus()`](#setAuthNStatus)-Callback mit dem Status-Code 0 ausgelöst, was auf einen Authentifizierungsfehler hinweist, sowie mit einem entsprechenden Fehler-Code.


>[!WARNING]
>
> Bei den Schritten, bei denen AccessEnabler die Kontrolle an die App abgibt (d. h. wenn das Dialogfeld zur Anbieterauswahl angezeigt wird oder wenn UIWebView/WKWebView oder SFSafariViewController angezeigt wird), hat der Benutzer die Möglichkeit, den Authentifizierungsfluss abzubrechen. In diesen Fällen ist Ihre App dafür verantwortlich, AccessEnabler über dieses Ereignis zu informieren und die [`setSelectedProvider()`](#setSelProv) API-Methode aufzurufen, wobei null als Parameter übergeben wird. Dadurch erhält AccessEnabler die Möglichkeit, den internen Status zu bereinigen und den Authentifizierungsfluss zurückzusetzen. Auf tvOS können Sie dieselbe Methode verwenden, um den Authentifizierungs-Abruf abzubrechen.


### Abmelde-Workflow {#logout}

Bei nativen Clients wird die Abmeldung ähnlich wie bei dem oben beschriebenen Authentifizierungsprozess gehandhabt.

1. Ihre Anwendung initiiert den Abmelde-Workflow mit einem Aufruf der (API`logout() `-Methode von AccessEnabler. Die Abmeldung ist das Ergebnis einer Reihe von HTTP-Umleitungsvorgängen, da der Benutzer sowohl von den Adobe Pass-Authentifizierungsservern als auch von den MVPD-Servern abgemeldet werden muss. Da dieser Fluss nicht mit einer einfachen HTTP-Anfrage abgeschlossen werden kann, die von der AccessEnabler-Bibliothek ausgegeben wird, muss ein `UIWebView/WKWebView or SFSafariViewController`-Controller instanziiert werden, um die HTTP-Umleitungsvorgänge verfolgen zu können.

1. Es wird ein ähnliches Muster wie beim Authentifizierungsfluss verwendet. Der iOS AccessEnabler-Trigger verwendet den `navigateToUrl:`-Callback oder den `navigateToUrl:useSVC:`, um einen `UIWebView/WKWebView or SFSafariViewController` Controller zu erstellen und die im `url` des Callbacks angegebene URL zu laden. Dies ist die URL des Abmeldeendpunkts auf dem Backend-Server. Für den tvOS AccessEnabler wird weder der `navigateToUrl:` Callback noch der `navigateToUrl:useSVC:` Callback aufgerufen.

1. Während mehrere Weiterleitungen durchlaufen, muss die Anwendung die Aktivität des Controllers überwachen `UIWebView/WKWebView or SFSafariViewController ` den Zeitpunkt erkennen, zu dem eine bestimmte benutzerdefinierte URL geladen wird. Beachten Sie, dass diese spezifische benutzerdefinierte URL tatsächlich ungültig ist und nicht vom Controller geladen werden soll. Sie darf von Ihrer Anwendung nur als Signal interpretiert werden, dass der Abmeldefluss abgeschlossen ist und dass es sicher ist, den Controller zu schließen. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss die Anwendung den Controller schließen und die Methode &quot;`handleExternalURL:url `&quot; von AccessEnabler aufrufen. Falls Ihre Anwendung einen Controller verwenden muss `SFSafariViewController `die spezifische benutzerdefinierte URL wird durch die `application's custom scheme` definiert (z. B. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), andernfalls wird diese spezifische benutzerdefinierte URL durch die ` ADOBEPASS_REDIRECT_URL  `Konstante (d. h. `adobepass://ios.app`) definiert.

1. Am Ende ruft AccessEnabler den [`setAuthenticationStatus()`](#setAuthNStatus) Callback mit dem Status-Code 0 auf, was den Erfolg des Abmeldevorgangs anzeigt.

Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss dadurch, dass der Benutzer in keiner Weise mit dem `UIWebView/WKWebView or SFSafariViewController`-Controller interagieren muss. Daher empfiehlt Adobe, das Steuerelement während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.

## Token {#tokens}

- [Definitionen und Verwendung](#definitions)
- [Richtlinien für das Zwischenspeichern](#caching)
- [Persistenz](#persistence)
- [Format](#format)
- [Gerätebindung](#device_binding)


### Definitionen und Verwendung {#definitions}

Die Lösung für die Berechtigung für die Adobe Pass-Authentifizierung umfasst die Generierung bestimmter Datenelemente (Token), die von der Adobe Pass-Authentifizierung nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungs-Workflows generiert werden. Diese Token werden lokal auf dem iOS-Gerät des Clients gespeichert.



Token haben eine begrenzte Lebensdauer; nach Ablauf müssen Token durch die erneute Initiierung der Authentifizierungs- und/oder Autorisierungs-Workflows erneut ausgestellt werden.



Es gibt drei Arten von Token, die während der Berechtigungs-Workflows ausgegeben werden:

- **Authentifizierungstoken:** Das Endergebnis des Benutzerauthentifizierungs-Workflows ist eine Authentifizierungs-GUID, die AccessEnabler verwenden kann, um Autorisierungsabfragen im Namen des Benutzers durchzuführen. Diese Authentifizierungs-GUID hat einen zugehörigen TTL-Wert (Time-to-Live), der sich von der Authentifizierungssitzung des Benutzers selbst unterscheiden kann. Ein Authentifizierungs-Token wird generiert, indem die Authentifizierungs-GUID an das Gerät gebunden wird, das die Authentifizierungsanfragen initiiert.
- **Autorisierungs-Token:** Gewährt Zugriff auf eine bestimmte geschützte Ressource, die durch eine eindeutige resourceID identifiziert wird. Sie besteht aus einer Autorisierungserteilung, die von der autorisierenden Partei zusammen mit der ursprünglichen Ressourcen-ID ausgestellt wird. Diese Informationen sind an das Gerät gebunden, das die Anfrage initiiert.
- **Kurzlebiges Medien-Token:** AccessEnabler gewährt durch Rückgabe eines kurzlebigen Medien-Tokens Zugriff auf die Hosting-Anwendung für eine bestimmte Ressource. Dieses Token wird basierend auf dem Autorisierungs-Token generiert, das zuvor für diese bestimmte Ressource abgerufen wurde. Außerdem ist dieses Token nicht an das Gerät gebunden und die zugehörige Lebensdauer ist erheblich kürzer (Standard: 5 Minuten).

Nach erfolgreicher Authentifizierung und Autorisierung gibt die Adobe Pass-Authentifizierung Authentifizierungs-, Autorisierungs- und kurzlebige Medien-Token aus. Diese Token sollten auf dem Gerät des Benutzers zwischengespeichert und für die Dauer der zugehörigen Lebensdauer verwendet werden.



### Richtlinien für das Zwischenspeichern {#caching}

- Authentifizierungstoken
- Autorisierungs-Token
- Kurzlebiges Medien-Token


#### Authentifizierungstoken

- **AccessEnabler 1.7:** Diese SDK führt eine neue Methode der Token-Speicherung ein, die mehrere Programmer-MVPD-Buckets und daher mehrere Authentifizierungstoken ermöglicht. Jetzt wird dasselbe Speicher-Layout sowohl für das Szenario „Authentifizierung pro Anforderer“ als auch für den normalen Authentifizierungsfluss verwendet. Der einzige Unterschied zwischen den beiden besteht in der Art und Weise, wie die Authentifizierung durchgeführt wird: „Authentifizierung per Requestor“ enthält eine neue Verbesserung (passive Authentifizierung), die es dem AccessEnabler ermöglicht, eine Back-Channel-Authentifizierung durchzuführen, basierend auf der Existenz eines Authentifizierungstokens im Speicher (für einen anderen Programmierer). Der Benutzer muss sich nur einmal authentifizieren und diese Sitzung wird verwendet, um Authentifizierungs-Token in zusätzlichen Apps zu erhalten. Dieser Rückkanal-Fluss erfolgt während des [`setRequestor()`](#setReq)-Aufrufs und ist für den Programmierer größtenteils transparent. **Es gibt jedoch eine wichtige Anforderung: Der Programmierer MUSS setRequestor() über den Haupt-Thread der Benutzeroberfläche aufrufen.**
- **AccessEnabler 1.6 und älter:** Die Art und Weise, wie Authentifizierungstoken auf dem Gerät zwischengespeichert werden, hängt von der Markierung &quot;**Authentifizierung pro Anforderer“** ab, die mit der aktuellen MVPD verknüpft ist:

<!-- end list -->

1. Wenn die Funktion „Authentifizierung pro Anfordernder“ deaktiviert ist, wird ein einziges Authentifizierungstoken lokal in der globalen Zwischenablage gespeichert; dieses Token wird für alle Anwendungen freigegeben, die in der aktuellen MVPD integriert sind.
1. Wenn die Funktion „Authentifizierung pro Anforderer“ aktiviert ist, wird ein Token explizit dem Programmierer zugeordnet, der den Authentifizierungsfluss durchgeführt hat (das Token wird nicht in der globalen Zwischenablage gespeichert, sondern in einer privaten Datei, die nur für die Anwendung dieses Programmierers sichtbar ist). Insbesondere wird Single Sign-On (SSO) zwischen verschiedenen Anwendungen deaktiviert; der Benutzer muss den Authentifizierungsfluss explizit ausführen, wenn er zu einer neuen Anwendung wechselt (vorausgesetzt, der Programmierer der zweiten Anwendung ist mit der aktuellen MVPD integriert und es gibt kein Authentifizierungstoken für diesen Programmierer im lokalen Cache).



#### Autorisierungs-Token

Es wird immer nur EIN Autorisierungs-Token PRO RESSOURCE vom AccessEnabler zwischengespeichert. Es können mehrere Autorisierungs-Token zwischengespeichert werden, sie sind jedoch mit verschiedenen Ressourcen verknüpft. Wenn ein neues Autorisierungs-Token ausgegeben wird und bereits ein altes Token für *dieselbe Ressource* vorhanden ist, überschreibt das neue Token den vorhandenen zwischengespeicherten Wert.



#### Medien-Token

Das kurzlebige Medien-Token sollte ÜBERHAUPT NICHT zwischengespeichert werden. Das Medien-Token sollte jedes Mal vom Server abgerufen werden, wenn eine Autorisierungs-API aufgerufen wird, da es auf die einmalige Verwendung beschränkt ist.



### Persistenz {#persistence}

Token müssen über mehrere aufeinander folgende Ausführungen desselben Programms hinweg persistent sein. Das bedeutet, dass nach dem Abrufen der Authentifizierungs- und Autorisierungs-Token und dem Schließen der Anwendung durch den Benutzer dieselben Token für die Anwendung verfügbar sind, wenn der Benutzer die Anwendung erneut öffnet. Darüber hinaus ist es wünschenswert, dass diese Token über mehrere Anwendungen hinweg persistent sind. Mit anderen Worten: Wenn ein Benutzer eine Anwendung verwendet, um sich bei einem bestimmten Identitätsanbieter anzumelden (und Authentifizierungs- und Autorisierungs-Token erfolgreich erhalten hat), können dieselben Token über eine andere Anwendung verwendet werden, und der Benutzer wird bei der Anmeldung über denselben Identitätsanbieter nicht mehr zur Eingabe von Anmeldeinformationen aufgefordert. Dieser nahtlose Authentifizierungs-/Autorisierungs-Workflow macht die Adobe Pass-Authentifizierungslösung zu einem echten TV-Everywhere
-Implementierung.



## iOS

Die iOS AccessEnabler-Bibliothek umgeht die Probleme der programmübergreifenden Datenfreigabe, indem die Tokendaten in einer „clipboard-ähnlichen“ Datenstruktur namens &quot;*Board“ gespeichert*. Diese gemeinsam genutzte Ressource auf Systemebene bietet die wichtigsten Zutaten, die die Implementierung des gewünschten Anwendungsfalls für persistente Token ermöglichen:

- **Unterstützung für strukturierten Speicher** - Die Einfügekarte ist nicht nur eine einfache, lineare, pufferähnliche Speicherstruktur. Es bietet einen wörterbuchähnlichen Speichermechanismus, der die Indizierung von Daten auf der Grundlage benutzerdefinierter Schlüsselwerte ermöglicht.
- **Unterstützung für die Datenpersistenz mithilfe des zugrunde liegenden Dateisystems** - Der Inhalt der Struktur der eingefügten Pinnwand kann als persistent markiert werden. In diesem Fall werden die Daten im internen Speicher des Geräts gespeichert.



Sobald ein bestimmtes Token im Token-Cache platziert ist, wird seine Gültigkeit von der AccessEnabler-Bibliothek zu verschiedenen Zeiten überprüft.  Ein gültiges Token wird wie folgt definiert:

- Die TTL des Tokens ist nicht abgelaufen.
- Der Aussteller des Tokens ist in der Liste der zulässigen Identitätsanbieter enthalten.



## tvOS

Da unter tvOS die Zwischenablage nicht verfügbar ist, verwendet die tvOS AccessEnabler-Bibliothek die NsUserDefaults als Speicheroption. Dies löst das Problem, Authentifizierungs- und Autorisierungs-Token beizubehalten, aber die gespeicherten Informationen können nicht zwischen verschiedenen Programmen freigegeben werden.



**Änderungen der iOS 10-Zwischenablage -** Beim Upgrade von einer früheren Version von iOS wird die Zwischenablage gelöscht. Das bedeutet, dass alle Anwendungen erneut authentifiziert werden müssen.



**Änderungen an iOS 7-Zwischenablage -** Aufgrund von Änderungen in der Funktionsweise von Zwischenablagen in iOS 7 ist die Cross-SSO zwischen Anwendungen, die auf iOS 7 ausgeführt werden, begrenzt. Anwendungen mit demselben `<Bundle Seed ID>` (auch als `<Team ID>` bezeichnet) teilen Token, d. h. Apps A1 und A2 vom selben Programmierer X teilen Token, während App A1 (Programmierer X) und App A3 (Programmierer Y) Token nicht teilen.

- Die Paket-Seed-ID/Team-ID ist in zwei Apps identisch, wenn sie von demselben Bereitstellungsprofil generiert werden. Weitere Informationen finden Sie unter diesem Link:
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Diese „Cross-SSO“-Beschränkung ist in iOS 7 vorhanden, unabhängig von der verwendeten Adobe Pass-Authentifizierungs-SDK.

Weitere Informationen zur SSO-Konfiguration für iOS 7 und höher finden Sie in dieser technischen Anmerkung (die technische Anmerkung gilt für Access Enabler v1.8 und höher): <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Token-Speicher (AccessEnabler 1.7)

Ab AccessEnabler 1.7 kann der Token-Speicher mehrere Programmer-MVPD-Kombinationen unterstützen, wobei eine mehrstufige verschachtelte Zuordnungsstruktur verwendet wird, die mehrere Authentifizierungstoken enthalten kann. Dieser neue Speicher hat keinerlei Auswirkungen auf die öffentliche AccessEnabler-API und erfordert keine Änderungen seitens des Programmierers. Hier ein Beispiel für
veranschaulicht diese neue Funktion:

1. Öffnen Sie App1 (entwickelt von Programmer1).
1. Authentifizierung mit MVPD1 (in Programmer1 integriert).
1. Aussetzen/Schließen Sie die aktuelle Anwendung und öffnen Sie App2 (entwickelt von Programmer2).
1. Nehmen wir an, dass Programmer2 nicht in MVPD2 integriert ist. Daher wird der Benutzer NICHT in App2 authentifiziert.
1. Authentifizierung bei MVPD2 (das mit Programmer2 integriert ist) in App2.
1. Wechseln Sie zurück zu App1. Der Benutzer wird weiterhin mit Programmer1 authentifiziert.

In älteren Versionen von AccessEnabler wurde der Benutzer in Schritt 6 als nicht authentifiziert gerendert, da der Token-Speicher zuvor nur ein Authentifizierungstoken unterstützt hatte.



Wenn Sie sich von einer Programmierer-/MVPD-Sitzung abmelden, wird der gesamte zugrunde liegende Speicher gelöscht, einschließlich aller anderen Programmierer-/MVPD-Authentifizierungstoken auf dem Gerät. Andererseits wird durch Abbrechen des Authentifizierungsflusses (Aufrufen von [`setSelectedProvider(null)`](#setSelProv)) DER zugrunde liegende Speicher NICHT gelöscht, sondern es wirkt sich nur auf den aktuellen Authentifizierungsversuch von Programmierer/MVPD aus (durch Löschen der MVPD für den aktuellen Programmierer).



### Token Importer (AccessEnabler 1.7)

Eine weitere speicherbezogene Funktion, die in AccessEnabler 1.7 enthalten ist, ermöglicht den Import von Authentifizierungs-Token aus älteren Speicherbereichen. Dieser „Token-Importer“ trägt dazu bei, die Kompatibilität zwischen aufeinander folgenden AccessEnabler-Versionen zu erreichen, wobei der SSO-Status auch bei einem Upgrade der Speicherversion erhalten bleibt. Das Import-Tool wird während des [`setRequestor()`](#setReq) ausgeführt und läuft in den folgenden beiden Szenarien (unter der Annahme, dass im aktuellen Speicher kein gültiges Authentifizierungstoken für den aktuellen Programmierer vorhanden ist):

- Die erste Installation einer 1.7-App, die von einem bestimmten Programmierer entwickelt wurde
- Upgrade-Pfad auf einen zukünftigen Access Enabler, der einen neuen Speicher verwendet

Der Importvorgang ist für den Programmierer transparent und erfordert keine Codeänderung in der Client-Anwendung.



### Token Sanitizer (AccessEnabler 1.7.5)

Ab AccessEnabler 1.7.5 läuft dieser Dienst auf [`setRequestor()`](#setReq)`. `Er wurde als Ergebnis des Umstiegs von iOS 7 von der WiFi MAC-Adresse auf den IDFA zum Tracking entwickelt. Die Bereinigung stellt sicher, dass der aktuelle Speicher nur gültige Authentifizierungstoken enthält (gültig in Bezug auf die Geräte-ID, die zuvor anhand der MAC-Adresse vor iOS7 berechnet wurde). Die Token-Bereinigung entfernt alle ungültigen Token.



Der Service „Token-Bereinigung“ ist am nützlichsten, wenn eine AccessEnabler 1.7.5-Anwendung auf iOS 6 verwendet wird und die Benutzerin bzw. der Benutzer dann auf iOS 7 aktualisiert. In diesem Fall sind alle Authentifizierungs-Token, die in iOS 6 abgerufen wurden, ungültig (aufgrund der Änderung des Geräte-ID-Algorithmus von der Verwendung der MAC-Adresse zum IDFA). Die Bereinigung löscht alle ungültigen Token, und die Authentifizierung des Benutzers wird dann aufgehoben.



Ohne das Entfernen ungültiger Token durch den Token-Sanitizer kann AccessEnabler aufgrund ungültiger Authentifizierungs-Token keine Autorisierungen abrufen. Dies erweist sich als schwierig für den Endbenutzer, das Problem zu debuggen, da Autorisierungsfehlermeldungen kryptisch sein können und nicht übermäßig hilfreich bei der Bestimmung der Ursache des Problems sind.



### Format {#format}

- [Authentifizierungs-Token](#authn_token)
- [AuthZ-Token](#authz_token)
- [Kurzes Medien-Token](#short_token)
- [Gerätebindung](#device_binding)


Beachten Sie, dass das Format der AuthN- und AuthZ-Token hier nur zur Hintergrundinformation enthalten ist. Die Struktur dieser Token kann durch die Adobe Pass-Authentifizierung jederzeit geändert werden. Programmierer müssen die genaue Struktur der AuthN- und AuthZ-Token nicht kennen, um ihre Apps zu implementieren, da die AuthN- und AuthZ-Token nicht auf dem lokalen Gerät verfügbar gemacht werden. Das Short Media *Token wird* Programm des Programmierers verfügbar gemacht.



#### Authentifizierungstoken {#authn_token}

In der folgenden Liste ist das Format des Authentifizierungstokens aufgeführt:

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


#### Autorisierungs-Token {#authz_token}

In der folgenden Liste ist das Format des Autorisierungs-Tokens aufgeführt:

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


#### Kurzes Medien-Token {#short_token}

In der folgenden Liste wird das Format des Kurzwahltokens angezeigt. Dieses Token wird für die Anwendung des Programmierers verfügbar gemacht. Sie wird am Ende eines erfolgreichen Berechtigungsprozesses an den Antrag des Programmierers übergeben:

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

Beachten Sie in den obigen XML-Auflistungen das Tag mit dem Titel `simpleTokenFingerprint`. Der Zweck dieses Tags ist es, native Geräte-ID-Individualisierungsinformationen zu enthalten. Die AccessEnabler-Bibliothek kann diese Personalisierungsinformationen abrufen und während der Berechtigungsaufrufe für die Adobe Pass-Authentifizierungsdienste verfügbar machen. Der Service verwendet diese Informationen und bettet sie in die tatsächlichen Token ein, wodurch die Token effektiv an ein bestimmtes Gerät gebunden werden. Das Endziel besteht darin, die Token nicht geräteübergreifend übertragbar zu machen.



Da es sich hierbei offensichtlich um eine sicherheitsbezogene Funktion handelt, sind diese Informationen aus Sicherheitssicht von Natur aus „sensibel“. Daher müssen diese Informationen sowohl vor Manipulationen als auch vor Abhörmaßnahmen geschützt werden. Das Problem des Abhörens wird gelöst, indem die Authentifizierungs-/Autorisierungsanfragen über das HTTPS-Protokoll gesendet werden. Der Manipulationsschutz wird durch eine digitale Signatur der Geräterkennungsinformationen gehandhabt. Die AccessEnabler-Bibliothek berechnet eine Geräte-ID aus den vom Gerät bereitgestellten Informationen und sendet dann die Geräte-ID „im Klartext“ als Abfrageparameter an die Adobe Pass-Authentifizierungsserver. Die Adobe Pass-Authentifizierungsserver signieren die Geräte-ID digital mit dem privaten Schlüssel von Adobe und fügen sie dem Authentifizierungstoken hinzu, das an AccessEnabler zurückgegeben wird. Daher ist die Geräte-ID an das Authentifizierungs-Token gebunden. Während des Autorisierungsflusses sendet der AccessEnabler die Geräte-ID erneut als gelöscht zusammen mit dem Authentifizierungstoken. Ein Fehlschlagen des Validierungsprozesses führt automatisch zum Fehlschlagen der Authentifizierungs-/Autorisierungs-Workflows. Die Adobe Pass-Authentifizierungsserver wenden den privaten Schlüssel auf die Geräte-ID an und vergleichen ihn mit dem Wert im Authentifizierungs-Token. Wenn sie nicht übereinstimmen, schlägt dieser Berechtigungsfluss fehl.



**Hinweis zur Gerätebindung in AccessEnabler 1.7.5:** Ab AccessEnabler 1.7.5 ändert sich die Art und Weise, wie die Geräte-ID berechnet wird, um Personalisierungsinformationen für ein iOS-Gerät bereitzustellen. Diese Änderung spiegelt eine Änderung in iOS 7 wider: Ab iOS 7 bietet Apple die WiFi-MAC-Adresse nicht mehr als Tracking-Option, sondern stattdessen die Kennung für Werbetreibende (IDFA). Da Individualisierungsinformationen für eine App, die auf iOS 7 ausgeführt wird, auf dem IDFA basieren und diese Informationen in die Berechtigungsfluss-Token eingebettet sind, bedeutet dies, dass es eine Reihe verschiedener möglicher Auswirkungen auf das Benutzererlebnis gibt, die aus dieser Änderung resultieren. Die verschiedenen möglichen Auswirkungen hängen davon ab, von welcher Version von iOS der Anwender ein Upgrade durchführt und von welcher Version des AccessEnabler der Programmierer ein Upgrade durchführt. Weitere Informationen zu dieser Änderung finden Sie in den Versionshinweisen zu AccessEnabler SDK 1.7.5.

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
