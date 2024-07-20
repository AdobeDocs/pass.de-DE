---
title: Anmeldung ohne Aktualisierung und Abmeldung
description: Anmeldung ohne Aktualisierung und Abmeldung
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1761'
ht-degree: 0%

---

# Anmeldung ohne Aktualisierung und Abmeldung {#tefresh-less-login-and-logout}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Bei Webanwendungen müssen Sie verschiedene mögliche Szenarien für die Authentifizierung und das Abmelden von Benutzern berücksichtigen.  MVPDs erfordern, dass sich Benutzer auf der Webseite des MVPD anmelden, um sich zu authentifizieren, wobei die folgenden zusätzlichen Faktoren ins Spiel kommen:

- Einige MVPDs erfordern eine vollständige Umleitung von Ihrer Site zur Anmeldeseite
- Bei einigen MVPDs müssen Sie einen iFrame auf Ihrer Site öffnen, um die Anmeldeseite des MVPD anzuzeigen.
- Einige Browser handhaben das iFrame-Szenario nicht gut. Für diese Browser ist es daher besser, ein Popup-Fenster anstelle des iFrame zu verwenden.

Vor der Adobe Pass-Authentifizierung 2.7 umfassten alle diese Szenarien zum Authentifizieren eines Benutzers eine vollständige Seitenaktualisierung der Seite des Programmierers. Für 2.7 und die folgenden Versionen verbesserte das Adobe Pass-Authentifizierungsteam diese Flüsse, sodass der Benutzer während des Anmelden- und Abmeldevorgangs keine Seitenaktualisierung in Ihrer App erleben muss.


## Detaillierte Beschreibung {#detailed_description}

Beginnen wir mit einer Zusammenfassung der ursprünglichen Authentifizierungs- und Abmeldevorgänge und folgen Sie dann den verbesserten Authentifizierungs- und Abmeldeabläufen. Beachten Sie, dass die ersten vier Abschnitte normale MVPDs (non-TempPass) behandeln, während der letzte Abschnitt die spezielle Implementierung beschreibt, die für TempPass angewendet werden muss:

- [Ursprünglicher Authentifizierungsfluss](#orig_authn)
- [Ursprünglicher Abmeldefluss](#orig_logout)
- [Verbesserter Authentifizierungsfluss](#improved_authn)
- [Verbesserter Abmeldefluss](#improved_logout)
- [TempPass-Fluss](#improved_temppass)

</br>

## Ursprüngliche Authentifizierungs-/Abmeldevorgänge {#orig_authn}

**Authentifizierung**

Die Adobe Pass-Authentifizierungs-Webclients bieten je nach den Anforderungen von MVPDs zwei Möglichkeiten zur Authentifizierung:

1. **Umleitung auf vollständige Seite -** Nachdem der Benutzer einen Anbieter ausgewählt hat    (mit Vollseitenumleitung konfiguriert) aus der MVPD-Auswahl auf der    Die Website des Programmierers, `setSelectedProvider(<mvpd>)` wird auf dem AccessEnabler aufgerufen und der Benutzer wird zur Anmeldeseite des MVPD weitergeleitet. Nachdem der Benutzer gültige Anmeldeinformationen angegeben hat, wird er auf die Website des Programmierers zurückgeleitet. Der AccessEnabler wird initialisiert und das Authentifizierungstoken wird während `setRequestor` aus der Adobe Pass-Authentifizierung abgerufen.
1. **iFrame/Popup-Fenster -** Nachdem der Benutzer einen (mit iFrame konfigurierten) Provider ausgewählt hat, wird `setSelectedProvider(<mvpd>)` auf dem AccessEnabler aufgerufen. Diese Aktion Trigger den Rückruf `createIFrame(width, height)` und benachrichtigt den Programmierer, einen iFrame (oder ein Popup - je nach Browser/Voreinstellungen) mit dem Namen `"mvpdframe"` und den bereitgestellten Dimensionen zu erstellen. Nachdem der iFrame/Popup erstellt wurde, lädt der AccessEnabler die Anmeldeseite des MVPD in den iFrame/Popup. Der Benutzer stellt gültige Anmeldeinformationen bereit und das iFrame/Popup wird zur Adobe Pass-Authentifizierung weitergeleitet, die ein JS-Snippet zurückgibt, das den iFrame/Popup schließt und die übergeordnete Seite (Programmierer-Website) neu lädt. Ähnlich wie bei Fluss 1 wird das Authentifizierungstoken während `setRequestor` abgerufen.

Der Rückruf `displayProviderDialog` (ausgelöst durch `getAuthentication`/`getAuthorization`) gibt eine Liste der MVPDs und der entsprechenden Einstellungen zurück. Mit der Eigenschaft `iFrameRequired` eines MVPD kann der Programmierer wissen, ob Fluss 1 oder Fluss 2 aktiviert werden soll. Beachten Sie, dass der Programmierer nur für Fluss 2 zusätzliche Aktionen (Erstellen eines iFrame/Popup) ausführen muss.

**Authentifizierung abbrechen**

Es gibt auch die Situation, in der der Benutzer den Authentifizierungsfluss explizit abbricht, indem er die Anmeldeseite schließt. Im Folgenden finden Sie die Szenarien und die vorgeschlagene Lösung für die Programmierer:

1. **Umleitung auf der vollständigen Seite -** Wenn die Anmeldeseite geschlossen ist, muss der Benutzer erneut zur Website des Programmierers navigieren und den gesamten Fluss von Anfang an initiieren. In diesem Szenario ist keine explizite Aktion auf der Seite des Programmierers erforderlich.
1. **iFrame -** Der Programmierer wird empfohlen, den iFrame in einer `div` -Komponente (oder einer ähnlichen UI-Komponente) zu hosten, an die eine Schließen -Schaltfläche angehängt ist. Wenn der Benutzer die Schaltfläche Schließen drückt, zerstört der Programmierer den iFrame zusammen mit der zugehörigen Benutzeroberfläche und führt `setSelectedProvider(null)` aus. Dieser Aufruf ermöglicht es dem AccessEnabler, seinen internen Status zu löschen, und ermöglicht es dem Benutzer, einen nachfolgenden Authentifizierungsfluss zu starten. `setAuthenticationStatus` und `sendTrackingData(AUTHENTICATION_DETECTION...)` werden ausgelöst, um einen fehlgeschlagenen Authentifizierungsfluss zu signalisieren (sowohl bei `getAuthentication` als auch bei `getAuthorization`).
1. **Popup -** Einige Browser können das Schließen des Fensters nicht genau erkennen. Daher muss hier ein anderer Ansatz gewählt werden (im Gegensatz zum obigen iFrame-Fluss). Adobe empfiehlt, dass der Programmierer einen Timer initialisiert, der regelmäßig überprüft, ob das Anmelde-Popup vorhanden ist. Wenn das Fenster nicht vorhanden ist, kann der Programmierer sicherstellen, dass der Benutzer den Anmeldefluss manuell abgebrochen hat und der Programmierer mit dem Aufruf von `setSelectedProvider(null)` fortfahren kann. Die ausgelösten Rückrufe sind mit denen im obigen Fluss 2 identisch.

</br>

## Ursprünglicher Abmeldefluss {#orig_logout}

Die Abmelde-API des AccessEnabler löscht den lokalen Status der Bibliothek und lädt die Abmelde-URL des MVPD in die aktuelle Registerkarte/das aktuelle Fenster. Der Browser navigiert zum Abmelde-Endpunkt des MVPD und nach Abschluss des Vorgangs wird der Benutzer zurück zur Website des Programmierers weitergeleitet. Die einzige Aktion, die im Namen des Benutzers erforderlich ist, besteht darin, die Schaltfläche/den Link zum Abmelden zu drücken und den Fluss zu initiieren. Für den Abmelde-Endpunkt des MVPD ist keine Benutzerinteraktion erforderlich.

**Ursprünglicher Authentifizierungs-/Abmeldefluss mit Seitenaktualisierung**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## Verbesserte Authentifizierung (ohne Aktualisierung) {#improved_authn}

>[!NOTE]
>
>Aufgrund der verbesserten Aktualisierungs- und Abmeldevorgänge müssen moderne HTML5-Technologien einschließlich Webnachrichten vom Browser unterstützt werden.

Sowohl die oben erläuterten Authentifizierungs- (Anmelde-) als auch die Abmelde-Flüsse bieten ein ähnliches Benutzererlebnis, indem die Hauptseite nach jedem Durchlauf neu geladen wird.  Die aktuelle Funktion soll das Benutzererlebnis verbessern, indem eine erneute (Hintergrund-)Anmeldung und Abmeldung bereitgestellt werden. Der Programmierer kann die Hintergrundanmeldung und -abmeldung aktivieren/deaktivieren, indem er zwei boolesche Flags (`backgroundLogin` und `backgroundLogout`) an den Parameter `configInfo` der API `setRequestor` übergibt. Standardmäßig sind die Hintergrundanmeldung/-abmeldung deaktiviert (dies bietet Kompatibilität mit der vorherigen Implementierung).

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

1. Die ganzseitige Umleitung wird durch eine neue Browser-Registerkarte ersetzt, in der die MVPD-Anmeldung durchgeführt wird. Der Programmierer muss eine neue Registerkarte (über `window.open`) mit dem Namen `mvpdwindow` erstellen, wenn der Benutzer einen MVPD auswählt (mit `iFrameRequired = false`). Der Programmierer führt dann `setSelectedProvider(<mvpd>)` aus, sodass der AccessEnabler die MVPD-Anmelde-URL in die neue Registerkarte laden kann. Nachdem der Benutzer gültige Anmeldeinformationen angegeben hat, schließt die Adobe Pass-Authentifizierung die Registerkarte und sendet eine window.postMessage an die Website des Programmierers, die dem AccessEnabler signalisiert, dass der Authentifizierungsfluss abgeschlossen ist. Die folgenden Rückrufe werden ausgelöst:

   - Wenn der Fluss durch `getAuthentication` initiiert wurde: `setAuthenticationStatus` und `sendTrackingData(AUTHENTICATION_DETECTION...)` werden ausgelöst, um die erfolgreiche/nicht erfolgreiche Authentifizierung zu signalisieren.

   - Wenn der Fluss durch `getAuthorization` initiiert wurde: `setToken/tokenRequestFailed` und `sendTrackingData(AUTHORIZATION_DETECTION...)` werden ausgelöst, um eine erfolgreiche/nicht erfolgreiche Autorisierung zu signalisieren.

1. Der iFrame-/Popup-Fensterfluss bleibt größtenteils unverändert, wobei der Unterschied darin besteht, dass die übergeordnete Seite nicht neu geladen wird, nachdem der Benutzer gültige Anmeldeinformationen angegeben hat. Der iFrame/Popup wird nach der Anmeldung automatisch geschlossen und ein `window.postMessage` wird an die übergeordnete Seite gesendet. Dadurch wird der AccessEnabler informiert, dass der Fluss abgeschlossen ist. Dieselben Rückrufe werden wie im vorherigen Fluss ausgelöst, **plus der folgende neue Rückruf:`destroyIFrame`**. Mit dem Rückruf `destroyIFrame` kann der Programmierer alle mit iFrame verbundenen/zusätzlichen Komponenten entfernen, z. B. Benutzeroberflächen-Dekorationen. Im alten Authentifizierungsfluss war das Vorhandensein dieses Rückrufs nicht erforderlich, da die Adobe Pass-Authentifizierung nach Abschluss der Anmeldung die Seite des Programmierers neu laden und damit alle darin enthaltenen Benutzeroberflächen-Komponenten zerstören würde.

</br>

>[!IMPORTANT]
> 
>Sie müssen den MVPD-Anmelde-iFrame oder das Popup-Fenster als direktes untergeordnetes Element der Seite laden, die die AccessEnabler-Instanz enthält. Wenn der MVPD-Anmelde-iFrame oder das Popup-Fenster zwei oder mehr Ebenen unter der Seite verschachtelt ist, die die AccessEnabler-Instanz enthält, kann der Fluss hängen. Wenn sich beispielsweise ein iFrame zwischen der Hauptseite und dem MVPD-iFrame (Seite =\> iFrame =\> MVPD-iFrame) befindet, kann der Anmeldefluss fehlschlagen.

</br>

**Authentifizierung abbrechen**

Dies sind die Flüsse zum Abbrechen der Authentifizierung:

1. **Registerkarte &quot;Browser&quot;-** Da es sich bei der Registerkarte im Wesentlichen um ein neues Fenster handelt, hat die Erfassung des zugehörigen Schließen-Ereignisses dieselben Einschränkungen wie in Szenario 3 aus den alten Authentifizierungsflüssen beschrieben. Darüber hinaus ist der Timer-Ansatz hier nicht möglich, da es nicht möglich ist, zwischen einem Tabulator zu unterscheiden, der manuell vom Benutzer geschlossen wurde, und einem Tab, der am Ende des Anmeldeflusses automatisch geschlossen wurde. Die Lösung besteht darin, dass der AccessEnabler &quot;still&quot;bleibt (keine Rückrufe werden ausgelöst), wenn der Benutzer den Fluss abbricht. Außerdem ist der Programmierer nicht verpflichtet, spezifische Maßnahmen zu ergreifen. Der Benutzer kann einen weiteren Authentifizierungsfluss starten, ohne den Fehler &quot;Fehler bei Mehrfachauthentifizierungsanforderungen&quot;zu erhalten (dieser Fehler wurde im AccessEnabler für die Hintergrundanmeldung deaktiviert).

1. **iFrame -** Der Programmierer kann den in Szenario 2 erläuterten Ansatz aus den alten Authentifizierungsflüssen verwenden (Wrapper-Benutzeroberfläche aus dem iFrame und der zugehörigen Schließen-Schaltfläche erstellen, die Trigger `setSelectedProvider(null)` ist. Dieser Ansatz ist zwar nicht mehr unbedingt erforderlich (mehrere Authentifizierungsflüsse sind für die Hintergrundanmeldung zulässig, wie in Szenario 1 erläutert), wird jedoch dennoch von Adobe empfohlen.

1. **Popup -** Dies entspricht dem obigen Fluss der Browser-Registerkarte.

</br>

## Verbesserter Abmeldefluss {#improved_logout}

Der neue Abmeldefluss wird in einem ausgeblendeten iFrame ausgeführt, wodurch die vollständige Seitenumleitung entfällt.  Dies ist möglich, da der Benutzer auf der MVPD-Abmeldeseite keine bestimmte Aktion ausführen muss.

Nach Abschluss des Abmeldevorgangs wird der iFrame an einen benutzerdefinierten Adobe Pass-Authentifizierungsendpunkt umgeleitet. Dadurch wird ein JS-Snippet bereitgestellt, das dem übergeordneten Element einen `window.postMessage` ausführt und dem AccessEnabler mitteilt, dass die Abmeldung abgeschlossen ist. Die folgenden Rückrufe werden ausgelöst: `setAuthenticationStatus()` und `sendTrackingData(AUTHENTICATION_DETECTION ...)`, was bedeutet, dass der Benutzer nicht mehr authentifiziert ist.

Die folgende Abbildung zeigt den Fluss &quot;Refresh-less&quot;, mit dem sich ein Benutzer bei seinem MVPD anmelden kann, ohne die Hauptseite Ihrer Anwendung zu aktualisieren:

**Verbesserter (nicht aktualisierbarer) Authentifizierungs-/Abmeldefluss**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass-Fluss {#improved_temppas}

Die Anmeldung ohne Aktualisierung folgt einem anderen Ansatz für MVPDs des Typs TempPass.

Da der TempPass-Fluss erfordert, dass ein Fenster automatisch erstellt und ohne explizite Benutzerinteraktion geschlossen wird, kann es bei einigen Browsern (Popup-Blockern) ein Problem darstellen. Daher implementiert der AccessEnabler die Login-Phase hinter den Kulissen, ohne dass ein vom Programmierer erstellter Webcontainer erforderlich ist.

Im Folgenden finden Sie die Aspekte, die der Programmierer bei der Implementierung von TempPass für die erneute Anmeldung und Abmeldung beachten muss:

- Bevor die Authentifizierung gestartet wird, muss das iFrame- oder Popup-Fenster nur für Nicht-TempPass-MVPDs erstellt werden. Der Programmierer kann erkennen, ob ein MVPD TempPass ist oder nicht, indem er die Eigenschaft `tempPass` des MVPD-Objekts liest (zurückgegeben von `setConfig()` / `displayProviderDialog()`).

- Der Rückruf `createIFrame()` muss eine Prüfung für TempPass enthalten und seine Logik nur ausführen, wenn der MVPD NICHT TempPass ist.

- Der Rückruf `destroyIFrame()` muss eine Prüfung für TempPass enthalten und seine Logik nur ausführen, wenn der MVPD NICHT TempPass ist.

- Die Rückrufe `setAuthenticationStatus()` und `sendTrackingData()` werden nach Abschluss der Authentifizierung aufgerufen (genau wie im Fluss Refresh-less für normale MVPDs).

>[!NOTE]
>
>Dieser Fluss ist nur für den refresh-less-TempPass verfügbar. Für den Aktualisierungsfluss muss TempPass explizit gehandhabt werden (wenn TempPass iFrame/Popup erfordert)

</br>

Das folgende Codebeispiel zeigt, wie ein MVPD-Fenster auf der Website eines Programmierers (sowohl für normale MVPDs als auch für TempPass) verarbeitet wird:

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
