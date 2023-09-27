---
title: iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden
description: iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Problem {#issue}

Der Benutzer durchläuft den Authentifizierungsfluss. Nachdem er seine Anmeldeinformationen mit seinem Provider erfolgreich eingegeben hat, wird er entweder zu einer Fehlerseite, einer Suchseite oder einer anderen benutzerdefinierten Seite zurückgeleitet, auf der er darüber informiert wird, dass `adobepass.ios.app` konnte nicht gefunden/gelöst werden.

## Erklärung {#explanation}

IOS: `adobepass.ios.app` wird als endgültige Umleitungs-URL verwendet, um anzugeben, dass der AuthN-Fluss abgeschlossen ist. An dieser Stelle muss die App eine Anfrage an den AccessEnabler senden, um das AuthN-Token abzurufen und den AuthN-Fluss abzuschließen.

Das Problem ist, dass `adobepass.ios.app` nicht vorhanden ist und eine Fehlermeldung in der `webView`. In älteren Versionen der iOS DemoApp wurde davon ausgegangen, dass dieser Fehler immer am Ende des AuthN-Flusses ausgelöst wird. Er wurde entsprechend eingerichtet (`indidFailLoadWithError`).

**Hinweis:** Dieses Problem wurde in späteren Versionen der DemoApp behoben (im iOS SDK-Download enthalten).

Leider ist diese Annahme nicht richtig. Es gibt einige so genannte &quot;intelligente&quot; DNS- oder Proxy-Server, die den aufgeworfenen Fehler nicht einfach weitergeben, sondern stattdessen eine der folgenden Aktionen durchführen:

- Benutzerdefinierte Fehlerseite erstellen
- Wechseln Sie zu einer Suchseite oder einem anderen Typ von Kundenseite oder Portal.

In diesen Fällen ist die Antwort, die an die iOS-WebView zurückgesendet wird, für die webView eine absolut gültige Antwort und Trigger NICHT den Fehler, von dem die alte DemoApp abhängig war.

## Lösung {#solution}

Nehmen Sie NICHT die gleiche Annahme wie die DemoApp vor. Erfassen Sie stattdessen die Anfrage, bevor sie ausgeführt wird (in `shouldStartLoadWithRequest`) und behandeln Sie sie entsprechend.

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

- NIE verwenden `adobepass.ios.app` direkt an eine beliebige Stelle im Code. Verwenden Sie stattdessen die Konstante . `ADOBEPASS_REDIRECT_URL`
- Die `return NO;` -Anweisung verhindert das Laden der Seite
- Stellen Sie sicher, dass die Variable `getAuthenticationToken` -Aufruf wird einmal und nur einmal in Ihrem Code aufgerufen. Mehrere Aufrufe an `getAuthenticationToken` führt zu nicht definierten Ergebnissen.
