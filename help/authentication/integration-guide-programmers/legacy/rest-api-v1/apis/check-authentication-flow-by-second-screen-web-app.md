---
title: Überprüfen des Authentifizierungsflusses durch die Web-App auf dem zweiten Bildschirm
description: Überprüfen des Authentifizierungsflusses durch die Web-App auf dem zweiten Bildschirm
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# (Legacy) Überprüfen des Authentifizierungsflusses durch die Web-App auf dem zweiten Bildschirm {#check-authentication-flow-by-second-screen-web-app}

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

Diese API sollte von der Web-Anwendung für die Anmeldung am zweiten Bildschirm verwendet werden, um zu bestätigen, dass die Adobe Pass-Authentifizierung die erfolgreiche Anmeldung von MVPD bestätigt hat. Es wird empfohlen, diese API aufzurufen, bevor dem Endbenutzer eine Erfolgsmeldung angezeigt wird, die ihn anweist, mit der Gerätekonsole fortzufahren, um mit den Workflows fortzufahren.


| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{registration code} | Web-App anmelden | &#x200B;1. Registrierungs-Code </br>    (Pfadkomponente)</br>2.  Antragsteller-</br>    (Obligatorisch) | GET | XML oder JSON mit Fehlerdetails, wenn nicht erfolgreich. | 200 - Erfolg   </br>403 - Verboten |

</br>

| Eingabeparameter | Beschreibung |
| ----------------- | --------------------------------------------------------------------------------------------- |
| Registrierungs-Code | Der Wert des Registrierungs-Codes, der vom Benutzer zu Beginn des Authentifizierungsflusses angegeben wurde. |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |


### Beispielantwort (im Fehlerfall) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [Zurück zur REST-API-Referenz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
