---
title: Temporärer Übergang
description: Temporärer Übergang
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# Temporärer Übergang {#temp-pass}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Funktionszusammenfassung {#tempass-featur-summary}

Mit dem Temp Pass können Programmierer temporären Zugriff auf ihren geschützten Inhalt anbieten, für Benutzer, die keine Kontoanmeldeinformationen mit einem MVPD haben.  Der temporäre Pass umfasst die folgenden Funktionen:

* Der Temp Pass kann konfiguriert werden, um temporären Zugriff für eine Vielzahl von Szenarien bereitzustellen, darunter die folgenden:
   * Ein Programmierer kann eine tägliche, kurze Vorschau (z.B. eine 10-minütige Vorschau) einer seiner Sites anbieten.
   * Ein Programmierer kann eine einzelne, lange Präsentation (z. B. vier Stunden) einer großen Sportereignis wie den Olympischen Spielen oder der NCAA-Marschmadness anbieten.
   * Ein Programmierer kann eine Kombination der beiden vorherigen Szenarien bereitstellen, z. B. einen anfänglichen längeren Anzeigezeitraum eines Tages, gefolgt von einer Reihe kurzer Zeiträume, die für einige weitere Tage täglich wiederholt werden.
* Programmierer geben die Dauer (Time-To-Live oder TTL) ihres Temp-Passes an.
* Der Temp Pass funktioniert pro Anfrage.  Beispielsweise könnte NBC einen 4-Stunden-Temp-Pass für den Anfragenden &quot;NBCOlympics&quot;einrichten.
* Programmierer können alle Token zurücksetzen, die einem bestimmten Anfragenden gewährt werden.  Der &quot;temporäre MVPD&quot;, der zur Implementierung des Temp Pass verwendet wird, muss mit aktivierter Option &quot;Authentifizierung pro Anforderer&quot;konfiguriert werden.
* **Der temporäre Pass-Zugriff wird einzelnen Benutzern auf bestimmten Geräten gewährt**. Nachdem der Zugriff auf den temporären Pass für einen Benutzer abläuft, kann dieser Benutzer keinen temporären Zugriff auf dasselbe Gerät erhalten, bis das abgelaufene [Autorisierungstoken](/help/authentication/kickstart/glossary.md#authz-token) dieses Benutzers vom Adobe Pass-Authentifizierungsserver gelöscht wird.


>[!NOTE]
>
>Der Temp Pass ist Teil des Premium Workflow-Pakets. Wenden Sie sich an Ihren Adobe Pass-Vertriebsmitarbeiter, wenn Sie diese Funktion nutzen möchten.

## Funktionsdetails {#tempass-featur-details}

* **Berechnung der Anzeigezeit** - Die Dauer, die ein temporärer Pass gültig bleibt, korreliert nicht mit der Zeit, die ein Benutzer mit der Anzeige von Inhalten in der Anwendung des Programmierers verbringt.  Bei der ersten Benutzeranfrage zur Autorisierung über den Temp Pass wird eine Ablaufzeit berechnet, indem die anfängliche aktuelle Anforderungszeit der vom Programmierer angegebenen TTL hinzugefügt wird. Diese Ablaufzeit ist mit der Geräte-ID des Benutzers und der Anforderer-ID des Programmierers verknüpft und in der Adobe Pass-Authentifizierungsdatenbank gespeichert. Jedes Mal, wenn der Benutzer versucht, mithilfe von Temp Pass vom selben Gerät aus auf Inhalte zuzugreifen, vergleicht die Adobe Pass-Authentifizierung die Server-Anforderungszeit mit der Ablaufzeit, die mit der Geräte-ID des Benutzers und der Anforderer-ID des Programmierers verknüpft ist. Wenn die Anforderungszeit des Servers kleiner als die Ablaufzeit ist, wird die Autorisierung erteilt. Andernfalls wird die Autorisierung verweigert.
* **Konfigurationsparameter** - Ein Programmierer kann die folgenden Parameter für den Temp-Pass angeben, um eine Temp-Pass-Regel zu erstellen:
   * **Token TTL** - Die Zeit, die ein Benutzer sehen darf, ohne sich bei einem MVPD anzumelden. Diese Zeit basiert auf der Uhr und läuft ab, unabhängig davon, ob der Benutzer Inhalte anschaut oder nicht.
  >[!NOTE]
  >Eine Anfrage-ID kann nicht mit mehr als einer Temp Pass-Regel verknüpft sein.
* **Authentifizierung/Autorisierung** - Geben Sie im Fluss &quot;Temp Pass&quot;den MVPD als &quot;Temp Pass&quot;an.  Die Adobe Pass-Authentifizierung kommuniziert nicht mit einem tatsächlichen MVPD im Temp Pass-Fluss, sodass der MVPD &quot;Temp Pass&quot; jede Ressource autorisiert. Programmierer können eine Ressource angeben, auf die über den Temp Pass zugegriffen werden kann, genau wie für die anderen Ressourcen auf ihrer Website. Die Media Verifier-Bibliothek kann wie gewohnt verwendet werden, um das Token für Kurzmedien vom Typ &quot;Temp Pass&quot;zu überprüfen und die Ressourcenüberprüfung vor der Wiedergabe zu erzwingen.
* **Tracking von Daten im Temp-Pass-Fluss** - Zwei Punkte in Bezug auf Tracking-Daten während eines Berechtigungsflusses für den temporären Pass:
   * Die Tracking-ID, die von der Adobe Pass-Authentifizierung an den Rückruf **sendTrackingData()** übergeben wird, ist ein Hash der Geräte-ID.
   * Da die im Temp Pass-Fluss verwendete MVPD-ID &quot;Temp Pass&quot;lautet, wird dieselbe MVPD-ID an **sendTrackingData()** zurückgegeben. Die meisten Programmierer werden Temp Pass-Metriken wahrscheinlich anders behandeln als tatsächliche MVPD-Metriken. Dies erfordert zusätzliche Arbeit in Ihrer Analytics-Implementierung.

Die folgende Abbildung zeigt den Fluss &quot;Temp Pass&quot;:

![Der Temp-Pass-Fluss](../../../assets/temp-pass-flow.png)

*Abbildung: Fluss des Temp-Durchlaufs*

## Implementieren des Vorlagenübergangs {#implement-tempass}

Auf der Adobe Pass-Authentifizierungsseite wird der Temp Pass mit dem Zusatz eines Pseudo-MVPD namens &quot;TempPass&quot; zur Serverkonfiguration des teilnehmenden Programmierers implementiert.  Dieses Pseudo-MVPD agiert wie ein tatsächlicher MVPD, der temporär Zugriff auf den geschützten Inhalt des Programmierers gewährt.

Auf der Programmiererseite wird der Temp Pass für die beiden Szenarien, die MVPDs für die Authentifizierung verwenden, wie folgt implementiert:

* **iFrame auf der Seite des Programmierers**. Der Temp Pass funktioniert unabhängig vom Authentifizierungstyp eines MVPD. Für das iFrame-Szenario sind jedoch zusätzliche Schritte erforderlich, um den aktuellen Authentifizierungsfluss abzubrechen und sich mit dem Temp Pass zu authentifizieren. Diese Schritte werden unten in der [iFrame-Anmeldung](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) angezeigt.
* **Weiterleiten zur MVPD-Anmeldeseite**. Im herkömmlichen Fall, dass die Benutzeroberfläche zum Auslösen des Temp Pass vor dem Starten der Authentifizierung mit einem MVPD angezeigt wird, sind keine besonderen Schritte erforderlich. Temp Pass sollte wie ein normaler MVPD behandelt werden.

Die folgenden Punkte gelten für beide Implementierungsszenarien:

* Der &quot;Temp Pass&quot; sollte nur für Benutzer, die noch keine Temp Pass-Autorisierung angefordert haben, in der MVPD-Auswahl angezeigt werden. Das Blockieren der Anzeige für nachfolgende Anforderungen kann erreicht werden, indem eine Markierung auf Cookies beibehalten wird. Dies funktioniert so lange, wie der Benutzer den Browser-Cache nicht löscht. Wenn Benutzer ihre Browser-Caches löschen, wird &quot;Temp Pass&quot;erneut in der Auswahl angezeigt und der Benutzer kann es erneut anfordern. Der Zugriff wird nur gewährt, wenn die Zeit des &quot;Temp Pass&quot; noch nicht abgelaufen ist.
* Wenn ein Benutzer den Zugriff über den Temp Pass anfordert, führt der Adobe Pass-Authentifizierungsserver während des Authentifizierungsprozesses nicht die übliche SAML-Anfrage (Security Assertion Markup Language) aus. Stattdessen gibt der Authentifizierungsendpunkt jedes Mal, wenn er aufgerufen wird, Erfolg zurück, während Token für das Gerät gültig sind.
* Wenn ein Temp-Pass abläuft, wird der Benutzer nicht mehr authentifiziert, da im Fluss &quot;Temp Pass&quot;das Authentifizierungstoken und das Autorisierungstoken dasselbe Ablaufdatum haben. Um Benutzern zu erklären, dass ihr Temp-Pass abgelaufen ist, müssen Programmierer das ausgewählte MVPD direkt nach dem Aufruf von `setRequestor()` abrufen und dann `checkAuthentication()` wie gewohnt aufrufen. Im Rückruf `setAuthenticationStatus()` kann geprüft werden, ob der Authentifizierungsstatus 0 ist. Wenn der ausgewählte MVPD also &quot;TempPass&quot;lautete, kann Benutzern eine Nachricht angezeigt werden, dass ihre Temp Pass-Sitzung abgelaufen ist.
* Wenn ein Benutzer das Token &quot;Temp Pass&quot;vor Ablauf löscht, generieren die nachfolgenden Berechtigungsprüfungen ein Token, dessen TTL der verbleibenden Zeit entspricht.
* Wenn der Benutzer das Token für den temporären Pass nach Ablauf löscht, geben die nachfolgenden Berechtigungsprüfungen &quot;Benutzer nicht autorisiert&quot;zurück.

Beispiele für die Codierung der in diesem Abschnitt beschriebenen Implementierungsdetails finden Sie in den Beispielen in [Beispielcode](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code) unten.

## Beispielcode {#tempass-sample-code}

Die folgenden Abschnitte zeigen, wie Sie die Adobe Pass Authentication API aufrufen, um den Temp Pass-Fluss zu implementieren:

* [iFrame-Anmeldungsbeispiel](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [Beispiel für automatische Anmeldung](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### iFrame-Anmeldungsbeispiel {#iframe-login-sample}

In diesem Beispiel wird gezeigt, wie der Temp Pass implementiert wird, wenn MVPDs die iFrame-Integration unterstützen:

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var ae, ifrm, providersMenu, previousSelectedProvider;
        var tempassSelected = false;
 
        $(document).ready(function() {
            ifrm = $('#ifrm');
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {wmode: "transparent", allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded() {
            ae = $('#accessEnabler')[0];
            ae.setProviderDialogURL("none");
            ae.setRequestor("sample_requestor_Id");
            previousSelectedProvider = ae.getSelectedProvider(); 
            ae.checkAuthentication();
        }
 
        function createIFrame() {
            providersMenu.hide();
 
            // If the user already used TempPass once, hide the button
            if ($.cookie("TPSelected") == "1"){
                $('#tempassBtn').hide();
            }
            ifrm.show();
        }
 
        function displayProviderDialog(providers) {
            if (tempassSelected) {
                // Remember in a cookie that the user selected temp pass
                $.cookie("TPSelected", "1", {expires: 366, path: '/'});
 
                // Authenticate with temp pass
                ae.setSelectedProvider("TempPass");
            } else {
                $('#loginBtn').hide();
                providersMenu = $('<select></select>');
 
                providersMenu.change(function(event){
                    ae.setSelectedProvider(event.target.value);
                });
 
                $.each(providers, function(k, v) {
                    // Add the MVPDs to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if(v.ID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:v.ID}).text(v.displayName));                       
                    }
                });
                $('body').append(providersMenu);
            }
        }
 
        function setAuthenticationStatus(status, code) {
            loginBtn = $('#loginBtn');
            logoutBtn = $('#logoutBtn');
            console.log(previousSelectedProvider);
 
            if (status == 1) {
                $('#selectedProvider').text("Authenticated with " + ae.getSelectedProvider().MVPD + "   ");
                loginBtn.hide();
                logoutBtn.show();
 
                // Get authorization
                ae.getAuthorization("sample_requestor_Id");
            } else {
                // If selected provider is TempPass but the user is not authenticated,
                //   infer that the TempPass period has expired, so reset the MVPD selection
                if (previousSelectedProvider && previousSelectedProvider.MVPD == "TempPass") {
                    previousSelectedProvider = null;
                    ae.setSelectedProvider(null);
                    alert("Your Temp Pass has expired, please login with your regular cable provider!");
                }
                loginBtn.show();
                logoutBtn.hide();
            }
        }
 
        function selectTempPass() {
            ifrm.hide();
 
            // Signal the fact that the user selected temp pass
            tempassSelected = true;
 
            // Cancel the current authentication flow
            ae.setSelectedProvider(null);
 
            // Retry authentication
            ae.getAuthentication();
 
        }
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="ae.getAuthentication();">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
           style="display: none" onclick="ae.logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px; width: 400px; height: 400px; border: 2px solid red;">
        <button id="tempassBtn"
           onclick="selectTempPass();"
             style="float:left">Don't know your credentials? Click here to get a Temp Pass.
        </button>
        <button onclick="window.location.reload()" style="float:right">X</button>
        <br />
        <hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="90%" height="90%" frameborder="0"></iframe>
    </div>
    <br/>
    <div id="ae" style="display: none">
        <p>Loading Access Enabler...</p>
    </div>
</body>
</html>
```

#### Anwendungsfälle für die iFrame-Anmeldung {#iframe-login-use-cases}

**So fordern Sie zum ersten Mal einen Temp-Pass an:**

1. Ein Benutzer greift auf die Seite des Programmierers zu und klickt auf den Anmelde-Link.
1. Die MVPD-Auswahl wird geöffnet und der Benutzer wählt einen MVPD aus der Liste aus.
1. Der Authentifizierungs-iFrame wird angezeigt. Dieser iFrame enthält einen Link &quot;Temp Pass&quot;.
1. Der Benutzer klickt auf &quot;Temp Pass&quot;, sodass der Programmierer einem Cookie ein Flag hinzufügt, um zu verhindern, dass der Benutzer bei nachfolgenden Seitenbesuchen den Link &quot;Temp Pass&quot;sieht.
1. Die Authentifizierungsanfrage für den Temp Pass erreicht die Adobe Pass-Authentifizierungsserver und generiert ein Authentifizierungstoken. Die TTL entspricht dem vom Programmierer für den Temp Pass festgelegten Zeitraum.
1. Die Autorisierungsanfrage für den Temp Pass erreicht die Adobe Pass-Authentifizierungsserver.
1. Die Adobe Pass-Authentifizierungsserver extrahieren die Geräte- und Anforderungs-IDs aus der Anforderung und speichern sie zusammen mit der Ablaufzeit in der Datenbank. Die Ablaufzeit wird berechnet als: anfängliche Zeit für die Anforderung des Temp Pass sowie TTL (vom Programmierer angegeben).
1. Die Adobe Pass-Authentifizierungsserver generieren ein Autorisierungstoken.
1. Der Benutzer greift auf den geschützten Inhalt zu.

**So fordern Sie einen Temp-Pass erneut an, nachdem ein wiederkehrender Benutzer von &quot;Temp Pass&quot;die Browsercookies gelöscht hat:**

1. Der Benutzer greift auf die Seite des Programmierers zu und klickt auf den Anmelde-Link.
1. Die MVPD-Auswahl wird geöffnet und der Benutzer wählt einen MVPD aus der Liste aus.
1. Der Authentifizierungs-iFrame wird angezeigt. Dieser iFrame enthält einen &quot;Temp Pass&quot;-Link (der Benutzer hat das ursprüngliche Cookie gelöscht, sodass der Programmierer nicht weiß, ob der Benutzer zuvor auf den Link &quot;Temp Pass&quot; geklickt hat).
1. Der Benutzer klickt erneut auf &quot;Temp Pass&quot;, sodass der Programmierer einem Cookie erneut ein Flag hinzufügt, um zu verhindern, dass der Benutzer bei nachfolgenden Seitenbesuchen den Link &quot;Temp Pass&quot;sieht.
1. Die Authentifizierungsanfrage für den Temp Pass erreicht die Adobe Pass-Authentifizierungsserver, die ein Authentifizierungstoken generieren. Die TTL ist jetzt die verbleibende Zeit für den Temp Pass (die Differenz zwischen der aktuellen Zeit und der mit der Geräte-ID verknüpften Ablaufzeit).
1. Die Autorisierungsanfrage für den Temp Pass erreicht die Adobe Pass-Authentifizierungsserver.
1. Die Adobe Pass-Authentifizierungsserver extrahieren die Geräte- und Anfragende-IDs aus der Anfrage und verwenden sie zum Abrufen der Ablaufzeit aus der Adobe Pass-Authentifizierungsdatenbank. Die aktuelle Zeit wird mit der Ablaufzeit verglichen.
1. Wenn der Temp Pass des Benutzers nicht abgelaufen ist, generieren die Adobe Pass-Authentifizierungsserver ein Autorisierungstoken.
1. Wenn der temporäre Pass des Benutzers nicht abgelaufen ist, kann der Benutzer auf den geschützten Inhalt zugreifen.

### Beispiel für automatische Anmeldung {#auto-login-sample}

Das folgende Beispiel zeigt einen Fall, in dem ein Benutzer beim Besuch einer Site automatisch mit TempPass angemeldet wird. Der Benutzer kann sich jederzeit mit einem regulären MVPD anmelden und wird gewarnt, wenn TempPass abgelaufen ist:

```HTML
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript"
             src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript"
             src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript"
             src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var REQUESTOR = "REF";
        var RESOURCE = "sample_requestor_Id";
        var selectedProvider = null;
        var mvpds;
        var hasTempPassMVPD = false;
 
        // Used to cache the mvpd picker
        var picker;
 
        $(document).ready(function() {
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded(){
            console.log("AccessEnabler loaded");
            ae = $('#accessEnabler')[0];
 
            // Make sure the default picker is disabled
            ae.setProviderDialogURL("none");
 
            ae.setRequestor(REQUESTOR);
            ae.checkAuthentication();
        }
 
        /**
         * Callback received as a result of setRequestor()
         *
         * @param xml object holding the configuration for the current REQUESTOR
         * including the MVPD list
         */
        function setConfig(config) {
            // Save the mvpd list
            var mvpdList = $.parseXML(config);
            mvpds = $(mvpdList).find('mvpd');
 
            // Create the picker only once and cache it
            if(!picker) {
                picker = $('<div id="mvpdPicker"/>');
 
                var providersMenu = $('<select id="mvpdList" multiple></select>');
 
                $.each(mvpds, function(k, v) {
                    var mvpdID = $(v).find("id").text();
                    var mvpdName = $(v).find("displayName").text();
 
                    // Add the mvpd's to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if (mvpdID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:mvpdID}).text(mvpdName));
                    } else {
                        hasTempPassMVPD = true;
                    }
                });
                picker.append(providersMenu);
                picker.append($('<br/>'));
                picker.append($('<input type="button" onclick="login()" value="login" />'));
                picker.append($('<input type="button" onclick="cancelPicker()" value="cancel" />'));                  
            }
 
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");             
            }
        }
 
        /**
         * Callback triggered for iFramed MVPD's
         */
        function createIFrame() {
            $('#mvpdPicker').remove();
            $('#ifrm').show();
        }
 
        /**
         * Hides the MVPD picker
         * when the user clicks "Cancel"
         */
        function cancelPicker() {
            $('#video').show();
            $('#mvpdPicker').remove();
            $('#loginBtn').show();
        }
 
        /**
         * Pops up the MVPD picker
         */
        function showPicker() {
            $('#video').hide();
            $('#loginBtn').hide();
            $('body').append(picker);
        }
 
        function logout() {
            $.removeCookie('tempPassUsed');
            ae.logout();
        }
 
        /**
         * Performs login with the selected MVPD
         */
        function login() {
            selectedProvider = $('#mvpdList').val()[0];
 
            // Make sure we clear out previously
            // selected. This is a must if we want to force
            // login with a real MVPD while still logged in with
            // TempPass, without doing an ae.logout()
            ae.setSelectedProvider(null);
            ae.getAuthentication();
        }

        /**
         * Callback triggered by AccessEnabler. This is usually
         * triggered in order to display the MVPD picker, but
         * since we already constructed, cached, and displayed the
         * picker, and the user already picked the MVPD, we don't need
         * to do anything here but state management
         */
        function displayProviderDialog() {
            // If the selected MVPD is TempPass
            // store this fact in a cookie,
            // otherwise clear it
            if (selectedProvider != 'TempPass') {
                $.removeCookie('tempPassUsed');
            } else {
                $.cookie("tempPassUsed", 1);
            }
 
            // Since the picker was already shown
            // and the user picked an MVPD,
            // just proceed to login
            ae.setSelectedProvider(selectedProvider);
        }
 
        function setAuthenticationStatus(status, code) {
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");
            } else if(status == 1) {
                selectedProvider = ae.getSelectedProvider().MVPD;
                $('#selectedProvider').text("Authenticated with " + selectedProvider + "   ");
 
                // If authenticated with TempPass
                // allow the user to login with
                // a real MVPD
                if (selectedProvider == "TempPass") {
                    $('#loginBtn').show();
                    $('#logoutBtn').hide();
                } else {
                    $('#loginBtn').hide();
                    $('#logoutBtn').show();
                }
 
                // Get authorization
                // Note: This is mandatory in order to "start" the temp pass countdown
                ae.checkAuthorization(RESOURCE);
            } else if(code != "Provider not Selected Error") {
                // Auto-authenticate with TempPass only if we infer
                // that TempPass has not expired, otherwise we
                // inform the user that TempPass has expired
                if ($.cookie('tempPassUsed') == 1) {
                   $('#selectedProvider').text("Your Temp Pass has expired, please log in with your cable provider!");
                   $('#logoutBtn').show();
                   showPicker();
                } else {
                    selectedProvider = 'TempPass';
                    ae.getAuthentication();
                }
            }
        }
 
        /**
         * Displays the picker as a result
         * of user action
         */
        function loginClicked() {
            $('#loginBtn').hide();
            showPicker();
        }
 
        /**
         * Callback triggered in case of authorization success
         */
        function setToken(token) {
            console.log(token);
            $('#video').html('<img src=">');
        }
 
        /**
         * Callback triggered in case of authz failure
         */
        function tokenRequestFailed(resource, status, message) {
            console.log(resource);
            $('#video').html('<p style="color: red">' + status + ': ' + message + '</p>');
        }
 
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="loginClicked()">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
        style="display: none" onclick="logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px;
         width: 400px; height: 400px; border: 2px solid red;">
        <button onclick="window.location.reload()" style="float:right">Close this window</button>
        <br /><hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="80%" height="80%" frameborder="0"></iframe>  
    </div>
    <br/>
 
    <div id="video"></div>
    <div id="ae" style="display: none"><p>Loading Access Enabler...</p></div>
</body>
</html>
```

## Verwenden mehrerer temporärer Durchgänge {#use-mult-tempass}

Bestimmte Ereignisse erfordern einen stufenweisen freien Zugang zu Inhalten, z. B. ein anfängliches Intervall für den freien Zugang (z. B. 4 Stunden), gefolgt von täglichen freien Zugriffen (z. B. 10 Minuten an jedem nachfolgenden Tag).  Damit ein Programmierer dieses Szenario implementieren kann, muss er es mit seinem Adobe-Ansprechpartner anordnen, um zwei temporäre MVPDs für den Programmierer zu konfigurieren.

Für dieses Beispielszenario (eine anfängliche 4-Stunden-kostenlose Sitzung, gefolgt von 10-minütigen freien Sitzungen) konfiguriert Adobe einen MVPD namens TempPass1 mit einer Time-To-Live (TTL) von 4 Stunden und einen TempPass2 mit einer TTL von 10 Minuten für den Folgezeitraum.  Beide sind mit der Anforderungs-ID des Programmierers verknüpft.

### Programmdurchführung {#mult-tempass-prog-impl}

Nachdem Adobe die beiden TempPass-Instanzen konfiguriert hat, werden die beiden zusätzlichen MVPDs (TempPass1 und TempPass2) in der MVPD-Liste des Programmierers angezeigt.  Der Programmierer muss die folgenden Schritte ausführen, um die verschiedenen temporären Durchgänge zu implementieren:

1. Melden Sie sich beim ersten Besuch des Benutzers auf der Site automatisch mit TempPass1 an. Sie können das obige Beispiel für die Autologin als Ausgangspunkt für diese Aufgabe verwenden.
1. Wenn Sie feststellen, dass TempPass1 abgelaufen ist, speichern Sie die Tatsache (in einem Cookie/lokalen Speicher) und stellen Sie dem Benutzer Ihre standardmäßige MVPD-Auswahl zur Verfügung. **Achten Sie darauf, TempPass1 und TempPass2 aus dieser Liste herauszufiltern**.
1. Wenn TempPass1 abgelaufen ist, müssen Sie diesen Benutzer an jedem nachfolgenden Tag automatisch mit TempPass2 anmelden.
1. Wenn TempPass2 abgelaufen ist, speichern Sie die Tatsache (in einem Cookie/lokalen Speicher) für den Tag und präsentieren Sie dem Benutzer Ihre standardmäßige MVPD-Auswahl. Stellen Sie erneut sicher, dass Sie TempPass1 und TempPass2 aus dieser Liste herausfiltern.
1. Setzen Sie an jedem neuen Tag um 00:00 Uhr alle temporären Durchgänge für TempPass2 zurück, indem Sie die Web-API [TempPass zurücksetzen](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass) verwenden.

>[!NOTE]
>**Programmierungshinweis:** Die Adobe Pass-Authentifizierung verfügt über keinen integrierten Mechanismus, um das kostenlose Streaming nach Ablauf der 10 Minuten zu stoppen.  Es ist Sache der Programmierer, den Zugriff nach Ablauf von TempPass2 zu beschränken. Um dies zu erreichen, können Programmierer in ihren Sites/Apps alle X Minuten einen &quot;checkAuthorization&quot;-Aufruf implementieren, wobei X der Zeitraum ist, den der Programmierer für seine Apps nutzt.

## Alle temporären Durchgänge zurücksetzen {#reset-all-tempass}

Bestimmte Geschäftsregeln erfordern eine regelmäßige Bereinigung des Temp Pass oder ein Ad-hoc-Zurücksetzen aller Temp Passes, die für eine bestimmte Anforderungs-ID und MVPD-ID ausgestellt wurden. Diese Funktion unterstützt Anwendungsfälle wie die folgenden:

* Täglicher 10-minütiger Temp-Pass (der Temp-Pass muss zu Beginn des Tages zurückgesetzt werden)
* Ein Temp Pass für alle Benutzer während der Neuigkeiten. (Der Temp-Pass muss für alle Geräte zurückgesetzt werden, sobald die brechenden Nachrichten beginnen.)
* Das Szenario mit dem mehrfachen Temp-Pass , das eine Kombination aus einem anfänglichen Anzeigezeitraum einer bestimmten Länge und nachfolgenden täglichen Zeiträumen einer anderen Länge bietet.

Um alle temporären Passes zurückzusetzen, stellt die Adobe Pass-Authentifizierung Programmierern eine Web-API für *public* bereit:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>Die obige URL ersetzt die vorherige Reset-API. Die alte Reset-API (v1) wird nicht mehr unterstützt.

* **Protokoll:** HTTPS
* **Host:**
   * Release - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Pfad:** /reset-tempass/v2/reset
* **Abfrageparameter:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Kopfzeilen:** ApiKey - 1232293681726481
* **Antwort:**
   * Erfolg - HTTP 204
   * Fehler:
      * HTTP 400 für eine falsche Anforderung
      * HTTP 401, wenn der APIKey nicht angegeben wurde
      * HTTP 403, wenn der APIKey ungültig ist

Beispiel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## Unterstützte Clients {#supp-clients}

Unterstützung des Tools für die vorübergehende Weiterleitung und Zurücksetzung nach Plattform:

| Adobe Pass-Authentifizierungs-Clients | Temporärer Pass | Tool zurücksetzen |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabler | JA | JA |
| Nativer Client iOS | JA | JA |
| Nativer Client tvOS | JA | JA |
| Nativer Client Android | JA | JA |
| Nativer Client fireTV | JA | JA |
| Clientlose API | JA | JA |

## Einschränkungen und bekannte Probleme {#limitations}

In diesem Abschnitt werden die Einschränkungen beschrieben, die für die aktuelle Implementierung des Temp Pass gelten.

**JavaScript SDK**: unterstützt das Zurücksetzen der Funktion zum Vorübergehen von Temp-Benachrichtigungen von Version **3.X und höher**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
