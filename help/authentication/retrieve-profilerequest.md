---
title: Rufen Sie Platform SSO profile-request ab
description: Rufen Sie Platform SSO profile-request ab
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Rufen Sie Platform SSO profile-request ab {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## REST-API-Endpunkte {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Diese Ressource erzeugt Profilanfragen für eine Anforderungs-ID und einen MVPD-Tupel.


| Endpunkt | aufgerufen  </br>von | Eingabe   </br>Parameter | HTTP  </br>Methode | Reaktion | HTTP  </br>Reaktion |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/{requestor}/profile-requests/{mvpd} | Streaming-App</br></br>oder</br></br>Programmiererdienst | 1. requestor (path param)</br>2. mvpd (path param)</br>3. deviceType (Obligatorisch) | GET | Der Content-Type der Antwort ist application/octet-stream, da die tatsächliche Payload für die Client-Anwendung undurchsichtig ist.</br></br>Die Antwort sollte von der Anwendung an die Plattform weitergeleitet werden</br></br>SSO-Engine für den Erhalt einer Profil-SSO. | 200 - Erfolg   </br>400 - Ungültige Anfrage |


| Eingabeparameter | Beschreibung |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| mvpd | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| deviceType | Die Apple-Plattform, für die wir versuchen, eine Profilanfrage zu erhalten.  Entweder **iOS** oder **tvOS**. |
