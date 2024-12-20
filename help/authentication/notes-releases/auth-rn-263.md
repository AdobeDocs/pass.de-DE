---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.63
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 2.63 {#pt-authn-263-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-263}

* [Build-Nummer](#build-number)
* [Neue Funktionen](#new-features)

### Build-Nummer {#build-number-263}

Adobe Pass-Authentifizierung: adobe-pass-**2.63**
Veröffentlichungsdatum: **09/20/2022 - 09/22/2022**

### Neue Funktionen {#new-features-263}

#### Verbesserung des Identifizierungsmechanismus der Plattform {#pf-identification-mech}

* Mit dieser Version haben wir den Mechanismus verbessert, der zur Identifizierung eines Geräts verwendet wird. Er ist nicht mehr von der Client-seitigen Implementierung abhängig. Dies wird eine genauere Granularität bei der Anwendung von Geschäftsregeln auf Plattformebene und ein besseres Verständnis der Traffic-Werte in ESM-Berichten ermöglichen.

* In Kürze wird eine neue ESM-Version mit neuen und verbesserten Berichten folgen, die die plattformbezogenen Felder aufzeigen.

* Weitere Informationen zu den geplanten Änderungen erhalten Sie von Ihrem Team.

#### MVPD-Selbstdegradierung {#mvpd-self-degradation}

Diese Funktion bietet MVPDs die Möglichkeit, ihre eigenen Authentifizierungs- und Autorisierungsendpunkte für Szenarien mit hohem Traffic vorübergehend zu umgehen, wenn die Last für diese jeweiligen Endpunkte zu hoch wird.


#### Proxy-ID in die Kopfzeile der Autorisierungsaufrufe einfügen {#add-proxied-id}

Diese Funktion fügt die ID eines Synacor-Proxy-MVPD in die Kopfzeile des Autorisierungsaufrufs ein. Dadurch kann Synacor Geschäftsregeln für jeden einzelnen Proxy einrichten (z. B. Routing zu verschiedenen Domains (pro Proxy-MVPD).


#### TVE-Dashboard {#tve-dashboard}

In dieser Version wurde ein Problem behoben, bei dem auf MVPD-Ebene festgelegte authN- oder authZ-TTL in den Konfigurationsberichten nicht korrekt berechnet wurden.


#### JavaScript SDK 4.6.0 {#js-sdk}

* Die Verwendung `eval` Funktion wurde entfernt, sodass die SDK mit der Content Security Policy konform ist.
* Es wurde ein Problem behoben, das verhinderte, dass der Authentifizierungsfluss erfolgreich abgeschlossen wurde, wenn der lokale Speicher des Browsers explizit von einer Partneranwendung gelöscht wurde.
