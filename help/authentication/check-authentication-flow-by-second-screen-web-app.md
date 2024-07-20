---
title: Überprüfen des Authentifizierungsflusses nach Second Screen Web App
description: Überprüfen des Authentifizierungsflusses nach Second Screen Web App
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Überprüfen des Authentifizierungsflusses nach Second Screen Web App {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch den [Drosselmechanismus](/help/authentication/throttling-mechanism.md) begrenzt

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Diese API sollte von der zweiten Bildschirmanmelde-Webanwendung verwendet werden, um zu bestätigen, dass die Adobe Pass-Authentifizierung die erfolgreiche Anmeldung von MVPD bestätigt hat. Es wird empfohlen, diese API aufzurufen, bevor dem Endbenutzer eine Erfolgsmeldung angezeigt wird, die ihn anweist, mit der Gerätekonsole fortzufahren und die Workflows fortzusetzen.


| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{Registrierungscode} | Webanwendung anmelden | 1. Registrierungs-Code </br>    (Pfadkomponente)</br>2.  requestor </br>    (Obligatorisch) | GET | XML oder JSON mit Fehlerdetails, falls nicht erfolgreich. | 200 - Erfolg   </br>403 - Verboten |

</br>

| Eingabeparameter | Beschreibung |
| ----------------- | --------------------------------------------------------------------------------------------- |
| Registrierungscode | Der vom Benutzer am Anfang des Authentifizierungsflusses angegebene Registrierungscode-Wert. |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |


### Beispielantwort (im Fall eines Fehlers) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [Zurück zur REST-API-Referenz](/help/authentication/rest-api-reference.md)
