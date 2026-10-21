---
title: Versionshinweise zu Adobe Pass Authentication 3.10.0
description: Versionshinweise zu Adobe Pass Authentication 3.10.0
hold: true
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: b754c97fc3c045e9511d07e22e0cc5387306a67d
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Versionshinweise zu Adobe Pass Authentication 3.10.0 {#authn-3100-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-3100}

* [Build-Nummer](#build-number-3100)
* [Versionsübersicht](#release-overview-3100)

### Build-Nummer {#build-number-3100}

Adobe Pass-Authentifizierung: adobe-pass-**3.10.0**\
Veröffentlichungsdatum: **10/27/2026 - 10/29/2026**

### Versionsübersicht {#release-overview-3100}

Diese Version konzentriert sich auf Single Sign-On-Steuerelemente, Verbesserungen an Benutzermetadaten und Zuverlässigkeitsverbesserungen für die REST-API V2 und die Konfigurationsverwaltung von MVPD.

#### Verbesserungen

* Die Steuerung auf Integrationsebene für das Service-Token Single Sign-On (SSO) wurde hinzugefügt. Service Token SSO wird nur verwendet, wenn es explizit für die Integration aktiviert ist.
* Es wurde Unterstützung für benutzerdefinierte Benutzermetadaten (z. B. `packageType`) während der Authentifizierung, Autorisierung und des Preflight-Vorgangs hinzugefügt. Autorisierungs- und PreFlight-Entscheidungen der REST-API V2 enthalten jetzt konfigurierte Benutzermetadaten, einschließlich zwischengespeicherter Antworten, wobei die Einstellungen für Metadatenzugriff und Verschlüsselung eingehalten werden.

#### Fehlerbehebungen

* Es wurde ein Problem behoben, das dazu führte, dass REST API V2 Apple SSO-Profilanforderungen mit einem SAML-Antwortfehler für Integrationen, die OAuth2-MVPDs über SAML verwendeten, fehlschlugen.
* Es wurde ein Problem behoben, bei dem PreFlight- und Autorisierungsentscheidungen für dieselbe Ressource die zwischengespeicherten Ablaufzeiten und Benutzermetadaten gegenseitig überschreiben konnten.
* Es wurde ein Problem behoben, bei dem das Löschen von Proxy-MVPDs nicht referenzierte Plattformeinstellungen zurückließ, die spätere Konfigurationsspeicherungen verhinderten.
