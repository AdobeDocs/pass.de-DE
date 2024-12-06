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
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-263}

* [Build-Nummer](#build-number)
* [Neue Funktionen](#new-features)

### Build-Nummer {#build-number-263}

Adobe Pass-Authentifizierung: adobe-pass-**2.63**
Releasedatum: **20.09.2022 - 22.09.2022**

### Neue Funktionen {#new-features-263}

#### Verbesserung des Mechanismus zur Identifizierung von Plattformen {#pf-identification-mech}

* Ab dieser Version haben wir den Mechanismus zur Identifizierung eines Geräts verbessert und sind nicht mehr von der clientseitigen Implementierung abhängig. Dies bietet eine genauere Granularität bei der Anwendung von Geschäftsregeln auf Plattformebene und ein besseres Verständnis der Traffic-Werte in ESM-Berichten.

* In Kürze wird eine neue ESM-Version mit neuen und verbesserten Berichten folgen, die die plattformbezogenen Felder verfügbar machen.

* Weitere Informationen zu den geplanten Änderungen erhalten Sie von Ihrem TAM.

#### MVPD-Selbstverschlechterung {#mvpd-self-degradation}

Diese Funktion bietet MVPDs die Möglichkeit, ihre eigenen Authentifizierungs- und Autorisierungsendpunkte für Szenarien mit hohem Traffic vorübergehend zu umgehen, wenn die Belastung dieser jeweiligen Endpunkte zu hoch wird.


#### Hinzufügen einer proximierten ID in der Kopfzeile der Autorisierungsaufrufe {#add-proxied-id}

Mit dieser Funktion wird die ID eines von Synacor proximierten MVPD im Header des Autorisierungsaufrufs hinzugefügt. Dadurch kann Synacor Geschäftsregeln für jeden einzelnen Proxy einrichten (z. B. Routing zu verschiedenen Domänen pro proximiertem MVPD).


#### TVE Dashboard {#tve-dashboard}

In dieser Version haben wir ein Problem behoben, bei dem authN oder authZ TTL auf MVPD-Ebene nicht korrekt in den Konfigurationsberichten berechnet wurden.


#### JavaScript SDK 4.6.0 {#js-sdk}

* Die Verwendung der Funktion `eval` wurde entfernt, sodass das SDK mit der Inhaltssicherheitsrichtlinie konform ist.
* Fehlerkorrektur - Der Authentifizierungsfluss kann jetzt erfolgreich abgeschlossen werden, wenn der lokale Speicher des Browsers explizit von einer Partner-Anwendung gelöscht wird.
