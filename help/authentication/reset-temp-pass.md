---
title: Zurücksetzen des Vorübergangs
description: Zurücksetzen des Vorübergangs
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 28d432891b7d7855e83830f775164973e81241fc
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# Zurücksetzen des Vorübergangs {#reset-temp-pass}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.
>
>Um die API zum Zurücksetzen des Temp-Pass zu verwenden, müssen Sie Folgendes tun:
>- Bitten Sie das Supportteam um eine Softwareanweisung für Ihre registrierte Anwendung
>- Abrufen eines Zugriffstokens basierend auf der [dynamischen Client-Registrierung](dynamic-client-registration.md)
> 

Damit **einen bestimmten Temp-Pass zurücksetzen**, stellt die Adobe Pass-Authentifizierung Programmierern eine Web-API für *public* bereit:

- **Umgebung:** gibt den Server-Endpunkt Adobe Pay-TV Pass an, der den Netzwerkaufruf Temp Pass zurücksetzen erhält. Mögliche Werte: **Prequal** (*mgmt-prequal.auth.adobe.com*), **Release** (*mgmt.auth.adobe.com*) oder **Benutzerdefiniert** (für interne Adobe-Tests reserviert).
- **OAuth2-Zugriffstoken:** Das OAuth2-Token ist erforderlich, um den Programmierer für die Adobe Pay-TV-Authentifizierung zu autorisieren. Ein solches Token kann von [Dynamic Client Registration](dynamic-client-registration.md) abgerufen werden.
- **Temp Pass ID:** Die eindeutige ID für den MVPD für den Temp Pass, der zurückgesetzt werden soll.(Ein Programmierer kann mehrere MVPDs mit Temp Pass verwenden und einen bestimmten zurücksetzen.)
- **Generischer Schlüssel:** einige temporäre Pass-MVPDs (d. h. [Promotional temp pass](promotional-temp-pass.md)).

Alle oben genannten Parameter (außer dem *generischen Schlüssel*) sind obligatorisch. Im Folgenden finden Sie ein Beispiel für Parameter und den zugehörigen Netzwerkaufruf (das Beispiel hat die Form eines *curl *command):

- **Umgebung:** Release (*mgmt.auth.adobe.com*)
- **OAuth2-Zugriffstoken:** &lt;access_token> von [Dynamische Client-Registrierung](dynamic-client-registration.md)
- **Programmierer-ID:** REF
- **Temp Pass ID:** TempPassREF
- **Generischer Schlüssel:** null (kein Wert angegeben)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

Es wird eine DELETE-HTTP-Anfrage an den Endpunkt **/reset** gesendet, wobei das *OAuth2 Access Token* im Autorisierungs-Header und die *Geräte-ID*, die *Anforderer-ID* und die *Temp Pass ID (MVPD ID)* als Parameter übergeben werden.

Wenn der Programmierer einen Wert für den *generischen Schlüssel* bereitstellt, wird ein weiterer HTTP-Aufruf ausgeführt (diesmal an den Endpunkt **/reset/generic** ), wobei der *generische Schlüssel* innerhalb des Anforderungsparameters *key* übergeben wird.

Setzen Sie beispielsweise den generischen Schlüssel *1} auf einen E-Mail-Adressen-Hash (für
Temp Pass-MVPDs, die diese Art von Funktionalität unterstützen) liefern die
nach HTTP-Aufruf (die E-Mail ist `user@domain.com` SHA-256)
hash ist `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):*

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Damit **einen bestimmten Temp-Pass für alle Geräte zurücksetzen**, stellt die Adobe Pass-Authentifizierung Programmierern eine Web-API für *public* bereit:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>Die obige URL ersetzt die vorherige Reset-API. Die alte Reset-API (v1) wird nicht mehr unterstützt.

- **Protokoll:** HTTPS
- **Host:**
   - Release - mgmt.auth.adobe.com
   - Prequal - mgmt-prequal.auth.adobe.com
- **Pfad:** /reset-tempass/v3/reset
- **Abfrageparameter:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **Header:** Authorization: Bearer &lt;access_token_here>
- **Antwort:**
   - Erfolg - HTTP 204
   - Fehler:
      - HTTP 400 für eine falsche Anforderung
      - HTTP 401 , wenn der Zugriff verweigert wird. Der Client MUSS ein neues access_token anfordern.
      - HTTP 403 , wenn die Client-ID keine Anfragen mehr ausführen darf. Es sollten neue Client-Anmeldeinformationen generiert werden.


Beispiel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
