---
title: Benutzermetadaten
description: Benutzermetadaten
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# Benutzermetadaten {#user-metadata}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## REST-API-Endpunkte {#clientless-endpoints}

`<REGGIE_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Abrufen von Metadaten, die MVPD über den authentifizierten Benutzer freigegeben hat.


| Endpunkt | aufgerufen  </br>von | Eingabe   </br>Parameter | HTTP  </br>Methode | Reaktion | HTTP  </br>Reaktion |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Streaming-App</br></br>oder</br></br>Programmiererdienst | 1. Antragsteller</br>2.  deviceId (Obligatorisch)</br>3.  device_info/X-Device-Info (erforderlich)</br>4.  deviceType</br>5.  deviceUser (nicht mehr unterstützt)</br>6.  appId (nicht mehr unterstützt) | GET | XML oder JSON, die Benutzermetadaten oder Fehlerdetails enthalten, falls dies nicht erfolgreich war. | 200 - Erfolg<p>404 - Keine Metadaten gefunden<p>412 - Ungültiges AuthN-Token (z. B. abgelaufenes Token) |


| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| device_info/<p>X-Device-Info | Informationen zum Streaming-Gerät.</br></br> **Hinweis:** Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br></br> Weitere Informationen finden Sie unter [Weitergeben von Geräte- und Verbindungsinformationen](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br></br> Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die [aufgeschlüsselt nach Gerätetyp](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) bei Verwendung von ClientLess, sodass verschiedene Arten der Analyse für z. B. Roku, AppleTV, Xbox usw. durchgeführt werden können.</br></br> Siehe [Vorteile der Verwendung des Parameters für Client-lose Gerätetypen in Pass-Metriken](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Hinweis:** Die `device_info` ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts.</br></br> **Hinweis:** Falls verwendet, `deviceUser` sollte dieselben Werte wie im [Registrierungscode erstellen](/help/authentication/registration-code-request.md) -Anfrage. |
| _appId_ | Die Anwendungs-ID/der Name. </br></br> **Hinweis:** Die `device_info` ersetzt diesen Parameter. Falls verwendet, `appId` sollte dieselben Werte wie im [Registrierungscode erstellen](/help/authentication/registration-code-request.md) -Anfrage. |

>[!NOTE]
> 
>Benutzer-Metadateninformationen sollten nach Abschluss des Authentifizierungsflusses verfügbar sein, können jedoch je nach MVPD und Metadatentyp über den Autorisierungsfluss aktualisiert werden.




## Beispielantwort {#sample-response}

Nach einem erfolgreichen Aufruf antwortet der Server mit einem XML- (Standard-) oder JSON-Objekt mit einer Struktur, die der unten dargestellten ähnelt:


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

Im Stammverzeichnis des Objekts befinden sich drei Knoten:

* *aktualisiert*: Gibt einen UNIX-Zeitstempel an, der das letzte Mal angibt, dass die Metadaten aktualisiert wurden. Diese Eigenschaft wird beim Generieren der Metadaten während der Authentifizierungsphase zunächst vom Server festgelegt. Nachfolgende Aufrufe (nachdem die Metadaten aktualisiert wurden) führen zu einem inkrementierten Zeitstempel.
* *data*: enthält die tatsächlichen Metadatenwerte.
* *verschlüsselt*: ein Array, das die verschlüsselten Eigenschaften auflistet. Um einen bestimmten Metadatenwert zu entschlüsseln, muss der Programmierer eine Base64-Dekodierung für die Metadaten durchführen und dann eine RSA-Entschlüsselung auf den resultierenden Wert anwenden. Dabei muss er den eigenen privaten Schlüssel verwenden (Adobe verschlüsselt die Metadaten auf dem Server mithilfe des öffentlichen Zertifikats des Programmierers).

Im Fall eines Fehlers gibt der Server ein XML- oder JSON-Objekt zurück, das eine detaillierte Fehlermeldung angibt.

Weitere Informationen finden Sie unter [Benutzermetadaten](/help/authentication/user-metadata-feature.md).

[Zurück zur REST-API-Referenz](/help/authentication/rest-api-reference.md)
