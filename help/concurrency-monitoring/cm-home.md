---
title: Einführung in die Überwachung von Parallelität
description: Einführung in die Überwachung von Parallelität
exl-id: 725cc64b-6b03-46e3-a038-41e9b1341c6b
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Einführung in die Überwachung von Parallelität {#intro}

Die gleichzeitige Überwachung ist ein Service, der es Inhaltsanbietern und Identitätsanbietern (MVPDs und Programmierern) ermöglicht, Einschränkungen für das gleichzeitige Video-Streaming über mehrere Anwendungen, Geräte und Plattformen hinweg zu definieren und durchzusetzen. Unabhängig davon, ob Sie als Programmierer steuern möchten, wie viele Streams ein Abonnent gleichzeitig sehen kann, oder ob Sie als MVPD Nutzungsrichtlinien für Ihre Inhaltspartner durchsetzen möchten, bietet die gleichzeitige Überwachung die Tools, die Sie benötigen.

## Was ist Parallelitätsüberwachung? {#what-is-cm}

Die gleichzeitige Überwachung ist ein zentralisierter Dienst, der aktive Video-Streaming-Sitzungen in Echtzeit verfolgt und verwaltet. Damit können Sie:

- **Gleichzeitige Streams pro Abonnent begrenzen** - Steuert, auf wie viele gleichzeitige Videostreams ein Benutzer zugreifen kann.
- **Erzwingen von Gerätebeschränkungen** - Streaming auf bestimmte Gerätetypen oder -mengen beschränken
- **Implementieren von standortbasierten Richtlinien** - Streaming auf der Grundlage des geografischen Standorts einschränken
- **Erstellen von inhaltsspezifischen Regeln** - Wenden Sie unterschiedliche Beschränkungen für Live- und VOD-Inhalte an
- **Überwachen von Nutzungsmustern** - Gewinnen Sie Einblicke in die Verwendung Ihrer Inhalte

## Funktionsweise {#how-it-works}

Die gleichzeitige Überwachung erfolgt über eine einfache, aber leistungsstarke API:

1. **Sitzungsinitialisierung** - Wenn ein Benutzer beginnt, Inhalte anzusehen, erstellt Ihre Anwendung eine Sitzung
2. **Richtlinienauswertung** - Der Service bewertet Ihre definierten Richtlinien im Hinblick auf die aktuelle Verwendung
3. **Echtzeit-Monitoring** - Heartbeat-Aufrufe halten Sitzungen aktiv und überwachen die Einhaltung von Vorschriften

## Neu bei der Parallelitätsüberwachung? {#new-to-cm}

Beginnen Sie mit unserem [Erste Schritte-Handbuch](getting-started/getting-started-overview.md) um die Grundlagen zu verstehen und Ihre erste Integration einzurichten.

