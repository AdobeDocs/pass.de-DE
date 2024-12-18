---
title: REST API V2-Cookbook (Client-to-Server)
description: REST API V2-Cookbook (Client-to-Server)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# REST API V2-Cookbook (Client-to-Server) {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) begrenzt.

## Schritte zur Implementierung der REST API V2 in clientseitigen Anwendungen {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Um die Adobe Pass REST API V2 zu implementieren, müssen Sie die folgenden Schritte ausführen, die in Phasen unterteilt sind.

## A. Registrierungsphase {#registration-phase}

### Schritt 1: App registrieren {#step-1-register-your-application}

Damit die Anwendung die Adobe Pass REST API V2 aufrufen kann, benötigt sie ein Zugriffstoken, das von der API-Sicherheitsschicht benötigt wird.

Um das Zugriffs-Token zu erhalten, muss die Anwendung die in der Dokumentation [Dynamische Client-Registrierung](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschriebenen Schritte ausführen.

## B. Authentifizierungsphase {#authentication-phase}

### Schritt 2: Auf vorhandene authentifizierte Profile überprüfen {#step-2-check-for-existing-authenticated-profiles}

Prüfungen der Streaming-Anwendung auf vorhandene authentifizierte Profile: <b>/api/v2/{serviceProvider}/profiles</b><br>
([Abrufen authentifizierter Profile](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Wenn kein Profil gefunden wird und die Streaming-Anwendung einen TempPass-Fluss implementiert
   * Befolgen Sie die Dokumentation zur Implementierung von [temporären Zugriffsflüssen](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).
* Wenn kein Profil gefunden wird und die Streaming-Anwendung einen Authentifizierungsfluss implementiert
   * Die Streaming-Anwendung ruft die Liste der für serviceProvider verfügbaren MVPDs ab: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Liste der verfügbaren MVPDs abrufen](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Die Streaming-Anwendung kann die Filterung der Liste der MVPDs implementieren und nur MVPDs anzeigen, die beim Ausblenden anderer MVPDs vorgesehen sind (TempPass, MVPDs testen, MVPDs in der Entwicklung usw.)
   * Streaming-Anwendung zeigt Auswahl an, Benutzer wählt das MVPD aus
   * Streaming-Anwendung erstellt eine Sitzung: <b>/api/v2/{serviceProvider}/sessions</b><br>
([Erstellen einer Authentifizierungssitzung](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * ein CODE und eine URL zurückgegeben, die für die Authentifizierung verwendet werden sollen
      * Wenn ein Profil gefunden wird, kann die Streaming-Anwendung zu <a href="#preauthorization-phase">C wechseln. Vorabautorisierungsphase </a> oder <a href="#authorization-phase">D. Autorisierungsphase </a>

### Schritt 3: Benutzer authentifizieren {#step-3-authenticate-the-user}

Verwenden eines Browsers oder einer Web-basierten Second Screen-Anwendung:

* Option 1. Die Streaming-Anwendung kann einen Browser oder eine Webansicht öffnen, die zu authentifizierende URL laden und der Benutzer landet auf der MVPD-Anmeldeseite, auf der die Anmeldeinformationen gesendet werden müssen
   * Benutzer gibt Anmeldung/Kennwort ein, endgültige Umleitung zeigt eine Erfolgsseite an
* Option 2. Streaming-Anwendung kann einen Browser nicht öffnen und nur den CODE anzeigen. <b>Eine separate Webanwendung </b> muss entwickelt werden, um den Benutzer aufzufordern, CODE einzugeben, URL zu erstellen und zu öffnen: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * Benutzer gibt Anmeldung/Kennwort ein, endgültige Umleitung zeigt eine Erfolgsseite an

### Schritt 4: Auf authentifizierte Profile überprüfen {#step-4-check-for-authenticated-profiles}

Die Streaming-Anwendung prüft, ob die Authentifizierung mit MVPD im Browser oder im zweiten Bildschirm abgeschlossen ist.

* Es wird empfohlen, alle 15 Sekunden auf <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br> abzurufen.
([Abrufen authentifizierter Profile für bestimmte MVPD](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Wenn in der Streaming-Anwendung keine MVPD-Auswahl vorgenommen wird, da die MVPD-Auswahl in der Anwendung &quot;Second Screen&quot;angezeigt wird, sollte die Abfrage mit CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br> erfolgen
([Abrufen authentifizierter Profile für bestimmten CODE](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* Der Abruf sollte 30 Minuten nicht überschreiten, falls 30 Minuten erreicht werden und die Streaming-Anwendung weiterhin aktiv ist, eine neue Sitzung initiiert und ein neuer CODE und eine neue URL zurückgegeben werden.
* Wenn die Authentifizierung abgeschlossen ist, ist die Rückgabe 200 mit authentifiziertem Profil.
* Die Streaming-Anwendung kann mit <a href="#preauthorization-phase">C fortfahren. Vorabautorisierungsphase </a> oder <a href="#authorization-phase">D. Autorisierungsphase </a>

## C. Phase der Vorabgenehmigung {#preauthorization-phase}

### Schritt 5: Auf vorab autorisierte Ressourcen prüfen {#step-5-check-for-preauthorized-resources}

Die Streaming-Anwendung bereitet sich darauf vor, die für den authentifizierten Benutzer verfügbaren Videos anzuzeigen, und hat die Möglichkeit, die
Zugriff auf diese Ressourcen.

* Schritt ist optional und wird ausgeführt, wenn die Anwendung die Ressourcen herausfiltern möchte, die im authentifizierten Benutzerpaket nicht verfügbar sind
* Aufruf an <b>/api/v2/{serviceProvider}/decision/preauthorize/{mvpd}</b><br>
([Vorabautorisierungsentscheidung mit einem bestimmten MVPD abrufen](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Genehmigungsphase {#authorization-phase}

### Schritt 6: Auf autorisierte Ressourcen prüfen {#step-6-check-for-authorized-resources}

Die Streaming-Anwendung bereitet die Wiedergabe eines Videos/Assets/einer Ressource vor, die vom Benutzer ausgewählt wurde.

* Schritt für jeden Start der Wiedergabe erforderlich
* Rufen Sie <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br> auf.
([Abrufen einer Autorisierungsentscheidung mit einem bestimmten MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * decision = &#39;Permit&#39;, Streaming-Gerät startet Streaming
   * decision = &#39;Deny&#39;, das Streaming-Gerät informiert den Benutzer darüber, dass er keinen Zugriff auf dieses Video hat

## E. Abmeldephase {#logout-phase}

### Schritt 7: Abmelden {#step-7-logout}

Streaming-Gerät: Benutzer möchte sich aus dem MVPD abmelden

* Rufen Sie <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br> auf.
([Initiate logout for specific MVPD](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Wenn die Antwort actionType=&#39;interactive&#39; und url vorhanden ist, öffnen Sie die URL in einem Browser/Zweiten Bildschirm, um die Abmeldung mit MVPD abzuschließen.
