---
title: Registrierung von iOS/tvOS-Anwendungen
description: Registrierung von iOS/tvOS-Anwendungen
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---


# (Veraltete) Registrierung von iOS/tvOS-Anwendungen {#iostvos-application-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung {#Intro}

Ab Version 3.0 des iOS/tvOS AccessEnabler SDK ändern wir den Authentifizierungsmechanismus für Adobe-Server. Anstatt ein öffentliches Schlüssel- und Geheimsystem zum Signieren der RequestorID zu verwenden, führen wir das Konzept einer Software-Anweisungszeichenfolge ein, mit der ein Zugriffstoken abgerufen werden kann, das später für alle Aufrufe verwendet wird, die SDK an unsere Server sendet. Zusätzlich zu einer Software-Anweisung benötigen Sie auch ein benutzerdefiniertes URL-Schema für Ihre Anwendung.

Weitere Informationen finden Sie unter [Übersicht über die dynamische Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Was ist eine Software-Anweisung? {#Soft_state}

Ein Software-Statement ist ein JWT-Token, das Informationen über Ihr Programm enthält. Jede Anwendung sollte über eine eindeutige Software-Anweisung verfügen, die von unseren Servern verwendet wird, um die Anwendung im System von Adobe zu identifizieren. Die Software-Anweisung muss beim Initialisieren der AccessEnabler-SDK übergeben werden und wird zur Registrierung der Anwendung bei Adobe verwendet. Bei der Registrierung erhält die SDK eine Client-ID und ein Client-Geheimnis, die zum Abrufen eines Zugriffs-Tokens verwendet werden. Jeder Aufruf, den SDK an unsere Server sendet, erfordert ein gültiges Zugriffstoken. Die SDK ist für die Registrierung der Anwendung, den Abruf und die Aktualisierung des Zugriffstokens verantwortlich.

**Hinweis:** Eine Software-Anweisung ist anwendungsspezifisch und dieselbe Software-Anweisung kann nicht für mehr als eine Anwendung verwendet werden. Bitte beachten Sie, dass die Anweisungen auf Programmierebene auch das gleiche befolgen, d.h. sie können nur für eine Anwendung verwendet werden - egal ob Einkanal oder Mehrkanal. Diese Einschränkung gilt auch für benutzerdefinierte Schemata.

## Wie erhalte ich ein Software-Statement? {#obtain}

### Wenn Sie Zugriff auf das TVE-Dashboard von Adobe haben:

- Öffnen Sie Ihren Browser und navigieren Sie zu <https://experience.adobe.com/#/pass/authentication>
- Navigieren Sie zu `Channels` Abschnitt und wählen Sie Ihren Kanal aus.
- Navigieren Sie zur Registerkarte `Registered Applications` .
- Klicken Sie auf `Add new application`.
- Geben Sie einen Namen und eine Version für Ihr Programm ein und wählen Sie   Plattformen, auf denen es verfügbar sein wird. iOS/tvOS in unserem Fall.
- Übertragen Sie Ihre Änderungen auf den Server und navigieren Sie dann zurück zur Registerkarte Registrierte Anwendungen Ihres Kanals.
- Es sollte eine Liste mit allen registrierten Anwendungen angezeigt werden. Klicken Sie auf die Schaltfläche   `Download` Schaltfläche in der soeben erstellten Anwendung. Möglicherweise müssen Sie einige Minuten warten, bevor Ihre Software-Erklärung zum Download bereit ist.
- Eine Textdatei wird heruntergeladen. Verwenden Sie den Inhalt als Software-Erklärung.

Weitere Informationen finden Sie unter [Verwaltung der dynamischen Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Wenn Sie keinen Zugriff auf das TVE-Dashboard von Adobe haben:

Senden Sie ein Ticket an <tve-support@adobe.com>. Bitte geben Sie alle notwendigen Informationen wie Kanal, Anwendungsname, Version und Plattformen an. Jemand aus unserem Supportteam wird ein Software-Statement für Sie erstellen.

## Wie wird die Software-Anweisung verwendet? {#use}

Nachdem Sie Ihre Software-Anweisung erhalten haben, müssen Sie sie als Parameter im Access Enabler-Konstruktor übergeben. Wir empfehlen, die Software-Erklärung an einem Remote-Standort zu hosten. Auf diese Weise können Sie die Software-Erklärung einfach widerrufen und ändern, ohne eine neue Version Ihres Programms freizugeben.

## Erstellen eines benutzerdefinierten URL-Schemas für Ihr Programm {#generating}

### Wenn Sie Zugriff auf das TVE-Dashboard von Adobe haben:

- Öffnen Sie Ihren Browser und navigieren Sie zu <https://experience.adobe.com/#/pass/authentication>
- Navigieren Sie zu `Channels` Abschnitt und wählen Sie Ihren Kanal aus.
- Navigieren Sie zur Registerkarte `Custom Schemes` .
- Klicken Sie auf `Generate a new custom scheme`.
- Für Ihr Programm wird ein neues benutzerdefiniertes Schema generiert. Beispiel: `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- Übertragen Sie Ihre Änderungen auf den Server.

### Wenn Sie keinen Zugriff auf das TVE-Dashboard von Adobe haben:

Senden Sie ein Ticket an <tve-support@adobe.com>. Bitte geben Sie die Kanal-ID an. Ein Mitarbeiter unseres Supportteams wird ein benutzerdefiniertes Schema für Sie erstellen.

## Verwendung des benutzerdefinierten Schemas {#use_custom}

Fügen Sie in der `info.plist`-Datei Ihrer Anwendung den folgenden Code hinzu:

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
