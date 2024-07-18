---
title: Initiate Logout
description: Abmeldung starten
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Initiate Logout {#initiate-logout}

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

Entfernen Sie AuthN- und AuthZ-Token aus dem Speicher.


| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logout | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Antragsteller</br>2.  deviceId (erforderlich)</br>3.  device_info/X-Device-Info (erforderlich)</br>4.  _deviceType_</br> 5.  _deviceUser_ (Veraltet)</br>6.  _appId_ (veraltet) | DELETE | Keines | 204 |


| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| device_info/</br></br>X-Device-Info | Informationen zum Streaming-Gerät.</br></br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br></br>Weitere Informationen finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br>Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die bei Verwendung von ClientLess [nach Gerätetyp aufgeschlüsselt sind](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type), sodass für Roku, AppleTV, Xbox usw. unterschiedliche Analysetypen durchgeführt werden können.</br></br>Siehe [Vorteile der Verwendung des Parameters für Client-lose Gerätetypen in Pass-Metriken ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Hinweis**: Der Parameter device_info ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts.</br></br>**Hinweis**: Bei Verwendung von sollte deviceUser dieselben Werte wie in der Anforderung [Registrierungscode erstellen](/help/authentication/registration-code-request.md) aufweisen. |
| _appId_ | Die Anwendungs-ID/der Name. </br></br>**Hinweis**: Der Parameter device_info ersetzt diesen. Bei Verwendung von sollte `appId` dieselben Werte wie in der Anfrage [Registrierungscode erstellen](/help/authentication/registration-code-request.md) aufweisen. |

>[!IMPORTANT]
> 
>Der Abmelde-Aufruf hat derzeit die folgende Einschränkung: Er löscht die AuthN- und AuthZ-Token aus dem Speicher (d. h. auf der Seite &quot;Programmer/Adobe Pass-Authentifizierung&quot;), **ruft jedoch nicht den MVPD-Abmelde-Endpunkt auf.**
