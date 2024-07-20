---
title: MVPD-Liste bereitstellen
description: MVPD-Liste bereitstellen
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---

# MVPD-Liste bereitstellen {#provide-mvpd-list}

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

## Beschreibung {#description}

Gibt eine Liste der konfigurierten MVPDs für den Anfragenden zurück.

| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>Beispiel:</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass-Authentifizierung | 1. Antragsteller</br>    (Pfadkomponente)</br>_2.  deviceType (nicht mehr unterstützt)_ | GET | XML oder JSON mit einer Liste von MVPDs. | 200 |

{style="table-layout:auto"}


| Eingabeparameter | Beschreibung |
| --------------- | ------------------------------------------------------------- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| *deviceType* | Gerätetyp. |

{style="table-layout:auto"}

### Beispielantwort {#sample-response}

Wie vorhandene MVPD XML-Antwort auf /config Servlet

Hinweis: Alle MVPDs, die für die Verwendung von Platform SSO konfiguriert sind, verfügen über die folgenden zusätzlichen Eigenschaften innerhalb des entsprechenden Knotens (JSON/XML):

* **enablePlatformServices (boolean):**-Markierung, die angibt, ob dieser MVPD über Platform SSO integriert ist
* **boardingStatus (string):** -Markierung, die angibt, ob der MVPD Platform SSO (SUPPORTED) vollständig unterstützt oder ob der MVPD nur in der Plattformauswahl (PICKER) angezeigt wird
* **displayInPlatformPicker (boolean):** , sollte dieser MVPD in der Plattformauswahl angezeigt werden
* **platformMappingId (Zeichenfolge):** die Kennung dieses MVPD, wie von der Plattform bekannt
* **requiredMetadataFields (String-Array):** die Metadatenfelder des Benutzers, die bei einer erfolgreichen Anmeldung verfügbar sein sollen
