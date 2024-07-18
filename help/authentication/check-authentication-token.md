---
title: Authentifizierungstoken überprüfen
description: Authentifizierungstoken überprüfen
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Authentifizierungstoken überprüfen {#check-authentication-token}

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

Gibt an, ob das Gerät über ein nicht abgelaufenes Authentifizierungstoken verfügt.

| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthin | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Antragsteller (erforderlich)</br>2.  deviceId (erforderlich)</br>3.  device_info/X-Device-Info (erforderlich)</br>4.  _deviceType_ </br>5.  _deviceUser_ (Veraltet)</br>6.  _appId_ (veraltet) | GET | XML oder JSON mit Fehlerdetails, falls nicht erfolgreich. | 200 - Erfolg   </br>403 - Kein Erfolg |

{style="table-layout:auto"}


| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| device_info/</br></br>X-Device-Info | Informationen zum Streaming-Gerät.</br></br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info weitergegeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. 2.</br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)--> |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die bei Verwendung von ClientLess [nach Gerätetyp aufgeschlüsselt sind](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type), sodass für Roku, AppleTV, Xbox usw. unterschiedliche Analysetypen durchgeführt werden können.</br></br>Weitere Informationen finden Sie unter [Vorteile der Verwendung des Parameters Clientless deviceType in den Adobe Pass-Authentifizierungsmetriken ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Hinweis**: device_info ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts. |
| _appId_ | Die Anwendungs-ID/der Name.</br>**Hinweis**: Der Parameter device_info ersetzt diesen. |

{style="table-layout:auto"}


## Antwort (falls nicht erfolgreich) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [Zurück zur REST-API-Referenz](/help/authentication/rest-api-reference.md)
