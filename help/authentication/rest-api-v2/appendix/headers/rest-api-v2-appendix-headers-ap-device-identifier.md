---
title: Header - AP-Device-Identifier
description: REST API V2 - Header - AP-Device Identifier
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---


# Header - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Die Anfragekopfzeile <b>AP-Device-Identifier</b> enthält die Streaming-Geräte-ID, wie sie von der Clientanwendung erstellt wurde.

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;type&gt; &lt;identifier&gt;</td>
   </tr>
   <tr>
      <td>Kopfzeilentyp</td>
      <td>Anforderungs-Header</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Nein</td>
   </tr>
</table>

## Richtlinien {#directives}

<b>&lt;type></b>

Der Gerätekennungstyp.

Es gibt nur einen unterstützten Typ, wie unten dargestellt.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Typ</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>Fingerabdruck</td>
      <td>Die Geräte-ID besteht aus einer undurchsichtigen Kennung und wird von der Clientanwendung erstellt.</td>
   </tr>
</table>


<b>&lt;identifier></b>

Der `Base64-encoded` -Wert der Geräte-ID.

## Beispiel {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
