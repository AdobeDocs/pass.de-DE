---
title: Short Media Token abrufen
description: short media token abrufen
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Short Media Token abrufen {#obtain-short-media-token}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch den [Drosselmechanismus](/help/authentication/throttling-mechanism.md) begrenzt

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Staging - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Staging - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Erhält ein Short Media Token.

| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br> oder</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>Beispiel:</br></br>&lt;SP_FQDN>/api/v1/tokens/media | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Antragsteller (erforderlich)</br>2.  deviceId (erforderlich)</br>3.  resource (erforderlich)</br>4.  device_info/X-Device-Info (erforderlich)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Veraltet)</br>7.  _appId_ (veraltet) | GET | XML oder JSON, die ein Base64-kodiertes Medien-Token oder Fehlerdetails enthalten, falls nicht erfolgreich. | 200 - Erfolg </br>403 - Kein Erfolg |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| resource | Eine Zeichenfolge, die eine resourceId (oder ein MRSS-Fragment) enthält, den von einem Benutzer angeforderten Inhalt identifiziert und von MVPD-Autorisierungsendpunkten erkannt wird. |
| device_info/</br></br>X-Device-Info | Informationen zum Streaming-Gerät.</br></br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br></br>Weitere Informationen finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die bei Verwendung von ClientLess nach Gerätetyp ]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) aufgeschlüsselt sind, sodass unterschiedliche Analysetypen durchgeführt werden können. [ Beispiel: Roku, AppleTV und Xbox.</br></br>Siehe [Vorteile der Verwendung des Client-losen Gerätetypparameters ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: Der Parameter device_info ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts.</br></br>**Hinweis**: Bei Verwendung von sollte deviceUser dieselben Werte wie in der Anforderung [Registrierungscode erstellen](/help/authentication/registration-code-request.md) aufweisen. |
| _appId_ | Die Anwendungs-ID/der Name. </br></br>**Hinweis**: Der Parameter device_info ersetzt diesen. Bei Verwendung von sollte `appId` dieselben Werte wie in der Anfrage [Registrierungscode erstellen](/help/authentication/registration-code-request.md) aufweisen. |

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



### Kompatibilität der Medienverifizierungsbibliothek

Das Feld `serializedToken` aus dem Aufruf &quot;Short Media Token abrufen&quot;ist ein Base64-kodiertes Token, das mit der Adobe Medien Verification Library verifiziert werden kann.
