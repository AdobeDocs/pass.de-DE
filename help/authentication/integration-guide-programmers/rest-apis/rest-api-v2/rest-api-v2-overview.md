---
title: REST API v2 - Übersicht
description: REST API v2 - Übersicht
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# REST API v2 - Übersicht {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Möchten Sie die Kosteneffizienz Ihrer TVE-Anwendungen verbessern?

Möchten Sie die Entwicklungszeit und die Ressourcen reduzieren, die zur Unterstützung von TVE-Anwendungen auf mehreren Plattformen erforderlich sind?

Möchten Sie plattformübergreifend ein konsistentes Benutzererlebnis sicherstellen?

Möchten Sie den Wartungsaufwand reduzieren und die Bereitstellung von Aktualisierungen, Fehlerbehebungen oder Verbesserungen vereinfachen?

## Einführung in die REST-API v2

Die Adobe Pass-Authentifizierung freut sich, die Einführung der REST-API V2 bekannt geben zu können, die dazu dient, das Benutzererlebnis zu verbessern und die Integration mit Pass-Services zu vereinfachen.

Wir freuen uns über die Möglichkeiten unserer neuen REST-API, die einen großen Schritt nach vorne für unsere Plattform darstellt und die Tür für neue Funktionen und Anwendungsabläufe öffnet.

## Neue Funktionen

Eindeutige Implementierung über alle Plattformen hinweg

Kundenanwendungen können jetzt plattformübergreifend dieselbe Implementierung verwenden, was das Starten neuer Funktionen oder die Pflege von Live-Apps erleichtert.

### Geräteübergreifendes SSO

Die REST-API V2 ermöglicht die sichere Weitergabe einer Authentifizierungssitzung zwischen verschiedenen Geräten. Durch einfache Übergabe der Sitzung zwischen Geräten können sich Benutzer auf ihrem Mobilgerät authentifizieren und Videos auf einem an ein Fernsehgerät angeschlossenen Gerät streamen, ohne dass eine erneute Authentifizierung erforderlich ist.

### Mehrere aktive Authentifizierungssitzungen

Verschiedene aktive MVPD-Sitzungen sind jetzt möglich, und Kunden können bei Bedarf zwischen der TempPass- und der regulären MVPD-Integration wechseln.

### Verbesserter Sicherheitsmechanismus

Die dynamische Client-Registrierung ist der Sicherheitsmechanismus, der für alle Flüsse und Funktionen verwendet wird. Es ermöglicht eine sicherere und detailliertere Kontrolle der Anwendungen von Kunden und kann Anwendungen auf allen Plattformen registrieren.

### Verbesserte Leistung für schnellere Reaktionszeiten

Verbesserte Caching-Mechanismen ermöglichen weniger Traffic zu MVPDs, verbessern die Antwortzeiten und reduzieren die Latenz. Insgesamt gibt es eine reduzierte Anzahl von API-Aufrufen, bis das Video gestartet wird.

### Verbesserte Fehlercodes für alle Flüsse

Erweiterte Fehler-Codes sind jetzt für alle Pass-Flüsse im selben Format verfügbar, mit zusätzlichen Details, die die Programme anleiten, um das gesamte Anwendererlebnis zu verbessern.

### Verbesserte Kontrolle über alle Authentifizierungssitzungen

Die neue REST-API V2 ermöglicht Aktionen für mehrere authentifizierte Sitzungen gleichzeitig.

### Geringere Wartungskosten

Alle Antworten und Fehlerinformationen sind jetzt normalisiert.

## Wie geht es weiter?

Alle Kundinnen und Kunden, die derzeit unsere APIs über SDKs oder REST-Aufrufe verwenden, können dies weiterhin tun, da wir planen, bis Ende 2025 weiterhin Support bereitzustellen.

Alle zukünftigen Entwicklungen werden jedoch auf der REST-API V2 aufbauen. Es wird dringend empfohlen, den Migrationsprozess zu starten, um von den neuesten Adobe Pass-Funktionen zu profitieren.

## Möchten Sie mehr erfahren?

Zu Beginn konsultieren Sie unsere öffentliche Dokumentation:

- [Glossar](rest-api-v2-glossary.md)
- [Checkliste](rest-api-v2-checklist.md)
- [Häufig gestellte Fragen](rest-api-v2-faqs.md)
- [APIs](apis/rest-api-v2-apis-overview.md)
- [Flows](flows/rest-api-v2-flows-overview.md)
- Cookbooks
- Anhang
- [Mindestsystemanforderungen](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Unser engagiertes Support-Team steht Ihnen auch bei Fragen oder technischer Unterstützung zur Verfügung.

## Möchten Sie die REST API V2 ausprobieren?

Sie können jetzt die REST-API V2 über unsere produktspezifische Seite auf der [Adobe Developer](https://developer.adobe.com/adobe-pass/)-Website erkunden.
