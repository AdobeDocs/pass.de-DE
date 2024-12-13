---
title: Temporären Pass von iOS zurücksetzen
description: Temporären Pass von iOS zurücksetzen
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# (Legacy) Temporären Pass auf iOS zurücksetzen {#reset-temp-pass-on-ios}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>

Die iOS-Demo-App enthält einen speziellen Bildschirm zum Zurücksetzen der TTL für den temporären Pass. Für den Rücksetzvorgang sind die folgenden Informationen erforderlich:

- **Umgebung:** gibt den Adobe-Pay-TV-Pass-Server-Endpunkt an, der den Netzwerkaufruf „Temp Pass zurücksetzen“ erhält. Mögliche Werte: **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **Release** (*mgmt.auth.adobe.com*) oder **Custom** (für das Adobe interner Tests reserviert).
- **OAuth2 Bearer Token:** das OAuth2 Token ist erforderlich, um den Programmierer für die Adobe-Pay-TV-Authentifizierung zu autorisieren. Ein solches Token kann vom dedizierten OAuth2-Endpunkt für die Pay-TV-Authentifizierung bezogen werden (z. B. *curl -u &quot;\&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accessToken?grant\_type=client\_credentials“*).
- **Anforderer-ID** Die eindeutige ID für den aktuellen Programmierer. Dieser Wert wird aus dem Hauptbildschirm der Demo-App (das Feld Anforderer ) gelesen.
- **Temp Pass ID:** die eindeutige ID für den Temp Pass MVPD.
- **Geräte-ID:** gehashte Geräte-ID, die von der Demo-App berechnet wird.
- **Generischer Schlüssel:** einige MVPDs mit Temp Pass (d. h. die nächste erweiterbare Funktion „Temp Pass„) unterstützen einen generischen Schlüssel zum Zurücksetzen des Temp Pass (neben der Geräte-ID).

Alle oben genannten Parameter (mit Ausnahme des *Generischer Schlüssel*) sind obligatorisch. Im Folgenden finden Sie ein Beispiel für Parameter und den zugehörigen Netzwerkaufruf, der von der Demo-App ausgeführt wird (das Beispiel ist in Form eines *curl*-Befehls):

- **Umgebung:** Version (*mgmt.auth.adobe.com*)
- **OAuth2 Bearer Token:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **Programmierer-ID:** REF
- **Temp Pass ID:** TempPassREF
- **Geräte-ID:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **Generischer Schlüssel:** null (kein Wert angegeben)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

Eine DELETE-HTTP-Anfrage wird an den Endpunkt **/reset** gesendet, wobei das *OAuth2 Bearer Token* im Autorisierungs-Header und die Parameter *Device ID*, *Requestor ID* und *Temp Pass ID (MVPD ID)* übergeben werden.

Wenn der Programmierer einen Wert für den *Generic Key* bereitstellt, wird ein weiterer HTTP-Aufruf ausgeführt (dieses Mal an den Endpunkt **/reset/generic**), wobei der *Generic Key* innerhalb des *key*-Anforderungsparameters übergeben wird.

Beispielsweise können Sie für den *Generischer Schlüssel* einen E-Mail-Adressen-Hash festlegen (für
MVPDs mit temporärem Pass (die diese Art von Funktion unterstützen) liefern die
nach HTTP-Aufruf (die E-Mail `user@domain.com` SHA-256
Hash ist `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Es ist wichtig zu betonen, dass das Zurücksetzen von Temp Pass in der Demo-App möglicherweise nicht dieselbe Wirkung für eine Programmierer-App auf demselben Gerät hat. Dies liegt daran, dass die Geräte-ID (wie von der Demo-App und dem AccessEnabler berechnet) möglicherweise nicht für alle Anwendungen auf dem Gerät gleich ist:

- iOS 6 und niedriger: Die Geräte-ID wird anhand der MAC-Adresse berechnet (die für alle Apps eindeutig ist). Wenn Sie also den Temp Pass in der Demo-App zurücksetzen, wird er in allen anderen Programmieranwendungen auf dem Gerät zurückgesetzt.

- iOS 7 und höher: Die Geräte-ID wird anhand des IDFV-Werts (ID für Anbieter) berechnet, der für alle Anwendungen mit demselben Paket-ID-Präfix (d. h. für alle Komponenten mit Ausnahme des letzten) eindeutig ist. Da die Demo-App und eine Programmierer-App unterschiedliche Bundle-IDs haben, hat das Zurücksetzen des temporären Passes in der Demo-App keine Auswirkungen auf eine Programmierer-App.

Der letzte Anwendungsfall (iOS 7 und höher) ist der häufigste. Schauen wir uns also an, wie Programmierer den Temp Pass für ihre Apps in dieser Situation zurücksetzen können. Es gibt mehrere Optionen:

1. Portieren Sie den Code aus der Demo-App in die Programmierer-App. Die Klassen *TempPassResetViewController* und *DeviceIdDemoApp* enthalten die Kernlogik zum Zurücksetzen des Temp Pass und können einfach geändert und in die Programmieranwendung aufgenommen werden.

1. Ausführen der HTTP-Anfrage zum Zurücksetzen des temporären Passes mit *curl*. Der Parameter device\_Id kann abgerufen werden, indem die IDFV der Programmieranwendung berechnet und ein SHA-256-Hash darauf angewendet wird (Beispielcode in der Klasse *DeviceIdDemoApp*).

1. Führen Sie einfach das Zurücksetzen über die Demo-App durch, indem Sie den gehashten IDFV der App des Programmierers als *Generischer Schlüssel* angeben. Dies führt zu zwei Netzwerkaufrufen: einem zum Zurücksetzen des temporären Passes für die Demo-App (für den Programmierer irrelevant) und einem zum Zurücksetzen des temporären Passes für die Programmierer-App.

Alle oben genannten Optionen sind ähnlich. Es liegt an dem Programmierer, eine je nach Einfachheit der Implementierung auszuwählen.
