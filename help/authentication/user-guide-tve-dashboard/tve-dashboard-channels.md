---
title: Kanäle
description: Erfahren Sie mehr über Kanäle und ihre verschiedenen Konfigurationen im TVE-Dashboard.
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# Kanäle {#channels}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle -Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Im Bereich **Kanäle** des TVE-Dashboards können Sie Einstellungen für die Kanäle anzeigen und verwalten, die einem bestimmten Programmierer zugeordnet sind. Sie können entsprechend Ihren Anforderungen auch [einen neuen Kanal hinzufügen](#add-new-channel).

Auf der Registerkarte **Kanäle** im linken Bereich wird eine Liste der verknüpften Kanäle mit den folgenden Details angezeigt:

* **Anzeigename**: Der Markenname des für kommerzielle Zwecke verwendeten Kanals.
* **Kanal-ID**: Eine eindeutige Kennung, die auch als Anforderer-ID bezeichnet wird.
* **Integrationen**: Die Anzahl der Verbindungen, die mit [MVPDs](/help/authentication/kickstart/glossary.md#mvpd) hergestellt wurden.

![Liste der vorhandenen Kanäle](../assets/tve-dashboard/new-tve-dashboard/channels/channels-list-view.png)

*Liste der vorhandenen Kanäle*

Geben Sie den Namen des Kanals in die Leiste **Suchen** oberhalb der Liste ein, um mehr über den Kanal zu erfahren.

## Verwalten von Kanalkonfigurationen {#manage-channel-conf}

Führen Sie die Schritte zum Verwalten verschiedener Einstellungen eines bestimmten Kanals aus.

1. Wählen Sie im linken Bereich die Registerkarte **Kanäle** aus.

1. Wählen Sie den Kanal aus der verfügbaren Liste aus.

1. Wählen Sie eine der folgenden Registerkarten aus, um die entsprechenden Einstellungen des ausgewählten Kanals anzuzeigen und zu bearbeiten:

   * [Allgemeine Einstellungen](#general-settings)
   * [Integrationen](#integrations)
   * [Zertifikate](#certificates)
   * [Domänen](#domains)
   * [Registrierte Anwendungen](#registered-applications)
   * [Benutzerdefinierte Schemata](#custom-schemes)

   ![Kanaleinstellungen](../assets/tve-dashboard/new-tve-dashboard/channels/channel-tabs-view.png)

   *Kanaleinstellungen*

>[!IMPORTANT]
>
> Weitere Informationen zum Aktivieren der Konfigurationsänderungen finden Sie unter [Überprüfen und Pushen von Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) .

### Allgemeine Einstellungen {#general-settings}

Auf dieser Registerkarte werden **Kanalinformationen** und **Analytics-Konfiguration** angezeigt.

#### Kanalinformationen {#channel-information}

In diesem Abschnitt können Sie die folgenden Details bearbeiten:

* **Anzeigename**: Der Markenname des für kommerzielle Zwecke verwendeten Kanals.

* **Standard-Umleitungs-URL**: Die Backup-Umleitungs-URL für Authentifizierung und Abmeldung.

* **Fehlerberichterstellung**: Bei Auswahl von **Ja** senden die Adobe Pass-SDKs Fehlerberichte an das Adobe Pass-Backend für Analysen.

![Kanalinformationen bearbeiten](../assets/tve-dashboard/new-tve-dashboard/channels/channel-general-settings-tab-view.png)

*Kanalinformationen bearbeiten*

#### Analytics-Konfiguration {#analytics-configuration}

In diesem Abschnitt können Sie die Weiterleitung von Adobe Pass-Authentifizierungsereignissen an Adobe Analytics konfigurieren.

Wenden Sie sich zur Aktivierung von **Analytics-Konfiguration** an Ihren technischen Kundenbetreuer (TAM), um weitere Informationen zum Einrichten der Report Suite-ID (RSID) zu erhalten.

![Analytics-Konfigurationen aktivieren](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-analytics-configuration-button.png)

*Analytics-Konfigurationen aktivieren*

Wählen Sie **Neue Analysekonfiguration hinzufügen** aus, um mehrere Konfigurationen hinzuzufügen.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Um die neue Analysekonfiguration aus dem Abschnitt **Analytics-Konfiguration** zu verwenden, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

### Integrationen {#integrations}

In diesem Tab wird eine Liste der verfügbaren Integrationen zwischen dem aktuell ausgewählten Kanal und den MVPDs angezeigt. Die Liste zeigt jede Integration mit ihrem Status an und zeigt an, ob sie aktiviert ist oder nicht. Wählen Sie eine bestimmte Integration aus dieser Liste aus, um auf detaillierte Informationen im Abschnitt [Integrationen](tve-dashboard-integrations.md) zuzugreifen.

![Liste der verfügbaren Integrationen](../assets/tve-dashboard/new-tve-dashboard/channels/channel-integrations-tab-view.png)

*Liste der verfügbaren Integrationen*

### Zertifikate {#certificates}

Auf dieser Registerkarte wird eine Liste der [verfügbaren Zertifikate](#available-certificates) und [vererbten verfügbaren Zertifikate](#inherited-avail-certificates) angezeigt, die in den Verschlüsselungsflüssen für Benutzermetadaten verwendet werden. Es werden Details zu jedem Zertifikat angezeigt, das Folgendes enthält:

* Der Status (ob für die Verwendung der **Benutzer-Metadatenverschlüsselung** aktiviert ist oder nicht)
* Seriennummer
* Name der Organisation des Emittenten
* Name der Betrefforganisation
* Ausgegebener Zeitpunkt
* Ablaufdatum
* Ein Dropdown-Menü zum Verschlüsseln von Benutzermetadaten (wenn Sie **Ja** auswählen, verschlüsselt das Zertifikat vertrauliche Benutzerinformationen wie z. B. Postleitzahlenwerte).

#### Verfügbare Zertifikate {#available-certificates}

Diese Zertifikate dienen als private oder öffentliche Schlüssel und werden zur Verschlüsselung von Benutzermetadaten verwendet.
Sie können die folgenden Änderungen im Abschnitt &quot;Verfügbare Zertifikate&quot;vornehmen:

* [Neues Zertifikat hinzufügen](#add-new-certificate)
* [Zertifikat löschen](#delete-certificate)

##### Neues Zertifikat hinzufügen {#add-new-certificate}

Gehen Sie wie folgt vor, um ein neues Zertifikat hinzuzufügen:

1. Wählen Sie oben im Abschnitt **Verfügbare Zertifikate** die Option **Neues Zertifikat hinzufügen** aus.

   ![Neues Zertifikat hinzufügen](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-certificate-button.png)

   *Neues Zertifikat hinzufügen*

1. Fügen Sie den öffentlichen Schlüssel Ihres Zertifikats in das Dialogfeld **Neues Zertifikat** ein.

1. Wählen Sie **Zertifikat hinzufügen** aus.

1. Suchen Sie das neue Zertifikat in der Liste der **Verfügbaren Zertifikate**.

   >[!IMPORTANT]
   >
   > Stellen Sie sicher, dass Ihre Systeme auf dem neuesten Stand sind und das neue Zertifikat verwenden können.

1. Wählen Sie im Dropdown-Menü **Verwendet zum Verschlüsseln von Benutzermetadaten** die Option **Ja** aus, um ein neues Zertifikat zu aktivieren.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Um das neue Zertifikat zu verwenden, das im Abschnitt **Verfügbare Zertifikate** aufgeführt ist, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

##### Zertifikat löschen {#delete-certificate}

Führen Sie die folgenden Schritte aus, um ein Zertifikat zu löschen.

1. Bewegen Sie den Mauszeiger über das Zertifikat, das Sie aus der Liste der **Verfügbaren Zertifikate** löschen möchten.

1. Wählen Sie **Entfernen** aus.

   ![Entfernen Sie das ausgewählte Zertifikat](../assets/tve-dashboard/new-tve-dashboard/channels/channel-delete-certificate-button.png)

   *Entfernen Sie das ausgewählte Zertifikat*

1. Wählen Sie **Löschen** aus dem Dialogfeld **Aktives Zertifikat löschen**.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Das Zertifikat wird erst aus dem Abschnitt **Verfügbare Zertifikate** gelöscht, nachdem [Änderungen überprüft und gepusht haben](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Vererbte verfügbare Zertifikate {#inherited-avail-certificates}

Medienunternehmen definieren diese Zertifikate auf ihrer eigenen Ebene. Alle mit demselben Medienunternehmen verknüpften Kanäle können diese Zertifikate verwenden.

![Vererbte verfügbare Zertifikate](../assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-available-certificates-panel-view.png)

*Vererbte verfügbare Zertifikate*

### Domänen {#domains}

Auf dieser Registerkarte wird eine Liste der verfügbaren Domänen angezeigt, über die der jeweilige Kanal mit der Adobe Pass-Authentifizierung kommuniziert.

Sie können die folgenden Änderungen an Domänen vornehmen:

* [Neue Domäne hinzufügen](#add-domains)
* [Domain löschen](#delete-domain)

>[!TIP]
>
> Vermeiden Sie das Hinzufügen einer neuen Subdomain, wenn eine allgemeinere Domäne in der Liste vorhanden ist.

#### Neue Domäne hinzufügen {#add-domains}

Führen Sie die folgenden Schritte aus, um eine Domäne hinzuzufügen.

1. Wählen Sie oben rechts im Abschnitt **Verfügbare Domänen** die Option **Neue Domäne hinzufügen** aus.

   ![Neue Domäne hinzufügen](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-domain-button.png)

   *Neue Domäne hinzufügen*

1. Geben Sie den Namen Ihrer Domäne im Dialogfeld **Neue Domäne** ein.

1. Wählen Sie **Domäne hinzufügen** aus, um eine neue Domäne für den ausgewählten Kanal hinzuzufügen.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Um die neue Domäne zu verwenden, die im Abschnitt **Verfügbare Domänen** aufgeführt ist, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

#### Domain löschen {#delete-domain}

Führen Sie die folgenden Schritte aus, um eine Domäne zu löschen.

1. Bewegen Sie den Mauszeiger über die Domäne, die Sie aus der Liste der **Verfügbaren Domänen** löschen möchten.

1. Wählen Sie **Entfernen** aus.

   ![Entfernen Sie die ausgewählte Domäne](../assets/tve-dashboard/new-tve-dashboard/channels/channel-remove-domain-button.png)

   *Entfernen Sie die ausgewählte Domäne*

1. Wählen Sie **Löschen** im Dialogfeld **Domäne löschen** aus.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Die Domäne wird erst aus dem Abschnitt **Verfügbare Domänen** gelöscht, nachdem [Änderungen überprüft und gepusht hat](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

Die ausgewählte Domäne ist nicht mehr verfügbar. Daher verliert die mit dieser Domäne verknüpfte Anwendung den Zugriff auf die Adobe Pass-Authentifizierungsdienste.

### Registrierte Anwendungen {#registered-applications}

Auf dieser Registerkarte wird eine Liste der registrierten Anwendungen angezeigt. Weitere Informationen zur Verwendung registrierter Anwendungen finden Sie in der Dokumentation [Übersicht zur dynamischen Client-Registrierung](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

Sie können die folgenden Aktionen für registrierte Anwendungen ausführen:

* [Neue registrierte Anwendung hinzufügen](#add-registered-applications)
* [Software-Anweisung herunterladen](#download-software-statement)

#### Neue registrierte Anwendung hinzufügen {#add-registered-applications}

Führen Sie diese Schritte aus, um eine neue registrierte Anwendung hinzuzufügen.

1. Wählen Sie oben rechts im Abschnitt **Registrierte Anwendungen** die Option **Neue Anwendung hinzufügen** aus.

   ![Neue Anwendung hinzufügen](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-application-button.png)

   *Neue Anwendung hinzufügen*

1. Wählen Sie **Plattformen** aus dem Dropdown-Menü im Dialogfeld **Neue Anwendung** aus.

   >[!IMPORTANT]
   >
   > Es wird empfohlen, registrierte Anwendungen mit spezifischeren und eingeschränkteren Berechtigungen zu erstellen, um die Sicherheit zu erhöhen und den unbefugten Zugriff zu verhindern. Daher sollten Sie beim Erstellen registrierter Anwendungen engere Optionen für die zugewiesene `platforms` verwenden.

1. Wählen Sie **Domänen** aus dem Dropdown-Menü aus.

   >[!IMPORTANT]
   >
   > Bei der Client-Registrierung kann die Client-Anwendung anfordern, eine Umleitungs-URL für die Fertigstellung des Authentifizierungsflusses verwenden zu dürfen. Wenn eine Client-Anwendung eine bestimmte Umleitungs-URL verwendet, wird sie anhand des in dieser Auswahl ausgewählten `domains` validiert.

1. Geben Sie den **Namen** der Anwendung ein.

1. Geben Sie die **Version** der Anwendung ein.

   >[!IMPORTANT]
   >
   > Es wird empfohlen, eine neue registrierte Anwendung für jede größere Aktualisierung Ihrer Clientanwendung zu erstellen, um deren Lebenszyklus und Nutzung zu verwalten. Erstellen Sie bei Bedarf ein Ticket über unser [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren technischen Kundenbetreuer (TAM), eine registrierte Anwendung zu widerrufen, um die Funktionalität einer bestimmten Client-Anwendungsversion zu blockieren.

1. Wählen Sie den Wert **Typ** &quot;DIRECT&quot;aus dem Dropdown-Menü aus.

1. Wählen Sie **Anwendung hinzufügen** aus.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Um die im Abschnitt **Registrierte Anwendungen** aufgelistete neue registrierte Anwendung zu verwenden, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

#### Software-Anweisung herunterladen {#download-software-statement}

Führen Sie diese Schritte aus, um eine Softwareanweisung herunterzuladen.

1. Bewegen Sie den Mauszeiger über die registrierte Anwendung, die Sie die Softwareanweisung aus der Liste der **Registrierten Anwendungen** herunterladen möchten.

1. Wählen Sie **Download** aus.

   ![Herunterladen einer Softwareanweisung](../assets/tve-dashboard/new-tve-dashboard/channels/channel-download-software-statement-button.png)

   *Herunterladen einer Softwareanweisung*

### Benutzerdefinierte Schemata {#custom-schemes}

Auf dieser Registerkarte wird eine Liste benutzerdefinierter Schemata angezeigt. Weitere Informationen zur Verwendung benutzerdefinierter Schemas finden Sie in der [Registrierung der iOS/tvOS-Anwendung](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

Sie können die folgenden Änderungen an benutzerdefinierten Schemata vornehmen:

* [Neues benutzerdefiniertes Schema erstellen](#generate-custom-schemes)

#### Neues benutzerdefiniertes Schema generieren {#generate-custom-schemes}

Führen Sie diese Schritte aus, um ein neues benutzerdefiniertes Schema zu erstellen.

1. Wählen Sie **Neues benutzerdefiniertes Schema generieren** aus.

   ![Erstellen eines neuen benutzerdefinierten Schemas](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-custom-scheme-button.png)

   *Erstellen eines neuen benutzerdefinierten Schemas*

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Um das neue benutzerdefinierte Schema zu verwenden, das im Abschnitt **Benutzerdefinierte Schemas** aufgeführt ist, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.

#### Vererbte benutzerdefinierte Schemata {#inherited-custom-schemes}

Medienunternehmen definieren diese benutzerdefinierten Schemas auf ihrer eigenen Ebene. Alle mit demselben Medienunternehmen verknüpften Kanäle können diese benutzerdefinierten Schemas verwenden.

![Vererbte benutzerdefinierte Schemas](../assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-custom-schemes-panel-view.png)

*Vererbte benutzerdefinierte Schemas*

## Neuen Kanal hinzufügen {#add-new-channel}

Führen Sie diese Schritte aus, um einen neuen Kanal hinzuzufügen.

1. Wählen Sie im linken Bereich die Registerkarte **Kanäle** aus.

1. Wählen Sie oben rechts im Abschnitt **Kanäle** die Option **Neuen Kanal hinzufügen** aus.

   ![Hinzufügen eines neuen Kanals](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-channel-button.png)

   *Hinzufügen eines neuen Kanals*

1. Wählen Sie **Programmierer-ID** aus dem Dropdown-Menü im Dialogfeld **Neuer Kanal** aus.

1. Geben Sie eine eindeutige Kennung in die **Kanal-ID** ein.

1. Geben Sie den Markennamen des für kommerzielle Zwecke verwendeten Kanals in den **Anzeigenamen** ein.

1. Wählen Sie **Kanal hinzufügen** aus.

Eine neue Konfigurationsänderung wurde erstellt und kann jetzt aktualisiert werden. Um den neuen Kanal zu verwenden, der im Abschnitt **Kanäle** aufgeführt ist, fahren Sie mit dem Fluss [Überprüfung und Push-Änderungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) fort.
