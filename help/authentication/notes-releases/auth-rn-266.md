---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.66
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.66
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 2.66 {#authn-266-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-266}

* [Build-Nummer](#build-number-266)
* [Versionsübersicht](#release-overview-266)

### Build-Nummer {#build-number-266}

Adobe Pass-Authentifizierung: adobe-pass-**2.66.0.1**
Veröffentlichungsdatum: **07/11/2023 - 07/13/2023**

### Versionsübersicht {#release-overview-266}

Mit dieser Version setzten wir interne Aktualisierungen für die neue REST-API fort.

#### Fehlerbehebungen {#release-overview-bugfixes-266}

* Fehlerkorrektur - Der Abmeldefluss für SAML-basierte MVPDs, bei denen der RelayState-Parameter in der Abmeldeanfrage fehlt, wird jetzt nicht mehr korrigiert. Nach der Veröffentlichung werden Konfigurationsaktualisierungen geplant, um den Abmeldefluss für betroffene MVPDs wiederherzustellen.
* Es wurde die Möglichkeit hinzugefügt, SSL-Zertifikate in unserer Konfiguration für SOAP-Autorisierungsendpunkte zu aktualisieren.
* Es wurde ein Eckenfall behoben, bei dem in einigen ESM-Berichten falsche Daten im Feld Programmierer protokolliert wurden.
