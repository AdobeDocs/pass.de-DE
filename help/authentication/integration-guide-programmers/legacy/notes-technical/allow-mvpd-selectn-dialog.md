---
title: Zulassen von MVPDs im Auswahldialogfeld
description: Zulassen von MVPDs im Auswahldialogfeld
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# (Legacy) Zulassen von MVPDs im Auswahldialogfeld {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Problem {#issue}

Der Programmierer möchte möglicherweise das Benutzererlebnis bei neuen MVPD-Integrationen testen oder überprüfen, bevor es für die Endbenutzer veröffentlicht wird.

## Lösung {#solution}

Im `displayProviderDialog()` Callback gibt die Adobe Pass-Authentifizierung alle MVPDs zurück, die in den ausgewählten Programmierer integriert sind (Anforderer-ID). Der Programmierer kann jedoch einen Filter auf das Rückgabe-Array von MVPDs anwenden und nur die anzeigen, die sich in beiden Listen befinden.

## Beispiel {#example}

Dieses Beispiel zeigt, wie im MVPD-Auswahldialogfeld nur CableCompany_1 und CableCompany_2 angezeigt werden und wie die CableCompany_NewIntegration nicht angezeigt wird.

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
