---
title: Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.0.0
description: Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.0.0
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.0.0 {#javascript-sdk-400-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Build-Nummer {#build-number-400}

Adobe Pass-Authentifizierung: JavaScript 4.0.0

Veröffentlichungsdatum: **07/05/2018**

## Versionsübersicht {#release-overview-400}

* Es wurde Unterstützung für den dynamischen Client-Registrierungsmechanismus implementiert, einen einheitlichen Sicherheitsmechanismus für alle Plattformen. Die Verwaltung des plattformübergreifenden Sicherheitszugriffs kann über das TVE-Dashboard vom Programmierer selbst durchgeführt werden.
* Beseitigt die Verwendung aller Drittanbieter-Cookies und fast aller Erstanbieter-Cookies für alle Funktionen (mit Ausnahme der Individualisierung, wo ein Erstanbieter-Cookie weiterhin verwendet wird - wird in Zukunft entfernt), wodurch es mit neuen Browser-Richtlinien rund um Drittanbieter-Cookies oder Cookies im Allgemeinen (z. B. ITP von Safari). Beachten Sie, dass die vorherige Version 3.x für Happy Flows nicht wie erwartet funktioniert, indem Sie nur Erstanbieter-Cookies verwenden. Dies kann sich jedoch mit der Veröffentlichung neuer Browser-Versionen ändern.
* Unterstützung für das Deaktivieren der Zwischenspeicherung für Vorabflugautorisierungsprüfungen.
* Diese Version ist NICHT abwärtskompatibel und wird unter dem neuen Pfad https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js gehostet. Um von dieser neuen JS-SDK zu profitieren, ist ein Migrationsprozess von Programmierern erforderlich, um ihre Web-Anwendungen mit dem neuen dynamischen Client-Registrierungsmechanismus zu registrieren und auf die neue JS-SDK-URL zu verweisen.

## Versionspaket {#release-package-400}

Die Produktions-URL lautet: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

Die Staging-URL lautet: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
