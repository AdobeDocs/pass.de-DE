---
title: Temporären Pass zurücksetzen
description: Temporären Pass zurücksetzen
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# Temporären Pass zurücksetzen {#reset-temp-pass}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Stellen Sie vor der Verwendung der API zum Zurücksetzen der temporären Übergabe sicher, dass die folgenden Voraussetzungen erfüllt sind:
>
> * Rufen Sie die Client-Anmeldeinformationen ab, wie in der API[Dokumentation zum Abrufen von Client](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)Anmeldeinformationen beschrieben.
> * Rufen Sie das Zugriffstoken ab, wie in der API[Dokumentation zum Abrufen des Zugriffstokens ](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Weitere Informationen zum Erstellen einer registrierten [ und zum Herunterladen der Software-Anweisung finden ](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) in der Dokumentation Übersicht zur Dynamic Client-Registrierung .

Zum **Zurücksetzen eines bestimmten temporären Passes für ein Gerät oder alle Geräte** stellt die Adobe Pass-Authentifizierung Programmierern eine *öffentliche* Web-API zur Verfügung:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **protocol:** HTTPS
* **host:**
   * Version - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Path:** /reset-tempass/v3/reset
* **Abfrageparameter:** device_id (wenn kein Wert angegeben wird, standardmäßig all), Requestor_id (erforderlich), mvpd_id (erforderlich), appId, deviceUser, Umgebung
* **Headers:** Autorisierung: Bearer &lt;access_token_here>
* **Antwort:**
   * Erfolg - HTTP 204
   * Failure:xw
      * HTTP 400 für eine falsche Anfrage
      * HTTP 401, wenn der Zugriff verweigert wird. Der Client MUSS ein neues Zugriffs-Token anfordern.
      * HTTP 403, wenn die Client-ID keine Anfragen mehr ausführen darf. Es sollten neue Client-Anmeldeinformationen generiert werden.


Beispiel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

Zum **Zurücksetzen des flexiblen temporären Übergangs für einen generischen Schlüssel oder alle Schlüssel** stellt die Adobe Pass-Authentifizierung Programmierern eine *öffentliche* Web-API bereit:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **protocol:** HTTPS
* **host:**
   * Version - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Path:** /reset-tempass/v3/reset/generic
* **Abfrageparameter:** key (wenn kein Wert angegeben wird, standardmäßig all), Requestor_id (erforderlich), mvpd_id (erforderlich), environment
* **Headers:** Autorisierung: Bearer &lt;access_token_here>
* **Antwort:**
   * Erfolg - HTTP 204
   * Fehler:
      * HTTP 400 für eine falsche Anfrage
      * HTTP 401, wenn der Zugriff verweigert wird. Der Client MUSS ein neues Zugriffs-Token anfordern.
      * HTTP 403, wenn die Client-ID keine Anfragen mehr ausführen darf. Es sollten neue Client-Anmeldeinformationen generiert werden.


Beispielsweise können Sie für den *Generischer Schlüssel* einen E-Mail-Adressen-Hash festlegen (für
MVPDs mit temporärem Pass (die diese Art von Funktion unterstützen) liefern die
nach HTTP-Aufruf (die E-Mail `user@domain.com` SHA-256
Hash ist `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
