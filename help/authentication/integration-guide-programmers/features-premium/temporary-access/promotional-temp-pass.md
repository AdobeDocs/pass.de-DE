---
title: Weiterleitungs-Temp-Pass
description: Weiterleitungs-Temp-Pass
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1510'
ht-degree: 0%

---

# Weiterleitungs-Temp-Pass {#promotional-temp-pass}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Funktionszusammenfassung {#feature-summary}

Mit dem temporären Weiterleitungs-Pass können Programmierer Benutzern, die keine Kontoanmeldeinformationen mit einem MVPD haben, temporären Zugriff auf ihren geschützten Inhalt anbieten.

Der Promotional Temp Pass ist für die Durchführung von Werbekampagnen konzipiert, bei denen ein Benutzer nach der Bereitstellung gültiger Identifizierungsinformationen (z. B. E-Mail-Adresse) an den Programmierer eine **vordefinierte Anzahl verschiedener VOD-Titel für einen vordefinierten Zeitraum verarbeiten kann**.

>[!IMPORTANT]
>
>Adobe speichert keine personenbezogenen Daten (PII). Daher muss der Programmierer einen Hash über die vom Unique User bereitgestellten Informationen über die Adobe Pass-Authentifizierungs-APIs festlegen.

Der temporäre Weiterleitungs-Pass basiert auf der Funktion [Temp Pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) , d. h. er umfasst alle Funktionen für den Vorübergehenden Pass.

Sobald die vordefinierte maximale Anzahl von VOD-Titeln oder der vordefinierte Zeitraum überschritten ist, kann dieser Benutzer erst dann auf Inhalte auf demselben Gerät oder mithilfe derselben Benutzerkennung (z. B. E-Mail-Adresse) zugreifen, wenn die Autorisierungstoken vom Adobe Pass-Authentifizierungsserver gelöscht werden.

>[!NOTE]
>
>Der Temp Pass ist Teil des Premium Workflow-Pakets. Wenden Sie sich an Ihren Adobe Pass-Vertriebsmitarbeiter, wenn Sie diese Funktion nutzen möchten.

## Vergleich von Temp Pass und Promotions-Temp-Pass {#tp-ptp-comparison}

| Temporärer Pass | Weiterleitungs-Temp-Pass |
|----------------------------------|----------------------------------------------------------------------------------------|
| Inhaltszugriff <ul><li>zeitbasiert</li></ul> | Inhaltszugriff <ul><li>zeitbasiert</li><li>basierend auf der Anzahl der Ressourcen</li></ul> |
| Zugriffssicherheit basierend auf <ul><li>Geräte-ID</li></ul> | Sicherheit basierend auf <ul><li>Geräte-ID</li><li>Hash über die bereitgestellten Benutzerkennungsinformationen (z. B. E-Mail)</li></ul> |
| Client-Fehler-API verfügbar | Client-Fehler-API verfügbar |
| Zurücksetzen/Bereinigen verfügbar | Zurücksetzen/Bereinigen verfügbar |

## Funktionsdetails {#feature-details}

Diese Funktion ermöglicht es Benutzern, von einem bestimmten Gerät (Telefon und Tablet) aus auf Werbeinhalte zuzugreifen, nachdem sie eindeutige Informationen wie die E-Mail-Adresse in der Anwendung des Programmierers bereitgestellt haben.

Der Programmierer stellt einen Hash über die PII des Benutzers in den Authentifizierungs- und Autorisierungs-APIs bereit. Dieser Hash wird zusammen mit der Geräte-ID verwendet, um einen eindeutigen Schlüssel zur Identifizierung des Benutzers und des Geräts zu generieren.

Basierend auf der Geräte-ID und den vom Benutzer angegebenen Informationen und entsprechend der unten stehenden Logik bestimmt die Adobe Pass-Authentifizierung, ob sich der Benutzer in einer neuen oder in einer vorhandenen Testphase befindet:

* neuer Hash über vom Benutzer bereitgestellte Informationen (z. B. E-Mail), neue Geräte-ID => neue Testversion
* vorhandener Hash über vom Benutzer bereitgestellte Informationen (z. B. E-Mail), neue Geräte-ID => vorhandene Testversion (mit vorhandenem Hash über den Benutzer bereitgestellte Informationen (z. B. E-Mail))
* neuer Hash über vom Benutzer bereitgestellte Informationen (z. B. E-Mail), vorhandene Geräte-ID => vorhandene Testversion (mit vorhandener Geräte-ID)
* vorhandener Hash über vom Benutzer bereitgestellte Informationen (z. B. E-Mail), vorhandene Geräte-ID => vorhandene Testversion

>[!NOTE]
>Die Validierung und das Hashing der vom Benutzer bereitgestellten Informationen wird vom Programmierer und nicht von Adobe durchgeführt.

**Die Funktion zum Weiterleiten von Vorlagenübergängen kann anhand der folgenden Eigenschaften konfiguriert werden:**

* Vom Benutzer bereitgestellter Informationsschlüssel (z. B. E-Mail)
* Anzahl der Ressourcen, zu deren Nutzung der Benutzer berechtigt ist
* TTL - das Zeitintervall, in dem der Benutzer berechtigt ist, die konfigurierte Anzahl von Ressourcen zu nutzen

### Benutzermetadaten {#user-metadata}

Um die Implementierung der Programmeranwendung zu erleichtern, werden die folgenden **Benutzer-Metadateninformationen** im Promotional Temp Pass mit entsprechenden Schlüsseln angezeigt (zum Aktivieren der Schlüssel wenden Sie sich an tve-support@adobe.com):

* **rest_resources**: die Anzahl der verbleibenden Ressourcen, die der aktuelle Benutzer nutzen darf
* **used_assets**: die Liste der Ressourcen, die der aktuelle Benutzer bereits verbraucht hat
* **expiration_date**: Das Ablaufdatum des aktuellen Benutzers

### Wie wird die Anzeigezeit berechnet? {#compute-viewing-time}

Die Dauer, die ein Temp-Pass gültig bleibt, korreliert nicht mit der Zeit, die ein Benutzer mit der Anzeige von Inhalten in der Anwendung des Programmierers verbringt. Bei der ersten Benutzeranfrage zur Autorisierung über den Promotional Temp Pass wird eine Ablaufzeit berechnet, indem die anfängliche aktuelle Anforderungszeit der vom Programmierer angegebenen TTL (duration time interval) hinzugefügt wird.

### Authentifizierung und Autorisierung {#authn-authz}

Bei temporären Weiterleitungs-Weiterleitungsflüssen kommunizieren Authentifizierung und Autorisierung nicht mit einem tatsächlichen MVPD, **sodass alle Autorisierungsanfragen erfolgreich sind** , solange alle diese Bedingungen erfüllt sind:

* Autorisierungstoken sind für bestimmte Ressourcen gültig
* Die Anzahl der **used_assets** ist niedriger als die vom Programmierer festgelegte Grenze.
* Der Wert **expiration_date** liegt nach dem aktuellen Datum.

### Preflight-Verhalten {#preflight-beh}

Wenn ein Preflight- oder eine Vorabautorisierungsanfrage für einen Promotional Temp Pass MVPD gestellt wird, enthält die entsprechende zurückgegebene Preflight-Antwort die gesamte Liste der Ressourcen aus der Preflight-Anfrage als Preflight-Erfolg.

Die Logik dahinter ist: Die Autorisierungsbedingungen für den temporären Weiterleitungs-Pass basieren auf der Zeit- und Ressourcennummerbegrenzung und nicht auf bestimmten Ressourcen. Insbesondere werden die aufgerufenen Ressourcen autorisiert, solange die Zeitbeschränkung eingehalten und die Ressourcenbegrenzung nicht überschritten wird.

### SSO {#sso}

SSO ist nicht für Instanzen von Promotional Temp Pass aktiviert, der mit aktivierter Option &quot;Authentifizierung pro Anforderer&quot;konfiguriert ist. Das bedeutet, dass der Benutzer beim Wechsel von Anwendung A zu einer anderen Anwendung B, die beide mit demselben Promotional Temp Pass integriert sind, nicht automatisch angemeldet wird.

### Abmelden {#logout}

Alle Token auf einem Gerät werden beim Abmelden gelöscht. Daher sollte der Wechsel vom Promotional Temp Pass zu einem regulären, vom Benutzer ausgewählten MVPD nicht auf diese Implementierung angewiesen sein. Es wird empfohlen, die Funktion `setSelectedProvider(null)` zu verwenden, um den Anwendungsstatus zu löschen und dann den Authentifizierungsfluss neu zu starten, was ein besseres Benutzererlebnis bietet.

### Flussdiagramm für den temporären Weiterlauf der Promotion {#promo-tempass-flowdia}

![Flussdiagramm für den temporären Weiterlauf der Promotion](../../../assets/promo-temp-pass-flow.png)

*Abbildung: Fluss für den temporären Weiterleitungstyp*

## Implementieren des Weiterleitungs-Temp-Übergangs {#impl-promo-tempass}

Für den vorübergehenden Weiterlauf der Werbeaktion sind die folgenden clientseitigen Funktionen erforderlich:

* **Informationen zur Benutzerkennung, z. B. Verbreitung der E-Mail-Adresse** (Senden der E-Mail-Adresse des Benutzers für die Authentifizierungs- und Autorisierungsflüsse). Die E-Mail ist für die Adobe Pass-Authentifizierung erforderlich, um die Authentifizierungs- und Autorisierungstoken zu binden (ähnlich wie bei `device_ID` , erforderlich für alle Aufrufe).
* **Authentifizierung erzwingen** - Ermöglicht dem Programmierer, einen Authentifizierungsfluss zu erzwingen, wenn der Benutzer bereits authentifiziert ist. Diese Funktion ist erforderlich, um eine Aktualisierung der Benutzermetadaten zu erzwingen (der Benutzer-Metadatenschlüssel **used_assets** enthält die Anzahl der verfügbaren Ressourcen), sobald die App gestartet wird. Da sich der Benutzer auf mehreren Geräten anmelden kann, sind die Benutzermetadaten, die beim Start der App auf dem Gerät vorhanden sind, nicht zuverlässig. Daher müssen wir sie aktualisieren, um den aktuellen Status für diesen bestimmten Benutzer (identifiziert durch E-Mail-Adresse) widerzuspiegeln.


>[!IMPORTANT]
>Authentifizierung erzwingen ist nur auf iOS und Android möglich.
>Die Adobe Pass-Authentifizierung verfügt nicht über einen integrierten Mechanismus, um das kostenlose Streaming nach Ablauf der X Minuten zu stoppen. Die Adobe Pass-Authentifizierung stellt die Ausgabe von Token für **Autorisierung** und **Kurzmedien** ein, sobald der Benutzer die kostenlosen Y-Ressourcen nutzt. Es ist Sache der Programmierer, den Zugriff einzuschränken, sobald der Promotiontempass abläuft.

## Sicherheit {#security}

>[!IMPORTANT]
>Adobe speichert keine personenbezogenen Daten (PII). Daher muss der Programmierer einen Hash über die vom Unique User bereitgestellten Informationen über die Adobe Pass-Authentifizierungs-APIs festlegen.

**Hashing of user identifier information**

Adobe empfiehlt die Verwendung der **SHA-2**-Familie oder ihrer spezifischen **SHA-256**-, **SHA-512**-Funktionen für Daten, bevor sie an Adobe gesendet werden.

Beispiel: **SHA-256** over **&quot;user@domain.com&quot;** is **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a3 32b18b88d09069fb7&quot;**.

## Zurücksetzen oder Bereinigen des Weiterleitungs-Temp-Übergangs {#reset-promo-tempass}

Bestimmte Geschäftsregeln erfordern eine regelmäßige Bereinigung von &quot;Promotional Temp Pass&quot;. Dazu stellt die Adobe Pass-Authentifizierung Programmierern eine Web-API für *public* bereit, wie nachfolgend beschrieben:

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protokoll: **https**</li><li>Host:<ul><li>Release: **mgmt.auth.adobe.com**</li><li>Prequal: **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Pfad: **/reset-tempass/v2/reset**</li><li>Abfrageparameter: **device_id=all&amp;requestor_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>headers: APIKey: **1232293681726481**</li> <li>Antwort:<ul><li>Erfolg: **HTTP 204**</li><li>Fehler: **HTTP 400** für falsche Anforderungen, **HTTP 401**, wenn ApiKey nicht angegeben ist, **HTTP 403**, wenn ApiKey ungültig ist</li></ul></li></ul> |

Zusätzlich zu den Anforderungen zum Bereinigen des Temp-Übergangs verwendet der Promotional Temp Pass den Hash über die Informationen zur Benutzerkennung, die bei Authentifizierung und Autorisierung zur Bereinigung als **generic_data** gesendet wurden.

Der Hash wird gesendet, nicht die gesamte JSON:

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Unterstützte Clients {#supported-clients}

| Adobe Pass-Authentifizierungs-Clients | Weiterleitungs-Temp-Pass | Tool zurücksetzen | Unterstützt dedizierten Antwort-Code/Client-Fehler |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| JS Access Enabler | JA | JA | JA (ab Version 3.0.0) |
| Nativer Client iOS | JA | JA | JA (ab Version 1.10) |
| Nativer Client Android | JA | JA | JA |
| Clientlose API | JA | JA | NO |


## Einschränkungen {#limitations}

In diesem Abschnitt werden die Einschränkungen beschrieben, die für die aktuelle Implementierung von &quot;Promotional Temp Pass&quot;gelten.

### Clientless {#lim-clientless}

**Smart-Geräte ohne eindeutige Geräte-ID**

Nicht alle Apps mit intelligenten Geräten können eine eindeutige Geräte-ID bereitstellen. Wenn keine vorhanden ist, kann die Adobe Pass-Authentifizierung die vom Adobe-Registrierungs-Code-Dienst generierte UUID als eindeutige Geräte-ID verwenden. Das bedeutet, dass bei der Abmeldung des Benutzers die Authentifizierungs- und Autorisierungstoken gelöscht werden. Sobald der Benutzer versucht, sich erneut zu authentifizieren, kann er dieses Mal mit verschiedenen Benutzerinformationen (z. B. E-Mail) erneut autorisieren. Adobe empfiehlt, einen UI-Fluss hinzuzufügen, der es einem Benutzer nicht erlaubt, das System zu &quot;täuschen&quot;und Logik hinzuzufügen, um zu bestimmen, ob es sich um einen neuen Benutzer handelt, der eine Testphase oder eine bestehende Testphase anfordert.

**Zurücksetzen/Bereinigen des Temp-Bestands**

&quot;Temp-Pass für ClientLess zurücksetzen&quot;ist in den spezifischen Fällen von Xbox360 und Xbox One nicht verfügbar, da diese Plattformen zusätzliche Geräte-ID-Parsing erfordern, was im Tool &quot;Temp-Pass zurücksetzen&quot;nicht möglich ist.

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
