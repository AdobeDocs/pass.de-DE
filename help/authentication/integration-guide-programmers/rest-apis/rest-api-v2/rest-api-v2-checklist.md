---
title: REST API v2-Checkliste
description: REST API v2-Checkliste
exl-id: 9095d1dd-a90c-4431-9c58-9a900bfba1cf
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2545'
ht-degree: 0%

---

# REST API v2-Checkliste {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

In diesem Dokument werden die obligatorischen Anforderungen und empfohlenen Vorgehensweisen für Programmierer, die Client-Anwendungen implementieren, die die Adobe Pass-Authentifizierung (REST [ V2) verwenden, an einer Stelle ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Das folgende Dokument muss bei der Implementierung der REST-API V2 als Teil Ihrer Akzeptanzkriterien betrachtet und als Checkliste verwendet werden, um sicherzustellen, dass alle erforderlichen Schritte unternommen wurden, um eine erfolgreiche Integration zu erreichen.

## Obligatorische Anforderungen {#mandatory-requirements}

### 1. Phase der Registrierung {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Registrierter Anwendungsumfang</i></td>
      <td>Verwenden Sie eine registrierte Anwendung mit dem Umfang der REST-API v2.</td>
      <td>Risiken, die HTTP 401-Fehlerantworten „nicht autorisiert“ auslösen, Systemressourcen überlasten und die Latenz erhöhen.</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>Zwischenspeichern von Client-Anmeldeinformationen</i></td>
      <td>Speichern Sie die Client-Anmeldeinformationen in einem persistenten Speicher und verwenden Sie sie für jede Zugriffstoken-Anfrage erneut.</td>
      <td>Riskiert einen Authentifizierungsverlust, wenn die Client-Anmeldeinformationen neu generiert werden.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Zugriffstoken-Caching</i></td>
      <td>Speichern Sie die Zugriffs-Token in einem persistenten Speicher und verwenden Sie sie bis zu ihrem Ablauf erneut.<br/><br/>Fordern Sie nicht für jeden REST API v2-Aufruf ein neues Token an. Aktualisieren Sie die Zugriffstoken erst, wenn sie ablaufen.</td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.</td>
   </tr>
</table>

### 2. Konfigurationsphase {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Konfigurationsabruf</i></td>
      <td>Rufen Sie die Konfigurationsantwort nur ab, wenn Sie vor der Authentifizierungsphase den Benutzer auffordern müssen, seinen MVPD (TV-Anbieter) auszuwählen.<br/><br/>Es ist nicht erforderlich, die Konfigurationsantwort abzurufen, wenn:<ul><li>Der Benutzer ist bereits authentifiziert.</li><li>Dem Benutzer wird temporärer Zugriff angeboten.</li><li>Die Benutzerauthentifizierung ist abgelaufen, aber der Benutzer kann aufgefordert werden, zu bestätigen, dass er weiterhin Abonnent der zuvor ausgewählten MVPD ist.</li></ul></td>
      <td>Risiken, die Systemressourcen zu überlasten und die Latenz zu erhöhen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Zwischenspeicherung der TV-Anbieterauswahl</i></td>
      <td>Speichern Sie die Auswahl des Pay-TV-Anbieters (MVPD) des Benutzers in einem beständigen Speicher, um sie in allen nachfolgenden Phasen zu verwenden:<ul><li>Der Benutzer hat im Konfigurationsantwortspeicher „ID“ für MVPD ausgewählt.</li><li>Der Benutzer hat im Konfigurationsantwortspeicher „displayName“ für MVPD ausgewählt.</li><li>Der Benutzer hat im Konfigurationsantwortspeicher „logoUrl“ für MVPD ausgewählt.</li></ul></td>
      <td>Risiken, die Systemressourcen zu überlasten und die Latenz zu erhöhen.</td>
   </tr>
</table>

### 3. Authentifizierungsphase {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Abrufmechanismus wird gestartet</i></td>
      <td>Starten Sie den Abrufmechanismus unter den folgenden Bedingungen: <br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Authentifizierung wird innerhalb der primären (Bildschirm-)Anwendung durchgeführt</a></b><ul><li>Die primäre (Streaming-)Anwendung sollte den Abfragevorgang starten, wenn die Benutzerin oder der Benutzer die endgültige Zielseite erreicht, nachdem die Browser-Komponente die in der Sitzungs-Endpunktanforderung für den Parameter „redirectUrl“ angegebene URL geladen hat.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Authentifizierung wird in einer sekundären (Bildschirm-)Anwendung durchgeführt</a></b><ul><li>Die primäre (Streaming-)Anwendung sollte den Abfragevorgang starten, sobald der Benutzer den Authentifizierungsprozess einleitet - direkt nach Erhalt der Antwort des Sitzungs-Endpunkts und Anzeige des Authentifizierungs-Codes für den Benutzer.</li></ul></td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Abrufmechanismus wird angehalten</i></td>
      <td>Beenden Sie den Abrufmechanismus unter den folgenden Bedingungen:<br/><br/><b>Erfolgreiche Authentifizierung</b><ul><li>Die Profilinformationen des Benutzers wurden erfolgreich abgerufen, wodurch sein Authentifizierungsstatus bestätigt wird. Daher ist keine Abfrage mehr erforderlich.</li></ul><br/><b>Authentifizierungssitzung und Code-Ablauf</b><ul><li>Wenn die Authentifizierungssitzung und der Authentifizierungscode ablaufen, muss der Benutzer den Authentifizierungsprozess neu starten, und das Abrufen mit dem vorherigen Authentifizierungscode sollte sofort gestoppt werden.</li></ul><br/><b>Neuer Authentifizierungs-Code generiert</b><ul><li>Wenn der Benutzer einen neuen Authentifizierungs-Code anfordert, wird die vorhandene Sitzung ungültig gemacht, und das Abrufen mit dem vorherigen Authentifizierungs-Code sollte sofort gestoppt werden.</li></ul></td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Konfigurieren des Abrufmechanismus</i></td>
      <td>Konfigurieren Sie die Häufigkeit des Abrufmechanismus unter den folgenden Bedingungen: <br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Authentifizierung wird innerhalb der primären (Bildschirm-)Anwendung durchgeführt</a></b><ul><li>Die primäre (Streaming-)Anwendung sollte alle 3-5 Sekunden oder länger abfragen.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Authentifizierung wird in einer sekundären (Bildschirm-)Anwendung durchgeführt</a></b><ul><li>Die primäre (Streaming-)Anwendung sollte alle 3-5 Sekunden eine Abfrage durchführen.</li></ul></td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Profile-Caching</i></td>
      <td>Speichern Sie Teile der Profilinformationen des Benutzers im beständigen Speicher, um die Leistung zu verbessern und unnötige REST-API-v2-Aufrufe zu minimieren.<br/><br/>Die Zwischenspeicherung sollte sich auf die folgenden Antwortfelder der Profile konzentrieren:<br/><br/><b>mvpd</b><ul><li>Die Client-Anwendung kann dies verwenden, um den vom Benutzer ausgewählten TV-Anbieter zu verfolgen und ihn während der Vorautorisierungs- oder Autorisierungsphase weiter zu verwenden.</li><li>Wenn das aktuelle Benutzerprofil abläuft, kann die Client-Anwendung die gespeicherte MVPD-Auswahl verwenden und den Benutzer einfach zur Bestätigung auffordern.</li></ul><br/><b>Attribute</b><ul><li>Wird verwendet, um das Benutzererlebnis basierend auf verschiedenen Benutzermetadatenschlüsseln (z. B. ZIP, MaxRating usw.) zu personalisieren.</li><li>Benutzermetadaten werden nach Abschluss des Authentifizierungsflusses verfügbar, daher muss die Client-Anwendung keinen separaten Endpunkt abfragen, um die Benutzermetadaten-Informationen abzurufen, da sie bereits in den Profilinformationen enthalten sind.</li><li>Bestimmte Metadatenattribute können während der Autorisierungsphase aktualisiert werden, je nach MVPD (z. B. Charta) und dem spezifischen Metadatenattribut (z. B. householdID). Daher muss die Client-Anwendung möglicherweise die Profil-APIs nach der Autorisierung erneut abfragen, um die neuesten Benutzermetadaten abzurufen.</li></ul></td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.</td>
   </tr>
</table>

### 4. (Optional) Phase vor der Autorisierung {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Abrufen von Entscheidungen vor der Autorisierung</i></td>
      <td>Verwenden Sie Entscheidungen zur Vorabautorisierung für die Inhaltsfilterung und niemals für Wiedergabeentscheidungen.</td>
      <td>Gefahr der Verletzung vertraglicher Vereinbarungen zwischen Programmer, MVPDs und Adobe.<br/><br/>Risiken, unsere Überwachungs- und Warnsysteme zu umgehen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Wiederholung von Entscheidungen vor Autorisierung</i></td>
      <td>Verarbeiten Sie die <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">erweiterten Fehler</a>Codes entsprechend und verwenden Sie das Aktionsfeld, um die erforderlichen Korrekturschritte zu bestimmen.<br/><br/>Nur eine begrenzte Anzahl erweiterter Fehler-Codes rechtfertigt einen erneuten Versuch, während für die meisten alternative Auflösungen wie im Aktionsfeld angegeben erforderlich sind.<br/><br/>Stellen Sie sicher, dass ein Wiederholungsmechanismus, der zum Abrufen von Entscheidungen vor der Autorisierung implementiert wird, nicht zu einer Endlosschleife führt und dass Wiederholungsversuche auf eine angemessene Anzahl begrenzt werden (d. h. 2-3).</td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Caching von Entscheidungen vor Autorisierung</i></td>
      <td>Erfolgreiche Cache-Zulassungsentscheidungen im Speicher, um die Leistung zu verbessern und unnötige REST-API-v2-Aufrufe zu minimieren, da Abonnementaktualisierungen während der Ausführung der Anwendung selten sind.</td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.</td>
   </tr>
</table>

### 5. Genehmigungsphase {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Abrufen von Autorisierungsentscheidungen</i></td>
      <td>Erhalten Sie Autorisierungsentscheidungen vor der Wiedergabe - unabhängig davon, ob eine Entscheidung vor der Autorisierung existiert.<br/><br/>Erlauben Sie Streams die unterbrechungsfreie Fortsetzung, selbst wenn das Medien-Token während der Wiedergabe abläuft, und fordern Sie eine neue Autorisierungsentscheidung an, die ein (neues) Medien-Token enthält, wenn der Benutzer seine nächste Wiedergabeanforderung durchführt, unabhängig davon, ob es sich um dieselbe oder eine andere Ressource handelt.<br/><br/>Live-Streams, die über längere Zeiträume laufen, können nach Videovorgängen eine neue Autorisierungsentscheidung anfordern, z. B. nach dem Anhalten von Inhalten, dem Initiieren von Werbeunterbrechungen oder dem Ändern der Konfigurationen auf Asset-Ebene, wenn der MRSS geändert wird.</td>
      <td>Gefahr der Verletzung vertraglicher Vereinbarungen zwischen Programmer, MVPDs und Adobe.<br/><br/>Risiken, unsere Überwachungs- und Warnsysteme zu umgehen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Wiederholung von Autorisierungsentscheidungen</i></td>
      <td>Verarbeiten Sie die <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">erweiterten Fehler</a>Codes entsprechend und verwenden Sie das Aktionsfeld, um die erforderlichen Korrekturschritte zu bestimmen.<br/><br/>Nur eine begrenzte Anzahl erweiterter Fehler-Codes rechtfertigt einen erneuten Versuch, während für die meisten alternative Auflösungen wie im Aktionsfeld angegeben erforderlich sind.<br/><br/>Stellen Sie sicher, dass ein zum Abrufen von Autorisierungsentscheidungen implementierter Wiederholungsmechanismus nicht zu einer Endlosschleife führt und dass Wiederholungsversuche auf eine angemessene Anzahl (d. h. 2-3) begrenzt werden.</td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.</td>
   </tr>
</table>

### 6. Abmeldephase {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Abmeldeunterstützung</i></td>
      <td>Implementieren Sie die Abmelde-API, damit sich Benutzer manuell abmelden können, wodurch ihr authentifiziertes Profil beendet wird, und folgen Sie dem für jedes entfernte Profil angegebenen REST API v2-Aktionsnamen:<ul><li>Bei MVPDs, die einen Abmeldeendpunkt unterstützen, muss die Client-Anwendung in einem Benutzeragenten zur angegebenen „URL“ navigieren.</li><li>Bei Profilen vom Typ „appleSSO“ muss die Client-Anwendung den Benutzer anweisen, sich auch auf Partnerebene abzumelden (Systemeinstellungen von Apple).</li></ul></td>
      <td>Risiken durch Fehlfunktion der Client-Anwendung aufgrund fehlender Unterstützung auf der Client-Anwendungsseite.</td>
   </tr>
</table>

### 7. Parameter und Kopfzeilen {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorisierungskopfzeile senden</i></td>
      <td>Senden Sie für <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md"> REST API v2</a>Anfrage die Kopfzeile „Authorization“.</td>
      <td>Risiken, die HTTP 401-Fehlerantworten „nicht autorisiert“ auslösen, Systemressourcen überlasten und die Latenz erhöhen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Kopfzeile „AP-DEVICE-IDENTIFIER“ senden</i></td>
      <td>Senden Sie für <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md"> REST API v2-Anfrage den Header </a>AP-Device-Identifier“.<br/><br/>Selbst wenn die Anfrage von einem Server im Namen eines Geräts stammt, muss der Kopfzeilenwert AP-Device-Identifier die tatsächliche Kennung des Streaming-Geräts widerspiegeln.</td>
      <td>Risiken, die HTTP 400-Fehlerantworten bei „Bad Request“ auslösen, Systemressourcen überlasten und die Latenz erhöhen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>X-DEVICE-INFO-Header senden</i></td>
      <td>Senden Sie die Kopfzeile <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> für jede REST API v2-Anfrage.<br/><br/>Selbst wenn die Anfrage von einem Server im Namen eines Geräts stammt, muss der Header-Wert für X-Device-Info die tatsächlichen Streaming-Geräteinformationen widerspiegeln.</td>
      <td>Risiken, die als von einer unbekannten Plattform stammend klassifiziert und als unsicher behandelt werden und restriktiveren Regeln unterliegen, wie beispielsweise kürzere Authentifizierungs-TTLs.<br/><br/>Darüber hinaus sind einige Felder, wie die connectionIp des Streaming-Geräts und connectionPort, für Funktionen wie die Basisauthentifizierung von Spectrum obligatorisch.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Kennung eines stabilen Geräts</i></td>
      <td>Berechnen und speichern Sie eine stabile Gerätekennung, die sich nicht über Aktualisierungen oder Neustarts hinweg für die Kopfzeile <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> ändert.<br/><br/>Bei Plattformen ohne Hardware-Kennung aus Anwendungsattributen eine eindeutige Kennung generieren und beibehalten.</td>
      <td>Riskiert einen Authentifizierungsverlust, wenn die Gerätekennung geändert wird.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>API-Referenzen folgen</i></td>
      <td>Vergewissern Sie sich, dass Sie nur die erwarteten Parameter und Header der REST-API v2 senden.</td>
      <td>Risiken, die HTTP 400-Fehlerantworten bei „Bad Request“ auslösen, Systemressourcen überlasten und die Latenz erhöhen.</td>
   </tr>
</table>

### 8. Umgang mit Fehlern {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Verbesserte Unterstützung für die Fehlercode-Behandlung</i></td>
      <td>Verarbeiten Sie die <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">erweiterten Fehler</a>Codes entsprechend und verwenden Sie das Aktionsfeld, um die erforderlichen Korrekturschritte zu bestimmen.<br/><br/>Nur eine begrenzte Anzahl erweiterter Fehler-Codes rechtfertigt einen erneuten Versuch, während für die meisten alternative Auflösungen wie im Aktionsfeld angegeben erforderlich sind.<br/><br/>Die meisten erweiterten Fehler-Codes, die in der Dokumentation <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">Erweiterte Fehler-Codes - REST API V2</a> aufgeführt sind, können vollständig verhindert werden, wenn sie während der Entwicklungsphase vor dem Start der Anwendung korrekt verarbeitet werden.</td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.<br/><br/>Es besteht das Risiko, dass die Client-Anwendung aufgrund einer fehlenden Handhabung der erweiterten Fehler-Codes nicht funktioniert, was zu unklaren Fehlermeldungen, falschen Benutzeranleitungen oder falschem Fallback-Verhalten führt.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Unterstützung der HTTP-Fehlerbehandlung</i></td>
      <td>Unterscheidung zwischen der Verarbeitung von HTTP-Fehlerantworten (z. B. 400, 401, 403, 404, 405, 500) und Erfolgsantworten (z. B. 200, 201), die erweiterte Fehler-Code-Payloads enthalten, wie oben beschrieben.<br/><br/>Nur eine begrenzte Anzahl von HTTP-Fehler-Codes rechtfertigt einen erneuten Versuch, während die meisten alternative Auflösungen erfordern.<br/><br/>Die meisten HTTP-Fehlerantworten können vollständig verhindert werden, wenn sie während der Entwicklungsphase vor dem Start der Anwendung korrekt verarbeitet werden.</td>
      <td>Es besteht das Risiko, dass Systemressourcen überlastet werden, die Latenz zunimmt und möglicherweise Fehlerantworten vom Typ „Zu viele Anfragen“ in HTTP 429 ausgelöst werden.<br/><br/>Es besteht das Risiko, dass die Client-Anwendung aufgrund einer fehlenden Handhabung der erweiterten Fehler-Codes nicht funktioniert, was zu unklaren Fehlermeldungen, falschen Benutzeranleitungen oder falschem Fallback-Verhalten führt.</td>
   </tr>
</table>

### 9. Prüfung {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Anforderungen</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Lebenszyklustests</i></td>
      <td>Entwickeln und testen Sie die Anwendung mit den offiziellen produktionsfremden Umgebungen für die Adobe Pass-Authentifizierung:<ul><li>Vorserienfertigung</li><li>Release-Staging</li></ul><br/>Führen Sie in diesen Umgebungen eine gründliche Qualitätssicherung (QA) durch, bevor Sie mit der Veröffentlichung in der Produktionsumgebung beginnen.<br/><br/>Client-Anwendungen dürfen nicht zur Produktionsfreigabe übergehen, ohne zunächst die End-to-End-Validierung in Nicht-Produktionsumgebungen abzuschließen.</td>
      <td>Risiken, die mit kritischen und schwerwiegenden Mängeln beginnen.<br/><br/>Ein kurzer und effizienter Debugging-Weg kann den Adobe Support und das Engineering daran hindern, schnell einzugreifen.</td>
   </tr>
</table>

## Empfohlene Verfahren {#recommended-practices}

### 1. Phase der Registrierung {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Practices</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validierung der Zugriffstoken</i></td>
      <td>Proaktive Überprüfung der Gültigkeit des Zugriffs-Tokens, um es nach Ablauf zu aktualisieren.<br/><br/>Stellen Sie sicher, dass jeder Wiederholungsmechanismus für die Verarbeitung von „nicht autorisierten“ HTTP 401-Fehlern zunächst das Zugriffstoken aktualisiert, bevor Sie die ursprüngliche Anfrage erneut versuchen.</td>
      <td>Risiken, die HTTP 401-Fehlerantworten „nicht autorisiert“ auslösen, Systemressourcen überlasten und die Latenz erhöhen.</td>
   </tr>
</table>

### 2. Konfigurationsphase {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Practices</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Konfigurations-Caching</i></td>
      <td>Speichern Sie die Konfigurationsantwort für einen kurzen Zeitraum (z. B. 3-5 Minuten) im Speicher oder im persistenten Speicher, um die Leistung zu verbessern und unnötige REST-API-v2-Aufrufe zu minimieren.</td>
      <td>Risiken, die Systemressourcen zu überlasten und die Latenz zu erhöhen.</td>
   </tr>
</table>

### 3. Authentifizierungsphase {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Practices</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validierung des Authentifizierungs-Codes (Authentifizierung auf dem 2. Bildschirm)</i></td>
      <td>Validieren Sie den Authentifizierungscode, der über die Benutzereingabe auf dem Bildschirm der zweiten Anwendung gesendet wurde, bevor Sie die API /api/v2/Authenticate aufrufen, unter den folgenden Bedingungen:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">Authentifizierung wird innerhalb der sekundären Anwendung (Bildschirm) mit vorab ausgewähltem mvpd durchgeführt</a></b><ul><li>Nutzen Sie <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">Authentifizierungssitzung fortsetzen</a> POST /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">Authentifizierung wird innerhalb der sekundären (Bildschirm-)Anwendung ohne vorab ausgewählte mvpd durchgeführt</a></b><ul><li>Verwenden Sie <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">Authentifizierungssitzung abrufen</a> - GET /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/>Die Client-Anwendung erhält einen Fehler, wenn der angegebene Authentifizierungs-Code falsch eingegeben wurde oder die Authentifizierungssitzung abgelaufen ist.</td>
      <td>Dadurch werden verschiedene Fehlerreaktionen und Workflow-Probleme bei der Authentifizierung riskiert.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Unterstützung mehrerer Profile</i></td>
      <td>Stellen Sie sicher, dass die Client-Anwendung mehrere Profile verarbeiten kann, indem Sie den Benutzer entweder zur Auswahl eines Profils auffordern oder eine benutzerdefinierte Logik anwenden, z. B. das Profil mit der längsten Gültigkeitsdauer automatisch auswählen.</td>
      <td>Risiken durch Fehlfunktion der Client-Anwendung aufgrund fehlender Unterstützung auf der Client-Anwendungsseite.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>(Optional) Unterstützung für Nicht-Basis-Flüsse</i></td>
      <td>Unterstützung nicht grundlegender Flüsse, falls das Client-Anwendungsgeschäft Folgendes erfordert:<ul><li>Heruntergestufte Zugriffsflüsse (Premium-Funktion)</li><li>Temporäre Zugriffsflüsse (Premium-Funktion)</li><li>Single-Sign-On-Zugriffsflüsse (Standardfunktion)</li></ul></td>
      <td>Es besteht das Risiko, dass das Benutzererlebnis nicht optimal ist.</td>
   </tr>
</table>

### 4. (Optional) Phase vor der Autorisierung {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Practices</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Benutzererlebnis</i></td>
      <td>Zeigen Sie klares Benutzer-Feedback an, wenn eine Vorautorisierungsentscheidung verweigert wird, indem Sie Nachrichten verwenden, die von MVPDs oder Adobe über erweiterte Fehlercodes bereitgestellt werden.</td>
      <td>Es besteht das Risiko, dass das Benutzererlebnis nicht optimal ist.</td>
   </tr>
</table>

### 5. Genehmigungsphase {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Practices</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validierung von Medien-Token</i></td>
      <td>Validieren von Medien-Token mithilfe der Bibliothek <a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">Media Token Verifier</a>.</td>
      <td>Risiko von Betrügereien wie Stream-Ripping.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Benutzererlebnis</i></td>
      <td>Zeigen Sie klares Benutzer-Feedback an, wenn eine Autorisierungsentscheidung verweigert wird, indem Sie Nachrichten verwenden, die von MVPDs oder Adobe über erweiterte Fehlercodes bereitgestellt werden.</td>
      <td>Es besteht das Risiko, dass das Benutzererlebnis nicht optimal ist.</td>
   </tr>
</table>

### 6. Abmeldephase {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Practices</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Benutzererlebnis</i></td>
      <td>Vermeiden Sie den automatischen (programmgesteuerten) Aufruf der Abmelde-API in Szenarien wie verweigerter Vorautorisierung oder Autorisierung, da die Abmelde-API nur als Antwort auf eine direkte Benutzeranfrage aufgerufen werden sollte.</td>
      <td>Es besteht das Risiko, den Benutzer zu verwirren, dass die Authentifizierung fehlschlägt.</td>
   </tr>
</table>

### 7. Parameter und Kopfzeilen {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Practices</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Wiederverwenden von Code</i></td>
      <td>Verwenden Sie den Code aus der REST-API v1 zur Berechnung der Gerätekennung und Geräteinformationen mit geringfügigen Anpassungen, stellen Sie jedoch sicher, dass Sie nur die erwarteten Parameter und Kopfzeilen der REST-API v2 senden.<br/><br/>Verwenden Sie Code aus der REST-API v1 zum Aufrufen der DCR-API, um ein Zugriffstoken abzurufen.</td>
      <td>-</td>
   </tr>
</table>

### 8. Prüfung {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Practices</th>
      <th style="background-color: #EFF2F7;">Risiken</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Testabdeckung</i></td>
      <td>Stellen Sie sicher, dass die folgenden grundlegenden Flüsse geräte- und plattformübergreifend getestet werden<br/><br/><b>Authentifizierungsflüsse</b><ul><li>Authentifizierungsszenario für Primäre Anwendungen (Bildschirm)</li><li>Authentifizierungsszenario für Sekundäre Anwendungen (Bildschirm)</li></ul><br/><b>(Optional) Vorautorisierungsflüsse</b><ul><li>Szenario für Testgenehmigungsentscheidungen</li><li>Szenario „Ablehnungsentscheidungen testen“</li></ul><br/><b>Autorisierungsflüsse</b><ul><li>Szenario für Testgenehmigungsentscheidungen</li><li>Szenario „Ablehnungsentscheidungen testen“</li></ul><br/><b>Abmeldevorgänge</b><br/><br/>Testen Sie außerdem ggf. weitere Zugriffsflüsse:<br/><br/><ul><li>Heruntergestufte Zugriffsflüsse (Premium-Funktion)</li><li>Temporäre Zugriffsflüsse (Premium-Funktion)</li><li>Single-Sign-On-Zugriffsflüsse (Standardfunktion)</li></ul><br/>Abdeckung der wichtigsten MVPD-Integrationen (einschließlich der am häufigsten verwendeten Anbieter).</td>
      <td>Risiken durch unvorhergesehene Fehler in der Produktion, insbesondere bei weniger häufig getesteten Plattformen oder seltenen Flüssen wie negativen Szenarien.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Test-Tools</i></td>
      <td>Verwenden Sie die <a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a>-Website.</td>
      <td>-</td>
   </tr>
</table>

## Zusammenfassung {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Phase</th>
      <th style="background-color: #EFF2F7;">Obligatorisch</th>
      <th style="background-color: #EFF2F7;">(dringend empfohlen)</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Registrierung</i></td>
      <td>Clientanmeldeinformationen zwischenspeichern<br/><br/> Zugriffstoken zwischenspeichern</td>
      <td>Zugriffstoken validieren und aktualisieren</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Konfiguration</i></td>
      <td>Minimieren des Abrufs von Konfigurationsantworten</td>
      <td>Cache-Konfigurationsantwort</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Authentifizierung</i></td>
      <td>Abrufmechanismus: Feinabstimmung<br/><br/> Zwischenspeichern von Profilteilen</td>
      <td>Unterstützung mehrerer Profile<br/><br/>Support-Beeinträchtigungsfunktion (bei Geschäftsanforderungen)<br/><br/>Support-TempPass-Funktion (bei Geschäftsanforderungen)<br/><br/>Support-Single-Sign-On-Funktion (bei Geschäftsanforderungen)</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Vorautorisierung</i></td>
      <td>Feinabstimmung des Mechanismus für Vorabautorisierungsentscheidungen <br/><br/>Wiederholung) zwischenspeichern</td>
      <td>Verbessern des Benutzererlebnisses durch Verwendung von Fehler-Codes für abgelehnte Entscheidungen vor Autorisierung</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorisierung</i></td>
      <td>Abrufen der Autorisierungsentscheidung bei Benutzeranfragen zur Optimierung <br/><br/> Wiedergabe-/Wiederholungsmechanismus</td>
      <td>Verbessern Sie das Benutzererlebnis durch die Verwendung von Fehler-Codes für die Validierung von <br/><br/>-Medien-Token</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Abmelden</i></td>
      <td>Implementieren der Abmelde-API, damit sich Benutzer manuell abmelden können</td>
      <td>Automatisches Aufrufen der Abmelde-API vermeiden</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Obligatorisch</th>
      <th style="background-color: #EFF2F7;">(dringend empfohlen)</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Parameter und Kopfzeilen</i></td>
      <td>Pflichtangaben für Kopfzeilen befolgen</td>
      <td>Wiederverwenden von Code aus der REST-API v1</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Fehlerbehandlung</i></td>
      <td>Implementieren der erweiterten Fehlerbehandlung<br/><br/>Implementieren der HTTP-Fehlerbehandlung</td>
      <td>-</td>
   </tr>
</table>
