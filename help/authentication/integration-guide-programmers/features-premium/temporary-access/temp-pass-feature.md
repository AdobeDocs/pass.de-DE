---
title: TempPass-Funktion
description: TempPass-Funktion
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# TempPass-Funktion {#temp-pass-feature}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

TempPass ist eine vielseitige Funktion, die es Programmierern ermöglicht, Benutzern ohne gültige MVPD-Kontoanmeldeinformationen temporären Zugriff auf ihre geschützten Inhalte anzubieten. Es dient als wirksames Instrument zur Interaktion mit Zuschauern, sei es über einfache Zugriffsszenarien oder zielgerichtete Werbekampagnen.

TempPass ist eine leistungsstarke Lösung für Programmierer, um:

* **Interagieren Sie Zuschauer** Bieten Sie einen Vorgeschmack auf Premium-Inhalte, um neue AbonnentInnen zu gewinnen.
* **Werbeaktionen fördern** Führen Sie zielgerichtete Kampagnen durch, um die Offenlegung von Inhalten zu erhöhen und die Markentreue aufzubauen.
* **Kontrolle behalten:** Verwalten Sie Zugriffszeiträume, setzen Sie Einschränkungen durch und setzen Sie den Zugriff nach Bedarf zurück, um ihn an Ihre Geschäftsziele anzupassen.

Die TempPass-Funktion wird bereitgestellt, indem eine Pseudo-MVPD (weiter „Temp Pass“ genannt) in die Adobe Pass-Authentifizierungs-Server-Konfiguration als Integration mit dem teilnehmenden Programmierer eingeführt wird. Die TempPass-Funktion ist in zwei Konfigurationen verfügbar:

* [Basic TempPass](#basic-temp-pass) für zeitbasierten Zugriff.
* [Werbe-TempPass](#promotional-temp-pass) für flexiblen kampagnengesteuerten Zugriff.

>[!IMPORTANT]
>
> Die TempPass-Funktion ist eine Premium-Funktion und erfordert eine aktuelle Lizenz von Adobe.

Die folgende Tabelle bietet einen kurzen Vergleich der Funktionen von Basic und Promotional TempPass:

| **Funktion** | **Basic TempPass** | **Werbe-TempPass** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **Zugriff auf Inhalte** | <ul><li>Zeitbasiert</li></ul> | <ul><li>Zeitbasiert</li><li>Auf eine maximale Anzahl von Ressourcen beschränkt</li></ul> |
| **Zugriffssicherheit basierend auf** | <ul><li>Geräte-ID</li></ul> | <ul><li>Geräte-ID</li><li>Hash der angegebenen Benutzerkennung-Informationen (z. B. E-Mail)</li></ul> |
| **Verbesserte Fehler-Codes** | Verfügbar | Verfügbar |
| **TempPass-Zurücksetzungsfunktion** | Verfügbar | Verfügbar |

>[!IMPORTANT]
> 
> Die Adobe Pass-Authentifizierung umfasst keinen integrierten Mechanismus zum automatischen Anhalten des laufenden Streams, sobald die zugewiesene Zeit (X Minuten) verstrichen ist. Es liegt in der Verantwortung der Programmierer, Zugriffsbeschränkungen durchzusetzen, sobald der TempPass während eines laufenden Streams abläuft.

Unabhängig davon, ob Sie eine Vorschau Ihrer Inhaltsbibliothek anzeigen oder ein Marquee-Ereignis bewerben, stellt TempPass Ihnen die Tools zur Verfügung, mit denen Sie Ihre Zielgruppe erweitern und gleichzeitig die Zugriffskontrolle behalten.

## Einfache TempPass {#basic-temp-pass}

Die grundlegende TempPass-Funktion ermöglicht es Programmierern, zeitlich begrenzten Zugriff auf Inhalte bereitzustellen, wobei verschiedene Szenarien berücksichtigt werden:

* **Kurze Vorschauen:** Bieten Sie kurze Vorschauen an, z. B. einen täglichen 10-minütigen Zugriffszeitraum, um potenzielle Abonnenten zu gewinnen.
* **Ereignisbasierter Zugriff:** Aktivieren Sie den längeren Zugriff für große Ereignisse, z. B. eine 4-stündige Sitzung.
* **Kombinationszugriff** Dauer kombinieren und abgleichen, z. B. einen anfangs verlängerten Anzeigezeitraum und dann kürzere tägliche Vorschauen über mehrere Tage.

Bestimmte Ereignisse erfordern möglicherweise einen stufenweisen freien Zugang zu Inhalten, z. B. eine zunächst verlängerte kostenlose Zugangsdauer (z. B. 4 Stunden), gefolgt von kürzeren täglichen kostenlosen Zugangsintervallen (z. B. 10 Minuten pro Tag). Um dieses Szenario zu implementieren, müssen sich Programmierer mit ihrem Adobe-Repräsentanten abstimmen, um zwei TempPass-MVPDs zu konfigurieren, die auf ihre Bedürfnisse zugeschnitten sind.

Um beispielsweise eine erste kostenlose 4-stündige Sitzung und anschließend täglich 10-minütige kostenlose Sitzungen anzubieten, kann Adobe für den Programmierer Folgendes konfigurieren:

* **TempPass1**: Wird mit einer TTL (Time-to-Live) von 4 Stunden konfiguriert, um den anfänglichen Zeitraum für den freien Zugriff abzudecken.
* **TempPass2**: Konfiguriert mit einer Time-to-Live (TTL) von 10 Minuten für die nachfolgenden täglichen freien Zugriffsintervalle.

Um eine ordnungsgemäße Funktionalität für den täglichen Zugriff sicherzustellen, muss TempPass2 für alle Geräte jeden Tag um 00:00 Uhr zurückgesetzt werden.

### Funktionsdetails {#basic-temp-pass-feature-details}

**Konfigurationsparameter:**

* **TTL (Time-To-Live):** können die Dauer des Zugriffs festlegen. Diese zeitbasierte TTL läuft unabhängig von der tatsächlichen Anzeigezeit ab.

**Benutzeridentifizierung:**

Die einfache TempPass-Funktion verwendet die Gerätekennung als Benutzeridentifizierungsparameter.

Die folgende Tabelle zeigt, wie Benutzeridentifizierungsparameter das Testerlebnis für Benutzende beeinflussen:

| Gerätekennung | Ergebnis |
|-------------------|----------------|
| Neu | Neue Testversion |
| Vorhandenes | Vorhandene Testversion |

**Zeitberechnung anzeigen:**

Die TTL stellt die Dauer von der ersten Autorisierungsanforderungszeit bis zur Ablaufzeit dar, unabhängig von der tatsächlichen Zeit, die mit der Anzeige von Inhalten verbracht wurde. Bei jeder zukünftigen Anfrage wird die aktuelle Serverzeit mit der gespeicherten Ablaufzeit verglichen, um den Zugriff zu autorisieren.

**Authentifizierung:**

Für Basic TempPass ist keine Authentifizierung erforderlich, sodass Sie direkt mit dem Autorisierungsschritt fortfahren können.

**Autorisierung:**

Da keine Interaktion mit einem tatsächlichen MVPD stattfindet, autorisiert der einfache „Temp Pass“-MVPD jede Ressource, sofern der TempPass gültig ist. Im Falle einer erfolgreichen Autorisierung bleibt die Media Token Verifier-Bibliothek anwendbar, um das Medien-Token zu überprüfen und eine Ressourcenvalidierung sicherzustellen, bevor die Inhaltswiedergabe gestartet wird.

Die Autorisierungsentscheidung basiert auf den Benutzeridentifizierungsparametern und der konfigurierten TTL. Um eine erfolgreiche Autorisierung für eine Ressource zu erhalten, müssen die folgenden Bedingungen von einer gültigen Anfrage erfüllt werden:

* **Nicht genutzte Dauer:** Die Ablaufzeit wird berechnet, indem die (in unseren Datenbanken gespeicherte) anfängliche Autorisierungsanforderungszeit zur konfigurierten TTL hinzugefügt wird. Die aktuelle Serverzeit wird mit dieser Ablaufzeit verglichen, um zu ermitteln, ob der TempPass noch gültig ist.

Wenn ein Benutzer die konfigurierte TTL überschreitet, kann er Inhalte nicht mehr auf demselben Gerät anzeigen, es sei denn, sein TempPass wird zurückgesetzt.

**Vorautorisierung:**

Wenn eine Vorautorisierungsanfrage für eine einfache „Temp Pass“-MVPD gestellt wird, gibt die Antwort die gesamte Liste der Ressourcen aus der Anfrage als erfolgreich vorautorisiert zurück. Dieses Verhalten spiegelt die Autorisierungslogik wider, da die Autorisierungsbedingungen auf Zeitlimits und nicht auf bestimmten Ressourcen basieren. Solange die Zeitbeschränkung gültig ist, werden die angeforderten Ressourcen autorisiert.

**Abmelden:**

Eine Abmeldung ist für Basic TempPass nicht erforderlich, sodass Sie direkt zum Authentifizierungsschritt wechseln können, indem Sie eine tatsächliche MVPD des Benutzers verwenden.

**Tracking von Daten und Analysen:**

Während des einfachen TempPass-Flusses verwenden Tracking-Daten eine Hash-Version der Geräte-ID, wobei die MVPD-ID auf „Temp Pass“ gesetzt ist. Programmierer sollten in ihren Analytics-Implementierungen zwischen TempPass-Metriken und standardmäßigen MVPD-Metriken unterscheiden.

## Werbe-TempPass {#promotional-temp-pass}

Die Werbe-TempPass-Funktion erweitert die Funktionen des einfachen TempPass, der speziell für die Ausführung von Werbekampagnen entwickelt wurde. Mit dieser Funktion können Programmierer Benutzerinnen und Benutzer ansprechen, indem sie den Zugriff auf eine vordefinierte Anzahl von VOD-Titeln für einen bestimmten Zeitraum nach dem Erfassen einer gültigen Benutzeridentifizierung, z. B. einer E-Mail-Adresse, ermöglichen.

Der Werbe-TempPass enthält alle Funktionen des grundlegenden TempPass, mit zusätzlicher Flexibilität für:

* Maximale Anzahl von VOD-Titeln definieren, auf die während des Aktionszeitraums zugegriffen werden kann.
* Festlegen des Zeitraums, in dem der Zugriff auf die Werbeaktion gültig ist.

Sobald ein Benutzer die vordefinierten Zugriffsbeschränkungen (Anzahl der VOD-Titel oder Dauer) überschreitet, kann er keine Inhalte mehr auf demselben Gerät oder mit derselben Benutzerkennung anzeigen, es sei denn, sein TempPass wird zurückgesetzt.

### Funktionsdetails {#promotional-temp-pass-feature-details}

**Konfigurationsparameter:**

* **Benutzerinformationsschlüssel:** Der Schlüssel, der verwendet wird, um die vom Benutzer bereitgestellte Kennung zu übermitteln, z. B. eine E-Mail-Adresse (d. h., der Schlüssel ist E-Mail).
* **Anzahl der Ressourcen:** Definiert, auf wie viele VOD-Titel eine Benutzerin oder ein Benutzer zugreifen kann.
* **TTL (Time-To-Live):** Die Dauer, während der Benutzende die zulässigen Ressourcen nutzen können.

**Benutzeridentifizierung:**

Die Funktion „Promotion TempPass“ verwendet den Hash der von Benutzenden bereitgestellten Kennung zusätzlich zur Gerätekennung als Benutzeridentifizierungsparameter.

>[!IMPORTANT]
>
> Die Validierung und das Hashing von vom Benutzer bereitgestellten Kennungen werden vom Programmierer verwaltet, nicht vom Adobe. Adobe speichert keine personenbezogenen Daten (PII). Daher ist der Programmierer dafür verantwortlich, bei der Interaktion mit den Adobe Pass-Authentifizierungs-APIs einen Hash der eindeutigen vom Benutzer bereitgestellten Kennung zu generieren und zu senden.

Adobe empfiehlt die Verwendung der **SHA-2**-Familie oder ihrer spezifischen **SHA-**, **SHA-512**-Funktionen für Daten, bevor diese an Adobe gesendet werden. Zum Beispiel ist **SHA-256** über **&quot;user@domain.com&quot;** **„f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7“**.

Die folgende Tabelle zeigt, wie Benutzeridentifizierungsparameter das Testerlebnis für Benutzende beeinflussen:

| Vom Benutzer bereitgestellter Kennungs-Hash | Gerätekennung | Ergebnis |
|-------------------------------|-------------------|---------------------------------------------------------|
| Neu | Neu | Neue Testversion |
| Vorhandenes | Neu | Bestehende Testversion (basierend auf dem vom Benutzer angegebenen Kennungs-Hash) |
| Neu | Vorhandenes | Vorhandene Testversion (basierend auf der Gerätekennung) |
| Vorhandenes | Vorhandenes | Vorhandene Testversion |

**Zeitberechnung anzeigen:**

Die TTL stellt die Dauer von der ersten Autorisierungsanforderungszeit bis zur Ablaufzeit dar, unabhängig von der tatsächlichen Zeit, die mit der Anzeige von Inhalten verbracht wurde. Bei jeder zukünftigen Anfrage wird die aktuelle Serverzeit mit der gespeicherten Ablaufzeit verglichen, um den Zugriff zu autorisieren.

**Authentifizierung:**

Für den Werbe-TempPass ist keine Authentifizierung erforderlich, sodass Sie direkt mit dem Autorisierungsschritt fortfahren können.

Um die Implementierung der Programmieranwendung zu unterstützen, stellt der Promotion-TempPass die folgenden Benutzermetadaten-Informationen bereit, auf die über entsprechende Schlüssel zugegriffen werden kann:

* **`remaining_resources`**: Gibt die Anzahl der Ressourcen an, die der Benutzer noch nutzen darf.
* **`used_assets`**: Stellt eine Liste der Ressourcen bereit, die der Benutzer bereits verbraucht hat.
* **`expiration_date`**: Zeigt das Ablaufdatum der vorübergehenden Prüfung für Werbeangebote durch den Benutzer an.

**Autorisierung:**

Da keine Interaktion mit einer tatsächlichen MVPD stattfindet, autorisiert die Promotion-MVPD „Temp Pass“ jede Ressource, sofern der TempPass gültig ist. Im Falle einer erfolgreichen Autorisierung bleibt die Media Token Verifier-Bibliothek anwendbar, um das Medien-Token zu überprüfen und eine Ressourcenvalidierung sicherzustellen, bevor die Inhaltswiedergabe gestartet wird.

Die Autorisierungsentscheidung basiert auf Benutzeridentifizierungsparametern sowie der konfigurierten Anzahl von Ressourcen und TTL. Um eine erfolgreiche Autorisierung für eine Ressource zu erhalten, müssen die folgenden Bedingungen von einer gültigen Anfrage erfüllt werden:

* **Nicht genutzte Dauer:** Die Ablaufzeit wird berechnet, indem die (in unseren Datenbanken gespeicherte) anfängliche Autorisierungsanforderungszeit zur konfigurierten TTL hinzugefügt wird. Die aktuelle Serverzeit wird mit dieser Ablaufzeit verglichen, um zu ermitteln, ob der TempPass noch gültig ist.
* **Nicht genutzte Ressourcen:** Die Anzahl der verbrauchten Ressourcen wird verfolgt (in unseren Datenbanken gespeichert). Die Anzahl der verbrauchten Ressourcen wird mit der konfigurierten Anzahl der Ressourcen verglichen, um zu ermitteln, ob der TempPass noch gültig ist.

Wenn ein Benutzer die konfigurierte TTL oder die Anzahl der Ressourcen überschreitet, kann er Inhalte nicht mehr auf demselben Gerät oder mit derselben vom Benutzer bereitgestellten Kennung anzeigen, es sei denn, sein TempPass wird zurückgesetzt.

**Vorautorisierung:**

Wenn eine Vorautorisierungsanfrage für eine MVPD mit „temporärem Pass“ für Werbeaktionen gestellt wird, gibt die Antwort die gesamte Liste der Ressourcen aus der Anfrage als erfolgreich vorautorisiert zurück. Dieses Verhalten spiegelt die Autorisierungslogik wider, da die Autorisierungsbedingungen auf Zeitlimits und der Gesamtzahl der aufgerufenen Ressourcen und nicht auf bestimmten Ressourcen basieren. Solange die Zeitbeschränkung gültig ist und das Ressourcenlimit nicht überschritten wurde, werden die angeforderten Ressourcen autorisiert.

**Abmelden:**

Eine Abmeldung ist für den Werbe-TempPass nicht erforderlich, sodass Sie direkt zum Authentifizierungsschritt wechseln können, indem Sie eine tatsächliche MVPD-Benutzerin bzw. einen tatsächlichen Benutzer verwenden.

**Tracking von Daten und Analysen:**

Während des temporären Flusses zu Werbezwecken verwenden Tracking-Daten eine Hash-Version der Geräte-ID, wobei die MVPD-ID auf „Temp Pass“ (Temporärer Pass) gesetzt ist. Programmierer sollten in ihren Analytics-Implementierungen zwischen TempPass-Metriken und standardmäßigen MVPD-Metriken unterscheiden.

## TempPass-API-Zugriff zurücksetzen {#reset-tempass-api-access}

Bevor Sie auf die API zum Zurücksetzen von TempPass zugreifen können, müssen Sie die erforderlichen Schritte im Prozess zur dynamischen Client-Registrierung (DCR) ausführen. Dieser obligatorische Prozess stellt sicher, dass Sie über das erforderliche Zugriffstoken für die Interaktion mit der Reset TempPass-API verfügen.

Umfassende Anweisungen finden Sie in der Dokumentation [Übersicht über die Dynamic Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## TempPass-API zurücksetzen - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

Um einen bestimmten TempPass für ein Gerät oder alle Geräte zurückzusetzen, stellt die Adobe Pass-Authentifizierung Programmierern eine API bereit, die sowohl für Standard- als auch für Werbe-TempPass funktioniert.

### Anfrage {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Abfrageparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Requestor_id</td>
      <td>Die interne eindeutige Kennung, die dem Dienstleister während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>Die interne eindeutige Kennung, die dem TempPass während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            Die Gerätekennung, für die dieser Rücksetzvorgang gültig ist.
            <br/><br/>
            Wenn kein Wert angegeben ist, gilt der Rücksetzvorgang für alle Geräte.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Generierung der Bearer-Token-Payload wird in der Dokumentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Zugriffs-Token abrufen</a> beschrieben.</td>
      <td><i>required</i></td>
   </tr>
</table>

### Antwort {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Kein Inhalt</td>
      <td>
        Das Zurücksetzen war erfolgreich.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Fehlerhafte Anfrage</td>
      <td>
        Die Anfrage ist ungültig. Der Client muss die Anfrage korrigieren und es erneut versuchen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Nicht autorisiert</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Verboten</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss neue Client-Anmeldeinformationen und ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
</table>

### Beispiele {#reset-tempass-v3-reset-samples}

#### TempPass für ein bestimmtes Gerät zurücksetzen {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### TempPass für alle Geräte zurücksetzen {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## TempPass-API zurücksetzen - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

Um einen bestimmten TempPass für einen generischen Schlüssel (vom Benutzer bereitgestellter Kennungs-Hash) oder alle Schlüssel zurückzusetzen, stellt die Adobe Pass-Authentifizierung Programmierern eine API bereit, die für den Werbe-TempPass funktioniert.

### Anfrage {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Abfrageparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Requestor_id</td>
      <td>Die interne eindeutige Kennung, die dem Dienstleister während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>Die interne eindeutige Kennung, die dem TempPass während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Schlüssel</td>
      <td>
            Der vom Benutzer bereitgestellte Kennungs-Hash, für den dieser Zurücksetzungsvorgang gültig ist.
            <br/><br/>
            Wenn kein Wert angegeben ist, gilt der Zurücksetzungsvorgang für alle Benutzer.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Generierung der Bearer-Token-Payload wird in der Dokumentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Zugriffs-Token abrufen</a> beschrieben.</td>
      <td><i>required</i></td>
   </tr>
</table>

### Antwort {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Kein Inhalt</td>
      <td>
        Das Zurücksetzen war erfolgreich.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Fehlerhafte Anfrage</td>
      <td>
        Die Anfrage ist ungültig. Der Client muss die Anfrage korrigieren und es erneut versuchen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Nicht autorisiert</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Verboten</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss neue Client-Anmeldeinformationen und ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
</table>

### Beispiele {#reset-tempass-v3-reset-generic-samples}

#### TempPass für einen bestimmten Schlüssel zurücksetzen {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### TempPass für alle Schlüssel zurücksetzen {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST-API V2 {#rest-api-v2}

Für die Nutzung der TempPass-Funktion ist die Implementierung von Code-Aktualisierungen erforderlich, um die Interaktion der TV Everywhere-Anwendung (TVE) mit der Adobe Pass-Authentifizierung ([-API V2) &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) ändern.

Eine umfassende Anleitung zu diesen Aktualisierungen und den zugehörigen Workflows finden Sie in der Dokumentation [Temporäre &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)&quot;.
