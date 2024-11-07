---
title: REST API V2-Cookbook (Server-zu-Server)
description: REST API V2-Cookbook (Server-zu-Server)
source-git-commit: c17e52dd52fa14c50d59945598d1913f02be2468
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---

# REST API V2-Cookbook (Server-zu-Server) {#rest-api-v2-cookbook-server-to-server}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Übersicht {#overview}

In diesem Cookbook-Dokument werden Best Practices für die Implementierung der Adobe Pass-Authentifizierung mit der REST-API V2 in einer Server-zu-Server-Architektur beschrieben.  Es bietet grundlegende Anforderungen, eine schrittweise Flussimplementierung und allgemeine Überlegungen für Produktionsumgebungen und -vorgänge.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.


## Komponenten {#components}

In einer funktionierenden Server-zu-Server-Lösung sind die folgenden Komponenten involviert:


| Typ | Komponente | Beschreibung |
| --- | --- | --- |
| Streaming-Gerät | Streaming-App | Die Programmeranwendung, die sich auf dem Streaming-Gerät des Benutzers befindet und authentifizierte Videos wiedergibt. |
| | \[Optional\] AuthN-Modul | Wenn das Streaming-Gerät über einen Benutzeragenten (d. h. einen Webbrowser) verfügt, ist das AuthN-Modul für die Authentifizierung des Benutzers auf dem MVPD IdP verantwortlich. |
| \[Optional\] AuthN-Gerät | AuthN App | Wenn das Streaming-Gerät über keinen Benutzeragenten (d. h. einen Webbrowser) verfügt, ist die AuthN-Anwendung eine Programmierer-Webanwendung, auf die über einen Webbrowser von einem Gerät eines anderen Benutzers aus zugegriffen wird. |
| Programmierungsinfrastruktur | Programmiererdienst | Ein Dienst, der das Streaming-Gerät mit dem Adobe Pass-Dienst verknüpft, um Authentifizierungs- und Autorisierungsentscheidungen zu erhalten. |
| Adobe-Infrastruktur | Adobe Pass-Dienst | Ein Dienst, der in den MVPD IdP- und AuthZ-Dienst integriert wird und Authentifizierungs- und Autorisierungsentscheidungen ermöglicht. |
| MVPD-Infrastruktur | MVPD IdP | Ein MVPD-Endpunkt, der einen auf Anmeldedaten basierenden Authentifizierungsdienst bereitstellt, um die Identität des Benutzers zu überprüfen. |
| | MVPD AuthZ-Dienst | Ein MVPD-Endpunkt, der Autorisierungsentscheidungen basierend auf den Abonnements des Benutzers, elterlichen Steuerelementen usw. bereitstellt. |


Weitere im Fluss verwendete Begriffe werden im Abschnitt
[Glossar](/help/authentication/glossary.md).

Das folgende Diagramm zeigt den gesamten Fluss:

![](/help/authentication/assets/rest-api-v2/cookbooks/apass-servertoserver-cookbook.png)

### Schritte zur Implementierung der REST API V2 in der Server-zu-Server-Architektur {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Um die Adobe Pass REST API V2 zu implementieren, müssen Sie die folgenden Schritte in mehrere Phasen unterteilen.

## 0. Voraussetzungen {#prerequisites}

Bei Server-zu-Server-Implementierungen muss die Streaming-App und der Programmiererdienst ein Protokoll erstellen, damit Programmierer-Service:
* eindeutige Identifizierung der Streaming-App auf dem Gerät
* handeln Sie im Namen der Streaming-App und kommunizieren Sie mit dem Adobe Pass-Dienst
* Informationen über die Streaming-App und das Gerät wie IP-Adresse, Quellanschluss und Geräteinformationen erfassen und speichern, um sie an Adobe Pass weiterzuleiten.
* Zurückgeben von Entscheidungen und Anweisungen an die Streaming-App

Parameter wie Geräte-ID, Client-ID und Client-Geheimnis (definiert unten) können entweder in der Streaming-App oder im Programmiererdienst gespeichert werden.

## A. Registrierungsphase {#registration-phase}

### Schritt 1: App registrieren {#step-1-register-your-application}

Damit die Anwendung die Adobe Pass REST API V2 aufrufen kann, benötigt sie ein Zugriffstoken, das von der API-Sicherheitsschicht benötigt wird. Bei Server-zu-Server-Implementierungen kann sich der Programmiererdienst im Namen einer Anwendungsinstanz registrieren.
Die Client-ID und die Client-Geheimwerte müssen für jedes Streaming-Gerät abgerufen werden.

Um das Zugriffs-Token zu erhalten, kann der Programmierer-Dienst im Namen einer Streaming-App agieren und muss die folgenden Schritte ausführen: [Dynamische Client-Registrierung](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B. Authentifizierungsphase {#authentication-phase}

### Schritt 2: Auf vorhandene authentifizierte Profile überprüfen {#step-2-check-for-existing-authenticated-profiles}

Der Programmiererdienst prüft im Namen der Streaming-App nach vorhandenen authentifizierten Profilen: `/api/v2/{serviceProvider}/profiles` ([Abrufen authentifizierter Profile](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Wenn kein Profil gefunden wurde und die Streaming-Anwendung einen TempPass-Fluss implementiert
   * Befolgen Sie die Dokumentation zur Implementierung von [temporären Zugriffsflüssen](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).
* Wenn kein Profil gefunden wurde, implementiert die Streaming-Anwendung einen Authentifizierungsfluss
   * <b>Schritt 2.a:</b> Der Programmiererdienst ruft die Liste der für serviceProvider verfügbaren MVPDs ab: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Liste der verfügbaren MVPDs abrufen](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Der Programmiererdienst kann die Filterung der Liste der MVPDs implementieren und nur MVPDs anzeigen, die zum Verstecken anderer bestimmt sind (TempPass, MVPDs testen, MVPDs in der Entwicklung usw.).
   * Wenn der Programmiererdienst eine gefilterte MVPD-Liste zurückgeben soll, damit die Streaming-App die Auswahl anzeigt, wählt der Benutzer das MVPD aus
   * Wenn der MVPD aus der Streaming-App ausgewählt wurde, erstellt der Programmiererdienst eine Sitzung: <b>/api/v2/{serviceProvider}/sessions</b><br>
([Erstellen einer Authentifizierungssitzung](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * ein CODE und eine URL zurückgegeben, die für die Authentifizierung verwendet werden sollen
      * Wenn ein Profil gefunden wird, kann der Programmiererdienst zu <a href="#preauthorization-phase">C wechseln. Vorautorisierungsphase </a>
   * Der Programmiererdienst sollte den CODE und die URL zur Streaming-App zurückgeben

### Schritt 3: Benutzer authentifizieren {#step-3-authenticate-the-user}

Verwenden eines Browsers oder einer Web-basierten Second Screen-Anwendung:

* Option 1. Die Streaming-App kann einen Browser oder eine Webansicht öffnen, die zu authentifizierende URL laden und der Benutzer landet auf der MVPD-Anmeldeseite, auf der die Anmeldeinformationen gesendet werden müssen
   * Benutzer eingeben Login/Passwort, endgültige Umleitung anzeigen Erfolgsseite
* Option 2. Die Streaming-App kann einen Browser nicht öffnen und nur den CODE anzeigen. <b>Eine separate Webanwendung, AuthN_APP, muss entwickelt werden</b>, um den Benutzer aufzufordern, CODE einzugeben, URL zu erstellen und zu öffnen: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * Benutzer eingeben Login/Passwort, endgültige Umleitung anzeigen Erfolgsseite

### Schritt 4: Auf authentifizierte Profile überprüfen {#step-4-check-for-authenticated-profiles}

Der Programmiererdienst prüft, ob die Authentifizierung mit MVPD im Browser oder im zweiten Bildschirm abgeschlossen ist.

* Es wird empfohlen, alle 15 Sekunden auf <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br> abzurufen.
([Abrufen authentifizierter Profile für bestimmte MVPD](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Wenn in der Streaming-Anwendung keine MVPD-Auswahl vorgenommen wird, da die MVPD-Auswahl in der Anwendung &quot;Second Screen&quot;angezeigt wird, sollte die Abfrage mit CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br> erfolgen
([Abrufen authentifizierter Profile für bestimmten CODE](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* Der Abruf sollte 30 Minuten nicht überschreiten, falls 30 Minuten erreicht werden und die Streaming-Anwendung weiterhin aktiv ist, eine neue Sitzung initiiert und ein neuer CODE und eine neue URL zurückgegeben werden.
* Wenn die Authentifizierung abgeschlossen ist, ist die Rückgabe 200 mit authentifiziertem Profil.
* Der Programmiererdienst kann mit <a href="#preauthorization-phase">C fortfahren. Vorautorisierungsphase </a>

## C. Phase der Vorabgenehmigung {#preauthorization-phase}

### Schritt 5: Auf vorab autorisierte Ressourcen prüfen {#step-5-check-for-preauthorized-resources}

Mit einem gültigen Authentifizierungsprofil für einen Benutzer kann der Programmiererdienst die
auf die verfügbaren Videos zugreifen und die Liste an die anzuzeigende Streaming-Anwendung weitergeben.

* Schritt ist optional und wird ausgeführt, wenn die Anwendung die Ressourcen filtern möchte, die im authentifizierten Benutzerpaket nicht verfügbar sind
* Aufruf an <b>/api/v2/{serviceProvider}/decision/preauthorize/{mvpd}</b><br>
([Vorabautorisierungsentscheidung mit einem bestimmten MVPD abrufen](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Genehmigungsphase {#authorization-phase}

### Schritt 6: Auf autorisierte Ressourcen prüfen {#step-6-check-for-authorized-resources}

Die Streaming-App bereitet die Wiedergabe eines Videos/Assets/einer Ressource vor, die vom Benutzer ausgewählt wurde.

* Schritt für jeden Start der Wiedergabe erforderlich
* Streaming-App übergibt diese Informationen an den Programmierer-Dienst
* Rufen Sie den Programmiererdienst für die Streaming-App auf: <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Abrufen einer Autorisierungsentscheidung mit einem bestimmten MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * decision = &#39;Permit&#39; , weist der Programmiererdienst die Streaming-App an, Streaming zu starten.
   * decision = &#39;Deny&#39;, Programmier-Dienst weist die Streaming-App an, den Benutzer darüber zu informieren, dass er keinen Zugriff auf dieses Video hat
   * im Prozess kann der Programmiererdienst andere Geschäftsregeln bewerten und geeignete Entscheidungen an die Streaming-App zurückgeben

## E. Abmeldephase {#logout-phase}

### Schritt 7: Abmelden {#step-7-logout}

Streaming-App: Benutzer möchte sich vom MVPD abmelden

* Die Streaming-App informiert den Programmierer-Service, dass er sich für diese bestimmte App aus dem MVPD abmelden muss.
* Der Programmiererdienst kann die Informationen bereinigen, die er über den authentifizierten Benutzer speichert.
* Aufruf des Programmiererdiensts <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Initiate logout for specific MVPD](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Wenn die Antwort actionType=&#39;interactive&#39; und url vorhanden ist, gibt der Programmiererdienst die URL an die Streaming-App zurück
* Basierend auf vorhandenen Funktionen kann die Streaming-App die URL im Browser öffnen (normalerweise die URL, die für die Authentifizierung verwendet wird).
* Wenn die Streaming-App keinen Browser hat oder es sich um eine andere Instanz als die bei der Authentifizierung handelt, kann der Fluss gestoppt werden, da die MVPD-Sitzung nicht im Browser-Cache beibehalten wurde.

## Umgebungen und funktionale Anforderungen{#environments}

Ein Programmierer sollte mindestens zwei Umgebungen erstellen: eine für die Produktion und eine oder mehrere für die Staging-Umgebung.


### Produktion

Die Produktionsumgebung sollte in hohem Maße verfügbar sein und für große oder unerwartete Spitzen (z. B. Live-Sportarten, Unterbrechungen der
News).



Der Adobe Pass-Dienst wird auf mehreren Rechenzentren ausgeführt, die geografisch in den USA verteilt sind.  Um die beste Reaktionszeit (d. h. die niedrigste Latenz) für den Adobe Pass-Dienst zu erreichen, sollte der Programmierer auch einen ähnlich geografisch verteilten Dienst erstellen
Infrastruktur.


Der Programmiererdienst sollte den DNS-Cache auf maximal 30 Sekunden begrenzen, falls Adobe Traffic neu ausführen muss. Dies kann vorkommen, wenn ein Rechenzentrum nicht mehr verfügbar ist.


Der Programmierer sollte den öffentlichen IP-Bereich der Produktionsumgebung bereitstellen. Diese IP-Adressen werden in der Adobe Pass-Infrastruktur in eine Zulassungsliste aufgenommen, in der der Zugriff und die Verwaltung durch die Nutzungsrichtlinien der Adobe Fair API möglich sind.

### Staging

Die Staging-Umgebung kann minimal sein, sollte jedoch alle Systemkomponenten und Geschäftslogik enthalten. Sie sollte ähnlich wie die Produktion funktionieren und Testversionen außerhalb der Produktion ermöglichen. Idealerweise kann die Staging-Umgebung mit den Adobe Pass-Testumgebungen verbunden werden, die vom Programmierer und bei Bedarf vom Adobe verwendet werden können, damit wir bei Tests und Fehlerbehebung helfen können.

### Funktionale Anforderungen

Der Programmiererdienst muss genaue Geräteridentifizierungsinformationen für das Gerät übermitteln, für das er die Datenflüsse ausführt. Darüber hinaus muss der Programmiererdienst die IP des Geräts, für das er die Flüsse ausführt (in einem x-forwarded-for-Header) zusammen mit dem Verbindungsquellenanschluss (im Feld für Geräteinformationen) übergeben:

Der Programmiererdienst sollte Daten und Formate senden, die von einzelnen MVPDs oder integrierten Apps benötigt werden (z. B. Geräte-IP, Quellport, Geräteinformationen, MRSS, optionale Daten wie ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->

Der Programmiererdienst muss beim Zwischenspeichern das Authentifizierungsprofil und die Gültigkeit von Entscheidungen beachten und die Authentifizierungen oder Entscheidungen bei der Benachrichtigung ungültig machen.

Der Programmiererdienst muss für Adobe freigegebene Zertifikate (für verschlüsselte Benutzermetadaten) aufbewahren.

## Verwandte Informationen {#related}

* [REST API V2-Referenz](/help/authentication/rest-api-v2/rest-api-v2-overview.md)
