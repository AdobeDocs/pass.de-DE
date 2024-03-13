---
title: Kostenlose Vorschau für den temporären Pass und den temporären Weiterleitungs-Pass
description: Kostenlose Vorschau für den temporären Pass und den temporären Weiterleitungs-Pass
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Kostenlose Vorschau für den temporären Pass und den temporären Weiterleitungs-Pass {#free-preview-for-temp-pass-and-promotional-temp-pass}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselmechanismus](/help/authentication/throttling-mechanism.md)

## REST-API-Endpunkte {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Ermöglicht die Erstellung eines Authentifizierungstokens für den Temp Pass und den Temp Pass für Promotions, ohne dass ein zweiter Bildschirm erforderlich ist.


| Endpunkt | aufgerufen  </br>von | Eingabe   </br>Parameter | HTTP  </br>Methode | Reaktion | HTTP  </br>Reaktion |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate/freepreview | Streaming-App</br></br>oder</br></br>Programmiererdienst | 1. requestor_id (erforderlich)</br>    </br>2.  deviceId (Obligatorisch)</br>    </br>3.  mso_id (erforderlich)</br>    </br>4.  domain_name (erforderlich)</br>    </br>5.  device_info/X-Device-Info (erforderlich)</br>6.  deviceType</br>    </br>7.  deviceUser (nicht mehr unterstützt)</br>    </br>8.  appId (nicht mehr unterstützt)</br>    </br>9.  generic_data (optional) | POST | Die erfolgreiche Antwort lautet &quot;No Content&quot;(Kein Inhalt) 204 und gibt an, dass das Token erfolgreich erstellt wurde und für die Authoring-Flüsse verwendet werden kann. | 204 - Kein Inhalt   </br>400 - Ungültige Anfrage |

<div>


| Eingabeparameter | Beschreibung |
| --- | --- |
| requestor_id | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| mso_id | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| domain_name | Der Domänenname, für den ein Token gewährt wird. Dies wird mit den Domänen des Dienstleisters verglichen, wenn ein Autorisierungstoken gewährt wird. |
| device_info/</br></br>X-Device-Info | Informationen zum Streaming-Gerät.</br></br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die [aufgeschlüsselt nach Gerätetyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) bei Verwendung von ClientLess, sodass verschiedene Arten der Analyse für z. B. Roku, AppleTV, Xbox usw. durchgeführt werden können.</br></br>Siehe [Vorteile der Verwendung clientloser Gerätetypparameter ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: Der Parameter device_info ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts.</br></br>**Hinweis**: Bei Verwendung von sollte deviceUser dieselben Werte wie im [Registrierungscode erstellen](/help/authentication/registration-code-request.md) -Anfrage. |
| _appId_ | Die Anwendungs-ID/der Name. </br></br>**Hinweis**: Der Parameter device_info ersetzt diesen. Falls verwendet, `appId` sollte dieselben Werte wie im [Registrierungscode erstellen](/help/authentication/registration-code-request.md) -Anfrage. |
| generic_data | Wird verwendet, um den Umfang des Tokens für den temporären Weiterleitungs-Pass zu begrenzen. |


### [Zurück zur REST-API-Referenz](/help/authentication/rest-api-reference.md)
