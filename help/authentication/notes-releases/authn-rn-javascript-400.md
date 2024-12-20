---
title: Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.0.0
description: Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.0.0
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.0.0 {#javascript-sdk-400-release-notes}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Build-Nummer {#build-no-javascript-sdk-400}

Adobe Pass-Authentifizierung: JavaScript 4.0.0

Veröffentlichungsdatum: **07/05/2018**


## Versionsübersicht {#overview-javascript-sdk-400}

* Es wurde Unterstützung für den dynamischen Client-Registrierungsmechanismus implementiert, einen einheitlichen Sicherheitsmechanismus für alle Plattformen. Die Verwaltung des plattformübergreifenden Sicherheitszugriffs kann über das TVE-Dashboard vom Programmierer selbst durchgeführt werden.
* Beseitigt die Verwendung aller Drittanbieter-Cookies und fast aller Erstanbieter-Cookies für alle Funktionen (mit Ausnahme der Individualisierung, wo ein Erstanbieter-Cookie weiterhin verwendet wird - wird in Zukunft entfernt), wodurch es mit neuen Browser-Richtlinien rund um Drittanbieter-Cookies oder Cookies im Allgemeinen (z. B. ITP von Safari). Beachten Sie, dass die vorherige Version 3.x für Happy Flows nicht wie erwartet funktioniert, indem Sie nur Erstanbieter-Cookies verwenden. Dies kann sich jedoch mit der Veröffentlichung neuer Browser-Versionen ändern.
* Unterstützung für das Deaktivieren der Zwischenspeicherung für Vorabflugautorisierungsprüfungen.
* Diese Version ist NICHT abwärtskompatibel und wird unter dem neuen Pfad https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js gehostet. Um von dieser neuen JS-SDK zu profitieren, ist ein Migrationsprozess von Programmierern erforderlich, um ihre Web-Anwendungen mit dem neuen dynamischen Client-Registrierungsmechanismus zu registrieren und auf die neue JS-SDK-URL zu verweisen.


## Versionspaket {#rel-pkg-javascript-sdk-400}

Die Produktions-URL lautet: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

Die Staging-URL lautet: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
