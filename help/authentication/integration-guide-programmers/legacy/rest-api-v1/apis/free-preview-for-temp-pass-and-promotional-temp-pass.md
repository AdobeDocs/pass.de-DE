---
title: Kostenlose Vorschau für den temporären Pass und den temporären Weiterleitungs-Pass
description: Kostenlose Vorschau für den temporären Pass und den temporären Weiterleitungs-Pass
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 0%

---

# Kostenlose Vorschau für den temporären Pass und den temporären Weiterleitungs-Pass {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

Ermöglicht die Erstellung eines Authentifizierungstokens für den Temp Pass und den Temp Pass für Promotions, ohne dass ein zweiter Bildschirm erforderlich ist.


| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. requestor_id (erforderlich)</br>    </br>2.  deviceId (Obligatorisch)</br>    </br>3.  mso_id (erforderlich)</br>    </br>4.  domain_name (erforderlich)</br>    </br>5.  device_info/X-Device-Info (erforderlich)</br>6.  deviceType</br>    </br>7.  deviceUser (nicht mehr unterstützt)</br>    </br>8.  appId (nicht mehr unterstützt)</br>    </br>9.  generic_data (optional) | POST | Die erfolgreiche Antwort lautet &quot;No Content&quot;(Kein Inhalt) 204 und gibt an, dass das Token erfolgreich erstellt wurde und für die Authoring-Flüsse verwendet werden kann. | 204 - Kein Inhalt   </br>400 - Ungültige Anfrage |

<div>


| Eingabeparameter | Beschreibung |
|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| mso_id | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| domain_name | Der Domänenname, für den ein Token gewährt wird. Dies wird mit den Domänen des Dienstleisters verglichen, wenn ein Autorisierungstoken gewährt wird. |
| device_info/</br></br>X-Device-Info | Informationen zum Streaming-Gerät.</br></br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br></br>Weitere Informationen finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die bei Verwendung von ClientLess [nach Gerätetyp aufgeschlüsselt sind](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type), sodass für Roku, AppleTV, Xbox usw. unterschiedliche Analysetypen durchgeführt werden können.</br></br>Siehe [Vorteile der Verwendung clientloser Gerätetypparameter ](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: Der Parameter device_info ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts.</br></br>**Hinweis**: Bei Verwendung von sollte deviceUser dieselben Werte wie in der Anforderung [Registrierungscode erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) aufweisen. |
| _appId_ | Die Anwendungs-ID/der Name. </br></br>**Hinweis**: Der Parameter device_info ersetzt diesen. Bei Verwendung von sollte `appId` dieselben Werte wie in der Anfrage [Registrierungscode erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) aufweisen. |
| generic_data | Wird verwendet, um den Umfang des Tokens für den temporären Weiterleitungs-Pass zu begrenzen. |


### [Zurück zur REST-API-Referenz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
