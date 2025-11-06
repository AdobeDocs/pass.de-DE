---
title: REST-API-Cookbook (Client-zu-Server)
description: REST-API-Cookbook-Client an Server.
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# (Legacy) REST-API-Cookbook (Client-zu-Server) {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Überblick {#overview}

Dieses Dokument enthält Schritt-für-Schritt-Anweisungen für das Entwicklungsteam eines Programmierers zur Integration eines „intelligenten Geräts“ (Spielkonsole, Smart-TV-App, Set-Top-Box usw.) in die Adobe Pass-Authentifizierung mithilfe von REST-API-Services. Dieser Client-zu-Server-Ansatz, bei dem REST-APIs anstelle einer Client-SDK verwendet werden, ermöglicht eine breitere Unterstützung verschiedener Plattformen, für die die Entwicklung einer erheblichen Anzahl eindeutiger SDKs nicht möglich wäre. Einen umfassenden technischen Überblick über die Funktionsweise der Clientless-Lösung finden Sie unter [Clientless-technische Übersicht](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md).


Dieser Ansatz erfordert zwei Komponenten (Streaming-App und AuthN-App), um die erforderlichen Flüsse abzuschließen: Start-, Registrierungs-, Autorisierungs- und View-Media-Flüsse in der Streaming-App und den Authentifizierungsfluss in Ihrer AuthN-App.

### Drosselmechanismus

Die Adobe Pass-Authentifizierungs-REST-API wird von einem [Einschränkungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gesteuert.

## Komponenten {#components}

In einer funktionierenden Client-zu-Server-Lösung sind die folgenden Komponenten beteiligt:



| Typ | Komponente | Beschreibung |
| --- | --- | --- |
| Streaming-Gerät | Streaming-App | Die Programmieranwendung, die sich auf dem Streaming-Gerät des Benutzers befindet und authentifizierte Videos abspielt. |
| | \[Optional\] Authentifizierungsmodul | Wenn das Streaming-Gerät über einen Benutzeragenten (d. h. einen Webbrowser) verfügt, ist das AuthN-Modul für die Authentifizierung des Benutzers auf der MVPD-ID verantwortlich. |
| \[Optional\] Authentifizierungs-Gerät | Authentifizierungs-App | Wenn das Streaming-Gerät keinen Benutzeragenten (d. h. Webbrowser) hat, ist die AuthN-Anwendung eine Programmierer-Webanwendung, auf die über einen Webbrowser von einem Gerät eines anderen Benutzers zugegriffen wird. |
| Adobe-Infrastruktur | Adobe Pass-Service | Ein Service, der mit dem MVPD IDp- und AuthZ-Service integriert ist und Authentifizierungs- und Autorisierungsentscheidungen bereitstellt. |
| MVPD-Infrastruktur | MVPD IDp | Ein MVPD-Endpunkt, der einen auf Berechtigungen basierenden Authentifizierungsdienst zur Überprüfung der Benutzeridentität bereitstellt. |
| | MVPD AuthZ-Service | Ein MVPD-Endpunkt, der Autorisierungsentscheidungen basierend auf Benutzerabonnements, Kindersicherung usw. bereitstellt. |

## Flows{#flows}

### Dynamische Client-Registrierung (DCR)

Adobe Pass verwendet DCR, um die Client-Kommunikation zwischen einer Programmieranwendung oder einem Server und den Adobe Pass-Services zu sichern. Der DCR-Ablauf ist separat und wird in der Dokumentation [Übersicht über die dynamische Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) beschrieben.


### Mobile-App-Flüsse für Streaming (intelligente Geräte)

![](../../../../assets/smart-device-app-flow.png)

#### Anlauffluss

1. Ihre App startet und lädt die anfängliche Benutzeroberfläche.

2. Geräte-ID abrufen/generieren.

3. Führen Sie einen Authentifizierungsaufruf aus, um zu sehen, ob das Gerät bereits authentifiziert ist.  Beispiel: [`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

4. Wenn der `checkauthn`-Aufruf erfolgreich ist, fahren Sie mit Schritt 2 fort.  Wenn dies fehlschlägt, starten Sie den Registrierungsfluss.



#### Registrierungsablauf

1. Rufen Sie einen Registrierungs-Code und eine URL für Ihren Benutzer ab, um auf Ihre Anmeldeanwendung für den zweiten Bildschirm zuzugreifen, und präsentieren Sie diese dem Benutzer:

   a. Senden Sie eine POST-Anfrage an den Registrierungs-Code-Service von Adobe und übergeben Sie eine gehashte Geräte-ID und eine „Registrierungs-URL“.  Beispiel: [`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)

   b. Zeigen Sie dem Benutzer den zurückgegebenen Registrierungs-Code und die URL an.

   c. Weisen Sie den Benutzer an, zu einem Web-fähigen Gerät zu wechseln, zur URL zu navigieren und dann den Registrierungs-Code einzugeben.



#### Autorisierungsfluss

1. Der Benutzer kehrt von der App für den zweiten Bildschirm zurück und drückt auf Ihrem Gerät auf die Schaltfläche „Weiter“. Alternativ können Sie einen Abrufmechanismus implementieren, um den Authentifizierungsstatus zu überprüfen. Die Adobe Pass-Authentifizierung empfiehlt jedoch die Schaltflächenmethode Weiter , anstatt den Abruf durchzuführen. <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)--> Beispiel: [\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)

2. Senden Sie eine GET-Anfrage an den Autorisierungs-Service für die Adobe Pass-Authentifizierung, um die Autorisierung zu initiieren. Beispiel: `<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* Wenn die Antwort auf Erfolg hinweist: Der Benutzer verfügt über ein gültiges AuthZ-Token UND der Benutzer ist berechtigt, die angeforderten Medien zu sehen (für diesen Benutzer gibt es ein gültiges AuthZ-Token).

* Wenn die Antwort auf einen Fehler hinweist: Untersuchen Sie die ausgelöste Ausnahme, um ihren Typ (AuthN, AuthZ oder etwas Anderes) zu bestimmen:

   * Wenn es sich um einen AuthN-Fehler handelte, starten Sie den Registrierungsfluss neu.

   * Wenn es sich um einen AuthZ-Fehler handelte, ist der Benutzer nicht berechtigt, die angeforderten Medien anzusehen, und dem Benutzer sollte eine Fehlermeldung angezeigt werden.

   * Wenn ein anderer Fehler aufgetreten ist (Verbindungsfehler, Netzwerkfehler usw.), zeigen Sie dem Benutzer eine entsprechende Fehlermeldung an.



#### Medienfluss anzeigen

1. Präsentieren Sie Medienoptionen. Der Benutzer wählt die anzuzeigenden Medien aus.

2. Sind die Medien geschützt?

   a. Ihre App prüft, ob die Medien geschützt sind.

   b. Wenn das Medium geschützt ist, startet Ihre App die Autorisierung
(AuthZ) Fluss oben.

   c. Wenn das Medium nicht geschützt ist, geben Sie das Medium für das
Benutzer.

3. Wiedergabe von Medien.


### AuthN-Programmfluss (2. Bildschirm)

![](../../../../assets/secnd-screen-authn-flow.png)

1. Erstellt eine Liste der MVPDs für diesen Benutzer. Beispiel: [`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)

1. Initiieren Sie den Authentifizierungsfluss.  Beispiel: [`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)

1. Überprüfen Sie, ob die Authentifizierung erfolgreich war. Beispiel:[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

1. Senden Sie den Benutzer zurück an Ihre Smartphone-App, um den Autorisierungsfluss abzuschließen.

## Partner-Single-Sign-on {#partner-sso}

Einige Geräte bieten dedizierte Unterstützung für Partner Single Sign-On (SSO):

* [APPLE SSO](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)

## Platform Single Sign-On {#platform-sso}

Einige Geräte bieten dedizierte Unterstützung für Platform Single Sign-On (SSO):

* [AMAZON SSO](../../sso-access/amazon-sso-cookbook-rest-api-v1.md)

## TempPass und Promotional TempPass für REST-API {#temppass}

Bei TempPass- und Promotion-TempPass-Implementierungen, bei denen der Benutzer keine Anmeldeinformationen eingeben muss, kann die Authentifizierung direkt in der Streaming-App implementiert werden.

**Um diese API verwenden zu können, muss die Streaming-App sicherstellen, dass die Geräte-ID, da sie für die Identifizierung des Tokens verwendet wird, zusammen mit den optionalen zusätzlichen Daten eindeutig ist.**


![](../../../../assets/temp-pass-promo-temppass.png)
