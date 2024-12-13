---
title: Registrierungseintrag löschen
description: Registrierungseintrag löschen
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 1%

---

# (Legacy) Registrierungseintrag löschen {#delete-registration-record}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselungsmechanismus) ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Beschreibung {#delete-record}

Löscht den Registrierungscode-Datensatz und gibt den Registrierungscode zur Wiederverwendung frei.

| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Beispiel:</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Streaming-App</br></br>oder</br></br>Programmierer-Service | 1. Antragsteller-ID </br>    (Pfadkomponente)</br>2.  Registrierungs-Code </br>    (Pfadkomponente) | DELETE | Keine | 204 |

{style="table-layout:auto"}

</br>

| Eingabeparameter | Beschreibung |
| --- | --- |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| Registrierungs-Code | Der Registrierungs-Code-Wert, der auf dem Streaming-Gerät angezeigt wird (der in den Authentifizierungsfluss eingegeben werden soll). |

{style="table-layout:auto"}

</br>

### [Zurück zur REST-API-Referenz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
