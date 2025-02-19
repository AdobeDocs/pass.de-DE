---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.65.1
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.65.1
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 2.65.1 {#authn-2651-rn}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-2651}

* [Build-Nummer](#build-number-2651)
* [Versionsübersicht](#release-overview-2651)

### Build-Nummer {#build-number-2651}

Adobe Pass-Authentifizierung: adobe-pass-**2.65.1**

Veröffentlichungsdatum: **06/20/2023 - 06/22/2023**

### Versionsübersicht {#release-overview-2651}

Mit dieser Version wird Folgendes hinzugefügt:

Ab dieser Version führen wir technischen Support für das Hinzufügen von Ratenbegrenzungsregeln für alle unsere APIs ein, um das gesamte Ökosystem vor falscher Verwendung zu schützen.

Das erste Ziel wird darin bestehen, das Anwendungsverhalten zu überwachen, um die richtigen Grenzwerte festzulegen. Im Rahmen dieser Version werden keine Grenzwerte angewendet. In Fällen, in denen der von einem bestimmten Programm generierte Traffic bestimmte Grenzwerte überschreitet, validieren wir die Implementierung gemeinsam mit jedem Kunden.

Die Regeln zur Ratenbegrenzung werden im Laufe dieses Jahres angewendet, nachdem wir den Traffic validiert haben, der von allen Anwendungen des Kunden generiert wurde.
