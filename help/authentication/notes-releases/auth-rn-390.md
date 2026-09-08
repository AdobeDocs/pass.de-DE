---
title: Versionshinweise zu Adobe Pass Authentication 3.9.0
description: Versionshinweise zu Adobe Pass Authentication 3.9.0
source-git-commit: 7ec140485418d07e16a181d43b651ea6de331477
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Versionshinweise zu Adobe Pass Authentication 3.9.0 {#authn-390-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-390}

* [Build-Nummer](#build-number-390)
* [Versionsübersicht](#release-overview-390)

### Build-Nummer {#build-number-390}

Adobe Pass-Authentifizierung: adobe-pass-**3.9.0.1**\
Veröffentlichungsdatum: **09/08/2026 - 09/10/2026**

### Versionsübersicht {#release-overview-390}

In dieser Version liegt der Schwerpunkt auf Verbesserungen der REST-API V2- und ESM-Metriken.

#### Verbesserungen

* Das Single Sign-On der REST-API V2-Partner wurde verbessert, um sicherzustellen, dass eine gültige Authentifizierungsanfrage für MVPDs zurückgegeben wird, die mit OAuth2 konfiguriert sind.
* Verbesserte REST-API-V2-Entscheidungen, um bei fehlgeschlagener Autorisierung anstelle einer leeren Antwort eine klare Fehlerantwort zurückzugeben.
* Verbesserte Registrierungs-Code-Generierung, um visuell mehrdeutige Zeichen zu verhindern, sodass Codes leichter zu lesen und korrekt einzugeben sind.
* ESM-Dashboard-Verbesserungen mit Unterstützung für Preflight-AuthZ-Metriken.
