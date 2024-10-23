---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.0.3
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.0.3
source-git-commit: 2f5e511f774e1a2d8b8b60084844edfe27be6c76
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 3.0.3 {#pt-authn-303-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-303}

* [Build-Nummer](#build-number-303)
* [Versionsübersicht](#release-overview-303)

### Build-Nummer {#build-number-2651}

Adobe Pass-Authentifizierung: adobe-pass-**3.0.3**
Releasedatum: **29.10.2024 - 31.10.2024**

### Neue Funktionen {#new-features-303}

#### REST API v2 {#rest-apis}

##### Code

* Verbesserungen der REST-API V2 (seit der Adobe Pass 3.0-Hauptversion [REST API V2](./rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Die Felder `notBefore` und `notAfter` in `/sessions/{code}` wurden hinzugefügt, um Informationen zur Gültigkeit des Authentifizierungscodes zurückzugeben.
* Verbesserte Plattformidentifizierung.

##### Dokumentation

* Informationen zum Starten mit der neuen REST API v2 finden Sie im Dokument [REST API v2 Overview](./rest-api-v2/rest-api-v2-overview.md) .

##### Instrumente

* Informationen zum Testen der neuen REST-API v2 finden Sie auf der neuen Adobe Pass-Authentifizierungsseite auf der Website [Adobe Developer](https://developer.adobe.com/adobe-pass) .
