---
title: REST-API-Referenz
description: REST-API-Referenz
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 2%

---

# (Legacy) REST-API-Referenz {#rest-api-reference}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Drosselmechanismus

Die Adobe Pass-Authentifizierungs-REST-API wird von einem [Einschränkungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gesteuert.

## Antwortformate {#response-formats}


>[!NOTE]
>
> Die in diesen Services bereitgestellten APIs können Antworten entweder in XML oder in JSON zurückgeben (für APIs, die eine Antwort zurückgeben). Es gibt drei verschiedene Möglichkeiten, das Antwortformat in der Anfrage anzugeben:
>
>* Setzen Sie HTTP Accept Header auf `application/xml` oder `application/json`.
>* Geben Sie in der Anfrage-Payload den Parameter `format=xml` oder `format=json` an.
>* Rufen Sie den Webservice-Endpunkt mit der Erweiterung `.xml` oder `.json` auf. Beispiel: `/regcode.xml` oder `/regcode.json`
>
>Sie können eine der oben genannten Methoden angeben. Das Angeben mehrerer Methoden mit widersprüchlichen Formaten kann zu Fehlern oder unerwünschter Ausgabe führen.

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Webservices-Zusammenfassung {#web_srvs_summary}

In der folgenden Tabelle sind die verfügbaren Web-Services für den Client-losen Ansatz aufgeführt. Klicken Sie auf die Endpunkte der Webdienste, um weitere Informationen zu erhalten (Beispielanfrage und -antwort, Eingabeparameter, HTTP-Methoden usw.)


| Sr | Webdienst-Endpunkt | Beschreibung | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Gehostet am | Aufgerufen von |
|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------|
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | Gibt den zufällig generierten Registrierungs-Code und den Anmeldeseiten-URI zurück. | 2 | Adobe </br>Reg Code Service | Intelligentes Gerät |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | Gibt einen Registrierungs-Code-Eintrag mit Registrierungs-Code-UUID, Registrierungs-Code und Hash-Geräte-ID zurück | 8 | Adobe </br>Reg Code Service | Adobe Pass-Authentifizierung |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br> {requestorId}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | Gibt die Liste der konfigurierten MVPDs für den Anforderer aus | 5 | Adobe </br>Adobe Pass </br>Authentication </br>Service | Anmeldung </br>Web </br>App |
| 4. | [&lt;SP_FQDN>/api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | Startet den Authentifizierungsprozess durch Benachrichtigung des MVPD-Auswahlereignisses. Erstellt einen Datensatz in der Authentifizierungsdatenbank, der abgeglichen wird, wenn eine erfolgreiche Antwort von MVPD empfangen wird (Schritt 13) | 7 | Adobe </br>Adobe Pass </br>Authentication </br>Service | Anmeldung </br>Web </br>App |
| 5. | SAML-Assertion-Verbraucher | Vorhandener SAML-Workflow zwischen Adobe Pass-Authentifizierung und MVPD | 13 | Adobe Pass </br>Authentication </br>Service | Adobe Pass-Authentifizierung |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Die Web-Anmeldeanwendung kann überprüfen, ob der Anmeldeversuch erfolgreich war |                                                                                             | Adobe Pass </br>Authentifizierung   </br>Service | Login   </br>Web   </br>App |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Ruft Authentifizierungs-Token-bezogene Metadaten ab | 15 | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md) | Löscht den Registrierungscode-Datensatz und gibt den Registrierungscode zur Wiederverwendung frei. | 16 | Adobe </br>Reg Code Service | Adobe Pass-Authentifizierung |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | Erhält die Autorisierungsantwort. | 17 | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) | Gibt an, ob das Gerät über ein nicht abgelaufenes Authentifizierungs-Token verfügt. |                                                                                             | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Gibt das AuthN-Token zurück, wenn vorhanden. |                                                                                             | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | Gibt das AuthZ-Token zurück, wenn vorhanden. |                                                                                             | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | Gibt das Short Media Token zurück, falls gefunden. Entspricht /api/v1/mediatoken |                                                                                             | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | Abrufen eines Short Media-Tokens |                                                                                             | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) | Ruft die Liste der vorab autorisierten Ressourcen ab |                                                                                             | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |
| 16. | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Ruft die Liste der vorab autorisierten Ressourcen ab |                                                                                             | Adobe Pass </br>Authentication </br>Service | Web-App anmelden |
| 17. | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | Entfernen von AuthN- und AuthZ-Token aus dem Speicher |                                                                                             | Adobe Pass </br>Authentifizierung   </br>Service | Intelligentes Gerät |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Ruft Benutzermetadaten nach Abschluss des Authentifizierungsflusses ab | Nicht zutreffend | Nicht zutreffend | Intelligentes Gerät |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Erstellen eines Authentifizierungstokens für temporären Pass oder promotionellen temporären Pass | Nicht zutreffend | Adobe Pass </br>Authentication </br>Service | Intelligentes Gerät |


## REST API-Sicherheit {#security}

Alle REST-APIs für die Adobe Pass-Authentifizierung müssen mit dem HTTPS-Protokoll aufgerufen werden, um eine sichere Kommunikation zu gewährleisten. Darüber hinaus sollten die meisten aufgerufenen APIs ein Zugriffstoken enthalten, das wie in der API-Dokumentation [Zugriffstoken abrufen](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben wurde.
