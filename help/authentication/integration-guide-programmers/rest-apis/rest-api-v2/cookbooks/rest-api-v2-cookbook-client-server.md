---
title: REST API V2-Cookbook (Client-zu-Server)
description: REST API V2-Cookbook (Client-zu-Server)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# REST API V2-Cookbook (Client-zu-Server) {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

## Schritte zum Implementieren der REST-API V2 in Client-seitigen Anwendungen {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Um die Adobe Pass REST API V2 zu implementieren, müssen Sie die folgenden Schritte ausführen, die in Phasen gruppiert sind.

## A. Phase der Registrierung {#registration-phase}

### Schritt 1: Registrieren des Programms {#step-1-register-your-application}

Damit die Anwendung Adobe Pass REST API V2 aufrufen kann, benötigt sie ein Zugriffstoken, das von der API-Sicherheitsebene benötigt wird.

Um das Zugriffs-Token zu erhalten, muss die Anwendung die Schritte ausführen, die in der Dokumentation [Dynamische Client-Registrierung](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben werden.

## B. Authentifizierungsphase {#authentication-phase}

### Schritt 2: Überprüfen auf vorhandene authentifizierte Profile {#step-2-check-for-existing-authenticated-profiles}

Streaming-Anwendungsprüfungen auf vorhandene authentifizierte Profile: <b>/api/v2/{serviceProvider}/profiles</b><br>
([Authentifizierte Profile abrufen](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Wenn kein Profil gefunden wird und die Streaming-Anwendung einen TempPass-Fluss implementiert
   * Dokumentation zur Implementierung von [Temporären Zugriffsflüssen](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Wenn kein Profil gefunden wird und die Streaming-Anwendung einen Authentifizierungsfluss implementiert
   * Die Streaming-Anwendung ruft die Liste der für den ServiceProvider verfügbaren MVPDs ab: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Liste der verfügbaren MVPDs ](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Streaming-Anwendungen können eine Filterung der Liste von MVPDs implementieren und nur MVPDs anzeigen, die beim Ausblenden anderer vorgesehen sind (TempPass, Test-MVPDs, MVPDs in Entwicklung usw.).
   * Die Streaming-Anwendung zeigt die Auswahl an. Der Benutzer wählt die MVPD aus.
   * Die Streaming-Anwendung erstellt eine Sitzung: <b>/api/v2/{serviceProvider}/sessions</b><br>
([Authentifizierungssitzung erstellen](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * Ein Code und eine URL für die Authentifizierung werden zurückgegeben
      * Wenn ein Profil gefunden wird, kann die Streaming-Anwendung mit <a href="#preauthorization-phase">C fortfahren. Vorautorisierungsphase</a> oder <a href="#authorization-phase">. Genehmigungsphase</a>

### Schritt 3: Benutzer authentifizieren {#step-3-authenticate-the-user}

Verwenden eines Browsers oder einer Web-basierten Second-Screen-Anwendung:

* Option 1. Streaming-Anwendungen können einen Browser oder eine Webansicht öffnen, die zu authentifizierende URL laden und den Benutzer auf die Anmeldeseite von MVPD bringen, auf der Anmeldeinformationen übermittelt werden müssen
   * Benutzer gibt Login/Passwort ein, die letzte Umleitung zeigt eine Erfolgsseite an
* Option 2. Streaming-Anwendung kann einen Browser nicht öffnen und nur den CODE anzeigen. <b>Es muss eine separate Web-Anwendung entwickelt werden</b> um den Benutzer zur Eingabe des CODES, zum Erstellen und Öffnen der URL aufzufordern: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * Benutzer gibt Login/Passwort ein, die letzte Umleitung zeigt eine Erfolgsseite an

### Schritt 4: Prüfen auf authentifizierte Profile {#step-4-check-for-authenticated-profiles}

Streaming-Anwendung prüft, ob Authentifizierung mit MVPD im Browser oder auf dem zweiten Bildschirm abgeschlossen ist

* Es wird empfohlen, im <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br> alle 15 Sekunden abzurufen
([Abrufen authentifizierter Profile für bestimmte MVPD](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Wenn in der Streaming-Anwendung keine MVPD ausgewählt wird, da die MVPD-Auswahl im zweiten Bildschirmprogramm angezeigt wird, sollte die Abfrage mit CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br> erfolgen
([Abrufen authentifizierter Profile für bestimmten CODE](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* Der Abruf sollte 30 Minuten nicht überschreiten. Falls 30 Minuten erreicht werden und die Streaming-Anwendung noch aktiv ist, muss eine neue Sitzung initiiert und ein neuer CODE und eine neue URL zurückgegeben werden
* Nach Abschluss der Authentifizierung wird 200 mit authentifiziertem Profil zurückgegeben
* Die Streaming-Anwendung kann zu <a href="#preauthorization-phase">C. Vorautorisierungsphase</a> oder <a href="#authorization-phase">. Genehmigungsphase</a>

## C. Phase vor der Autorisierung {#preauthorization-phase}

### Schritt 5: Prüfen auf vorautorisierte Ressourcen {#step-5-check-for-preauthorized-resources}

Die Streaming-Anwendung bereitet die Anzeige der Videos vor, die für den authentifizierten Benutzer verfügbar sind, und hat die Möglichkeit, die
Zugriff auf diese Ressourcen.

* Dieser Schritt ist optional und wird ausgeführt, wenn die Anwendung die Ressourcen herausfiltern möchte, die im authentifizierten Benutzerpaket nicht verfügbar sind
* Aufruf an <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
([Abrufen einer Vorabautorisierungsentscheidung mit einer bestimmten MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Genehmigungsphase {#authorization-phase}

### Schritt 6: Prüfen auf autorisierte Ressourcen {#step-6-check-for-authorized-resources}

Die Streaming-Anwendung bereitet die Wiedergabe eines vom Benutzer ausgewählten Videos/Assets/einer vom Benutzer ausgewählten Ressource vor.

* Dieser Schritt ist für jeden Spielstart notwendig
* Call <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Abrufen einer Autorisierungsentscheidung mit einer bestimmten MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * Entscheidung = „Zulassen“, Streaming-Gerät startet
   * Entscheidung = „Ablehnen“, das Streaming-Gerät informiert den Benutzer darüber, dass er keinen Zugriff auf dieses Video hat

## E. Abmeldephase {#logout-phase}

### Schritt 7: Abmelden {#step-7-logout}

Streaming-Gerät: Der Benutzer möchte sich von der MVPD abmelden

* Aufruf <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Starten des Abmeldens für bestimmte MVPD](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Wenn die Antwort actionType=&#39;interactive&#39; und url vorhanden ist, öffnen Sie die URL in einem Browser/zweiten Bildschirm, um die Abmeldung mit MVPD abzuschließen
