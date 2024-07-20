---
title: Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.4.0
description: Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.4.0
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.4.0 {#javascript-sdk-440-release-notes}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:

## Build-Nummer {#build-no-javascript-sdk-440}

Adobe Pass-Authentifizierung: JavaScript 4.4.0

Releasedatum: **22.06.2021**


## Versionsübersicht {#overview-javascript-sdk-440}

### Neue Funktionen {#new-features-javascript-sdk-440}

Vorabgenehmigung

* Neue Vorabautorisierungs-API - dies ist der neue API-Aufruf, der für unsere Preflight-Autorisierungsfunktion verwendet wird. Dadurch wird auch eine verbesserte Fehlerberichterstellung für den Preflight-Fluss eingeführt.
* Diese Funktion ist auf Anfrage verfügbar, da sie bei der Konfiguration der Primetime-Authentifizierung aktiviert werden muss. Weitere Informationen zur Aktivierung dieser Funktion erhalten Sie von Ihrem TAM.
* Veraltet die API checkPreauthorizedResources .

Plattformkennung

* Fügt die Kopfzeile AP-SDK-Identifier zu allen SDK-Aufrufen hinzu, um SDK-Typ und -Version besser zu identifizieren.

sonstige

* Interne Verbesserungen der Architektur.


### Fehlerkorrekturen {#bug-fixes-javascript-sdk-440}

* Korrektur der Race-Bedingung, die beim gleichzeitigen Aufruf von setRequestor und getAuthentication erzeugt wird.
* Es wurde ein Problem behoben, das das korrekte Laden von Berechtigungen in Staging-Umgebungen verhinderte.
* Es wurde ein Problem behoben, durch das der Hintergrund-Abmeldefluss in Safari-Browsern nicht abgeschlossen werden konnte, sodass der Benutzer bis zu einer Aktualisierung der Seite authentifiziert schien. Es wurde eine Zeitüberschreitung eingeführt, die derzeit auf 30 Sekunden eingestellt ist. Wenn also während dieser Zeit keine Antwort vom Primetime-Authentifizierungsserver eingeht, ruft das SDK den Rückruf setAuthenticationStatus auf.

## Versionspaket {#rel-pkg-javascript-sdk-440}

Die Produktions-URL lautet: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

Die Staging-URL lautet: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
