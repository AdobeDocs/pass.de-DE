---
title: Anmeldung und Abmeldung ohne Aktualisierung
description: Anmeldung und Abmeldung ohne Aktualisierung
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1784'
ht-degree: 0%

---

# (Legacy) Anmeldung und Abmeldung ohne Aktualisierung {#tefresh-less-login-and-logout}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Überblick {#overview}

Bei Web-Anwendungen müssen Sie einige verschiedene mögliche Szenarien für die Authentifizierung und das Abmelden von Benutzern berücksichtigen.  Bei MVPDs müssen sich Benutzer zur Authentifizierung auf der MVPD-Webseite anmelden, wobei die folgenden zusätzlichen Faktoren eine Rolle spielen:

- Einige MVPDs erfordern eine vollständige Umleitung von Ihrer Website zu ihrer Anmeldeseite
- Bei einigen MVPDs müssen Sie einen iFrame auf Ihrer Site öffnen, um die Anmeldeseite von MVPD anzuzeigen
- Einige Browser verarbeiten das iFrame-Szenario nicht gut. Daher ist für diese Browser eine bessere Alternative, ein Popup-Fenster anstelle des iFrame zu verwenden

Vor der Adobe Pass-Authentifizierung 2.7 erforderten alle diese Szenarien zur Authentifizierung eines Benutzers eine vollständige Seitenaktualisierung der Programmiererseite. Für die Version 2.7 und die nachfolgenden Versionen verbesserte das Adobe Pass-Authentifizierungsteam diese Abläufe, sodass der Benutzer während der Anmeldung und des Abmeldens keine Seitenaktualisierung in Ihrer App erleben muss.


## Ausführliche Beschreibung {#detailed_description}

Beginnen wir mit einer Zusammenfassung der ursprünglichen Authentifizierungs- und Abmeldevorgänge und folgen ihr dann mit den verbesserten Authentifizierungs- und Abmeldevorgängen. Beachten Sie, dass sich die ersten vier Abschnitte auf normale MVPDs (non-TempPass) beziehen, während der letzte Abschnitt die spezielle Implementierung beschreibt, die für TempPass angewendet werden muss:

- [Ursprünglicher Authentifizierungsfluss](#orig_authn)
- [Ursprünglicher Abmeldefluss](#orig_logout)
- [Verbesserter Authentifizierungsfluss](#improved_authn)
- [Verbesserter Abmeldefluss](#improved_logout)
- [TempPass-Fluss](#improved_temppass)

</br>

## Ursprüngliche Authentifizierung/Abmeldevorgänge {#orig_authn}

**Authentifizierung**

Die Adobe Pass-Authentifizierungs-Webclients haben je nach den Anforderungen der MVPDs zwei Möglichkeiten zur Authentifizierung:

1. **Vollständige Seitenumleitung -** Nachdem der Benutzer einen Anbieter ausgewählt hat    (konfiguriert mit vollständiger Seitenumleitung) über die MVPD-Auswahl auf der    Die Website des Programmierers, `setSelectedProvider(<mvpd>)` auf AccessEnabler aufgerufen wird, und der Benutzer wird zur Anmeldeseite von MVPD weitergeleitet. Nachdem der Benutzer gültige Anmeldeinformationen angegeben hat, wird er zurück zur Website des Programmierers weitergeleitet. AccessEnabler wird initialisiert und das Authentifizierungstoken wird während der `setRequestor` von der Adobe Pass-Authentifizierung abgerufen.
1. **iFrame / Popup-Fenster -** Nachdem der Benutzer einen Provider ausgewählt hat (der mit iFrame konfiguriert ist), wird `setSelectedProvider(<mvpd>)` im AccessEnabler aufgerufen. Durch diese Aktion wird der `createIFrame(width, height)`-Callback Trigger und der Programmierer aufgefordert, einen iFrame (oder ein Popup - je nach Browser/Voreinstellungen) mit dem Namen `"mvpdframe"` und den angegebenen Abmessungen zu erstellen. Nachdem der iFrame/das Popup erstellt wurde, lädt AccessEnabler die Anmeldeseite von MVPD in den iFrame/das Popup. Der Benutzer gibt gültige Anmeldeinformationen an und der iFrame/das Popup wird zur Adobe Pass-Authentifizierung weitergeleitet, die ein JS-Fragment zurückgibt, das den iFrame/das Popup schließt und die übergeordnete Seite (Programmierer-Website) neu lädt. Ähnlich wie bei Fluss 1 wird das Authentifizierungstoken während der `setRequestor` abgerufen.

Der `displayProviderDialog` Callback (ausgelöst durch `getAuthentication`/`getAuthorization`) gibt eine Liste der MVPDs und ihrer entsprechenden Einstellungen zurück. Die `iFrameRequired`-Eigenschaft eines MVPD ermöglicht es dem Programmierer zu wissen, ob er Fluss 1 oder Fluss 2 aktivieren soll. Beachten Sie, dass der Programmierer nur für Fluss 2 zusätzliche Aktionen ausführen muss (Erstellen eines iFrames/Popup).

**Authentifizierung**

Es gibt auch die Situation, dass der Benutzer den Authentifizierungsfluss explizit abbricht, indem er die Anmeldeseite schließt. Im Folgenden finden Sie die Szenarien und die vorgeschlagene Lösung für die Programmierer:

1. **Vollständige Seitenumleitung -** Wenn die Anmeldeseite geschlossen ist, muss der Benutzer erneut zur Website des Programmierers navigieren und den gesamten Fluss von Anfang an starten. In diesem Szenario ist keine explizite Aktion seitens des Programmierers erforderlich.
1. **iFrame -** Es wird empfohlen, den iFrame in einem `div` (oder einer ähnlichen UI-Komponente) zu hosten, an den eine Schaltfläche Schließen angehängt ist. Wenn der Benutzer die Schaltfläche Schließen drückt, zerstört der Programmierer den iFrame zusammen mit der zugehörigen Benutzeroberfläche und führt `setSelectedProvider(null)` durch. Dieser Aufruf ermöglicht dem AccessEnabler, seinen internen Status zu löschen, und ermöglicht es dem Benutzer, einen nachfolgenden Authentifizierungsfluss zu initiieren. `setAuthenticationStatus` und `sendTrackingData(AUTHENTICATION_DETECTION...)` werden ausgelöst, um einen fehlgeschlagenen Authentifizierungsfluss zu signalisieren (sowohl auf `getAuthentication` als auch auf `getAuthorization`).
1. **Popup -** Einige Browser können das Fensterschließungsereignis nicht genau erkennen, sodass hier ein anderer Ansatz gewählt werden muss (im Gegensatz zum iFrame-Fluss oben). Adobe empfiehlt, dass der Programmierer einen Zeitgeber initialisiert, der regelmäßig überprüft, ob das Anmelde-Popup vorhanden ist. Wenn das Fenster nicht vorhanden ist, kann der Programmierer sicherstellen, dass der Benutzer den Anmeldefluss manuell abgebrochen hat, und der Programmierer kann mit dem Aufruf von `setSelectedProvider(null)` fortfahren. Die ausgelösten Callbacks sind die gleichen wie im obigen Fluss 2.

</br>

## Ursprünglicher Abmeldefluss {#orig_logout}

Die Abmelde-API von AccessEnabler löscht den lokalen Status der Bibliothek und lädt die Abmelde-URL von MVPD in die aktuelle Registerkarte/das aktuelle Fenster. Der Browser navigiert zum Abmelde-Endpunkt von MVPD und nach Abschluss des Vorgangs wird der Benutzer zurück zur Website des Programmierers weitergeleitet. Die einzige Aktion, die im Namen des Benutzers erforderlich ist, ist das Drücken der Schaltfläche/des Links Abmelden und das Initiieren des Flusses; es ist keine Benutzerinteraktion auf dem Abmelde-Endpunkt von MVPD erforderlich.

**Ursprüngliche Authentifizierung/Abmeldefluss mit Seitenaktualisierung**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## Verbesserte Authentifizierung (ohne Aktualisierung) {#improved_authn}

>[!NOTE]
>
>Die verbesserten Aktualisierungs-losen Anmelde- und Abmeldevorgänge erfordern, dass der Browser moderne HTML5-Technologien einschließlich Web-Messaging unterstützt.

Sowohl der Authentifizierungs- (Anmelde-) als auch der Abmelde-Fluss bieten ein ähnliches Benutzererlebnis, indem die Hauptseite nach Abschluss jedes Flusses neu geladen wird.  Die aktuelle Funktion zielt darauf ab, das Benutzererlebnis zu verbessern, indem sie eine aktualisierungsfreie (Hintergrund-)Anmeldung und Abmeldung ermöglicht. Der Programmierer kann die Hintergrundanmeldung und die Abmeldung aktivieren/deaktivieren, indem er zwei boolesche Flags (`backgroundLogin` und `backgroundLogout`) an den `configInfo` der `setRequestor`-API übergibt. Standardmäßig sind Anmeldung/Abmeldung im Hintergrund deaktiviert (dies sorgt für Kompatibilität mit der vorherigen Implementierung).

**Beispiel:**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**Authentifizierung**

Die folgenden Punkte beschreiben den Übergang zwischen den ursprünglichen Authentifizierungsflüssen und den verbesserten Flüssen:

1. Die ganzseitige Umleitung wird durch eine neue Browser-Registerkarte ersetzt, auf der die MVPD-Anmeldung ausgeführt wird. Der Programmierer muss eine neue Registerkarte (über `window.open`) mit dem Namen `mvpdwindow` erstellen, wenn der Benutzer eine MVPD (mit `iFrameRequired = false`) auswählt. Der Programmierer führt dann `setSelectedProvider(<mvpd>)` aus, sodass AccessEnabler die MVPD-Anmelde-URL in die neue Registerkarte laden kann. Nachdem der Benutzer gültige Anmeldeinformationen angegeben hat, schließt die Adobe Pass-Authentifizierung die Registerkarte und sendet window.postMessage an die Website des Programmierers, die dem AccessEnabler signalisiert, dass der Authentifizierungsfluss abgeschlossen ist. Die folgenden Callbacks werden ausgelöst:

   - Wenn der Fluss von `getAuthentication` initiiert wurde: `setAuthenticationStatus` und `sendTrackingData(AUTHENTICATION_DETECTION...)` werden ausgelöst, um eine erfolgreiche/nicht erfolgreiche Authentifizierung zu signalisieren.

   - Wenn der Fluss von `getAuthorization` initiiert wurde, werden `setToken/tokenRequestFailed` und `sendTrackingData(AUTHORIZATION_DETECTION...)` ausgelöst, um eine erfolgreiche/nicht erfolgreiche Autorisierung anzuzeigen.

1. Der Fluss des iFrame-/Popup-Fensters bleibt größtenteils unverändert, mit dem Unterschied, dass die übergeordnete Seite nicht neu geladen wird, nachdem der Benutzer gültige Anmeldeinformationen angegeben hat. Der iFrame/das Popup wird nach der Anmeldung automatisch geschlossen und eine `window.postMessage` wird an die übergeordnete Seite gesendet, wodurch der AccessEnabler über den Abschluss des Flusses informiert wird. Die gleichen Callbacks werden wie im vorherigen Fluss ausgelöst **plus der folgende neue Callback:`destroyIFrame`**. Der `destroyIFrame`-Callback ermöglicht es dem Programmierer, alle mit iFrame verknüpften/zusätzlichen Komponenten zu entfernen, z. B. Benutzeroberflächendekorationen. Das Vorhandensein dieses Callbacks war im alten Authentifizierungsfluss nicht erforderlich, da die Adobe Pass-Authentifizierung nach Abschluss der Anmeldung die Programmiererseite neu laden und so alle darauf befindlichen Benutzeroberflächenkomponenten zerstören würde.

</br>

>[!IMPORTANT]
> 
>Sie müssen den MVPD-Anmelde-iFrame oder das Popup-Fenster als direkt untergeordnetes Element der Seite laden, die die AccessEnabler-Instanz enthält. Wenn der MVPD-Anmelde-iFrame oder das Popup-Fenster zwei oder mehr Ebenen unterhalb der Seite mit der AccessEnabler-Instanz verschachtelt sind, kann der Fluss hängen bleiben. Wenn sich beispielsweise ein iFrame zwischen der Hauptseite und dem MVPD iFrame (Page =\> iFrame =\> MVPD iFrame) befindet, kann der Anmeldefluss fehlschlagen.

</br>

**Authentifizierung**

Dies sind die Flüsse zum Abbrechen der Authentifizierung:

1. **Browser-Registerkarte -** Da die Registerkarte im Wesentlichen ein neues Fenster ist, hat die Erfassung ihres Close-Ereignisses die gleichen Einschränkungen, die in Szenario 3 der alten Authentifizierungsflüsse erläutert wurden. Außerdem ist hier der Timer-Ansatz nicht möglich, da nicht zwischen einem manuell vom Benutzer geschlossenen Tab und einem automatisch am Ende des Login-Flusses geschlossenen Tab unterschieden werden kann. Die Lösung besteht darin, dass der AccessEnabler „stumm“ bleibt (es werden keine Callbacks ausgelöst), wenn der Benutzer den Fluss abbricht. Außerdem ist der Programmierer nicht verpflichtet, bestimmte Aktionen durchzuführen. Der Benutzer kann einen weiteren Authentifizierungsfluss initiieren, ohne den Fehler „Fehler bei mehreren Authentifizierungsanfragen“ zu erhalten (dieser Fehler wurde in AccessEnabler für die Hintergrundanmeldung deaktiviert).

1. **iFrame -** Der Programmierer kann den in Szenario 2 beschriebenen Ansatz aus den alten Authentifizierungsflüssen verwenden (Erstellen der Wrapper-Benutzeroberfläche aus dem iFrame und der zugehörigen Schaltfläche Schließen , die Trigger `setSelectedProvider(null)`. Obwohl dieser Ansatz keine zwingende Voraussetzung mehr ist (mehrere Authentifizierungsflüsse sind für die Hintergrundanmeldung zulässig, wie in Szenario 1 oben erörtert), wird er weiterhin von Adobe empfohlen.

1. **Popup -** Dieser Vorgang ist identisch mit dem Fluss auf der Registerkarte „Browser“ oben.

</br>

## Verbesserter Abmeldefluss {#improved_logout}

Der neue Abmeldefluss wird in einem ausgeblendeten iFrame ausgeführt, sodass die vollständige Seitenumleitung entfällt.  Dies ist möglich, da der/die Benutzende auf der Abmeldeseite von MVPD keine spezielle Aktion ausführen muss.

Nachdem der Abmeldefluss abgeschlossen ist, wird der iFrame an einen benutzerdefinierten Authentifizierungsendpunkt für Adobe Pass umgeleitet. Dies stellt ein JS-Snippet bereit, das eine `window.postMessage` an das übergeordnete Element durchführt und den AccessEnabler darüber informiert, dass die Abmeldung abgeschlossen ist. Die folgenden Callbacks werden ausgelöst: `setAuthenticationStatus()` und `sendTrackingData(AUTHENTICATION_DETECTION ...)`, was anzeigt, dass der Benutzer nicht mehr authentifiziert ist.

Die folgende Abbildung zeigt den Fluss ohne Aktualisierung, der es einem Benutzer ermöglicht, sich bei seiner MVPD anzumelden, ohne die Hauptseite Ihrer Anwendung zu aktualisieren:

**Verbesserter Authentifizierungs-/Abmeldefluss (ohne Aktualisierung**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass-Fluss {#improved_temppas}

Die „Refresh-Less“-Anmeldung folgt einem anderen Ansatz für MVPDs vom Typ TempPass.

Da der TempPass-Fluss erfordert, dass ein Fenster automatisch erstellt und ohne explizite Benutzerinteraktion geschlossen wird, kann er bei einigen Browsern (Popup-Blockern) ein Problem darstellen. Daher führt AccessEnabler die Anmeldephase hinter den Kulissen durch, ohne dass ein vom Programmierer erstellter Web-Container erforderlich ist.

Im Folgenden finden Sie die Aspekte, die der Programmierer beachten muss, wenn er TempPass für die Anmeldung und das Abmelden ohne Aktualisierung implementiert:

- Bevor Sie mit der Authentifizierung beginnen, muss das iFrame- oder Popup-Fenster nur für Nicht-TempPass-MVPDs erstellt werden. Der Programmierer kann erkennen, ob eine MVPD TempPass ist oder nicht, indem er die `tempPass`-Eigenschaft des MVPD-Objekts liest (zurückgegeben von `setConfig()` / `displayProviderDialog()`).

- Der `createIFrame()`-Callback muss eine Prüfung für TempPass enthalten und seine Logik nur ausführen, wenn die MVPD NICHT TempPass ist.

- Der `destroyIFrame()`-Callback muss eine Prüfung für TempPass enthalten und seine Logik nur ausführen, wenn die MVPD NICHT TempPass ist.

- Die `setAuthenticationStatus()`- und `sendTrackingData()`-Callbacks werden nach Abschluss der Authentifizierung aufgerufen (genau wie im Fluss „Refresh-less“ für normale MVPDs).

>[!NOTE]
>
>Dieser Fluss ist nur für nicht aktualisierbare TempPass verfügbar. Für den Aktualisierungsfluss muss TempPass explizit verarbeitet werden (wenn TempPass iFrame / Popup erfordert)

</br>

Das folgende Codebeispiel veranschaulicht die Handhabung eines MVPD-Fensters auf der Website eines Programmierers (sowohl für normale MVPDs als auch für TempPass):

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
