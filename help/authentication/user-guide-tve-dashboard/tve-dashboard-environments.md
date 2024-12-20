---
title: TVE-Dashboard-Umgebungen
description: Machen Sie sich mit der Verwendung und Funktionsweise verschiedener Umgebungen im TVE-Dashboard vertraut.
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Umgebungen {#environments}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Nutzung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Das TVE-Dashboard bietet verschiedene Umgebungen, die speziell auf die Erfüllung bestimmter Zwecke im Rahmen der Adobe Pass-Authentifizierung angepasst werden. Es gibt zwei primäre Umgebungen:

* **Prequal**: Die Umgebung für die Vorqualifizierung dient als Testumgebung für die Vorbereitung und das Testen neuer Builds vor der Bereitstellung in der Produktion.

* **Release**: Die Veröffentlichungsumgebung hostet die fertigen und getesteten Builds für die Produktion.

In jeder Umgebung gibt es zwei verschiedene Profile:

* **Staging**: Das Staging-Profil stellt eine Verbindung zum Staging-Server von MVPD her, um Integrationen vor der Live-Schaltung zu testen und zu validieren.

* **Produktion**: Das Produktionsprofil stellt eine Verbindung zum Produktionsprofil von MVPD her, um die tatsächlichen Produktionsaktivitäten zu ermitteln.

## Anwendungsbeispiele

Die Umgebungen im TVE-Dashboard dienen verschiedenen Anwendungsfällen während des gesamten Anwendungslebenszyklus. Diese Umgebungen bieten Ihnen folgende Möglichkeiten:

### Prä-Staging

* Validieren Sie neue unveröffentlichte Funktionen des Adobe Pass-Authentifizierungsservers mithilfe der Staging-Endpunkte von MVPD.
* Wird hauptsächlich vom Adobe Pass-Authentifizierungs-Produkt-Team zum Hinzufügen und Validieren neuer MVPD-Integrationen verwendet.

### Prävalenzproduktion

* Validieren Sie neue unveröffentlichte Funktionen oder Konfigurationen des Adobe Pass-Authentifizierungsservers mithilfe der Produktionsendpunkte von MVPD.
* Validieren Sie neue Programmversionen für jeden Kanal mithilfe der Produktionsendpunkte von MVPD.
* Validieren Sie jede Konfigurationsänderung, bevor Sie sie in die Produktion verschieben.

### Release-Staging

* Validieren Sie neue Anwendungsversionen für jeden Kanal mithilfe der Staging-Endpunkte von MVPD.
* Führen Sie in dieser Umgebung Leistungs- oder Kapazitätstests durch.

### Release-Produktion

* Stellt die Live-Umgebung mit dem neuesten Adobe Pass-Release-Build dar, der allgemein für alle Endbenutzer verfügbar ist.
* Behält die Stabilität im Code und in der Konfiguration bei und spiegelt sofort Konfigurationsänderungen im Programm des Endbenutzers wider.

## Wechseln von Umgebungen {#switch-environments}

Führen Sie die Schritte aus, um zwischen TVE-Dashboard-Umgebungen für die Adobe Pass-Authentifizierung zu wechseln.

1. Melden Sie sich mit Ihren Programmiereranmeldeinformationen an.

1. Wählen Sie die gewünschte Staging- oder Produktionsumgebung aus **Dropdown** Menü „Umgebung“ oben im linken Bedienfeld.

   ![TVE Dashboard Environments Dropdown](../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Das Dropdown-Menü TVE Dashboard-Umgebung für die Adobe Pass-Authentifizierung*

>[!NOTE]
>
> Die Konfigurationen können in jeder Umgebung je nach Ihren Einstellungen variieren.
