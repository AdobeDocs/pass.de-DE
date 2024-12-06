---
title: Autorisierung initiieren
description: Autorisierung initiieren
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# Autorisierung initiieren {#initiate-authorization}

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

Erhält die Antwort auf die Autorisierung.

| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Antragsteller (erforderlich)</br>2.  deviceId (erforderlich)</br>3.  resource (erforderlich)</br>4.  device_info/X-Device-Info (erforderlich)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Veraltet)</br>7.  _appId_ (veraltet)</br>8.  Zusätzliche Parameter (optional) | GET | XML oder JSON mit Autorisierungsdetails oder Fehlerdetails, falls nicht erfolgreich. Siehe Beispiele unten. | 200 - Erfolg </br>403 - Kein Erfolg |

{style="table-layout:auto"}

</br>


| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| resource | Eine Zeichenfolge, die eine resourceId (oder ein MRSS-Fragment) enthält, den von einem Benutzer angeforderten Inhalt identifiziert und von MVPD-Autorisierungsendpunkten erkannt wird. |
| device_info/</br></br>X-Device-Info | Informationen zum Streaming-Gerät.</br></br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br></br>Weitere Informationen finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die bei Verwendung von ClientLess [nach Gerätetyp aufgeschlüsselt sind](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type), sodass für Roku, AppleTV, Xbox usw. unterschiedliche Analysetypen durchgeführt werden können.</br></br>Siehe [Vorteile des Parameters für Client-lose Gerätetypen in den Pass-Metriken ](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: Der Parameter device_info ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts. |
| _appId_ | Die Anwendungs-ID/der Name. </br></br>**Hinweis**: Der Parameter device_info ersetzt diesen. |
| zusätzliche Parameter | Der Aufruf kann auch optionale Parameter enthalten, die andere Funktionen wie </br></br>* generic_data - ermöglicht die Verwendung von [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)</br></br>Beispiel: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**IP-Adresse des Streaming-Geräts**</br>
>Bei Client-zu-Server-Implementierungen wird die IP-Adresse des Streaming-Geräts implizit mit diesem Aufruf gesendet.  Bei Server-zu-Server-Implementierungen, bei denen der **regcode** -Aufruf vom Programmierer-Dienst und nicht vom Streaming-Gerät erfolgt, muss die folgende Kopfzeile die IP-Adresse des Streaming-Geräts übergeben:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>wobei `<streaming\_device\_ip>` die öffentliche IP-Adresse des Streaming-Geräts ist.</br></br>
>Beispiel :</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Beispielantwort {#sample-response}

* **1: Erfolg**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpres d>
    &lt;requestor>sampleRequestorId&lt;/requestor>
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
>Wenn die Antwort von einem Proxy-MVPD stammt, kann sie ein zusätzliches Element namens `proxyMvpd` enthalten.



* **2: Autorisierung verweigert**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
