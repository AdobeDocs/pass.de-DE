---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.8.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.8.0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%
---
# Versionshinweise zur Adobe Pass-Authentifizierung 3.8.0 {#authn-380-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-380}

* [Build-Nummer](#build-number-380)
* [Versionsübersicht](#release-overview-380)

### Build-Nummer {#build-number-380}

Adobe Pass-Authentifizierung: adobe-pass-**3.8.0**\
Veröffentlichungsdatum: **08/11/2026 - 08/13/2026**

### Versionsübersicht {#release-overview-380}

Diese Version konzentriert sich auf Stabilitätserweiterungen und Sicherheitsaktualisierungen für die Adobe Pass-Authentifizierungsdienste.

#### Fehlerbehebungen

* Es wurde ein Problem behoben, das aufgrund bestimmter ungültiger Zeichen in der deviceId zu HTTP 500-Fehlern in V2-APIs führte.

#### Verbesserungen

* Verbesserte Verarbeitung von Aktualisierungs-Token zur Unterstützung der rollierenden Token-Erneuerung.
* Verbesserte Besucher-ID-Erkennung auf sekundären Geräten für Analysen.
* Verbesserte URL-Parametervalidierung zur Stärkung der Sicherheitskontrollen und Verbesserung der allgemeinen Systemintegrität.
* TVE Dashboard Version 1.5.2 mit kleineren Verbesserungen der Benutzeroberfläche.
