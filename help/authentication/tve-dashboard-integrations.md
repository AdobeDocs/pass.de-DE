---
title: TVE Dashboard-Integrationen
description: Erfahren Sie mehr über die Integrationen zwischen Ihren Kanälen und MVPDs und über die Verwaltung von Integrationen.
exl-id: 0add340b-120c-4e82-8e3c-6c190d77cf7e
source-git-commit: c2dcea9e4170a3e10654bcd3f8d2f5cdb82c9603
workflow-type: tm+mt
source-wordcount: '2093'
ht-degree: 0%

---

# Integrationen

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle -Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Im Abschnitt **Integrationen** des TVE-Dashboards können Sie Einstellungen für die Integrationen zwischen Ihren Kanälen und MVPDs anzeigen und verwalten. Sie können entsprechend Ihren Anforderungen auch [eine neue Integration erstellen](#create-new-integration).

Auf der Registerkarte **Integrationen** im linken Bereich wird eine Liste der vorhandenen Integrationen mit den folgenden Details angezeigt:

* Status, der angibt, ob die Integration aktuell aktiv oder inaktiv ist
* Integration von bestimmten Kanälen mit entsprechenden MVPDs
* Kanalname mit Kanal-ID
* MVPD-Anzeigename und MVPD-ID

![Liste der vorhandenen Integrationen](assets/integrations-list.png)

*Liste der vorhandenen Integrationen*

Geben Sie den Namen des Kanals oder MVPD in die Leiste **Suchen** oberhalb der Liste ein, um mehr über die Integration zu erfahren.

## Verwalten von Integrationskonfigurationen {#manage-integration-conf}

Führen Sie diese Schritte aus, um eine bestimmte Integration zu verwalten.

1. Wählen Sie im linken Bereich die Registerkarte **Integrationen** aus.
1. Wählen Sie eine Integration aus der bereitgestellten Liste aus, um verschiedene Einstellungen in den folgenden Abschnitten anzuzeigen und zu bearbeiten:

   * [Endpunktauswahl](#endpoint-selection)
   * [Plattformeinstellungen](#platform-settings)
   * [Benutzermetadaten](#user-metadata)

>[!IMPORTANT]
>
> Weitere Informationen zum Aktivieren der Konfigurationsänderungen finden Sie unter [Überprüfen und Pushen von Änderungen](/help/authentication/tve-dashboard-review-push-changes.md) .

### Endpunktauswahl {#endpoint-selection}

In diesem Abschnitt können Sie die Endpunkte des MVPD auswählen, der für die Authentifizierung, Autorisierung und Abmeldung über die entsprechenden Dropdown-Menüs verwendet wird.

![Endpunkte für Authentifizierungs-, Autorisierungs- und Abmeldevorgänge](assets/endpoint-selection.png)

*Endpunkte für Authentifizierungs-, Autorisierungs- und Abmeldevorgänge*

>[!NOTE]
>
>MVPDs können einen oder mehrere Endpunkte für jeden Fluss bereitstellen. Bei der Integration eines neuen Kanals muss der MVPD für jeden Fluss seinen bevorzugten Endpunkt angeben.

>[!IMPORTANT]
>
>Jede Änderung an Endpunkten wirkt sich auf das Gesamtverhalten einer Integration aus. Diese Änderungen sollten erst nach Bestätigung durch den MVPD implementiert werden.

### Plattformeinstellungen {#platform-settings}

In diesem Abschnitt können Sie Integrationseinstellungen für alle [Plattformen](/help/authentication/tve-dashboard-reports.md#platforms) anzeigen und bearbeiten. Sie können diese Einstellungen anhand einzelner Plattformen ändern. Sie können beispielsweise die Dauer der Autorisierungs-TTL in Android anpassen und dabei einen Standardwert für eine andere Plattform beibehalten.

Jede Eigenschaft in den Plattformeinstellungen übernimmt einen vom MVPD festgelegten Standardwert, kann aber bei Bedarf angepasst werden.

>[!IMPORTANT]
>
>Es ist eine Vereinbarung mit dem MVPD erforderlich, um die für jede Eigenschaft in den Plattformeinstellungen festgelegten Werte zu bestimmen.

>[!IMPORTANT]
>
> Die Einstellungsvererbung folgt einer Kette von MVPD-Einstellungen (die am häufigsten verwendet werden), dann von MVPD-Endpunkt, Integration, Plattformkategorie und Plattform (die den spezifischsten Wert enthält).

**Plattformeinstellungen** wird verwendet, um Einstellungen für jede Ebene in der Vererbungskette zu überschreiben. Die verfügbaren Ebenen in der Kette sind wie folgt gruppiert:

* **Standard für alle**: Legen Sie Werte für Eigenschaften fest, die unabhängig von den Implementierungen des Programmierers universell auf allen Plattformen gelten, wenn bestimmte Plattformwerte nicht definiert sind.

* **Desktop-Geräte**: Legen Sie Werte für Eigenschaften fest, die unabhängig von der Programmiermethode (JS-SDK oder REST-API) für alle Desktop- und Laptop-Computer gelten.

* **Mobilgeräte**: Legen Sie Werte für Eigenschaften fest, die für alle Mobilgeräte gelten, einschließlich **iOS**, **Android** und anderen, unabhängig vom Programmierungsansatz (SDK oder REST-API).

* **TV Connected Devices**: Legen Sie Werte für Eigenschaften fest, die für alle TV-angeschlossenen Geräte gelten, einschließlich **tvOS**, **Roku**, **FireTV** und anderen Geräten, unabhängig von der Programmiermethode (SDK oder REST API).

* **Nicht identifizierte Geräte**: Legen Sie Werte für Eigenschaften fest, die für alle Geräte gelten, auf denen der aktuelle Mechanismus die Plattform nicht genau identifizieren kann. In solchen Fällen wenden Sie die restriktivsten Regeln an, die vom MVPD definiert werden.

  ![Kategorie der Plattformen und ihrer Geräte](assets/platform-settings.png)

  *Kategorie der Plattformen und ihrer Geräte*

Auswählen <img alt= "Symbol der Vererbungskette" src="./assets/multiple-icon.svg" width="25"> -Symbol rechts neben jeder Eigenschaft, um die Eigenschaften zu untersuchen, die für jede oben beschriebene Vererbungsstufe verwendet werden.

#### Am häufigsten verwendete Geschäftsabläufe {#most-used-flows}

Der Abschnitt **Plattformeinstellungen** enthält eine Reihe von Eigenschaften, die in verschiedenen Geschäftsabläufen verwendet werden. Die tatsächlichen Eigenschaften können je nach den in der jeweiligen Integration ausgewählten MVPDs variieren. Nachfolgend finden Sie die am häufigsten verwendeten Flüsse:

**AuthN TTL und AuthZ TTL auf allen Plattformen**

>[!IMPORTANT]
>
>TTL-Werte für Authentifizierung (AuthN) TTL und Autorisierung (AuthZ) müssen konsistent mit MVPD-Einstellungen übereinstimmen.

Führen Sie diese Schritte aus, um die TTL für Authentifizierung und Autorisierung für alle Plattformen für eine bestimmte Integration zu ändern.

1. Wählen Sie im linken Bereich die Registerkarte **Integrationen** aus.
1. Wählen Sie die Integration aus, für die Sie die Werte AuthN TTL und AuthZ TTL ändern möchten.
1. Navigieren Sie zum Abschnitt **Plattformeinstellungen** .

1. Wählen Sie die Registerkarte **Standard für alle** unter **Plattformeinstellungen**.

   >[!NOTE]
   >
   >Wenn Sie die Dauer von **AuthN TTL** und **AuthZ TTL** für eine Plattformkategorie oder eine bestimmte Plattform ändern möchten, wählen Sie die Plattform entsprechend aus.

   ![Ändern der TTL-Dauer der AuthN-TTL-AuthZ-TTL für alle Plattformen](assets/authn-ttl-authz-ttl-for-all-platform.png)

   *Ändern der TTL-Dauer der AuthN-TTL-AuthZ-TTL für alle Plattformen*

   **A.** AuthN TTL property **B.** AuthZ TTL property

1. Wählen Sie die Nach-oben- und Nach-unten-Pfeile aus, um die Dauer für die Anzahl der Tage, Stunden, Minuten und Sekunden in den Eigenschaften **AuthN TTL** und **AuthZ TTL** anzupassen.

Die Dauer von **AuthN TTL** und **AuthZ TTL** für alle Plattformen wird erst aktualisiert, nachdem [Änderungen überprüft und gepusht haben](/help/authentication/tve-dashboard-review-push-changes.md).

**Plattform-SSO aktivieren**

>[!IMPORTANT]
>
>Die Eigenschaft **Single Sign On aktivieren** wird ausschließlich auf den Plattformen *iOS, tvOS, Roku und FireTV* unterstützt. Dies gilt nur für Integrationen mit MVPDs, die Single Sign-on für diese Plattformen unterstützen.

Führen Sie diese Schritte aus, um SSO für eine bestimmte Integration und Plattform zu aktivieren oder zu deaktivieren.

1. Wählen Sie im linken Bereich die Registerkarte **Integrationen** aus.
1. Wählen Sie die Integration aus, für die Sie Single Sign-on aktivieren oder deaktivieren möchten.

1. Navigieren Sie zum Abschnitt **Plattformeinstellungen** .

1. Wählen Sie unter **Plattformeinstellungen** eine bestimmte Plattform oder Kategorie von Plattformen aus, für die Sie die einmalige Anmeldung aktivieren möchten.

   ![Single Sign-On für eine bestimmte Plattform aktivieren](assets/single-sign-on.png)

   *Single Sign-On für eine bestimmte Plattform aktivieren*

   **A.** Eigenschaft &quot;Single Sign On&quot;**B.** Eigenschaft &quot;Durchsetzen von Plattformberechtigungen&quot;

1. Wählen Sie **Ja** aus, um die Option **Nein** zu aktivieren oder zu deaktivieren, und wählen Sie im Dropdown-Menü **Single Sign-On aktivieren** die Option aus.

1. Wählen Sie **Ja** aus, um die Option **Nein** zu aktivieren oder zu deaktivieren, und wählen Sie im Dropdown-Menü **Plattformberechtigungen erzwingen** die Option aus.

   Die Eigenschaft **Erzwingen der Plattformberechtigungen** steuert, ob die Entscheidung des Benutzers, den Plattformzugriff auf die Plattform **Zulassen** oder **Ablehnen** zu aktivieren, eingehalten wird.

   Wenn beispielsweise sowohl **Einmalige Anmeldung aktivieren** als auch **Plattformberechtigungen erzwingen** aktiviert sind und der Benutzer sich dafür entscheidet, den Plattformzugriff auf sein TV-Provider-Abonnement zu verweigern, kann die entsprechende Anwendung (Kanal) das Adobe Pass-Authentifizierungstoken nicht verwenden, das von einer anderen Anwendung (Kanal) abgerufen wurde.

Die Eigenschaft **Single Sign On** für eine ausgewählte Plattform wird erst aktiviert oder deaktiviert, nachdem [Änderungen überprüft und gepusht hat](/help/authentication/tve-dashboard-review-push-changes.md).

**Aktivieren der Authentifizierung mit der Startseite**

Führen Sie diese Schritte aus, um die Authentifizierung für OAuth2-basierte MVPDs zu aktivieren oder zu deaktivieren.

1. Wählen Sie im linken Bereich die Registerkarte **Integrationen** aus.
1. Wählen Sie die Integration aus, für die Sie die Authentifizierung für die Startseite aktivieren oder deaktivieren möchten.
1. Navigieren Sie zum Abschnitt **Plattformeinstellungen** .
1. Wählen Sie unter **Plattformeinstellungen** eine bestimmte Plattform oder Kategorie von Plattformen aus, für die Sie die Authentifizierung für die Startseite aktivieren möchten.

   ![Aktivieren der Authentifizierung für eine bestimmte Plattform auf der Basis der Startseite](assets/attempt-hba.png)

   *Aktivieren der Authentifizierung für eine bestimmte Plattform auf der Basis der Startseite*

   **A.** HBA-Eigenschaft versuchen **B.** HBA AuthN TTL-Eigenschaft

1. Wählen Sie **Ja** aus, um zu aktivieren, und **Nein**, um sie zu deaktivieren, aus dem Dropdown-Menü **HBA versuchen** .

>[!IMPORTANT]
>
>Eine Änderung der Dauer der Eigenschaft **HBA AuthN TTL** sollte vermieden werden. Dies kann zu unerwarteten Fehlern im Autorisierungsprozess führen.

Die Eigenschaft **HBA-Versuch** für einen bestimmten MVPD wird erst aktiviert oder deaktiviert, nachdem [Änderungen überprüft und gepusht hat](/help/authentication/tve-dashboard-review-push-changes.md).

#### Hinzufügen weiterer Eigenschaften {#add-more-properties}

Mit **Weitere Eigenschaften hinzufügen** können Sie zusätzliche spezifische Eigenschaften für Integrationen hinzufügen, insbesondere für weniger häufige Flüsse.

Sie können die folgenden Eigenschaften hinzufügen:

* Wählen Sie für alle Plattformen auf der linken Seite für die Registerkarte &quot;Alle **&quot;die Option** Standard .
* Wählen Sie für eine Plattformkategorie links die Registerkarte **Desktop-Geräte**, **Mobilgeräte** oder **TV-vernetzte Geräte** aus.
* Wählen Sie für ein bestimmtes Gerät auf der linken Seite die Registerkarte **iOS**, **Android**, **tvOS**, **Roku** oder **FireTV** aus.

Im Folgenden finden Sie einige Beispiele für verschiedene Flüsse, die durch Hinzufügen der folgenden Eigenschaften aktiviert werden können:

**Ändern der Anzahl für vorab autorisierte Ressourcen**

Die meisten MVPDs unterstützen standardmäßig einen Preflight authZ-Aufruf mit bis zu 5 Ressourcen-IDs.
Wenn MVPDs jedoch zustimmen, diese Beschränkung zu erhöhen, können Sie zum Menü **Weitere Eigenschaften hinzufügen** navigieren und im Optionsmenü die Option **Max. Ressourcen des Preflight** auswählen.

**Preflight Max Resources** fügt ein neues Attribut hinzu, in dem das vereinbarte Limit mit dem MVPD angegeben werden kann.

![Eigenschaft &quot;Preflight Max Resources&quot; hinzufügen](assets/preflight-max-resources.png)

*Eigenschaft &quot;Preflight Max Resources&quot; hinzufügen*

Die Eigenschaft **Max. Ressourcen für Preflight** wird erst hinzugefügt, nachdem [Änderungen überprüft und gepusht haben](/help/authentication/tve-dashboard-review-push-changes.md).

**Ändern des MVPD-Anzeigenamens oder der Logo-URL**

Für Programmierer-Anwendungen, die nicht ihre MVPD-Auswahl erstellen möchten und stattdessen auf bereitgestellte Konfigurationen angewiesen sind, können Sie zur &quot;**Add more properties**&quot;navigieren und &quot;**Display Name**&quot;oder &quot;**Logo URL**&quot; auswählen, um den erforderlichen Anzeigenamen oder Logo-URLs für jeden MVPD im Optionsmenü hinzuzufügen.

Je nach Geräteplattform und gewünschtem Benutzererlebnis können für dasselbe MVPD unterschiedliche Werte für diese Eigenschaften verwendet werden.

![Anzeigename oder Logo-URL-Eigenschaft hinzufügen](assets/displayname-logourl.png)

*Anzeigename oder Logo-URL-Eigenschaft hinzufügen*

Die Eigenschaft **Anzeigename** oder **Logo-URL** wird erst hinzugefügt, nachdem [Änderungen überprüft und gepusht hat](/help/authentication/tve-dashboard-review-push-changes.md).

**Anfordern eines neuen Authentifizierungsflusses beim Wechseln der App (des Kanals)**

Wenn Sie beim Wechsel zwischen Apps eine neue Authentifizierung erzwingen möchten. In diesem Fall können Sie zur Eigenschaft **Weitere Eigenschaften hinzufügen** navigieren und die Eigenschaft **Auth pro Aggregator** auswählen.

Durch Hinzufügen von **Auth pro Aggregator** wird das Single Sign-on für den jeweiligen Kanal effektiv unterbrochen.

![Auth pro Aggregatoreigenschaft hinzufügen](assets/auth-per-aggregator.png)

*Auth pro Aggregatoreigenschaft hinzufügen*

Die Eigenschaft **Auth per Aggregator** wird erst hinzugefügt, nachdem [Änderungen überprüft und gepusht hat](/help/authentication/tve-dashboard-review-push-changes.md).

Wählen Sie nach dem Hinzufügen **Ja** aus, um die Eigenschaft **Auth pro Aggregator** für eine ausgewählte Integration zu aktivieren.

#### Eigenschaften löschen {#delete-properties}

Auswählen <img alt= "Schaltfläche &quot;Eigenschaft löschen&quot;" src="./assets/delete-icon.svg" width="25"> rechts neben jeder Eigenschaft, um nicht mehr benötigte Eigenschaften zu löschen.

>[!NOTE]
>
>Bestimmte Eigenschaften können nicht entfernt werden, da sie obligatorische Anforderungen für den ausgewählten MVPD sind.

Die Eigenschaft wird erst aus dem Abschnitt **Plattformeinstellungen** gelöscht, nachdem [Änderungen überprüft und gepusht hat](/help/authentication/tve-dashboard-review-push-changes.md).

### Benutzermetadaten {#user-metadata}

In diesem Abschnitt können Sie Einstellungen für jeden Benutzer-Metadatenparameter aktualisieren, der vom MVPD freigegeben wird.

>[!NOTE]
>
>Jeder MVPD kann verschiedene Parameter gemeinsam nutzen. Weitere Informationen zu den Parametern, die ein bestimmter MVPD freigeben kann, erhalten Sie von Ihrem Adobe-Support-Mitarbeiter.

Im Abschnitt für Benutzermetadaten werden die folgenden Spalten angezeigt:

**Schlüssel**: Stellt die tatsächlichen Benutzer-Metadatenparameter dar, die in der API zum Extrahieren von Werten verwendet werden sollen.

**Beschreibung**: Bietet eine kurze Beschreibung der einzelnen Benutzer-Metadatenparameter.

**Verschlüsselt**: In dieser Spalte können Sie Parameter in der API aktivieren oder deaktivieren, indem Sie im Dropdown-Menü **Ja** bzw. **Nein** auswählen. Die Auswahl von **Ja** bedeutet, dass der Parameterwert in der API verschlüsselt wird. Die Verschlüsselung wird mithilfe eines Zertifikats durchgeführt, das durch einen Gültigkeitsbereich von **Benutzer-Metadaten** definiert ist.

>[!TIP]
>
>
> Stellen Sie stets sicher, dass der Parameter **ZIP** verschlüsselt ist.

Weitere Informationen zu verfügbaren Zertifikaten finden Sie in den Abschnitten [Programmierer](/help/authentication/tve-dashboard-programmers.md#available-certificates) und [Kanäle](/help/authentication/tve-dashboard-channels.md#available-certificates) .

**Aktiviert**: In dieser Spalte können Sie die Parameter in der API aktivieren bzw. deaktivieren, indem Sie im Dropdown-Menü **Ja** bzw. **Nein** auswählen.

![Für Benutzermetadaten verfügbare Parameter](assets/user-metadata.png)

*Für Benutzermetadaten verfügbare Parameter*

## Neue Integration erstellen {#create-new-integration}

Gehen Sie wie folgt vor, um eine neue Integration mit einem neuen MVPD in Ihrem aktuellen Setup zu erstellen:

1. Wählen Sie im linken Bereich die Registerkarte **Integrationen** aus.
1. Wählen Sie oben rechts im Abschnitt **Integrationen** die Option **Neue Integration erstellen** .

   ![Erstellen einer neuen Integration](assets/create-new-integration.png)

   *Erstellen einer neuen Integration*

   Die folgenden Abschnitte werden angezeigt:

   **Kanal und MVPD auswählen**

   Wählen Sie einen **Kanal** aus dem Dropdown-Menü **Kanal auswählen** aus, um eine neue Integration hinzuzufügen. Nachdem Sie den Kanal ausgewählt haben, wählen Sie den erforderlichen **MVPD** aus dem Dropdown-Menü **MVPD auswählen** aus, um ihn in den ausgewählten Kanal zu integrieren.

   ![Kanal und MVPD auswählen](assets/select-channel-mvpd.png)

   *Kanal und MVPD auswählen*

   **Endpunkte auswählen**

   Nach Auswahl des erforderlichen MVPD wird der Abschnitt **Endpunkt auswählen** mit den für diesen MVPD konfigurierten Standardendpunkten vorausgefüllt.

   >[!IMPORTANT]
   >
   >Ändern Sie die Standard-Endpunkte in keinem Fluss, es sei denn, dies wird speziell vom MVPD angegeben.

   ![Endpunkte auswählen ](assets/select-endpoints.png)

   *Endpunkte auswählen*

   **Zusätzliche Informationen**

   Dieser Abschnitt enthält verschiedene Eigenschaften, die für den ausgewählten MVPD im Abschnitt **Kanal auswählen und MVPD** konfiguriert werden müssen.

   >[!NOTE]
   >
   > Die tatsächlichen Eigenschaften können je nach den im Abschnitt **Kanal auswählen und MVPD** ausgewählten MVPDs unterschiedlich sein.

   Sie können beispielsweise die **AuthN TTL** oder die **Partner-ID** (Kanal-ID) für Co-Branding-Zwecke auf der MVPD-Anmeldeseite in der folgenden Abbildung bearbeiten.

   ![Zusätzliche Informationen bearbeiten](assets/additional-information.png)

   *Zusätzliche Informationen bearbeiten*

   Wählen Sie oben rechts im Abschnitt **Neue Integration erstellen** die Option **Integration speichern**.

Eine neue Integration wird erst erstellt, nachdem [Änderungen überprüft und gepusht hat](/help/authentication/tve-dashboard-review-push-changes.md).


## Integration deaktivieren {#disable-integratgion}

Gehen Sie wie folgt vor, um eine Integration zu deaktivieren:

1. Wählen Sie im linken Bereich die Registerkarte **Integrationen** aus.
1. Wählen Sie die Integration aus, die Sie deaktivieren möchten.
1. Deaktivieren Sie den Umschalter, der oben rechts in der ausgewählten Integration verfügbar ist.

   ![Integration deaktivieren](assets/disable-integration.png)

   *Integration deaktivieren*

Die Integration wird erst deaktiviert, nachdem [Änderungen überprüft und gepusht hat](/help/authentication/tve-dashboard-review-push-changes.md).

Nachdem die Integration deaktiviert wurde, verlieren Endbenutzer die Möglichkeit, sich mit dem spezifischen MVPD zu authentifizieren oder zu autorisieren.
