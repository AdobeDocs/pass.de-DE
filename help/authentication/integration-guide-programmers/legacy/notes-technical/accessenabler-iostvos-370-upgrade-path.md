---
title: Aktualisierungspfad für AccessEnabler iOS/tvOS 3.7.0
description: Aktualisierungspfad für AccessEnabler iOS/tvOS 3.7.0
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# (Legacy) Upgrade-Pfad für AccessEnabler iOS/tvOS 3.7.0 {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>

Änderungen am Schlüsselbund-Speicher der [neuen AccessEnabler-Version 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md) sind nicht kompatibel mit der Implementierung des Schlüsselbund-Speichers der AccessEnabler-Version unter 3.7.0.

Der Upgrade-Pfad für eine Anwendung, die die neue AccessEnabler-Version 3.7.0 verwendet, migriert alle Token aus früheren Versionen des Schlüsselbund-Speichers. Daher sollten Endbenutzenden **während des Aktualisierungsprozesses des AccessEnabler-Frameworks keine**-/Autorisierungssitzungen verloren gehen).

## Bekannte Einschränkungen

Bei Implementierern können einige der unten beschriebenen Einschränkungen auftreten.


1. Reguläres (Adobe) SSO funktioniert nicht zwischen einer Anwendung mit AccessEnabler Version 3.7.0 und einer Anwendung mit AccessEnabler Version/s unter 3.7.0, auch nicht bei Anwendungen, die vom gleichen Anbieter entwickelt wurden.

   >[!IMPORTANT]
   >
   >* SSO auf Systemebene (Apple) ist davon nicht betroffen!
   >
   >* Reguläres (Adobe) SSO funktioniert weiterhin, wenn beide Anwendungen vom gleichen Anbieter entwickelt werden und AccessEnabler-Versionen unter 3.7.0 verwenden!
   >
   >* Das reguläre (Adobe) SSO funktioniert, wenn beide Anwendungen vom gleichen Anbieter entwickelt wurden und AccessEnabler Version 3.7.0 verwenden!


1. Wenn eine Anwendung mit AccessEnabler Version 3.7.0 auf eine niedrigere Version von AccessEnabler heruntergestuft wird, werden neu generierte Token nicht migriert. Daher können Endbenutzende den Verlust von Authentifizierungs-/Autorisierungssitzungen erleben, ohne dies zu erwarten.

   >[!IMPORTANT]
   >
   >* Endbenutzer, die über SSO auf Systemebene (Apple) authentifiziert wurden, sind davon nicht betroffen!
   >* Endbenutzer, die bereits vor dem Update auf die neue Anwendung mit AccessEnabler Version 3.7.0 authentifiziert wurden, sind davon nicht betroffen!

1. Wenn eine Anwendung mit AccessEnabler Version 3.7.0 auf eine niedrigere Version von AccessEnabler herabgestuft wird, werden gelöschte Token nicht quittiert. Daher können Endbenutzer das Vorhandensein von Authentifizierungs-/Autorisierungssitzungen erleben, ohne dies zu erwarten.
