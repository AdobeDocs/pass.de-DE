---
title: Zurücksetzen des Vorübergangs
description: Zurücksetzen des Vorübergangs
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# Zurücksetzen des Vorübergangs {#reset-temp-pass}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Stellen Sie vor Verwendung der API zum Zurücksetzen des Temp Pass sicher, dass die folgenden Voraussetzungen erfüllt sind:
>
> * Rufen Sie die Client-Anmeldeinformationen ab, wie in der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) beschrieben.
> * Rufen Sie das Zugriffstoken ab, wie in der API-Dokumentation [Zugriffstoken abrufen](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben.
>
> Weitere Informationen zum Erstellen einer registrierten Anwendung und Herunterladen der Softwareanweisung finden Sie in der Dokumentation zur [Übersicht über die dynamische Client-Registrierung](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

Damit **einen bestimmten Temp-Pass für ein Gerät oder alle Geräte zurücksetzen**, stellt die Adobe Pass-Authentifizierung Programmierern eine Web-API für *public* bereit:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **Protokoll:** HTTPS
* **Host:**
   * Release - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Pfad:** /reset-tempass/v3/reset
* **Abfrageparameter:** device_id(wenn kein Wert angegeben wird, ist standardmäßig &quot;all&quot;), requestor_id (erforderlich), mvpd_id (erforderlich), appId, deviceUser, environment
* **Header:** Authorization: Bearer &lt;access_token_here>
* **Antwort:**
   * Erfolg - HTTP 204
   * Fehler:xw
      * HTTP 400 für eine falsche Anforderung
      * HTTP 401 , wenn der Zugriff verweigert wird. Der Client MUSS ein neues access_token anfordern.
      * HTTP 403 , wenn die Client-ID keine Anfragen mehr ausführen darf. Es sollten neue Client-Anmeldeinformationen generiert werden.


Beispiel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

Um den flexiblen Temp-Pass für einen generischen Schlüssel oder alle Schlüssel zurücksetzen zu können, stellt die Adobe Pass-Authentifizierung Programmierern eine Web-API für *public* bereit:****

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **Protokoll:** HTTPS
* **Host:**
   * Release - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Pfad:** /reset-tempass/v3/reset/generic
* **Abfrageparameter:** key (wenn kein Wert angegeben wird, ist standardmäßig all), requestor_id (erforderlich), mvpd_id (erforderlich), environment
* **Header:** Authorization: Bearer &lt;access_token_here>
* **Antwort:**
   * Erfolg - HTTP 204
   * Fehler:
      * HTTP 400 für eine falsche Anforderung
      * HTTP 401 , wenn der Zugriff verweigert wird. Der Client MUSS ein neues access_token anfordern.
      * HTTP 403 , wenn die Client-ID keine Anfragen mehr ausführen darf. Es sollten neue Client-Anmeldeinformationen generiert werden.


Setzen Sie beispielsweise den generischen Schlüssel *1} auf einen E-Mail-Adressen-Hash (für
Temp Pass-MVPDs, die diese Art von Funktionalität unterstützen) liefern die
nach HTTP-Aufruf (die E-Mail ist `user@domain.com` SHA-256)
hash ist `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):*

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
