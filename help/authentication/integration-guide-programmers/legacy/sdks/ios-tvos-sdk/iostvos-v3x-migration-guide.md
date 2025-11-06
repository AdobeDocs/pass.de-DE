---
title: Migrationshandbuch für iOS/tvOS v3.x
description: Migrationshandbuch für iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---

# (Legacy) Migrationshandbuch für iOS/tvOS v3.x {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

>[!TIP]
> 
> **Hinweise:**
>
> - Ab iOS SDK Version 3.1 können Implementierungsprogramme jetzt WKWebView oder UIWebView austauschbar verwenden. Da UIWebView veraltet ist, sollten Apps zu WKWebView migriert werden, um Probleme mit zukünftigen iOS-Versionen zu vermeiden.
> - Beachten Sie, dass die Migration einfach einen Wechsel der UIWebView-Klasse mit WKWebView bedeuten würde. Es gibt keine spezielle Arbeit in Bezug auf Adobe AccessEnabler.

</br>

## Build-Einstellungen aktualisieren {#update}

Diese Version enthält Funktionen in der Sprache SWIFT. Wenn Ihre App vollständig Objective-C ist, müssen Sie das Kontrollkästchen „Swift-Standardbibliotheken immer einbetten“ in den Build-Einstellungen des Ziels auf „Ja“ setzen. Wenn diese Option festgelegt ist, scannt Xcode die gebündelten Frameworks in Ihrer App und kopiert, falls eines von ihnen Swift-Code enthält, die entsprechenden Bibliotheken in das Bundle Ihrer App. Wenn Sie die Build-Einstellungen nicht aktualisieren, stürzt Ihre App möglicherweise mit Fehlern ab, die darauf hinweisen, dass sie AccessEnabler.framework oder verschiedene `ibswift*`-Bibliotheken nicht laden kann.

</br>

## Software-Kontoauszug hinzufügen {#add}

> Informationen zum Abrufen Ihrer Software-Anweisung finden Sie hier
> Seite:
> [Anmeldung](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Sobald Sie Ihre Software-Anweisung haben, empfehlen wir, sie auf einem Remote-Server zu hosten, damit Sie sie einfach widerrufen oder ändern können, ohne eine neue Programmversion in der App Store bereitzustellen. Rufen Sie beim Start der Anwendung Ihre Softwareanweisung vom Remote-Speicherort ab und übergeben Sie sie im AccessEnabler-Konstruktor:

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API-Informationen hier: [iOS-/tvOS-API-Referenz](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Hinzufügen des benutzerdefinierten URL-Schemas {#add-custom}

> Informationen zum Abrufen eines benutzerdefinierten URL-Schemas finden Sie auf dieser Seite: [Abrufen eines Kunden-URL-Schemas](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Nachdem Sie das benutzerdefinierte URL-Schema abgerufen haben, müssen Sie es der Datei „info.plist“ des Programms hinzufügen. Das benutzerdefinierte Schema hat folgendes Format: `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. Sie müssen den Doppelpunkt und die Schrägstriche auslassen, wenn Sie ihn zur Datei hinzufügen. Das obige Beispiel wird `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

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

Dies gilt nur, wenn Ihre Anwendung zuvor die manuelle Verarbeitung des Safari View Controllers (SVC) über den [setOptions(\[„handleSVC“:true ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)&quot;\]) aktiviert hat, und für bestimmte MVPDs, für die Safari View Controller (SVC) erforderlich ist. Daher müssen die URLs der Authentifizierungs- und Abmelde-Endpunkte durch einen SFSafariViewController anstelle eines UIWebView/WKWebView Controllers geladen werden.

Während der Authentifizierungs- und Abmeldevorgänge muss die Anwendung die Aktivität des `SFSafariViewController `Controllers überwachen, während mehrere Weiterleitungen durchlaufen werden. Ihre Anwendung muss den Zeitpunkt erkennen, zu dem sie eine bestimmte benutzerdefinierte URL lädt, die von Ihrer `application's custom URL scheme` definiert wird (z. B.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. Wenn der Controller diese spezifische benutzerdefinierte URL lädt, muss die Anwendung die `SFSafariViewController` schließen und die Methode &quot;`handleExternalURL:url `&quot; von AccessEnabler aufrufen.

Fügen Sie in Ihrem `AppDelegate` die folgende Methode hinzu:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API-Informationen hier: [Handle External URL](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Aktualisieren der setRequest-Methodensignatur {#update-setreq}

Da der neue SDK einen neuen Authentifizierungsmechanismus verwendet, ist der Parameter „signedRequestId“ oder der öffentliche Schlüssel und das öffentliche Geheimnis (für tvOS) nicht erforderlich. Die `setRequestor` Methode ist vereinfacht und benötigt nur die RequestorID.

### iOS

Dieser Code:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

wird zu:

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

wird zu:

```swift
    accessEnabler.setRequestor(requestorId)
```

> API-Informationen hier: [Anforderer festlegen](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Ersetzen Sie die getAuthenticationToken-Methode durch die handleExternalURL-Methode. {#replace}

`getAuthentication` Methode wurde in der Vergangenheit zum Abschließen des Authentifizierungsflusses verwendet. Da sein Name irreführend war, wurde er in `handleExternalURL` umbenannt und verwendet die URL als Parameter.

Alle Vorkommen von diesem ändern:

```swift
    accessEnabler.getAuthenticationToken()
```

in diesen:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API-Informationen hier: [Handle External URL](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
