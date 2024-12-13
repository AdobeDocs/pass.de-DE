---
title: Adobe Pass-Authentifizierung und das neue Android 6-Berechtigungsmodell „Marshmallow“
description: Adobe Pass-Authentifizierung und das neue Android 6-Berechtigungsmodell „Marshmallow“
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# (Legacy) Adobe Pass-Authentifizierung und das neue Android 6-Berechtigungsmodell „Marshmallow“ {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>

Die neue Android 6 Marshmallow-Version führt einige Aktualisierungen des Berechtigungsmodells ein, die sich auf das Verhalten von Apps auswirken können, die die bestehende Adobe Pass Authentication SDK Version 1.8 und älter verwenden.

Als neue Funktion bietet das neue Android-Betriebssystem [granulare Kontrolle über die Berechtigungen, die Apps zum Zeitpunkt der Installation und zur Laufzeit benötigen](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html).

>[!IMPORTANT]
>
>Die unten beschriebenen Änderungen betreffen **nur Programme, die speziell für Android 6.0 entwickelt wurden** (targetSdkVersion=23). Sie wirken sich nicht auf ältere Anwendungen aus, die beim Upgrade auf Android 6.0 bereits auf dem Benutzergerät installiert sind.


Insbesondere für Apps, die in Android Studio mit [API Level 23](http://developer.android.com/sdk/api_diff/23/changes.html) entwickelt wurden und die die Adobe Pass-Authentifizierungs-SDK verwenden, muss der Entwickler benutzerdefinierten Code schreiben (siehe Code-Snippet unten), [ den Trigger für das Dialogfeld „Berechtigungen zulassen/verweigern“](https://developer.android.com/training/permissions/requesting.html).

Im Folgenden finden Sie den Code-Auszug, der zum Anfordern des Schreibzugriffs auf den externen Speicher des Geräts verwendet wird:

```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
        != PackageManager.WRITE_EXTERNAL_STORAGE) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE);

        // MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```




**Aus Benutzersicht werden** Benutzer bei der Installation von einem Fenster begrüßt, in dem sie aufgefordert werden, Lese-/Schreibberechtigungen für Dateien zu bestätigen (siehe Abbildung 2 unten). Dies führt zu einem der beiden folgenden Ergebnisse:

1. Wenn der Benutzer **bestätigt** werden die Berechtigungen beibehalten, der reguläre Authentifizierungsfluss wird beibehalten und Token werden im globalen Speicher gespeichert. Benutzer bleiben in der App und über Apps hinweg mithilfe der Adobe Pass-Authentifizierung authentifiziert, solange die Token gültig sind.
1. Wenn der Benutzer **verweigert** die Berechtigungen, schlagen Schreibaktionen im Speicher fehl und die Benutzer werden nur authentifiziert, bis sie die App verlassen. Beachten Sie, dass einige Anwendungen beim Wechsel zwischen Vordergrund und Hintergrund neu initialisiert werden, sodass die Benutzer bei dieser Aktion abgemeldet werden. Token werden NICHT gespeichert und die Benutzer müssen sich jedes Mal authentifizieren, wenn sie die App verwenden.


>[!TIP]
>
>Eine Funktion, die die Speicherresilienz einführt, ist derzeit für die Adobe Pass-Authentifizierung SDK 1.9 in Entwicklung. Der neue SDK ist für **Release in der letzten Oktoberwoche** vorgesehen. Die Anwendung greift immer dann auf das Schreiben im Sandbox-Speicher der Anwendung zurück, wenn der allgemeine Speicher nicht verwendet werden kann. Dies gilt für den Fall, dass Benutzer für Anwendungen, die auf API-Ebene 23 entwickelt wurden, KEINE Lese-/Schreibberechtigungen für den globalen Speicher akzeptieren. Die Token werden pro App einzeln gespeichert, was bedeutet, dass Single-Sign-On zwischen Apps mit Adobe Pass-Authentifizierung deaktiviert ist.


![](../../../assets/android-permissions-request.png)

*Abbildung: Das Dialogfeld „Berechtigungsanfrage“ für geschriebene Apps, die auf API-Ebene 23 abzielen*

>[!IMPORTANT]
>
> Adobe empfiehlt **seinen Partnern, Apps mit API Level 22 (targetSdkVersion=22) oder älter zu entwickeln, um ein optimales Benutzererlebnis im Authentifizierungsprozess zu gewährleisten**.
