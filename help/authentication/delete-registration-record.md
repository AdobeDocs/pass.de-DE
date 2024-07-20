---
title: Registrierungsdatensatz löschen
description: Registrierungsauflistung löschen
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 1%

---

# Registrierungsdatensatz löschen {#delete-registration-record}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch den [Drosselmechanismus](/help/authentication/throttling-mechanism.md) begrenzt

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Beschreibung {#delete-record}

Löscht den reg-Code-Datensatz und gibt den reg-Code zur Wiederverwendung frei.

| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Beispiel:</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Anforderer-ID </br>    (Pfadkomponente)</br>2.  Registrierungs-Code </br>    (Pfadkomponente) | DELETE | Keines | 204 |

{style="table-layout:auto"}

</br>

| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| Registrierungscode | Der Registrierungscode-Wert, der auf dem Streaming-Gerät angezeigt wird (in den Authentifizierungsfluss einzugeben). |

{style="table-layout:auto"}

</br>

### [Zurück zur REST-API-Referenz](/help/authentication/rest-api-reference.md)
