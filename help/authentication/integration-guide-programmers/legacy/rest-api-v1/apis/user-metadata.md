---
title: Benutzermetadaten
description: Benutzermetadaten
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# (Veraltete) Benutzermetadaten {#user-metadata}

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

`<REGGIE_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Rufen Sie Metadaten ab, die MVPD über den authentifizierten Benutzer freigegeben hat.


| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Streaming-App</br></br>oder</br></br>Programmierer-Service | &#x200B;1. Antragsteller</br>2.  deviceId (obligatorisch)</br>3.  device_info/X-device-info (obligatorisch)</br>4.  deviceType</br>5.  deviceUser (veraltet)</br>6.  appId (veraltet) | GET | XML oder JSON mit Benutzermetadaten oder Fehlerdetails, falls nicht erfolgreich. | 200 - Erfolg<p>404 - Keine Metadaten gefunden<p>412 - Ungültiges AuthN-Token (z. B. abgelaufenes Token) |


| Eingabeparameter | Beschreibung |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| device_info/<p>x-device-info | Informationen zu Streaming-Geräten.</br></br> **Hinweis:** Dies kann als URL-Parameter an device_info übergeben werden, sollte jedoch aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen für eine GET-URL als X-Device-Info im HTTP-Header übergeben werden. </br></br> Details finden Sie unter [Übergeben von Geräte- und Verbindungsinformationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br> Wenn dieser Parameter richtig festgelegt ist, bietet ESM Metriken an, die [nach Gerätetyp aufgeschlüsselt) ](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics) Verwendung von Clientless sind, sodass verschiedene Arten der Analyse für z. B. Roku, AppleTV, Xbox usw. durchgeführt werden können.</br></br> Siehe [Vorteile der Verwendung von Client-losen Gerätetypparametern in Kennzahlen für ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br> **Hinweis:** Der `device_info` ersetzt diesen Parameter. |
| _deviceUser_ | Die Geräte-Benutzerkennung.</br></br> **Hinweis:** Wenn verwendet, sollten `deviceUser` dieselben Werte wie in der Anfrage [Registrierungs-Code erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) haben. |
| _appId_ | Die Anwendungs-ID/-name. </br></br> **Hinweis:** Der `device_info` ersetzt diesen Parameter. Wenn verwendet, sollten `appId` dieselben Werte wie in der Anfrage [Registrierungs-Code erstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) haben. |

>[!NOTE]
> 
>Informationen zu Benutzermetadaten sollten nach Abschluss des Authentifizierungsflusses verfügbar sein, können jedoch je nach MVPD und Metadatentyp im Autorisierungsfluss aktualisiert werden.




## Beispielantwort {#sample-response}

Nach einem erfolgreichen Aufruf antwortet der Server mit einem XML- (Standard) oder JSON-Objekt mit einer ähnlichen Struktur wie der unten gezeigten:


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

Im Stammverzeichnis des -Objekts befinden sich drei Knoten:

* *Aktualisiert*: Gibt einen UNIX-Zeitstempel an, der das letzte Mal darstellt, dass die Metadaten aktualisiert wurden. Diese Eigenschaft wird zunächst vom Server beim Generieren der Metadaten während der Authentifizierungsphase festgelegt. Nachfolgende Aufrufe (nachdem die Metadaten aktualisiert wurden) führen zu einem inkrementellen Zeitstempel.
* *data*: enthält die tatsächlichen Metadatenwerte.
* *Encrypted*: Ein Array, das die verschlüsselten Eigenschaften auflistet. Um einen bestimmten Metadatenwert zu entschlüsseln, muss der Programmierer eine Base64-Decodierung der Metadaten durchführen und dann eine RSA-Entschlüsselung auf den resultierenden Wert anwenden, indem er seinen eigenen privaten Schlüssel verwendet (Adobe verschlüsselt die Metadaten auf dem Server mithilfe des öffentlichen Zertifikats des Programmierers).

Im Falle eines Fehlers gibt der Server ein XML- oder JSON-Objekt zurück, das eine detaillierte Fehlermeldung angibt.

Weitere Informationen finden Sie unter [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

[Zurück zur REST-API-Referenz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
