---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.4.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.4.0
exl-id: 7a9b8c6d-4e5f-4a3b-8c7d-9e0f1a2b3c4d
source-git-commit: 58da8137988f0146716b56ac7a960c683b204d53
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 3.4.0 {#authn-340-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-340}

* [Build-Nummer](#build-number-340)
* [Versionsübersicht](#release-overview-340)

### Build-Nummer {#build-number-340}

Adobe Pass-Authentifizierung: adobe-pass-**3.4.0**
Veröffentlichungsdatum: **09/16/2025 - 09/18/2025**

### Versionsübersicht {#release-overview-340}

#### REST API v2

* Unterstützung für [Experience Cloud ID (ECID) wurde hinzugefügt](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md) um die Benutzeridentifizierungs- und Tracking-Funktionen zu verbessern.
* Die Kopfzeilen „AP-Device-Identifier“ und „X-Device-Info“ wurden in „optional“ für die REST-API V2 ([-API) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### Fehlerbehebungen

* Es wurde ein Problem behoben, bei dem die MVPD-ID und die Proxy-MVPD-ID im Token der REST-API-V2-Antwort [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) umgekehrt wurden.
* Es wurde ein Problem behoben, bei dem die Anforderer-ID nicht im Token der REST-API-Antwort [Entscheidungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) vorhanden war.
* Es wurde ein Problem behoben, bei dem die Übergabe zu vieler Ressourcen an die REST-API V2 [Vorabautorisierungsentscheidungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)-API zu einer leeren Entscheidungsliste in der Antwort führte, anstatt zu [erweitertem Fehlercode](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **too_many_resources**.

#### Sonstiges

* Behobene Sicherheitslücken.