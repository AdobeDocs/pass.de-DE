---
title: SSO auf iOS bei Verwendung von Adobe Pass Authentication Access Enabler
description: SSO auf iOS bei Verwendung von Adobe Pass Authentication Access Enabler
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%

---

# (Legacy) SSO auf iOS bei Verwendung von Adobe Pass Authentication Access Enabler {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>

## Überblick

Single Sign-On (SSO) zwischen Apps mit Adobe Pass-Authentifizierung funktioniert je nach Betriebssystem auf unterschiedliche Weise.

In diesem Dokument wird **SSO auf iOS** bei Verwendung des Adobe Pass Authentication **Access Enabler** behandelt.

**Access Enabler** **1.10** ist die neueste Version der nativen Adobe Pass Authentication iOS SDK. Adobe empfiehlt dringend, zu dieser Version zu wechseln, anstatt bei einer älteren Version zu bleiben. Wenn Sie eine ältere Version von Access Enabler verwenden, können Sie die neueste Version ([) ](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

SSO auf iOS wird durch die folgenden Bedingungen bestimmt:

- Apps müssen denselben **Token-Speicher** verwenden (in Form einer benutzerdefinierten Zwischenablage, die vom Access Enabler erstellt wird).
- Apps müssen dieselbe **Geräte-ID** generieren (Access Enabler berechnet die Geräte-ID basierend auf der MAC-Adresse oder der IDFV, je nach Betriebssystemversion).

## Verhalten

Das SSO-Verhalten sieht wie folgt aus:

- **iOS 6 und niedriger**: SSO funktioniert automatisch zwischen Apps, die von demselben Team oder verschiedenen Teams entwickelt wurden. Die Geräte-ID wird anhand der MAC-Adresse berechnet (in allen Apps wird derselbe Wert erzeugt), und der Speicherbereich ist in allen Apps gleich (benutzerdefinierte Pinnwand ist in allen Apps ab iOS 6 verfügbar).
   - **Wichtig:** Beachten Sie, dass mit der iOS SDK-Version 1.9.4 [das Mindestziel für die iOS-Bereitstellung auf iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library) erhöht wurde
- **iOS 7 und höher**: SSO funktioniert unter folgenden Bedingungen:

1. Apps werden mit demselben Apple-Verteilungsprofil veröffentlicht oder mit Profilen, die zum selben Team gehören. Dies ist die einzige Möglichkeit für Apps, benutzerdefinierte Zwischenablagen in iOS 7 und höher freizugeben. In allen anderen Szenarien wird die Pinnwand pro Anwendung in Sandboxes eingefügt. Von [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] und +\[`UIPasteboard pasteboardWithUniqueName`\] ist der angegebene Name jetzt eindeutig, damit nur die Apps in derselben Programmgruppe auf die Zwischenablage zugreifen können. Wenn Entwicklerinnen und Entwickler versuchen, eine Zwischenablage mit einem bereits vorhandenen Namen zu erstellen, und sie nicht Teil derselben App-Suite sind, erhalten sie eine eigene eindeutige und private Zwischenablage. Beachten Sie, dass dies keine Auswirkungen auf das vom System bereitgestellte Zwischenablage, Allgemein und Suchen hat.

1. Apps haben dasselbe Bundle-ID-Präfix (alle Komponenten außer dem letzten). Nur Anwendungen, die dasselbe Paket-ID-Präfix verwenden, berechnen dieselbe ID. Von [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): Auf IOS 7 werden alle Komponenten des Bundles mit Ausnahme der letzten Komponente zum Generieren der Anbieter-ID verwendet. Wenn die Bundle-ID nur eine einzige Komponente hat, wird die gesamte Bundle-ID verwendet.

Konzentrieren wir uns jetzt auf das Szenario **iOS 7 und höher** da es für echte Benutzer am häufigsten auftritt:

Beide Bedingungen (gemeinsame Nutzung eines Profils desselben Entwicklungsteams und Verwendung eines gemeinsamen Bundle-ID-Präfixes) sind obligatorische Bedingungen für SSO.

Im Folgenden finden Sie die möglichen Kombinationen und die resultierenden Ergebnisse:

- **Profile aus demselben Team und demselben Bundle-ID-Präfix**: Apps verwenden denselben Pinnwand-Speicher und dieselbe Geräte-ID (IDFV). Ein Benutzer muss sich nur einmal authentifizieren (in der ersten verwendeten App) und der Authentifizierungsstatus wird für alle anderen Apps freigegeben. Beispielfluss:

1. Benutzer öffnet App A (mit Paket-ID *com.x.y.AppA*) und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App A durch
1. Benutzer öffnet App B (mit Bundle-ID *com.x.y.AppB*) und wird automatisch authentifiziert, indem die Berechtigungsdaten aus der App freigegeben werden
A (aus Schritt 2)
1. Benutzer öffnet App A und ist weiterhin authentifiziert (ab Schritt 2)



- **Profile desselben Teams, aber unterschiedlicher Bundle-ID-Präfixe**: Apps verwenden denselben Pinnwand-Speicher, aber unterschiedliche Geräte-IDs (IDFVs). Ein Benutzer muss sich einmal pro App authentifizieren. Beispielfluss:

1. Benutzer öffnet App A (mit Paket-ID *com.x.y.AppA*) und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App A durch
1. Der Benutzer öffnet App B (mit Bundle-ID *com.z.AppB*) und Access Enabler erkennt das von der ersten App erhaltene Token (da der Speicher freigegeben wird), versucht jedoch aufgrund der unterschiedlichen Geräte-IDs nicht, es über SSO zu verwenden
1. Benutzer führt Authentifizierung in App B durch
1. Benutzer öffnet App A und ist weiterhin authentifiziert (ab Schritt 2)



- **Profile aus verschiedenen Teams (Paket-ID ist in diesem Szenario irrelevant)**: Apps haben unterschiedliche Pinnwand-Speicher und SSO wird zwischen ihnen deaktiviert. Ein Benutzer muss sich einmal pro App authentifizieren und die Authentifizierungssitzungen bleiben beim Wechsel zwischen Apps bestehen. Beispielfluss:


1. Benutzer öffnet App A und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App A durch
1. Benutzer öffnet App B und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App B durch
1. Benutzer öffnet App A und wird authentifiziert (ab Schritt 2)
1. Benutzer öffnet App B und wird authentifiziert (ab Schritt 4)

**Hinweis:** Beachten Sie, dass die oben genannten Bedingungen für SSO bei der Installation von Anwendungen über die **Apple App Store** gelten. Wenn die Apps auf einem Simulator bereitgestellt werden (für den das App-Signieren nicht gilt), mit Xcode installiert oder über ein Ad-hoc-Profil verteilt werden, können unterschiedliche Ergebnisse erzielt werden.

**Wichtig:** Hinweis (**bezüglich AccessEnabler v1.8**): Das zweite oben beschriebene Szenario (Profile desselben Teams, aber verschiedene Bundle-ID-Präfixe) führt zu einem sehr unangenehmen Benutzererlebnis für Benutzer von **AccessEnabler v1.8** in allen Anwendungen, die vom selben Team (Medienunternehmen) entwickelt werden. Der Benutzer wird automatisch abgemeldet, wenn er zwischen Apps desselben Medienunternehmens wechselt. Daher müssen Anwendungsentwickler bei der Entscheidung über die Paket-ID und das Verteilungsprofil vorsichtig sein. Das genaue Szenario in diesem Fall wird unten dargestellt:

Apps teilen sich den gleichen Pinnwand-Speicher, haben jedoch unterschiedliche Geräte-IDs (IDFVs). Ein Benutzer muss sich einmal pro Programm authentifizieren (**die Authentifizierungssitzungen werden beim Wechsel zwischen Programmen gelöscht**. Beispielfluss:

1. Benutzer öffnet App A (mit Paket-ID *com.x.y.AppA*) und ist nicht authentifiziert
1. Benutzer führt Authentifizierung in App A durch
1. Der Benutzer öffnet App B (mit Bundle-ID *com.z.AppB*) und die von App A erstellten Berechtigungsdaten werden automatisch vom Access Enabler gelöscht (Sicherheitsmechanismus, der einen Konflikt zwischen der aktuell berechneten Geräte-ID in App B und der in den Berechtigungs-Token gespeicherten ID erkennt - erstellt von App A)
1. Benutzer führt Authentifizierung in App B durch
1. Der Benutzer öffnet App A und die von App B erstellten Berechtigungsdaten werden automatisch vom Access Enabler gelöscht (Sicherheitsmechanismus, der einen Konflikt zwischen der aktuell berechneten Geräte-ID in App A und der in den von App B erstellten Berechtigungs-Token gespeicherten ID erkennt)
