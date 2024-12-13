---
title: Authentifizierungstoken abrufen
description: Authentifizierungstoken abrufen
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# (Legacy) Authentifizierungstoken abrufen {#retrieve-authentication-token}

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

Ruft das Authentifizierungs-Token (AuthN) ab.

| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn</br></br>Beispiel: </br></br>&lt;SP_FQDN>/api/v1/tokens/authn | Streaming-App</br></br>oder</br></br>Programmierer-Service | 1. Antragsteller (obligatorisch)</br>2.  deviceId (obligatorisch)</br>3.  device_info/X-device-info (obligatorisch)</br>4.  _deviceType_ (veraltet)</br>5.  _deviceUser_ (veraltet)</br>6.  _appId_ (veraltet) | GET | XML oder JSON mit Authentifizierungsinformationen oder Fehlerdetails, falls nicht erfolgreich. | 200 - Erfolg.  </br>404 - Token nicht gefunden </br>410 - Token abgelaufen |

{style="table-layout:auto"}


| Eingabeparameter | Beschreibung |
| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| device_info/</br></br>X-device-info | Informationen zu Streaming-Geräten.</br></br>**Hinweis**: Dies kann als URL-Parameter an device_info übergeben werden, sollte jedoch aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen für eine GET-URL als X-Device-Info im HTTP-Header übergeben werden. </br></br>Vollständige Details finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>**Hinweis**: device_info ersetzt diesen Parameter. |
| _deviceUser_ | Die Geräte-Benutzerkennung.</br></br>**Hinweis**: Falls verwendet, sollte deviceUser dieselben Werte wie in der Anfrage [Registrierungs-Code erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) haben. |
| _appId_ | Die Anwendungs-ID/-name. </br></br>**Hinweis**: device_info ersetzt diesen Parameter. Wenn verwendet, sollten `appId` dieselben Werte wie in der Anfrage [Registrierungs-Code erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) haben. |

{style="table-layout:auto"}

</br>

### Beispielantwort {#response}



#### Erfolg

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON:**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





#### Authentifizierungs-Token nicht gefunden:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```


**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found"
    }
```
