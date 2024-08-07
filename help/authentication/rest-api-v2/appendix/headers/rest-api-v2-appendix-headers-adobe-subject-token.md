---
title: Header - Adobe-Subject-Token
description: REST API V2 - Header - Adobe-Subject-Token
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 1%

---


# Header - Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Der Anforderungsheader <b>Adobe-Subject-Token</b> enthält die eindeutige Plattformkennung als `JWS` oder `JWE`, die von einem Identitätsdienst oder einer Bibliothek abgerufen wird, die außerhalb von Adobe Pass-Authentifizierungssystemen ausgeführt wird.

Diese Kopfzeile ist für die Verwendung in SSO-fähigen Flüssen (Single Sign-on) vorgesehen, die die Platform Identity-Methode nutzen.

Weitere Informationen zu den für Single Sign-on (SSO) aktivierten Flüssen, die die Platform Identity-Methode nutzen, finden Sie in der Dokumentation [Single Sign-on mit Platform-Identitätsflüssen](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) .

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

<b>unique_platform_identifier</b>

Die JSON-Websignatur (`JWS`) oder JSON-Webverschlüsselung (`JWE`), die ein signiertes oder verschlüsseltes JSON-Web-Token (`JWT`) mit eindeutigen Informationen zur Plattformkennung enthält.

Dies ist für die folgenden Plattformen verfügbar:

* [Amazon](../../../amazon-fireos-sso-using-clientless-api-cookbook.md)

## Beispiele {#examples}

Sehen Sie sich die Beispiele für die folgenden Plattformen an:

* [Amazon](../../../amazon-fireos-sso-using-clientless-api-cookbook.md)
