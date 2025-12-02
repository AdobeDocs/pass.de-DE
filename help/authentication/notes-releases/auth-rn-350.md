---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.5.0
description: Erfahren Sie mehr über die neuen Funktionen, Änderungen und bekannten Probleme in dieser Version.
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---


# Versionshinweise zur Adobe Pass-Authentifizierung 3.5.0

Letzte Aktualisierung: Di 09 Dez 2025 00:00:00 GMT+0000 (Koordinierte Weltzeit)

* Themen:
* Authentifizierung

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-350}

* [Build-Nummer](#build-number-350)
* [Versionsübersicht](#release-overview-350)

### Build-Nummer {#build-number-350}

Adobe Pass-Authentifizierung: adobe-pass-**3.5.0**\
Veröffentlichungsdatum: **12/09/2025 - 12/11/2025**

### Versionsübersicht {#release-overview-350}

#### REST API v2

* Es wurde eine neue Dimension in ESM („API„) hinzugefügt, um Kunden bei der Erstellung von Berichten zu unterstützen, die Ereignisse anhand des Implementierungstyps unterscheiden: SDK, REST V1 oder REST V2.

#### Fehlerbehebungen

* Es wurde ein Problem behoben, bei dem benutzerdefinierte Meldungen zur Verschlechterung, die im TVE-Dashboard festgelegt wurden, nicht in den Fehlerdetails der REST-API V2 angezeigt wurden.
* Es wurde ein Problem in der REST-API V2 behoben, bei dem `authenticated_profile_expired` Fehlercode nicht zurückgegeben wurde, wenn authentifizierte Profile abgelaufen waren.
* Es wurde ein Problem behoben, bei dem die Berechnungen der Autorisierungslatenz und die TTL-Werte vor dem Flug in der REST-API V2 falsch waren.
* Es wurde ein Problem behoben, bei dem ein inkonsistentes Antwortformat zurückgegeben wurde, wenn das DCR-Token abläuft.
