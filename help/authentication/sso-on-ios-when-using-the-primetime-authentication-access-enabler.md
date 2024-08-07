---
title: SSO in iOS bei Verwendung des Adobe Pass Authentication Access Enabler
description: SSO in iOS bei Verwendung des Adobe Pass Authentication Access Enabler
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# SSO in iOS bei Verwendung des Adobe Pass Authentication Access Enabler {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Übersicht

Single Sign-On (SSO) zwischen authentifizierungsfähigen Apps von Adobe Pass funktioniert je nach Betriebssystem auf unterschiedliche Weise.

Dieses Dokument behandelt **SSO auf iOS** bei Verwendung der Adobe Pass-Authentifizierung **Access Enabler**.

**Access Enabler** **1.10** ist die neueste Version des nativen Adobe Pass Authentication iOS SDK. Adobe empfiehlt dringend, dass Sie zu dieser Version wechseln, anstatt eine ältere Version zu verwenden. Wenn Sie eine ältere Version des Access Enabler verwenden, können Sie die neueste Version [hier](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library) herunterladen.

Die einmalige Anmeldung in iOS wird von folgenden Bedingungen bestimmt:

- Apps müssen denselben **Token-Speicher** verwenden (in Form einer benutzerdefinierten Zwischenablage, die vom Access Enabler erstellt wird).
- Apps müssen dieselbe **Geräte-ID** generieren (Access Enabler berechnet die Geräte-ID je nach Betriebssystemversion anhand der MAC-Adresse oder IDFV-Adresse).

## Verhalten

Das SSO-Verhalten sieht wie folgt aus:

- **iOS 6 und niedriger**: Die einmalige Anmeldung funktioniert automatisch zwischen Apps, die vom selben Team oder verschiedenen Teams entwickelt werden. Die Geräte-ID wird anhand der MAC-Adresse berechnet (derselbe Wert wird in allen Apps erzeugt) und der Speicherbereich ist für alle Apps gleich (benutzerdefinierte Whiteboard ist für Apps unter iOS 6 und niedriger nutzbar).
   - **Wichtig:** Bitte beachten Sie, dass die iOS SDK-Version 1.9.4 [ das Mindestangebot an iOS-Implementierungen auf iOS 7 erhöht hat.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 und höher**: SSO funktioniert unter folgenden Bedingungen:

1. Apps werden mit demselben Apple-Verteilungsprofil oder Profilen veröffentlicht, die demselben Team angehören. Dies ist die einzige Möglichkeit für Apps, benutzerdefinierte Whiteboards auf iOS 7 und höher freizugeben. In allen anderen Szenarien ist die Zwischenablage je Anwendung mit Sandbox versehen. Ab [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] und +\[`UIPasteboard pasteboardWithUniqueName`\] geben Sie jetzt den angegebenen Namen ein, damit nur die Apps in derselben Anwendungsgruppe auf die Whiteboard zugreifen können. Wenn der Entwickler versucht, eine Zwischenablage mit einem bereits vorhandenen Namen zu erstellen, der nicht zur gleichen App-Suite gehört, erhält er eine eigene eindeutige und private Zwischenablage. Beachten Sie, dass sich dies nicht auf die vom System bereitgestellten Zwischenablagen, die allgemeinen und die Suche auswirkt.

1. Apps haben dasselbe Bundle-ID-Präfix (alle Komponenten außer der letzten). Nur Anwendungen, die dasselbe Bundle-ID-Präfix verwenden, berechnen denselben IDFV. Von [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): In IOS 7 werden alle Komponenten des Bundles mit Ausnahme der letzten Komponente zum Generieren der Anbieter-ID verwendet. Wenn die Bundle-ID nur eine einzelne Komponente enthält, wird die gesamte Bundle-ID verwendet.

Konzentrieren wir uns nun auf das Szenario **&#39;iOS 7 und höher&#39;** , da es für echte Benutzer am häufigsten vorkommt:

Beide Bedingungen (Freigabe eines Profils aus demselben Entwicklungsteam und Verwendung eines gemeinsamen Bundle-Identifikations-Präfixes) sind obligatorische Bedingungen für die einmalige Anmeldung.

Im Folgenden finden Sie die möglichen Kombinationen und deren Ergebnisse:

- **Profile desselben Teams und desselben Bundle-ID-Präfixes**: Apps verwenden denselben Whiteboard-Speicher und dieselbe Geräte-ID (IDFV). Ein Benutzer muss sich nur einmal authentifizieren (in der ersten verwendeten App) und der Authentifizierungsstatus wird für alle anderen Apps freigegeben. Beispielfluss:

1. Benutzer öffnet App A (mit Paket-ID *com.x.y.AppA*) und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App A durch
1. Benutzer öffnet App B (mit Bundle-ID *com.x.y.AppB*) und wird automatisch authentifiziert, indem die Berechtigungsdaten aus der App freigegeben werden
A (aus Schritt 2)
1. Benutzer öffnet App A und ist weiterhin authentifiziert (aus Schritt 2)



- **Profile desselben Teams, aber unterschiedlicher Bundle-ID-Präfixe**: Apps verwenden denselben Whiteboard-Speicher, haben aber unterschiedliche Geräte-IDs (IDFVs). Ein Benutzer muss sich einmal pro App authentifizieren. Beispielfluss:

1. Benutzer öffnet App A (mit Paket-ID *com.x.y.AppA*) und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App A durch
1. Benutzer öffnet App B (mit Bundle-ID *com.z.AppB*) und der Access Enabler erkennt das Token, das von der ersten App abgerufen wurde (da der Speicher freigegeben ist), versucht jedoch aufgrund der verschiedenen Geräte-IDs nicht, es über SSO zu verwenden.
1. Benutzer führt Authentifizierung in App B durch
1. Benutzer öffnet App A und ist weiterhin authentifiziert (aus Schritt 2)



- **Profile aus verschiedenen Teams (die Bundle-ID ist in diesem Szenario irrelevant)**: Apps verfügen über unterschiedliche Whiteboard-Speicher und SSO wird zwischen ihnen deaktiviert. Ein Benutzer muss sich einmal pro App authentifizieren, und die Authentifizierungssitzungen bleiben beim Wechseln zwischen Apps persistent. Beispielfluss:


1. Benutzer öffnet App A und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App A durch
1. Benutzer öffnet App B und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App B durch
1. Benutzer öffnet App A und ist authentifiziert (aus Schritt 2)
1. Benutzer öffnet App B und ist authentifiziert (aus Schritt 4)

**Hinweis:** Beachten Sie, dass die oben genannten Bedingungen für die einmalige Anmeldung gelten, wenn Anwendungen über den **Apple App Store** installiert werden. Wenn die Apps auf einem Simulator bereitgestellt werden (wo App-Signaturen nicht angewendet werden), mit Xcode installiert oder über ein Ad Hoc-Profil verteilt werden, erhalten Sie möglicherweise unterschiedliche Ergebnisse.

**Wichtig:** Hinweis (**in Bezug auf AccessEnabler v1.8**): Das zweite oben beschriebene Szenario (Profile desselben Teams, aber unterschiedlicher Bundle-ID-Präfixe) schafft ein sehr unangenehmes Benutzererlebnis für Benutzer von **AccessEnabler v1.8** in Anwendungen, die vom selben Team (Medienunternehmen) entwickelt werden. Der Benutzer wird automatisch abgemeldet, wenn er zwischen Apps desselben Medienunternehmens wechselt. Daher müssen Anwendungsentwickler bei der Entscheidung über die Paket-ID und das Verteilungsprofil vorsichtig sein. Das genaue Szenario in diesem Fall wird nachfolgend beschrieben:

Apps nutzen denselben Zwischenspeicherplatz, haben aber unterschiedliche Geräte-IDs (IDFVs). Ein Benutzer muss sich einmal pro App authentifizieren, **jedoch werden die Authentifizierungssitzungen beim Wechsel zwischen Apps** gelöscht. Beispielfluss:

1. Benutzer öffnet App A (mit Paket-ID *com.x.y.AppA*) und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App A durch
1. Der Benutzer öffnet App B (mit der Bundle-ID *com.z.AppB*) und die von App A erstellten Berechtigungsdaten werden automatisch vom Access Enabler gelöscht (Sicherheitsmechanismus, der einen Konflikt zwischen der derzeit berechneten Geräte-ID in App B und der ID erkennt, die in den von App A erstellten Berechtigungstokens gespeichert ist).
1. Benutzer führt Authentifizierung in App B durch
1. Benutzer öffnet App A und die von App B erstellten Berechtigungsdaten werden automatisch vom Access Enabler gelöscht (Sicherheitsmechanismus, der einen Konflikt zwischen der derzeit berechneten Geräte-ID in App A und der ID erkennt, die in den von App B erstellten Berechtigungstoken gespeichert ist).
