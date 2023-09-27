---
title: Amazon FireOS-Anwendungsregistrierung
description: Amazon FireOS-Anwendungsregistrierung
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Amazon FireOS-Anwendungsregistrierung {#amazon-fireos-application-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Einführung {#intro}

Ab Version 3.0 des FireOS AccessEnabler SDK ändern wir den Authentifizierungsmechanismus mit Adobe-Servern. Statt einen öffentlichen Schlüssel und ein geheimes System zum Signieren der Anforderer-ID zu verwenden, führen wir das Konzept einer Software Statement-Zeichenfolge ein, die verwendet werden kann, um ein Zugriffstoken zu erhalten, das später für alle Aufrufe verwendet wird, die das SDK an unsere Server sendet. Zusätzlich zu einer Software-Anweisung müssen Sie auch einen Deep-Link für Ihre Anwendung erstellen.

Weitere Informationen finden Sie unter [Dynamische Kundenregistrierung](/help/authentication/dynamic-client-registration.md)

## Was ist eine Software-Anweisung? {#what}

Eine Software-Anweisung ist ein JWT-Token, das Informationen über Ihre Anwendung enthält. Jede Applikation sollte über eine eindeutige Software-Anweisung verfügen, die von unseren Servern verwendet wird, um die Applikation im Adobe-System zu identifizieren. Die Software-Anweisung muss übergeben werden, wenn Sie das AccessEnabler SDK initialisieren. Sie wird zur Registrierung der Anwendung bei Adobe verwendet. Nach der Registrierung erhält das SDK eine Client-ID und ein Client-Geheimnis, die zum Abrufen eines Zugriffstokens verwendet werden. Jeder Aufruf, den das SDK an unsere Server sendet, erfordert ein gültiges Zugriffstoken. Das SDK ist für die Registrierung der Anwendung, den Abruf und die Aktualisierung des Zugriffstokens zuständig.

**Hinweis:** Softwareanweisungen sind App-spezifisch und eine einzelne Software-Anweisung kann nicht für mehrere Anwendungen verwendet werden. Beachten Sie, dass dies auch für Anwendungen gilt, die Zugriff auf mehrere Kanäle bieten.

## Wie erhalte ich eine Software-Anweisung? {#how-to}

### Wenn Sie Zugriff auf das Adobe TVE-Dashboard haben:

1. Öffnen Sie den Browser und navigieren Sie zu `https://console.auth.adobe.com`.

1. Navigieren Sie zum **[!UICONTROL Channels]** und wählen Sie dann Ihren Kanal aus.

1. Navigieren Sie zum **[!UICONTROL Registered Applications]** Registerkarte.

1. Klicks **[!UICONTROL Add new application]**.

1. Geben Sie einen Namen und eine Version für Ihre Anwendung ein und wählen Sie die Plattformen aus, auf denen sie verfügbar sein wird (z. B. Android).

1. Stellen Sie eine **[!UICONTROL Domain Name]** durch Auswahl aus einer Liste von Domains, die bereits für Ihren Programmierer konfiguriert sind.

1. Übertragen Sie Ihre Änderungen auf den Server und navigieren Sie dann zurück zum Kanal **[!UICONTROL Registered Applications]** Registerkarte.

   Es sollte eine Liste mit allen registrierten Anwendungen angezeigt werden.

1. Klicks **[!UICONTROL Download]** auf der Anwendung, die Sie gerade erstellt haben.

   Möglicherweise müssen Sie einige Minuten warten, bis Ihre Software-Anweisung zum Download bereit ist.

   Eine Textdatei wird heruntergeladen. Verwenden Sie den Inhalt als Ihre Software-Anweisung.

Weitere Informationen finden Sie unter [Dynamisches Client-Registrierungs-Management](/help/authentication/dynamic-client-registration-management.md)

### Wenn Sie keinen Zugriff auf das Adobe TVE Dashboard haben:

Senden Sie ein Ticket an [tve-support@adobe.com](mailto:tve-support@adobe.com). Schließen Sie alle erforderlichen Informationen ein, einschließlich Kanal, Anwendungsname, Version und Plattformen. Jemand aus unserem Supportteam wird eine Softwareanweisung für Sie erstellen.

## Verwendung der Software-Anweisung {#use}

Nachdem Sie die Software-Anweisung erhalten haben, müssen Sie sie als Parameter im Access Enabler-Konstruktor übergeben. Adobe empfiehlt, die Softwareanweisung an einem Remote-Standort zu hosten. Auf diese Weise können Sie die Software-Anweisung einfach widerrufen und ändern, ohne eine neue Version Ihrer Anwendung veröffentlichen zu müssen.

## Verwendung der Software-Anweisung {#use-both}

In der Ressourcendatei Ihrer Anwendung `strings.xml` den folgenden Code hinzufügen:

```XML
<string name="software_statement">softwarestatement value</string>
```
