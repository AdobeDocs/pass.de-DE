---
title: Umgang mit 409-Konflikten
description: Erfahren Sie, wie Sie mit 409-Konflikt-Fehlern umgehen können, wenn gleichzeitige Nutzungsbeschränkungen erreicht werden
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---


# Umgang mit 409-Konflikten {#handling-409-errors}

Wenn ein(e) Benutzende(r) versucht, einen neuen Stream zu starten und eine maximale gleichzeitige Nutzung erreicht, gibt die gleichzeitige Überwachung eine **409-Konflikt** Antwort zurück. Um ein gutes Benutzererlebnis zu bieten, ist es von entscheidender Bedeutung, zu verstehen, wie dieser Fehler behandelt wird.

## Was ist ein 409-Konflikt? {#what-is-a-409-conflict}

Ein 409-Konflikt tritt auf, wenn:

1. **Benutzer versucht, einen neuen Stream zu starten**
2. **Richtlinienauswertung bestimmt** dass die Anfrage die Limits überschreiten würde
3. **Das System gibt 409** mit detaillierten Konfliktinformationen zurück.
4. **Die Anwendung muss entscheiden** wie der Konflikt gehandhabt wird

### 409-Reaktionsstruktur

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "id": "advice-1",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1",
                "contentType": "live"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Grundlegendes zur Antwort {#understanding-the-response}

### Schlüsselfelder

- **`decision`**: Immer „ABLEHNEN“ für 409 Konflikte
- **`associatedAdvice`**: Array von Hinweisobjekten, die den Verstoß erklären
- **`conflicts`**: Liste der aktiven Sitzungen, die den Konflikt verursachen
- **`terminationCode`**: Eindeutiger Code zum Beenden einer bestimmten Sitzung

### Arten von Ratschlägen

#### Hinweis zu Regelverletzungen

```json
{
  "adviceType": "rule-violation",
  "attributes": {
    "rule": "max_streams",
    "threshold": 3,
    "current": 4,
    "conflicts": [...]
  }
}
```

#### Remote-Terminierungsempfehlung

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## Umgangsstrategien {#handling-strategies}

- Im FIFO-Modus kann die neue Sitzung durch Beenden einer bestehenden Sitzung gestartet werden.
- Im LIFO-Modus blockiert CM die neue Sitzung und informiert den Benutzer


## Best Practices {#best-practices}

### &#x200B;1. Benutzerkommunikation löschen

- **Limit erläutern** - Benutzer sollten verstehen, warum sie blockiert werden
- **Verfügbare Optionen anzeigen** - Wie sie den Konflikt lösen können
- **Kontext bereitstellen** - Anzeigen, welche Sitzungen aktiv sind

### &#x200B;2. Ordnungsgemäße Fehlerbehandlung

- **Nicht abstürzen** - Behandeln Sie 409-Fehler auf elegante Weise
- **Alternativen bereitstellen** - Möglichkeiten zur Lösung des Konflikts anbieten
- **Benutzerstatus speichern** - Die Inhaltsauswahl nicht verlieren

### &#x200B;3. Überlegungen zum Benutzererlebnis

- **Schnelle Lösung** - Erleichtern Sie die Lösung von Konflikten
- **Klare Auswahlmöglichkeiten** - Benutzer sollten ihre Optionen verstehen
- **Konsistentes Verhalten** - Konflikte jedes Mal auf die gleiche Weise handhaben

### &#x200B;4. Technische Überlegungen

- **Antwort sorgfältig analysieren** - Alle relevanten Informationen extrahieren
- **Randfälle behandeln** - Was passiert, wenn keine Konflikte zurückgegeben werden?
- **Konflikte protokollieren** - Richtlinienverletzungen für die Analyse verfolgen


