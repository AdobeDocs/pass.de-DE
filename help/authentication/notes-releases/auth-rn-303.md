---
title: Versionshinweise zu Adobe Pass Authentication 3.0.3
description: Versionshinweise zu Adobe Pass Authentication 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Versionshinweise zu Adobe Pass Authentication 3.0.3 {#authn-303-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-303}

* [Build-Nummer](#build-number-303)
* [Versionsübersicht](#release-overview-303)

### Build-Nummer {#build-number-303}

Adobe Pass-Authentifizierung: adobe-pass-**3.0.3**

Veröffentlichungsdatum: **10/29/2024 - 10/31/2024**

### Versionsübersicht {#release-overview-303}

#### REST API v2

##### Code

* Verbesserungen der REST-API V2 (seit Bereitstellung der Hauptversion von Adobe Pass 3.0 [REST-API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Es wurden `notBefore`- und `notAfter` für `/sessions/{code}` hinzugefügt, um Informationen zur Gültigkeit des Authentifizierungs-Codes zurückzugeben.
* Verbesserte Plattformidentifizierung.

##### Dokumentation

* Informationen zu den ersten Schritten mit der neuen REST-API v2 finden Sie im Dokument [REST-API v2 - Übersicht](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

##### Tools

* Informationen zum Testen der neuen REST-API v2 finden Sie auf der neuen Seite zur Adobe Pass-Authentifizierung von der [Adobe Developer](https://developer.adobe.com/adobe-pass)-Website.
