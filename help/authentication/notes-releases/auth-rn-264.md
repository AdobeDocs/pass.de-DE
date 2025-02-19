---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.64
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 2.64 {#authn-264-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-264}

* [Build-Nummer](#build-number-264)
* [Versionsübersicht](#release-overview-264)

### Build-Nummer {#build-number-264}

Adobe Pass-Authentifizierung: adobe-pass-**2.64**

Veröffentlichungsdatum: **11/08/2022 - 11/10/2022**

### Versionsübersicht {#release-overview-264}

* Aktualisierungen der Infrastruktur, die auf kürzere Server-Reaktionszeiten abzielen und die Gesamtleistung des Systems verbessern.
* Verbesserungen am neuen Mechanismus zur Plattformidentifizierung.
* Die Möglichkeit, unaufgeforderte Authentifizierungsantworten von MVPDs zu blockieren, bei denen der Parameter „in_response_to“ in der SAML-Bestätigung fehlt.

#### Fehlerbehebungen

* Fehlerkorrektur - Einige alte TempPass-Token werden jetzt korrekt formatiert.
* Behebung kleinerer Probleme im Authentifizierungsfluss auf dem zweiten Bildschirm.
