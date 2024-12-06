---
title: Android-Anwendungsregistrierung
description: Android-Anwendungsregistrierung
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Android-Anwendungsregistrierung {#android-application-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#intro}

Ab Version 3.0 des Android AccessEnabler SDK ändern wir den Authentifizierungsmechanismus mit Adobe-Servern. Statt einen öffentlichen Schlüssel und ein geheimes System zum Signieren der Anforderer-ID zu verwenden, führen wir das Konzept einer Software Statement-Zeichenfolge ein, die verwendet werden kann, um ein Zugriffstoken zu erhalten, das später für alle Aufrufe verwendet wird, die das SDK an unsere Server sendet. Zusätzlich zu einer Software-Anweisung müssen Sie auch einen Deep-Link für Ihre Anwendung erstellen.

Weitere Informationen finden Sie unter [Übersicht über die dynamische Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Was ist eine Software-Anweisung? {#what}

Eine Software-Anweisung ist ein JWT-Token, das Informationen über Ihre Anwendung enthält. Jede Anwendung sollte über eine eindeutige Softwareanweisung verfügen, die von unseren Servern verwendet wird, um die Anwendung im Adobe-System zu identifizieren.

Die Software-Anweisung muss übergeben werden, wenn Sie das `AccessEnabler`-SDK initialisieren. Es wird verwendet, um die Anwendung bei Adobe zu registrieren. Bei der Registrierung erhält das SDK eine Client-ID und ein Client-Geheimnis, mit dem ein Zugriffstoken abgerufen wird. Jeder Aufruf, den das SDK an Adobe-Server durchführt, erfordert ein gültiges Zugriffstoken. Das SDK ist für die Registrierung der Anwendung, den Abruf und die Aktualisierung des Zugriffstokens zuständig.

>[!NOTE]
>
>Softwareanweisungen sind App-spezifisch und eine einzelne Software-Anweisung kann nicht für mehrere Anwendungen verwendet werden. Bitte beachten Sie, dass Softwareanweisungen auf Programmebene die gleichen Einschränkungen aufweisen, sie können nur für eine einzelne Anwendung verwendet werden, ob für einzelne Kanäle oder für mehrere Kanäle.

## So erhalten Sie eine Software-Anweisung {#how-to-get-ss}

Hier finden Sie Möglichkeiten, eine Software-Anweisung zu erhalten.

### Wenn Sie Zugriff auf das Adobe TVE Dashboard haben

1. Öffnen Sie Ihren Browser und navigieren Sie zu [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

1. Navigieren Sie zum Abschnitt **[!UICONTROL Channels]** und wählen Sie dann Ihren Kanal aus.

1. Navigieren Sie zur Registerkarte **[!UICONTROL Registered Applications]** .

1. Klicken Sie auf **[!UICONTROL Add new application]**.

1. Benennen Sie die Anwendung und geben Sie eine Version an.

1. Wählen Sie die Plattformen aus, auf denen die Anwendung verfügbar sein soll (in diesem Fall Android).

1. Geben Sie einen &quot;**[!UICONTROL Domain Name]**&quot;-Wert an, indem Sie aus einer Liste von Domänen auswählen, die bereits für Ihren Programmierer konfiguriert sind.

1. Übertragen Sie Ihre Änderungen auf den Server und navigieren Sie dann zurück zur Registerkarte **[!UICONTROL Registered Applications]** Ihres Kanals.

   Es sollte eine Liste mit allen registrierten Anwendungen angezeigt werden. Wählen Sie **[!UICONTROL Download]** in der von Ihnen erstellten Anwendung aus. Möglicherweise müssen Sie einige Minuten warten, bis Ihre Software-Anweisung für den Download bereit ist.

   Eine Textdatei wird heruntergeladen. Verwenden Sie den Inhalt als Ihre Software-Anweisung.

Weitere Informationen finden Sie unter [Dynamisches Client-Registrierungs-Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Wenn Sie keinen Zugriff auf das Adobe TVE Dashboard haben

Senden Sie ein Ticket an `tve-support@adobe.com`. Schließen Sie die erforderlichen Informationen wie Kanal, Anwendungsname, Version und Plattformen ein. Jemand aus unserem Support-Team wird eine Softwareanweisung für Sie erstellen.

## Verwendung der Software-Anweisung {#how-to-use-ss}

Nachdem Sie die Software-Anweisung erhalten haben, müssen Sie sie als Parameter im Access Enabler-Konstruktor übergeben. Wir empfehlen, die Software-Anweisung an einem Remote-Standort zu hosten. Auf diese Weise können Sie die Software-Anweisung einfach widerrufen und ändern, ohne eine neue Version Ihrer Anwendung veröffentlichen zu müssen.

## Erstellen und Verwenden eines Deep-Links für Ihre Anwendung {#create}

Verwenden Sie in Android als Deep-Link-Wert die Rückseite des Domänennamens, der beim Erstellen der Software-Anweisung ausgewählt wurde.

Der erstellte Deep-Link sollte auf dem Android-Gerät einen eindeutigen Wert aufweisen. Wenn mehrere Anwendungen denselben Deep-Link-Wert verwenden, stören die Authentifizierungs- und Abmeldevorgänge.

## Verwendung der Software-Anweisung und des Deep-Links {#use-both}

Fügen Sie in der Ressourcendatei `strings.xml` Ihrer Anwendung den folgenden Code hinzu:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
