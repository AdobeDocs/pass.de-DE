---
title: Versionshinweise zur gleichzeitigen Überwachung von Adobe Pass 2.6.0
description: Versionshinweise zur gleichzeitigen Überwachung von Adobe Pass 2.6.0
exl-id: f24980e3-ffe8-4b5e-8adc-ae443baed40f
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---

# Versionshinweise zur gleichzeitigen Überwachung von Adobe Pass 2.6.0 {#cm-260}


Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:



## Veröffentlichungsdatum: 11/10/2016



## Neue Funktionen

Diese Version bietet die Möglichkeit, vorhandene Streams zu beenden, damit der aktuelle Stream gestartet werden kann (auch als Kill-Stream bezeichnet).



**Remote-Terminierung**

* Bei einer 409-Konflikt-Antwort trägt jede Sitzung, die im Feld „Konflikte“ des Ratschlags aufgeführt ist, das Attribut „terminationCode“.
* Der/die Benutzende kann mit der Liste der widersprüchlichen Sitzungen aufgefordert werden und die zu löschenden auswählen
* Remote-Sitzungen können nur beendet werden, indem ein X-Terminate-Anfrage-Header (mit den ausgewählten Terminations-Codes als Werte) innerhalb eines Sitzungsinitialisierungsversuchs übergeben wird.
* Für die Antwort „410 Gone“ wurde ein neuer Typ von „Hinweis“ definiert, um anzugeben, welche Sitzung die aktuelle beendet hat.


Weitere Informationen finden Sie in der aktualisierten Dokumentation .



>[!NOTE]
>
>Die Sitzungsdefinition, die für die Auflistung aktiver Sitzungen verwendet wird, wurde aktualisiert, um den Anwendungs- und Gerätenamen (anstelle der Anwendungs-ID) einzuschließen.




## Fehlerbehebungen {#bug-fixes}

Doppelte Kopfzeilen in der Server-Antwort wurden entfernt (die Fehlerbehebung betrifft sowohl CORS-Kopfzeilen als auch die Datumskopfzeile).




## Bekannte Probleme {#known-issues}

Nicht zutreffend
