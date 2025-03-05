---
title: Header - Autorisierung
description: REST API v2 - Kopfzeile - Autorisierung
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---


# Header - Autorisierung {#header-authorization}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#overview}

Der <b>Autorisierungs</b>-Anfrage-Header enthält das `Bearer` Zugriffstoken, das von der Client-Anwendung für den Zugriff auf durch Adobe Pass geschützte APIs benötigt wird.

Weitere Informationen zum Mechanismus für den Zugriff auf Adobe Pass-geschützte APIs finden Sie in der Dokumentation [Übersicht über die dynamische Client-Registrierung](../../../rest-api-dcr/dynamic-client-registration-overview.md).

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Authorization</b>: Bearer &lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>Kopfzeilentyp</td>
      <td>Anfrage-Header</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Ja</td>
   </tr>
</table>

## Anweisungen {#directives}

<b>&lt;access_token></b>

Der Wert des Zugriffstokens ist ein undurchsichtiger Wert mit einer begrenzten Lebensdauer (z. B. 24 Stunden), der von Adobe Pass abgerufen werden muss, wie in der API-Dokumentation zum [ des Zugriffstokens ](../../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

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
