---
title: Ersetzen eines Platform SSO-Tokens durch ein Adobe-Token
description: Ersetzen eines Platform SSO-Tokens durch ein Adobe-Token
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# (Legacy) Ersetzen eines Platform SSO-Tokens durch ein Adobe-Token {#exchange-a-platform-sso-token-for-an-adobe-token}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselungsmechanismus) &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Ermöglicht den „Austausch“ eines Platform SSO-Profils gegen ein Adobe-Token.

| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | Streaming-App</br></br>oder</br></br>Programmierer-Service | 1. Antragsteller (obligatorisch)</br>    </br>2.  deviceId (obligatorisch)</br>    </br>3.  MVPD (obligatorisch)</br>    </br>4.  deviceType (Obligatorisch)</br>    </br>5.  SAMLReantwort (obligatorisch)</br>    </br>6.  deviceUser (veraltet)</br>    </br>7.  appId (veraltet) | POST | Die erfolgreiche Antwort lautet 204 Kein Inhalt. Dies bedeutet, dass das Token erfolgreich erstellt wurde und für die Autorisierungsflüsse verwendet werden kann. | 204 - Kein Inhalt   </br>400 - Fehlerhafte Anfrage |


| Eingabeparameter | Beschreibung |
| --- | --- |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| mvpd | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| deviceType | Die Apple-Plattform, für die wir eine Profilanfrage abrufen möchten.  Entweder **iOS** oder **tvOS**. |
| SAMLResponse | Das tatsächliche Profil, wie von Platform SSO zurückgegeben. |
| _deviceUser_ | Die Geräte-Benutzerkennung. |
| _appId_ | Die Anwendungs-ID/-name. |
