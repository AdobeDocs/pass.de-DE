---
title: Anzeige von MVPDs im Auswahldialogfeld verhindern
description: Anzeige von MVPDs im Auswahldialogfeld verhindern
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 0%

---

# (Legacy) Verhindern, dass MVPDs im Auswahldialogfeld angezeigt werden

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Problem {#issue-prevent-mvpd-sel-dialog}

Sie müssen verhindern, dass bestimmte MVPDs in der MVPD-Auswahl angezeigt werden („Blockierungsliste„).


## Lösung {#solution-prevent-mvpd-sel-dialog}

Die Lösung besteht darin, beim Aufruf von `displayProviderDialog()` eine Blockierungsauflistung vorzunehmen.

Wenn Sie beispielsweise möchten, dass CableCompany_1 und CableCompany_2 nicht in der MVPD-Auswahl angezeigt werden, tun Sie etwas wie das folgende Beispiel.

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
