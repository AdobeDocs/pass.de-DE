---
title: WKWebView-Unterstützung für iOS SDK 3.1+
description: WKWebView-Unterstützung für iOS SDK 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# WKWebView-Unterstützung für iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

**Da Apple UIWebView auf iOS eingestellt hat, wurde das iOS SDK 3.1 mit Unterstützung für WKWebView aktualisiert.**

## Kompatibilität {#compatibility}

Ab iOS SDK-Version 3.1 können Implementoren jetzt WKWebView oder UIWebView synonym verwenden. Da UIWebView von Apple nicht mehr unterstützt wird, sollten Apps zu WKWebView migrieren, um Probleme mit zukünftigen iOS-Versionen zu vermeiden.

Beachten Sie, dass die Migration lediglich bedeutet, dass die UIWebView-Klasse mit WKWebView gewechselt wird. Es gibt keine spezielle Arbeit in Bezug auf Adobe AccessEnabler.

## Bekannte Probleme {#known-issues}

Adobe AccessEnabler verwendete eine ausgeblendete interne UIWebView-Instanz, um für bestimmte MVPDs eine &quot;[passive Authentifizierung](/help/authentication/sso-passive-authn.md)&quot; durchzuführen. Der &quot;passive&quot;Fluss war für MVPDs nützlich, die Authentifizierung für jede Anfragende-ID erfordern. Von diesem Fluss profitierten jene Programmierer, die dieselbe Team-ID über mehrere iOS-Anwendungen hinweg nutzten, um ein SSO-Erlebnis (Adobe SSO) zu simulieren. Diese Funktion wird derzeit von einer begrenzten Anzahl von MVPDs verwendet.

Die Funktion nutzte ein UIWebView-Verhalten, das es Adobe ermöglichte, die Authentifizierungs-Cookies zu erfassen und sie während des &quot;passiven&quot;Flusses erneut abzuspielen. WKWebView bietet eine höhere Sicherheit, die verhindert, dass Adobe die bei der Anmeldung eingestellten Cookies erfasst und mithilfe einer verborgenen Instanz von WKWebView erneut abspielt. Aufgrund dieser Sicherheitsverbesserung und angesichts der Tatsache, dass der &quot;passive&quot;Fluss nur einer sehr begrenzten Anzahl von MVPDs in einem sehr spezifischen Implementierungsszenario zugute kam (mehrere Anwendungen mit derselben Team-ID), entfernte Adobe die &quot;passive Authentifizierungsfunktion&quot;für MVPDs, die zur Authentifizierung Webansichten verwenden.

Die Funktion ist weiterhin für MVPDs vorhanden, die für die Verwendung von SFSafariViewController konfiguriert sind. Beachten Sie jedoch, dass in diesem Fall die &quot;passive&quot;Authentifizierung für den Benutzer sichtbar ist, da SFSafariViewController nicht auf &quot;verborgene&quot;Weise verwendet werden kann.
