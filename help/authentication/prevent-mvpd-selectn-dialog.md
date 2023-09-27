---
title: Verhindern der Anzeige von MVPDs im Dialogfeld "Auswahl"
description: Verhindern der Anzeige von MVPDs im Dialogfeld "Auswahl"
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---

# Verhindern der Anzeige von MVPDs im Dialogfeld &quot;Auswahl&quot;

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Problem {#issue-prevent-mvpd-sel-dialog}

Sie müssen verhindern, dass bestimmte MVPDs (&quot;Blockierungsliste&quot;) im MVPD-Selektor angezeigt werden.


## Lösung {#solution-prevent-mvpd-sel-dialog}

Die Lösung besteht darin, eine Blockierungsliste zu erstellen, wenn `displayProviderDialog()` aufgerufen wird.

Wenn Sie z. B. möchten, dass CableCompany_1 und CableCompany_2 nicht im MVPD-Selektor angezeigt werden, würden Sie etwas wie das tun, was im folgenden Beispiel gezeigt wird.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```

<!--
**Related Information**

* [Allow MVPDs in the Selection Dialog](/help/authentication/allow-mvpd-selectn-dialog.md)
* **Code samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
