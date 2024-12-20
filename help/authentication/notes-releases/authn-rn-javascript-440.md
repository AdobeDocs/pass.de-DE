---
title: Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.4.0
description: Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.4.0
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript 4.4.0 {#javascript-sdk-440-release-notes}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Build-Nummer {#build-no-javascript-sdk-440}

Adobe Pass-Authentifizierung: JavaScript 4.4.0

Veröffentlichungsdatum: **06/22/2021**


## Versionsübersicht {#overview-javascript-sdk-440}

### Neue Funktionen {#new-features-javascript-sdk-440}

Vorfluggenehmigung

* Neue vorab autorisierte API : Dies ist der neue API-Aufruf für unsere PreFlight-Autorisierungsfunktion, der auch eine erweiterte Fehlerberichterstattung für den PreFlight-Fluss einführt.
* Diese Funktion ist auf Anfrage verfügbar, da sie in der Konfiguration der Primetime-Authentifizierung aktiviert ist. Wenden Sie sich an Ihren TAM, um weitere Informationen zur Aktivierung dieser Funktion zu erhalten.
* Verwirft die API checkPreauthorizedResources.

Plattformidentifizierung

* Fügt bei allen SDK-Aufrufen die Kopfzeile AP-SDK-Identifier hinzu, um SDK-Typ und -Version besser zu identifizieren.

Sonstige

* Interne architektonische Verbesserungen.


### Fehlerbehebungen {#bug-fixes-javascript-sdk-440}

* Behebung einer Racebedingung, die erzeugt wird, wenn setRequestor und getAuthentication gleichzeitig aufgerufen werden.
* Es wurde ein Problem behoben, das verhinderte, dass Berechtigungen korrekt in Staging-Umgebungen geladen wurden.
* Es wurde ein Problem behoben, das verhinderte, dass der Abmeldefluss im Hintergrund in Safari-Browsern abgeschlossen wurde. Daher schien der Benutzer weiterhin authentifiziert zu sein, bis eine Seitenaktualisierung stattfand. Es wurde eine maximale Wartezeit eingeführt, die derzeit auf 30 Sekunden festgelegt ist. Wenn während dieses Zeitraums keine Antwort vom Primetime-Authentifizierungsserver erfolgt, ruft die SDK den Rückruf setAuthenticationStatus auf.

## Versionspaket {#rel-pkg-javascript-sdk-440}

Die Produktions-URL lautet: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

Die Staging-URL lautet: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
