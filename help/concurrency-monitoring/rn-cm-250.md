---
title: Versionshinweise zur Überwachung der Parallelität mit Adobe Pass 2.5.0
description: Versionshinweise zur Überwachung der Parallelität mit Adobe Pass 2.5.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---


# Versionshinweise zur Überwachung der Parallelität mit Adobe Pass 2.5.0 {#cm-250}

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme in dieser Version beschrieben:

Releasedatum: 21.04.2016

## Neue Funktionen {#new-features}

Die Concurrency Monitoring API v2.0 ist jetzt in der Produktion verfügbar.

Die V2-Version vereinheitlicht Heartbeat- und Abfrageaufrufe und vereinfacht die Implementierung erheblich, wenn beide APIs gleichzeitig verwendet werden.



### Sitzungslebenszyklus {#session-lifecycle}

* Schreibgeschützte APIs zum Erstellen, Keep-Alive und Beenden einer Sitzung - d. h. keine Abfragen mehr, die Entscheidung wird sowohl bei Sitzungsinit- als auch bei Heartbeat-Aufrufen zurückgegeben.
* Das Heartbeat-Intervall wird jetzt vom Server über die Abläufe-Header gesteuert, die sowohl für die Sitzungsinit- als auch für Keep-Alive-Aufrufe festgelegt werden. Dies ermöglicht die serverseitige Konfiguration von Heartbeat-Intervallen und einem weniger harcodierten Wert in den Apps.
* Der glückliche Pfad (konformes Verhalten des Benutzers und der App) ist mit 2XX-Status-Codes &quot;gepflastert&quot;.

### Umgang mit Fehlern {#error-handling}

* Ablehnungsentscheidungen, fehlende Metadaten oder fehlendes Anwendungsverhalten werden als 4XX-Antworten gekennzeichnet (Konflikt, ungültige Anforderung, Nicht autorisiert, Verschwinden usw.)

* Wann immer dies sinnvoll ist, enthält die Antwort Folgendes:

   * dazugehörige Beratung - detaillierte Erläuterung(en) des Fehlschlagens, der dem Benutzer angezeigt werden soll.

   * Verpflichtungen - obligatorische Aktionen, die die Anwendung ausführen muss (z. B. Aktualisierung der Metadaten, Abmeldung von Adobe Pass).

### Metadaten {#metadata}

* Standard anstelle benutzerdefinierter Metadaten: Dadurch kann die API erkennen, ob falsche Metadaten gesendet werden oder ob die für Regeln erforderlichen Metadaten fehlen.
* Die erforderlichen Attribute für jede Anwendung werden jetzt durch einen API-Aufruf bereitgestellt und sollten von Clients zwischengespeichert werden.
Hinweise

>[!NOTE]
>
>Die V1-Version ist weiterhin verfügbar. Wenn Kunden Abfragen außerhalb des Heartbeat-Aufrufs verwenden müssen, kann die V1-Version verwendet werden.




### Fehlerkorrekturen {#bug-fixes}

Nicht zutreffend

### Bekannte Probleme {#known-issues}

Nicht zutreffend
