---
title: Header - AP-Visitor-Identifier
description: REST API v2 - Kopfzeile - AP-Visitor-Identifier
exl-id: b4f8e2a1-9c7d-4e3a-8f5b-2d1c6e9a4b7f
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# Header - AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Überblick {#overview}

Der <b>AP-Visitor-Identifier</b>-Header enthält die `ECID`, die das Client-Programm benötigt, um eine Besucherin oder einen Besucher über Adobe Experience Cloud-Lösungen hinweg eindeutig zu identifizieren.

Weitere Informationen zur Verwendung von ECID bei der Adobe Pass-Authentifizierung finden Sie in der Dokumentation [Verwenden der Experience Cloud-ID bei der Adobe Pass](../../../../features-premium/analytics/exp-cloud-id-authn.md).

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b>: &lt;visitor_identifier&gt;</td>
   </tr>
   <tr>
      <td>Kopfzeilentyp</td>
      <td>Anfrage-Header</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Nein</td>
   </tr>
</table>

## Beispiele {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
