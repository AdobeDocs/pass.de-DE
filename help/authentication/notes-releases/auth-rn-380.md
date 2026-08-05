---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.8.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.8.0
hold: true
source-git-commit: ce9e8de3d69699d03cf68c86be1bb811967501dc
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
