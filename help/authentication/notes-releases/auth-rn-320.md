---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.2.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.2.0
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
source-git-commit: fcdf50b2caad20deef15fceeb3e23f4195c0078d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 3.2.0 {#authn-320-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-320}

* [Build-Nummer](#build-number-320)
* [Versionsübersicht](#release-overview-320)

### Build-Nummer {#build-number-320}

Adobe Pass-Authentifizierung: adobe-pass-**3.2.0**

Veröffentlichungsdatum: **06/10/2025 - 06/12/2025**

### Versionsübersicht {#release-overview-320}

#### REST API v2

* Es wurde ein neuer `missing_parameters_fallback` für Fälle hinzugefügt, in denen Parameter in der Antwort [Sitzungs-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) fehlen.
* Ein neues Feld „Gerät“ wurde zur Antwort [Sitzungs-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) hinzugefügt.

#### Neue Funktionen

* Erweitern Sie die Degradation-API und fügen Sie die Funktion hinzu, Degradation-Regeln für mehrere MVPDs in einem einzigen Aufruf anzuwenden

#### Fehlerbehebungen

* Fehlerkorrektur - Der Authentifizierungsfluss kann jetzt erfolgreich abgeschlossen werden, wenn die Verarbeitung der Benutzermetadaten fehlgeschlagen ist.
* Es wurde ein Problem behoben, bei dem der Autorisierungsgrund nicht korrekt berechnet wurde.
* Es wurde ein Problem behoben, bei dem in der Profilantwort keine upstreamUserID vorhanden war.
* Es wurde ein Problem behoben, das dazu führte, dass die Authentifizierungsanfrage keine Bereichsinformationen für REST-API V2-Initiierungs-Authentifizierungsanfragen enthielt.
