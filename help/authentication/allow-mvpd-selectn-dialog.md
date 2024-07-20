---
title: MVPDs im Dialogfeld "Auswahl"zulassen
description: MVPDs im Dialogfeld "Auswahl"zulassen
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 0%

---

# MVPDs im Dialogfeld &quot;Auswahl&quot;zulassen {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Problem {#issue}

Der Programmierer kann die Benutzererfahrung neuer MVPD-Integrationen testen oder überprüfen wollen, bevor er an Endbenutzer veröffentlicht wird.

## Lösung {#solution}

Im Rückruf `displayProviderDialog()` gibt die Adobe Pass-Authentifizierung alle mit dem ausgewählten Programmierer (Anforderer-ID) integrierten MVPDs zurück. Der Programmierer kann jedoch einen Filter auf das Rückgabe-Array von MVPDs anwenden und nur diejenigen anzeigen, die sich in beiden Listen befinden.

## Beispiel {#example}

In diesem Beispiel wird gezeigt, wie nur CableCompany_1 und CableCompany_2 im MVPD-Auswahldialogfeld angezeigt werden und CableCompany_NewIntegration nicht angezeigt wird.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
