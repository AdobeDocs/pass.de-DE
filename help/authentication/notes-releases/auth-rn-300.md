---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.0
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 3.0 {#authn-300-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-300}

* [Build-Nummer](#build-number-300)
* [Versionsübersicht](#release-overview-300)

### Build-Nummer {#build-number-300}

Adobe Pass-Authentifizierung: adobe-pass-**3.0**

Veröffentlichungsdatum: **09/10/2024 - 09/12/2024**

### Versionsübersicht {#release-overview-300}

#### REST API v2

##### Code

* Ab Adobe-Pass-**3.0** können neue und bestehende Client-Anwendungen in die neue REST API v2 integriert oder zu dieser migriert werden, um von den neuesten Adobe Pass-Funktionen zu profitieren.

##### Dokumentation

* Informationen zum Einstieg in die neue REST-API v2 finden Sie in den folgenden Dokumenten:
   * [REST API v2 - APIs - Übersicht](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 - Flüsse - Übersicht](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* Die URLs für öffentliche Dokumente der REST-API v1 haben sich geändert. Siehe die folgenden Dokumente:
   * [REST API v1 - APIs - Übersicht](../integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
   * [REST API v1 - APIs - Referenz](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### Tools

* Informationen zum Testen der neuen REST-API v2 finden Sie auf der neuen Seite zur Adobe Pass-Authentifizierung von der [Adobe Developer](https://developer.adobe.com/adobe-pass)-Website.

#### Fehlerbehebungen

* Es wurde ein Problem behoben, bei dem der Umleitungs-URL-Parameter nicht verwendet wurde, wenn er in der Abmeldeanfrage vorhanden war.
