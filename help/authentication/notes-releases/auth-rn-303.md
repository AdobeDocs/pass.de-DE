---
title: Versionshinweise zu Adobe Pass Authentication 3.0.3
description: Versionshinweise zu Adobe Pass Authentication 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Versionshinweise zu Adobe Pass Authentication 3.0.3 {#pt-authn-303-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-303}

* [Build-Nummer](#build-number-303)
* [Versionsübersicht](#release-overview-303)

### Build-Nummer {#build-number-2651}

Adobe Pass-Authentifizierung: adobe-pass-**3.0.3**
Veröffentlichungsdatum: **10/29/2024 - 10/31/2024**

### Neue Funktionen {#new-features-303}

#### REST API v2 {#rest-apis}

##### Code

* Verbesserungen der REST-API V2 (seit Bereitstellung der Hauptversion von Adobe Pass 3.0 [REST-API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Es wurden `notBefore`- und `notAfter` für `/sessions/{code}` hinzugefügt, um Informationen zur Gültigkeit des Authentifizierungs-Codes zurückzugeben.
* Verbesserte Plattformidentifizierung.

##### Dokumentation

* Informationen zu den ersten Schritten mit der neuen REST-API v2 finden Sie im Dokument [REST-API v2 - Übersicht](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

##### Tools

* Informationen zum Testen der neuen REST-API v2 finden Sie auf der neuen Seite zur Adobe Pass-Authentifizierung von der [Adobe Developer](https://developer.adobe.com/adobe-pass)-Website.
