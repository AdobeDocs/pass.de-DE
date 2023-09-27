---
title: Zugriff auf Enabler Android SDK Single Sign-On (SSO) in Android 10-Apps
description: Zugriff auf Enabler Android SDK Single Sign-On (SSO) in Android 10-Apps
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# Zugriff auf Enabler Android SDK Single Sign-On (SSO) in Android 10-Apps {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht

Single Sign-On (SSO) zwischen authentifizierungsfähigen Apps von Adobe Pass ist auf Geräten mit Android OS über Access Enabler Android SDK verfügbar. Um Single Sign-On (SSO) auf Android-Geräten anzubieten, verwenden die Access Enabler Android SDK-Version 3.2.1 (neueste Version) und frühere Versionen eine freigegebene Datenbankdatei, die in einer Android-Speicherimplementierung gespeichert ist und auf die alle mit Adobe Pass Authentication-basierten Apps zugreifen können.

Google in der neuesten Version von Android 10 führte jedoch zu einigen Änderungen, &quot;um Benutzern mehr Kontrolle über ihre Dateien zu geben und die Datei-Unübersichtlichkeit zu beschränken, werden Apps mit Android 10 (API-Ebene 29) und höher standardmäßig über einen Umfang auf ein externes Speichergerät oder einen Speicherplatz verfügen. Solche Apps können nur ihr App-spezifisches Verzeichnis sehen `\[...\]`&quot;. Weitere Informationen zu diesen Änderungen am Speicher von Android 10 finden Sie unter [Daten- und Dateispeicherdokumentation für Android](https://developer.android.com/training/data-storage/files/external-scoped).

Als Ergebnis dieser Änderungen bietet die Single Sign-On (SSO) von Access Enabler Android-Version **3.2.1 SDK (neueste Version)** und frühere Versionen können auf Android 10-Geräten betroffen sein, wie im nächsten Abschnitt beschrieben.

Siehe [Roku SSO-Übersicht](/help/authentication/roku-sso-overview.md).

## Verhalten

Abhängig von der **[!UICONTROL target SDK level]** oder die Verwendung **android:requestLegacyExternalStorage** manifest attribute the Single Sign-On (SSO) von Access Enabler Android-SDK Version 3.2.1 (neueste Version) und früheren Versionen werden sich derzeit wie folgt verhalten:

- Ihre App-Ziele **Android 9 (API-Ebene 28)** oder niedriger **-\>** Single Sign-On (SSO) **wird funktionieren**
- Ihre App-Ziele **Android 10** **(API-Ebene 29)** und **set** der Wert von **requestLegacyExternalStorage auf true** in der Manifestdatei Ihrer App **-\>** Single Sign-On (SSO) **wird funktionieren**
- Ihre App-Ziele **Android 10** **(API-Ebene 29)** und **nicht festgelegt** der Wert von **requestLegacyExternalStorage auf true** in der Manifestdatei Ihrer App **-\>** Single Sign-On (SSO) **funktioniert nicht**


>[!TIP]
>
> Bevor das Adobe Pass Authentication Access Enabler Android SDK vollständig mit dem Scoped-Speicher kompatibel ist, können Sie sich vorübergehend vom Opt-out aus der Ziel-SDK-Ebene Ihrer App oder dem Manifestattribut requestLegacyExternalStorage ausschließen, wie in der öffentlichen Erklärung erläutert [Android-Dokumentation](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
