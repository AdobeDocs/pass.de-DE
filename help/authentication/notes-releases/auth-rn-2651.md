---
title: Versionshinweise zur Adobe Pass-Authentifizierung 2.65.1
description: Versionshinweise zur Adobe Pass-Authentifizierung 2.65.1
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# Versionshinweise zur Adobe Pass-Authentifizierung 2.65.1 {#authn-2651-rn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:

## Server-seitige und Web-Clients {#server-side-web-clients-2651}

* [Build-Nummer](#build-number-2651)
* [Versionsübersicht](#release-overview-2651)

### Build-Nummer {#build-number-2651}

Adobe Pass-Authentifizierung: adobe-pass-**2.65.1**
Releasedatum: **20.06.2023 - 22.06.2023**

### Versionsübersicht {#release-overview-2651}

Diese Version fügt Folgendes hinzu:

Ab dieser Version bieten wir technischen Support für das Hinzufügen von Regeln zur Ratenbegrenzung für alle unsere APIs, um das gesamte Ökosystem vor falscher Nutzung zu schützen.

Das ursprüngliche Ziel besteht darin, das Verhalten der Anwendungen zu überwachen, um die richtigen Grenzwerte festzulegen, und im Rahmen dieser Version werden keine Beschränkungen angewendet. Für Fälle, in denen der durch eine bestimmte Anwendung generierte Traffic bestimmte Beschränkungen überschreitet, arbeiten wir mit jedem Kunden zusammen, um die Implementierung zu validieren.

Die Regeln zur Begrenzung der Rate werden Ende dieses Jahres angewendet, nachdem wir den aus allen Kundenanwendungen generierten Traffic validiert haben.
