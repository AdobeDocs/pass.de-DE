---
title: REST-API-Cookbook (Server-zu-Server)
description: REST-API-Cookbook-Server zu Server.
exl-id: 36ad4a64-dde8-4a5f-b0fe-64b6c0ddcbee
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1856'
ht-degree: 0%

---

# (Legacy) REST-API-Cookbook (Server-zu-Server) {#rest-api-cookbook-server-to-server}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Überblick {#overview}

In diesem Dokument werden Best Practices für die Implementierung der Adobe Pass-Authentifizierung mithilfe von Server-zu-Server-Architekturen beschrieben.  Es enthält grundlegende Anforderungen, eine schrittweise Implementierung von Flüssen und allgemeine Überlegungen zu Produktionsumgebungen und zum Betrieb.

### Drosselmechanismus

Die Adobe Pass-Authentifizierungs-REST-API wird von einem [Einschränkungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gesteuert.


## Komponenten {#components}

In einer funktionierenden Server-zu-Server-Lösung sind die folgenden Komponenten beteiligt:


| Typ | Komponente | Beschreibung |
| --- | --- | --- |
| Streaming-Gerät | Streaming-App | Die Programmieranwendung, die sich auf dem Streaming-Gerät des Benutzers befindet und authentifizierte Videos abspielt. |
| | \[Optional\] Authentifizierungsmodul | Wenn das Streaming-Gerät über einen Benutzeragenten (d. h. einen Webbrowser) verfügt, ist das AuthN-Modul für die Authentifizierung des Benutzers auf der MVPD-ID verantwortlich. |
| \[Optional\] Authentifizierungs-Gerät | Authentifizierungs-App | Wenn das Streaming-Gerät keinen Benutzeragenten (d. h. Webbrowser) hat, ist die AuthN-Anwendung eine Programmierer-Webanwendung, auf die über einen Webbrowser von einem Gerät eines anderen Benutzers zugegriffen wird. |
| Programmiererinfrastruktur | Programmierdienst | Ein Service, der das Streaming-Gerät mit dem Adobe Pass-Service verknüpft, um Authentifizierungs- und Autorisierungsentscheidungen abzurufen. |
| Adobe-Infrastruktur | Adobe Pass-Service | Ein Service, der mit dem MVPD IDp- und AuthZ-Service integriert ist und Authentifizierungs- und Autorisierungsentscheidungen bereitstellt. |
| MVPD-Infrastruktur | MVPD IDp | Ein MVPD-Endpunkt, der einen auf Berechtigungen basierenden Authentifizierungsdienst zur Überprüfung der Benutzeridentität bereitstellt. |
| | MVPD AuthZ-Service | Ein MVPD-Endpunkt, der Autorisierungsentscheidungen basierend auf Benutzerabonnements, Kindersicherung usw. bereitstellt. |

## Flows {#flows}

### Dynamische Client-Registrierung (DCR)


Adobe Pass verwendet DCR, um die Client-Kommunikation zwischen einer Programmieranwendung oder einem Server und den Adobe Pass-Services zu sichern. Der DCR-Ablauf ist separat und in der Dokumentation [Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) beschrieben.


### Authentifizierung (authN)

Der Authentifizierungsfluss wird verwendet, um es einem Benutzer zu ermöglichen, sich zu identifizieren
auf ihre MVPD, um festzustellen, ob die Benutzerin bzw. der Benutzer über ein gültiges Konto verfügt.

1. Der Benutzer startet die Streaming-Geräte-App und versucht, sich anzumelden oder geschützte Inhalte anzuzeigen.
2. Die Streaming-Geräte-App fordert den Programmierer-Service auf, um festzustellen, ob das Gerät bereits authentifiziert ist.
3. Der Programmierer-Service registriert die App mithilfe von DCR.
4. Der Programmierer-Service überprüft den Authentifizierungsstatus des Streaming-Geräts, indem er die Adobe Pass-Service-API **checkauthn** aufruft.
5. Wenn der Aufruf **checkauthn** den Status zurückgibt, dass das Benutzergerät authentifiziert ist, kann die App mit dem Autorisierungsfluss fortfahren.
6. Wenn der Aufruf **checkauthn** den Status zurückgibt, dass das Benutzergerät NICHT authentifiziert ist, sollte die App auf die Anmeldung einer Benutzeranfrage warten.
7. Wenn der Benutzer eine direkte Anmeldung anfordert (z. B. die Anmeldeschaltfläche auswählt) oder sich indirekt anmeldet (z. B. geschützte Inhalte auswählt, wenn diese noch nicht authentifiziert sind), sendet die Streaming-Geräte-App eine Anfrage an den Programmierdienst, um die Benutzerauthentifizierung zu initiieren. Der Programmierer-Service fordert einen eindeutigen Registrierungs-Code (regcode) an und empfängt ihn, indem er die Adobe Pass-Service-API **regcode** aufruft.
8. Der Programmierer-Service ruft auch die Liste der aktuellen MVPDs und Attribute ab, indem er die Adobe Pass-Service-API **config** aufruft. Hinweis: Diese API kann auch früher im Fluss aufgerufen und zwischengespeichert werden.
9. Der Programmierer-Service gibt den Regcode an die Streaming-Geräte-App und die verarbeitete MVPD-Liste zurück, die in Schritt \#7 angefordert wird. Hinweis: Das verarbeitete MVPD-Listenformat wird vom Programmierer angegeben und kann gefiltert werden, um explizit bestimmte MVPDs zuzulassen oder zu blockieren (d. h. Zulassungslisten oder Blockierungslisten).
10. Wenn sich die Datei von der des AuthN-Geräts unterscheidet (d. h., der „zweite Bildschirm„), entweder aufgrund einer Auswahl oder aus Notwendigkeit (d. h. das Streaming-Gerät unterstützt keinen Benutzeragenten), sollte das Streaming-Gerät den Regcode und einen URI anzeigen, damit die Benutzerin oder der Benutzer auf die AuthN-Anwendung zugreifen kann. Der Benutzer gibt den URI in den Benutzeragenten auf dem AuthN-Gerät ein, um die AuthN-Anwendung zu starten, und gibt dann den Recode in diese Anwendung ein. Wenn das Streaming-Gerät mit dem AuthN-Gerät identisch ist, kann der Regcode programmgesteuert an das AuthN-Modul übergeben werden.
11. Das AuthN-Modul initiiert die Benutzerauthentifizierung mit der MVPD durch Anzeige einer MVPD-Auswahl. Nachdem der Benutzer die MVPD ausgewählt hat, ruft das AuthN-Modul **Authentifizieren** mit dem RegCode auf, wodurch der Benutzeragent zur MVPD-IDp weitergeleitet wird. Wenn sich der Benutzer erfolgreich bei der MVPD authentifiziert hat, wird der Benutzeragent zurück über den Adobe Pass-Service geleitet, wo die erfolgreiche Authentifizierung mit dem RegCode aufgezeichnet wird, und dann zurück zum AuthN-Modul geleitet wird.
12. Wenn sich das Streaming-Gerät vom AuthN-Gerät unterscheidet, sollte das AuthN-Gerät dem Benutzer eine Meldung über die erfolgreiche Authentifizierung und die Schritte zum Fortfahren anzeigen (z. B. „Erfolg\! Sie können jetzt zu Ihrer Spielkonsole zurückkehren, um \[…\]„) fortzusetzen. Wenn das Streaming-Gerät mit dem AuthN-Gerät identisch ist, kann das Streaming-Gerät den Abschluss der Authentifizierung programmgesteuert erkennen.



Das folgende Diagramm veranschaulicht den Authentifizierungsfluss:

![](/help//authentication/assets/authn-flow.png)

### Autorisierung (authZ)

Mit dem Autorisierungsfluss wird bestimmt, ob ein Benutzer berechtigt ist, auf angeforderte Inhalte zuzugreifen.

1. Jedes Mal, wenn Benutzende versuchen, geschützte Inhalte in der Streaming-Geräte-App anzuzeigen, ruft die Streaming-Geräte-App den Programmierer-Service auf, identifiziert den Inhalt und fordert die Berechtigung und Informationen an, die zum Starten des Streams erforderlich sind.
1. Der Programmierdienst ruft die Adobe Pass-API **authorize** auf und übergibt die Ressourcen-ID zusammen mit anderen erforderlichen Parametern. Der Adobe-Service ruft den MVPD AuthZ-Service mit der Ressourcen-ID auf und erhält eine Autorisierungsentscheidung, die dann zurück an den Programmierer-Service übergeben wird. Diese Autorisierungsentscheidung wird vom Adobe Pass-Service für einen konfigurierbaren Zeitraum zwischengespeichert. Bei nachfolgenden **authorize**-Aufrufen des Programmierdienstes an den Adobe Pass-Service wird der zwischengespeicherte Wert zurückgegeben, solange er gültig ist.
1. Wenn die Autorisierung erteilt wird, sollte der Programmierdienst die Adobe Pass **/tokens/media**-API aufrufen, die ein signiertes Medien-Token zurückgibt. Der Programmierdienst sollte das Medien-Token mithilfe der Media Token Verifier-Bibliothek (JAR) überprüfen. Wenn gültig, sollte der Programmiererdienst die Berechtigung und die erforderliche zum Starten des Streams (z. B. Stream-URL) zurückgeben, die in Schritt \#1 angefordert wurden.
1. Wenn die Autorisierung verweigert wird, gibt der **authorize**-Aufruf einen Fehlercode und eine Beschreibung an den Programmiererdienst zurück. Der Programmierer-Service sollte den Fehlercode und die Beschreibung (oder eine vom Programmierer geänderte Meldung) an die Anforderung in Schritt \#1 zurückgeben.

Das folgende Diagramm veranschaulicht den Autorisierungsfluss:

![](/help//authentication/assets/authz-flow.png)

### Abmelden

Mit dem Abmeldefluss kann ein Benutzer die Identität derzeit entfernen
Der Anwendung zugeordnet.

1. Wenn der Benutzer eine Abmeldung anfordert (d. h. das aktuelle, mit der Anwendung verknüpfte MVPD-Konto vom Gerät entfernen), ruft die Streaming-Geräte-App den Programmierer-Service auf und fordert ihn auf, sich vom Gerät abzumelden.
1. Der Programmierdienst sollte die Adobe Pass-API **Abmelden** aufrufen.

Das folgende Diagramm veranschaulicht den Abmeldefluss:

![](/help//authentication/assets/logout-flow.png)

### \[Optional\] Vorautorisierung (auch Pre-flight genannt)

Mit der Vorautorisierung können Sie aus einer Reihe von Ressourcen schnell ermitteln, auf welche Ressourcen ein Benutzer möglicherweise Zugriff hat.  Das Ergebnis dieses Aufrufs wird normalerweise verwendet, um die Benutzeroberfläche für einen einzelnen Benutzer anzupassen.

1. Sobald der Benutzer authentifiziert ist, kann das Streaming-Gerät den Programmierer-Service aufrufen, um den Inhalt anzufordern, zu dem der Benutzer zum Streamen berechtigt ist.

1. Der Programmierer-Service sollte die Adobe Pass-API **preauthorize** mit einer Liste von Ressourcen-IDs aufrufen, bei denen es sich um eine einfache Zeichenfolge handelt, die normalerweise einen Kanal darstellt, den ein Benutzer streamen darf. *Hinweis: Derzeit ist der Aufruf****preauthorize*** *so konfiguriert, dass die Liste auf fünf (5) Ressourcen-IDs beschränkt ist. Wenn mehr als fünf Ressourcen benötigt werden, können* Aufrufe ****** preauthorize *durchgeführt werden, oder der Aufruf kann so konfiguriert werden, dass er mit einer Vereinbarung der MVPDs mehr als fünf Ressourcen annimmt. Implementierungsprogramme sollten die Kosten eines*-***-***-*sowohl für MVPD-Ressourcen als auch die Antwortzeit für den Programmierer im Auge behalten und ihre Verwendung des -Aufrufs umsichtig strukturieren.*

1. Der **preauthorize**-Aufruf antwortet dem Programmierer-Service mit einem JSON-Objekt, das einen TRUE- oder FALSE-Wert für jede Ressourcen-ID in der Anfrage enthält, die angibt, ob der Benutzer für den zugehörigen Kanal berechtigt ist oder nicht. *Hinweis: Wenn eine MVPD keine Antwort für eine bestimmte Ressourcen-ID bereitstellt (z. B. aufgrund von Netzwerkfehlern oder Zeitüberschreitungen), wird der Wert standardmäßig auf „FALSE“ gesetzt.*

1. Der Programmierer-Service sollte die Aufrufantwort **preauthorize** verwenden, um eine vom Programmierer definierte benutzerdefinierte Antwort auf das Streaming-Gerät zu erstellen, normalerweise um die Präsentation für den Benutzer basierend auf seinen Berechtigungen zu personalisieren.

Das folgende Diagramm veranschaulicht den Vorautorisierungsfluss:

![](/help//authentication/assets/preauthz-flow.png)


### \[Optional\] Metadaten

Metadaten können verwendet werden, um Benutzerinformationen abzurufen, die von der MVPD freigegeben werden.
Beispiele hierfür sind Benutzer-ID, Postleitzahl usw.

1. Sobald der Benutzer authentifiziert ist, kann der Programmierer-Service die Adobe Pass-API **usermetadata** aufrufen, um Informationen über den authentifizierten Benutzer anzufordern.

1. Die Antwort enthält alle Metadaten, die für den jeweiligen Benutzer verfügbar sind. Die spezifischen Felder werden für jede Programmierer-/MVPD-Integration separat konfiguriert.

Das folgende Diagramm veranschaulicht den Vorautorisierungsfluss:



![](/help//authentication/assets/user-metadata-api-preauthz.png)



## Umgebungen und funktionale Anforderungen{#environments}



Ein Programmierer sollte mindestens zwei Umgebungen erstellen: eine für die Produktion und eine oder mehrere für das Staging.


### Produktion

Die Produktionsumgebung sollte hochverfügbar sein und für große oder unerwartete Spitzen (z. B. Live-Sport, Unterbrechungen) angemessen skaliert werden
Nachrichten).



Der Adobe Pass-Service läuft auf mehreren Rechenzentren, die über verschiedene Standorte in den USA verteilt sind.  Um die beste Reaktionszeit (d. h. niedrigste Latenz) des Adobe Pass-Services zu erzielen, sollte der Programmierer auch einen ähnlichen geografisch verteilten Service erstellen
Infrastruktur.


Der Programmierdienst sollte den DNS-Cache auf maximal 30 Sekunden beschränken, falls Adobe Traffic umleiten muss. Dies kann vorkommen, wenn ein Rechenzentrum nicht mehr verfügbar ist.


Der Programmierer sollte den öffentlichen IP-Bereich der Produktionsumgebung bereitstellen. Diese werden in eine Zulassungsliste von IPs in der Adobe Pass-Infrastruktur aufgenommen, auf die zugegriffen werden kann und die von den Adobe-Richtlinien für die faire API-Nutzung verwaltet werden.

### Staging

Die Staging-Umgebung kann minimal sein, sollte jedoch alle Systemkomponenten und Geschäftslogik enthalten. Es sollte ähnlich wie die Produktion funktionieren und das Testen von Releases außerhalb der Produktion ermöglichen. Idealerweise kann die Staging-Umgebung mit den Adobe Pass-Testumgebungen verbunden werden, damit sie vom Programmierer und bei Bedarf von Adobe verwendet werden kann, sodass wir beim Testen und bei der Fehlerbehebung helfen können.

### Funktionale Anforderungen

Der Programmierdienst muss genaue Informationen zur Gerätekennung des Geräts weitergeben, für das er die Flüsse ausführt. Darüber hinaus muss der Programmierer-Service die IP des Geräts, für das er die Flüsse ausführt, (in einer Kopfzeile „x-forwarded-for„) zusammen mit dem Verbindungsquellen-Port (im Feld Geräteinformationen ) übergeben:

    **X-Forwarded-For : \&lt;client\_ip\>**
    
    wobei \&lt;client\_ip\> die öffentliche Client-IP-Adresse ist
    
    
    
    Der Header muss bei **regcode**- und **authorize**-Aufrufen hinzugefügt werden
    
    Beispiele :
    
    POST /reggie/v1/{req\_id}/regcode HTTP/1.1
    
    X-Forwarded-For:203.45.101.20
    
    
    
    GET /api/v1/authorize HTTP/1.1
    
    X-Forwarded-For:203.45.101.20



Der Programmierdienst sollte Daten und Formate senden, die von einzelnen MVPDs oder integrierten Apps benötigt werden (z. B. Geräte-IP, Quell-Port, Geräteinformationen, MRSS, optionale Daten wie ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.


Der Programmierdienst muss beim Caching authN- und authZ-TTLs berücksichtigen und die authN- oder authZ-Sitzung bei einer Benachrichtigung ungültig machen.

Der Programmierer muss Zertifikate verwalten, die mit Adobe geteilt werden.
