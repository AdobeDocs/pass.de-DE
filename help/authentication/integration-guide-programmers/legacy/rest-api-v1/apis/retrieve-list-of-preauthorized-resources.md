---
title: Abrufen einer Liste vorab autorisierter Ressourcen
description: Abrufen einer Liste vorab autorisierter Ressourcen
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Abrufen einer Liste vorab autorisierter Ressourcen {#retrieve-list-of-preauthorized-resources}

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

Anfrage an die Adobe Pass-Authentifizierung zum Abrufen der Liste der bereits autorisierten Ressourcen.

Es gibt zwei Gruppen von APIs: einen Satz für die Streaming-App oder den Programmierer-Dienst und einen Satz für die Second Screen Web App. Auf dieser Seite wird die API für die Streaming-App oder den Programmiererdienst beschrieben.


| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Antragsteller (erforderlich)</br>2.  deviceId (erforderlich)</br>3.  Ressourcenliste (erforderlich)</br>4.  device_info/X-Device-Info (erforderlich)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Veraltet)</br>7.  _appId_ (veraltet) | GET | XML oder JSON, die einzelne Entscheidungen vor der Autorisierung oder Fehlerdetails enthalten. Siehe Beispiele unten. | 200 - Erfolg</br></br>400 - Ungültige Anfrage</br></br>401 - Nicht autorisiert</br></br>405 - Methode nicht zulässig </br></br>412 - Vorbedingung fehlgeschlagen</br></br>500 - Interner Server-Fehler |


| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| Ressourcenliste | Eine Zeichenfolge, die eine kommagetrennte Liste von resourceIds enthält, die den Inhalt identifiziert, auf den ein Benutzer zugreifen kann, und die von MVPD-Autorisierungsendpunkten erkannt wird. |
| device_info/</br></br>X-Device-Info | Informationen zum Streaming-Gerät.</br></br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br></br>Weitere Informationen finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die bei Verwendung von ClientLess [nach Gerätetyp aufgeschlüsselt sind](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type), sodass verschiedene Analysetypen durchgeführt werden können, z. B. Roku, AppleTV und Xbox.</br></br>Siehe, [Vorteile der Verwendung des Parameters für Client-lose Gerätetypen in Pass-Metriken ](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: Der `device_info` ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts. |
| _appId_ | Die Anwendungs-ID/der Name. </br></br>**Hinweis**: Der Parameter device_info ersetzt diesen. |



### Beispielantwort {#sample-response}



**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
