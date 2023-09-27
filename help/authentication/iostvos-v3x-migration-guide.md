---
title: Migrationshandbuch für iOS/tvOS v3.x
description: Migrationshandbuch für iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# Migrationshandbuch für iOS/tvOS v3.x {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!TIP]
> 
> **Hinweise:**
>
> - Ab iOS SDK-Version 3.1 können Implementoren jetzt WKWebView oder UIWebView austauschbar verwenden. Da UIWebView veraltet ist, sollten Apps zu WKWebView migrieren, um Probleme mit zukünftigen iOS-Versionen zu vermeiden.
> - Beachten Sie, dass die Migration lediglich bedeutet, dass die UIWebView-Klasse mit WKWebView gewechselt wird. Es gibt keine spezielle Arbeit in Bezug auf Adobe AccessEnabler.

</br>

## Build-Einstellungen aktualisieren {#update}

Diese Version enthält Funktionen, die in der SWIFT-Sprache geschrieben wurden. Wenn Ihre App vollständig Objective-C ist, müssen Sie das Kontrollkästchen &quot;Swift-Standardbibliotheken immer einbetten&quot;in den Build-Einstellungen Ihres Ziels auf &quot;Ja&quot;setzen. Wenn diese Option festgelegt ist, scannt Xcode die gebündelten Frameworks in Ihrer App und kopiert, wenn eines davon Swift-Code enthält, die entsprechenden Bibliotheken in das Bundle Ihrer App. Wenn Sie die Build-Einstellungen nicht aktualisieren, stürzt Ihre App möglicherweise mit Fehlern ab, die darauf hinweisen, dass sie AccessEnabler.framework oder verschiedene `ibswift*` -Bibliotheken.

</br>

## Hinzufügen Ihrer Softwareanweisung {#add}

> Informationen zum Abrufen Ihrer Softwareanweisung finden Sie hier .
> Seite:
> [Anwendungsregistrierung](/help/authentication/iostvos-application-registration.md)

Sobald Sie über Ihre Softwareanweisung verfügen, sollten Sie sie auf einem Remote-Server hosten, damit Sie sie einfach widerrufen oder ändern können, ohne eine neue Anwendungsversion in der App Store bereitzustellen. Wenn die Anwendung gestartet wird, rufen Sie Ihre Softwareanweisung vom Remote-Standort ab und übergeben Sie sie im AccessEnabler-Konstruktor:

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API-Informationen hier: [API-Referenz zu iOS/tvOS](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Hinzufügen des benutzerdefinierten URL-Schemas {#add-custom}

> Informationen zum Abrufen eines benutzerdefinierten URL-Schemas finden Sie auf dieser Seite: [Abrufen eines Kunden-URL-Schemas](/help/authentication/iostvos-application-registration.md)

Nachdem Sie das benutzerdefinierte URL-Schema abgerufen haben, müssen Sie es zur Datei info.plist der Anwendung hinzufügen. Das benutzerdefinierte Schema hat folgendes Format: `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. Sie müssen den Doppelpunkt und die Schrägstriche beim Hinzufügen zur Datei weglassen. Das obige Beispiel wird `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## Abfangen von Aufrufen im benutzerdefinierten URL-Schema {#intercept}

Dies gilt nur, wenn Ihre Anwendung zuvor die manuelle Verarbeitung des Safari View Controller (SVC) über das [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/iostvos-sdk-api-reference.md) Rufen Sie und für bestimmte MVPDs auf, die den Safari View Controller (SVC) erfordern, sodass das Laden der URLs der Authentifizierungs- und Abmelde-Endpunkte von einem SFSafariViewController-Controller anstelle eines UIWebView-/WKWebView-Controllers erforderlich ist.

Während der Authentifizierung und des Abmeldevorgangs muss Ihre Anwendung die Aktivität der `SFSafariViewController `Controller, während er mehrere Umleitungen durchläuft. Ihre Anwendung muss den Moment erkennen, in dem sie eine bestimmte benutzerdefinierte URL lädt, die von Ihrer `application's custom URL scheme` (z. B.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss Ihre Anwendung die `SFSafariViewController` und rufen Sie AccessEnabler `handleExternalURL:url `API-Methode.

In der `AppDelegate` die folgende Methode hinzufügen:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API-Informationen hier: [Umgang mit externen URLs](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Aktualisieren der Signatur der setRequestor-Methode {#update-setreq}

Da das neue SDK einen neuen Authentifizierungsmechanismus verwendet, ist weder der Parameter signedRequestId noch der öffentliche Schlüssel und das Geheimnis (für tvOS) erforderlich. Die `setRequestor` -Methode vereinfacht und benötigt nur die Anforderer-ID.

### iOS

Dieser Code:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

wird:

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

Dieser Code:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

wird:

```swift
    accessEnabler.setRequestor(requestorId)
```

> API-Informationen hier: [Anforderer festlegen](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Ersetzen der getAuthenticationToken-Methode durch die handleExternalURL-Methode {#replace}

`getAuthentication` -Methode wurde in der Vergangenheit zum Abschließen des Authentifizierungsflusses verwendet. Da der Name irreführend war, wurde er in `handleExternalURL` und nimmt die URL als Parameter.

Ändern Sie alle Vorkommen:

```swift
    accessEnabler.getAuthenticationToken()
```

wie folgt:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API-Informationen hier: [Umgang mit externen URLs](/help/authentication/iostvos-sdk-api-reference.md)
