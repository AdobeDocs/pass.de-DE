---
title: Übersicht über JavaScript SDK
description: Übersicht über JavaScript SDK
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# Übersicht über (ältere) JavaScript SDK {#javascript-sdk-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung

Adobe empfiehlt dringend, zur neuesten JS-Version 4.x der AccessEnabler-Bibliothek zu migrieren.

Die Adobe Pass-Authentifizierungs-JavaScript-Integration bietet Programmierern eine TV-Everywhere-Lösung in der bekannten JS-Webanwendungs-Entwicklungsumgebung. Die Hauptkomponenten der Integration sind Ihre „High-Level“-Anwendung (Benutzerinteraktion, Videopräsentation) und die von Adobe bereitgestellte „Low-Level“-AccessEnabler-Bibliothek, die Ihnen den Einstieg in die Berechtigungsflüsse ermöglicht und die Kommunikation mit Adobe Pass-Authentifizierungsservern übernimmt.

In den folgenden Abschnitten finden Sie Beschreibungen und Beispiele, die speziell für die JavaScript AccessEnabler-Integration gelten.

>[!IMPORTANT]
>
>In diesem Dokument wird die Implementierung für eine Desktop-Web-Lösung beschrieben. Die JavaScript-Bibliothek wird auf Mobilplattformen nicht unterstützt (z. B. Safari auf iOS, Chrome auf Android). Verwenden Sie unsere nativen SDKs, wenn Sie eine mobile Plattform (iOS, Android, Windows) als Ziel verwenden möchten.

## Erstellen des MVPD-Auswahldialogfelds {#creating-the-mvpd-selection-dialog}

Damit sich ein Benutzer bei seiner MVPD anmeldet und authentifiziert wird, muss seine Seite oder sein Player eine Möglichkeit bieten, seine MVPD zu identifizieren. Für die Entwicklung wird eine Standardversion eines MVPD-Auswahldialogfelds bereitgestellt. Zur Verwendung in der Produktion müssen Sie Ihren eigenen MVPD-Selektor implementieren.

Wenn Sie bereits wissen, wer der Anbieter des Kunden ist, können Sie [MVPD programmgesteuert festlegen](/help/authentication/home.md) ohne Benutzerinteraktion. Die Technik ist dieselbe, umgeht jedoch den Schritt, das Dialogfeld „Anbieterauswahl“ aufzurufen und den Kunden aufzufordern, seine MVPD auszuwählen.

## Anzeigen des Dienstleisters {#displaying-the-service-provider}

Im folgenden Codebeispiel wird veranschaulicht, wie der Dienstleister für den aktuellen Kunden ermittelt und angezeigt wird:

**HTML** - Auf dieser Seite wird der Seite ein Abschnitt hinzugefügt, in dem der vom Kunden ausgewählte Anbieter angezeigt wird, sofern er bereits angemeldet ist:

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


**JavaScript** Diese JavaScript-Datei fragt den Access Enabler für den aktuellen Anbieter ab, wenn der Benutzer bereits angemeldet ist, und zeigt das Ergebnis in dem für ihn reservierten Seitenabschnitt an. Außerdem wird ein MVPD-Auswahldialogfeld implementiert:

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

Rufen Sie `logout()` auf, um den Abmeldevorgang zu starten. Diese Methode benötigt keine Argumente. Er meldet den aktuellen Benutzer ab, löscht alle Authentifizierungs- und Autorisierungsinformationen für diesen Benutzer und löscht alle AuthN- und AuthZ-Token aus dem lokalen System.

Es gibt einige Fälle, in denen Ihr Player nicht für die Verarbeitung von Benutzerabmeldungen verantwortlich ist:



- **Wenn die Abmeldung von einer Site initiiert wird, die nicht in die Adobe Pass-Authentifizierung integriert ist.** In diesem Fall kann die MVPD den einmaligen Abmelde-Service der Adobe Pass-Authentifizierung über eine Browser-Umleitung aufrufen. (Das Aufrufen von SLO über einen Backchannel-Aufruf wird derzeit nicht unterstützt.)

>[!NOTE]
>
>Wenn der Benutzer den Computer lange genug inaktiv lässt, um seine Token ablaufen zu lassen, kann er dennoch zu seiner Sitzung zurückkehren und die Abmeldung erfolgreich initiieren. Die Adobe Pass-Authentifizierung stellt sicher, dass alle Token gelöscht werden, und benachrichtigt die MVPD, auch ihre Sitzung zu löschen.

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
