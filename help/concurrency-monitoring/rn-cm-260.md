---
title: Versionshinweise zur Überwachung der Parallelität mit Adobe Pass 2.6.0
description: Versionshinweise zur Überwachung der Parallelität mit Adobe Pass 2.6.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Versionshinweise zur Überwachung der Parallelität mit Adobe Pass 2.6.0 {#cm-260}


Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:



## Releasedatum: 10.11.2016



## Neue Funktionen

Diese Version bietet die Möglichkeit, vorhandene Streams zu beenden, um den Start des aktuellen Streams zu ermöglichen (auch Abbruch-Stream genannt).



**Remote-Terminierung**

* Bei einer 409-Konfliktantwort enthält jede Sitzung, die im Feld &quot;Konflikte&quot;der Beratung aufgeführt ist, das Attribut &quot;termincode&quot;.
* Der Benutzer kann mit der Liste der in Konflikt stehenden Sitzungen aufgefordert werden und die(n) zum Abbrechen auswählen können
* Remote-Sitzungen können nur durch Übergabe eines Anfrage-Headers &quot;X-Terminate&quot;(mit den ausgewählten Terminierungscodes als Werten) innerhalb eines Sitzungsinit-Versuchs beendet werden.
* Für die Antwort &quot;Vorbei 410 - Vorbei&quot;wurde ein neuer Typ von &quot;Beratung&quot;definiert, um die Sitzung anzugeben, bei der die aktuelle Sitzung beendet wurde.


Weitere Informationen finden Sie in der aktualisierten Dokumentation .



>[!NOTE]
>
>Die Sitzungsdefinition, die für die Auflistung aktiver Sitzungen verwendet wird, wurde aktualisiert und enthält nun den Anwendungsnamen und den Gerätenamen (anstelle der Anwendungs-ID).




## Fehlerkorrekturen {#bug-fixes}

Duplizierte Header in der Serverantwort wurden entfernt (die Korrektur betrifft sowohl CORS-Header als auch Datum eins).




## Bekannte Probleme {#known-issues}

Nicht zutreffend
