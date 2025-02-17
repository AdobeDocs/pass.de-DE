---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.64.1
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 2.64.1 {#authn-264-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-2641}

* [Build-Nummer](#build-number-2641)
* [Versionsübersicht](#release-overview-2641)

### Build-Nummer {#build-number-2641}

Adobe Pass-Authentifizierung: adobe-pass-**2.64.1**

Veröffentlichungsdatum: **01/31/2023 - 02/02/2023**

### Versionsübersicht {#release-overview-2641}

Mit dieser Version wird Folgendes hinzugefügt:

* Die Möglichkeit, unaufgeforderte Authentifizierungsantworten von MVPDs zu blockieren, bei denen der Parameter „in_response_to“ in der SAML-Bestätigung fehlt.
* Eine verbesserte Validierung über den Parameter redirect_url zur Erfüllung der Sicherheitsanforderungen.
* Eine verbesserte Protokollierung der Geräteinformationen für Authentifizierungsanfragen für den zweiten Bildschirm unter Verwendung von Informationen, die von dem ursprünglichen Gerät bereitgestellt werden.
