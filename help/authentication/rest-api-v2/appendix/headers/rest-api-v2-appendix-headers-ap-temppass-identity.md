---
title: Header - AP-TempPass-Identity
description: REST API V2 - Header - AP-TempPass-Identity
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 3%

---


# Header - AP-TempPass-Identity {#header-ap-temppass-identity}

## Übersicht {#overview}

Der Anforderungsheader <b>AP-TempPass-Identity</b> enthält die Benutzeridentitätsdaten, die zum Erzielen des Promo-TempPass verwendet werden.

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>: &lt;user_identity_information&gt;</td>
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

<b>&lt;user_identity_information></b>

Der `Base64-encoded` -Wert für die mit dem Endbenutzer verknüpften Benutzeridentitätsdaten, denen ein temporärer Werbedruck gewährt werden muss.

## Beispiele {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
