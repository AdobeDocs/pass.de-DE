---
title: Authentifizierungstoken abrufen
description: Authentifizierungstoken abrufen
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 0%

---

# Authentifizierungstoken abrufen {#retrieve-authentication-token}

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

Ruft Authentifizierungs-Token (AuthN) ab.

| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn</br></br>Beispiel:</br></br>&lt;SP_FQDN>/api/v1/tokens/authn | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Antragsteller (erforderlich)</br>2.  deviceId (erforderlich)</br>3.  device_info/X-Device-Info (erforderlich)</br>4.  _deviceType_ (nicht mehr unterstützt)</br>5.  _deviceUser_ (Veraltet)</br>6.  _appId_ (veraltet) | GET | XML oder JSON mit Authentifizierungsinformationen oder Fehlerdetails, falls nicht erfolgreich. | 200 - Erfolg.  </br>404 - Token nicht gefunden </br>410 - Token abgelaufen |

{style="table-layout:auto"}


| Eingabeparameter | Beschreibung |
| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| device_info/</br></br>X-Device-Info | Informationen zum Streaming-Gerät.</br></br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br></br>Weitere Informationen finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>**Hinweis**: Der Parameter device_info ersetzt diesen. |
| _deviceUser_ | Die Benutzer-ID des Geräts.</br></br>**Hinweis**: Bei Verwendung von sollte deviceUser dieselben Werte wie in der Anforderung [Registrierungscode erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) aufweisen. |
| _appId_ | Die Anwendungs-ID/der Name. </br></br>**Hinweis**: Der Parameter device_info ersetzt diesen. Bei Verwendung von sollte `appId` dieselben Werte wie in der Anfrage [Registrierungscode erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) aufweisen. |

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





#### Authentifizierungstoken nicht gefunden:

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
