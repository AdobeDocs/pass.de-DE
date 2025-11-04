---
title: Authentifizierungstoken überprüfen
description: Authentifizierungstoken überprüfen
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# (Legacy) Authentifizierungstoken überprüfen {#check-authentication-token}

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

Gibt an, ob das Gerät über ein nicht abgelaufenes Authentifizierungstoken verfügt.

| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | Streaming-App</br></br>oder</br></br>Programmierer-Service | &#x200B;1. Antragsteller (obligatorisch)</br>2.  deviceId (obligatorisch)</br>3.  device_info/X-device-info (obligatorisch)</br>4.  _deviceType_ </br>5.  _deviceUser_ (veraltet)</br>6.  _appId_ (veraltet) | GET | XML oder JSON mit Fehlerdetails, wenn nicht erfolgreich. | 200 - Erfolg   </br>403 - Kein Erfolg |

{style="table-layout:auto"}


| Eingabeparameter | Beschreibung |
| --- | --- |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| device_info/</br></br>X-device-info | Informationen zu Streaming-Geräten.</br></br>**Hinweis**: Dies kann als URL-Parameter an device_info übergeben werden, sollte jedoch aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen für eine GET-URL als X-Device-Info im HTTP-Header übergeben werden. </br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter richtig festgelegt ist, bietet ESM Metriken an, die [nach Gerätetyp aufgeschlüsselt) ](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) Verwendung von Clientless sind, sodass verschiedene Arten der Analyse für z. B. Roku, AppleTV, Xbox usw. durchgeführt werden können.</br></br>Weitere Informationen finden Sie unter [Vorteile der Verwendung des Parameters „Clientless deviceType“ in Adobe Pass-Authentifizierungsmetriken ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Hinweis**: Dieser Parameter wird durch „device_info“ ersetzt. |
| _deviceUser_ | Die Geräte-Benutzerkennung. |
| _appId_ | Die Anwendungs-ID/-name.</br>**Hinweis**: device_info ersetzt diesen Parameter. |

{style="table-layout:auto"}


## Antwort (falls nicht erfolgreich) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [Zurück zur REST-API-Referenz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
