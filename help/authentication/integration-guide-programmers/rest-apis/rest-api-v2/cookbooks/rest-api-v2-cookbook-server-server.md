---
title: REST API V2-Cookbook (Server-zu-Server)
description: REST API V2-Cookbook (Server-zu-Server)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# REST API V2-Cookbook (Server-zu-Server) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

In diesem Cookbook-Dokument werden Best Practices für die Implementierung der Adobe Pass-Authentifizierung mit der REST-API V2 in einer Server-zu-Server-Architektur beschrieben. Es enthält grundlegende Anforderungen, eine schrittweise Implementierung von Flüssen und allgemeine Überlegungen zu Produktionsumgebungen und zum Betrieb.

## Komponenten {#components}

In einer funktionierenden Server-zu-Server-Lösung sind die folgenden Komponenten beteiligt:

| Typ | Komponente | Beschreibung |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Streaming-Gerät | Streaming-App | Die Programmieranwendung, die sich auf dem Streaming-Gerät des Benutzers befindet und authentifizierte Videos abspielt. |
|                           | \[Optional\] Authentifizierungsmodul | Wenn das Streaming-Gerät über einen Benutzeragenten (d. h. einen Webbrowser) verfügt, ist das AuthN-Modul für die Authentifizierung des Benutzers auf der MVPD-ID verantwortlich. |
| \[Optional\] Authentifizierungs-Gerät | Authentifizierungs-App | Wenn das Streaming-Gerät keinen Benutzeragenten (d. h. Webbrowser) hat, ist die AuthN-Anwendung eine Programmierer-Webanwendung, auf die über einen Webbrowser von einem Gerät eines anderen Benutzers zugegriffen wird. |
| Programmiererinfrastruktur | Programmierdienst | Ein Service, der das Streaming-Gerät mit dem Adobe Pass-Service verknüpft, um Authentifizierungs- und Autorisierungsentscheidungen abzurufen. |
| Adobe-Infrastruktur | Adobe Pass-Service | Ein Service, der mit dem MVPD IDp- und AuthZ-Service integriert ist und Authentifizierungs- und Autorisierungsentscheidungen bereitstellt. |
| MVPD-Infrastruktur | MVPD IDp | Ein MVPD-Endpunkt, der einen auf Berechtigungen basierenden Authentifizierungsdienst zur Überprüfung der Benutzeridentität bereitstellt. |
|                           | MVPD AuthZ-Service | Ein MVPD-Endpunkt, der Autorisierungsentscheidungen basierend auf Benutzerabonnements, Kindersicherung usw. bereitstellt. |

Zusätzliche im Fluss verwendete Begriffe werden im [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) definiert.

Das folgende Diagramm veranschaulicht den gesamten Fluss:

![REST API V2-Cookbook (Server-zu-Server)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### Schritte zum Implementieren der REST-API V2 in der Server-zu-Server-Architektur {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Um die Adobe Pass REST API V2 zu implementieren, müssen Sie die folgenden Schritte ausführen, die in Phasen gruppiert sind.

## 0. Voraussetzungen {#prerequisites}

Bei Server-zu-Server-Implementierungen müssen die Streaming-App und der Programmiererdienst ein Protokoll für den Programmiererdienst einrichten, damit sie:

* Eindeutige Identifizierung der Streaming-App auf dem Gerät.
* Handeln Sie im Namen der Streaming-App und kommunizieren Sie mit dem Adobe Pass-Service.
* Erfassen und speichern Sie Informationen zur Streaming-App und zum Gerät, z. B. IP-Adresse, Quell-Port und Geräteinformationen, um sie an Adobe Pass weiterzugeben.
* Rückgabe von Entscheidungen und Anweisungen an die Streaming-App.

Parameter wie Geräte-ID, Client-ID, Client-Geheimnis (wie unten definiert) können entweder in der Streaming-App oder im Programmierdienst gespeichert werden.

## A. Phase der Registrierung {#registration-phase}

### Schritt 1: Registrieren des Programms {#step-1-register-your-application}

Damit die Anwendung Adobe Pass REST API V2 aufrufen kann, benötigt sie ein Zugriffstoken, das von der API-Sicherheitsebene benötigt wird.

Bei Server-zu-Server-Implementierungen kann sich der Programmierdienst im Namen einer Anwendungsinstanz registrieren. Die Werte für die Client-Anmeldeinformationen (Client-ID und Client-Geheimnis) müssen jedoch für jedes Streaming-Gerät abgerufen werden.

Um das Zugriffs-Token zu erhalten, kann der Programmierer-Service im Namen einer Streaming-App handeln und muss die in der Dokumentation [Dynamische Client-Registrierung](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschriebenen Schritte ausführen.

## B. Authentifizierungsphase {#authentication-phase}

### Schritt 2: Überprüfen auf vorhandene authentifizierte Profile {#step-2-check-for-existing-authenticated-profiles}

Der Programmierer-Service prüft im Auftrag der Streaming-App, ob vorhandene authentifizierte Profile vorhanden sind: `/api/v2/{serviceProvider}/profiles` ([Abrufen authentifizierter Profile](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)).

* Wenn kein Profil gefunden wird und die Streaming-Anwendung einen TempPass-Fluss implementiert
   * Dokumentation zur Implementierung von [Temporären Zugriffsflüssen](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Wenn kein Profil gefunden wird, implementiert die Streaming-Anwendung einen Authentifizierungsfluss
   * <b>Schritt 2.a:</b> Der Programmierdienst ruft die Liste der für den Dienstanbieter verfügbaren MVPDs ab: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Liste der verfügbaren MVPDs ](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Der Programmierdienst kann die Filterung der Liste der MVPDs implementieren und nur MVPDs anzeigen, die beim Ausblenden anderer (TempPass, Test-MVPDs, MVPDs in Entwicklung usw.) vorgesehen sind.
   * Der Programmierdienst sollte eine gefilterte MVPD-Liste für die Streaming-App zurückgeben, damit die Auswahl angezeigt wird. Der Benutzer wählt die MVPD aus.
   * Wenn MVPD aus der Streaming-App ausgewählt ist, erstellt der Programmiererdienst eine Sitzung: <b>/api/v2/{serviceProvider}/sessions</b><br>
([Authentifizierungssitzung erstellen](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * Ein Code und eine URL für die Authentifizierung werden zurückgegeben
      * Wenn ein Profil gefunden wird, kann der Programmiererdienst mit <a href="#preauthorization-phase">C fortfahren. Vorautorisierungsphase</a>
   * Der Programmierdienst sollte den CODE und die URL an die Streaming-App zurückgeben

### Schritt 3: Benutzer authentifizieren {#step-3-authenticate-the-user}

Verwenden eines Browsers oder einer Web-basierten Second-Screen-Anwendung:

* Option 1. Die Streaming-App kann einen Browser oder eine Webansicht öffnen, die zu authentifizierende URL laden und den Benutzer auf die Anmeldeseite von MVPD leiten, auf der Anmeldeinformationen übermittelt werden müssen
   * Benutzer gibt Login/Passwort ein, die letzte Umleitung zeigt eine Erfolgsseite an
* Option 2. Die Streaming-App kann einen Browser nicht öffnen und nur den CODE anzeigen. <b>Es muss eine separate Web-Anwendung, AuthN_APP, entwickelt werden</b> um den Benutzer zur Eingabe des CODES, zum Erstellen und Öffnen der URL aufzufordern: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * Benutzer gibt Login/Passwort ein, die letzte Umleitung zeigt eine Erfolgsseite an

### Schritt 4: Prüfen auf authentifizierte Profile {#step-4-check-for-authenticated-profiles}

Der Programmierdienst prüft, ob die Authentifizierung mit MVPD im Browser oder auf dem zweiten Bildschirm abgeschlossen ist

* Es wird empfohlen, im <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br> alle 15 Sekunden abzurufen
([Abrufen authentifizierter Profile für bestimmte MVPD](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Wenn in der Streaming-Anwendung keine MVPD ausgewählt wird, da die MVPD-Auswahl im zweiten Bildschirmprogramm angezeigt wird, sollte die Abfrage mit CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br> erfolgen
([Abrufen authentifizierter Profile für bestimmten CODE](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* Der Abruf sollte 30 Minuten nicht überschreiten. Falls 30 Minuten erreicht werden und die Streaming-Anwendung noch aktiv ist, muss eine neue Sitzung initiiert und ein neuer CODE und eine neue URL zurückgegeben werden
* Nach Abschluss der Authentifizierung wird 200 mit authentifiziertem Profil zurückgegeben
* Der Programmierdienst kann zu <a href="#preauthorization-phase">C. Vorautorisierungsphase</a>

## C. Phase vor der Autorisierung {#preauthorization-phase}

### Schritt 5: Prüfen auf vorautorisierte Ressourcen {#step-5-check-for-preauthorized-resources}

Bei einem gültigen Authentifizierungsprofil für einen Benutzer hat der Programmierer-Service die Möglichkeit, den Zugriff auf die verfügbaren Videos zu überprüfen und die Liste zur Anzeige an die Streaming-Anwendung zu übergeben.

* Dieser Schritt ist optional und wird ausgeführt, wenn die Anwendung die Ressourcen herausfiltern möchte, die im authentifizierten Benutzerpaket nicht verfügbar sind
* Aufruf an <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
([Abrufen einer Vorabautorisierungsentscheidung mit einer bestimmten MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Genehmigungsphase {#authorization-phase}

### Schritt 6: Prüfen auf autorisierte Ressourcen {#step-6-check-for-authorized-resources}

Die Streaming-App bereitet die Wiedergabe eines Videos/Assets/einer Ressource vor, das/die vom Benutzer ausgewählt wurde.

* Dieser Schritt ist für jeden Spielstart notwendig
* Die Streaming-App übergibt diese Informationen an den Programmierer-Service
* Der Programmierer-Service ruft im Namen der Streaming-App <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br> auf
([Abrufen einer Autorisierungsentscheidung mit einer bestimmten MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * decision = „Allow“ (Berechtigung), weist der Programmierer-Service die Streaming-App an, das Streaming zu starten
   * decision = &#39;Ablehnen&#39;, weist der Programmierer-Service die Streaming-App an, den Benutzer darüber zu informieren, dass er keinen Zugriff auf dieses Video hat
   * Dabei kann der Programmierer-Service andere Geschäftsregeln bewerten und die entsprechende Entscheidung an die Streaming-App zurückgeben

## E. Abmeldephase {#logout-phase}

### Schritt 7: Abmelden {#step-7-logout}

Streaming-App: Der Benutzer möchte sich von der MVPD abmelden

* Die Streaming-App informiert den Programmierer-Service, dass er sich für diese spezifische App vom MVPD abmelden muss.
* Der Programmierdienst kann die gespeicherten Informationen über den authentifizierten Benutzer bereinigen
* Der Programmierdienst-Aufruf <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Starten des Abmeldens für bestimmte MVPD](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Wenn die Antwort actionType=&#39;interactive&#39; und url vorhanden ist, gibt der Programmierer-Service die URL an die Streaming-App zurück
* Basierend auf den vorhandenen Funktionen kann die Streaming-App die URL im Browser öffnen (in der Regel dieselbe URL wie für die Authentifizierung)
* Wenn die Streaming-App keinen Browser hat oder es sich um eine andere Instanz als die bei der Authentifizierung handelt, kann der Fluss angehalten werden, da die MVPD-Sitzung nicht im Browsercache gespeichert wurde.

## Umgebungen und funktionale Anforderungen{#environments}

Ein Programmierer sollte mindestens zwei Umgebungen erstellen: eine für die Produktion und eine oder mehrere für das Staging.

### Produktion

Die Produktionsumgebung sollte für große oder unerwartete Spitzen (z. B. Live-Sport, aktuelle Nachrichten) hochverfügbar sein und entsprechend skaliert werden.

Der Adobe Pass-Service läuft auf mehreren Rechenzentren, die über verschiedene Standorte in den USA verteilt sind. Um die beste Reaktionszeit (d. h. die niedrigste Latenz) des Adobe Pass-Service zu erzielen, sollte der Programmierer auch eine ähnliche geografisch verteilte Service-Infrastruktur erstellen.

Der Programmierdienst sollte den DNS-Cache auf maximal 30 Sekunden beschränken, falls Adobe Traffic umleiten muss. Dies kann vorkommen, wenn ein Rechenzentrum nicht mehr verfügbar ist.

Der Programmierer sollte den öffentlichen IP-Bereich der Produktionsumgebung bereitstellen. Diese werden in eine Zulassungsliste von IPs in der Adobe Pass-Infrastruktur für den Zugriff aufgenommen und von den Richtlinien für die faire API-Nutzung der Adobe verwaltet.

### Staging

Die Staging-Umgebung kann minimal sein, sollte jedoch alle Systemkomponenten und Geschäftslogik enthalten. Es sollte ähnlich wie die Produktion funktionieren und das Testen von Releases außerhalb der Produktion ermöglichen. Idealerweise kann die Staging-Umgebung mit den Adobe Pass-Testumgebungen verbunden werden, damit sie vom Programmierer bzw. der Programmiererin verwendet werden kann, und bei Bedarf durch Adobe. Auf diese Weise können wir beim Testen und bei der Fehlerbehebung helfen.

### Funktionale Anforderungen

Der Programmierdienst muss genaue Informationen zur Gerätekennung des Geräts weitergeben, für das er die Flüsse ausführt. Darüber hinaus muss der Programmierer-Service die IP des Geräts, für das er die Flüsse ausführt, (in einer Kopfzeile „x-forwarded-for„) zusammen mit dem Verbindungsquellen-Port (im Feld Geräteinformationen ) übergeben:

Der Programmierdienst sollte Daten und Formate senden, die von einzelnen MVPDs oder integrierten Apps benötigt werden (z. B. Geräte-IP, Quell-Port, Geräteinformationen, MRSS, optionale Daten wie ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

Der Programmierdienst muss die Gültigkeit von Authentifizierungsprofilen und Entscheidungen beim Zwischenspeichern respektieren und die Authentifizierungen oder Entscheidungen bei der Benachrichtigung ungültig machen.

Der Programmierdienst muss Zertifikate verwalten, die mit Adobe (für verschlüsselte Benutzermetadaten) freigegeben sind.

## Ergänzende Informationen {#related}

* [REST API v2-Referenz](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
