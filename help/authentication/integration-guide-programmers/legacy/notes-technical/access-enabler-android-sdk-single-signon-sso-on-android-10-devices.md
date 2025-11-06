---
title: Zugriff auf Enabler Android SDK Single Sign-On (SSO) für Android 10-Apps
description: Zugriff auf Enabler Android SDK Single Sign-On (SSO) für Android 10-Apps
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# (Legacy) Access Enabler Android SDK Single Sign-on (SSO) für Android 10-Apps {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Überblick

Single Sign-On (SSO) zwischen Apps mit Adobe Pass-Authentifizierung ist auf Geräten mit Android OS über Access Enabler Android SDK verfügbar. Um Single Sign-On (SSO) auf Android-Geräten anbieten zu können, verwenden Access Enabler Android SDK Version 3.2.1 (neueste Version) und frühere Versionen eine freigegebene Datenbankdatei, die in einer Android-Speicherimplementierung gespeichert ist, auf die alle Apps mit Adobe Pass-Authentifizierung zugreifen können.

Google in der neuesten Version von Android 10 hat jedoch einige Änderungen hervorgebracht, um Benutzenden mehr Kontrolle über ihre Dateien zu geben und die Dateiüberlastung zu begrenzen. Apps, die auf Android 10 (API-Ebene 29) und höher abzielen, erhalten standardmäßig umfassenden Zugriff auf ein externes Speichergerät oder einen Speicherbereich. Solche Apps können nur ihre App-spezifischen `\[...\]` sehen“. Weitere Informationen zu diesen Android 10-Speicheränderungen finden Sie unter [Dokumentation zur Daten- und Dateispeicherung für Android](https://developer.android.com/training/data-storage/files/external-scoped).

Infolge dieser Änderungen können das Single Sign-On (SSO), das von Access Enabler Android Version **3.2.1 SDK (aktuelle)** und früheren Versionen angeboten wird, auf Android 10-Geräten beeinflusst werden, wie im nächsten Abschnitt erläutert.

## Verhalten

Je nach **[!UICONTROL target SDK level]** Ihrer App oder der Verwendung des Manifestattributs **android:requestLegacyExternalStorage** verhält sich das Single Sign-On (SSO), das von Access Enabler Android Version 3.2.1 SDK (neueste Version) und früheren Versionen angeboten wird, derzeit wie folgt:

- Ihre App für **Android 9 (API-Ebene 28)** oder niedriger **-\>** Single Sign-On (SSO) **funktioniert**
- Ihre App ist für **Android 10** **(API-Ebene 29)** vorgesehen und **&#x200B;**&#x200B;den Wert von **requestLegacyExternalStorage in der Manifestdatei**-\>**&#x200B;** SSO (Single Sign-On) Ihrer App auf **gesetzt**
- Ihre App ist für **Android 10** **(API-Ebene 29)** vorgesehen und **&#x200B;**&#x200B;den Wert von **requestLegacyExternalStorage in der Manifestdatei**-\>**SSO (Single Sign-On) Ihrer App auf** nicht **festgelegt**

>[!TIP]
>
> Bevor Adobe Pass Authentication Access Enabler Android SDK vollständig mit dem Speicherbereich kompatibel ist, können Sie vorübergehend auf der Grundlage der SDK-Zielebene Ihrer App oder des Attributs requestLegacyExternalStorage-Manifest abmelden, wie in der öffentlichen [Android-Dokumentation](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage) erläutert.
