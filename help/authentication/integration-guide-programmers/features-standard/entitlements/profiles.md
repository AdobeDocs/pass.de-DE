---
title: Profile
description: Profile
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Profile {#profiles}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Profile werden von der Adobe Pass-Authentifizierung [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) erstellt, wenn sich ein Benutzer erfolgreich bei seinem Pay-TV-Anbieter (MVPD) authentifiziert.

Der Profiltyp variiert je nach der verwendeten Authentifizierungsmethode:

* **normal**

  Erstellt durch einfache Authentifizierung.

* **Apple SSO**

  Über Single Sign-on (SSO) mit dem Framework des Videoabonnentenkontos von Apple erstellt.

* **Platform SSO**

  Über Single Sign-on (SSO) unter Verwendung einer Platform-Identität erstellt.

* **Service-Token SSO**

  Über Single Sign-on (SSO) mit einem Service-Token erstellt.

Profile speichern Schlüsseldaten, die Clientanwendungen Folgendes ermöglichen:

* Bestimmen Sie den Authentifizierungsstatus des Benutzers.
* Identifizieren Sie die verwendete Authentifizierungsmethode.
* Identifizieren Sie den Identitätsanbieter.
* Zugriff [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

Profile werden sicher im Backend der Adobe Pass-Authentifizierung gespeichert und mit der ID der anfragenden Anwendung, des Geräts und des Dienstleisters verknüpft. Sie bleiben für einen begrenzten Zeitraum gültig, wie in der [Authentication Time-to-Live (TTL) definiert](#authentication-ttl-management).

## TTL-Verwaltung (Time-to-Live) für die Authentifizierung {#authentication-ttl-management}

Die TTL (Authentication Time-to-Live) definiert, wie lange ein Benutzer authentifiziert bleibt, bevor er sich erneut authentifizieren muss. Dieser Zeitraum ist begrenzt und muss mit MVPD-Vertretern vereinbart werden. TTL-Werte können je nach folgenden Kriterien variieren:

* Plattformkategorie (z. B. Desktop-Computer, Mobilgeräte, an das Fernsehgerät angeschlossene Geräte)
* Spezifische Plattform (z. B. iOS, Android, tvOS, Roku, FireTV)

Die TTL für die Authentifizierung (AuthN) kann über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder von einem Adobe Pass-Authentifizierungsbeauftragten, der in Ihrem Namen handelt, angezeigt und geändert werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

## REST-API V2 {#rest-api-v2}

Die Profile können mit den folgenden APIs abgerufen werden:

* [Profile abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Abrufen eines Profils für ein bestimmtes MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Abrufen eines Profils für einen bestimmten Code](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Weitere Informationen zur Struktur **Profile finden Sie in** Abschnitten **Antwort** und Beispiele der oben genannten APIs.

Weitere Informationen zur Integration der oben genannten APIs finden Sie in den folgenden Dokumenten:

* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [Häufig gestellte Fragen zur Authentifizierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
