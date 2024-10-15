---
title: Kopfzeile - Autorisierung
description: REST API V2 - Header - Authorization
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---


# Kopfzeile - Autorisierung {#header-authorization}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Der Anforderungsheader <b>Autorisierung</b> enthält das Zugriffstoken `Bearer` , das von der Clientanwendung für den Zugriff auf Adobe Pass-geschützte APIs benötigt wird.

Weitere Informationen zum Mechanismus für den Zugriff auf Adobe Pass-geschützte APIs finden Sie in der Dokumentation [Übersicht über die dynamische Client-Registrierung](../../../dcr-api/dynamic-client-registration-overview.md) .

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Autorisierung</b>: Träger&lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>Kopfzeilentyp</td>
      <td>Anforderungs-Header</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Ja</td>
   </tr>
</table>

## Richtlinien {#directives}

<b>&lt;access_token></b>

Der Zugriffstoken-Wert ist ein undurchsichtiger Wert mit einer begrenzten Live-Lebensdauer (z. B. 24 Stunden), der von Adobe Pass abgerufen werden muss, wie in der API-Dokumentation zum [Zugriffstoken abrufen](../../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben.

## Beispiele {#examples}

```JSON
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
