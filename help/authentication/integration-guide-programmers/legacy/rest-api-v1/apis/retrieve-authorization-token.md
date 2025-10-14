---
title: Autorisierungs-Token abrufen
description: Autorisierungs-Token abrufen
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 0%

---

# (Legacy) Autorisierungs-Token abrufen {#retrieve-authorization-token}

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

Ruft das Autorisierungs-Token (AuthZ) ab.


| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authz</br></br>Beispiel: </br></br>&lt;SP_FQDN>/api/v1/tokens/authz | Streaming-App</br></br>oder</br></br>Programmierer-Service | 1. Antragsteller (obligatorisch)</br>2.  deviceId (obligatorisch)</br>3.  Ressource (obligatorisch)</br>4.  device_info/X-device-info (obligatorisch)</br>5.  _deviceType_</br> 6.  _deviceUser_ (veraltet)</br>7.  _appId_ (veraltet) | GET | 1. Erfolg</br> 2.  Authentifizierungstoken-</br>    Nicht gefunden oder abgelaufen:   </br>    XML-</br>    für Auth-Token nicht gefunden</br>3.  Autorisierungs-Token-</br>    Nicht gefunden: </br>    XML-</br> 4.  Autorisierungs-Token-</br>    Abgelaufen: </br>    XML-Erklärung | 200 - Erfolg </br>412 - Kein AuthN</br></br>404 - Kein AuthZ</br></br>410 - AuthZ abgelaufen |

{style="table-layout:auto"}

</br>

| Eingabeparameter | Beschreibung |
| --- | --- |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| Ressource | Eine Zeichenfolge, die eine resourceId (oder ein MRSS-Fragment) enthält, den von einem Benutzer angeforderten Inhalt identifiziert und von MVPD-Autorisierungsendpunkten erkannt wird. |
| device_info/</br></br>X-device-info | Informationen zu Streaming-Geräten.</br></br>**Hinweis**: Dies kann als URL-Parameter an device_info übergeben werden, sollte jedoch aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen für eine GET-URL als X-Device-Info im HTTP-Header übergeben werden. </br></br>Vollständige Details finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter richtig festgelegt ist, bietet ESM Metriken an, die [nach Gerätetyp aufgeschlüsselt) &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) Verwendung von Clientless sind, sodass verschiedene Arten der Analyse durchgeführt werden können, z. B. Roku, AppleTV und Xbox.</br></br>Siehe [Vorteile der Verwendung von Client-losen Gerätetypparametern in Kennzahlen &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: der Parameter wird durch „device_info“ ersetzt. |
| _deviceUser_ | Die Geräte-Benutzerkennung. |
| _appId_ | Die Anwendungs-ID/-name. </br></br>**Hinweis**: device_info ersetzt diesen Parameter. |

{style="table-layout:auto"}


### Beispielantwort {#response}



#### Erfolg

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON:**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### Authentifizierungstoken nicht gefunden oder abgelaufen:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### Autorisierungs-Token nicht gefunden:

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
        "message": "Not Found",
        "details": null
    }
```

</br>



#### Autorisierungs-Token abgelaufen:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
