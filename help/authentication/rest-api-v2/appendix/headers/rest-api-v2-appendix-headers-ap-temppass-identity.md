---
title: Header - AP-TempPass-Identity
description: REST API V2 - Header - AP-TempPass-Identity
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 2%

---


# Header - AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

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
