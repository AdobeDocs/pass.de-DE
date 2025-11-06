---
title: Abbaumerkmal
description: Abbaumerkmal
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Abbaumerkmal {#degradation-feature}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

In der dynamischen Welt von Live-Sport und Großveranstaltungen ist es wichtig, ein nahtloses Zuschauererlebnis zu gewährleisten. Hoher Traffic während dieser Ereignisse kann die Authentifizierungs- und Autorisierungsendpunkte des Multi-Channel Video Programming Distributor (MVPD) überfordern und zu Verzögerungen oder Unterbrechungen führen.

Die Adobe Pass-Authentifizierung löst diese Probleme mit der **Degradation Feature**, einer Lösung, die die temporäre Umgehung bestimmter MVPD-Authentifizierungs- und Autorisierungsendpunkte ermöglicht. Diese Funktion ist besonders bei Spitzen-Traffic-Ereignissen nützlich, bei denen die Reaktionszeiten aufgrund einer hohen Belastung der MVPD-Systeme abnehmen können.

Die **Degradation Feature** kann eine wichtige Sicherheitsmaßnahme für Programmierer sein, die die Kontinuität des Dienstes sicherstellt. Während die Hauptzielgruppe Live-Sport- und Nachrichtenkanäle umfasst, erstreckt sich ihr Nutzen auf jeden Programmierer, der versucht, das Risiko von Störungen durch MVPD-Endpunkte zu minimieren.

>[!IMPORTANT]
>
> Die Degradation API ist eine Premium-Funktion und erfordert eine aktuelle Lizenz von Adobe.

Durch Anwendung einer Degradationsregel können Programmierer die automatische Authentifizierung oder Autorisierung vorübergehend aktivieren und so den unterbrechungsfreien Zugriff auf Inhalte für den Zeitraum sicherstellen, in dem die Degradation angewendet wird. Abbaumaßnahmen werden immer von einem Programmierer auf der Grundlage vorab vereinbarter Vereinbarungen mit MVPDs initiiert. Auch wenn Adobe derzeit keine direkte Beeinträchtigung des Triggers feststellt, können zukünftige Funktionen eine proaktive Verwaltung einschließen, wenn Service Level Agreements (SLAs) abgeschlossen werden.

Diese Funktion kann zusammen mit einer [Nutzungsüberwachungs-API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) verwendet werden. Auf der Grundlage von Vorabvereinbarungen mit MVPDs bietet die Adobe Pass-Authentifizierung ein leistungsstarkes Tool, um Benutzererlebnis, Zuverlässigkeit und Betriebskontrolle in kritischen Momenten auszugleichen.

>[!IMPORTANT]
>
> Diese Funktion lässt die Umgehung des Adobe Pass-Authentifizierungsdienstes selbst nicht zu. Wenn die Adobe Pass-Authentifizierung nicht verfügbar ist, gibt es keinen integrierten Mechanismus innerhalb dieses Service, um den Benutzerzugriff zu erleichtern. In solchen Fällen können Sites oder Anwendungen ihre eigenen alternativen Routing-Lösungen implementieren, um die Bereitstellung von Inhalten aufrechtzuerhalten.

## API-Zugriff beeinträchtigt {#degradation-api-access}

Bevor Sie auf die [Abbau-](#degradation-api)) zugreifen können, müssen Sie die erforderlichen Schritte im Prozess der dynamischen Client-Registrierung (DCR) ausführen. Dieser obligatorische Prozess stellt sicher, dass Sie über das erforderliche Zugriffstoken für die Interaktion mit der Degradation-API verfügen.

Umfassende Anweisungen finden Sie in der Dokumentation [Übersicht über die Dynamic Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Abbauungs-API {#degradation-api}

Die Degradation API ist eine RESTful-API, mit der Programmierer Abbauregeln für bestimmte MVPDs verwalten können. Die API bietet die Möglichkeit, den Status für die aktiven Abbauregeln zu aktivieren, zu entfernen und abzurufen.

Weitere Informationen zur Degradation-API finden Sie im folgenden Zendesk-Dokument [Adobe Pass-Authentifizierung | Beeinträchtigungs-API v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3) und suchen Sie nach der herunterzuladenden PDF-Datei.

## REST-API V2 {#rest-api-v2}

Für die Nutzung der Abbaufunktion ist die Implementierung von Code-Aktualisierungen erforderlich, um die Interaktion der TV Everywhere-Anwendung (TVE) mit der Adobe Pass-Authentifizierung ([-API V2) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) ändern.

Eine umfassende Anleitung zu diesen Aktualisierungen und den zugehörigen Workflows finden Sie in der Dokumentation [Gestörte ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)&quot;.
