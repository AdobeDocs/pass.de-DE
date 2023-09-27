---
title: REST-API-Referenz
description: REST-API-Referenz
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 4%

---

# REST-API-Referenz {#rest-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Antwortformate {#response-formats}


>[!NOTE]
>
> Die in diesen Diensten bereitgestellten APIs können Antworten in XML oder JSON zurückgeben (für APIs, die eine Antwort zurückgeben). Es gibt 3 verschiedene Möglichkeiten, das Antwortformat in der Anfrage anzugeben:
>
>* Setzen Sie HTTP Accept Header auf `application/xml` oder `application/json`.
>* Geben Sie in der Anfrage-Payload den Parameter an `format=xml` oder `format=json`.
>* Aufrufen des Webdienst-Endpunkts mit Erweiterung `.xml` oder `.json`. Beispiel: `/regcode.xml` oder `/regcode.json`
>
>Sie können eine der oben genannten Methoden angeben. Das Angeben mehrerer Methoden mit widersprüchlichen Formaten kann zu Fehlern oder unerwünschten Ausgaben führen.

## REST-API-Endpunkte {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Web Services - Zusammenfassung {#web_srvs_summary}

In der folgenden Tabelle sind die verfügbaren Webdienste für den clientlosen Ansatz aufgeführt. Klicken Sie auf die Webdienst-Endpunkte, um weitere Informationen zu erhalten (Beispielanfrage und -antwort, Eingabeparameter, HTTP-Methoden usw.).


| Sr | Web Service Endpoint | Beschreibung | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | gehostet bei | aufgerufen von |
| --- | --- | --- | --- | --- | --- |
| 1. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode](/help/authentication/registration-code-request.md) | Gibt zufällig generierten Registrierungs-Code und Anmeldeseiten-URI zurück | 2 | Adobe  </br>Reg Code-Dienst | Smart Device |
| 2. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/return-registration-record.md) | Gibt den Registrierungs-Code-Datensatz mit Registrierungs-Code-UUID, Registrierungs-Code und Hash-Geräte-ID zurück | 8 | Adobe  </br>Reg Code-Dienst | Adobe Pass-Authentifizierung |
| 3. | [&lt;sp_fqdn>/api/v1/config/  </br>  {requestorId}](/help/authentication/provide-mvpd-list.md) | Gibt eine Liste der konfigurierten MVPDs für den Anforderer zurück | 5 | Adobe  </br>Adobe Pass  </br>Authentifizierung  </br>Dienst | Anmelden  </br>Web  </br>App |
| 4. | [&lt;sp_fqdn>/api/v1/authenticate](/help/authentication/initiate-authentication.md) | Initiiert den AuthN-Prozess durch Information zum MVPD-Auswahlereignis. Erstellt einen Datensatz in der Authentifizierungsdatenbank, der abgestimmt wird, wenn eine erfolgreiche Antwort von MVPD empfangen wird (Schritt 13) | 7 | Adobe  </br>Adobe Pass  </br>Authentifizierung  </br>Dienst | Anmelden  </br>Web  </br>App |
| 5. | SAML Assertion Consumer | Vorhandener SAML-Workflow zwischen Adobe Pass-Authentifizierung und MVPD | 13 | Adobe Pass  </br>Authentifizierung  </br>Dienst | Adobe Pass-Authentifizierung |
| 6. | [&lt;sp_fqdn>/api/v1/checkauthn/  </br>  {registrationCode}](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | Die Web-Anwendung &quot;Anmeldung&quot;kann prüfen, ob der versucht wurde, den Anmeldevorgang erfolgreich durchzuführen |     | Adobe Pass  </br>Authentifizierung   </br>Dienst | Anmelden   </br>Web   </br>App |
| 7. | [&lt;sp_fqdn>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Ruft AuthN-Token-bezogene Metadaten ab | 15 | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |
| 8. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/delete-registration-record.md) | Löscht den Reg-Codedatensatz und gibt den Reg-Code zur Wiederverwendung frei | 16 | Adobe  </br>Reg Code-Dienst | Adobe Pass-Authentifizierung |
| 9. | [&lt;sp_fqdn>/api/v1/authorize](/help/authentication/initiate-authorization.md) | Erhält die Antwort auf die Autorisierung. | 17 | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |
| 10. | [&lt;sp_fqdn>/api/v1/checkauthin](/help/authentication/check-authentication-token.md) | Gibt an, ob das Gerät über ein nicht abgelaufenes AuthN-Token verfügt. |     | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |
| 11. | [&lt;sp_fqdn>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Gibt das AuthN-Token zurück, falls gefunden. |     | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |
| 12. | [&lt;sp_fqdn>/api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | Gibt das AuthZ-Token zurück, falls gefunden. |     | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |
| 13. | [&lt;sp_fqdn>/api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | Gibt das Short Media Token aus, falls vorhanden - identisch mit /api/v1/mediatoken |     | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |
| 14. | [&lt;sp_fqdn>/api/v1/mediatoken](/help/authentication/obtain-short-media-token.md) | Erhält Short Media Token |     | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |
| 15. | [&lt;sp_fqdn>/api/v1/preauthorize](/help/authentication/retrieve-list-of-preauthorized-resources.md) | Ruft die Liste der vorab autorisierten Ressource ab |     | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |
| 16. | [&lt;sp_fqdn>/api/v1/preauthorize/{code}](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Ruft die Liste der vorab autorisierten Ressourcen ab |     | Adobe Pass  </br>Authentifizierung  </br>Dienst | Webanwendung anmelden |
| 17. | [&lt;sp_fqdn>/api/v1/logout](/help/authentication/initiate-logout.md) | AuthN- und AuthZ-Token aus dem Speicher entfernen |     | Adobe Pass  </br>Authentifizierung   </br>Dienst | Smart Device |
| 18. | [&lt;sp_fqdn>/api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Ruft Benutzermetadaten nach Abschluss des Authentifizierungsflusses ab | Nicht zutreffend | Nicht zutreffend | Smart Device |
| 19. | [&lt;sp_fqdn>/api/v1/authenticate/freepreview](/help/authentication/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Erstellen eines Authentifizierungstokens für den temporären Pass oder den temporären Weiterleitungs-Pass | Nicht zutreffend | Adobe Pass  </br>Authentifizierung  </br>Dienst | Smart Device |


## REST API Security {#security}

Alle clientlosen Adobe Pass-Authentifizierungs-APIs müssen zur sicheren Kommunikation mithilfe des HTTPS-Protokolls aufgerufen werden. Darüber hinaus sollten die meisten APIs, die aufgerufen werden, ein Zugriffstoken enthalten, das von [Dynamische Kundenregistrierung](/help/authentication/dynamic-client-registration.md).
