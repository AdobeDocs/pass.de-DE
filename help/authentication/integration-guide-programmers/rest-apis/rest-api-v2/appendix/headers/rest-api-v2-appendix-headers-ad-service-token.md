---
title: Kopfzeile - AD-Service-Token
description: REST API v2 - Kopfzeile - AD-Service-Token
exl-id: 856f76fc-cde6-4b3f-81f7-deaa0df015dc
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---

# Kopfzeile - AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#overview}

Der <b>AD-Service-Token</b>-Anforderungsheader enthält die eindeutige Benutzerkennung, die von einem Identity Service abgerufen `JWS`, der außerhalb von Adobe Pass-Authentifizierungssystemen ausgeführt wird.

Dieser Header ist für die Verwendung in SSO-fähigen Flüssen (Single Sign-On) vorgesehen, die die Service-Token-Methode verwenden.

Weitere Informationen zu den für Single Sign-on (SSO) aktivierten Flüssen, die die Service-Token-Methode verwenden, finden Sie in der Dokumentation [Single Sign-on using Service Token Flows](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD-Service-Token</b>: &lt;unique_user_identifier&gt;</td>
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

## Anweisungen {#directives}

<b>unique_user_identifier</b>

Die JSON-Web-Signatur (`JWS`), ein signiertes JSON-Web-Token (`JWT`), das Informationen zur eindeutigen Benutzerkennung enthält.

Die `JWT` weist die folgenden Attribute auf:

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>ISS</td>
      <td>Die eindeutige Kennung, die mit der Entität verknüpft ist, die der Anwendung einen externen Identity Service für Single Sign-on (SSO) anbietet.</td>
   </tr>
   <tr>
      <td>Untergruppe</td>
      <td>Die eindeutige Kennung für den Benutzer, die vom externen Identity Service zurückgegeben wird.</td>
   </tr>
   <tr>
      <td>Hilfsmittel</td>
      <td>Die Zielgruppe, die "Adobe" sein sollte.</td>
   </tr>
   <tr>
      <td>IAT</td>
      <td>Der zum Zeitstempel für das aktuelle JWT ausgestellte .</td>
   </tr>
   <tr>
      <td>Exp</td>
      <td>Der Ablaufzeitstempel für das aktuelle JWT.</td>
   </tr>
</table>

Die `JWT` muss mit `SHA256withRSA` Algorithmus signiert werden.

Die `JWT` muss mit einem privaten Schlüssel signiert werden, der Teil eines Paares von privatem RSA-Schlüssel ist - einem öffentlichen Schlüssel, der vom externen Identity Service verwaltet wird.

Der öffentliche Schlüssel dieses Paares muss an die Adobe Pass-Authentifizierung übergeben werden, um `JWT` Token erkennen zu können, die mit dem oben genannten privaten Schlüssel signiert sind.

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
