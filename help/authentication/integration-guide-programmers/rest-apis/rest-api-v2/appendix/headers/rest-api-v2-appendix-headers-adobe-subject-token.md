---
title: Header - Adobe-subject-token
description: REST API v2 - Kopfzeile - Adobe-Subject-Token
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 1%

---

# Header - Adobe-subject-token {#header-adobe-subject-token}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#overview}

Der Anforderungsheader <b>Adobe-Subject-Token</b> enthält die eindeutige Plattformkennung `JWS` oder `JWE`, die von einem Identity Service oder einer Bibliothek abgerufen wurde, der/die außerhalb von Adobe Pass-Authentifizierungssystemen ausgeführt wird.

Dieser Header ist für die Verwendung in SSO-fähigen Flüssen (Single Sign-On) vorgesehen, die die Platform-Identitätsmethode verwenden.

Weitere Informationen zu den für Single Sign-on (SSO) aktivierten Flüssen, die die Platform-Identitätsmethode verwenden, finden Sie in der Dokumentation [Single Sign-on mit Platform-Identitätsflüssen](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) .

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

* [Amazon SSO-Cookbook (REST API v2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## Beispiele {#examples}

Sehen Sie sich die Beispiele für die folgenden Plattformen an:

* [Amazon SSO-Cookbook (REST API v2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
