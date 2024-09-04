---
title: Versionshinweise zur Adobe Pass-Authentifizierung 3.0
description: Versionshinweise zur Adobe Pass-Authentifizierung 3.0
source-git-commit: 82fd63a0e208a90753235b81fa52757283c9d314
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 3.0 {#pt-authn-300-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-300}

* [Build-Nummer](#build-number-300)
* [Versionsübersicht](#release-overview-300)

### Build-Nummer {#build-number-2651}

Adobe Pass-Authentifizierung: adobe-pass-**3.0**
Releasedatum: **10.09.2024 - 12.09.2024**

### Neue Funktionen {#new-features-300}

#### REST API v2 {#rest-apis}

##### Code

* Ab der Adobe-Pass-**3.0**-Version können neue und vorhandene Clientanwendungen die neue REST-API v2 integrieren oder auf sie migrieren, um von den neuesten Adobe Pass-Funktionen zu profitieren.

##### Dokumentation

* Informationen zum Einstieg in die neue REST-API v2 finden Sie in den folgenden Dokumenten:
   * [REST API v2 - APIs - Übersicht](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 - Flows - Übersicht](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
* Die URLs für öffentliche Dokumente der REST API v1 wurden geändert. Weitere Informationen finden Sie in den folgenden Dokumenten:
   * [REST API v1 - APIs - Übersicht](./rest-api-overview.md)
   * [REST API v1 - APIs - Referenz](./rest-api-reference.md)

##### Instrumente

* Informationen zum Testen der neuen REST-API v2 finden Sie auf der neuen Adobe Pass-Authentifizierungsseite auf der Website [Adobe Developer](https://developer.adobe.com/adobe-pass) .

### Fehlerkorrekturen {#bug-fixes-300}

* Es wurde ein Problem behoben, durch das der Umleitungs-URL-Parameter nicht verwendet wurde, wenn er in der Abmeldeanforderung vorhanden war.