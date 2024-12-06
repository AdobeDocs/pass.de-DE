---
title: Plattform-SSO-Token für ein Adobe-Token austauschen
description: Plattform-SSO-Token für ein Adobe-Token austauschen
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Plattform-SSO-Token für ein Adobe-Token austauschen {#exchange-a-platform-sso-token-for-an-adobe-token}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch den [Drosselmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) begrenzt

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Ermöglicht den &quot;Austausch&quot;eines Platform SSO-Profils durch ein Adobe-Token.

| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Antragsteller (erforderlich)</br>    </br>2.  deviceId (Obligatorisch)</br>    </br>3.  mvpd (erforderlich)</br>    </br>4.  deviceType (Obligatorisch)</br>    </br>5.  SAMLResponse (erforderlich)</br>    </br>6.  deviceUser (nicht mehr unterstützt)</br>    </br>7.  appId (nicht mehr unterstützt) | POST | Die erfolgreiche Antwort lautet &quot;No Content&quot;(Kein Inhalt) 204 und gibt an, dass das Token erfolgreich erstellt wurde und für die Authoring-Flüsse verwendet werden kann. | 204 - Kein Inhalt   </br>400 - Ungültige Anfrage |


| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| mvpd | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| deviceType | Die Apple-Plattform, für die wir versuchen, eine Profilanfrage zu erhalten.  Entweder **iOS** oder **tvOS**. |
| SAMLResponse | Das von Platform SSO zurückgegebene tatsächliche Profil. |
| _deviceUser_ | Die Benutzer-ID des Geräts. |
| _appId_ | Die Anwendungs-ID/der Name. |
