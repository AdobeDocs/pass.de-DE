---
title: Autorisierung starten
description: Autorisierung starten
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# (Legacy) Autorisierung einleiten {#initiate-authorization}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

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

Erhält die Autorisierungsantwort.

| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | Streaming-App</br></br>oder</br></br>Programmierer-Service | 1. Antragsteller (obligatorisch)</br>2.  deviceId (obligatorisch)</br>3.  Ressource (obligatorisch)</br>4.  device_info/X-device-info (obligatorisch)</br>5.  _deviceType_</br> 6.  _deviceUser_ (veraltet)</br>7.  _appId_ (veraltet)</br>8.  Zusätzliche Parameter (optional) | GET | XML oder JSON mit Autorisierungsdetails oder Fehlerdetails, wenn nicht erfolgreich. Siehe Beispiele unten. | 200 - Erfolg </br>403 - Kein Erfolg |

{style="table-layout:auto"}

</br>


| Eingabeparameter | Beschreibung |
| --- | --- |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| Ressource | Eine Zeichenfolge, die eine resourceId (oder ein MRSS-Fragment) enthält, den von einem Benutzer angeforderten Inhalt identifiziert und von MVPD-Autorisierungsendpunkten erkannt wird. |
| device_info/</br></br>X-device-info | Informationen zu Streaming-Geräten.</br></br>**Hinweis**: Dies kann als URL-Parameter an device_info übergeben werden, sollte jedoch aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen für eine GET-URL als X-Device-Info im HTTP-Header übergeben werden. </br></br>Vollständige Details finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter richtig festgelegt ist, bietet ESM Metriken an, die [nach Gerätetyp aufgeschlüsselt) ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) Verwendung von Clientless sind, sodass verschiedene Arten der Analyse für z. B. Roku, AppleTV, Xbox usw. durchgeführt werden können.</br></br>Siehe [Vorteile des Client-losen Gerätetyp-Parameters in Kennzahlen ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: Der Parameter wird durch „device_info“ ersetzt. |
| _deviceUser_ | Die Geräte-Benutzerkennung. |
| _appId_ | Die Anwendungs-ID/-name. </br></br>**Hinweis**: device_info ersetzt diesen Parameter. |
| Zusätzliche Parameter | Der Aufruf kann auch optionale Parameter enthalten, die andere Funktionen ermöglichen, z. B.: </br></br>* generic_data - Aktiviert die Verwendung von [Werbe-TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)</br></br>Beispiel: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**IP-Adresse des Streaming-Geräts**</br>
>Bei Client-zu-Server-Implementierungen wird die IP-Adresse des Streaming-Geräts mit diesem Aufruf implizit gesendet.  Bei Server-zu-Server-Implementierungen, bei denen der **regcode**-Aufruf vom Programmierdienst und nicht vom Streaming-Gerät erfolgt, ist der folgende Header erforderlich, um die IP-Adresse des Streaming-Geräts zu übergeben:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>wobei `<streaming\_device\_ip>` die öffentliche IP-Adresse des Streaming-Geräts ist.</br></br>
>Beispiel : </br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Beispielantwort {#sample-response}

* **1. Fall: Erfolg**
</br>
  * **XML:**
  </br>

    „XML
    &lt;?xml version=„1.0“ encoding=„UTF-8“ standalone=„yes“?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;Requestor>sampleRequestorId&lt;/Requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>Wenn die Antwort von einer Proxy-MVPD stammt, kann sie ein zusätzliches Element namens `proxyMvpd` enthalten.



* **Fall 2: Genehmigung verweigert**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
