---
title: iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden
description: iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Problem {#issue}

Der Benutzer durchläuft den Authentifizierungsfluss. Nachdem er seine Anmeldeinformationen erfolgreich bei seinem Provider eingegeben hat, wird er entweder zu einer Fehlerseite, einer Suchseite oder einer anderen benutzerdefinierten Seite zurückgeleitet, auf der er darüber informiert wird, dass `adobepass.ios.app` nicht gefunden/aufgelöst werden konnte.

## Erklärung {#explanation}

In iOS wird `adobepass.ios.app` als endgültige Umleitungs-URL verwendet, um anzugeben, dass der AuthN-Fluss abgeschlossen ist. An dieser Stelle muss die App eine Anfrage an den AccessEnabler senden, um das AuthN-Token abzurufen und den AuthN-Fluss abzuschließen.

Das Problem besteht darin, dass `adobepass.ios.app` nicht tatsächlich vorhanden ist und eine Fehlermeldung in der `webView` Trigger. In älteren Versionen der iOS DemoApp wurde davon ausgegangen, dass dieser Fehler immer am Ende des AuthN-Flusses ausgelöst wird. Er wurde so eingerichtet, dass er entsprechend verarbeitet wird (`indidFailLoadWithError`).

**Hinweis:** Dieses Problem wurde in späteren Versionen der DemoApp behoben (im iOS SDK-Download enthalten).

Leider ist diese Annahme nicht richtig. Es gibt einige so genannte &quot;intelligente&quot; DNS- oder Proxy-Server, die den aufgeworfenen Fehler nicht einfach weitergeben, sondern stattdessen eine der folgenden Aktionen durchführen:

- Benutzerdefinierte Fehlerseite erstellen
- Wechseln Sie zu einer Suchseite oder einem anderen Typ von Kundenseite oder Portal.

In diesen Fällen ist die Antwort, die an die iOS-WebView zurückgesendet wird, für die webView eine absolut gültige Antwort und Trigger NICHT den Fehler, von dem die alte DemoApp abhängig war.

## Lösung {#solution}

Nehmen Sie NICHT die gleiche Annahme wie die DemoApp vor. Erfassen Sie stattdessen die Anfrage, bevor sie ausgeführt wird (in `shouldStartLoadWithRequest`), und behandeln Sie sie entsprechend.

Beispiel für das Abfangen der Anfrage vor der Ausführung:

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

- Verwenden Sie niemals `adobepass.ios.app` direkt an einer beliebigen Stelle im Code. Verwenden Sie stattdessen die Konstante `ADOBEPASS_REDIRECT_URL`
- Die Anweisung `return NO;` verhindert, dass die Seite geladen wird.
- Stellen Sie sicher, dass der `getAuthenticationToken` -Aufruf einmal und nur einmal in Ihrem Code aufgerufen wird. Mehrere Aufrufe von `getAuthenticationToken` führen zu nicht definierten Ergebnissen.
