---
title: Migrieren der MVPD-Anmeldeseite von iFrame zu Popup
description: Migrieren der MVPD-Anmeldeseite von iFrame zu Popup
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 0%

---

# (Veraltet) Migrieren der MVPD-Anmeldeseite von iFrame zu Popup {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Popup im Vergleich zu iFrame {#popup-vs-iframe}

Einige Benutzende haben Probleme mit Drittanbieter-Cookies bei der iFrame-Implementierung einer MVPD-Anmeldeseite festgestellt.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Das Adobe Pass-Authentifizierungsteam **empfiehlt die Implementierung des Popup-/neuen Fensters auf der Anmeldeseite** anstelle der iFrame-Version in Firefox und Safari.  Wenn Sie jedoch eine Anmeldeseite für Internet Explorer implementieren, können Probleme mit der Popup-Implementierung auftreten. Die IE-Probleme werden dadurch verursacht, dass die Adobe Pass-Authentifizierung nach der Authentifizierung der Benutzenden mit ihrer MVPD im Popup-Fenster eine Umleitung der übergeordneten Seite erzwingt, die von Internet Explorer als Popup-Blocker angezeigt wird. Das Adobe Pass-Authentifizierungsteam **empfiehlt die Implementierung der iFrame-Anmeldung für Internet Explorer**.

Der in dieser Technote vorgestellte Beispiel-Code verwendet eine Hybridimplementierung von iFrame und Popup - durch Öffnen eines iFrames in Internet Explorer und eines Popup in den anderen Browsern.

Da bereits eine iFrame-Implementierung vorhanden ist, enthält der erste Teil der technischen Anmerkung den Code für die iFrame-Implementierung und der zweite Teil die Änderungen, die erforderlich sind, um die Popup-Implementierung als Standard zu berücksichtigen.


## MVPD-Auswahl mit Anmeldeseite in einem iFrame {#mvpd-pickr-iframe}

Frühere Code-Beispiele haben eine HTML-Seite gezeigt, die das Tag &lt;div> enthält, in dem der iFrame zusammen mit der Schaltfläche iFrame schließen erstellt werden soll:

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

Hier finden Sie den zugehörigen **JavaScript**-Code:

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## MVPD-Auswahl mit Anmeldeseite in einem Popup-Fenster {#mvpd-pickr-popup}

Da wir keinen **iFrame** mehr verwenden, enthält der HTML-Code weder den iFrame noch die Schaltfläche zum Schließen des iFrames. Das div, das zuvor den iFrame **mvpddiv**) enthielt, wird beibehalten und für Folgendes verwendet:

* um den Benutzer darüber zu informieren, dass die Anmeldeseite von MVPD bereits geöffnet ist, wenn der Popup-Fokus verloren geht
* So geben Sie einen Link an, um den Fokus wieder auf das Popup zu legen

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

Die Liste der MVPDs wird im div namens **picker** als select **-mvpdList**.

Ein neuer API-Callback wird verwendet - **setConfig(configXML)**. Der Rückruf wird nach dem Aufruf der Funktion setRequestor(RequestorID) ausgelöst. Dieser Callback gibt die Liste der MVPDs zurück, die mit der zuvor festgelegten RequestorID integriert sind. In der Rückrufmethode wird die eingehende XML geparst und die Liste der MVPDs zwischengespeichert. Die MVPD-Auswahl wird ebenfalls erstellt, aber nicht angezeigt.

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

Nachdem die Funktion getAuthentication() oder getAuthorization() aufgerufen wurde, wird der Rückruf displayProviderDialog() ausgelöst. Normalerweise wäre innerhalb dieses Callbacks die MVPD-Liste erstellt und angezeigt worden. Da die MVPD-Auswahl bereits erstellt ist, bleibt nur noch, sie den Benutzenden anzuzeigen.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

Nachdem der/die Benutzende eine MVPD aus der Auswahl ausgewählt hat, muss das Popup erstellt werden. Einige Browser blockieren das Popup möglicherweise, wenn es mit „about“ :blank einer Seite erstellt wurde, die sich in einer anderen Domain befindet. Daher wird empfohlen, es mit dem Host-Namen zu öffnen, von dem aus der AccessEnabler geladen wird.

In der iFrame-Implementierung erfolgte das Zurücksetzen des Authentifizierungsflusses durch die Schaltfläche btnCloseIframe und die JavaScript-Funktion closeIframeAction(), aber jetzt ist das Dekorieren des iFrames nicht mehr möglich. Dasselbe Verhalten wird also erreicht, indem darauf geachtet wird, wann das Popup geschlossen wird (entweder durch den Benutzer oder durch Beenden des Authentifizierungsflusses). Es wurde ein Code-Fragment hinzugefügt, das auch für den Fall hilfreich ist, dass der Benutzer den Fokus des Popup-Fensters verliert:

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

Beim Rückruf createIFrame() wird **mvpddiv** div angezeigt.

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* Der Beispielcode enthält eine hartcodierte Variable für die verwendete RequestorID - „REF“, die durch eine echte Anforderer-ID des Programmierers ersetzt werden sollte.
>* Der Beispiel-Code wird nur von einer Domain, die auf der Whitelist der verwendeten Anforderer-ID zugeordnet ist, ordnungsgemäß ausgeführt.
>* Da der gesamte Code zum Herunterladen verfügbar ist, wurde der in dieser Technote angezeigte Code gekürzt. Ein vollständiges Beispiel finden Sie unter **JS iFrame vs. Popup-Beispiel**.
>* Die externen JavaScript-Bibliotheken wurden von [Google Hosted Services](https://developers.google.com/speed/libraries/) verknüpft.
