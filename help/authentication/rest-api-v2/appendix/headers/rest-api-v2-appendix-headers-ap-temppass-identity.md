---
title: Header - AP-TempPass-Identity
description: REST API V2 - Header - AP-TempPass-Identity
exl-id: a6238a58-a3f1-495d-a9d1-82475f5ffc60
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
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

X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
