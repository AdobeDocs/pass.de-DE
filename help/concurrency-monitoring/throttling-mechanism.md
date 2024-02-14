---
title: Drosselmechanismus
description: Drosselmechanismus
source-git-commit: 1390c09d3de6b4608cdcc97b3f053cce71e84639
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 1%

---


# Drosselmechanismus {#throttling-mechanism}

## Einführung {#introduction}

Adobe muss in seiner Rolle als Datenverarbeiter geeignete Maßnahmen ergreifen, um sicherzustellen, dass die Benutzer unserer Kunden Ressourcen gerecht nutzen und der Dienst nicht mit unnötigen API-Anfragen überflutet wird. Dazu haben wir einen Drosselmechanismus eingerichtet.\
Eine Anwendung zur Überwachung der Parallelität kann von mehreren Benutzern verwendet werden und ein Benutzer kann über mehrere Sitzungen verfügen. Daher hat der Dienst innerhalb eines bestimmten Zeitraums Beschränkungen für die Anzahl der zulässigen Aufrufe pro Benutzer/Sitzung.\
Wenn das Limit erreicht wurde, werden die Anforderungen mit einem bestimmten Antwortstatus markiert (HTTP 429 Zu viele Anforderungen). Jeder nachfolgende Aufruf, der nach Erhalt der Antwort &quot;429 Zu viele Anforderungen&quot;erfolgt, sollte mit einer Abklingzeit von mindestens 1 Minute erfolgen, um sicherzustellen, dass eine gültige Geschäftsantwort eingeht.

## Mechanik - Übersicht {#mechanism-overview}

Der Mechanismus bestimmt die maximale Anzahl der zulässigen Aufrufe für jeden Endpunkt der Überwachung der Parallelität innerhalb eines bestimmten Zeitintervalls.
Sobald diese maximale Anzahl von Anrufen erreicht ist, wird unser Dienst mit &#39;429 Zu viele Anfragen&#39; antworten. Der Dienst benötigt weitere 60 Sekunden, um die Begrenzung erneut auf ihren Maximalwert zu initialisieren.

Die mit Einschränkungen konfigurierten Endpunkte sind:
1. Neue Sitzung erstellen: POST /sessions/{idp}/{subject}
2. Heartbeat-Aufruf: POST /sessions/{idp}/{subject}/{sessionId}
3. Beenden einer Sitzung: DELETE /sessions/{idp}/{subject}/{sessionId}

Die Einschränkungen werden auf zwei Ebenen konfiguriert:
1. Sitzung: dieselbe eindeutige {sessionId} Parameter gesendet `Heartbeat` und `Terminate a session` aufrufen.
2. user: same eindeutige {subject} Parameter gesendet `Create a new session` aufrufen.

Das Limit für die Einschränkungen auf Sitzungsebene ist auf 200 Anforderungen innerhalb einer Minute festgelegt.\
Das Limit für die Einschränkungen auf Benutzerebene ist auf 200 Anforderungen innerhalb einer Minute festgelegt.\
Diese beiden Einschränkungen (Einschränkungen auf Sitzungsebene und Einschränkungen auf Benutzerebene) sind konfigurierbar und wir werden sie aktualisieren, falls sie durch gültige Integrationsszenarien erreicht werden. Zu diesem Zweck empfehlen wir, das Supportteam zu kontaktieren.

Hier ist ein Szenario für die Beschränkung auf Sitzungsebene:

| Zeit | Senden einer Anfrage an CM | Anzahl der Anfragen | Antwort von CM | Erklärung |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Zweiter 10 | POST /sessions/idp1/subject1/session1 | 50 | Alle Anrufe erhalten &quot;202 Akzeptiert&quot; | 50 vom Limit aus verbrauchte Aufrufe |
| Second 50 | POST /sessions/idp1/subject1/session1 | 151 | 150 Aufrufe erhalten &quot;202 Akzeptiert&quot;und 1 Aufruf erhält &quot;429 Zu viele Anfragen&quot; | 200 vom Limit verbrauchte Aufrufe und 1 Aufruf erhält 429 Antworten |
| Second 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 Aufruf erhält &quot;429 Zu viele Anfragen&quot; | Noch keine Aufrufe im Limit verfügbar |
| Second 70 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 Aufruf erhält &quot;202 Akzeptiert&quot; | Auf 200 verfügbare Aufrufe begrenzen, da seit dem zweiten 10. Lebensjahr 60 Sekunden vergangen sind |

und ein Szenario für die Einschränkungen auf Benutzerebene:

| Zeit | Senden einer Anfrage an CM | Anzahl der Anfragen | Antwort von CM | Erklärung |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Zweiter 10 | POST /sessions/idp1/subject1 | 50 | 50 Anrufe erhalten &quot;202 Akzeptiert&quot; | 50 vom Limit aus verbrauchte Aufrufe |
| Second 50 | POST /sessions/idp1/subject1 | 151 | 150 Aufrufe erhalten &quot;202 Akzeptiert&quot;und 1 Aufruf erhält &quot;429 Zu viele Anfragen&quot; | 200 vom Limit verbrauchte Aufrufe und 1 Aufruf erhält 429 Antworten |
| Second 61 | POST /sessions/idp1/subject1 | 1 | 1 Aufruf erhält &quot;429 Zu viele Anfragen&quot; | Noch keine Aufrufe im Limit verfügbar |
| Second 70 | POST /sessions/idp1/subject1 | 1 | 1 Aufruf erhält &quot;202 Akzeptiert&quot; | Auf 200 verfügbare Aufrufe begrenzen, da seit dem zweiten 10. Lebensjahr 60 Sekunden vergangen sind |


## Empfehlungen zur Kundenintegration {#customer-integration-recommendations}

Bei einer korrekten Implementierung erhalten die Kunden keine Antwort &quot;429 Zu viele Anforderungen&quot;.\
Adobe empfiehlt jedoch, dass jeder Kunde die Antwort &quot;429 Zu viele Anforderungen&quot;entsprechend mit den oben genannten technischen Details verarbeitet.
