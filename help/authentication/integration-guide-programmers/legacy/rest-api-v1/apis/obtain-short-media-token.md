---
title: Abrufen eines Short Media Tokens
description: Abrufen eines Short Media Tokens
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---

# (Legacy) Short Media Token abrufen {#obtain-short-media-token}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselungsmechanismus) ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Staging - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Staging - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Ruft das kurze Medien-Token ab.

| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br> oder</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>Beispiel:</br></br>&lt;SP_FQDN>/api/v1/tokens/media | Streaming-App</br></br>oder</br></br>Programmierer-Service | 1. Antragsteller (obligatorisch)</br>2.  deviceId (obligatorisch)</br>3.  Ressource (obligatorisch)</br>4.  device_info/X-device-info (obligatorisch)</br>5.  _deviceType_</br> 6.  _deviceUser_ (veraltet)</br>7.  _appId_ (veraltet) | GET | XML oder JSON mit einem Base64-kodierten Medien-Token oder Fehlerdetails, wenn der Vorgang nicht erfolgreich war. | 200 - Erfolg </br>403 - Kein Erfolg |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| Eingabeparameter | Beschreibung |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| Ressource | Eine Zeichenfolge, die eine resourceId (oder ein MRSS-Fragment) enthält, den von einem Benutzer angeforderten Inhalt identifiziert und von MVPD-Autorisierungsendpunkten erkannt wird. |
| device_info/</br></br>X-device-info | Informationen zu Streaming-Geräten.</br></br>**Hinweis**: Dies kann als URL-Parameter an device_info übergeben werden, sollte jedoch aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen für eine GET-URL als X-Device-Info im HTTP-Header übergeben werden. </br></br>Vollständige Details finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter richtig festgelegt ist, bietet ESM Metriken an, [ bei Verwendung von Clientless (nach Gerätetyp ]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) aufgeschlüsselt sind, sodass verschiedene Analysetypen für ausgeführt werden können. Zum Beispiel Roku, AppleTV und Xbox.</br></br>Siehe [Vorteile der Verwendung von clientless-DeviceType-](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: Der Parameter wird durch device_info ersetzt. |
| _deviceUser_ | Die Geräte-Benutzerkennung.</br></br>**Hinweis**: Falls verwendet, sollte deviceUser dieselben Werte wie in der Anfrage [Registrierungs-Code erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) haben. |
| _appId_ | Die Anwendungs-ID/-name. </br></br>**Hinweis**: device_info ersetzt diesen Parameter. Wenn verwendet, sollten `appId` dieselben Werte wie in der Anfrage [Registrierungs-Code erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) haben. |

{style="table-layout:auto"}

### Beispielantwort {#response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON:**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### Kompatibilität der Media Verification Library

Das aus dem Aufruf „Kurzmedientoken abrufen“ `serializedToken` Feld ist ein Base64-kodiertes Token, das mit der Adobe Medien-Verifizierungsbibliothek verifiziert werden kann.
