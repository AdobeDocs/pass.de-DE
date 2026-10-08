---
title: Platform-SSO-Profilanfrage abrufen
description: Platform-SSO-Profilanfrage abrufen
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 1%
---
# (Legacy) Platform-SSO-Profilanfrage abrufen {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselungsmechanismus) ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Diese Ressource erzeugt Profilanfragen für eine Anforderer-ID und ein MVPD-Tupel.


| Endpunkt | Called </br>by | Input </br>params | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | Streaming-App</br></br>oder</br></br>Programmierer-Service | &#x200B;1. Requestor (path param)</br>2. mvpd (Pfadparameter)</br>3. deviceType (Obligatorisch) | GET | Der Content-Typ der Antwort lautet application/octet-stream, da die tatsächliche Payload für die Client-Anwendung undurchsichtig ist.</br></br>Die Antwort sollte von der Anwendung zum Abrufen eines Profil-SSO an </br></br> Platform-SSO-Engine weitergeleitet werden. | 200 - Erfolgreich </br>400 - Fehlerhafte Anfrage |


| Eingabeparameter | Beschreibung |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| mvpd | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| deviceType | Die Apple-Plattform, für die wir eine Profilanfrage abrufen möchten.  Entweder **iOS** oder **tvOS**. |
