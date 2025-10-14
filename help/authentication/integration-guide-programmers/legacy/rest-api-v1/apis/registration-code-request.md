---
title: Anmeldeseite
description: Anmeldeseite
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 0%

---

# (Veraltete) Registrierungsseite {#registration-page}

## REST-API-Endpunkte {#clientless-endpoints}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselungsmechanismus) &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## Beschreibung {#create-reg-code-svc}

Gibt den zufällig generierten Registrierungs-Code und den Anmeldeseiten-URI zurück.

| Endpunkt | Called <br>by | Eingabe   <br>Parameter | HTTP <br>Methode | Antwort | HTTP <br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>Beispiel:<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Streaming-App<br>oder<br>Programmierer-Service | 1. <br>    (Pfadkomponente)<br>2.  deviceId (gehasht)   <br>    (mandatory)<br>3.  device_info/X-device-info (obligatorisch)<br>4.  mvpd (optional)<br>5.  TTL (optional)<br> | POST | XML oder JSON mit einem Registrierungs-Code und Informationen oder Fehlerdetails, falls nicht erfolgreich. Siehe Beispiele unten. | 201 |

{style="table-layout:auto"}

| Eingabeparameter | Typ | Beschreibung |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autorisierung | Header-<br>: Bearer &lt;access_token> | DCR-Zugriffstoken |
| Akzeptieren | Header-<br>: application/json | Geben Sie an, welchen Inhaltstyp der Client verstehen soll |
| Antragsteller | Abfrageparameter | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Abfrageparameter | Die Geräte-ID-Bytes. |
| device_info/<br>X-device-info | device_info: Hauptteil <br> X-Device-info: Header | Informationen zu Streaming-Geräten.<br>**Hinweis**: Dies kann als URL-Parameter an device_info übergeben werden, sollte jedoch aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen für eine GET-URL als X-Device-Info im HTTP-Header übergeben werden. <br>Vollständige Details finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| mvpd | Abfrageparameter | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| TTL | Abfrageparameter | Wie lange diese Regcode-Zeit in Sekunden dauern soll.<br>**Hinweis**: Der maximal zulässige Wert für ttl beträgt 36000 Sekunden (10 Stunden). Höhere Werte führen zu einer 400-HTTP-Antwort (fehlerhafte Anfrage). Wenn `ttl` leer gelassen wird, legt die Adobe Pass-Authentifizierung den Standardwert von 30 Minuten fest. |
| _deviceType_ | Abfrageparameter | Veraltet, sollte nicht mehr verwendet werden. |
| _deviceUser_ | Abfrageparameter | Veraltet, sollte nicht mehr verwendet werden. |
| _appId_ | Abfrageparameter | Veraltet, sollte nicht mehr verwendet werden. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**IP-Adresse des Streaming-Geräts**
><br>
>Bei Client-zu-Server-Implementierungen wird die IP-Adresse des Streaming-Geräts mit diesem Aufruf implizit gesendet.  Bei Server-zu-Server-Implementierungen, bei denen der **regcode**-Aufruf vom Programmierdienst und nicht vom Streaming-Gerät erfolgt, ist der folgende Header erforderlich, um die IP-Adresse des Streaming-Geräts zu übergeben:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>wobei `<streaming\_device\_ip>` die öffentliche IP-Adresse des Streaming-Geräts ist.
><br><br>
>Beispiel : <br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
><br>

### Antwort-JSON


#### JSON-BEISPIELE für Registrierungs-Code

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| Elementname | Beschreibung |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| ID | Vom Registrierungs-Code-Service generierte UUID |
| Code | Vom Registrierungs-Code-Service generierter Registrierungs-Code |
| Antragsteller | Antragsteller-ID |
| mvpd | MVPD-ID |
| Erzeugt | Zeitstempel der Erstellung des Registrierungs-Codes (in Millisekunden seit dem 1. Januar 1970 GMT) |
| Expires | Zeitstempel, wann der Registrierungscode abläuft (in Millisekunden seit dem 1. Januar 1970 GMT) |
| deviceId | Eindeutige Base64-Geräte-ID |
| info:deviceId | Base64-Gerätetyp |
| info:deviceInfo | Base64 Normalisierte Geräteinformationen basieren auf Informationen, die von Benutzeragenten, X-Device-Info oder device_info empfangen wurden |
| info:userAgent | Von der Anwendung gesendeter Benutzeragent |
| info:originalUserAgent | Von der Anwendung gesendeter Benutzeragent |
| info:authorizationType | OAUTH2 für Aufrufe mit DCR |
| info:sourceApplicationInformation | Anwendungsinformationen wie im DCR konfiguriert |

{style="table-layout:auto"}


<br>

### Beispiel für eine JSON-Fehlermeldung (#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

