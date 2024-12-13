---
title: Client-loser API-Fluss ohne Geräte-ID
description: Client-loser API-Fluss ohne Geräte-ID
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# (Veraltete) Client-lose API-Fluss ohne Geräte-ID {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>


## Problem

Nicht alle Apps für intelligente Geräte können eine eindeutige Geräte-ID bereitstellen.  Da deviceId ein obligatorischer Parameter ist, gibt der Service einen 400-Fehler zurück, wenn er nicht übergeben wird.


## Temporäre Lösung/Problemumgehung

Für Clients ohne Geräte-ID:

1. Rufen Sie den Registrierungs-Code-Dienst beim ersten Mal mit `deviceId=dummy` auf
1. Extrahieren Sie die UUID aus der Antwort. Die UUID ist im Element „id“ der Antwort des Registrierungs-Codes (XML- und JSON-Antwortformate) verfügbar.
1. Rufen Sie den Registrierungsdienst ein zweites Mal auf. Dieses Mal `deviceId=<uuid obtained in step #2>`
1. Anzeigen des in Schritt 3 erhaltenen Registrierungs-Codes in der Konsolen-Benutzeroberfläche


Nachdem diese Schritte ausgeführt wurden, verwendet die Adobe Pass-Authentifizierung die UUID als Geräte-ID. Speichern Sie diese Geräte-ID (UUID) im lokalen Speicher des Geräts. Für den Fall, dass der Benutzer einen neuen Registrierungs-Code generiert, sollten Sie erneut die Schritte 1 bis 4 ausführen und dann die zuvor gespeicherte Geräte-ID (UUID) durch die neue ersetzen.



## Dauerlösung

Adobe wird dies in einer zukünftigen Version ändern, indem `deviceId` beim Erstellen des Reg-Codes zu einer optionalen Payload wird und UUID als Token-Schlüssel anstelle von `deviceId` verwendet wird, wenn `deviceId` nicht vorhanden ist.

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
