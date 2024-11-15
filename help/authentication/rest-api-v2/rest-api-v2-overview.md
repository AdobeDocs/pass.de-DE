---
title: REST API V2 - Überblick
description: REST API V2 - Überblick
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# REST API V2 - Überblick {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Möchten Sie die Kosteneffizienz Ihrer TVE-Anwendungen verbessern?

Möchten Sie die Entwicklungszeit und die Ressourcen reduzieren, die zur Unterstützung von TVE-Anwendungen auf mehreren Plattformen erforderlich sind?

Möchten Sie plattformübergreifend ein einheitliches Benutzererlebnis sicherstellen?

Möchten Sie den Wartungsaufwand reduzieren und die Bereitstellung von Aktualisierungen, Fehlerbehebungen oder Verbesserungen vereinfachen?

## Einführung in die REST-API V2

Adobe Pass Authentication freut sich, die Einführung der REST API V2 bekannt geben zu können, mit der das Benutzererlebnis verbessert und die Integration mit Pass Services vereinfacht werden soll.

Wir freuen uns über die Möglichkeiten unserer neuen REST-API, die einen wichtigen Schritt nach vorn für unsere Plattform darstellt und die Tür für neue Funktionen und Anwendungsflüsse öffnet.

## Neue Funktionen

Eindeutige Implementierung auf allen Plattformen

Die Anwendungen von Kunden können jetzt plattformübergreifend dieselbe Implementierung verwenden, was das Starten neuer Funktionen oder Verwalten von Live-Apps erleichtert.

### Geräteübergreifende SSO

REST API V2 ermöglicht die sichere Übertragung einer Authentifizierungssitzung zwischen verschiedenen Geräten. Durch einfache Übergabe der Sitzung zwischen Geräten können Benutzer sich auf ihrem Mobilgerät authentifizieren und Videos auf einem mit TV verbundenen Gerät streamen, ohne sich erneut authentifizieren zu müssen.

### Mehrere aktive Authentifizierungssitzungen

Verschiedene aktive MVPD-Sitzungen sind jetzt möglich und Kunden können bei Bedarf zwischen TempPass und regulärer MVPD-Integration wechseln.

### Verbesserter Sicherheitsmechanismus

Die dynamische Kundenregistrierung ist der Sicherheitsmechanismus, der für alle Datenflüsse und Funktionen verwendet wird. Sie ermöglicht eine sicherere und granularere Steuerung der Anwendungen von Kunden und kann Anwendungen auf allen Plattformen registrieren.

### Verbesserte Leistung für schnellere Reaktionszeiten

Verbesserte Caching-Mechanismen ermöglichen weniger Traffic zu MVPDs, verbessern die Reaktionszeiten und reduzieren die Latenz. Insgesamt ist die Anzahl der API-Aufrufe bis zum Videostart reduziert.

### Verbesserte Fehlercodes für alle Flüsse

Erweiterte Fehlercodes sind jetzt für alle Pass-Flüsse im selben Format verfügbar, mit zusätzlichen Details, die die Anwendungen zur Verbesserung des Gesamterlebnisses der Benutzer anleiten.

### Verbesserte Kontrolle über alle Authentifizierungssitzungen

Die neue REST-API V2 ermöglicht Aktionen für mehrere authentifizierte Sitzungen gleichzeitig.

### Reduzierung der Wartungskosten

Alle Antworten und Fehlerinformationen wurden nun normalisiert.

## Wie geht es weiter?

Alle Kunden, die derzeit unsere APIs über SDKs oder REST-Aufrufe verwenden, können dies auch weiterhin tun, da wir Ende 2025 weiterhin Unterstützung bieten möchten.

Alle künftigen Entwicklungen werden jedoch auf der REST-API V2 basieren. Wir empfehlen dringend, den Migrationsprozess zu starten, um von den neuesten Adobe Pass-Funktionen zu profitieren.

## Möchten Sie mehr erfahren?

Besuchen Sie zunächst unsere öffentliche Dokumentation:

- [Glossar](./rest-api-v2-glossary.md)
- [FAQs](./rest-api-v2-faqs.md)
- [APIs](./apis/rest-api-v2-apis-overview.md)
- [Flüsse](./flows/rest-api-v2-flows-overview.md)
- Cookbooks
- Anhang
- [Mindestsystemanforderungen](/help/authentication/minimum-system-requirements.md)

Unser spezielles Supportteam steht Ihnen auch bei Fragen und technischer Unterstützung zur Verfügung.

## Möchten Sie die REST API V2 ausprobieren?

Sie können jetzt die REST-API V2 über unsere produktspezifische Seite von der Website [Adobe Developer](https://developer.adobe.com/adobe-pass/) aus aufrufen.
