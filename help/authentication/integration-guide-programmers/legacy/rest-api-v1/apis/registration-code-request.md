---
title: Registrierungsseite
description: Registrierungsseite
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# Registrierungsseite {#registration-page}

## REST-API-Endpunkte {#clientless-endpoints}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch den [Drosselmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) begrenzt

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## Beschreibung {#create-reg-code-svc}

Gibt zufällig generierten Registrierungscode und Anmeldeseiten-URI zurück.

| Endpunkt | <br>von aufgerufen | Eingabe   <br>Parameter | HTTP <br>Methode | Reaktion | HTTP <br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>Beispiel:<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Streaming-App<br>oder<br>Programmierer-Dienst | 1. Anforderer <br>    (Pfadkomponente)<br>2.  deviceId (Hash)   <br>    (Obligatorisch)<br>3.  device_info/X-Device-Info (erforderlich)<br>4.  mvpd (optional)<br>5.  ttl (optional)<br> | POST | XML oder JSON, die einen Registrierungs-Code und Informationen oder Fehlerdetails enthalten, falls diese nicht erfolgreich sind. Siehe Beispiele unten. | 201 |

{style="table-layout:auto"}

| Eingabeparameter | Typ | Beschreibung |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autorisierung | Header <br> Wert: Bearer &lt;access_token> | DCR-Zugriffstoken |
| Accept | Header <br> Value: application/json | angeben, welchen Inhaltstyp der Client verstehen sollte |
| Anfragender | Abfrageparameter | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Abfrageparameter | Die Geräte-ID-Bytes. |
| device_info/<br>X-Device-Info | device_info: body <br> X-Device-Info: Header | Informationen zum Streaming-Gerät.<br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. <br>Weitere Informationen finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| mvpd | Abfrageparameter | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| ttl | Abfrageparameter | Wie lange dieser Regcode in Sekunden leben soll.<br>**Hinweis**: Der maximal zulässige Wert für ttl beträgt 36.000 Sekunden (10 Stunden). Höhere Werte führen zu einer 400-HTTP-Antwort (ungültige Anfrage). Wenn `ttl` leer ist, setzt die Adobe Pass-Authentifizierung den Standardwert von 30 Minuten. |
| _deviceType_ | Abfrageparameter | Veraltet, sollte nicht verwendet werden. |
| _deviceUser_ | Abfrageparameter | Veraltet, sollte nicht verwendet werden. |
| _appId_ | Abfrageparameter | Veraltet, sollte nicht verwendet werden. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**IP-Adresse des Streaming-Geräts**
><br>
>Bei Client-zu-Server-Implementierungen wird die IP-Adresse des Streaming-Geräts implizit mit diesem Aufruf gesendet.  Bei Server-zu-Server-Implementierungen, bei denen der Aufruf von **regcode** als Programmiererdienst und nicht als Streaming-Gerät erfolgt, ist der folgende Header erforderlich, um die IP-Adresse des Streaming-Geräts zu übergeben:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>wobei `<streaming\_device\_ip>` die öffentliche IP-Adresse des Streaming-Geräts ist.
><br><br>
>Beispiel :<br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### Antwort-JSON


#### JSON-BEISPIELE FÜR Registrierungscode

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
| id | Vom Registrierungs-Code-Dienst generierte UUID |
| code | Registrierungs-Code, der vom Registrierungs-Code-Dienst generiert wird |
| Anfragender | Anforderer-ID |
| mvpd | Mvpd ID |
| generiert | Zeitstempel der Erstellung des Registrierungs-Codes (in Millisekunden seit dem 1. Januar 1970 GMT) |
| expires | Zeitstempel, wenn der Registrierungs-Code abläuft (in Millisekunden seit dem 1. Januar 1970 GMT) |
| deviceId | Base64 Eindeutige Geräte-ID |
| info:deviceId | Base64-Gerätetyp |
| info:deviceInfo | Base64 Normalisierte Geräteinformationen basierend auf Informationen, die von User-Agent, X-Device-Info oder device_info empfangen werden |
| info:userAgent | Benutzeragent, der von der Anwendung gesendet wird |
| info:originalUserAgent | Benutzeragent, der von der Anwendung gesendet wird |
| info:authorizationType | OAUTH2 für Aufrufe mit DCR |
| info:sourceApplicationInformation | Anwendungsinformationen wie im DCR konfiguriert |

{style="table-layout:auto"}


<br>

### Beispiel für JSON-Fehlermeldung (#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

