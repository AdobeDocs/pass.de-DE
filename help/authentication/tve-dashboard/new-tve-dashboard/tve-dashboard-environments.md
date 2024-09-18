---
title: Tv Dashboard-Umgebungen
description: Machen Sie sich mit der Verwendung und Funktionsweise verschiedener Umgebungen im TVE-Dashboard vertraut.
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Umgebungen {#environments}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle -Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Das TVE-Dashboard bietet verschiedene Umgebungen, die an bestimmte Zwecke innerhalb der Adobe Pass-Authentifizierung angepasst wurden. Es gibt zwei primäre Umgebungen:

* **Prequal**: Die Umgebung für die Vorqualifizierung dient als Testgrundlage für die Vorbereitung und den Test neuer Builds vor der Bereitstellung in der Produktion.

* **Version**: Die Veröffentlichungsumgebung hostet die finalisierten und getesteten Builds für die Produktion.

In jeder Umgebung gibt es zwei verschiedene Profile:

* **Staging**: Das Staging-Profil stellt eine Verbindung zum Staging-Server des MVPD her, um Integrationen vor der Live-Schaltung zu testen und zu validieren.

* **Produktion**: Das Produktionsprofil stellt eine Verbindung zum Produktionsprofil des MVPD für tatsächliche Produktionsaktivitäten her.

## Anwendungsbeispiele

Die Umgebungen im TVE-Dashboard unterstützen verschiedene Anwendungsfälle während des gesamten Anwendungslebenszyklus. Diese Umgebungen ermöglichen Ihnen Folgendes:

### Prequal Staging

* Validieren Sie neue nicht veröffentlichte Funktionen des Adobe Pass-Authentifizierungsservers mithilfe der Staging-Endpunkte von MVPD.
* Wird hauptsächlich vom Adobe Pass Authentication-Produktteam zum Hinzufügen und Validieren neuer MVPD-Integrationen verwendet.

### Prequal Production

* Validieren Sie neue nicht veröffentlichte Funktionen oder Konfigurationen des Adobe Pass-Authentifizierungsservers mithilfe der Produktions-Endpunkte von MVPD.
* Validieren Sie neue Anwendungsversionen für jeden Kanal mithilfe der Produktions-Endpunkte von MVPD.
* Validieren Sie jede Konfigurationsänderung, bevor Sie sie in die Produktion übernehmen.

### Release-Staging

* Validieren Sie neue Anwendungsversionen für jeden Kanal mithilfe der Staging-Endpunkte von MVPD.
* Führen Sie in dieser Umgebung Leistungs- oder Kapazitätstests durch.

### Versionsproduktion

* Stellt die Live-Umgebung mit dem neuesten Adobe Pass-Release-Build dar, der allgemein für alle Endbenutzer verfügbar ist.
* Behält Stabilität im Code und in der Konfiguration bei und spiegelt sofort Konfigurationsänderungen in der Anwendung des Endbenutzers wider.

## Wechseln von Umgebungen {#switch-environments}

Führen Sie die Schritte aus, um zwischen Adobe Pass Authentication TVE Dashboard-Umgebungen zu wechseln.

1. Melden Sie sich mit Ihren Programmiereranmeldeinformationen an.

1. Wählen Sie die erforderliche Staging- oder Produktionsumgebung aus dem Dropdown-Menü **Umgebung** oben im linken Bereich aus.

   ![Dropdown-Liste &quot;TVE Dashboard environment&quot;](../../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Das Dropdown-Menü für die Adobe Pass-Authentifizierungs-TVE-Dashboard-Umgebung*

>[!NOTE]
>
> Die Konfigurationen können je nach Ihren Einstellungen in den einzelnen Umgebungen variieren.
