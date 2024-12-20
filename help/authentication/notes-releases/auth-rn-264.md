---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.64
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 2.64 {#authn-264-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#ss-web-clients}

* [Build-Nummer](#build-no-264)
* [Neue Funktionen](#new-featres-264)
* [Fehlerbehebungen](#bug-fixes-264)

### Build-Nummer {#build-no-264}

Adobe Pass-Authentifizierung: adobe-pass-**2.64**

Veröffentlichungsdatum: **11/08/2022 - 11/10/2022**

### Neue Funktionen {#new-featres-264}

* Aktualisierungen der Infrastruktur, die auf kürzere Server-Reaktionszeiten abzielen und die Gesamtleistung des Systems verbessern.
* Verbesserungen am neuen Mechanismus zur Plattformidentifizierung.
* Die Möglichkeit, unaufgeforderte Authentifizierungsantworten von MVPDs zu blockieren, bei denen der Parameter „in_response_to“ in der SAML-Bestätigung fehlt.

### Fehlerbehebungen {#bug-fixes-264}

* Fehlerkorrektur - Einige alte TempPass-Token werden jetzt korrekt formatiert.
* Behebung kleinerer Probleme im Authentifizierungsfluss auf dem zweiten Bildschirm.
