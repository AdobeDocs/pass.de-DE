---
title: AccessEnabler iOS/tvOS 3.7.0 Aktualisierungspfad
description: AccessEnabler iOS/tvOS 3.7.0 Aktualisierungspfad
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---

# AccessEnabler iOS/tvOS 3.7.0 Aktualisierungspfad {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

Änderungen des Schlüsselspeicherplatzes aus der [neuen AccessEnabler-Version 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md) sind nicht mit der Implementierung des Schlüsselchain-Speichers aus der AccessEnabler-Version unter 3.7.0 kompatibel.

Der Aktualisierungspfad für eine Anwendung, die die neue AccessEnabler-Version 3.7.0 verwendet, migriert alle Token aus früheren Versionen des Keychain-Speichers. Daher sollten Endbenutzer **während des Aktualisierungsprozesses des AccessEnabler-Frameworks keine Authentifizierungs-/Autorisierungssitzungen** verlieren.

## Bekannte Einschränkungen

Einige Einschränkungen, die unten beschrieben werden, können bei Implementierern auftreten.


1. Regular (Adobe) SSO funktioniert nicht zwischen einer Anwendung, die AccessEnabler Version 3.7.0 verwendet, und einer Anwendung, die AccessEnabler-Versionen unter 3.7.0 verwendet, auch nicht für Anwendungen, die von demselben Anbieter entwickelt wurden.

   >[!IMPORTANT]
   >
   >* Die SSO auf Systemebene (Apple) ist nicht betroffen!
   >
   >* Die reguläre (Adobe-) SSO funktioniert weiterhin, wenn beide Anwendungen vom selben Anbieter entwickelt werden und AccessEnabler-Versionen unter 3.7.0 verwenden!
   >
   >* Regular (Adobe) SSO funktioniert, wenn beide Anwendungen vom selben Anbieter entwickelt werden und AccessEnabler Version 3.7.0 verwenden!


1. Wenn eine Anwendung mit AccessEnabler Version 3.7.0 auf eine niedrigere Version von AccessEnabler herabgestuft wird, werden neue generierte Token nicht migriert. Daher kann es bei Endbenutzern zu einem Verlust von Authentifizierungs-/Autorisierungssitzungen kommen, ohne dass dies erwartet wird.

   >[!IMPORTANT]
   >
   >* Endbenutzer, die über die SSO auf Systemebene (Apple) authentifiziert wurden, sind davon nicht betroffen!
   >* Endbenutzer, die bereits authentifiziert wurden, bevor sie mit AccessEnabler Version 3.7.0 auf die neue Anwendung aktualisiert wurden, sind davon nicht betroffen!

1. Wenn eine Anwendung mit AccessEnabler Version 3.7.0 auf eine niedrigere Version von AccessEnabler herabgestuft wird, werden gelöschte Token nicht quittiert. Daher kann es vorkommen, dass Endbenutzer Authentifizierungs-/Autorisierungssitzungen vorfinden, ohne dass dies erwartet wird.
