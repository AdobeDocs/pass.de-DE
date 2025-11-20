---
title: API-Referenz - Übersicht
description: Vollständige Referenz für die Parallelitätsüberwachungs-API einschließlich Endpunkten, Authentifizierung und Antwortformaten
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# API-Referenz - Übersicht {#api-reference-overview}

Die Parallelitätsüberwachungs-API bietet eine RESTful-Schnittstelle zum Verwalten von Streaming-Sitzungen und Durchsetzen von Richtlinien für die gleichzeitige Verwendung. Diese Referenz bietet eine vollständige Dokumentation für alle Endpunkte, Authentifizierungsmethoden, Anfrage-/Antwortformate und Fehlerbehandlung.

## API-Basis-URLs

### Produktionsumgebung

```
https://streams.adobeprimetime.com/v2/
```

### Staging-Umgebung

```
https://streams-stage.adobeprimetime.com/v2/
```

**Hinweis:** Verwenden Sie immer die Staging-Umgebung für Entwicklung und Tests. Produktionsberechtigungen werden nur nach erfolgreicher Staging-Integration bereitgestellt.

## Authentifizierung

Alle API-Aufrufe erfordern eine HTTP-Standardauthentifizierung mit den Anmeldeinformationen Ihrer Anwendung:

- **Benutzername:** Ihre Anwendungs-ID (bereitgestellt von Adobe)
- **Kennwort:** Leere Zeichenfolge

### Beispiel einer Authentifizierungs-Kopfzeile

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## Standards für Antwortformate

### Erfolgsantworten

Alle erfolgreichen Antworten folgen dieser Struktur:

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Fehlerantworten

Alle Fehlerantworten folgen dieser Struktur:

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### Ergebnisformat der Auswertung

Bei der Bewertung von Richtlinien (insbesondere für 409 Konflikte) enthalten die Antworten ein Bewertungsergebnis:

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Gängige HTTP-Status-Codes

| Code | Beschreibung | Bei Rückgabe |
|------|----------------------|------------------------------------------------|
| 200 | OK | Erfolgreiche GET-Anfragen |
| 202 | Erstellt/Akzeptiert | Erfolgreiche Sitzungserstellung/aufgezeichneter Heartbeat |
| 400 | Fehlerhafte Anfrage | Ungültige Parameter oder fehlende erforderliche Felder |
| 401 | Nicht autorisiert | Ungültige oder fehlende Authentifizierung |
| 403 | Verboten | Unzureichende Berechtigungen |
| 404 | Sitzungs-ID nicht gefunden | Sitzungs-ID nicht vom CM-Service generiert |
| 409 | Konflikt | Richtlinienverletzung (gleichzeitiges Limit erreicht) |
| 410 | Abgelaufen | Sitzung abgelaufen oder wurde beendet |
| 429 | Zu viele Anfragen | Ratenlimit überschritten |

## Übergabemethoden für Parameter

### Pfadparameter

Erforderliche Parameter, die Teil des URL-Pfads sind:

1. `{idp}` - Identitäts-Provider-Kennung
2. `{subject}` - Benutzerkennung (normalerweise aus Adobe Pass)
3. `{sessionId}` - Sitzungskennung (in der Location-Kopfzeile zurückgegeben)

### Zusätzliche Parameter

Optionale Parameter werden in der URL übergeben:

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### Formulardaten (POST/PUT)

Metadaten und Sitzungsdaten im Anfrageinhalt:

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### Kopfzeilen

Spezielle Parameter, die in HTTP-Headern übergeben werden:

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## Best Practices für die Fehlerbehandlung

### 409 Konfliktbehandlung

Wenn Sie eine 409-Konflikt-Antwort erhalten:

1. **Analysieren Sie das Bewertungsergebnis** um die Richtlinienverletzung zu verstehen
2. **Extrahieren von Konfliktinformationen** aus `associatedAdvice`
3. **Präsentieren von Optionen für den Benutzer** basierend auf Ihrer LIFO/FIFO-Strategie
4. **Verwenden von Terminations-Codes** wenn LIFO-Verhalten implementiert wird

### 410 Nicht mehr verfügbar

Wenn Sie eine „410 Gone“-Antwort erhalten:

1. **Überprüfen, ob die Antwort einen Textkörper enthält** - zeigt eine Remote-Beendigung an.
2. **Einlesehinweise** um zu verstehen, warum die Sitzung beendet wurde
3. **Benutzeroberfläche aktualisieren** um das Ende der Sitzung widerzuspiegeln
4. **Anmutig behandeln** - Die Sitzung hat möglicherweise eine natürliche Zeitüberschreitung erfahren.
5. **Neue Sitzung starten** - Starten Sie eine neue Sitzung, falls dies angemessen erscheint

### Ratenbegrenzung

Wenn Sie eine 429-zu-viele-Anfrage erhalten:

1. **Anruffrequenz** auf bis zu 200 Anfragen pro Minute beschränken, was der von CM akzeptierte Maximalwert ist
2. **Senden Sie Heartbeats im erforderlichen Intervall** von 1 pro Minute.

## Test-Tools

### Interactive API Explorer

Verwenden Sie unsere [Swagger-Benutzeroberfläche](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) für interaktive Tests:

1. Geben Sie Ihre Anwendungs-ID oben rechts ein
2. Auf „Erkunden“ klicken, um die Authentifizierung festzulegen
3. Testen von Endpunkten mit echten Parametern
4. Beispiele für Anfragen/Antworten anzeigen
