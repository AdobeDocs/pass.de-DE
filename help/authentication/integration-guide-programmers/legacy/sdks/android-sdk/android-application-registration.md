---
title: Registrierung der Android-Anwendung
description: Registrierung der Android-Anwendung
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 0%

---

# Registrierung von (veralteten) Android-Anwendungen {#android-application-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung {#intro}

Ab Version 3.0 von Android AccessEnabler SDK ändern wir den Authentifizierungsmechanismus für die Server von Adobe. Anstatt ein öffentliches Schlüssel- und Geheimsystem zum Signieren der RequestorID zu verwenden, führen wir das Konzept einer Software-Anweisungszeichenfolge ein, mit der ein Zugriffstoken abgerufen werden kann, das später für alle Aufrufe verwendet wird, die SDK an unsere Server sendet. Zusätzlich zu einem Software-Statement müssen Sie auch einen Deep-Link für Ihre Applikation erstellen.

Weitere Informationen finden Sie unter [Übersicht über die dynamische Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Was ist eine Software-Anweisung? {#what}

Ein Software-Statement ist ein JWT-Token, das Informationen über Ihr Programm enthält. Jede Anwendung sollte über eine eindeutige Software-Anweisung verfügen, die von unseren Servern verwendet wird, um die Anwendung im System von Adobe zu identifizieren.

Die Software-Anweisung muss übergeben werden, wenn Sie die `AccessEnabler` SDK initialisieren. Sie wird verwendet, um die Anwendung bei Adobe zu registrieren. Bei der Registrierung erhält die SDK eine Client-ID und ein Client-Geheimnis, mit denen ein Zugriffstoken abgerufen wird. Jeder Aufruf von SDK an Adobe-Server erfordert ein gültiges Zugriffstoken. Die SDK ist für die Registrierung der Anwendung, den Abruf und die Aktualisierung des Zugriffstokens verantwortlich.

>[!NOTE]
>
>Software-Anweisungen sind anwendungsspezifisch und eine einzelne Software-Anweisung kann nicht für mehr als ein Programm verwendet werden. Beachten Sie, dass Anweisungen auf Programmiererebene dieselbe Einschränkung haben. Sie können nur für eine einzige Anwendung verwendet werden, unabhängig davon, ob es sich um einen einzelnen Kanal oder mehrere Kanäle handelt.

## So erhalten Sie eine Software-Anweisung {#how-to-get-ss}

Im Folgenden finden Sie Möglichkeiten, wie Sie eine Software-Erklärung erhalten können.

### Bei Zugriff auf das TVE-Dashboard von Adobe

1. Öffnen Sie Ihren Browser und navigieren Sie zum [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

1. Navigieren Sie zu **[!UICONTROL Channels]** Abschnitt und wählen Sie dann Ihren Kanal aus.

1. Navigieren Sie zur Registerkarte **[!UICONTROL Registered Applications]** .

1. Klicken Sie auf **[!UICONTROL Add new application]**.

1. Benennen Sie das Programm und geben Sie eine Version an.

1. Wählen Sie die Plattformen aus, auf denen die Anwendung verfügbar sein soll (in diesem Fall Android).

1. Stellen Sie eine **[!UICONTROL Domain Name]** bereit, indem Sie aus einer Liste von Domains auswählen, die bereits für Ihren Programmierer konfiguriert sind.

1. Übertragen Sie Ihre Änderungen auf den Server und navigieren Sie dann zurück zur Registerkarte **[!UICONTROL Registered Applications]** Ihres Kanals.

   Es sollte eine Liste mit allen registrierten Anwendungen angezeigt werden. Wählen Sie **[!UICONTROL Download]** für das von Ihnen erstellte Programm aus. Möglicherweise müssen Sie einige Minuten warten, bevor Ihre Software-Erklärung zum Download bereit ist.

   Eine Textdatei wird heruntergeladen. Verwenden Sie den Inhalt als Software-Erklärung.

Weitere Informationen finden Sie unter [Verwaltung der dynamischen Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Wenn Sie keinen Zugriff auf das TVE-Dashboard von Adobe haben

Senden Sie ein Ticket an `tve-support@adobe.com`. Schließen Sie die erforderlichen Informationen wie Kanal, Anwendungsname, Version und Plattformen ein. Jemand aus unserem Support-Team erstellt ein Software-Statement für Sie.

## Verwendung der Software-Anweisung {#how-to-use-ss}

Nachdem Sie die Software-Anweisung erhalten haben, müssen Sie sie als Parameter im Access Enabler-Konstruktor übergeben. Wir empfehlen, die Software-Erklärung an einem Remote-Standort zu hosten. Auf diese Weise können Sie die Software-Erklärung einfach widerrufen und ändern, ohne eine neue Version Ihres Programms freizugeben.

## Erstellen und Verwenden eines Deep-Links für Ihr Programm {#create}

Verwenden Sie in Android als Deep-Link-Wert die Rückseite des Domain-Namens, der beim Erstellen der Software-Anweisung ausgewählt wurde

Der erstellte Deep-Link sollte auf dem Android-Gerät einen eindeutigen Wert aufweisen. Wenn mehrere Anwendungen denselben Deep-Link-Wert verwenden, werden die Authentifizierungs- und Abmeldeflüsse beeinträchtigt.

## Verwendung der Software-Anweisung und des Deep-Links {#use-both}

Fügen Sie in der Ressourcendatei Ihres Programms `strings.xml` folgenden Code hinzu:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
