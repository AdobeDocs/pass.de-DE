---
title: Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.0.0
description: Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.0.0
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.0.0 {#javascript-sdk-400-release-notes}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:

## Build-Nummer {#build-no-javascript-sdk-400}

Adobe Pass-Authentifizierung: JavaScript 4.0.0

Releasedatum: **07/05/2018**


## Versionsübersicht {#overview-javascript-sdk-400}

* Implementierung der Unterstützung für einen dynamischen Client-Registrierungsmechanismus, einen einheitlichen Sicherheitsmechanismus für alle Plattformen. Der plattformübergreifende Sicherheitszugriff kann über das TVE-Dashboard vom Programmierer selbst verwaltet werden.
* Beseitigt die Verwendung aller Drittanbieter-Cookies und fast aller Erstanbieter-Cookies für alle Funktionen (mit Ausnahme der Individualisierung, bei der weiterhin ein Erstanbieter-Cookie verwendet wird - entfernt wird). Dadurch wird die Verwendung mit neuen Browserrichtlinien rund um Drittanbieter-Cookies oder Cookies im Allgemeinen (z. B. ITP von Safari). Beachten Sie, dass die vorherige Version 3.x für die glücklichen Abläufe erwartungsgemäß funktioniert, da nur Erstanbieter-Cookies verwendet werden. Dies kann sich jedoch mit der Veröffentlichung neuer Browserversionen ändern.
* Unterstützung für das Deaktivieren der Zwischenspeicherung für Preflight-Autorisierungsüberprüfungen.
* Diese Version ist NICHT abwärtskompatibel und wird unter einem neuen Pfad gehostet: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . Um von diesem neuen JS-SDK profitieren zu können, ist ein Migrationsprozess von Programmierern erforderlich, um ihre Webanwendungen mithilfe des neuen dynamischen Client-Registrierungsmechanismus zu registrieren und auf die neue JS-SDK-URL zu verweisen.


## Versionspaket {#rel-pkg-javascript-sdk-400}

Die Produktions-URL lautet: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

Die Staging-URL lautet: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
