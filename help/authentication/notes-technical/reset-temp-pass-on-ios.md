---
title: Zurücksetzen des Temp-Übergangs auf iOS
description: Zurücksetzen des Temp-Übergangs auf iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---

# Zurücksetzen des Temp-Übergangs auf iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

Die iOS Demo App verfügt über einen speziellen Bildschirm zum Zurücksetzen der TTL für den Vorübergehenden Pass. Die folgenden Informationen sind für den Reset-Vorgang erforderlich:

- **Umgebung:** gibt den Server-Endpunkt Adobe Pay-TV Pass an, der den Netzwerkaufruf Temp Pass zurücksetzen erhält. Mögliche Werte: **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **Release** (*mgmt.auth.adobe.com*) oder **Benutzerdefiniert** (für interne Adobe-Tests reserviert).
- **OAuth2-Trägertoken:** Das OAuth2-Token ist erforderlich, um den Programmierer für die Adobe Pay-TV-Authentifizierung zu autorisieren. Ein solches Token kann vom dedizierten Pay-TV-Authentifizierungs-OAuth2-Endpunkt abgerufen werden (z. B. *curl -u &quot;\&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_access?grant\_type=client\_credentials&quot;*).
- **Anforderer-ID:** Die eindeutige ID für den aktuellen Programmierer. Dieser Wert wird vom Hauptbildschirm der Demo App (Feld Anforderer ) gelesen.
- **Temp Pass ID:** die eindeutige ID für den Temp Pass MVPD.
- **Geräte-ID:** von der Demo App berechnete Hash-Geräte-ID.
- **Generischer Schlüssel:** Einige temporäre Pass-MVPDs (d. h. die nächste erweiterbare Funktion für den Temp-Pass) unterstützen einen generischen Schlüssel zum Zurücksetzen des Temp-Passes (neben der Geräte-ID).

Alle oben genannten Parameter (außer dem *generischen Schlüssel*) sind obligatorisch. Im Folgenden finden Sie ein Beispiel für Parameter und den zugehörigen Netzwerkaufruf, die von der Demo App ausgeführt werden (das Beispiel lautet wie folgt: *curl *command):

- **Umgebung:** Release (*mgmt.auth.adobe.com*)
- **OAuth2-Trägertoken:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **Programmierer-ID:** REF
- **Temp Pass ID:** TempPassREF
- **Geräte-ID:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa 639991
- **Generischer Schlüssel:** null (kein Wert angegeben)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

Es wird eine DELETE-HTTP-Anfrage an den Endpunkt **/reset** gesendet, wobei das *OAuth2-Träger-Token* im Autorisierungs-Header und die *Geräte-ID*, die *Anforderer-ID* und die *Temp Pass-ID (MVPD ID)* als Parameter übergeben werden.

Wenn der Programmierer einen Wert für den *generischen Schlüssel* bereitstellt, wird ein weiterer HTTP-Aufruf ausgeführt (diesmal an den Endpunkt **/reset/generic** ), wobei der *generische Schlüssel* innerhalb des Anforderungsparameters *key* übergeben wird.

Setzen Sie beispielsweise den generischen Schlüssel *1} auf einen E-Mail-Adressen-Hash (für
Temp Pass-MVPDs, die diese Art von Funktionalität unterstützen) liefern die
nach HTTP-Aufruf (die E-Mail ist `user@domain.com` SHA-256)
hash ist `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):*

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Es ist wichtig zu betonen, dass das Zurücksetzen von Temp Pass in der Demo App möglicherweise nicht die gleiche Wirkung für eine Programmierer-App auf demselben Gerät hat. Dies liegt daran, dass die Geräte-ID (wie von der Demo App und dem AccessEnabler berechnet) möglicherweise nicht für alle Anwendungen auf dem Gerät identisch ist:

- iOS 6 und niedriger: Die Geräte-ID wird mit der MAC-Adresse berechnet (die für alle Apps eindeutig ist). Wenn Sie also die Option &quot;Temp Pass&quot;in der Demo App zurücksetzen, wird sie in allen anderen Programmer-Apps auf dem Gerät zurückgesetzt.

- iOS 7 und höher: Die Geräte-ID wird anhand des IDFV-Werts (ID für Anbieter) berechnet, der für alle Anwendungen mit demselben Bundle-ID-Präfix eindeutig ist (d. h. für alle Komponenten außer der letzten). Da die Demo-App und eine Programmierer-App unterschiedliche Bundle-IDs aufweisen, hat das Zurücksetzen von Temp Pass in der Demo App keine Auswirkungen auf eine Programmierer-App.

Der letzte Anwendungsfall (iOS 7 und höher) ist der häufigste. Daher sollten wir sehen, wie Programmierer in dieser Situation den Temp Pass für ihre Apps zurücksetzen können. Es gibt mehrere Optionen:

1. Portieren Sie den Code von der Demo App an die Programmer-App. Die Klassen *TempPassResetViewController* und *DeviceIdDemoApp* enthalten die Kernlogik zum Zurücksetzen von Temp Pass. Sie können einfach geändert und in die Programmer-App aufgenommen werden.

1. Führen Sie die HTTP-Anfrage zum Zurücksetzen des Temp-Übergangs mit *curl* aus. Der Parameter device\_Id kann abgerufen werden, indem der IDFV der Programmer-App berechnet und ein SHA-256-Hash darauf angewendet wird (Beispielcode in der Klasse *DeviceIdDemoApp* ).

1. Führen Sie einfach das Zurücksetzen aus der Demo App durch, indem Sie den Hash-IDFV der App des Programmierers als *generischen Schlüssel* angeben. Dies führt zu zwei Netzwerkaufrufen: einem zum Zurücksetzen des Temp Pass für die Demo-App (für den Programmierer nicht relevant) und einem zum Zurücksetzen des Temp Pass für die Programmer-App.

Alle obigen Optionen sind ähnlich, es liegt beim Programmierer, eine zu wählen, abhängig von der einfachen Implementierung.
