---
title: Debugging des AccessEnabler iOS/tvOS-SDK mithilfe von Konsolen-App-Protokollen
description: Debugging des AccessEnabler iOS/tvOS-SDK mithilfe von Konsolen-App-Protokollen
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# Debugging des AccessEnabler iOS/tvOS-SDK mithilfe von Konsolen-App-Protokollen {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Übersicht

In diesem Dokument wird die Entwicklung des iOS/tvOS SDK-Protokollierungsmechanismus von AccessEnabler zusammen mit einigen nützlichen Details zum Debugging des AccessEnabler-Frameworks mithilfe der Konsolen-App-Protokolle erfasst und dargestellt.

## Status des Protokollierungsmechanismus

Der Protokollierungsmechanismus von AccessEnabler iOS/tvOS dient dazu, nützliche Meldungen zur Fehlerbehebung bei möglichen Problemen zu senden, auf die eine Anwendung mit dem AccessEnabler-Framework stoßen könnte.

### AccessEnabler iOS/tvOS 3.5.0 und höher

Ab AccessEnabler iOS/tvOS 3.5.0 werden mit dem Protokollierungsmechanismus die folgenden Verbesserungen als Änderungen eingeführt:

* AccessEnabler-Framework verwendet die empfohlene Implementierung von Apple [OSLog](https://developer.apple.com/documentation/os/oslog).

* AccessEnabler Framework bietet die Möglichkeit, Konsolen-App-Protokolle nach Subsystem zu filtern: **com.adobe.pass.AccessEnabler**. Alle vom SDK ausgegebenen Nachrichten sind Teil von com.adobe.pass.AccessEnabler.

* AccessEnabler Framework ermöglicht das Filtern von Konsolen-App-Protokollen basierend auf Any (Präfix): **[AccessEnabler]**. Alle vom SDK ausgegebenen Nachrichten erhalten das Präfix [AccessEnabler].

* Das AccessEnabler-Framework bietet die Möglichkeit, Konsolen-App-Protokolle nach Kategorie: **debug**, **error** in Verbindung mit einem der beiden oben genannten Kriterien zu filtern: Subsystem oder Any (Präfix).

## Debugging mithilfe von Konsolen-App-Protokollen

Je nach den untersuchten Problemen können Sie die vom AccessEnabler-Framework ausgegebenen Protokollmeldungen ein- oder ausschließen. Daher finden Sie unten einige nützliche Details, die Ihnen bei Untersuchungen und bei der Verwendung von Konsolen-App-Protokollen helfen können.


### AccessEnabler iOS/tvOS 3.5.0 und höher

#### Einschließlich {#including}

Um eine der vom AccessEnabler-Framework ausgegebenen Protokollmeldungen sehen zu können, müssen Sie zunächst **1} im Aktionsabschnitt der Konsole-App die Optionen &quot;Info-Nachrichten einschließen&quot;und &quot;Debug-Nachrichten einschließen&quot;auswählen, wie in der Abbildung unten dargestellt.**

![](../assets/include-info-debug-msg.png)


Um die Funktionen des AccessEnabler iOS/tvOS SDK und **siehe** des AccessEnabler-Frameworks debuggen zu können, können Sie:

* Suchen Sie in der Konsolen-App mit der Option **Subsystem** , die dem Wert com.adobe.pass.AccessEnabler entspricht, wie in der Abbildung unten dargestellt.

![](../assets/subsys-console-app.png)

* Suchen Sie in der Konsolen-App mit der Option **Any** , die die
  [AccessEnabler] -Wert wie in der Abbildung unten dargestellt.

![](../assets/any-optn-console-app.png)

Neben den beiden oben genannten Kriterien können Sie auch die Option **Kategorie** in Verbindung mit **Subsystem** oder **Any (Präfix)** verwenden, um explizit nach den vom AccessEnabler iOS/tvOS-SDK ausgegebenen **Debug**- oder **Fehler**-Nachrichten zu suchen.

#### Ausschließen

Um die Funktionalität anderer Komponenten besser debuggen und **** vom AccessEnabler-Framework ausschließen zu können, können Sie Folgendes tun:

* Suchen Sie in der Konsolen-App mit der Option **Subsystem** , die nicht mit dem Wert com.adobe.pass.AccessEnabler übereinstimmt.
* Suchen Sie in der Konsolen-App mit der Option **Any** , die den Wert [AccessEnabler] nicht enthält.

## Melden eines Problems

Beachten Sie beim Melden eines Problems bei der Adobe Pass-Authentifizierung die folgenden Vorschläge:

* Bitte versuchen Sie, die Reproduktionsschritte anzugeben.
* Bitte versuchen Sie, die Betriebssystemversion(en) und das Gerätemodell/die Geräte anzugeben, bei denen das Problem auftritt.
* Bitte versuchen Sie, die Version des AccessEnabler iOS/tvOS SDK anzugeben, in der das Problem auftritt.
* Versuchen Sie, alle Logging-Meldungen des AccessEnabler iOS/tvOS SDK mit einer der beiden Optionen zu erfassen und anzuhängen, die im Abschnitt [Einschließen](#including) beschrieben sind.
