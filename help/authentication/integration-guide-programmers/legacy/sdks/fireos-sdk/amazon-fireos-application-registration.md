---
title: Registrierung der Amazon FireOS-Anwendung
description: Registrierung der Amazon FireOS-Anwendung
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 0%

---

# (Legacy) Registrierung der Amazon FireOS-Anwendung {#amazon-fireos-application-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>

## Einführung {#intro}

Ab Version 3.0 der FireOS AccessEnabler SDK ändern wir den Authentifizierungsmechanismus bei Adobe-Servern. Anstatt ein öffentliches Schlüssel- und Geheimsystem zum Signieren der RequestorID zu verwenden, führen wir das Konzept einer Software-Anweisungszeichenfolge ein, mit der ein Zugriffstoken abgerufen werden kann, das später für alle Aufrufe verwendet wird, die SDK an unsere Server sendet. Zusätzlich zu einer Software-Erklärung müssen Sie auch einen Deep-Link für Ihre Anwendung erstellen.

Weitere Informationen finden Sie unter [Übersicht über die dynamische Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Was ist eine Software-Anweisung? {#what}

Ein Software-Statement ist ein JWT-Token, das Informationen über Ihr Programm enthält. Jede Anwendung sollte über eine eindeutige Software-Anweisung verfügen, die von unseren Servern zur Identifizierung der Anwendung im Adobe-System verwendet wird. Die Software-Anweisung muss bei der Initialisierung des AccessEnabler SDK übergeben werden und wird zur Registrierung der Anwendung beim Adobe verwendet. Bei der Registrierung erhält die SDK eine Client-ID und ein Client-Geheimnis, die zum Abrufen eines Zugriffs-Tokens verwendet werden. Jeder Aufruf, den SDK an unsere Server sendet, erfordert ein gültiges Zugriffstoken. Die SDK ist für die Registrierung der Anwendung, den Abruf und die Aktualisierung des Zugriffstokens verantwortlich.

**Hinweis:** Software-Anweisungen sind anwendungsspezifisch und eine einzelne Software-Anweisung kann nicht für mehr als eine Anwendung verwendet werden. Beachten Sie, dass dies auch für Anwendungen gilt, die Zugriff auf mehrere Kanäle bieten.

## Wie erhalte ich ein Software-Statement? {#how-to}

### Wenn Sie Zugriff auf das Adobe-Dashboard haben:

1. Öffnen Sie Ihren Browser und navigieren Sie zu `https://experience.adobe.com/#/pass/authentication`.

1. Navigieren Sie zum Abschnitt **[!UICONTROL Channels]** und wählen Sie dann Ihren Kanal aus.

1. Navigieren Sie zur Registerkarte **[!UICONTROL Registered Applications]** .

1. Klicken Sie auf **[!UICONTROL Add new application]**.

1. Geben Sie einen Namen und eine Version für Ihr Programm an und wählen Sie die Plattformen aus, auf denen es verfügbar sein soll (z. B. Android).

1. Stellen Sie eine **[!UICONTROL Domain Name]** bereit, indem Sie aus einer Liste von Domains auswählen, die bereits für Ihren Programmierer konfiguriert sind.

1. Übertragen Sie Ihre Änderungen auf den Server und navigieren Sie dann zurück zur Registerkarte **[!UICONTROL Registered Applications]** Ihres Kanals.

   Es sollte eine Liste mit allen registrierten Anwendungen angezeigt werden.

1. Klicken Sie **[!UICONTROL Download]** auf die soeben erstellte Anwendung.

   Möglicherweise müssen Sie einige Minuten warten, bevor Ihre Software-Erklärung zum Download bereit ist.

   Eine Textdatei wird heruntergeladen. Verwenden Sie den Inhalt als Software-Erklärung.

Weitere Informationen finden Sie unter [Verwaltung der dynamischen Client-Registrierung](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Wenn Sie keinen Zugriff auf das TVE-Dashboard von Adobe haben:

Ticket an [tve-support@adobe.com](mailto:tve-support@adobe.com) senden. Fügen Sie alle erforderlichen Informationen hinzu, einschließlich Kanal, Anwendungsname, Version und Plattformen, und jemand von unserem Support-Team wird ein Software-Statement für Sie erstellen.

## Verwendung der Software-Anweisung {#use}

Nachdem Sie die Software-Anweisung erhalten haben, müssen Sie sie als Parameter im Access Enabler-Konstruktor übergeben. Adobe empfiehlt, die Software-Erklärung an einem Remote-Standort zu hosten. Auf diese Weise können Sie die Software-Erklärung einfach widerrufen und ändern, ohne eine neue Version Ihres Programms freizugeben.

## Verwendung der Software-Anweisung {#use-both}

Fügen Sie in der Ressourcendatei Ihres Programms `strings.xml` folgenden Code hinzu:

```XML
<string name="software_statement">softwarestatement value</string>
```
