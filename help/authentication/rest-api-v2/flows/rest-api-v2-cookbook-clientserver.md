---
title: REST API V2 - Cookbook - Schritte zur Client-/Server-Implementierung
description: REST API V2 - Cookbook - Schritte zur Client-/Server-Implementierung
source-git-commit: 5ba888b35731ef64b3dd1878f2fa55083989f857
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# REST API V2 - Cookbook - Schritte zur Client-/Server-Implementierung {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

## Schritte zur Implementierung der REST API V2 in clientseitigen Anwendungen {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Um die Adobe Pass REST API V2 zu implementieren, müssen Sie die folgenden Schritte in mehrere Phasen unterteilen.

## A. Registrierungsphase {#registration-phase}

### Schritt 1: App registrieren {#step-1-register-your-application}
Damit die Anwendung die Adobe Pass REST API V2 aufrufen kann, benötigt sie ein Zugriffstoken, das von der API-Sicherheitsschicht benötigt wird.
Um das Zugriffs-Token zu erhalten, muss die Anwendung die folgenden Schritte ausführen:
[Dynamische Client-Registrierung](./dynamic-client-registration.md)

## B. Authentifizierungsphase {#authentication-phase}

### Schritt 2: Auf vorhandene authentifizierte Profile überprüfen {#step-2-check-for-existing-authenticated-profiles}
Prüfungen der Streaming-Anwendung auf vorhandene authentifizierte Profile: <b>/api/v2/{serviceProvider}/profiles</b><br>
([Abrufen authentifizierter Profile](./apis/profiles-apis/rest-api-v2-retrieve-authenticated-profiles.md))

* wenn kein Profil gefunden wurde und die Streaming-Anwendung einen TempPass-Fluss implementiert
   * Befolgen Sie die Dokumentation zur Implementierung von [temporären Zugriffsflüssen](./temporary-access-flows/rest-api-v2-access-temporary-flows.md).
* Wenn kein Profil gefunden wurde, implementiert die Streaming-Anwendung einen Authentifizierungsfluss.
   * Streaming-Anwendung ruft die Liste der für serviceProvider verfügbaren MVPDs ab: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Liste der verfügbaren MVPDs abrufen](./apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Die Streaming-Anwendung kann die Filterung der Liste der MVPDs implementieren und nur MVPDs anzeigen, die beim Ausblenden anderer MVPDs vorgesehen sind (TempPass, MVPDs testen, MVPDs in der Entwicklung usw.)
   * Anzeigenauswahl der Streaming-Anwendung, Benutzer wählt den MVPD aus
   * Streaming-Anwendung erstellt eine Sitzung: <b>/api/v2/{serviceProvider}/sessions</b><br>
([Erstellen einer Authentifizierungssitzung](./apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * ein CODE und eine URL zurückgegeben, die für die Authentifizierung verwendet werden sollen
      * Wenn ein Profil gefunden wird, kann die Streaming-Anwendung zu <a href="#preauthorization-phase">C wechseln. Vorabautorisierungsphase </a> oder <a href="#authorization-phase">D. Autorisierungsphase </a>

### Schritt 3: Benutzer authentifizieren {#step-3-authenticate-the-user}
Verwenden eines Browsers oder einer Web-basierten Second Screen-Anwendung:

* Option 1. Die Streaming-Anwendung kann einen Browser oder eine Webansicht öffnen, die zu authentifizierende URL laden und der Benutzer landet auf der MVPD-Anmeldeseite, auf der die Anmeldeinformationen gesendet werden müssen
   * Benutzer eingeben Login/Passwort, endgültige Umleitung anzeigen Erfolgsseite
* Option 2. Streaming-Anwendung kann einen Browser nicht öffnen und nur den CODE anzeigen. <b>Es muss eine separate Webanwendung entwickelt werden </b>, um den Benutzer aufzufordern, CODE einzugeben, URL zu erstellen und zu öffnen: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * Benutzer eingeben Login/Passwort, endgültige Umleitung anzeigen Erfolgsseite

### Schritt 4: Auf authentifizierte Profile überprüfen {#step-4-check-for-authenticated-profiles}
Die Streaming-Anwendung prüft, ob die Authentifizierung mit MVPD im Browser oder im zweiten Bildschirm abgeschlossen ist.

* Die Abfrage alle 15 Sekunden wird für <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br> empfohlen
([Abrufen authentifizierter Profile für bestimmte MVPD](.apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Wenn in der Streaming-Anwendung keine MVPD-Auswahl vorgenommen wird, da der MVPD-Wähler in der Anwendung &quot;Second Screen&quot;angezeigt wird, sollte die Abfrage mit CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br> erfolgen.
([Abrufen authentifizierter Profile für bestimmten CODE](./apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* Die Abfrage sollte 30 Minuten nicht überschreiten, falls 30 Minuten erreicht werden und die Streaming-Anwendung weiterhin aktiv ist, eine neue Sitzung initiiert und ein neuer CODE und eine neue URL zurückgegeben werden.
* Wenn die Authentifizierung abgeschlossen ist, ist die Rückgabe 200 mit authentifiziertem Profil.
* Die Streaming-Anwendung kann zu <a href="#preauthorization-phase">C wechseln. Vorabautorisierungsphase </a> oder <a href="#authorization-phase">D. Autorisierungsphase </a>

## C. Phase der Vorabgenehmigung {#preauthorization-phase}

### Schritt 5: Auf vorab autorisierte Ressourcen prüfen {#step-5-check-for-preauthorized-resources}
Die Streaming-Anwendung bereitet sich darauf vor, die für den authentifizierten Benutzer verfügbaren Videos anzuzeigen, und hat die Möglichkeit, die
Zugriff auf diese Ressourcen.
* Dieser Schritt ist optional und wird ausgeführt, wenn die Anwendung die Ressourcen filtern möchte, die im authentifizierten Benutzerpaket nicht verfügbar sind
* Aufruf an <b>/api/v2/{serviceProvider}/entscheidungen/preauthorize/{mvpd}</b><br>
([Vorabautorisierungsentscheidung mit einem bestimmten MVPD abrufen](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))


## D. Genehmigungsphase {#authorization-phase}

### Schritt 6: Auf autorisierte Ressourcen prüfen {#step-6-check-for-authorized-resources}
Die Streaming-Anwendung bereitet die Wiedergabe eines Videos/Assets/einer Ressource vor, die vom Benutzer ausgewählt wurde.

* Schritt ist für jeden Start der Wiedergabe erforderlich
* call <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Abrufen einer Autorisierungsentscheidung mit einem bestimmten MVPD](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * decision = &#39;Permit&#39; , Streaming-Gerät startet Streaming
   * decision = &#39;Deny&#39;, das Streaming-Gerät informiert den Benutzer darüber, dass er keinen Zugriff auf dieses Video hat

## E. Abmeldephase {#logout-phase}

### Schritt 7: Abmelden {#step-7-logout}
Streaming-Gerät : Benutzer möchte sich vom MVPD abmelden

* call <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Initiate logout for specific MVPD](.apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Wenn die Antwort actionType=&#39;interactive&#39; und url vorhanden ist, öffnen Sie die URL in einem Browser/Zweiten Bildschirm, um die Abmeldung mit MVPD abzuschließen.

