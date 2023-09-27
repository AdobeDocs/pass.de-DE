---
title: Migrieren der MVPD-Anmeldeseite von iFrame zu Popup
description: Migrieren der MVPD-Anmeldeseite von iFrame zu Popup
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---

# Migrieren der MVPD-Anmeldeseite von iFrame zu Popup {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Popup versus iFrame {#popup-vs-iframe}

Bei einigen Benutzern traten bei der iFrame-Implementierung einer MVPD-Anmeldeseite Probleme mit Drittanbieter-Cookies auf.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Das Adobe Pass-Authentifizierungsteam **empfiehlt die Implementierung der Anmeldeseite für Popup/neues Fenster** anstatt der iFrame-Version in Firefox und Safari.  Wenn Sie jedoch eine Anmeldeseite für Internet Explorer implementieren, kann es bei der Popup-Implementierung zu Problemen kommen. Die IE-Probleme werden dadurch verursacht, dass die Adobe Pass-Authentifizierung nach der Authentifizierung des Benutzers mit seinem MVPD im Popup-Fenster eine Umleitung der übergeordneten Seite erzwingt, die von Internet Explorer als Popup-Blocker betrachtet wird. Das Adobe Pass-Authentifizierungsteam **empfiehlt die Implementierung der iFrame-Anmeldung für Internet Explorer**.

Der in dieser Technote vorgestellte Beispielcode verwendet eine hybride Implementierung von iFrame und Popup - Öffnen eines iFrame in Internet Explorer und ein Popup in den anderen Browsern.

Wenn man bedenkt, dass bereits eine iFrame-Implementierung vorhanden ist, zeigt der erste Teil der Technote den Code für die iFrame-Implementierung und der zweite Teil die Änderungen an, um die Popup-Implementierung als Standard aufzunehmen.


## MVPD-Auswahl mit Anmeldeseite in einem iFrame {#mvpd-pickr-iframe}

Frühere Codebeispiele zeigten eine HTML-Seite, die die &lt;div> -Tag, in dem der iFrame zusammen mit der Schließen-iFrame-Schaltfläche erstellt werden soll:

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

Hier ist die zugehörige **JavaScript** code:

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

Da wir keine **iFrame** Der HTML-Code enthält nicht mehr den iFrame oder die Schaltfläche zum Schließen des iFrames. Das div, das zuvor den iFrame enthielt: **mvpddiv** - wird aufbewahrt und für Folgendes verwendet:

* , um den Benutzer darüber zu informieren, dass die MVPD-Anmeldeseite bereits geöffnet ist, wenn der Popup-Fokus verloren geht
* um einen Link bereitzustellen, um den Fokus wieder auf das Popup zu lenken

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

Die Liste der MVPDs wird im div angezeigt, **Picker** als Auswahl **-mvpdList**.

Es wird ein neuer API-Rückruf verwendet: **setConfig(configXML)**. Der Rückruf wird ausgelöst, nachdem die Funktion setRequestor(requestorID) aufgerufen wurde. Dieser Rückruf gibt die Liste der MVPDs zurück, die in die zuvor festgelegte requestorID integriert sind. In der Callback-Methode wird die eingehende XML analysiert und die Liste der MVPDs zwischengespeichert. Die MVPD-Auswahl wird ebenfalls erstellt, aber nicht angezeigt.

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

Nachdem die Funktion getAuthentication() oder getAuthorization() aufgerufen wurde, wird der Rückruf displayProviderDialog() ausgelöst. Normalerweise wäre innerhalb dieses Rückrufs die MVPD-Liste erstellt und angezeigt worden. Da die MVPD-Auswahl bereits erstellt wurde, muss sie nur noch dem Benutzer angezeigt werden.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

Nachdem der Benutzer einen MVPD aus der Auswahl ausgewählt hat, muss das Popup erstellt werden. Einige Browser blockieren das Popup möglicherweise, wenn es mit about:blank oder einer Seite erstellt wird, die sich in einer anderen Domäne befindet. Daher wird empfohlen, es mit dem Hostnamen zu öffnen, von dem aus der AccessEnabler geladen wird.

In der iFrame-Implementierung wurde das Zurücksetzen des Authentifizierungsflusses durch die btnCloseIframe-Schaltfläche und die JavaScript-Funktion closeIframeAction() durchgeführt, aber das Dekorieren des iFrame ist jetzt nicht mehr möglich. Dasselbe Verhalten wird also erreicht, indem beobachtet wird, wann das Popup geschlossen wird (entweder durch den Benutzer oder durch Beenden des Authentifizierungsflusses). Es wurde ein Codeausschnitt hinzugefügt, der auch hilft, wenn der Benutzer den Fokus auf das Popup verliert:

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

Im Rückruf createIFrame() wird die **mvpddiv** div wird angezeigt.

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
>* Der Beispielcode enthält eine fest codierte Variable für die verwendete Anforderer-ID - &quot;REF&quot;, die durch eine reale Programmierer-Anforderer-ID ersetzt werden sollte.
>* Der Beispielcode wird nur ordnungsgemäß von einer auf die Whitelist gesetzten Domäne ausgeführt, die mit der verwendeten Anforderer-ID verknüpft ist.
>* Da der gesamte Code zum Herunterladen verfügbar ist, wurde der in dieser Technote vorgestellte Code abgeschnitten. Ein vollständiges Beispiel finden Sie unter **JS-iFrame vs. Popup-Beispiel**.
>* Die externen JavaScript-Bibliotheken wurden aus [Von Google gehostete Dienste](https://developers.google.com/speed/libraries/).
