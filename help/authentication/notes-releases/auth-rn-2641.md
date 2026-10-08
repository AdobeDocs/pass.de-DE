---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.64.1
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 0%
---
# Versionshinweise zur Adobe Pass-Authentifizierung 2.64.1 {#authn-264-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

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
