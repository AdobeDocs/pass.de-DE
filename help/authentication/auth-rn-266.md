---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.66
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.66
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 2.66 {#authn-266-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-266}

* [Build-Nummer](#build-number-266)
* [Versionsübersicht](#release-overview-266)

### Build-Nummer {#build-number-266}

Adobe Pass-Authentifizierung: adobe-pass-**2,66,0,1**
Releasedatum: **11.7.2023 - 07.13.2023**

### Versionsübersicht {#release-overview-266}

Mit dieser Version haben wir interne Updates für die neue REST-API fortgesetzt.

#### Fehlerbehebungen {#release-overview-bugfixes-266}

* Korrektur des Abmeldeflusses für SAML-basierte MVPDs, bei dem der RelayState-Parameter in der Abmeldeanforderung fehlte. Wir werden nach der Veröffentlichung Konfigurationsaktualisierungen durchführen, um den Abmeldefluss für betroffene MVPDs wiederherzustellen.
* Es wurde die Möglichkeit hinzugefügt, SSL-Zertifikate in unserer Konfiguration für SOAP-Autorisierungsendpunkte zu aktualisieren.
* Es wurde ein Eckenfall behoben, bei dem in einigen ESM-Berichten im Feld Programmierer falsche Daten protokolliert wurden.
