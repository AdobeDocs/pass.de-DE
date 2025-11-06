---
title: Kopfzeile - x-roku-reserved-roku-connect-token
description: REST API v2 - Kopfzeile - x-roku-reserved-roku-connect-token
exl-id: 21016d5b-4d10-4018-a82c-f2797b2d9fb9
source-git-commit: 560cdcdabb3d507121912938d687a47020e85293
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Kopfzeile - x-roku-reserved-roku-connect-token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Überblick {#overview}

Der <b>X-Roku-Reserved-Roku-Connect-Token</b>-Anfrage-Header enthält die eindeutige Plattformkennung als `JWS` oder `JWE`, die von einem Identitätsdienst oder einer Bibliothek abgerufen wurde, der/die außerhalb von Adobe Pass-Authentifizierungssystemen ausgeführt wird.

Dieser Header ist für die Verwendung in SSO-fähigen Flüssen (Single Sign-On) vorgesehen, die die Platform-Identitätsmethode verwenden.

Weitere Informationen zu den für Single Sign-on (SSO) aktivierten Flüssen, die die Platform-Identitätsmethode verwenden, finden Sie in der Dokumentation [Single Sign-on mit Platform-Identitätsflüssen](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) .

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

<b>unique_platform_identifier</b>

Die JSON-Web-Signatur (`JWS`) oder JSON-Web-Verschlüsselung (`JWE`), die ein signiertes oder verschlüsseltes JSON-Web-Token (`JWT`) ist, das eindeutige Plattformkennungsinformationen enthält.

Dies ist für die folgenden Plattformen verfügbar:

* [Roku SSO Cookbook (REST API v2)](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
