---
title: Drosselmechanismus
description: Drosselmechanismus
source-git-commit: cbb45cae576332e2b63027992c597b834210988d
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 1%

---


# Drosselmechanismus {#throttling-mechanism}

## Einführung {#introduction}

Adobe muss in seiner Rolle als Datenverarbeiter geeignete Maßnahmen ergreifen, um sicherzustellen, dass die Benutzer unserer Kunden Ressourcen gerecht nutzen und der Dienst nicht mit unnötigen API-Anfragen überflutet wird. Dazu haben wir einen Drosselmechanismus eingerichtet.
Eine Anwendung zur Überwachung der Parallelität kann von mehreren Benutzern verwendet werden und ein Benutzer kann über mehrere Sitzungen verfügen. Daher hat der Dienst innerhalb eines bestimmten Zeitraums Beschränkungen für die Anzahl der zulässigen Aufrufe pro Benutzer/Sitzung.
Wenn das Limit erreicht wurde, werden die Anforderungen mit einem bestimmten Antwortstatus markiert (HTTP 429 Zu viele Anforderungen). Jeder nachfolgende Aufruf, der nach Erhalt der Antwort &quot;429 Zu viele Anforderungen&quot;erfolgt, sollte mit einer Abkühlzeit von mindestens einer Minute erfolgen, um sicherzustellen, dass eine gültige Geschäftsantwort eingeht.

## Mechanik - Übersicht {#mechanism-overview}

Der Mechanismus bestimmt die maximale Anzahl der zulässigen Aufrufe für jeden Endpunkt der Überwachung der Parallelität innerhalb eines bestimmten Zeitintervalls.
Sobald diese maximale Anzahl von Anrufen erreicht ist, wird unser Dienst mit &#39;429 Zu viele Anfragen&#39; antworten. Die Kopfzeile &quot;Läuft ab&quot;der Antwort &quot;429&quot;enthält den Zeitstempel, zu dem der nächste Aufruf als gültig betrachtet wird oder zu dem Zeitpunkt, zu dem die Drosselung abläuft. Derzeit läuft die Drosselung nach einer Minute ab der ersten 429-Antwort ab.

Die mit Einschränkungen konfigurierten Endpunkte sind:
1. Neue Sitzung erstellen: POST /sessions/{idp}/{subject}
2. Heartbeat-Aufruf: POST /sessions/{idp}/{subject}/{sessionId}
3. Beenden einer Sitzung: DELETE /sessions/{idp}/{subject}/{sessionId}

Die Einschränkungen werden auf zwei Ebenen konfiguriert:
1. Sitzung: dieselbe eindeutige {sessionId} Parameter gesendet `Heartbeat` und `Terminate a session` aufrufen.
2. user: same eindeutige {subject} Parameter gesendet `Create a new session` aufrufen.

Das Limit für die Einschränkungen auf Sitzungsebene ist auf 200 Anforderungen innerhalb einer Minute festgelegt.\
Das Limit für die Einschränkungen auf Benutzerebene ist auf 200 Anforderungen innerhalb einer Minute festgelegt.\
Diese beiden Beschränkungen (Einschränkungen auf Sitzungsebene und Einschränkungen auf Benutzerebene) sind konfigurierbar und wir werden sie aktualisieren, falls sie durch gültige Integrationsszenarien erreicht werden. Zu diesem Zweck empfehlen wir, das Supportteam zu kontaktieren.

**Szenario für die Beschränkung auf Sitzungsebene:**

| Zeit | Senden einer Anfrage an CM | Anzahl der Anfragen | Antwort von CM | Erklärung |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Zweiter 10 | POST /sessions/idp1/subject1/session1 | 50 | Alle Anrufe erhalten &quot;202 Akzeptiert&quot; | 50 vom Limit aus verbrauchte Aufrufe |
| Second 50 | POST /sessions/idp1/subject1/session1 | 151 | 150 Aufrufe erhalten &quot;202 Akzeptiert&quot;und 1 Aufruf erhält &quot;429 Zu viele Anfragen&quot; | 200 vom Limit verbrauchte Aufrufe und 1 Aufruf erhält 429 Antworten |
| Second 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 Aufruf erhält &quot;429 Zu viele Anfragen&quot; | Noch keine Aufrufe im Limit verfügbar |
| Second 70 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 Aufruf erhält &quot;202 Akzeptiert&quot; | Auf 200 verfügbare Aufrufe begrenzen, da seit dem zweiten 10. Lebensjahr 60 Sekunden vergangen sind |

**Szenario für die Einschränkungen auf Benutzerebene:**

| Zeit | Senden einer Anfrage an CM | Anzahl der Anfragen | Antwort von CM | Erklärung |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Zweiter 10 | POST /sessions/idp1/subject1 | 50 | 50 Anrufe erhalten &quot;202 Akzeptiert&quot; | 50 vom Limit aus verbrauchte Aufrufe |
| Second 50 | POST /sessions/idp1/subject1 | 151 | 150 Aufrufe erhalten &quot;202 Akzeptiert&quot;und 1 Aufruf erhält &quot;429 Zu viele Anfragen&quot; | 200 vom Limit verbrauchte Aufrufe und 1 Aufruf erhält 429 Antworten |
| Second 61 | POST /sessions/idp1/subject1 | 1 | 1 Aufruf erhält &quot;429 Zu viele Anfragen&quot; | Noch keine Aufrufe im Limit verfügbar |
| Second 70 | POST /sessions/idp1/subject1 | 1 | 1 Aufruf erhält &quot;202 Akzeptiert&quot; | Auf 200 verfügbare Aufrufe begrenzen, da seit dem zweiten 10. Lebensjahr 60 Sekunden vergangen sind |

**429 Antwortbeispiel:**

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

Bei einer korrekten Implementierung erhalten die Kunden keine Antwort &quot;429 Zu viele Anforderungen&quot;.
Adobe empfiehlt jedoch, dass jeder Kunde die Antwort &quot;429 Zu viele Anforderungen&quot;entsprechend mit den oben genannten technischen Details verarbeitet. Bei der Verarbeitung der Antwort sollte die Kopfzeile &quot;Läuft ab&quot;verwendet werden, um zu bestimmen, wann die nächste gültige Anfrage gesendet werden soll.
