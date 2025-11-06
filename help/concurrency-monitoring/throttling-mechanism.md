---
title: Drosselmechanismus
description: Drosselmechanismus
exl-id: 15236570-1a75-42fb-9bba-0e2d7a59c9f6
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# Drosselmechanismus {#throttling-mechanism}

## Einführung {#introduction}

Adobe muss in seiner Rolle als Auftragsverarbeiter geeignete Maßnahmen ergreifen, um sicherzustellen, dass die Benutzer unserer Kunden die Ressourcen gerecht nutzen und der Service nicht mit unnötigen API-Anfragen überschwemmt wird. Dazu haben wir einen Drosselungsmechanismus eingerichtet.
Eine Anwendung zur Überwachung gleichzeitiger Nutzung kann von mehreren Benutzern verwendet werden, und ein Benutzer kann mehrere Sitzungen haben. Daher sind für den Service Limits für die Anzahl der akzeptierten Aufrufe pro Benutzer/Sitzung innerhalb eines bestimmten Zeitintervalls konfiguriert.
Wenn das Limit erreicht wurde, werden die Anfragen mit einem bestimmten Antwortstatus markiert (HTTP 429: Zu viele Anfragen). Jeder nachfolgende Aufruf, der nach Eingang einer „429-zu-viele-Anfragen“-Antwort erfolgt, sollte mit mindestens einer Minute Abkühlzeit erfolgen, um sicherzustellen, dass eine gültige Geschäftsantwort eingeht.

## Mechanism - Übersicht {#mechanism-overview}

Der Mechanismus bestimmt die maximale Anzahl akzeptierter Aufrufe für jeden Gleichzeitigkeitsüberwachungs-Endpunkt innerhalb eines bestimmten Zeitintervalls.
Sobald diese maximale Anzahl von Anrufen erreicht ist, wird unser Service mit „429 Zu viele Anfragen“ antworten. Die 429-Antwort-Kopfzeile „Expires“ enthält den Zeitstempel, wann der nächste Aufruf als gültig betrachtet würde oder wann die Drosselung abläuft. Derzeit läuft die Drosselung nach einer ab   Minute ab der ersten Antwort von 429.

Die mit Einschränkungen konfigurierten Endpunkte sind:
1. Neue Sitzung erstellen: POST /sessions/{idp}/{subject}
2. Heartbeat-Aufruf: POST /sessions/{idp}/{subject}/{sessionId}
3. Beenden einer Sitzung: DELETE /sessions/{idp}/{subject}/{sessionId}

Die Drosselung wird auf zwei Ebenen konfiguriert:
1. Sitzung: Derselbe eindeutige {sessionId}, der in `Heartbeat` Aufruf und `Terminate a session` Aufruf gesendet wird.
2. Benutzer: Derselbe eindeutige {subject}, der bei `Create a new session` Aufruf gesendet wurde.

Das Limit für Einschränkungen auf Sitzungsebene ist auf 200 Anfragen innerhalb einer Minute festgelegt.\
Das Limit für Einschränkungen auf Benutzerebene ist auf 200 Anfragen innerhalb einer Minute festgelegt.\
Beide Limits (Einschränkungen auf Sitzungsebene und Einschränkungen auf Benutzerebene) sind konfigurierbar und werden aktualisiert, falls sie über gültige Integrationsszenarien erreicht werden. Dazu empfehlen wir, sich an das Support-Team zu wenden.

**Szenario für Einschränkungen auf Sitzungsebene:**

| Uhrzeit | Anforderung an CM senden | Anzahl der Anfragen | Antwort von CM erhalten | Erklärung |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Zweite 10 | POST /sessions/idp1/subject1/session1 | 50 | Alle Anrufe erhalten „202 Accepted“ | Vom Limit verbrauchte 50 Aufrufe |
| Zweite 50 | POST /sessions/idp1/subject1/session1 | 151 | 150 Anrufe erhalten „202 Akzeptiert“ und 1 Anruf erhält „429 Zu viele Anfragen“ | 200 Aufrufe werden vom Limit verbraucht, und 1 Aufruf erhält eine Antwort von 429 |
| Zweite 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 Anruf erhält „429 Zu viele Anfragen“ | Noch keine Aufrufe im Limit verfügbar |
| Zweite 70 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 Aufruf erhält „202 Accepted“ | Beschränkung auf 200 verfügbare Aufrufe, da seit Sekunde 10 60 Sekunden vergangen sind |

**Szenario für Einschränkungen auf Benutzerebene:**

| Uhrzeit | Anforderung an CM senden | Anzahl der Anfragen | Antwort von CM erhalten | Erklärung |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Zweite 10 | POST /sessions/idp1/subject1 | 50 | 50 Anrufe erhalten „202 Angenommen“ | Vom Limit verbrauchte 50 Aufrufe |
| Zweite 50 | POST /sessions/idp1/subject1 | 151 | 150 Anrufe erhalten „202 Akzeptiert“ und 1 Anruf erhält „429 Zu viele Anfragen“ | 200 Aufrufe werden vom Limit verbraucht, und 1 Aufruf erhält eine Antwort von 429 |
| Zweite 61 | POST /sessions/idp1/subject1 | 1 | 1 Anruf erhält „429 Zu viele Anfragen“ | Noch keine Aufrufe im Limit verfügbar |
| Zweite 70 | POST /sessions/idp1/subject1 | 1 | 1 Aufruf erhält „202 Accepted“ | Beschränkung auf 200 verfügbare Aufrufe, da seit Sekunde 10 60 Sekunden vergangen sind |

Beispiel einer **429-Antwort:**

```
HTTP/2 429
date: Thu, 15 Feb 2024 07:54:20 GMT
content-length: 0
vary: Origin
vary: Access-Control-Request-Method
vary: Access-Control-Request-Headers
cache-control: no-store
expires: Thu, 15 Feb 2024 07:54:41 GMT
strict-transport-security: max-age=31536000 ; includeSubDomains
x-xss-protection: 1; mode=block
x-frame-options: DENY
x-content-type-options: nosniff
```

## Empfehlungen zur Kundenintegration {#customer-integration-recommendations}

Bei korrekter Implementierung erhalten die Kunden keine Antwort „429 Zu viele Anfragen“.
Adobe empfiehlt jedoch, dass jeder Kunde die Antwort „429 Zu viele Anfragen“ entsprechend den oben beschriebenen technischen Details verarbeitet. Bei der Verarbeitung der Antwort sollte der Header „Läuft ab“ verwendet werden, um zu bestimmen, wann die nächste gültige Anfrage gesendet werden soll.
