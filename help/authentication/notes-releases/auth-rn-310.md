---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.1.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.1.0
source-git-commit: 4ad5ea619f64a78a72f69228c9ae3c83a7b66f24
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 3.1.0 {#authn-310-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-310}

* [Build-Nummer](#build-number-310)
* [Versionsübersicht](#release-overview-310)

### Build-Nummer {#build-number-310}

Adobe Pass-Authentifizierung: adobe-pass-**3.1.0**

Veröffentlichungsdatum: **02/25/2025 - 02/27/2025**

### Versionsübersicht {#release-overview-310}

#### REST API v2

* Neuer `partner_logout` Aktionsname und `partner_interactive` Aktionstyp, um in der REST-API v2 (Logout-API)-Antwort zwischen regulärem Abmelden und Single Sign[On-Abmelden &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) Partnern zu unterscheiden.
* Neues `reason`, das mehr Einblicke in den Aktionsnamen in den Antworten der REST-API v2 [Sessions-](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) und [Sessions SSO-](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) bietet.

#### Fehlerbehebungen

* Es wurde ein Problem behoben, das Spektrumabonnentinnen und -abonnenten daran hinderte, sich über die REST-API v2 [Authentifizierungs-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) zu authentifizieren.
* Es wurde ein Problem behoben, das verhinderte, dass Ereignisse, die über die REST API V2 [Authentifizierungs-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) generiert wurden, ordnungsgemäß in ESM aggregiert wurden.
* Es wurde ein Problem behoben, das zu einer falschen Berechnung `notBefore` Zeitstempels für die Benutzerprofile in der REST-API v2-Antwort [Profile-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) führte.

#### JavaScript SDK

* Version 3.5.0 von [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md) entfernt.