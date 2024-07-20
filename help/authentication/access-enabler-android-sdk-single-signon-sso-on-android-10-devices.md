---
title: Zugriff auf Enabler Android SDK Single Sign-On (SSO) in Android 10-Apps
description: Zugriff auf Enabler Android SDK Single Sign-On (SSO) in Android 10-Apps
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 0%

---

# Zugriff auf Enabler Android SDK Single Sign-On (SSO) in Android 10-Apps {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht

Single Sign-On (SSO) zwischen authentifizierungsfähigen Apps von Adobe Pass ist auf Geräten mit Android OS über Access Enabler Android SDK verfügbar. Um Single Sign-On (SSO) auf Android-Geräten anzubieten, verwenden die Access Enabler Android SDK-Version 3.2.1 (neueste Version) und frühere Versionen eine freigegebene Datenbankdatei, die in einer Android-Speicherimplementierung gespeichert ist und von allen mit Adobe Pass Authentication verbundenen Apps aufgerufen werden kann.

Google in der neuesten Version von Android 10 führte jedoch zu einigen Änderungen, &quot;um Benutzern mehr Kontrolle über ihre Dateien zu geben und die Datei-Unübersichtlichkeit zu begrenzen. Apps, die auf Android 10 (API-Ebene 29) und höher abzielen, erhalten standardmäßig einen Scoped-Zugriff auf ein externes Speichergerät oder einen Scoped-Speicher. Solche Apps können nur das App-spezifische Verzeichnis `\[...\]` sehen. Weitere Informationen zu diesen Speicheränderungen in Android 10 finden Sie in der [Dokumentation zur Daten- und Dateispeicherung für Android](https://developer.android.com/training/data-storage/files/external-scoped).

Aufgrund dieser Änderungen können sich die Single Sign-On (SSO), die von Access Enabler Android-Version **3.2.1 SDK (neueste Version)** angeboten wird, und frühere Versionen auf Android 10-Geräten auswirken, wie im nächsten Abschnitt erläutert.

Siehe [Roku SSO Overview](/help/authentication/roku-sso-overview.md).

## Verhalten

Je nach **[!UICONTROL target SDK level]** Ihrer App oder der Verwendung des Manifestattributs **android:requestLegacyExternalStorage** verhält sich das Single Sign-On (SSO), das vom Access Enabler Android-SDK Version 3.2.1 (neueste Version) angeboten wird, wie folgt:

- Ihre App zielt auf **Android 9 (API-Ebene 28)** oder niedrigere **-\>** Single-Sign-On (SSO) **ab**
- Ihre App zielt auf **Android 10** **(API-Ebene 29)** ab und setzt **** den Wert von **requestLegacyExternalStorage in der Manifestdatei Ihrer App auf &quot;true**&quot;**-\>** Single Sign-On (SSO) **funktioniert**
- Ihre App zielt auf **Android 10** **(API-Ebene 29)** ab und setzt **nicht** den Wert von **requestLegacyExternalStorage in der Manifestdatei Ihrer App auf &quot;true**&quot;**-\>** Single Sign-On (SSO) **funktioniert nicht**


>[!TIP]
>
> Bevor das Adobe Pass Authentication Access Enabler Android SDK vollständig mit dem Scoped Storage kompatibel ist, können Sie sich vorübergehend vom Opt-out aus der Ziel-SDK-Ebene Ihrer App oder dem Manifestattribut requestLegacyExternalStorage ausschließen, wie in der öffentlichen [Android-Dokumentation](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage) erläutert.
