---
title: Anhang B „Debugging-Tipps“
description: Anhang B „Debugging-Tipps“
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---

# (Veraltet) Anhang B: Tipps zum Debugging {#appendix-b-debugging-tips}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.


## Löschen temporärer Daten {#clearing-temporary-data}

Die Adobe Pass-Authentifizierung speichert temporäre Daten wie Browser-Cache, LSOs-Cache und Cookies. Das Löschen temporärer Daten ist wichtig, um sicherzustellen, dass Sie beim Testen einen sauberen Anfang machen können.

- [Löschen des Browser-Cache und von Cookies](#clearing-the-browser-cache-and-cookies)
- [Löschen des LSO-Cache](#clearing-lsos-cache)


## Löschen des Browser-Cache und von Cookies {#clearing-the-browser-cache-and-cookies}

Er ist browsersicher, aber in Firefox: „Tools“ -\> „Aktuelle Historie löschen…“ -\> „Zeitspanne bis zum Löschen:“ „Alles“ auswählen; und auf „Details“: „Cookies“ und „Cache“ markieren -\> auf „Jetzt löschen“ klicken.


## Löschen des LSO-Cache {#clearing-lsos-cache}

Zugriff auf die [Flash Player-Hilfe](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

Wählen Sie die ```entitlement.\*``` aus (abhängig davon, was getestet wird) und klicken Sie auf „Website löschen“.


## Debugging-Tools {#tools}

Adobe Pass-Authentifizierungsingenieure verwenden die folgenden Debugging-Tools:

- Firebug - <http://www.getfirebug.com/>
- Flashbug (funktioniert mit der Debugversion des Flash-Players)
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
