---
title: Übersicht über REST API V2-APIs
description: Übersicht über REST API V2-APIs
exl-id: 62b48bf0-d200-4949-b268-8f8ea2daabfa
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 297578d3b311fbc2a5702f1bcc947e94463fb884
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%
---
# Übersicht über REST API V2-APIs {#rest-api-v2-apis-overview}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Zu Beginn konsultieren Sie die öffentliche Dokumentation der APIs, die für Sie in der folgenden Liste interessant sind.

## Basis-URLs {#rest-api-v2-apis-base-urls}

Verwenden Sie die unten stehende umgebungsspezifische Basis-URL für REST-API-V2-Anfragen. Hängen Sie die für jede API dokumentierten relativen `path` an diese Basis-URL an.

Einige API-Antworten geben interaktive URLs zurück, die das Streaming-Gerät in einem Browser oder Benutzeragenten öffnen muss. Beispiele sind das `url` , das von [Abmeldung für bestimmte mvpd initiieren](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) und die von [Authentifizierung im Benutzeragenten durchführen](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) zurückgegebene `Location`-Umleitung.

Diese interaktiven URLs verwenden denselben Host des Adobe Pass Service Providers (SP). Öffnen Sie eine absolute interaktive URL genau wie zurückgegeben. Wenn Adobe Pass eine relative interaktive URL zurückgibt, die mit `/adobe-services/` beginnt, lösen Sie sie für die Zielumgebung gegen den SP-Host auf:

| Umgebung | REST API V2 und interaktive Basis-URL |
| --- |--------------------------------------|
| Produktion | `https://sp.auth.adobe.com` |
| Staging | `https://sp.auth-staging.adobe.com` |

## Konfiguration {#rest-api-v2-apis-configuration}

* [Abrufen der Konfiguration für einen bestimmten Dienstleister](configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

## Sitzungen {#rest-api-v2-apis-sessions}

* [Authentifizierungssitzung erstellen](sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Authentifizierungssitzung fortsetzen](sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Authentifizierungssitzung abrufen](sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Authentifizierung im Benutzeragenten durchführen](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)

## Profile {#rest-api-v2-apis-profiles}

* [Profile abrufen](profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Abrufen eines Profils für ein bestimmtes MVPD](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Abrufen eines Profils für einen bestimmten Code](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

## Entscheidungen {#rest-api-v2-apis-decisions}

* [Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD](decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Abrufen von Entscheidungen vor der Autorisierung mithilfe bestimmter MVPD](decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## Abmelden {#rest-api-v2-apis-logout}

* [Initiieren des Abmeldens für bestimmte MVPDs](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

## Partner-Single-Sign-on {#rest-api-v2-apis-partner-single-sign-on}

* [Partnerauthentifizierungsanfrage abrufen](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Erstellen und Abrufen eines Profils mithilfe der Antwort zur Partnerauthentifizierung](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

## Möchten Sie die REST API V2 ausprobieren?

Sie können jetzt die REST-API V2 über unsere produktspezifische Seite auf der [Adobe Developer](https://developer.adobe.com/adobe-pass/)-Website erkunden.
