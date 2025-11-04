---
title: Kostenlose Vorschau für temporären Pass und Werbe-Temporärpass
description: Kostenlose Vorschau für temporären Pass und Werbe-Temporärpass
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Legacy) Kostenlose Vorschau für temporären Pass und Werbe-Temporärpass {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

Ermöglicht die Erstellung eines Authentifizierungs-Tokens für Temp Pass und Promotion Temp Pass, ohne dass ein zweiter Bildschirm erforderlich ist.


| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freePreview | Streaming-App</br></br>oder</br></br>Programmierer-Service | &#x200B;1. Requestor_id (obligatorisch)</br>    </br>2.  deviceId (obligatorisch)</br>    </br>3.  mso_id (obligatorisch)</br>    </br>4.  domain_name (obligatorisch)</br>    </br>5.  device_info/X-device-info (obligatorisch)</br>6.  deviceType</br>    </br>7.  deviceUser (veraltet)</br>    </br>8.  appId (veraltet)</br>    </br>9.  generic_data (optional) | POST | Die erfolgreiche Antwort lautet 204 Kein Inhalt. Dies bedeutet, dass das Token erfolgreich erstellt wurde und für die Autorisierungsflüsse verwendet werden kann. | 204 - Kein Inhalt   </br>400 - Fehlerhafte Anfrage |

<div>


| Eingabeparameter | Beschreibung |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Requestor_id | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| mso_id | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| domain_name | Der Domain-Name, für den ein Token gewährt wird. Dieser wird mit den Domains des Dienstleisters verglichen, wenn ein Autorisierungs-Token erteilt wird. |
| device_info/</br></br>X-device-info | Informationen zu Streaming-Geräten.</br></br>**Hinweis**: Dies kann als URL-Parameter an device_info übergeben werden, sollte jedoch aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen für eine GET-URL als X-Device-Info im HTTP-Header übergeben werden. </br></br>Vollständige Details finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter richtig festgelegt ist, bietet ESM Metriken an, die [nach Gerätetyp aufgeschlüsselt) ](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) Verwendung von Clientless sind, sodass verschiedene Arten der Analyse für z. B. Roku, AppleTV, Xbox usw. durchgeführt werden können.</br></br>Siehe [Vorteile der Verwendung von Client-losen Gerätetypparametern ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: der Parameter wird durch „device_info“ ersetzt. |
| _deviceUser_ | Die Geräte-Benutzerkennung.</br></br>**Hinweis**: Falls verwendet, sollte deviceUser dieselben Werte wie in der Anfrage [Registrierungs-Code erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) haben. |
| _appId_ | Die Anwendungs-ID/-name. </br></br>**Hinweis**: device_info ersetzt diesen Parameter. Wenn verwendet, sollten `appId` dieselben Werte wie in der Anfrage [Registrierungs-Code erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) haben. |
| generic_data | Wird verwendet, um den Umfang des Tokens für die Übergabe der temporären Werbeaktion zu begrenzen. |


### [Zurück zur REST-API-Referenz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
