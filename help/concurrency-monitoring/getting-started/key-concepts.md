---
title: Schlüsselkonzepte
description: Lernen Sie die grundlegenden Konzepte der Parallelitätsüberwachung kennen, einschließlich Sitzungen, Richtlinien, Metadaten und mehr
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# Schlüsselkonzepte {#key-concepts}

Für eine erfolgreiche Implementierung ist es von entscheidender Bedeutung, die Kernkonzepte der Parallelitätsüberwachung zu verstehen. In diesem Handbuch werden die grundlegenden Bausteine und ihre Funktionsweise erläutert.

## Grundbegriffe {#core-concepts}

### Sitzungen

Eine **Sitzung** stellt eine aktive Video-Streaming-Instanz dar. Stellen Sie sich dies als ein „Ticket“ vor, mit dem ein Benutzer Inhalte ansehen kann.

#### Sitzungslebenszyklus

1. **Erstellung** - Wenn Benutzende beginnen, Inhalte anzuzeigen
2. **Aktiv** - Während der Benutzer zusieht (durch Heartbeats verwaltet)
3. **Beendigung** - Wenn der Benutzer die Überwachung beendet oder die Sitzung abläuft

#### Sitzungseigenschaften

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### Richtlinien

**Richtlinien** sind Geschäftsregeln, die Einschränkungen und Einschränkungen für gleichzeitiges Streaming definieren. Sie bestimmen, wann Benutzer neue Streams starten können.

#### Richtlinienkomponenten

- **Regeln** - Spezifische Beschränkungen und Bedingungen
- **Metadatenanforderungen** - Welche Daten werden benötigt?
- **Auswertungslogik** - Wie die Richtlinie angewendet wird

#### Beispielrichtlinie

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### Metadaten

**Metadaten** bietet Kontext zu jeder Sitzung. Es enthält Informationen über das Gerät, den Inhalt, den Benutzer und die Anwendung.

#### Metadatenkategorien

| Kategorie | Beispiele | Zweck |
|-----------------|------------------------------------------|---------------------------------|
| **Gerät** | `deviceId`, `deviceType`, `osName` | Geräte identifizieren und kategorisieren |
| **Inhalt** | `channel`, `contentType`, `assetId` | Beschreiben, was beobachtet wird |
| **Benutzer** | `subject`, `subscriptionTier` | Benutzerspezifische Informationen |
| **Anwendung** | `applicationName`, `applicationPlatform` | App-spezifische Details |
| **Standort** | `country`, `hba` | Geografische und Netzwerkinformationen |

#### Erforderliche und optionale Metadaten

- **Erforderliche Metadaten** - Muss für die Sitzungserstellung angegeben werden
- **Optionale Metadaten** - Kann für erweiterte Richtlinien bereitgestellt werden
- **Standard-Metadaten** - Vordefinierte Felder, die für alle Programme verfügbar sind
- **Benutzerdefinierte Metadaten** - Von Ihnen definierte anwendungsspezifische Felder

## Identität und Authentifizierung {#identity-and-authentication}

### Identitätsanbieter (IdP)

Ein **Identitätsanbieter** ist der Dienst, der Benutzer authentifiziert und ihre Identitätsinformationen bereitstellt. Bei der Adobe Pass-Integration handelt es sich normalerweise um eine MVPD.

#### IdP-Beispiele

- Kabelunternehmen (Comcast, Charter usw.)
- Satellitenanbieter (DirectTV, DISH)
- Streaming-Dienste (Netflix, Hulu)
- Mobilnetzbetreiber (Verizon, AT&amp;T)

### Subjekt

Ein **Subjekt** ist die eindeutige Kennung für einen Benutzer in einem Identitätsanbieter. Dies ist normalerweise die Konto-ID oder Abonnenten-ID des Benutzers.

## Sitzungsverwaltung {#session-management}

### Heartbeat

**Heartbeats** sind periodische API-Aufrufe, die Sitzungen aktiv halten. Sie sagen dem System: „Dieser Benutzer sieht immer noch zu.“

#### Heartbeat-Anforderungen

- **Häufigkeit** - Alle 60 Sekunden (empfohlen)
- **Zweck** - Sitzung am Leben halten, Metadaten aktualisieren
- **Beendigung** - Sitzung läuft ab, wenn Heartbeats anhalten


### Session Termination

Sitzungen können auf verschiedene Weise beendet werden:

1. **Benutzer beobachtet nicht mehr** - Anwendung ruft DELETE-Endpunkt auf
2. **Sitzung läuft ab** - Kein Heartbeat innerhalb der Zeitüberschreitung empfangen
3. **Richtlinienverletzung** - Neue Sitzung beendet die alte (FIFO)
4. **Remote Termination** - Ein anderes Gerät beendet die Sitzung

## Richtlinienauswertung {#policy-evaluation}

### Funktionsweise von Richtlinien

1. **Benutzer fordert neuen Stream an**
2. **System wertet Richtlinien** der aktuellen Nutzung aus
3. **Entscheidung getroffen** - Zulassen oder Ablehnen
4. **Antwort gesendet** - Erfolgs- oder Konfliktinformationen

## Konfliktlösung {#conflict-resolution}

### Was ist ein Konflikt?

Ein **Konflikt** tritt auf, wenn ein Benutzer versucht, einen neuen Stream zu starten, aber seine Beschränkungen überschreiten würde.

### Konfliktbehandlung

Wenn ein Konflikt auftritt, gibt das System eine 409-Konfliktantwort mit detaillierten Informationen zurück:

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
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### Lösungsstrategien

#### LIFO (Last In, First Out)

- **Alte Sitzungen sind geschützt**
- **Neue Sitzung ist blockiert**
- **Einfachere Benutzeroberfläche**
- **Besser für erweiterte Anzeige**


#### FIFO (First In, First Out)

- **Neue Sitzung beendet eine bestehende**
- **Der Benutzer wählt die zu stoppende Sitzung aus**
- **Komplexere Benutzeroberfläche erforderlich**
- **Besser für Inhaltswechsel**

## Anwendungen und Mandanten {#applications-and-tenants}

### Anwendung

Ein **Programm** ist ein Softwareprogramm, das mit der gleichzeitigen Überwachung integriert ist. Jede Anwendung verfügt über eine eindeutige ID und kann über eigene Richtlinien verfügen.

#### Anwendungsbeispiele

- Mobile App (iOS/Android)
- Web-Anwendung
- Smart-TV-App
- Set-Top-Box-Anwendung

### Mandant

Ein **Mandant** ist eine Organisation, der eine oder mehrere Anwendungen gehören. Mandanten können Richtlinien programmübergreifend freigeben.

#### Mandantenstruktur

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## Umgebungen {#environments}

### Staging-Umgebung

- **Zweck** - Entwicklung und Tests
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **Anmeldeinformationen** - Anwendungs-IDs testen
- **Data** - Nur Testdaten

### Produktionsumgebung

- **Zweck** - Live-Benutzer-Traffic
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **Anmeldeinformationen** - Produktions-Anwendungs-IDs
- **Data** - Echtzeit-Benutzerdaten

## Integrationsfluss {#integration-flow}

### Grundlegender Ablauf

1. **Benutzerauthentifizierung** - Mit Adobe Pass authentifizieren
2. **Sitzungserstellung** - Erstellen einer CM-Sitzung mit Benutzermetadaten
3. **Heartbeat-Verwaltung** - Senden von regelmäßigen Heartbeats
4. **Sitzungsbeendigung** - Beenden, wenn der Benutzer das Beobachten beendet

### Fehlerbehandlung

1. **404 Not Found** - Verarbeiten von Anfragen mit Sitzungs-ID, die nicht vom CM-Service generiert wurden
2. **409 Conflicts** - Umgang mit Richtlinienverletzungen
3. **410 nicht mehr verfügbar** - Sitzungsende verwalten
4. **Netzwerkfehler** - Beheben von Verbindungsproblemen
5. **Authentifizierungsfehler** - Probleme mit Berechtigungen beheben


## Allgemeine Terminologie {#common-terminology}

| Begriff | Definition |
|-----------------|--------------------------------------------|
| **Sitzung** | Aktive Video-Streaming-Instanz |
| **Richtlinie** | Geschäftsregel zur Definition von Nutzungsbeschränkungen |
| **Metadaten** | Kontextuelle Informationen zu Sitzungen |
| **IDP** | Identitätsanbieter (Authentifizierungsdienst) |
| **Betreff** | Benutzerkennung in einem IDP |
| **Heartbeat** | Periodischer Aufruf, um die Sitzung aktiv zu halten |
| **Konflikt** | Richtlinienverletzung beim Starten eines neuen Streams |
| **LIFO** | Last In, First Out Konfliktlösung |
| **FIFO** | First In, First Out Konfliktlösung |
| **MANDANT** | Organisation, die Programme besitzt |
| **Anwendung** | Software-Programm mit CM |

