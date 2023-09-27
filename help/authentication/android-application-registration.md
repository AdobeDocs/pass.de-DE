---
title: Registrierung von Android-Anwendungen
description: Registrierung von Android-Anwendungen
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Registrierung von Android-Anwendungen {#android-application-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#intro}

Ab Version 3.0 des Android AccessEnabler SDK ändern wir den Authentifizierungsmechanismus mit Adobe-Servern. Statt einen öffentlichen Schlüssel und ein geheimes System zum Signieren der Anforderer-ID zu verwenden, führen wir das Konzept einer Software-Anweisungszeichenfolge ein, mit der ein Zugriffstoken abgerufen werden kann, das später für alle Aufrufe verwendet wird, die das SDK an unsere Server sendet. Zusätzlich zu einer Softwareanweisung müssen Sie auch einen Deep-Link für Ihre Anwendung erstellen.

Weitere Informationen finden Sie unter [Dynamische Kundenregistrierung](/help/authentication/dynamic-client-registration.md)

## Was ist eine Software-Anweisung? {#what}

Eine Software-Anweisung ist ein JWT-Token, das Informationen über Ihre Anwendung enthält. Jede Applikation sollte über eine eindeutige Softwareanweisung verfügen, die von unseren Servern verwendet wird, um die Applikation im Adobe-System zu identifizieren. Die Software-Anweisung muss übergeben werden, wenn Sie das AccessEnabler SDK initialisieren. Sie wird zur Registrierung der Anwendung bei Adobe verwendet. Nach der Registrierung erhält das SDK eine Client-ID und ein Client-Geheimnis, die zum Abrufen eines Zugriffstokens verwendet werden. Jeder Aufruf, den das SDK an unsere Server sendet, erfordert ein gültiges Zugriffstoken. Das SDK ist für die Registrierung der Anwendung, den Abruf und die Aktualisierung des Zugriffstokens zuständig.

>[!NOTE]
>
>Softwareanweisungen sind App-spezifisch und eine einzelne Software-Anweisung kann nicht für mehrere Anwendungen verwendet werden. Bitte beachten Sie, dass Softwareanweisungen auf Programmebene die gleichen Einschränkungen aufweisen, sie können nur für eine einzelne Anwendung verwendet werden, ob für einzelne Kanäle oder für mehrere Kanäle.

## Wie erhalte ich eine Software-Anweisung? {#how-to-get-ss}

### Wenn Sie Zugriff auf das Adobe TVE-Dashboard haben:

* Öffnen Sie den Browser und navigieren Sie zu [Adobe Pass TVE-Dashboard](https://console.auth.adobe.com).
* Navigieren Sie zu `Channels` und wählen Sie Ihren Kanal aus.
* Navigieren Sie zu `Registered Applications` Registerkarte.
* Klicken Sie auf `Add new application`.
* Geben Sie einen Namen und eine Version für Ihre Anwendung an und wählen Sie die Plattformen aus, auf denen sie verfügbar sein soll. Android in unserem Fall.
* Geben Sie einen Domänennamen an, indem Sie aus einer Liste von Domänen auswählen, die bereits für Ihren Programmierer konfiguriert sind.
* Übertragen Sie Ihre Änderungen auf den Server und navigieren Sie dann zurück zur Registerkarte Registrierte Anwendungen des Kanals.
* Es sollte eine Liste mit allen registrierten Anwendungen angezeigt werden. Wählen Sie die **Herunterladen** auf der Anwendung, die Sie gerade erstellt haben. Möglicherweise müssen Sie einige Minuten warten, bis Ihre Software-Anweisung für den Download bereit ist.
* Eine Textdatei wird heruntergeladen. Verwenden Sie den Inhalt als Ihre Software-Anweisung.

Weitere Informationen finden Sie unter [Dynamisches Client-Registrierungs-Management](/help/authentication/dynamic-client-registration-management.md)

### Wenn Sie keinen Zugriff auf das Adobe TVE Dashboard haben:

Senden Sie ein Ticket an `tve-support@adobe.com`. Bitte fügen Sie alle notwendigen Informationen wie Kanal, Anwendungsname, Version und Plattformen hinzu. Jemand aus unserem Supportteam wird eine Softwareanweisung für Sie erstellen.

## Wie wird die Software-Anweisung verwendet? {#how-to-use-ss}

Nachdem Sie Ihre Software-Anweisung erhalten haben, müssen Sie sie als Paramenter im Access Enabler-Konstruktor übergeben. Wir empfehlen, die Software-Anweisung an einem Remote-Standort zu hosten. Auf diese Weise können Sie die Software-Anweisung einfach widerrufen und ändern, ohne eine neue Version Ihrer Anwendung veröffentlichen zu müssen.

## Erstellen und Verwenden eines Deep-Links für Ihre Anwendung {#create}

Verwenden Sie unter Android als Deep-Link-Wert die Rückseite des Domänennamens, der beim Erstellen der Software-Anweisung ausgewählt wurde.

Der erstellte Deep-Link sollte auf dem Android-Gerät einen eindeutigen Wert aufweisen. Wenn mehrere Anwendungen denselben Deep-Link-Wert verwenden, stören die Authentifizierungs- und Abmeldevorgänge.

## Verwendung der Software-Anweisung und des Deep-Links {#use-both}

In der Ressourcendatei Ihrer Anwendung `strings.xml` den folgenden Code hinzufügen:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
