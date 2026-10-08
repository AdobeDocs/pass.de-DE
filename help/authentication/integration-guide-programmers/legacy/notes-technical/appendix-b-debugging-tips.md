---
title: Anhang B „Debugging-Tipps“
description: Anhang B „Debugging-Tipps“
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%
---
# (Veraltet) Anhang B: Tipps zum Debugging {#appendix-b-debugging-tips}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Löschen temporärer Daten {#clearing-temporary-data}

Die Adobe Pass-Authentifizierung speichert temporäre Daten wie Browser-Cache, LSOs-Cache und Cookies. Das Löschen temporärer Daten ist wichtig, um sicherzustellen, dass Sie beim Testen einen sauberen Anfang machen können.

- [Löschen des Browser-Cache und von Cookies](#clearing-the-browser-cache-and-cookies)
- [Löschen des LSO-Cache](#clearing-lsos-cache)


## Löschen des Browser-Cache und von Cookies {#clearing-the-browser-cache-and-cookies}

Er ist browsersicher, aber in Firefox: „Tools“ -\> „Aktuelle Historie löschen…“ -\> „Zeitspanne bis zum Löschen:“ „Alles“ auswählen; und auf „Details“: „Cookies“ und „Cache“ markieren -\> auf „Jetzt löschen“ klicken.


## Löschen des LSO-Cache {#clearing-lsos-cache}

Rufen Sie die [Flash Player Hilfe](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html) auf.

Wählen Sie die `entitlement.\*` aus (abhängig davon, was getestet wird) und klicken Sie auf „Website löschen“.


## Debugging-Tools {#tools}

Adobe Pass-Authentifizierungsingenieure verwenden die folgenden Debugging-Tools:

- Firebug - <http://www.getfirebug.com/>
- Flashbug (funktioniert mit der Debugversion des Flash-Players)
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>

