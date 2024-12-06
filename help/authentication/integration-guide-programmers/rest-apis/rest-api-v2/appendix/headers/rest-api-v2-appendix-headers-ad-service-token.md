---
title: Header - AD-Service-Token
description: REST API V2 - Header - AD-Service-Token
exl-id: 856f76fc-cde6-4b3f-81f7-deaa0df015dc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---

# Header - AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Der Anforderungsheader <b>AD-Service-Token</b> enthält die eindeutige Benutzer-ID als `JWS`, die von einem Identitätsdienst erhalten wurde, der außerhalb von Adobe Pass-Authentifizierungssystemen ausgeführt wird.

Diese Kopfzeile ist für die Verwendung in SSO-fähigen Flüssen (Single Sign-on) vorgesehen, die die Service Token-Methode nutzen.

Weitere Informationen zu den für die einmalige Anmeldung (SSO) aktivierten Flüssen, die die Service-Token-Methode nutzen, finden Sie in der Dokumentation [Single Sign-on mit Service-Token-Flüssen](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) .

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD-Service-Token</b>: &lt;unique_user_identifier&gt;</td>
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

<b>unique_user_identifier</b>

Die JSON-Websignatur (`JWS`), ein signiertes JSON-Web-Token (`JWT`) mit Informationen zur eindeutigen Benutzerkennung.

Der `JWT` weist die folgenden Attribute auf:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>is</td>
      <td>Die eindeutige Kennung, die mit der Entität verknüpft ist, die der Anwendung einen externen Identitätsdienst anbietet, um Single Sign-on (SSO) zu erreichen.</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>Die eindeutige Kennung für den Benutzer, die vom externen Identitätsdienst zurückgegeben wird.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>Die Zielgruppe, die "Adobe"sein sollte.</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>Die zum Zeitstempel für das aktuelle JWT herausgegeben.</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>Der Ablaufzeitstempel für das aktuelle JWT.</td>
   </tr>
</table>

Der `JWT` muss mit dem `SHA256withRSA`-Algorithmus signiert werden.

Der `JWT` muss mit einem privaten Schlüssel signiert werden, der Teil eines privaten RSA-Schlüssels ist - eines öffentlichen Schlüssels, der vom externen Identitätsdienst verwaltet wird.

Der öffentliche Schlüssel dieses Paares muss an die Adobe Pass-Authentifizierung übergeben werden, um `JWT` Token erkennen zu können, die mit dem oben genannten privaten Schlüssel signiert wurden.

## Beispiele {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
