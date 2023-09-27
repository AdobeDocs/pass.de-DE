---
title: Clientloser API-Ablauf bei Fehlen der Geräte-ID
description: Clientloser API-Ablauf bei Fehlen der Geräte-ID
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# Clientloser API-Ablauf bei Fehlen der Geräte-ID {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>


## Problem

Nicht alle Apps mit intelligenten Geräten können eine eindeutige Geräte-ID bereitstellen.  Da deviceId ein obligatorischer Parameter ist, gibt der Dienst einen 400-Fehler zurück, wenn er nicht übergeben wird.


## Temporäre Lösung/Problemumgehung

Für Clients ohne Geräte-ID:

1. Rufen Sie den Registrierungs-Code-Dienst zum ersten Mal mit `deviceId=dummy`
1. Extrahieren Sie aus der Antwort die UUID. Die UUID ist im Element &quot;id&quot;der Registrierungscode-Antwort (XML- und JSON-Antwortformate) verfügbar.
1. Rufen Sie den Registrierungsdienst ein zweites Mal auf. Übergeben Sie diese Zeit `deviceId=<uuid obtained in step #2>`
1. Anzeigen des Registrierungs-Codes, der in Schritt 3 auf der Benutzeroberfläche der Konsole abgerufen wurde


Nachdem diese Schritte ausgeführt wurden, verwendet die Adobe Pass-Authentifizierung die UUID als Geräte-ID. Speichern Sie diese Geräte-ID (UUID) im lokalen Speicher des Geräts. Falls der Benutzer einen neuen Registrierungs-Code generiert, sollten Sie die Schritte 1 bis 4 erneut ausführen und dann die zuvor gespeicherte Geräte-ID (UUID) durch die neue ersetzen.



## Ständige Lösung

Adobe wird dies in einer zukünftigen Version ändern, indem Sie `deviceId` eine optionale Nutzlast beim Erstellen des reg-Codes und bei Verwendung von UUID als Token-Schlüssel anstelle von `deviceId`, wenn `deviceId` nicht vorhanden ist.

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
