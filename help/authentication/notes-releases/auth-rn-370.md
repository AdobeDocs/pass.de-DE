---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.7.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.7.0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%
---
# Versionshinweise zur Adobe Pass-Authentifizierung 3.7.0 {#authn-370-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-370}

* [Build-Nummer](#build-number-370)
* [Versionsübersicht](#release-overview-370)

### Build-Nummer {#build-number-370}

Adobe Pass-Authentifizierung: adobe-pass-**3.7.0.2**\
Veröffentlichungsdatum: **05/12/2026 - 05/14/2026**

### Versionsübersicht {#release-overview-370}

Diese Version konzentriert sich auf MVPD-Integrations-Upgrades, Fehlerbehebungen und Verbesserungen am TVE-Dashboard.

#### MVPD-Integrationen

* PKCE-Unterstützung für OAuth2-basierte MVPD-Authentifizierung wurde hinzugefügt.

#### Verbesserungen

* Freigabe von TVE Dashboard Version 1.5.1.

#### Fehlerbehebungen

* Es wurde ein Problem behoben, das dazu führte, dass Apple SSO bestimmte MVPD-Konfigurationskonflikte übersah.
* Es wurde ein Problem behoben, das zu HTTP 500-Fehlern bei der Autorisierungsverweigerung aufgrund von ungültigen Zeichen in der Antwort-Kopfzeile führte.
