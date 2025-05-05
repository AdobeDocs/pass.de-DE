---
title: iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden
description: iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---

# (Legacy) iOS-Authentifizierungsfehler - adobepass.ios.app wurde nicht gefunden {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Problem {#issue}

Der Benutzer durchläuft den Authentifizierungsfluss und wird nach der erfolgreichen Eingabe der Anmeldeinformationen bei seinem Anbieter entweder zurück zu einer Fehlerseite, zu einer Suchseite oder zu einer anderen benutzerdefinierten Seite weitergeleitet, wo er darüber informiert wird, dass `adobepass.ios.app` nicht gefunden/aufgelöst werden konnte.

## Erklärung {#explanation}

In iOS wird `adobepass.ios.app` als endgültige Umleitungs-URL verwendet, um anzugeben, dass der AuthN-Fluss abgeschlossen ist. An dieser Stelle muss die App eine Anfrage an AccessEnabler stellen, um das AuthN-Token abzurufen und den AuthN-Fluss abzuschließen.

Das Problem besteht darin, dass `adobepass.ios.app` nicht wirklich existiert und eine Fehlermeldung im `webView` Trigger. In älteren Versionen der iOS DemoApp wurde davon ausgegangen, dass dieser Fehler immer am Ende des AuthN-Flusses ausgelöst wird, und für die entsprechende Behandlung eingerichtet (`indidFailLoadWithError`).

**Hinweis:** Dieses Problem wurde in späteren Versionen der DemoApp behoben (im iOS SDK-Download enthalten).

Leider ist diese Annahme NICHT korrekt. Es gibt einige so genannte „intelligente“ DNS- oder Proxy-Server, die den ausgelösten Fehler nicht einfach weitergeben, sondern stattdessen einen der folgenden Schritte ausführen:

- Erstellen einer benutzerdefinierten Fehlerseite
- Weiterleiten an eine Suchseite oder einen anderen Typ von Kundenseite oder Portal.

In diesen Fällen ist die Antwort, die an die iOS-WebView zurückgegeben wird, eine vollkommen gültige Antwort, soweit die WebView betroffen ist, und führt NICHT zum Trigger des Fehlers, von dem die alte DemoApp abhängig war.

## Lösung {#solution}

Machen Sie NICHT die gleiche Annahme, die die DemoApp macht. Stattdessen sollten Sie die Anfrage abfangen, bevor Sie ausgeführt wird (in `shouldStartLoadWithRequest`), und sie entsprechend verarbeiten.

Beispiel für das Abfangen der Anforderung vor ihrer Ausführung:

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

Beachten Sie Folgendes:

- Verwenden Sie `adobepass.ios.app` NIE direkt an einer Stelle im Code. Verwenden Sie stattdessen die konstante `ADOBEPASS_REDIRECT_URL`
- Die `return NO;`-Anweisung verhindert das Laden der Seite
- Stellen Sie sicher, dass der `getAuthenticationToken` Aufruf nur einmal im Code aufgerufen wird. Mehrere Aufrufe von `getAuthenticationToken` führen zu undefinierten Ergebnissen.
