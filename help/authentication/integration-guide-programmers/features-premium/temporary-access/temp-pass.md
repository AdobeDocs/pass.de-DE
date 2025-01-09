---
title: Temporärer Durchlauf
description: Temporärer Durchlauf
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# Temporärer Durchlauf {#temp-pass}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Funktionsübersicht {#tempass-featur-summary}

Mit Temp Pass können Programmierer Benutzern, die über keine Kontoanmeldeinformationen für eine MVPD verfügen, temporären Zugriff auf ihre geschützten Inhalte bieten.  Temporärer Pass umfasst die folgenden Funktionen:

* Temporärer Pass kann so konfiguriert werden, dass er temporären Zugriff für eine Vielzahl von Szenarien bietet, darunter die folgenden:
   * Ein Programmierer kann eine tägliche, kurze (z. B. eine 10-minütige Vorschau) Version einer seiner Sites anbieten.
   * Ein Programmierer kann eine einzige, lange Präsentation (z. B. vier Stunden) eines großen Sportereignisses wie Olympia oder NCAA March Madness anbieten.
   * Ein Programmierer kann eine Kombination der beiden vorherigen Szenarien bereitstellen, z. B. einen anfänglichen, längeren Betrachtungszeitraum an einem Tag gefolgt von einer Reihe kurzer Zeiträume, die sich täglich für einige nachfolgende Tage wiederholen.
* Programmierer geben die Dauer (Time-to-Live, oder TTL) ihres temporären Passes an.
* Temporärer Pass wird pro Anfordernder ausgeführt.  Beispielsweise könnte NBC einen 4-Stunden-Temp-Pass für den Anforderer „NBCOlympics“ einrichten.
* Programmierer können alle einem bestimmten Anforderer gewährten Token zurücksetzen.  Die „temporäre MVPD&quot;, die zur Implementierung des temporären Passes verwendet wird, muss mit aktivierter „Authentifizierung pro Anforderer“ konfiguriert werden.
* **Temp Pass-Zugriff wird einzelnen Benutzern auf bestimmten Geräten gewährt**. Nach Ablauf des temporären Passzugriffs für einen Benutzer kann dieser Benutzer keinen temporären Zugriff auf dasselbe Gerät erhalten, bis das abgelaufene Autorisierungstoken dieses Benutzers vom Adobe Pass-Authentifizierungsserver gelöscht wird.


>[!NOTE]
>
>Temp Pass ist Teil des Premium-Workflow-Pakets. Wenden Sie sich an Ihren Adobe Pass-Vertriebsmitarbeiter, wenn Sie diese Funktion verwenden möchten.

## Funktionsdetails {#tempass-featur-details}

* **So wird die Anzeigezeit berechnet** - Die Zeit, die ein temporärer Durchlauf gültig bleibt, korreliert nicht mit der Zeit, die ein Benutzer mit der Anzeige von Inhalten auf dem Programm des Programmierers verbringt.  Bei der ersten Benutzeranforderung für die Autorisierung über einen temporären Pass wird eine Ablaufzeit berechnet, indem die anfängliche aktuelle Anforderungszeit zur vom Programmierer angegebenen TTL hinzugefügt wird. Diese Ablaufzeit ist mit der Geräte-ID des Benutzers und der Anforderer-ID des Programmierers verknüpft und in der Adobe Pass-Authentifizierungsdatenbank gespeichert. Jedes Mal, wenn Benutzende mithilfe von „Temp Pass“ von demselben Gerät aus auf Inhalte zugreifen möchten, vergleicht die Adobe Pass-Authentifizierung die Server-Anfragezeit mit der Ablaufzeit, die mit der Geräte-ID der Benutzenden und der Anforderer-ID des Programmierers verknüpft ist. Wenn die Server-Anfragezeit kürzer als die Ablaufzeit ist, wird die Autorisierung gewährt, andernfalls wird die Autorisierung verweigert.
* **Konfigurationsparameter** - Die folgenden Parameter für den temporären Durchgang können von einem Programmierer angegeben werden, um eine Regel für den temporären Durchgang zu erstellen:
   * **Token-TTL**: Die Zeitspanne, die ein Benutzer ohne Anmeldung bei einer MVPD ansehen darf. Diese Zeit ist uhrbasiert und läuft ab, unabhängig davon, ob der Benutzer Inhalte ansieht oder nicht.
  >[!NOTE]
  >Einer Anforderer-ID kann nicht mehr als eine Temp-Pass-Regel zugeordnet sein.
* **Authentifizierung/Autorisierung** - Im Fluss „Temp Pass“ geben Sie den MVPD als „Temp Pass“ an.  Die Adobe Pass-Authentifizierung kommuniziert nicht mit einer tatsächlichen MVPD im Fluss „Temp Pass“, sodass die MVPD „Temp Pass“ jede Ressource autorisiert. Programmierer können eine Ressource, auf die zugegriffen werden kann, mit Temp Pass angeben, genau wie für die anderen Ressourcen auf ihrer Site. Die Media Verifier-Bibliothek kann wie gewohnt verwendet werden, um das kurze Medien-Token „Temp Pass“ zu überprüfen und eine Ressourcenüberprüfung vor der Wiedergabe zu erzwingen.
* **Tracking-Daten im temporären Pass-Fluss** - Zwei Punkte bezüglich Tracking-Daten während eines temporären Pass-Berechtigungsflusses:
   * Die Tracking-ID, die von der Adobe Pass-Authentifizierung an Ihren Callback **sendTrackingData()** übergeben wird, ist ein Hash der Geräte-ID.
   * Da die im Fluss für den temporären Pass verwendete MVPD-ID „Temp Pass“ lautet, wird dieselbe MVPD-ID an „sendTrackingData **() zurückgegeben**. Die meisten Programmierer werden die Metriken des temporären Passes wahrscheinlich anders behandeln wollen als die tatsächlichen MVPD-Metriken. Dies erfordert einige zusätzliche Arbeit in Ihrer Analytics-Implementierung.

Die folgende Abbildung zeigt den Fluss „Temporärer Pass“:

![Der temporäre Pass-Fluss](../../../assets/temp-pass-flow.png)

*Abbildung: Der temporäre Durchfluss*

## Temporären Pass implementieren {#implement-tempass}

Auf der Adobe Pass-Authentifizierungsseite wird Temp Pass implementiert, indem ein Pseudo-MVPD mit dem Namen „TempPass“ zur Serverkonfiguration des beteiligten Programmierers hinzugefügt wird.  Diese Pseudo-MVPD agiert wie eine eigentliche MVPD, die vorübergehend Zugriff auf den geschützten Inhalt des Programmierers gewährt.

Auf der Programmiererseite wird Temp Pass für die beiden Szenarien, die MVPDs für die Authentifizierung verwenden, wie folgt implementiert:

* **iFrame auf Programmiererseite**. Temporärer Pass funktioniert unabhängig vom Authentifizierungstyp eines MVPDs, aber für das iFrame-Szenario sind zusätzliche Schritte erforderlich, um den aktuellen Authentifizierungsfluss abzubrechen und sich mit temporärem Pass zu authentifizieren. Diese Schritte werden im Abschnitt [iFrame-Anmeldung](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) unten dargestellt.
* **Umleitung zur Anmeldeseite von MVPD**. Im herkömmlichen Fall, dass die Benutzeroberfläche zum Auslösen des temporären Durchgangs vor dem Beginn der Authentifizierung mit einer MVPD angezeigt wird, sind keine besonderen Schritte erforderlich. Temporärer Pass sollte wie ein regulärer MVPD behandelt werden.

Die folgenden Punkte gelten für beide Implementierungsszenarien:

* Der „Temp Pass“ sollte in der MVPD-Auswahl nur für Benutzende angezeigt werden, die noch keine Autorisierung für den Temp Pass angefordert haben. Das Blockieren der Anzeige für nachfolgende Anfragen kann erreicht werden, indem Cookies mit einem Flag versehen werden. Dies funktioniert so lange, wie der Benutzer den Browser-Cache nicht löscht. Wenn Benutzer ihre Browser-Caches löschen, wird in der Auswahl erneut „Temporärer Pass“ angezeigt und der Benutzer kann ihn erneut anfordern. Der Zugriff wird nur gewährt, wenn die Zeitspanne „Temp Pass“ noch nicht abgelaufen ist.
* Wenn ein(e) Benutzende(r) den Zugriff über Temp Pass anfordert, führt der Adobe Pass-Authentifizierungsserver während des Authentifizierungsprozesses seine übliche SAML-Anfrage (Security Assertion Markup Language) nicht durch. Stattdessen gibt der Authentifizierungsendpunkt bei jedem Aufruf von Erfolg zurück, solange Token für das Gerät gültig sind.
* Wenn ein temporärer Pass abläuft, wird sein Benutzer nicht mehr authentifiziert, da im Temp Pass-Fluss das Authentifizierungs-Token und das Autorisierungs-Token dasselbe Ablaufdatum haben. Um Benutzern zu erklären, dass ihr temporärer Pass abgelaufen ist, müssen Programmierer die ausgewählte MVPD direkt nach dem Aufruf von `setRequestor()` abrufen und dann `checkAuthentication()` wie gewohnt aufrufen. Im `setAuthenticationStatus()`-Callback kann überprüft werden, ob der Authentifizierungsstatus 0 ist. Wenn der ausgewählte MVPD „TempPass“ war, kann Benutzern eine Nachricht angezeigt werden, dass ihre Temp-Pass-Sitzung abgelaufen ist.
* Wenn ein(e) Benutzende(r) das Token „Temp Pass“ vor dem Ablauf löscht, erzeugen die nachfolgenden Berechtigungsprüfungen ein Token, das eine TTL hat, die der verbleibenden Zeit entspricht.
* Wenn der Benutzer das Token „Temporärer Pass“ nach Ablauf löscht, geben die nachfolgenden Berechtigungsprüfungen „Benutzer nicht autorisiert“ zurück.

Beispiele für die Programmierung der [ in diesem Abschnitt beschriebenen Implementierungsdetails finden ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code) in den Beispielen unter „Beispielcode“ unten.

## Beispielcode {#tempass-sample-code}

In den folgenden Abschnitten wird gezeigt, wie Sie die Adobe Pass-Authentifizierungs-API aufrufen, um den temporären Passfluss zu implementieren:

* [Beispiel für iFrame-Anmeldung](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [Beispiel für automatische Anmeldung](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### Beispiel für iFrame-Anmeldung {#iframe-login-sample}

Dieses Beispiel zeigt, wie Temp Pass für den Fall implementiert wird, dass MVPDs die iFrame-Integration unterstützen:

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

#### iFrame-Login-Anwendungsfälle {#iframe-login-use-cases}

**So fordern Sie zum ersten Mal einen temporären Pass an:**

1. Ein Benutzer greift auf die Seite des Programmierers zu und klickt auf den Anmelde-Link.
1. Die MVPD-Auswahl wird geöffnet, und der Benutzer wählt eine MVPD aus der Liste aus.
1. Der Authentifizierungs-iFrame wird angezeigt. Dieser iFrame enthält einen Link „Temporärer Pass“.
1. Der Benutzer klickt auf „Temp Pass“, sodass der Programmierer einem Cookie eine Markierung hinzufügt, um zu verhindern, dass der Benutzer den Link „Temp Pass“ bei nachfolgenden Besuchen der Seite sieht.
1. Die Authentifizierungsanfrage „Temp Pass“ erreicht die Adobe Pass-Authentifizierungsserver und sie generieren ein Authentifizierungstoken. Die TTL entspricht dem vom Programmierer für den temporären Durchlauf festgelegten Zeitraum.
1. Die Autorisierungsanfrage „Temp Pass“ erreicht die Adobe Pass-Authentifizierungsserver.
1. Die Adobe Pass-Authentifizierungsserver extrahieren die Geräte- und Anforderer-IDs aus der Anfrage und speichern sie zusammen mit der Ablaufzeit in der Datenbank. Die Ablaufzeit wird wie folgt berechnet: anfängliche Anforderungszeit für temporäre Übergänge plus TTL (vom Programmierer angegeben).
1. Die Adobe Pass-Authentifizierungsserver generieren ein Autorisierungstoken.
1. Der Benutzer greift auf den geschützten Inhalt zu.

**Um einen temporären Pass erneut anzufordern, nachdem ein zurückkehrender temporärer Pass-Benutzer die Browser-Cookies gelöscht hat:**

1. Der Benutzer greift auf die Seite des Programmierers zu und klickt auf den Anmelde-Link.
1. Die MVPD-Auswahl wird geöffnet, und der Benutzer wählt eine MVPD aus der Liste aus.
1. Der Authentifizierungs-iFrame wird angezeigt. Dieser iFrame enthält einen Link „Temp Pass“ (der Benutzer hat das ursprüngliche Cookie gelöscht, sodass der Programmierer nicht weiß, ob der Benutzer zuvor auf den Link „Temp Pass“ geklickt hat).
1. Der Benutzer klickt erneut auf „Temp Pass“, sodass der Programmierer einem Cookie erneut eine Markierung hinzufügt, um zu verhindern, dass der Benutzer bei nachfolgenden Besuchen der Seite den Link „Temp Pass“ sieht.
1. Die Authentifizierungsanfrage „Temp Pass“ erreicht die Adobe Pass-Authentifizierungsserver, die ein Authentifizierungstoken generieren. Die TTL ist jetzt die verbleibende Zeit für den temporären Durchlauf (die Differenz zwischen der aktuellen Zeit und der Ablaufzeit, die mit der Geräte-ID verknüpft ist).
1. Die Autorisierungsanfrage „Temp Pass“ erreicht die Adobe Pass-Authentifizierungsserver.
1. Die Adobe Pass-Authentifizierungsserver extrahieren die Geräte- und Anforderer-IDs aus der Anfrage und verwenden sie, um die Ablaufzeit aus der Adobe Pass-Authentifizierungsdatenbank abzurufen. Die aktuelle Zeit wird mit der Ablaufzeit verglichen.
1. Wenn der temporäre Pass des Benutzers nicht abgelaufen ist, generieren die Adobe Pass-Authentifizierungsserver ein Autorisierungs-Token.
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

Bestimmte Ereignisse erfordern einen stufenweisen freien Zugang zu Inhalten, z. B. ein anfängliches Intervall des freien Zugriffs (z. B. 4 Stunden), gefolgt von täglichen freien Zugriffen (z. B. 10 Minuten an jedem folgenden Tag).  Damit ein Programmierer dieses Szenario implementieren kann, muss er dies mit seinem Adobe-Kontakt arrangieren, um zwei temporäre MVPDs für den Programmierer zu konfigurieren.

In diesem Beispielszenario (eine erste kostenlose 4-stündige Sitzung, gefolgt von täglichen 10-minütigen kostenlosen Sitzungen) konfiguriert Adobe einen MVPD namens TempPass1 mit einer Lebensdauer von 4 Stunden und einen TempPass2 mit einer TTL von 10 Minuten für den folgenden Zeitraum.  Beide sind mit der Anforderer-ID des Programmierers verknüpft.

### Implementierung durch Programmierer {#mult-tempass-prog-impl}

Nachdem Adobe die beiden TempPass-Instanzen konfiguriert hat, werden die beiden zusätzlichen MVPDs (TempPass1 und TempPass2) in der MVPD-Liste des Programmierers angezeigt.  Der Programmierer muss die folgenden Schritte ausführen, um die mehreren temporären Durchgänge zu implementieren:

1. Melden Sie sich beim ersten Besuch der Benutzerin bzw. des Benutzers auf der Website automatisch mit TempPass1 an. Sie können das obige Beispiel für die automatische Anmeldung als Ausgangspunkt für diese Aufgabe verwenden.
1. Wenn Sie feststellen, dass TempPass1 abgelaufen ist, speichern Sie die Tatsache (in einem Cookie/lokalen Speicher) und präsentieren Sie den Benutzenden Ihre standardmäßige MVPD-Auswahl. **Achten Sie darauf, TempPass1 und TempPass2 aus dieser Liste herauszufiltern**.
1. Wenn TempPass1 abgelaufen ist, melden Sie diesen Benutzer an jedem folgenden Tag mit TempPass2 automatisch an.
1. Wenn TempPass2 abgelaufen ist, speichern Sie den Fakt (in einem Cookie/lokalen Speicher) für den Tag und präsentieren Sie den Benutzenden Ihre standardmäßige MVPD-Auswahl. Achten Sie erneut darauf, TempPass1 und TempPass2 aus dieser Liste herauszufiltern.
1. Setzen Sie an jedem neuen Tag um 00:00 Uhr alle temporären Durchläufe für TempPass2 mithilfe der Web-API [TempPass zurücksetzen](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass) zurück.

>[!NOTE]
>**Programmierhinweis:** Adobe Pass-Authentifizierung verfügt über keinen integrierten Mechanismus zum Stoppen des kostenlosen Streaming nach Ablauf der 10 Minuten.  Programmierer können den Zugriff nach Ablauf von TempPass2 einschränken. Zu diesem Zweck können Programmierer alle X Minuten einen Aufruf „checkAuthorization“ in ihre Sites/Apps implementieren, wobei X der Zeitraum ist, den der Programmierer für seine Apps als sinnvoll erachtet.

## Alle temporären Durchgänge zurücksetzen {#reset-all-tempass}

Bestimmte Geschäftsregeln erfordern eine regelmäßige Bereinigung des temporären Passes oder ein Ad-hoc-Zurücksetzen aller für eine bestimmte Anforderer-ID und MVPD-ID ausgestellten temporären Passes. Diese Funktion unterstützt Anwendungsfälle wie die folgenden:

* Ein täglicher 10-minütiger Temp Pass (der Temp Pass muss zu Beginn des Tages zurückgesetzt werden)
* Ein temporärer Pass steht allen Benutzern während der aktuellen Nachrichten zur Verfügung. (Der temporäre Pass muss für alle Geräte zurückgesetzt werden, sobald die aktuellen Nachrichten beginnen.)
* Das Szenario des mehrfachen temporären Durchlaufs bietet eine Kombination aus einem anfänglichen Betrachtungszeitraum von einer Länge, gefolgt von nachfolgenden täglichen Zeiträumen von einer anderen Länge.

Zum Zurücksetzen aller temporären Kennungen stellt die Adobe Pass-Authentifizierung Programmierern eine *öffentliche* Web-API zur Verfügung:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>Die obige URL ersetzt die vorherige Zurücksetzen-API. Die alte Zurücksetzen-API (v1) wird nicht mehr unterstützt.

* **protocol:** HTTPS
* **host:**
   * Version - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **Path:** /reset-tempass/v2/reset
* **Abfrageparameter:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Kopfzeilen:** API-Schlüssel - 1232293681726481
* **Antwort:**
   * Erfolg - HTTP 204
   * Fehler:
      * HTTP 400 für eine falsche Anfrage
      * HTTP 401, wenn der API-Schlüssel nicht angegeben wurde
      * HTTP 403, wenn der API-Schlüssel ungültig ist

Beispiel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## Unterstützte Clients {#supp-clients}

Unterstützung von Temp Pass and Reset Tool nach Plattform:

| Adobe Pass-Authentifizierungsclients | Temp Pass | Werkzeug zum Zurücksetzen |
|:--------------------------------------:|:---------:|:----------:|
| JS Access Enabler | JA | JA |
| Native Client-iOS | JA | JA |
| Natives Client-tvOS | JA | JA |
| Native Client-Android | JA | JA |
| Native Client-FireTV | JA | JA |
| Clientless-API | JA | JA |

## Einschränkungen und bekannte Probleme {#limitations}

In diesem Abschnitt werden die Einschränkungen beschrieben, die für die aktuelle Implementierung von Temp Pass gelten.

**JavaScript SDK**: Unterstützt die Funktion zum Zurücksetzen des temporären Übergangs von Version **3.X und höher**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
