---
title: REST-API-Referenz
description: REST-API-Referenz
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 2%

---

# REST-API-Referenz {#rest-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Drosselmechanismus

Die Adobe Pass-Authentifizierungs-REST-API wird von einem [Drosselmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) verwaltet.

## Antwortformate {#response-formats}


>[!NOTE]
>
> Die in diesen Diensten bereitgestellten APIs können Antworten in XML oder JSON zurückgeben (für APIs, die eine Antwort zurückgeben). Es gibt 3 verschiedene Möglichkeiten, das Antwortformat in der Anfrage anzugeben:
>
>* Setzen Sie HTTP Accept Header auf `application/xml` oder `application/json`.
>* Geben Sie in der Anfrage-Payload den Parameter `format=xml` oder `format=json` an.
>* Rufen Sie den Webdienst-Endpunkt mit der Erweiterung `.xml` oder `.json` auf. Beispiel: `/regcode.xml` oder `/regcode.json`
>
>Sie können eine der oben genannten Methoden angeben. Das Angeben mehrerer Methoden mit widersprüchlichen Formaten kann zu Fehlern oder unerwünschten Ausgaben führen.

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Web Services - Zusammenfassung {#web_srvs_summary}

In der folgenden Tabelle sind die verfügbaren Webdienste für den clientlosen Ansatz aufgeführt. Klicken Sie auf die Webdienst-Endpunkte, um weitere Informationen zu erhalten (Beispielanfrage und -antwort, Eingabeparameter, HTTP-Methoden usw.).


| Sr | Web Service Endpoint | Beschreibung | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | gehostet bei | aufgerufen von |
|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------|
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | Gibt zufällig generierten Registrierungs-Code und Anmeldeseiten-URI zurück | 2 | Adobe </br>Reg-Code-Dienst | Smart Device |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | Gibt den Registrierungs-Code-Datensatz mit Registrierungs-Code-UUID, Registrierungs-Code und Hash-Geräte-ID zurück | 8 | Adobe </br>Reg-Code-Dienst | Adobe Pass-Authentifizierung |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br> {requestorId}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | Gibt eine Liste der konfigurierten MVPDs für den Anforderer zurück | 5 | Adobe </br>Adobe Pass </br>authentication </br>service | Anmelden </br>Web </br>App |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | Initiiert den AuthN-Prozess durch Information zum MVPD-Auswahlereignis. Erstellt einen Datensatz in der Authentifizierungsdatenbank, der abgestimmt wird, wenn eine erfolgreiche Antwort von MVPD empfangen wird (Schritt 13) | 7 | Adobe </br>Adobe Pass </br>authentication </br>service | Anmelden </br>Web </br>App |
| 5. | SAML Assertion Consumer | Vorhandener SAML-Workflow zwischen Adobe Pass-Authentifizierung und MVPD | 13 | Adobe Pass </br>authentication </br>-Dienst | Adobe Pass-Authentifizierung |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Die Web-Anwendung &quot;Anmeldung&quot;kann prüfen, ob der versucht wurde, den Anmeldevorgang erfolgreich durchzuführen |                                                                                             | Adobe Pass </br>Authentifizierung   </br>-Dienst | Anmelden   </br> Web   </br> App |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Ruft AuthN-Token-bezogene Metadaten ab | 15 | Adobe Pass </br>authentication </br>-Dienst | Smart Device |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md) | Löscht den Reg-Codedatensatz und gibt den Reg-Code zur Wiederverwendung frei | 16 | Adobe </br>Reg-Code-Dienst | Adobe Pass-Authentifizierung |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | Erhält die Antwort auf die Autorisierung. | 17 | Adobe Pass </br>authentication </br>-Dienst | Smart Device |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) | Gibt an, ob das Gerät über ein nicht abgelaufenes AuthN-Token verfügt. |                                                                                             | Adobe Pass </br>authentication </br>-Dienst | Smart Device |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Gibt das AuthN-Token zurück, falls gefunden. |                                                                                             | Adobe Pass </br>authentication </br>-Dienst | Smart Device |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | Gibt das AuthZ-Token zurück, falls gefunden. |                                                                                             | Adobe Pass </br>authentication </br>-Dienst | Smart Device |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/integration-guide-programmer/rest-apis/rest-api-v1/apis/obtain-short-media-token.md | Gibt das Short Media Token aus, falls vorhanden - identisch mit /api/v1/mediatoken |                                                                                             | Adobe Pass </br>authentication </br>-Dienst | Smart Device |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | Erhält Short Media Token |                                                                                             | Adobe Pass </br>authentication </br>-Dienst | Smart Device |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) | Ruft die Liste der vorab autorisierten Ressource ab |                                                                                             | Adobe Pass </br>authentication </br>-Dienst | Smart Device |
| 16. | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Ruft die Liste der vorab autorisierten Ressourcen ab |                                                                                             | Adobe Pass </br>authentication </br>-Dienst | Webanwendung anmelden |
| 17. | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | AuthN- und AuthZ-Token aus dem Speicher entfernen |                                                                                             | Adobe Pass </br>Authentifizierung   </br>-Dienst | Smart Device |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Ruft Benutzermetadaten nach Abschluss des Authentifizierungsflusses ab | Nicht zutreffend | Nicht zutreffend | Smart Device |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Erstellen eines Authentifizierungstokens für den temporären Pass oder den temporären Weiterleitungs-Pass | Nicht zutreffend | Adobe Pass </br>authentication </br>-Dienst | Smart Device |


## REST API Security {#security}

Alle Adobe Pass Authentication REST-APIs müssen zur sicheren Kommunikation mithilfe des HTTPS-Protokolls aufgerufen werden. Darüber hinaus sollten die meisten aufgerufenen APIs ein Zugriffstoken enthalten, das Sie wie in der API-Dokumentation [Zugriffstoken abrufen](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben erhalten haben.
