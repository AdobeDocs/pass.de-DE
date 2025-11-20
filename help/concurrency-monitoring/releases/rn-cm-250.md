---
title: Versionshinweise zur gleichzeitigen Überwachung von Adobe Pass 2.5.0
description: Versionshinweise zur gleichzeitigen Überwachung von Adobe Pass 2.5.0
exl-id: da392b18-a2aa-4f51-a75f-2c5b65b2b073
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Versionshinweise zur gleichzeitigen Überwachung von Adobe Pass 2.5.0 {#cm-250}

Auf dieser Seite werden neue Funktionen, Änderungen und bekannte Probleme dieser Version beschrieben:

Veröffentlichungsdatum: 04/21/2016

## Neue Funktionen {#new-features}

Die Parallelitätsüberwachungs-API v2.0 ist jetzt in der Produktion verfügbar.

Die V2-Version vereinheitlicht Heartbeat- und Abfrageaufrufe und vereinfacht die Implementierung erheblich, wenn diese beiden APIs gleichzeitig verwendet werden.



### Sitzungslebenszyklus {#session-lifecycle}

* Schreibgeschützte APIs für die Erstellung, die Keep-Alive und das Beenden einer Sitzung - d. h. keine Abfragen mehr, die Entscheidung wird sowohl bei Sitzungsinit- als auch bei Heartbeat-Aufrufen zurückgegeben
* Das Heartbeat-Intervall wird jetzt vom Server über Expires-Header gesteuert, die sowohl bei Sitzungsinitialisierungs- als auch bei Keep-Alive-Aufrufen festgelegt werden. Dies ermöglicht eine Server-seitige Konfiguration von Heartbeat-Intervallen und einen weniger harcodierten Wert in den Apps.
* Der Happy Path (konformes Verhalten sowohl des Benutzers als auch der App) wird mit 2XX-Status-Codes „gepflastert“

### Fehlerbehandlung {#error-handling}

* Ablehnen von Entscheidungen, fehlende Metadaten oder fehlerhaftes Anwendungsverhalten werden als 4XX-Antworten gekennzeichnet (Konflikt, Fehlerhafte Anfrage, Nicht autorisiert, Nicht mehr verfügbar usw.)

* Wenn es sinnvoll ist, beinhaltet die Antwort:

   * Zugehörige Ratschläge - detaillierte Erklärung(en) des Fehlers, die dem Benutzer mitzuteilen ist.

   * Pflichten - Obligatorische Aktionen, die die Anwendung ausführen muss (z. B.: Metadaten aktualisieren, sich von Adobe Pass abmelden).

### Metadaten {#metadata}

* Standard- statt benutzerdefinierter Metadaten: Auf diese Weise kann die API feststellen, ob falsche Metadaten gesendet werden oder ob die von Regeln benötigten Metadaten fehlen.
* Die erforderlichen Attribute für jede Anwendung werden jetzt durch einen API-Aufruf bereitgestellt und sollten von Clients zwischengespeichert werden.
Notizen

>[!NOTE]
>
>Die Version V1 ist weiterhin verfügbar. Wenn Kundinnen und Kunden Abfragen außerhalb des Heartbeat-Aufrufs verwenden müssen, kann die V1-Version verwendet werden.




### Fehlerbehebungen {#bug-fixes}

Nicht zutreffend

### Bekannte Probleme {#known-issues}

Nicht zutreffend
