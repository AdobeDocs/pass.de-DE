---
title: MVPD-Liste bereitstellen
description: MVPD-Liste bereitstellen
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 2%

---

# (Legacy) MVPD-Liste bereitstellen {#provide-mvpd-list}

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

## Beschreibung {#description}

Gibt die Liste der konfigurierten MVPDs für den Anforderer aus.

| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>Beispiel:</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass-Authentifizierung | 1. Antragsteller</br>    (Pfadkomponente)</br>_2.  deviceType (veraltet)_ | GET | XML oder JSON mit einer Liste von MVPDs. | 200 |

{style="table-layout:auto"}


| Eingabeparameter | Beschreibung |
| --------------- | ------------------------------------------------------------- |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| *deviceType* | Gerätetyp. |

{style="table-layout:auto"}

### Beispielantwort {#sample-response}

Wie bestehende MVPD XML-Antwort auf das Servlet /config

Hinweis: Alle MVPDs, die für die Verwendung von Platform SSO konfiguriert sind, haben die folgenden zusätzlichen Eigenschaften in ihrem entsprechenden Knoten (JSON/XML):

* **enablePlatformServices (Boolesch):** Flag, das angibt, ob diese MVPD über Platform-SSO integriert ist
* **boardingStatus (Zeichenfolge):** Flag, das angibt, ob MVPD Platform SSO vollständig unterstützt (UNTERSTÜTZT) oder ob die MVPD nur in der Plattformauswahl (Auswahl) angezeigt wird
* **displayInPlatformPicker (Boolescher Wert):** sollte diese MVPD in der Plattformauswahl angezeigt werden
* **platformMappingId (Zeichenfolge):** die Kennung dieser MVPD, die von der Plattform als „Platform“ bezeichnet wird
* **requiredMetadataFields (Zeichenfolgen-Array):** Die Benutzer-Metadatenfelder, die bei einer erfolgreichen Anmeldung verfügbar sein sollen
