---
title: Kanäle
description: Erfahren Sie mehr über Kanäle und ihre verschiedenen Konfigurationen im TVE-Dashboard.
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# Kanäle {#channels}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Im **Kanäle** des TVE-Dashboards können Sie Einstellungen für die Kanäle anzeigen und verwalten, die mit einem bestimmten Programmierer verknüpft sind. Sie können auch [neuen Kanal hinzufügen](#add-new-channel) je nach Ihren Anforderungen.

Auf **Registerkarte** Kanäle“ im linken Bereich wird eine Liste verknüpfter Kanäle mit den folgenden Details angezeigt:

* **Anzeigename**: Der Markenname des Kanals, der für kommerzielle Zwecke verwendet wird.
* **Kanal-ID**: Eine eindeutige Kennung, auch als Anforderer-ID bezeichnet.
* **Integrationen**: Die Anzahl der Verbindungen, die mit (MVPDs[&#x200B; hergestellt &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd).

![Liste der vorhandenen Kanäle](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channels-list-view.png)

*Liste der vorhandenen Kanäle*

Geben Sie den Namen des Kanals in der Leiste **Suche** über der Liste ein, um mehr über den Kanal zu erfahren.

## Kanalkonfigurationen verwalten {#manage-channel-conf}

Führen Sie die Schritte aus, um verschiedene Einstellungen eines bestimmten Kanals zu verwalten.

1. Wählen Sie die **Kanäle** im linken Bedienfeld aus.

1. Wählen Sie den Kanal in der Liste Verfügbar aus.

1. Wählen Sie eine der folgenden Registerkarten aus, um die entsprechenden Einstellungen des ausgewählten Kanals anzuzeigen und zu bearbeiten:

   * [Allgemeine Einstellungen](#general-settings)
   * [Integrationen](#integrations)
   * [Zertifikate](#certificates)
   * [Domains](#domains)
   * [Registrierte Anwendungen](#registered-applications)
   * [Benutzerdefinierte Schemata](#custom-schemes)

   ![Kanaleinstellungen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-tabs-view.png)

   *Kanaleinstellungen*

>[!IMPORTANT]
>
> Weitere Informationen [&#x200B; Aktivieren der Konfigurationsänderungen finden Sie unter &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) und Push-Änderungen .

### Allgemeine Einstellungen {#general-settings}

Auf dieser Registerkarte finden Sie **Kanalinformationen** und **Analytics-Konfiguration**.

#### Kanalinformationen {#channel-information}

In diesem Abschnitt können Sie die folgenden Details bearbeiten:

* **Anzeigename**: Der Markenname des Kanals, der für kommerzielle Zwecke verwendet wird.

* **Standard-Umleitungs-URL**: Die Sicherungs-Umleitungs-URL für die Authentifizierung und die Abmeldung.

* **Fehlerberichterstattung**: Bei Auswahl von **Ja** senden die Adobe Pass SDKs Fehlerberichte zur Analyse an das Adobe Pass-Backend.

![Kanalinformationen bearbeiten](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-general-settings-tab-view.png)

*Kanalinformationen bearbeiten*

#### Analytics-Konfiguration {#analytics-configuration}

In diesem Abschnitt können Sie die Weiterleitung von Adobe Pass-Authentifizierungsereignissen an Adobe Analytics konfigurieren.

Um die **Analytics-Konfiguration** zu aktivieren, wenden Sie sich an Ihren Technical Account Manager (TAM), um weitere Informationen zum Einrichten der Report Suite-ID (RSID) zu erhalten.

![Aktivieren von Analytics-Konfigurationen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-analytics-configuration-button.png)

*Aktivieren von Analytics-Konfigurationen*

Wählen Sie **Neue Analytics-Konfiguration hinzufügen** aus, um mehrere Konfigurationen hinzuzufügen.

Eine neue Konfigurationsänderung wurde erstellt und ist für die Server-Aktualisierung bereit. Um die neue Analytics-Konfiguration im Abschnitt **Analytics-Konfiguration** zu verwenden, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

### Integrationen {#integrations}

Auf dieser Registerkarte wird eine Liste der verfügbaren Integrationen zwischen dem aktuell ausgewählten Kanal und MVPDs angezeigt. Die Liste zeigt jede Integration zusammen mit ihrem Status an und gibt an, ob sie aktiviert ist oder nicht. Wählen Sie eine bestimmte Integration aus dieser Liste aus, um auf detaillierte Informationen im Abschnitt [Integrationen](tve-dashboard-integrations.md) zuzugreifen.

![Liste der verfügbaren Integrationen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-integrations-tab-view.png)

*Liste der verfügbaren Integrationen*

### Zertifikate {#certificates}

Auf dieser Registerkarte wird eine Liste [verfügbaren Zertifikate](#available-certificates) und [vererbten verfügbaren Zertifikate](#inherited-avail-certificates) angezeigt, die in den Verschlüsselungsflüssen der Benutzermetadaten verwendet werden. Es werden Details zu jedem Zertifikat angezeigt, das Folgendes enthält:

* Der Status (unabhängig davon, ob er für **Benutzermetadatenverschlüsselung** Verwendung aktiviert ist oder nicht)
* Seriennummer
* Name der ausstellenden Organisation
* Name der betroffenen Organisation
* Ausstellungsdatum
* Ablaufdatum
* Ein Dropdown-Menü zum Verschlüsseln von Benutzermetadaten (wenn Sie **Ja** auswählen, verschlüsselt das Zertifikat sensible Benutzerinformationen wie Postleitzahlwerte).

#### Verfügbare Zertifikate {#available-certificates}

Diese Zertifikate dienen als private oder öffentliche Schlüssel und werden für die Verschlüsselung von Benutzermetadaten verwendet.
Im Abschnitt Verfügbare Zertifikate können Sie die folgenden Änderungen vornehmen:

* [Neues Zertifikat hinzufügen](#add-new-certificate)
* [Zertifikat löschen](#delete-certificate)

##### Neues Zertifikat hinzufügen {#add-new-certificate}

Gehen Sie wie folgt vor, um ein neues Zertifikat hinzuzufügen:

1. Wählen **oben** Abschnitt **Verfügbare Zertifikate** die Option „Neues Zertifikat hinzufügen“ aus.

   ![Neues Zertifikat hinzufügen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-certificate-button.png)

   *Neues Zertifikat hinzufügen*

1. Fügen Sie den öffentlichen Schlüssel Ihres Zertifikats in das Dialogfeld **Neues Zertifikat** ein.

1. Wählen Sie **Zertifikat hinzufügen** aus.

1. Suchen Sie das neue Zertifikat in der Liste der **verfügbaren Zertifikate**.

   >[!IMPORTANT]
   >
   > Stellen Sie sicher, dass Ihre Systeme auf dem neuesten Stand sind und das neue Zertifikat verwenden können.

1. Wählen Sie **Ja** aus **Dropdown-Menü Wird zum Verschlüsseln von Benutzermetadaten verwendet** aus, um ein neues Zertifikat zu aktivieren.

Eine neue Konfigurationsänderung wurde erstellt und ist für die Server-Aktualisierung bereit. Um das neue Zertifikat zu verwenden, das im Abschnitt **Verfügbare Zertifikate** aufgeführt ist, fahren Sie mit dem Fluss [Überprüfen und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

##### Zertifikat löschen {#delete-certificate}

Gehen Sie wie folgt vor, um ein Zertifikat zu löschen.

1. Bewegen Sie den Mauszeiger über das Zertifikat, das Sie aus der Liste der **verfügbaren Zertifikate“** möchten.

1. Wählen Sie **Entfernen** aus.

   ![Das ausgewählte Zertifikat entfernen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-delete-certificate-button.png)

   *Das ausgewählte Zertifikat entfernen*

1. Wählen **Löschen** im Dialogfeld **Aktives Zertifikat löschen** aus.

Eine neue Konfigurationsänderung wurde erstellt und ist für die Server-Aktualisierung bereit. Das Zertifikat wird aus dem Abschnitt **Verfügbare Zertifikate** erst nach [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) gelöscht.

#### Vererbte verfügbare Zertifikate {#inherited-avail-certificates}

Medienunternehmen definieren diese Zertifikate auf ihrer eigenen Ebene. Alle Kanäle, die demselben Medienunternehmen zugeordnet sind, können diese Zertifikate verwenden.

![Vererbte verfügbare Zertifikate](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-available-certificates-panel-view.png)

*Vererbte verfügbare Zertifikate*

### Domains {#domains}

Auf dieser Registerkarte wird eine Liste der verfügbaren Domains angezeigt, über die der entsprechende Kanal mit der Adobe Pass-Authentifizierung kommuniziert.

Sie können die folgenden Änderungen an Domains vornehmen:

* [Neue Domain hinzufügen](#add-domains)
* [Domain löschen](#delete-domain)

>[!TIP]
>
> Vermeiden Sie das Hinzufügen einer neuen Subdomain, wenn in der Liste eine allgemeinere Domain vorhanden ist.

#### Neue Domain hinzufügen {#add-domains}

Gehen Sie wie folgt vor, um eine Domain hinzuzufügen.

1. Wählen **oben rechts** Abschnitt **Verfügbare Domains** die Option „Neue Domain hinzufügen aus.

   ![Neue Domain hinzufügen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-domain-button.png)

   *Neue Domain hinzufügen*

1. Geben Sie den Namen Ihrer Domain im Dialogfeld **Neue Domain** ein.

1. Wählen Sie **Domain hinzufügen** aus, um eine neue Domain für den ausgewählten Kanal hinzuzufügen.

Eine neue Konfigurationsänderung wurde erstellt und ist für die Server-Aktualisierung bereit. Um die neue Domain im Abschnitt **Verfügbare Domains** zu verwenden, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

#### Domain löschen {#delete-domain}

Gehen Sie wie folgt vor, um eine Domain zu löschen.

1. Bewegen Sie den Mauszeiger über die Domain, die Sie aus der Liste der **verfügbaren Domains“** möchten.

1. Wählen Sie **Entfernen** aus.

   ![Entfernen Sie die ausgewählte Domain](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-remove-domain-button.png)

   *Entfernen Sie die ausgewählte Domain*

1. Wählen **&#x200B;**&#x200B;im Dialogfeld **Domain löschen** die Option „Löschen“ aus.

Eine neue Konfigurationsänderung wurde erstellt und ist für die Server-Aktualisierung bereit. Die Domain wird erst nach dem **(Überprüfung und Push**&#x200B;Änderungen) aus dem Abschnitt [Verfügbare Domains](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) gelöscht.

Die ausgewählte Domain ist nicht mehr zur Verwendung verfügbar. Infolgedessen verliert die mit dieser Domain verknüpfte Anwendung den Zugriff auf die Adobe Pass-Authentifizierungsdienste.

### Registrierte Anwendungen {#registered-applications}

Auf dieser Registerkarte wird eine Liste der registrierten Anwendungen angezeigt. Weitere Informationen zur Verwendung registrierter Anwendungen finden Sie in der Dokumentation [Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Sie können mit registrierten Programmen die folgenden Aktionen durchführen:

* [Neue registrierte Anwendung hinzufügen](#add-registered-applications)
* [Software-Erklärung herunterladen](#download-software-statement)

#### Neue registrierte Anwendung hinzufügen {#add-registered-applications}

Führen Sie diese Schritte aus, um eine neue registrierte Anwendung hinzuzufügen.

1. Wählen **oben rechts** Abschnitt **Registrierte Anwendungen“ die Option „Neue Anwendung**.

   ![Neue Anwendung hinzufügen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-application-button.png)

   *Neue Anwendung hinzufügen*

1. Wählen Sie **Plattformen** aus dem Dropdown-Menü im Dialogfeld **Neue Anwendung** aus.

   >[!IMPORTANT]
   >
   > Es wird empfohlen, registrierte Anwendungen mit spezifischeren und eingeschränkteren Berechtigungen zu erstellen, um die Sicherheit zu erhöhen und nicht autorisierten Zugriff zu verhindern. Daher sollten Sie beim Erstellen registrierter Anwendungen die Verwendung engerer Optionen für die zugewiesenen `platforms` in Betracht ziehen.

1. Wählen **Domains** aus dem Dropdown-Menü aus.

   >[!IMPORTANT]
   >
   > Im Rahmen des Client-Registrierungsprozesses kann die Client-Anwendung beantragen, dass ihr die Verwendung einer Umleitungs-URL für die Fertigstellung des Authentifizierungsflusses erlaubt wird. Wenn ein Client-Programm eine bestimmte Umleitungs-URL verwendet, wird es anhand der in dieser Auswahl ausgewählten `domains` validiert.

1. Geben Sie **Name** der Anwendung ein.

1. Geben Sie die **Version** der Anwendung ein.

   >[!IMPORTANT]
   >
   > Es wird empfohlen, für jedes größere Update Ihrer Client-Anwendung eine neue registrierte Anwendung zu erstellen, um deren Lebenszyklus und Nutzung zu verwalten. Erstellen Sie bei Bedarf ein Ticket über unseren [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren Technical Account Manager (TAM), eine registrierte Anwendung zu widerrufen, um die Funktionalität einer bestimmten Client-Anwendungsversion zu blockieren.

1. Wählen Sie **Typ** Wert „DIRECT“ aus dem Dropdown-Menü aus.

1. Wählen Sie **Anwendung hinzufügen** aus.

Eine neue Konfigurationsänderung wurde erstellt und ist für die Server-Aktualisierung bereit. Um die neue registrierte Anwendung zu verwenden, die im Abschnitt **Registrierte Anwendungen** aufgeführt ist, fahren Sie mit dem Fluss [Überprüfen und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

#### Software-Erklärung herunterladen {#download-software-statement}

Führen Sie die folgenden Schritte aus, um eine Software-Erklärung herunterzuladen.

1. Bewegen Sie den Mauszeiger über die registrierte Anwendung, von der Sie die Software-Anweisung aus der Liste **Registrierte Anwendungen“** möchten.

1. Wählen Sie **Herunterladen** aus.

   ![Software-Erklärung herunterladen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-download-software-statement-button.png)

   *Software-Erklärung herunterladen*

### Benutzerdefinierte Schemata {#custom-schemes}

Auf dieser Registerkarte wird eine Liste benutzerdefinierter Schemata angezeigt. Weitere Informationen zur Verwendung benutzerdefinierter Schemata finden Sie unter Registrierung der [iOS-/tvOS-Anwendung](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

Sie können die folgenden Änderungen an benutzerdefinierten Schemata vornehmen:

* [Neues benutzerdefiniertes Schema erstellen](#generate-custom-schemes)

#### Neues benutzerdefiniertes Schema erstellen {#generate-custom-schemes}

Führen Sie die folgenden Schritte aus, um ein neues benutzerdefiniertes Schema zu erstellen.

1. Wählen Sie **Neues benutzerdefiniertes Schema erstellen** aus.

   ![Neues benutzerdefiniertes Schema erstellen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-custom-scheme-button.png)

   *Neues benutzerdefiniertes Schema erstellen*

Eine neue Konfigurationsänderung wurde erstellt und ist für die Server-Aktualisierung bereit. Um das neue benutzerdefinierte Schema zu verwenden, das im Abschnitt **Benutzerdefinierte Schemata** aufgeführt ist, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

#### Vererbte benutzerdefinierte Schemata {#inherited-custom-schemes}

Medienunternehmen definieren diese benutzerdefinierten Schemata auf ihrer eigenen Ebene. Alle Kanäle, die demselben Medienunternehmen zugeordnet sind, können diese benutzerdefinierten Schemata verwenden.

![Vererbte benutzerdefinierte Schemata](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-custom-schemes-panel-view.png)

*Vererbte benutzerdefinierte Schemata*

## Neuen Kanal hinzufügen {#add-new-channel}

Gehen Sie wie folgt vor, um einen neuen Kanal hinzuzufügen.

1. Wählen Sie die **Kanäle** im linken Bedienfeld aus.

1. Wählen **oben rechts** Abschnitt „Kanäle“ **Neuen Kanal**.

   ![Neuen Kanal hinzufügen](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-channel-button.png)

   *Neuen Kanal hinzufügen*

1. Wählen **Programmierer-ID** aus dem Dropdown-Menü im Dialogfeld **Neuer Kanal** aus.

1. Geben Sie in die **Kanal-ID“ eine eindeutige Kennung**.

1. Geben Sie den Markennamen des für kommerzielle Zwecke verwendeten Kanals in das Feld &quot;**&quot;**.

1. Wählen Sie **Kanal hinzufügen** aus.

Eine neue Konfigurationsänderung wurde erstellt und ist für die Server-Aktualisierung bereit. Um den neuen Kanal im Abschnitt **Kanäle** zu verwenden, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.
