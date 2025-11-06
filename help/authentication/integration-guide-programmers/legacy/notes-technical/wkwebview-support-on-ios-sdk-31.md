---
title: WKWebView-Unterstützung in iOS SDK 3.1+
description: WKWebView-Unterstützung in iOS SDK 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# (Legacy) WKWebView-Unterstützung in iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>

**Da Apple UIWebView auf iOS einstellt, haben wir iOS SDK 3.1 aktualisiert und unterstützen nun WKWebView.**

## Kompatibilität {#compatibility}

Ab iOS SDK Version 3.1 können Implementierungsprogramme jetzt WKWebView oder UIWebView austauschbar verwenden. Da UIWebView von Apple nicht mehr unterstützt wird, sollten Apps zu WKWebView migriert werden, um Probleme mit zukünftigen iOS-Versionen zu vermeiden.

Beachten Sie, dass die Migration einfach einen Wechsel der UIWebView-Klasse mit WKWebView bedeuten würde. Es gibt keine spezielle Arbeit in Bezug auf Adobe AccessEnabler.

## Bekannte Probleme {#known-issues}

AccessEnabler von Adobe verwendete eine verborgene interne UIWebView-Instanz, um für [ MVPDs eine „passive ](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)&quot; durchzuführen. Der „passive“ Fluss war für MVPDs nützlich, für die eine Authentifizierung für jede Anforderer-ID erforderlich war. Von diesem Fluss profitierten Programmierer, die dieselbe Team-ID in mehreren iOS-Anwendungen verwendeten, um ein SSO-Erlebnis (Adobe SSO) zu simulieren. Diese Funktion wird derzeit von einer begrenzten Anzahl von MVPDs verwendet.

Die Funktion verwendete ein Verhalten der UIWebView, das es Adobe ermöglichte, die Authentifizierungs-Cookies zu erfassen und sie während des „passiven“ Flusses wiederzugeben. WKWebView führt eine höhere Sicherheit ein, die verhindert, dass Adobe die bei der Anmeldung gesetzten Cookies erfasst und über eine ausgeblendete Instanz von WKWebView erneut abspielt. Aufgrund dieser Sicherheitsverbesserung und in Anbetracht der Tatsache, dass der „passive“ Fluss nur einer sehr begrenzten Anzahl von MVPDs in einem sehr spezifischen Implementierungsszenario (mehrere Anwendungen mit derselben Team-ID) zugutekam, entfernte Adobe die Funktion „passive Authentifizierung“ für MVPDs, die Webansichten zur Authentifizierung verwendeten.

Die Funktion ist weiterhin für MVPDs vorhanden, die für die Verwendung von SFSafariViewController konfiguriert sind. Beachten Sie jedoch, dass in diesem Fall die „passive“ Authentifizierung für den Benutzer sichtbar ist, da SFSafariViewController nicht auf eine „versteckte“ Weise verwendet werden kann.
