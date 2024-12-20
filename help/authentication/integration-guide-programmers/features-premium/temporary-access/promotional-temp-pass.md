---
title: Temporärer Pass für Werbeaktionen
description: Temporärer Pass für Werbeaktionen
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1510'
ht-degree: 0%

---

# Temporärer Pass für Werbeaktionen {#promotional-temp-pass}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Funktionsübersicht {#feature-summary}

Mit dem Werbe-Temp-Pass können Programmierer Benutzern, die keine Anmeldeinformationen für ein MVPD haben, temporären Zugriff auf ihre geschützten Inhalte anbieten.

Der temporäre Werbeausweis ist für die Ausführung von Werbekampagnen konzipiert, bei denen ein Benutzer, nachdem er dem Programmierer gültige Identifizierungsinformationen (z. B. eine E-Mail-Adresse) bereitgestellt hat, in der Lage ist, eine **vordefinierte Anzahl verschiedener VOD-Titel für einen vordefinierten Zeitraum zu**.

>[!IMPORTANT]
>
>Adobe speichert keine personenbezogenen Daten (PII). Daher muss der Programmierer einen Hash über die eindeutigen Benutzerinformationen setzen, die in den Adobe Pass-Authentifizierungs-APIs bereitgestellt werden.

Der Werbe-Temp-Pass basiert auf der Funktion [Temp Pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md), d. h. er enthält alle Funktionen für den Temp-Pass.

Sobald die maximale vordefinierte Anzahl von VOD-Titeln oder der vordefinierte Zeitraum überschritten ist, kann dieser Benutzer erst dann auf Inhalte auf demselben Gerät oder unter Verwendung derselben Benutzerkennung (z. B. E-Mail-Adresse) zugreifen, wenn die Autorisierungs-Token vom Adobe Pass-Authentifizierungsserver gelöscht wurden.

>[!NOTE]
>
>Temp Pass ist Teil des Premium-Workflow-Pakets. Wenden Sie sich an Ihren Adobe Pass-Vertriebsmitarbeiter, wenn Sie diese Funktion verwenden möchten.

## Temporärer Pass und Vergleich der Werbe-Temp-Pass {#tp-ptp-comparison}

| Temp Pass | Temporärer Pass für Werbeaktionen |
|----------------------------------|----------------------------------------------------------------------------------------|
| Zugriff auf Inhalte <ul><li>zeitbasiert</li></ul> | Zugriff auf Inhalte <ul><li>zeitbasiert</li><li>Basierend auf der Anzahl der Ressourcen</li></ul> |
| Zugriffssicherheit basierend auf <ul><li>Geräte-ID</li></ul> | Sicherheit basierend auf <ul><li>Geräte-ID</li><li>Hash über bereitgestellten Benutzerkennung-Informationen (z. B. E-Mail)</li></ul> |
| Client-Fehler-API verfügbar | Client-Fehler-API verfügbar |
| Zurücksetzen/Bereinigen verfügbar | Zurücksetzen/Bereinigen verfügbar |

## Funktionsdetails {#feature-details}

Diese Funktion ermöglicht es Benutzenden, auf Werbeinhalte von einem bestimmten Gerät (Smartphone und Tablet) zuzugreifen, nachdem sie eindeutige Informationen wie die E-Mail-Adresse im Programm des Programmierers bereitgestellt haben.

Der Programmierer stellt einen Hash-Wert über die PII des Benutzers in den Authentifizierungs- und Autorisierungs-APIs bereit. Dieser Hash wird zusammen mit der Geräte-ID zum Generieren eines eindeutigen Schlüssels verwendet, um den Benutzer und das Gerät zu identifizieren.

Basierend auf der Geräte-ID und den vom Benutzer angegebenen Informationen und der folgenden Logik bestimmt die Adobe Pass-Authentifizierung, ob der Benutzer sich in einer neuen oder einer bestehenden Testversion befindet:

* neuer Hash-Wert für vom Benutzer bereitgestellte Informationen (z. B. E-Mail), neue Geräte-ID => neue Testversion
* vorhandener Hash über von Benutzenden bereitgestellte Informationen (z. B. E-Mail), neue Geräte-ID => vorhandene Testversion (mit vorhandenem Hash über von Benutzenden bereitgestellte Informationen (z. B. E-Mail))
* neuer Hash-Wert für von Benutzenden bereitgestellte Informationen (z. B. E-Mail), vorhandene Geräte-ID => vorhandene Testversion (mit vorhandener Geräte-ID)
* vorhandener Hash über von Benutzenden bereitgestellte Informationen (z. B. E-Mail), vorhandene Geräte-ID => vorhandene Testversion

>[!NOTE]
>Die Validierung und das Hashing der vom Benutzer bereitgestellten Informationen werden vom Programmierer und nicht vom Adobe verarbeitet.

**Die Funktion „Promotion Temp Pass“ kann basierend auf den folgenden Eigenschaften konfiguriert werden:**

* Vom Benutzer bereitgestellter Informationsschlüssel (z. B. E-Mail)
* Anzahl der Ressourcen, die der Benutzer nutzen darf
* TTL : das Zeitintervall, in dem der Benutzer berechtigt ist, die konfigurierte Anzahl von Ressourcen zu nutzen

### Benutzermetadaten {#user-metadata}

Um die Implementierung der Programmieranwendung zu erleichtern, werden die folgenden **Benutzermetadaten-Informationen“** Werbe-Temp-Pass mit entsprechenden Schlüsseln bereitgestellt (zum Aktivieren der Schlüssel kontaktieren Sie tve-support@adobe.com):

* **remaining_resources**: Die Anzahl der verbleibenden Ressourcen, die der aktuelle Benutzer nutzen darf
* **used_assets**: Die Liste der Ressourcen, die der aktuelle Benutzer bereits verbraucht hat
* **expiration_date**: Das Ablaufdatum des aktuellen Benutzers

### Wie wird die Betrachtungszeit berechnet? {#compute-viewing-time}

Die Zeit, die ein temporärer Pass gültig bleibt, entspricht nicht der Zeit, die ein Benutzer mit der Anzeige von Inhalten im Programm des Programmierers verbringt. Bei der ersten Benutzeranfrage um Autorisierung über einen temporären Pass für Werbeangebote wird eine Ablaufzeit berechnet, indem die anfängliche aktuelle Anforderungszeit zur vom Programmierer angegebenen TTL (duration time interval) hinzugefügt wird.

### Authentifizierung und Autorisierung {#authn-authz}

Bei Flüssen mit temporärem Pass für Werbeangebote kommunizieren die Authentifizierung und Autorisierung nicht mit einer tatsächlichen MVPD **daher werden alle Autorisierungsanfragen erfolgreich**, solange alle diese Bedingungen erfüllt sind:

* Autorisierungs-Token sind für angegebene Ressourcen gültig
* Die Anzahl **used_assets** ist niedriger als die vom Programmierer festgelegte Grenze
* **expiration_date** Wert liegt nach dem aktuellen Datum.

### Verhalten vor dem Flug {#preflight-beh}

Wenn eine PreFlight- oder PreAuthorization-Anfrage für einen Promotion Temp Pass MVPD gestellt wird, enthält die entsprechende Preflight-Antwort, die zurückgegeben wird, die gesamte Liste der Ressourcen aus der Preflight-Anfrage als Preflight-Erfolg.

Die Logik dahinter ist: Die Genehmigungsbedingungen für den vorübergehenden Durchlauf im Rahmen einer Promotion basieren auf dem Limit für Zeit und Ressourcennummern und nicht auf bestimmten Ressourcen. Genauer gesagt werden die aufgerufenen Ressourcen autorisiert, solange die Zeitbeschränkung eingehalten wird und das Limit für Ressourcen nicht überschritten wird.

### SSO {#sso}

SSO ist nicht für Instanzen des temporären Durchlaufs für Werbeaktionen aktiviert, bei denen die „Authentifizierung pro Antragsteller“ aktiviert ist. Das bedeutet, dass, wenn der Benutzer von Anwendung A zu einer anderen Anwendung B wechselt, die beide mit demselben Werbe-Temp-Pass integriert sind, der Benutzer nicht automatisch angemeldet wird.

### Abmelden {#logout}

Alle Token auf einem Gerät werden beim Abmelden gelöscht. Daher sollte der Wechsel vom temporären Pass für Werbeangebote zu einem regulären benutzerausgewählten MVPD nicht von dieser Implementierung abhängig sein. Es wird empfohlen, die Funktion `setSelectedProvider(null)` zu verwenden, um den Anwendungsstatus zu löschen und dann den Authentifizierungsfluss neu zu starten, was ein besseres Benutzererlebnis bietet.

### Diagramm zum Werbe-Temp-Pass {#promo-tempass-flowdia}

![Diagramm zum temporären Pass zu Werbezwecken](../../../assets/promo-temp-pass-flow.png)

*Abbildung: Zeitüberschreitungsfluss der Promotion*

## Implementieren des temporären Durchgangs für Werbeaktionen {#impl-promo-tempass}

Für den temporären Durchlauf für Werbeaktionen sind die folgenden Client-seitigen Funktionen erforderlich:

* **Informationen zur Benutzerkennung, z. B. zur Verbreitung** E-Mail-Adresse (Versand der E-Mail-Adresse des Benutzers bei Authentifizierungs- und Autorisierungsflüssen). Die E-Mail wird von der Adobe Pass-Authentifizierung benötigt, um die Authentifizierungs- und Autorisierungs-Token zu binden (ähnlich wie beim -`device_ID`, der für alle -Aufrufe erforderlich ist).
* **Authentifizierung erzwingen** - ermöglicht es dem Programmierer, einen Authentifizierungsfluss zu erzwingen, wenn der Benutzer bereits authentifiziert ist. Diese Funktion ist erforderlich, um bei jedem Start der App eine Aktualisierung der Benutzermetadaten zu erzwingen (der Benutzermetadatenschlüssel **used_assets** enthält die Anzahl der verfügbaren Ressourcen). Da sich der Benutzer auf mehreren Geräten anmelden kann, sind die Benutzermetadaten, die beim Start der App auf dem Gerät vorhanden sind, unzuverlässig. Wir müssen sie aktualisieren, um den aktuellen Status für diesen bestimmten Benutzer (identifiziert durch eine E-Mail-Adresse) widerzuspiegeln.


>[!IMPORTANT]
>Authentifizierung erzwingen ist nur auf iOS und Android möglich.
>Die Adobe Pass-Authentifizierung verfügt über keinen integrierten Mechanismus, um das kostenlose Streaming zu stoppen, nachdem die X Minuten vergangen sind. Die Adobe Pass-Authentifizierung stellt keine **Autorisierungs** und **Short Media**-Token mehr aus, sobald die Benutzerin oder der Benutzer die Y freien Ressourcen verbraucht. Es liegt an den Programmierern, den Zugriff einzuschränken, sobald der Werbe-Temp-Pass abläuft.

## Sicherheit {#security}

>[!IMPORTANT]
>Adobe speichert keine personenbezogenen Daten (PII). Daher muss der Programmierer einen Hash über die eindeutigen Benutzerinformationen setzen, die in den Adobe Pass-Authentifizierungs-APIs bereitgestellt werden.

**Hashing von Benutzerkennung-Informationen**

Adobe empfiehlt die Verwendung der **SHA-2**-Familie oder ihrer spezifischen **SHA-**, **SHA-512**-Funktionen für Daten, bevor diese an Adobe gesendet werden.

Zum Beispiel ist **SHA-256** über **&quot;user@domain.com&quot;** **„f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7“**.

## Temporärer Pass zum Zurücksetzen oder Bereinigen der Promotion {#reset-promo-tempass}

Bestimmte Geschäftsregeln erfordern eine regelmäßige Bereinigung des temporären Durchgangs für eine Promotion. Zu diesem Zweck stellt die Adobe Pass-Authentifizierung Programmierern eine (öffentliche *Web* API bereit, die unten beschrieben wird:

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protokoll: **https**</li><li>Host:<ul><li>Version: **mgmt.auth.adobe.com**</li><li>Prequal: **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Pfad: **/reset-tempass/v2/reset**</li><li>Abfrageparameter: **device_id=all&amp;Requestor_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>Kopfzeilen: API-Schlüssel: **1232293681726481**</li> <li>Antwort:<ul><li>Erfolg: **HTTP 204**</li><li>Fehler: **HTTP 400** für falsche Anfragen, **HTTP 401** wenn kein API-Schlüssel angegeben ist, **HTTP 403** wenn der API-Schlüssel ungültig ist.</li></ul></li></ul> |

Zusätzlich zu den Anforderungen für das Bereinigen des temporären Übergangs verwendet der temporäre Übergangs zu Werbezwecken den Hash-over-Benutzerkennungsinformationen, die als &quot;**_data“ bei der Authentifizierung und** zur Bereinigung gesendet werden.

Der Hash wird gesendet, nicht die gesamte JSON:

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Unterstützte Clients {#supported-clients}

| Adobe Pass-Authentifizierungsclients | Temporärer Pass für Werbeaktionen | Werkzeug zum Zurücksetzen | Unterstützt dedizierten Antwort-Code / Client-Fehler |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| JS Access Enabler | JA | JA | JA (ab Version 3.0.0) |
| Native Client-iOS | JA | JA | JA (ab Version 1.10) |
| Native Client-Android | JA | JA | JA |
| Clientless-API | JA | JA | NEIN |


## Einschränkungen {#limitations}

In diesem Abschnitt werden die Einschränkungen beschrieben, die für die aktuelle Implementierung des temporären Durchlaufs für Werbeaktionen gelten.

### clientless {#lim-clientless}

**Intelligente Geräte ohne eindeutige Geräte-ID**

Nicht alle Apps für intelligente Geräte können eine eindeutige Geräte-ID bereitstellen. Ist keine vorhanden, kann die Adobe Pass-Authentifizierung die vom Adobe-Registrierungs-Code-Service generierte UUID als eindeutige Geräte-ID verwenden. Das bedeutet, dass die Authentifizierungs- und Autorisierungs-Token gelöscht werden, wenn sich der Benutzer abmeldet. Sobald der Benutzer versucht, sich erneut zu authentifizieren, kann er sich dieses Mal mit anderen Benutzerinformationen (z. B. E-Mail) erneut autorisieren. Adobe empfiehlt das Hinzufügen eines Benutzeroberflächen-Flusses, der es einem Benutzer nicht ermöglicht, das System zu „täuschen“ und eine Logik hinzuzufügen, um festzustellen, ob es sich um einen neuen Benutzer handelt, der eine Testversion anfordert, oder um eine vorhandene Testversion.

**Temporärer Pass wird zurückgesetzt/bereinigt**

In den spezifischen Fällen von Xbox360 und Xbox One ist das Zurücksetzen des Temporärübergangs für Clientless nicht verfügbar, da diese Plattformen ein zusätzliches Analysieren der Geräte-ID erfordern, das im Tool zum Zurücksetzen des Temporärübergangs nicht möglich ist.

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
