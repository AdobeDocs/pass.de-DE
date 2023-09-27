---
title: AccessEnabler iOS/tvOS 3.7.0 Aktualisierungspfad
description: AccessEnabler iOS/tvOS 3.7.0 Aktualisierungspfad
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# AccessEnabler iOS/tvOS 3.7.0 Aktualisierungspfad {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

Änderungen am Keychain-Speicher von [neue AccessEnabler-Version 3.7.0](/help/authentication/authn-rn-ios-tvos-370.md) sind nicht mit der Implementierung des Keychain-Speichers aus der AccessEnabler-Version unter 3.7.0 kompatibel.

Der Aktualisierungspfad für eine Anwendung, die die neue AccessEnabler-Version 3.7.0 verwendet, migriert alle Token aus früheren Versionen des Keychain-Speichers. Daher werden die Endnutzer **sollte keinen Verlust von Authentifizierungs-/Autorisierungssitzungen erleben** während des Aktualisierungsprozesses des AccessEnabler-Frameworks.

## Bekannte Einschränkungen

Einige Einschränkungen, die unten beschrieben werden, können bei Implementierern auftreten.


1. Regular (Adobe) SSO funktioniert nicht zwischen einer Anwendung, die AccessEnabler Version 3.7.0 verwendet, und einer Anwendung, die AccessEnabler-Versionen unter 3.7.0 verwendet, auch nicht für Anwendungen, die von demselben Anbieter entwickelt wurden.

   - **Wichtig:**
      - Die SSO auf Systemebene (Apple) ist nicht betroffen!
      - Die reguläre (Adobe-) SSO funktioniert weiterhin, wenn beide Anwendungen vom selben Anbieter entwickelt werden und AccessEnabler-Versionen unter 3.7.0 verwenden!
      - Regular (Adobe) SSO funktioniert, wenn beide Anwendungen vom selben Anbieter entwickelt werden und AccessEnabler Version 3.7.0 verwenden!

1. Wenn eine Anwendung mit AccessEnabler Version 3.7.0 auf eine niedrigere Version von AccessEnabler herabgestuft wird, werden neue generierte Token nicht migriert. Daher kann es bei Endbenutzern zu einem Verlust von Authentifizierungs-/Autorisierungssitzungen kommen, ohne dass dies erwartet wird.

   - **Wichtig:**
      - Endbenutzer, die über die SSO auf Systemebene (Apple) authentifiziert wurden, sind davon nicht betroffen!
      - Endbenutzer, die bereits authentifiziert wurden, bevor sie mit AccessEnabler Version 3.7.0 auf die neue Anwendung aktualisiert wurden, sind davon nicht betroffen!

1. Wenn eine Anwendung mit AccessEnabler Version 3.7.0 auf eine niedrigere Version von AccessEnabler herabgestuft wird, werden gelöschte Token nicht quittiert. Daher kann es vorkommen, dass Endbenutzer Authentifizierungs-/Autorisierungssitzungen vorfinden, ohne dass dies erwartet wird.
