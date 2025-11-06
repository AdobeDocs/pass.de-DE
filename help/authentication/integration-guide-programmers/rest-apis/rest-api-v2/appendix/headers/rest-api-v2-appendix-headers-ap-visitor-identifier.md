---
title: Header - AP-Visitor-Identifier
description: REST API v2 - Kopfzeile - AP-Visitor-Identifier
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: 7d19319d83af0122624ab3b80b1f31fc0cbbf182
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
