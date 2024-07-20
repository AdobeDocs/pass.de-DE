---
title: Übersicht über das JavaScript SDK
description: Übersicht über das JavaScript SDK
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# Übersicht über das JavaScript SDK {#javascript-sdk-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung

Adobe empfiehlt dringend, zur neuesten JS v4.x der AccessEnabler-Bibliothek zu migrieren.

Die Integration von Adobe Pass Authentication JavaScript bietet Programmierern eine TV-Überall-Lösung in der bekannten Entwicklungsumgebung für JS-Webanwendungen. Die Hauptkomponenten der Integration sind Ihre &quot;High-Level&quot;-Anwendung (Benutzerinteraktion, Videopräsentation) und die von der Adobe bereitgestellte &quot;Low-Level&quot;-AccessEnabler-Bibliothek, die Ihren Eintrag zu den Berechtigungsflüssen bereitstellt und die Kommunikation mit Adobe Pass-Authentifizierungsservern verarbeitet.

Der allgemeine Berechtigungsfluss für die Adobe Pass-Authentifizierung wird im [Berechtigungsfluss für Programmierer](/help/authentication/entitlement-flow.md) behandelt, und das JavaScript-Integrations-Cookbook führt Sie durch die Implementierung. In den folgenden Abschnitten finden Sie Beschreibungen und Beispiele für die Integration von JavaScript AccessEnabler.

>[!IMPORTANT]
>
>In diesem Dokument wird die Implementierung für eine Desktop-Web-Lösung beschrieben. Die JavaScript-Bibliothek wird nicht auf mobilen Plattformen unterstützt (z. B. Safari auf iOS, Chrome auf Android). Verwenden Sie unsere nativen SDKs, wenn Sie eine mobile Plattform (iOS, Android, Windows) verwenden möchten.

## Erstellen des MVPD-Auswahldialogfelds {#creating-the-mvpd-selection-dialog}

Damit sich ein Benutzer bei seinem MVPD anmelden und authentifiziert werden kann, muss Ihre Seite oder Ihr Player eine Möglichkeit bieten, mit der er seinen MVPD identifizieren kann. Für die Entwicklung wird eine Standardversion eines MVPD-Auswahldialogfelds bereitgestellt. Zur Verwendung in der Produktion müssen Sie Ihren eigenen MVPD-Selektor implementieren.

Wenn Sie bereits wissen, wer der Anbieter des Kunden ist, können Sie den MVPD programmgesteuert ](/help/authentication/home.md) festlegen, ohne dass der Benutzer interagiert. [ Die Technik ist identisch, umgeht jedoch den Schritt, das Dialogfeld Provider Selector aufzurufen und den Kunden zur Auswahl seines MVPD aufzufordern.

## Anzeigen des Dienstleisters {#displaying-the-service-provider}

Im folgenden Codebeispiel wird gezeigt, wie der Dienstleister für den aktuellen Kunden erkannt und angezeigt wird:

**HTML** - Auf dieser Seite wird ein Abschnitt zur Seite hinzugefügt, in dem der ausgewählte Anbieter des Kunden angezeigt wird, sofern er bereits angemeldet ist:

```HTML
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
            "http://www.w3.org/TR/html4/loose.dtd"> 
    <html>
    <head>
        <title>Get the distributor that is used for the authentication</title>
        <script type="text/javascript" src="libs/jquery-1.4.4.min.js"></script>
        <script type="text/javascript" src="libs/swfobject.js"></script>
        <script type="text/javascript" src="scripts/sample6.js"></script>
    </head>
    <body>
        <div id="alternative">
        <a href="http://www.adobe.com/go/getflashplayer"> 
            <img src="http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif" 
                 alt="Get Adobe Flash player"/> </a>
        </div> 
        <div id="user_authenticated">Please wait while we verify your authentication status</div> 
        <div id="control">
        <button id="sign_in" style="display:none;">Sign in</button>
        <button id="sign_out" style="display:none;">Sign out</button>
        </div> 
        <div id="distributor"></div> 
        <div id="provider_selector" style="display:none;">
        <select id="provider_list"></select>
        <button id="login">Login</button>
        <button id="cancel">Cancel</button>
        </div> 
        <div id="video_player">This is a video player showing an image as a preview.  You are not yet authorized to see this content.  Make sure that you first sign in.
        </div> 
        <div id="get_authorization">
        <button id="request_access" style="display:none;">Request access</button>
        </div> 
    </body>
    </html>
```


**JavaScript** Diese JavaScript-Datei fragt den Access Enabler für den aktuellen Provider ab, wenn der Benutzer bereits angemeldet ist, und zeigt das Ergebnis im dafür reservierten Seitenabschnitt an. Außerdem wird ein MVPD-Auswahldialogfeld implementiert:

```JS
    $(function() {
        embedAE();
    }); 
    function embedAE() {
        var flashvars = {};
        var params = {
            bgcolor: "#869ca7",
            quality: "high",
            allowscriptaccess: "always"
        };
        var attributes = {
            id: "ae_swf",
            align: "middle",
            name: "ae_swf"
        };
        swfobject.embedSWF(
                "https://entitlement.auth-staging.adobe.com/entitlement/AccessEnabler.swf",      
                "alternative", "1", "1", "10.1.53", "", flashvars, params, attributes);
    } 
    function getAE() {
        return document.getElementById("ae_swf");
    } 
    function swfLoaded() {
        var swf = getAE();
        initBindings();
        // don't load the default provider-selection dialog 
        swf.setProviderDialogURL("none");
        // identify yourself 
        swf.setRequestor("sample_requestor_Id");
        swf.checkAuthentication();
    } 
    // Define the button actions for the provider dialog
    function initBindings() {
        var provider_selector = $('#provider_selector');
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var login = $('#login');
        var cancel = $('#cancel');
        var request_access = $('#request_access'); 
        sign_out.click(function() {
            getAE().logout();
        });
        sign_in.click(function() {
            getAE().getAuthentication();
        }); 
        login.click(function() {
            var selected_mvpd_id = $("#provider_list option:selected").val();
            getAE().setSelectedProvider(selected_mvpd_id);
        }); 
        cancel.click(function() {
            getAE().setSelectedProvider(null);
            provider_selector.hide();
        }); 
        request_access.click(function(){
            getAE().getAuthorization("sample_requestor_Id");
        });
    }
    // Called if user is already authenticated; queries AE to find the current user's MVPD, 
    // Adds the result to the UI 
    function setDistributor() {
        var distributor = $("#distributor");
        var selectedProvider = getAE().getSelectedProvider();
        distributor.replaceWith('<div id="distributor">' +
                                'Courtesy of ' +
                                selectedProvider.MVPD +
                                '</div>');
    } 
    // Adjust the contents of the provider dialog, based on authentication status 
    // Adds the call to check and display the current distributor, if the user is already
    // logged in. 
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        var user_authenticated = $("#user_authenticated");
        var video_player = $("#video_player");
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var request_access = $('#request_access'); 
        if (isAuthenticated == 1) {
            setDistributor();
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are authenticated - if you wish you can sign out
                                           </div>');
            sign_out.show();
            sign_in.hide();
            video_player.replaceWith('<div id="video_player">Now you can ask for access.</div>');
            request_access.show();
        } else {
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are not authenticated please sign in
                                           </div>');
            sign_in.show();
            sign_out.hide();
        }
    } 
    // Allow user to choose a provider 
    function displayProviderDialog(providers) {
        var provider_list = $("#provider_list");
        var options = provider_list.attr('options');
        for (var index in providers) {
            options[index] = new Option(providers[index].displayName,
                                        providers[index].ID);
        }
        //select the first one by default 
        if (!$("#provider_list option:selected").length)
            $("#provider_list option[0]").attr('selected', 'selected'); 
        $('#provider_selector').show();
    } 
    // Handle the authorization response by sending token to video player 
    function setToken(requested_resource_id, token) {
        var video_player = $("#video_player");
        var request_access = $("#request_access");
        video_player.replaceWith('<div id="video_player">Now you are authorized to ' +
                                 requested_resource_id +
                                 ' content with ' + token + ' . </div>');
    }
```

## Abmelden {#logout}

Rufen Sie `logout()` auf, um den Abmeldevorgang zu starten. Diese Methode akzeptiert keine Argumente. Er meldet den aktuellen Benutzer ab, löscht alle Authentifizierungs- und Autorisierungsinformationen für diesen Benutzer und löscht alle AuthN- und AuthZ-Token aus dem lokalen System.

In einigen Fällen ist Ihr Player nicht für die Verarbeitung von Benutzerabmeldungen verantwortlich:



- **Wenn der Abmeldevorgang von einer Site initiiert wird, die nicht mit der Adobe Pass-Authentifizierung integriert ist.** In diesem Fall kann der MVPD den Adobe Pass Authentication Single Logout-Dienst über eine Browserumleitung aufrufen. (Der Aufruf von SLO über einen Backchannel-Aufruf wird derzeit nicht unterstützt.)

>[!NOTE]
>
>Wenn der Benutzer seinen Computer so lange im Leerlauf lässt, dass seine Token ablaufen, kann er dennoch zu seiner Sitzung zurückkehren und die Abmeldung erfolgreich starten. Die Adobe Pass-Authentifizierung stellt sicher, dass alle Token gelöscht werden, und benachrichtigt den MVPD darüber, dass er auch seine Sitzung löscht.

Der folgende JavaScript-Code veranschaulicht die Abmeldung (Deauthentifizierung) eines derzeit authentifizierten Benutzers:

```JS
    [...]
    /*
     * @param isAuthenticated authentication status: 1 - Authenticated, 0 - not authenticated 
     * @param errorCode Any error that occurred when determining the authentication status.
     * An empty string if no error occurred.
     */
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        if (isAuthenticated == 1 ) {
            /* User is authenticated – we can logout / deauthenticate */
            accessEnablerObject.logout();
            /* Logs out the current user, clearing all authentication
             * and authorization information for that user. Deletes
             * all authN and authZ tokens from the user's system.
             */
        } else {
            /*
             * User is NOT authenticated – we do not need to logout;
             * Show the log-in image/button/message
             */
        }
    }
```

<!--
**Related Information**

- [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
- [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
- [JavaScript Code Samples](#javascript-code-samples)
- [Understanding Tokens](#understanding_tokens)
-->
