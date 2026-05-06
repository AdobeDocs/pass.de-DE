---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.7.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.7.0
hold: true
source-git-commit: 170d49b06e4ac8b31a840ee1bc5fac114bb3aa0b
workflow-type: tm+mt
source-wordcount: '142'
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

* Es wurde Unterstützung für Bell MVPD mit OAuth2 mit PKCE hinzugefügt.

#### Verbesserungen

* Freigabe von TVE Dashboard Version 1.5.1.

#### Fehlerbehebungen

* Es wurde ein Problem behoben, das dazu führte, dass Apple SSO bestimmte MVPD-Konfigurationskonflikte übersah.
* Es wurde ein Problem behoben, das zu HTTP 500-Fehlern bei der Autorisierungsverweigerung aufgrund von ungültigen Zeichen in der Antwort-Kopfzeile führte.
