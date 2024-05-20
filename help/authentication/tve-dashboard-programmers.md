---
title: Programmierer
description: Erfahren Sie mehr über Programmierer und ihre Konfigurationen im TVE-Dashboard.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Programmierer {#programmers}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle -Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Die **Programmierer** im TVE-Dashboard können Sie die Einstellungen für die [Programmierer](/help/authentication/glossary.md#programmer) mit Ihren Kontoberechtigungen verknüpft sind. Sie können auch [Hinzufügen eines neuen Programmierers](#add-new-programmer) gemäß Ihren Anforderungen.

Die **Programmierer** im linken Bereich eine Liste vorhandener Programmierer mit den folgenden Details angezeigt:

* **Programmierer-ID**: Eine Medienunternehmen-ID im System.
* **Kanäle**: Die Anzahl der mit einem Programmierer verknüpften Kanäle.

![Liste vorhandener Programmierer](assets/programmers-list.png)

*Liste vorhandener Programmierer*

Geben Sie den Namen des Programmierers in die **Suche** oberhalb der Liste, um mehr über einen Programmierer zu erfahren.

## Verwalten von Programmierkonfigurationen {#manage-programmer-conf}

Führen Sie diese Schritte aus, um verschiedene Einstellungen eines bestimmten Programmierers zu verwalten.

1. Wählen Sie die **Programmierer** im linken Bereich.
1. Wählen Sie einen Programmierer aus der Liste aus.
1. Wählen Sie eine der folgenden Registerkarten aus, um die entsprechenden Einstellungen des ausgewählten Programmierers anzuzeigen und zu bearbeiten:

   * [Kanäle](#channels)
   * [Zertifikate](#certificates)
   * [Registrierte Anwendungen](#registered-applications)
   * [Benutzerdefinierte Schemata](#custom-schemes)

   ![Programmiereinstellungen](assets/programmer-settings.png)

   *Programmiereinstellungen*

>[!IMPORTANT]
>
> Ansicht [Änderungen überprüfen und pushen](/help/authentication/tve-dashboard-review-push-changes.md) Weitere Informationen zum Aktivieren der Konfigurationsänderungen.

### Kanäle {#channels}

Auf dieser Registerkarte wird eine Liste von Kanälen angezeigt, die mit einem aktuellen Programmierer verknüpft sind. Wählen Sie einen bestimmten Kanal aus dieser Liste aus, um auf detaillierte Informationen im Abschnitt [Kanäle](/help/authentication/tve-dashboard-channels.md) Abschnitt.

Um einen neuen Kanal für den ausgewählten Programmierer hinzuzufügen, wählen Sie **Neuen Kanal hinzufügen** von der oberen rechten Ecke von **Verfügbare Kanäle** Abschnitt. Lernen [Hinzufügen eines neuen Kanals](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Neuen Kanal hinzufügen](assets/programmers-channels.png)

*Neuen Kanal hinzufügen*

### Zertifikate {#certificates}

Auf dieser Registerkarte wird eine Liste der [verfügbare Zertifikate](#available-certificates) wird in den Verschlüsselungsabläufen für Benutzermetadaten verwendet. Es werden Details zu jedem Zertifikat angezeigt, das Folgendes enthält:

* Der Status (ob für **Anwendermetadaten-Verschlüsselung** Verwendung oder nicht)
* Seriennummer
* Name der Organisation des Emittenten
* Name der Betrefforganisation
* Ausgegebener Zeitpunkt
* Ablaufdatum
* Ein Dropdown-Menü zum Verschlüsseln von Benutzermetadaten (Wenn Sie **Ja**, verschlüsselt das Zertifikat vertrauliche Benutzerinformationen, wie z. B. Postleitzahlenwerte).

#### Verfügbare Zertifikate {#available-certificates}

Diese Zertifikate dienen als private oder öffentliche Schlüssel und werden zur Verschlüsselung von Benutzermetadaten verwendet. Alle mit demselben Medienunternehmen verknüpften Kanäle können diese Zertifikate verwenden.

Sie können die folgenden Änderungen an den verfügbaren Zertifikaten vornehmen:

* [Neues Zertifikat hinzufügen](#add-new-certificate)
* [Zertifikat löschen](#delete-certificate)

##### Neues Zertifikat hinzufügen {#add-new-certificate}

Führen Sie die folgenden Schritte aus, um ein neues Zertifikat hinzuzufügen.

1. Auswählen **Neues Zertifikat hinzufügen** oben rechts im **Verfügbare Zertifikate** Abschnitt.

   ![Neues Zertifikat hinzufügen](assets/programmer-add-new-certificate.png)

   *Neues Zertifikat hinzufügen*

1. Fügen Sie den öffentlichen Schlüssel Ihres Zertifikats in den **Neues Zertifikat** Dialogfeld.
1. Auswählen **Zertifikat hinzufügen**.

   Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. So verwenden Sie das neue Zertifikat, das im **Verfügbare Zertifikate** und fahren Sie mit dem [Änderungen überprüfen und pushen](/help/authentication/tve-dashboard-review-push-changes.md) Fluss.

1. Suchen Sie das neue Zertifikat in der Liste der **Verfügbare Zertifikate**.

   >[!IMPORTANT]
   >
   > Stellen Sie sicher, dass Ihre Systeme auf dem neuesten Stand sind und das neue Zertifikat verwenden können.

1. Auswählen **Ja** von **Dient zum Verschlüsseln von Benutzermetadaten** Dropdown-Menü, um ein neues Zertifikat zu aktivieren.

##### Zertifikat löschen {#delete-certificate}

Führen Sie die folgenden Schritte aus, um ein Zertifikat zu löschen.

1. Bewegen Sie den Mauszeiger über das Zertifikat, das Sie aus der Liste der **Verfügbare Zertifikate**.
1. Auswählen **Entfernen**.

   ![Das ausgewählte Zertifikat entfernen](assets/programmer-remove-certificate.png)

   *Das ausgewählte Zertifikat entfernen*

1. Auswählen **Löschen** auf **Zertifikat löschen** Dialogfeld.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Das Zertifikat wird aus dem **Verfügbare Zertifikate** Abschnitt nur nach [Änderungen überprüfen und pushen](/help/authentication/tve-dashboard-review-push-changes.md).

### Registrierte Anwendungen {#registered-applications}

Dieser Tab enthält eine Liste der Registrierungen für Anwendungen. Ansicht [Dynamisches Client-Registrierungs-Management](/help/authentication/dynamic-client-registration-management.md), um weitere Informationen zu erhalten.

### Benutzerdefinierte Schemata {#custom-schemes}

Auf dieser Registerkarte wird eine Liste benutzerdefinierter Schemata angezeigt. Ansicht [Registrierung der iOS/tvOS-Anwendung](/help/authentication/iostvos-application-registration.md) und [Dynamisches Client-Registrierungs-Management](/help/authentication/dynamic-client-registration-management.md), um weitere Informationen zu erhalten.

## Hinzufügen neuer Programmierer {#add-new-programmer}

Führen Sie diese Schritte aus, um eine neue Programmierentität hinzuzufügen.

1. Wählen Sie die **Programmierer** im linken Bereich.
1. Auswählen **Hinzufügen neuer Programmierer** oben rechts im **Programmierer** Abschnitt.

   ![Hinzufügen eines neuen Programmierers](assets/add-new-programmer.png)

   *Hinzufügen eines neuen Programmierers*

1. Geben Sie die Medienunternehmen-Kennung ein in **Programmierer-ID** im **Neuer Programmierer** Dialogfeld.
1. Geben Sie einen kommerziellen Markennamen ein, der in der Konsole unter angezeigt werden soll **Anzeigename**.
1. Auswählen **Hinzufügen von Programmierern**.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. So verwenden Sie den neuen Programmierer, der in der **Programmierer** und fahren Sie mit dem [Änderungen überprüfen und pushen](/help/authentication/tve-dashboard-review-push-changes.md) Fluss.

