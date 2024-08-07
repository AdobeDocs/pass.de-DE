---
title: Vermeiden Sie die Verwendung von '&'reg_code in /authenticate request
description: Vermeiden Sie die Verwendung von '&'reg_code in /authenticate request
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Vermeiden Sie die Verwendung von &#39;&amp;&#39;reg_code in /authenticate request {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>



## Problem

Der IE 9-Browser interpretiert &quot;\&amp;reg&quot;als speziellen Befehl und konvertiert ihn in ®.

## Erklärung

Wenn die `/authenticate` -Anfrage wie folgt zusammengestellt wird ...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


... wird es vom IE-Browser wie unten interpretiert und in diesem Format an Adobe gesendet:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


Der Anforderer\_id wird als univision®\_code=EKAFMFI interpretiert, da kein &quot;&amp;&quot;vorhanden ist und Adobe keinen &quot;`regCode`&quot;-Parameter findet, mit dem das Token verknüpft werden soll.  Es besteht die Möglichkeit, dass das AuthN-Token überhaupt nicht erstellt wird. In diesem Fall finden `/checkauthn`-Aufrufe keine Token.



## Lösung

Eine der folgenden Optionen sollte dieses Problem beheben:

1. Vermeiden Sie die Verwendung des `&reg_code` -Parameters zwischen den anderen Abfragezeichenfolgen-Parametern.  Verschieben Sie sie stattdessen in den ersten Abfragezeichenfolgenparameter in der Anforderungs-URL, sodass die Anforderungs-URL wie folgt lautet:


       &lt;FQDN>authenticate?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   Auf diese Weise wird der Parameter `&reg` nicht falsch interpretiert.

1. Normalisieren Sie `&reg_code` mit `&amp;reg_code`.

1. Adobe könnte eine neue Funktion einführen, mit der ein Fehlercode als Reaktion auf einen Authentifizierungsaufruf an den zweiten Bildschirm zurückgesendet werden kann, falls die AuthN-Token-Erstellung fehlgeschlagen ist.
