---
title: Programmierer
description: Erfahren Sie mehr über Programmierer und ihre Konfigurationen im TVE-Dashboard.
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# Programmierer {#programmers}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle -Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Im Abschnitt **Programmierer** des TVE-Dashboards können Sie Einstellungen für die mit Ihren Kontoberechtigungen verknüpften [Programmierer](/help/authentication/glossary.md#programmer) anzeigen und verwalten. Sie können entsprechend Ihren Anforderungen auch [einen neuen Programmierer](#add-new-programmer) hinzufügen.

Auf der Registerkarte **Programmierer** im linken Bereich wird eine Liste der vorhandenen Programmierer mit den folgenden Details angezeigt:

* **Programmierer-ID**: Eine Medienunternehmen-ID im System.
* **Kanäle**: Die Anzahl der mit einem Programmierer verknüpften Kanäle.

![Liste der vorhandenen Programmierer](assets/programmers-list.png)

*Liste der vorhandenen Programmierer*

Geben Sie den Namen des Programmierers in die Leiste **Suchen** oberhalb der Liste ein, um mehr über einen Programmierer zu erfahren.

## Verwalten von Programmierkonfigurationen {#manage-programmer-conf}

Führen Sie diese Schritte aus, um verschiedene Einstellungen eines bestimmten Programmierers zu verwalten.

1. Wählen Sie im linken Bereich die Registerkarte **Programmierer** aus.
1. Wählen Sie einen Programmierer aus der Liste aus.
1. Wählen Sie eine der folgenden Registerkarten aus, um die entsprechenden Einstellungen des ausgewählten Programmierers anzuzeigen und zu bearbeiten:

   * [Kanäle](#channels)
   * [Zertifikate](#certificates)
   * [Registrierte Anwendungen](#registered-applications)
   * [Benutzerdefinierte Schemata](#custom-schemes)

   ![Programmierereinstellungen](assets/programmer-settings.png)

   *Programmierereinstellungen*

>[!IMPORTANT]
>
> Weitere Informationen zum Aktivieren der Konfigurationsänderungen finden Sie unter [Überprüfen und Pushen von Änderungen](/help/authentication/tve-dashboard-review-push-changes.md) .

### Kanäle {#channels}

Auf dieser Registerkarte wird eine Liste von Kanälen angezeigt, die mit einem aktuellen Programmierer verknüpft sind. Wählen Sie einen bestimmten Kanal aus dieser Liste aus, um auf detaillierte Informationen im Abschnitt [Kanäle](/help/authentication/tve-dashboard-channels.md) zuzugreifen.

Um einen neuen Kanal für den ausgewählten Programmierer hinzuzufügen, wählen Sie **Neuen Kanal hinzufügen** aus der oberen rechten Ecke des Bereichs **Verfügbare Kanäle** aus. Erfahren Sie, wie Sie [einen neuen Kanal hinzufügen](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Hinzufügen eines neuen Kanals](assets/programmers-channels.png)

*Hinzufügen eines neuen Kanals*

### Zertifikate {#certificates}

Auf dieser Registerkarte wird eine Liste der [verfügbaren Zertifikate](#available-certificates) angezeigt, die in den Verschlüsselungsflüssen für Benutzermetadaten verwendet werden. Es werden Details zu jedem Zertifikat angezeigt, das Folgendes enthält:

* Der Status (ob für die Verwendung der **Benutzer-Metadatenverschlüsselung** aktiviert ist oder nicht)
* Seriennummer
* Name der Organisation des Emittenten
* Name der Betrefforganisation
* Ausgegebener Zeitpunkt
* Ablaufdatum
* Ein Dropdown-Menü zum Verschlüsseln von Benutzermetadaten (Wenn Sie &quot;**Ja**&quot;auswählen, verschlüsselt das Zertifikat vertrauliche Benutzerinformationen wie z. B. Postleitzahlenwerte).

#### Verfügbare Zertifikate {#available-certificates}

Diese Zertifikate dienen als private oder öffentliche Schlüssel und werden zur Verschlüsselung von Benutzermetadaten verwendet. Alle mit demselben Medienunternehmen verknüpften Kanäle können diese Zertifikate verwenden.

Sie können die folgenden Änderungen an den verfügbaren Zertifikaten vornehmen:

* [Neues Zertifikat hinzufügen](#add-new-certificate)
* [Zertifikat löschen](#delete-certificate)

##### Neues Zertifikat hinzufügen {#add-new-certificate}

Führen Sie die folgenden Schritte aus, um ein neues Zertifikat hinzuzufügen.

1. Wählen Sie oben rechts im Abschnitt **Verfügbare Zertifikate** die Option **Neues Zertifikat hinzufügen** aus.

   ![Neues Zertifikat hinzufügen](assets/programmer-add-new-certificate.png)

   *Neues Zertifikat hinzufügen*

1. Fügen Sie den öffentlichen Schlüssel Ihres Zertifikats in das Dialogfeld **Neues Zertifikat** ein.
1. Wählen Sie **Zertifikat hinzufügen** aus.

   Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Um das neue Zertifikat zu verwenden, das im Abschnitt **Verfügbare Zertifikate** aufgeführt ist, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/tve-dashboard-review-push-changes.md) fort.

1. Suchen Sie das neue Zertifikat in der Liste der **Verfügbaren Zertifikate**.

   >[!IMPORTANT]
   >
   > Stellen Sie sicher, dass Ihre Systeme auf dem neuesten Stand sind und das neue Zertifikat verwenden können.

1. Wählen Sie im Dropdown-Menü **Verwendet zum Verschlüsseln von Benutzermetadaten** die Option **Ja** aus, um ein neues Zertifikat zu aktivieren.

##### Zertifikat löschen {#delete-certificate}

Führen Sie die folgenden Schritte aus, um ein Zertifikat zu löschen.

1. Bewegen Sie den Mauszeiger über das Zertifikat, das Sie aus der Liste der **Verfügbaren Zertifikate** löschen möchten.
1. Wählen Sie **Entfernen** aus.

   ![Entfernen Sie das ausgewählte Zertifikat](assets/programmer-remove-certificate.png)

   *Entfernen Sie das ausgewählte Zertifikat*

1. Wählen Sie **Löschen** im Dialogfeld **Zertifikat löschen** aus.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Das Zertifikat wird erst aus dem Abschnitt **Verfügbare Zertifikate** gelöscht, nachdem [Änderungen überprüft und gepusht haben](/help/authentication/tve-dashboard-review-push-changes.md).

### Registrierte Anwendungen {#registered-applications}

Dieser Tab enthält eine Liste der Registrierungen für Anwendungen.

### Benutzerdefinierte Schemata {#custom-schemes}

Auf dieser Registerkarte wird eine Liste benutzerdefinierter Schemata angezeigt. Anzeigen der [Registrierung der iOS/tvOS-Anwendung](/help/authentication/iostvos-application-registration.md).

## Hinzufügen neuer Programmierer {#add-new-programmer}

Führen Sie diese Schritte aus, um eine neue Programmierentität hinzuzufügen.

1. Wählen Sie im linken Bereich die Registerkarte **Programmierer** aus.
1. Wählen Sie oben rechts im Abschnitt **Programmierer** die Option **Neuen Programmierer hinzufügen** aus.

   ![Hinzufügen eines neuen Programmierers](assets/add-new-programmer.png)

   *Hinzufügen eines neuen Programmierers*

1. Geben Sie die Medienunternehmen-ID in **Programmierer-ID** im Dialogfeld **Neuer Programmierer** ein.
1. Geben Sie unter **Anzeigename** einen kommerziellen Markennamen ein, der in der Konsole angezeigt werden soll.
1. Wählen Sie **Programmierer hinzufügen** aus.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Um den im Abschnitt **Programmierer** aufgelisteten neuen Programmierer zu verwenden, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/tve-dashboard-review-push-changes.md) fort.
