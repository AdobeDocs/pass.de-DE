---
title: REST API v2 - Übersicht
description: REST API v2 - Übersicht
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: 63dc9636f74f8eee1af6205c4d31a01df4503050
workflow-type: tm+mt
source-wordcount: '530'
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

## Einführung in die REST-API v2 {#rest-api-v2-introduction}

Die Adobe Pass-Authentifizierung freut sich, die Einführung der REST-API V2 bekannt geben zu können, die dazu dient, das Benutzererlebnis zu verbessern und die Integration mit Pass-Services zu vereinfachen.

Wir freuen uns über die Möglichkeiten unserer neuen REST-API, die einen großen Schritt nach vorne für unsere Plattform darstellt und die Tür für neue Funktionen und Anwendungsabläufe öffnet.

## Neue Funktionen {#rest-api-v2-whats-new}

Eindeutige Implementierung über alle Plattformen hinweg

Kundenanwendungen können jetzt plattformübergreifend dieselbe Implementierung verwenden, was das Starten neuer Funktionen oder die Pflege von Live-Apps erleichtert.

### Geräteübergreifendes SSO {#rest-api-v2-cross-device-sso}

Die REST-API V2 ermöglicht die sichere Weitergabe einer Authentifizierungssitzung zwischen verschiedenen Geräten. Durch einfache Übergabe der Sitzung zwischen Geräten können sich Benutzer auf ihrem Mobilgerät authentifizieren und Videos auf einem an ein Fernsehgerät angeschlossenen Gerät streamen, ohne dass eine erneute Authentifizierung erforderlich ist.

### Mehrere aktive Authentifizierungssitzungen {#rest-api-v2-multiple-active-authentication-sessions}

Verschiedene aktive MVPD-Sitzungen sind jetzt möglich, und Kunden können bei Bedarf zwischen der TempPass- und der regulären MVPD-Integration wechseln.

### Verbesserter Sicherheitsmechanismus {#rest-api-v2-enhanced-security-mechanism}

Die dynamische Client-Registrierung ist der Sicherheitsmechanismus, der für alle Flüsse und Funktionen verwendet wird. Es ermöglicht eine sicherere und detailliertere Kontrolle der Anwendungen von Kunden und kann Anwendungen auf allen Plattformen registrieren.

### Verbesserte Leistung für schnellere Reaktionszeiten {#rest-api-v2-improved-performance}

Verbesserte Caching-Mechanismen ermöglichen weniger Traffic zu MVPDs, verbessern die Antwortzeiten und reduzieren die Latenz. Insgesamt gibt es eine reduzierte Anzahl von API-Aufrufen, bis das Video gestartet wird.

### Verbesserte Fehlercodes für alle Flüsse {#rest-api-v2-enhanced-error-codes}

Erweiterte Fehler-Codes sind jetzt für alle Pass-Flüsse im selben Format verfügbar, mit zusätzlichen Details, die die Programme anleiten, um das gesamte Anwendererlebnis zu verbessern.

### Verbesserte Kontrolle über alle Authentifizierungssitzungen {#rest-api-v2-improved-control}

Die neue REST-API V2 ermöglicht Aktionen für mehrere authentifizierte Sitzungen gleichzeitig.

### Geringere Wartungskosten {#rest-api-v2-reduce-maintenance-costs}

Alle Antworten und Fehlerinformationen sind jetzt normalisiert.

## Wie geht es weiter? {#rest-api-v2-whats-next}

Alle Kundinnen und Kunden, die derzeit unsere APIs über SDKs oder REST-Aufrufe verwenden, können dies weiterhin tun, da wir planen, bis Ende 2025 weiterhin Support bereitzustellen.

Alle zukünftigen Entwicklungen werden jedoch auf der REST-API V2 aufbauen. Es wird dringend empfohlen, den Migrationsprozess zu starten, um von den neuesten Adobe Pass-Funktionen zu profitieren.

## Möchten Sie mehr erfahren? {#rest-api-v2-want-to-learn-more}

Zu Beginn konsultieren Sie unsere öffentliche Dokumentation:

- [Webinar](#rest-api-v2-webinar)
- [Glossar](rest-api-v2-glossary.md)
- [Checkliste](rest-api-v2-checklist.md)
- [KI-Regeln](rest-api-v2-ai-rules.md)
- [Häufig gestellte Fragen](rest-api-v2-faqs.md)
- [APIs](apis/rest-api-v2-apis-overview.md)
- [Flows](flows/rest-api-v2-flows-overview.md)
- Cookbooks
- Anhang
- [Mindestsystemanforderungen](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Unser engagiertes Support-Team steht Ihnen auch bei Fragen oder technischer Unterstützung zur Verfügung.

### Webinar {#rest-api-v2-webinar}

Sehen Sie sich das Webinar zur neuen REST API V2 an, in dem wir einen Überblick über die neuen Funktionen und Features sowie eine Live-Demo zur REST API V2 in Aktion gegeben haben.

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## Möchten Sie die REST API V2 ausprobieren? {#rest-api-v2-want-to-try}

Sie können jetzt die REST-API V2 über unsere produktspezifische Seite auf der [Adobe Developer](https://developer.adobe.com/adobe-pass/)-Website erkunden.
