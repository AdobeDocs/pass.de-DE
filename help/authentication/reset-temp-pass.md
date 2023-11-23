---
title: Zurücksetzen des Vorübergangs
description: Zurücksetzen des Vorübergangs
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Zurücksetzen des Vorübergangs {#reset-temp-pass}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.
>
>Um die API zum Zurücksetzen des Temp-Pass zu verwenden, müssen Sie Folgendes tun:
>- Bitten Sie das Supportteam um eine Softwareanweisung für Ihre registrierte Anwendung
>- Abrufen eines Zugriffstokens basierend auf [Dynamische Kundenregistrierung](dynamic-client-registration.md)
> 

Um **Zurücksetzen eines bestimmten Temporärs**, bietet die Adobe Pass-Authentifizierung Programmierern eine *öffentlich* Web-API:

- **Umgebung:** gibt den Adobe Pay-TV Pass-Server-Endpunkt an, der den zurückgesetzten Temp Pass-Netzwerkaufruf erhält. Mögliche Werte: **Prequal** (*mgmt-prequal.auth.adobe.com*), **Version** (*mgmt.auth.adobe.com*) oder **Benutzerdefiniert** (für interne Adobe-Tests reserviert).
- **OAuth2-Zugriffstoken:** Das OAuth2-Token ist für die Autorisierung des Programmierers für die Adobe Pay-TV-Authentifizierung erforderlich. Ein solches Token kann von [Dynamische Kundenregistrierung](dynamic-client-registration.md).
- **Temp Pass ID:** die eindeutige ID für den MVPD für den Temp-Pass, der zurückgesetzt werden soll.(Ein Programmierer kann mehrere MVPDs mit Temp Pass verwenden und einen bestimmten zurücksetzen.)
- **Generischer Schlüssel:** einige Temp Pass MVPDs (d.h. [vorübergehender Promotionsübergang](promotional-temp-pass.md)).

Alle oben genannten Parameter (mit Ausnahme der *Generischer Schlüssel*) sind obligatorisch. Im Folgenden finden Sie ein Beispiel für Parameter und den zugehörigen Netzwerkaufruf (das Beispiel hat die Form eines *curl *command):

- **Umgebung:** Release (*mgmt.auth.adobe.com*)
- **OAuth2-Zugriffstoken:** &lt;access_token> von [Dynamische Kundenregistrierung](dynamic-client-registration.md)
- **Programmierer-ID:** REF
- **Temp Pass ID:** TempPassREF
- **Generischer Schlüssel:** null (kein Wert angegeben)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

eine DELETE-HTTP-Anforderung an die **/reset** Endpunkt übergeben. *OAuth2-Zugriffstoken* im Autorisierungs-Header und dem *Geräte-ID*, *Anforderer-ID* und *Temp Pass ID (MVPD ID)* als Parameter.

Wenn der Programmierer einen Wert für die *Generischer Schlüssel*, wird ein weiterer HTTP-Aufruf ausgeführt (diesmal zum **/reset/generic** -Endpunkt) übergeben. *Generischer Schlüssel* innerhalb der *key* Anforderungsparameter.

Beispielsweise können Sie die *Generischer Schlüssel* zu einem E-Mail-Adressen-Hash (für MVPDs mit temporärem Pass, die diese Funktion unterstützen) führt zum folgenden HTTP-Aufruf (die E-Mail lautet `user@domain.com` der SHA-256-Hash lautet `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Um **Zurücksetzen eines bestimmten Temp Pass für alle Geräte**, bietet die Adobe Pass-Authentifizierung Programmierern eine *öffentlich* Web-API:

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
- **Kopfzeilen:** Authorization: Bearer &lt;access_token_here>
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
