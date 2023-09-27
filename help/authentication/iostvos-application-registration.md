---
title: Registrierung von iOS/tvOS-Anwendungen
description: Registrierung von iOS/tvOS-Anwendungen
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# Registrierung von iOS/tvOS-Anwendungen {#iostvos-application-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#Intro}

Ab Version 3.0 des iOS/tvOS AccessEnabler SDK ändern wir den Authentifizierungsmechanismus mit Adobe-Servern. Statt einen öffentlichen Schlüssel und ein geheimes System zum Signieren der Anforderer-ID zu verwenden, führen wir das Konzept einer Software-Anweisungszeichenfolge ein, mit der ein Zugriffstoken abgerufen werden kann, das später für alle Aufrufe verwendet wird, die das SDK an unsere Server sendet. Zusätzlich zu einer Softwareanweisung benötigen Sie auch ein benutzerdefiniertes URL-Schema für Ihre Anwendung.

Weitere Informationen finden Sie unter [Dynamische Kundenregistrierung](/help/authentication/dynamic-client-registration.md)

## Was ist eine Software-Anweisung? {#Soft_state}

Eine Software-Anweisung ist ein JWT-Token, das Informationen über Ihre Anwendung enthält. Jede Applikation sollte über eine eindeutige Softwareanweisung verfügen, die von unseren Servern verwendet wird, um die Applikation im Adobe-System zu identifizieren. Die Software-Anweisung muss übergeben werden, wenn Sie das AccessEnabler SDK initialisieren. Sie wird zur Registrierung der Anwendung bei Adobe verwendet. Nach der Registrierung erhält das SDK eine Client-ID und ein Client-Geheimnis, die zum Abrufen eines Zugriffstokens verwendet werden. Jeder Aufruf, den das SDK an unsere Server sendet, erfordert ein gültiges Zugriffstoken. Das SDK ist für die Registrierung der Anwendung, den Abruf und die Aktualisierung des Zugriffstokens zuständig.

**Hinweis:** Eine Software-Anweisung ist anwendungsspezifisch und dieselbe Softwareaussage kann nicht für mehrere Anwendungen verwendet werden. Bitte beachten Sie, dass Softwareanweisungen auf Programmebene ebenfalls dem gleichen folgen, d.h. sie können nur für einzelne Anwendungen verwendet werden - ob für einzelne Kanäle oder für mehrere Kanäle. Diese Einschränkung gilt auch für benutzerdefinierte Schemata.

## Wie erhalte ich eine Software-Anweisung? {#obtain}

### Wenn Sie Zugriff auf das Adobe TVE-Dashboard haben:

- Öffnen Sie den Browser und navigieren Sie zu <https://console.auth.adobe.com>
- Navigieren Sie zu `Channels` und wählen Sie Ihren Kanal aus.
- Navigieren Sie zu `Registered Applications` Registerkarte.
- Klicken Sie auf `Add new application`.
- Geben Sie einen Namen und eine Version für Ihre Anwendung an und wählen Sie die Plattformen aus, auf denen sie verfügbar sein soll. In unserem Fall iOS/tvOS.
- Übertragen Sie Ihre Änderungen auf den Server und navigieren Sie dann zurück zur Registerkarte Registrierte Anwendungen Ihres Kanals .
- Es sollte eine Liste mit allen registrierten Anwendungen angezeigt werden. Klicken Sie auf   `Download` auf der Anwendung, die Sie gerade erstellt haben. Möglicherweise müssen Sie einige Minuten warten, bis Ihre Software-Anweisung für den Download bereit ist.
- Eine Textdatei wird heruntergeladen. Verwenden Sie die Inhalte als Ihre Software-Anweisung.

Weitere Informationen finden Sie unter [Dynamisches Client-Registrierungs-Management](/help/authentication/dynamic-client-registration-management.md).

### Wenn Sie keinen Zugriff auf das Adobe TVE Dashboard haben:

Senden Sie ein Ticket an <tve-support@adobe.com>. Bitte fügen Sie alle notwendigen Informationen wie Kanal, Anwendungsname, Version und Plattformen hinzu und jemand aus unserem Supportteam wird eine Softwareanweisung für Sie erstellen.

## Wie wird die Software-Anweisung verwendet? {#use}

Nachdem Sie Ihre Software-Anweisung erhalten haben, müssen Sie sie als Paramenter im Access Enabler-Konstruktor übergeben. Wir empfehlen, die Software-Anweisung an einem Remote-Standort zu hosten. Auf diese Weise können Sie die Software-Anweisung einfach widerrufen und ändern, ohne eine neue Version Ihrer Anwendung veröffentlichen zu müssen.

## Erstellen eines benutzerdefinierten URL-Schemas für Ihre Anwendung {#generating}

### Wenn Sie Zugriff auf das Adobe TVE-Dashboard haben:

- Öffnen Sie den Browser und navigieren Sie zu <https://console.auth.adobe.com>
- Navigieren Sie zu `Channels` und wählen Sie Ihren Kanal aus.
- Navigieren Sie zu `Custom Schemes` Registerkarte.
- Klicken Sie auf `Generate a new custom scheme`.
- Für Ihre Anwendung wird ein neues benutzerdefiniertes Schema generiert. Beispiel: `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- Übertragen Sie Ihre Änderungen auf den Server.

### Wenn Sie keinen Zugriff auf das Adobe TVE Dashboard haben:

Senden Sie ein Ticket an <tve-support@adobe.com>. Bitte geben Sie die Kanal-ID an und jemand aus unserem Support-Team wird ein benutzerdefiniertes Schema für Sie erstellen.

## Verwendung des benutzerdefinierten Schemas {#use_custom}

In der `info.plist` -Datei den folgenden Code hinzufügen:

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
