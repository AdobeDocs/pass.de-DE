---
title: Vermeiden Sie die Verwendung von '&'reg_code' in der /authentication-Anfrage
description: Vermeiden Sie die Verwendung von '&'reg_code' in der /authentication-Anfrage
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# (Legacy) Verwenden Sie den &quot;&amp;„reg_code“ nicht in der /authentication-Anfrage {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>



## Problem

Der IE 9-Browser interpretiert &#39;\&amp;reg&#39; als speziellen Befehl und konvertiert ihn in ®.

## Erklärung

Wenn die `/authenticate` Anfrage wie folgt aufgebaut ist…


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


…es wird vom IE-Browser wie unten interpretiert und in diesem Format an Adobe gesendet:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


Die Requestor\_id wird als univision®\_code=EKAFMFI interpretiert, da kein &#39;&amp;&#39; vorhanden ist, und Adobe findet keinen `regCode`, mit dem das Token verknüpft werden soll.  Es besteht die Möglichkeit, dass das AuthN-Token überhaupt nicht erstellt wird. In diesem Fall können `/checkauthn`-Aufrufe keine Token finden.



## Lösung

Eine der folgenden Optionen sollte dieses Problem lösen:

1. Vermeiden Sie die Verwendung des `&reg_code`-Parameters zwischen den anderen Abfragezeichenfolgenparametern.  Verschieben Sie sie stattdessen in den ersten Abfragezeichenfolgenparameter in der Anfrage-URL, wodurch die Anfrage-URL wie folgt aussieht:


       &lt;FQDN>authentifizieren?reg_code =EKAFMFI&amp;Requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   Auf diese Weise wird der `&reg` nicht falsch interpretiert.

1. Normalisieren Sie `&reg_code` wie mit `&amp;reg_code`.

1. Adobe könnte eine neue Funktion einführen, um als Reaktion auf einen Authentifizierungsaufruf einen Fehlercode zurück an den 2. Bildschirm zu senden, wenn die Erstellung des AuthN-Tokens fehlgeschlagen ist.
