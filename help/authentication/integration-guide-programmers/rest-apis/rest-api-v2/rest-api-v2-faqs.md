---
title: Häufig gestellte Fragen zur REST API V2
description: Häufig gestellte Fragen zur REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: ebe0a53e3ba54c2effdef45c1143deea0e6e57d3
workflow-type: tm+mt
source-wordcount: '9566'
ht-degree: 0%

---

# Häufig gestellte Fragen zur REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Dieses Dokument bietet allgemeine Antworten auf häufig gestellte Fragen zur Verwendung der Adobe Pass Authentication REST API V2.

Weitere Informationen zur REST API V2 insgesamt finden Sie in der Dokumentation [REST API V2 - Übersicht](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) .

## Allgemeine FAQs {#general-faqs}

Beginnen Sie mit diesem Abschnitt, wenn Sie an einem Programm arbeiten, das die REST-API V2 integrieren muss, unabhängig davon, ob es sich um ein neues oder ein bestehendes Programm handelt, das von [REST-API V1](#migration-rest-api-v1-to-rest-api-v2) oder [SDK migriert](#migration-sdk-to-rest-api-v2).

Weitere Informationen zu Migrationsdetails und -schritten finden Sie auch in den nächsten Abschnitten.

### Häufig gestellte Fragen zur Registrierungsphase {#registration-phase-faqs-general}

+++Häufig gestellte Fragen zur Registrierungsphase

Weitere Informationen finden Sie in der [Häufig gestellte Fragen zur Dynamic Client-Registrierung (DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Häufig gestellte Fragen zur Konfigurationsphase {#configuration-phase-faqs-general}

+++Häufig gestellte Fragen zur Konfigurationsphase

#### &#x200B;1. Was ist der Zweck der Konfigurationsphase? {#configuration-phase-faq1}

Die Konfigurationsphase dient dazu, der Client-Anwendung die Liste der MVPDs bereitzustellen, mit denen sie aktiv integriert ist, sowie Konfigurationsdetails (z. B. `id`, `displayName`, `logoUrl` usw.), die von der Adobe Pass-Authentifizierung für jede MVPD gespeichert wurden.

Die Konfigurationsphase dient als erforderlicher Schritt für die Authentifizierungsphase, wenn das Client-Programm den Benutzer bzw. die Benutzerin auffordern muss, seinen/ihren TV-Anbieter auszuwählen.

#### &#x200B;2. Ist die Konfigurationsphase obligatorisch? {#configuration-phase-faq2}

Die Konfigurationsphase ist nicht obligatorisch. Die Client-Anwendung muss die Konfiguration nur abrufen, wenn der Benutzer seine MVPD zur Authentifizierung oder erneuten Authentifizierung auswählen muss.

Die Client-Anwendung kann diese Phase in den folgenden Szenarien überspringen:

* Der Benutzer ist bereits authentifiziert.
* Dem Benutzer wird temporärer Zugriff über die Basis- oder Werbe-Funktion [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) angeboten.
* Die Benutzerauthentifizierung ist abgelaufen, aber die Client-Anwendung hat die zuvor ausgewählte MVPD als benutzererlebnismotivierte Auswahl zwischengespeichert und fordert die Benutzenden lediglich auf zu bestätigen, dass sie weiterhin Abonnenten dieser MVPD sind.

#### &#x200B;3. Was ist eine Konfiguration und wie lange ist sie gültig? {#configuration-phase-faq3}

Die Konfiguration ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration) definiert ist.

Die Konfiguration enthält eine Liste von MVPDs, die durch die folgenden Attribute `id`, `displayName`, `logoUrl` usw. definiert sind, die vom Endpunkt [Configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) abgerufen werden können.

Die Client-Anwendung darf die Konfiguration nur abrufen, wenn die Benutzerin bzw. der Benutzer ihre MVPD zur Authentifizierung oder erneuten Authentifizierung auswählen muss.

Die Client-Anwendung kann die Konfigurationsantwort verwenden, um eine UI-Auswahl mit verfügbaren MVPD-Optionen zu präsentieren, wenn der Benutzer seinen TV-Anbieter auswählen muss.

Die Client-Anwendung muss die vom Benutzer ausgewählte MVPD-Kennung speichern, wie sie im `id` auf Konfigurationsebene von MVPD angegeben ist, um mit der Authentifizierungs-, Vorautorisierungs-, Autorisierungs- oder Abmeldephase fortzufahren.

Weitere Informationen finden Sie in der Dokumentation [Konfiguration abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) .

#### &#x200B;4. Sollte die Client-Anwendung die Konfigurationsantwortinformationen in einem persistenten Speicher zwischenspeichern? {#configuration-phase-faq4}

Die Client-Anwendung darf die Konfiguration nur abrufen, wenn die Benutzerin bzw. der Benutzer ihre MVPD zur Authentifizierung oder erneuten Authentifizierung auswählen muss.

Die Client-Anwendung sollte die Konfigurationsantwortinformationen in einem Arbeitsspeicher zwischenspeichern, um unnötige Anfragen zu vermeiden und das Benutzererlebnis zu verbessern, wenn:

* Der Benutzer ist bereits authentifiziert.
* Dem Benutzer wird temporärer Zugriff über die Basis- oder Werbe-Funktion [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) angeboten.
* Die Benutzerauthentifizierung ist abgelaufen, aber die Client-Anwendung hat die zuvor ausgewählte MVPD als benutzererlebnismotivierte Auswahl zwischengespeichert und fordert die Benutzenden lediglich auf zu bestätigen, dass sie weiterhin Abonnenten dieser MVPD sind.

#### &#x200B;5. Kann die Client-Anwendung eine eigene Liste von MVPDs verwalten? {#configuration-phase-faq5}

Die Client-Anwendung kann ihre eigene Liste von MVPDs verwalten, erfordert jedoch, dass die MVPD-IDs mit der Adobe Pass-Authentifizierung synchronisiert bleiben. Es wird daher empfohlen, die von der Adobe Pass-Authentifizierung bereitgestellte Konfiguration zu verwenden, um sicherzustellen, dass die Liste aktuell und korrekt ist.

Die Client-Anwendung erhält einen [Fehler](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) von der Adobe Pass-Authentifizierungs-REST-API V2, wenn die angegebene MVPD-Kennung ungültig ist oder keine aktive Integration mit dem angegebenen [Dienstleister“ ](rest-api-v2-glossary.md#service-provider).

#### &#x200B;6. Kann die Client-Anwendung die Liste der MVPDs filtern? {#configuration-phase-faq6}

Die Client-Anwendung kann die Liste der in der Konfigurationsantwort bereitgestellten MVPDs filtern, indem ein benutzerdefinierter Mechanismus implementiert wird, der auf ihrer eigenen Geschäftslogik und ihren Anforderungen basiert, z. B. dem Benutzerstandort oder dem Benutzerverlauf der vorherigen Auswahl.

Die Client-Anwendung kann die Liste der MVPDs [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) oder der MVPDs filtern, deren Integration noch in Entwicklung oder Test ist.

#### &#x200B;7. Was passiert, wenn die Integration mit einer MVPD deaktiviert und als inaktiv markiert ist? {#configuration-phase-faq7}

Wenn die Integration mit einer MVPD deaktiviert und als inaktiv markiert ist, wird die MVPD aus der Liste der MVPDs entfernt, die in weiteren Konfigurationsantworten bereitgestellt werden, und es sind zwei wichtige Folgen zu berücksichtigen:

* Die nicht authentifizierten Benutzer dieser MVPD können die Authentifizierungsphase mit dieser MVPD nicht mehr abschließen.
* Die authentifizierten Benutzenden dieser MVPD können die Vorautorisierungs-, Autorisierungs- oder Abmeldephasen mit dieser MVPD nicht mehr abschließen.

Die Client-Anwendung erhält einen [Fehler](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) von der Adobe Pass-Authentifizierungs-REST-API V2, wenn der ausgewählte Benutzer in MVPD keine aktive Integration mehr mit dem angegebenen [Dienstleister](rest-api-v2-glossary.md#service-provider) hat.

#### &#x200B;8. Was passiert, wenn die Integration mit MVPD wieder aktiviert und als aktiv markiert wird? {#configuration-phase-faq8}

Wenn die Integration mit einer MVPD wieder aktiviert und als aktiviert markiert ist, wird die MVPD wieder in die Liste der MVPDs aufgenommen, die in weiteren Konfigurationsantworten bereitgestellt werden, und es sind zwei wichtige Konsequenzen zu beachten:

* Die nicht authentifizierten Benutzer dieser MVPD können die Authentifizierungsphase mithilfe dieser MVPD erneut abschließen.
* Die authentifizierten Benutzer dieser MVPD können die Vorautorisierungs-, Autorisierungs- oder Abmeldephasen mit dieser MVPD erneut abschließen.

#### &#x200B;9. Wie kann ich die Integration mit MVPD aktivieren oder deaktivieren? {#configuration-phase-faq9}

Dieser Vorgang kann über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter ausgeführt werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Häufig gestellte Fragen zur Authentifizierungsphase {#authentication-phase-faqs-general}

+++Häufig gestellte Fragen zur Authentifizierungsphase

#### &#x200B;1. Was ist der Zweck der Authentifizierungsphase? {#authentication-phase-faq1}

Der Zweck der Authentifizierungsphase besteht darin, der Client-Anwendung die Möglichkeit zu geben, die Identität des Benutzers zu überprüfen und Benutzermetadaten-Informationen abzurufen.

Die Authentifizierungsphase fungiert als erforderlicher Schritt für die Vorautorisierungsphase oder Autorisierungsphase, wenn die Client-Anwendung Inhalte wiedergeben muss.

#### &#x200B;2. Ist die Authentifizierungsphase obligatorisch? {#authentication-phase-faq2}

Die Authentifizierungsphase ist obligatorisch. Die Client-Anwendung muss den Benutzer authentifizieren, wenn sie innerhalb der Adobe Pass-Authentifizierung kein gültiges Profil hat.

Die Client-Anwendung kann diese Phase in den folgenden Szenarien überspringen:

* Der Benutzer ist bereits authentifiziert und das Profil ist weiterhin gültig.
* Dem Benutzer wird temporärer Zugriff über die Basis- oder Werbe-Funktion [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) angeboten.

Die Fehlerbehandlung in der Client-Anwendung erfordert die Verarbeitung der [Fehler](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)-Codes (z. B. `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated` usw.), die angeben, dass die Client-Anwendung eine Authentifizierung des Benutzers erfordert.

#### &#x200B;3. Was ist eine Authentifizierungssitzung und wie lange ist sie gültig? {#authentication-phase-faq3}

Die Authentifizierungssitzung ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session) definiert ist.

Die Authentifizierungssitzung speichert Informationen zum initiierten Authentifizierungsprozess, die vom Sitzungs-Endpunkt abgerufen werden können.

Die Authentifizierungssitzung ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Ausgabe durch den `notAfter` Zeitstempel angegeben wird. Dies gibt an, wie lange der Benutzer bis zum Abschluss des Authentifizierungsprozesses muss, bevor der Fluss neu gestartet werden muss.

Die Client-Anwendung kann die Antwort der Authentifizierungssitzung verwenden, um zu erfahren, wie mit dem Authentifizierungsprozess fortgefahren werden soll. Beachten Sie, dass es Fälle gibt, in denen der Benutzer sich nicht authentifizieren muss, z. B. bei Bereitstellung eines temporären Zugriffs, eingeschränktem Zugriff oder wenn der Benutzer bereits authentifiziert ist.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Authentifizierungssitzung-API erstellen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Authentifizierungssitzung-API fortsetzen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundlegender Authentifizierungsfluss innerhalb der sekundären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;4. Was ist ein Authentifizierungs-Code und wie lange ist er gültig? {#authentication-phase-faq4}

Der Authentifizierungs-Code ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code) definiert ist.

Der Authentifizierungs-Code speichert einen eindeutigen Wert, der generiert wird, wenn ein Benutzer den Authentifizierungsprozess initiiert, und identifiziert die Authentifizierungssitzung eines Benutzers eindeutig, bis der Prozess abgeschlossen ist oder die Authentifizierungssitzung abläuft.

Der Authentifizierungs-Code ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Initiierung der Authentifizierungssitzung durch den `notAfter` Zeitstempel angegeben wird. Er gibt an, wie lange der Benutzer den Authentifizierungsprozess abschließen muss, bevor der Fluss neu gestartet werden muss.

Die Client-Anwendung kann den Authentifizierungs-Code verwenden, um zu überprüfen, ob der Benutzer die Authentifizierung erfolgreich abgeschlossen hat, und um die Profilinformationen des Benutzers abzurufen, normalerweise über einen Abrufmechanismus.

Die Client-Anwendung kann den Authentifizierungs-Code auch verwenden, um dem Benutzer zu ermöglichen, den Authentifizierungsprozess entweder auf demselben Gerät oder auf einem zweiten (Bildschirm-)Gerät abzuschließen oder fortzusetzen, da die Authentifizierungssitzung noch nicht abgelaufen ist.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Authentifizierungssitzung-API erstellen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Authentifizierungssitzung-API fortsetzen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundlegender Authentifizierungsfluss innerhalb der sekundären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;5. Wie kann die Client-Anwendung erkennen, ob der Benutzer einen gültigen Authentifizierungscode eingegeben hat und dass die Authentifizierungssitzung noch nicht abgelaufen ist? {#authentication-phase-faq5}

Die Client-Anwendung kann den vom Benutzer in eine sekundäre (Bildschirm-)Anwendung eingegebenen Authentifizierungscode überprüfen, indem sie eine Anfrage an einen der Sitzungs-Endpunkte sendet, der dafür verantwortlich ist, die Authentifizierungssitzung fortzusetzen oder Authentifizierungssitzungsinformationen abzurufen, die mit dem Authentifizierungscode verknüpft sind.

Die Client-Anwendung erhält einen [Fehler](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2), wenn der angegebene Authentifizierungscode falsch eingegeben wurde oder wenn die Authentifizierungssitzung abgelaufen ist.

Weitere Informationen finden Sie in den Dokumenten [Fortsetzen der Authentifizierungssitzung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) und [Abrufen der ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)).

#### &#x200B;6. Wie kann die Client-Anwendung erkennen, ob der Benutzer bereits authentifiziert ist? {#authentication-phase-faq6}

Die Client-Anwendung kann einen der folgenden Endpunkte abfragen, die überprüfen können, ob ein Benutzer bereits authentifiziert ist, und Profilinformationen zurückgeben:

* [Profil-Endpunkt-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profile-Endpunkt für bestimmte MVPD-APIs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profile-Endpunkt für bestimmte (Authentifizierungs-)Code-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### &#x200B;7. Was ist ein Profil und wie lange ist es gültig? {#authentication-phase-faq7}

Das Profil ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile) definiert ist.

Das Profil speichert Informationen über die Authentifizierungsgültigkeit des Benutzers, Metadateninformationen und viele mehr, die vom Endpunkt „Profiles“ abgerufen werden können.

Die Client-Anwendung kann das Profil verwenden, um den Authentifizierungsstatus des Benutzers zu ermitteln, auf Benutzermetadaten-Informationen zuzugreifen, die zur Authentifizierung verwendete Methode zu finden oder die Entität zu ermitteln, die zur Bereitstellung der Identität verwendet wird.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Profil-Endpunkt-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profile-Endpunkt für bestimmte MVPD-APIs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profile-Endpunkt für bestimmte (Authentifizierungs-)Code-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Das Profil ist für einen begrenzten Zeitraum gültig, der bei der Abfrage durch den `notAfter` Zeitstempel angegeben wird. Dies gibt an, wie lange der Benutzer über eine gültige Authentifizierung verfügt, bevor er die Authentifizierungsphase erneut durchlaufen muss.

Dieser begrenzte Zeitraum, der als Authentifizierung (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) bezeichnet wird, kann über das Adobe Pass [TVE-Dashboard ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsbeauftragten angezeigt und geändert werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;8. Sollte die Client-Anwendung die Profilinformationen des Benutzers in einem beständigen Speicher zwischenspeichern? {#authentication-phase-faq8}

Die Client-Anwendung sollte Teile der Profilinformationen des Benutzers in einem beständigen Speicher zwischenspeichern, um unnötige Anfragen zu vermeiden und das Benutzererlebnis unter Berücksichtigung der folgenden Aspekte zu verbessern:

| Attribut | Benutzererlebnis |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | Die Client-Anwendung kann dies verwenden, um den vom Benutzer ausgewählten TV-Anbieter zu verfolgen und ihn während der Vorautorisierungs- oder Autorisierungsphase weiter zu verwenden.<br/><br/>Wenn das aktuelle Benutzerprofil abläuft, kann die Client-Anwendung die gespeicherte MVPD-Auswahl verwenden und den Benutzer einfach zur Bestätigung auffordern. |
| `attributes` | Die Client-Anwendung kann dies verwenden, um das Benutzererlebnis basierend auf verschiedenen [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)-Schlüsseln (z. B. `zip`, `maxRating` usw.) zu personalisieren.<br/><br/>Benutzermetadaten werden nach Abschluss des Authentifizierungsflusses verfügbar, daher muss die Client-Anwendung keinen separaten Endpunkt abfragen, um die [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)-Informationen abzurufen, da sie bereits in den Profilinformationen enthalten sind.<br/><br/>Bestimmte Metadatenattribute können je nach MVPD und spezifischem Metadatenattribut während des Autorisierungsflusses aktualisiert werden. Daher muss die Client-Anwendung möglicherweise die Profil-APIs erneut abfragen, um die neuesten Benutzermetadaten abzurufen. |
| `notAfter` | Die Client-Anwendung kann dies verwenden, um das Ablaufdatum des Benutzerprofils zu verfolgen.<br/><br/>Die Fehlerbehandlung in der Client-Anwendung erfordert die Verarbeitung der [Fehler](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)-Codes (z. B. `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated` usw.), was darauf hinweist, dass die Client-Anwendung eine Authentifizierung des Benutzers erfordert. |

#### &#x200B;9. Kann die Client-Anwendung das Benutzerprofil erweitern, ohne dass eine erneute Authentifizierung erforderlich ist? {#authentication-phase-faq9}

Anzahl

Das Benutzerprofil kann ohne Benutzerinteraktion nicht über seine Gültigkeit hinaus verlängert werden, da sein Ablauf durch die mit MVPDs eingerichtete Authentifizierungs-TTL bestimmt wird.

Daher muss die Client-Anwendung den Benutzer auffordern, sich erneut zu authentifizieren und mit der MVPD-Anmeldeseite zu interagieren, um sein Profil auf unserem System zu aktualisieren.

Bei MVPDs, die [Home-Based Authentication](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) (HBA) unterstützen, muss der Benutzer jedoch keine Anmeldeinformationen eingeben.

#### &#x200B;10. Was sind die Anwendungsfälle für jeden verfügbaren Profilendpunkt? {#authentication-phase-faq10}

Die einfachen Profil-Endpunkte sind so konzipiert, dass sie Client-Anwendungen die Möglichkeit bieten, den Authentifizierungsstatus des Benutzers zu kennen, auf Benutzermetadaten-Informationen zuzugreifen, die zur Authentifizierung verwendete Methode oder die Entität zu finden, die zur Bereitstellung der Identität verwendet wird.

Jeder Endpunkt eignet sich für einen bestimmten Anwendungsfall wie folgt:

| API | Beschreibung | Anwendungsfall |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Profile-Endpunkt-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | Alle Benutzerprofile abrufen. | **Der Benutzer öffnet die Client-Anwendung zum ersten Mal**<br/><br/> In diesem Szenario verfügt die Client-Anwendung nicht über die vom Benutzer ausgewählte MVPD-Kennung, die in einem persistenten Speicher zwischengespeichert ist.<br/><br/>Daher wird eine einzige Anfrage gesendet, um alle verfügbaren Benutzerprofile abzurufen. |
| [Profile-Endpunkt für bestimmte MVPD-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | Rufen Sie das Benutzerprofil ab, das mit einer bestimmten MVPD verknüpft ist. | **Der Benutzer kehrt nach der Authentifizierung bei einem vorherigen Besuch zur Client-Anwendung zurück**<br/><br/> In diesem Fall muss die zuvor ausgewählte MVPD-Kennung des Benutzers in einem persistenten Speicher zwischengespeichert sein.<br/><br/>Daher wird eine einzige Anfrage gesendet, um das Benutzerprofil für diese spezifische MVPD abzurufen. |
| [Profile-Endpunkt für bestimmte (Authentifizierungs) Code-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Rufen Sie das Benutzerprofil ab, das mit einem bestimmten Authentifizierungs-Code verknüpft ist. | **Benutzer initiiert den Authentifizierungsprozess**<br/><br/> In diesem Szenario muss die Client-Anwendung feststellen, ob der Benutzer die Authentifizierung erfolgreich abgeschlossen hat, und muss seine Profilinformationen abrufen.<br/><br/>Daher wird ein Abrufmechanismus gestartet, um das mit dem Authentifizierungs-Code verknüpfte Benutzerprofil abzurufen. |

Weitere Informationen finden Sie in den Dokumenten [Fluss „Grundlegende Profile“, der in der ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) ausgeführt wird[ und „Fluss „Grundlegende Profile“, der in der sekundären Anwendung ausgeführt ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)).

Der SSO-Endpunkt Profile erfüllt einen anderen Zweck. Er bietet der Client-Anwendung die Möglichkeit, ein Benutzerprofil mithilfe der Antwort der Partnerauthentifizierung zu erstellen und es in einem einzigen, einmaligen Vorgang abzurufen.

| API | Beschreibung | Anwendungsfall |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Profile SSO Endpoint API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | Erstellen und Abrufen von Benutzerprofilen mithilfe der Antwort zur Partnerauthentifizierung. | **Benutzer erlaubt der Anwendung die Verwendung von Partner-Single-Sign-on zur Authentifizierung**<br/><br/> In diesem Szenario muss die Client-Anwendung ein Benutzerprofil erstellen, nachdem sie die Antwort zur Partnerauthentifizierung erhalten hat, und sie in einem einzigen, einmaligen Vorgang abrufen. |

Für alle nachfolgenden Abfragen müssen die Endpunkte „Basic Profiles“ verwendet werden, um den Authentifizierungsstatus des Benutzers zu bestimmen, auf Benutzermetadaten-Informationen zuzugreifen, die zur Authentifizierung verwendete Methode zu finden oder die Entität zu finden, die zur Bereitstellung der Identität verwendet wird.

Weitere Informationen finden Sie in den Dokumenten [Single Sign-on using Partner Flows](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) und [Apple SSO Cookbook (REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md).

#### &#x200B;11. Was sollte die Client-Anwendung tun, wenn die Benutzerin bzw. der Benutzer über mehrere MVPD-Profile verfügt? {#authentication-phase-faq11}

Die Entscheidung, mehrere Profile zu unterstützen, hängt von den Geschäftsanforderungen der Client-Anwendung ab.

Die meisten Benutzer haben nur ein Profil. Wenn jedoch mehrere Profile vorhanden sind (wie unten beschrieben), ist die Client-Anwendung dafür verantwortlich, das beste Benutzererlebnis für die Profilauswahl zu bestimmen.

Die Client-Anwendung kann den Benutzer auffordern, das gewünschte MVPD-Profil auszuwählen, oder die Auswahl automatisch vornehmen, z. B. das erste Benutzerprofil aus der Antwort oder das Profil mit der längsten Gültigkeitsdauer auswählen.

Die REST-API v2 unterstützt mehrere Profile, um Folgendes zu ermöglichen:

* Benutzer, die möglicherweise zwischen einem regulären MVPD-Profil und einem Profil wählen müssen, das über Single Sign-on (SSO) abgerufen wurde.
* Benutzern wird temporärer Zugriff angeboten, ohne dass sie sich von ihrem regulären MVPD abmelden müssen.
* Benutzende mit MVPD-Abonnement in Kombination mit Direct-to-Consumer-Services (DTC).
* Benutzende mit mehreren MVPD-Abonnements.

#### &#x200B;12. Was passiert, wenn Benutzerprofile ablaufen? {#authentication-phase-faq12}

Wenn Benutzerprofile ablaufen, werden sie nicht mehr in die Antwort vom Endpunkt „Profile“ aufgenommen.

Wenn der Endpunkt Profiles eine leere Antwort auf die Profilzuordnung zurückgibt, muss die Client-Anwendung eine neue Authentifizierungssitzung erstellen und den Benutzer zur erneuten Authentifizierung auffordern.

Weitere Informationen finden Sie in der Dokumentation [Erstellen einer Authentifizierungssitzung-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) .

#### &#x200B;13. Wann werden Benutzerprofile ungültig? {#authentication-phase-faq13}

Benutzerprofile werden in den folgenden Szenarien ungültig:

* Wann die Authentifizierungs-TTL abläuft, wie durch den `notAfter` Zeitstempel in der Antwort des Profilendpunkts angegeben.
* Wenn die Client-Anwendung den Header-[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) ändert.
* Wenn die Client-Anwendung die Client-Anmeldeinformationen aktualisiert, die zum Abrufen des [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)-Header-Werts verwendet werden.
* Wenn die Client-Anwendung die Software-Anweisung widerruft oder aktualisiert, die zum Abrufen der Client-Anmeldeinformationen verwendet wird.

#### &#x200B;14. Wann sollte die Client-Anwendung den Abrufmechanismus starten? {#authentication-phase-faq14}

Um Effizienz zu gewährleisten und unnötige Anfragen zu vermeiden, muss die Client-Anwendung den Abrufmechanismus unter folgenden Bedingungen starten:

**Authentifizierung wird innerhalb der primären (Bildschirm-)Anwendung durchgeführt**

Die primäre (Streaming-)Anwendung sollte den Abfragevorgang starten, wenn die Benutzerin oder der Benutzer die endgültige Zielseite erreicht, nachdem die Browser-Komponente die in der Endpunktanforderung `redirectUrl`Sitzungen[ für den ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) angegebene URL geladen hat.

**Authentifizierung wird in einer sekundären (Bildschirm-)Anwendung durchgeführt**

Die primäre (Streaming-) Anwendung sollte den Abfragevorgang starten, sobald der Benutzer den Authentifizierungsprozess einleitet - direkt nach Erhalt der Endpunktantwort [Sitzungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) und Anzeige des Authentifizierungs-Codes für den Benutzer.

#### &#x200B;15. Wann sollte die Client-Anwendung den Abrufmechanismus stoppen? {#authentication-phase-faq15}

Um Effizienz zu gewährleisten und unnötige Anfragen zu vermeiden, muss die Client-Anwendung den Abrufmechanismus unter folgenden Bedingungen stoppen:

**Erfolgreiche Authentifizierung**

Die Profilinformationen des Benutzers wurden erfolgreich abgerufen, wodurch der Authentifizierungsstatus bestätigt wird. Zu diesem Zeitpunkt ist keine Abfrage mehr erforderlich.

**Authentifizierungssitzung und Code-Ablauf**

Die Authentifizierungssitzung und der Code laufen ab, wie durch den `notAfter` Zeitstempel (z. B. 30 Minuten) in der Endpunktantwort [Sitzungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) angegeben. In diesem Fall muss der Benutzer den Authentifizierungsprozess neu starten, und das Abrufen mit dem vorherigen Authentifizierungs-Code sollte sofort gestoppt werden.

**Neuer Authentifizierungs-Code generiert**

Wenn der Benutzer einen neuen Authentifizierungscode auf dem Primärgerät (Bildschirm) anfordert, ist die vorhandene Sitzung nicht mehr gültig und das Abrufen mit dem vorherigen Authentifizierungscode sollte sofort gestoppt werden.

#### &#x200B;16. Welches Intervall zwischen Aufrufen sollte die Client-Anwendung für den Abrufmechanismus verwenden? {#authentication-phase-faq16}

Um die Effizienz zu gewährleisten und unnötige Anfragen zu vermeiden, muss die Client-Anwendung die Häufigkeit des Abrufmechanismus unter den folgenden Bedingungen konfigurieren:

| **Authentifizierung wird innerhalb der primären (Bildschirm-)Anwendung durchgeführt** | **Authentifizierung wird in einer sekundären (Bildschirm-)Anwendung durchgeführt** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| Die primäre (Streaming-)Anwendung sollte alle 3-5 Sekunden oder länger abfragen. | Die primäre (Streaming-)Anwendung sollte alle 3-5 Sekunden oder länger abfragen. |

#### &#x200B;17. Wie viele Abrufanfragen kann die Client-Anwendung maximal senden? {#authentication-phase-faq17}

Die Client-Anwendung muss die aktuellen Beschränkungen einhalten, die vom Adobe Pass-Authentifizierungsmechanismus ([) definiert ](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits).

Die Fehlerbehandlung in der Client-Anwendung muss den Fehlercode [429: Zu viele Anfragen](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response) verarbeiten können, der angibt, dass die Client-Anwendung die maximal zulässige Anzahl von Anfragen überschritten hat.

Weitere Informationen finden Sie in der Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) .

#### &#x200B;18. Wie kann die Client-Anwendung die Metadateninformationen des Benutzers abrufen? {#authentication-phase-faq18}

Die Client-Anwendung kann einen der folgenden Endpunkte abfragen, die [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) als Teil der Profilinformationen zurückgeben können:

* [Profil-Endpunkt-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profile-Endpunkt für bestimmte MVPD-APIs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profile-Endpunkt für bestimmte (Authentifizierungs-)Code-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Benutzermetadaten werden nach Abschluss des Authentifizierungsflusses verfügbar, daher muss die Client-Anwendung keinen separaten Endpunkt abfragen, um die [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)-Informationen abzurufen, da sie bereits in den Profilinformationen enthalten sind.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Bestimmte Metadatenattribute können je nach MVPD und spezifischem Metadatenattribut während des Autorisierungsflusses aktualisiert werden. Daher muss die Client-Anwendung möglicherweise die oben genannten APIs erneut abfragen, um die neuesten Benutzermetadaten abzurufen.

#### &#x200B;19. Wie sollte die Client-Anwendung einen eingeschränkten Zugriff verwalten? {#authentication-phase-faq19}

Die [Abbaufunktion](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) ermöglicht es der Client-Anwendung, ein nahtloses Streaming-Erlebnis für Benutzende aufrechtzuerhalten, auch wenn bei den Authentifizierungs- oder Autorisierungs-Services ihrer MVPD Probleme auftreten.

Zusammenfassend lässt sich sagen, dass dadurch trotz temporärer Service-Unterbrechungen in MVPD ein unterbrechungsfreier Zugriff auf Inhalte sichergestellt werden kann.

Da Ihr Unternehmen beabsichtigt, die Premium-Funktion zur Beeinträchtigung zu verwenden, muss die Client-Anwendung eingeschränkte Zugriffsflüsse handhaben, die beschreiben, wie sich REST-API-v2-Endpunkte in solchen Szenarien verhalten.

Weitere Informationen finden Sie in der Dokumentation [Gestörte ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)&quot;.

#### &#x200B;20. Wie sollte die Client-Anwendung den temporären Zugriff verwalten? {#authentication-phase-faq20}

Mit [TempPass-Funktion](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) kann die Client-Anwendung dem Benutzer temporären Zugriff gewähren.

Zusammenfassend lässt sich sagen, dass dies für Benutzende interessant sein kann, indem für einen bestimmten Zeitraum ein zeitlich begrenzter Zugriff auf Inhalte oder eine vordefinierte Anzahl von VOD-Titeln bereitgestellt wird.

Da Ihr Unternehmen beabsichtigt, die Premium-TempPass-Funktion zu verwenden, muss die Client-Anwendung temporäre Zugriffsflüsse verarbeiten, die beschreiben, wie sich REST-API-v2-Endpunkte in solchen Szenarien verhalten.

In früheren API-Versionen musste sich die Client-Anwendung von einem Benutzer abmelden, der sich bei seiner regulären MVPD authentifiziert hatte, um einen temporären Zugriff anzubieten.

Mit der REST-API v2 kann die Client-Anwendung bei der Autorisierung eines Streams nahtlos zwischen einer regulären MVPD und einer TempPass-MVPD wechseln, sodass sich der Benutzer nicht mehr abmelden muss.

Weitere Informationen finden Sie in der Dokumentation [Temporäre Zugriffsflüsse](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) .

#### &#x200B;21. Wie sollte die Client-Anwendung den geräteübergreifenden Single-Sign-On-Zugriff verwalten? {#authentication-phase-faq21}

Die REST-API v2 kann geräteübergreifendes Single Sign-on aktivieren, wenn die Client-Anwendung eine konsistente eindeutige Benutzerkennung über Geräte hinweg bereitstellt.

Diese Kennung, auch als Service-Token bezeichnet, muss von der Client-Anwendung durch die Implementierung oder Integration eines externen Identity Services Ihrer Wahl generiert werden.

Weitere Informationen finden Sie in der Dokumentation [Single Sign-on using Service Token Flows](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

+++

### Häufig gestellte Fragen zur Vorautorisierungsphase {#preauthorization-phase-faqs-general}

+++Häufig gestellte Fragen zur Vorautorisierungsphase

#### &#x200B;1. Was ist der Zweck der Vorabgenehmigungsphase? {#preauthorization-phase-faq1}

Der Zweck der Vorautorisierungsphase besteht darin, der Client-Anwendung die Möglichkeit zu geben, eine Untergruppe von Ressourcen aus ihrem Katalog darzustellen, auf die der Benutzer zugreifen könnte.

Die Vorautorisierungsphase kann das Benutzererlebnis verbessern, wenn der Benutzer die Client-Anwendung zum ersten Mal öffnet oder zu einem neuen Abschnitt navigiert.

#### &#x200B;2. Ist die Phase vor der Autorisierung obligatorisch? {#preauthorization-phase-faq2}

Die Vorautorisierungsphase ist nicht obligatorisch. Die Client-Anwendung kann diese Phase überspringen, wenn sie einen Katalog von Ressourcen präsentieren möchte, ohne sie zuerst auf der Grundlage der Benutzerberechtigung zu filtern.

#### &#x200B;3. Was ist eine Entscheidung vor der Zulassung? {#preauthorization-phase-faq3}

Die Vorautorisierung ist ein Begriff, der in der [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization)-Dokumentation definiert ist, während der Entscheidungsbegriff auch im [Glossar) ](rest-api-v2-glossary.md#decision) ist.

Die Vorautorisierungsentscheidung speichert Informationen zum Abfrageergebnis des MVPD-Vorautorisierungsprozesses, die vom Endpunkt Decisions Preauthorize abgerufen werden können.

Die Client-Anwendung kann die Entscheidungen vor der Autorisierung verwenden, um eine Untergruppe von Ressourcen darzustellen, auf die der TV-Anbieter (informative) Entscheidungen dem Benutzer Zugriff gewähren würde.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Abrufen der Vorabautorisierungsentscheidungen-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Grundlegender Vorautorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### &#x200B;4. Sollte die Client-Anwendung die Entscheidungen zur Vorabautorisierung in einem persistenten Speicher zwischenspeichern? {#preauthorization-phase-faq4}

Die Client-Anwendung ist nicht erforderlich, um Entscheidungen vor der Autorisierung in einem persistenten Speicher zu speichern. Es wird jedoch empfohlen, Zulassungsentscheidungen im Speicher zwischenzuspeichern, um das Benutzererlebnis zu verbessern. Auf diese Weise können unnötige Aufrufe des Endpunkts Decisions Preauthorize für bereits vorautorisierte Ressourcen vermieden werden, wodurch die Latenz verringert und die Leistung verbessert wird.

#### &#x200B;5. Wie kann der Client-Antrag feststellen, warum eine Vorabautorisierungsentscheidung verweigert wurde? {#preauthorization-phase-faq5}

Die Client-Anwendung kann den Grund für eine abgelehnte Vorautorisierungsentscheidung ermitteln, indem sie den [Fehlercode und die Nachricht) überprüft](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) die in der Antwort vom Vorautorisierungs-Endpunkt für Entscheidungen enthalten sind. Diese Details geben insight den spezifischen Grund an, aus dem die Vorabautorisierungsanfrage abgelehnt wurde. So kann das Benutzererlebnis oder der Trigger über die erforderliche Verarbeitung in der Anwendung informiert werden.

Stellen Sie sicher, dass ein Wiederholungsmechanismus, der zum Abrufen von Entscheidungen vor der Autorisierung implementiert ist, nicht zu einer Endlosschleife führt, wenn die Entscheidung vor der Autorisierung abgelehnt wird.

Erwägen Sie, weitere Zustellversuche auf eine angemessene Anzahl zu beschränken und Ablehnungen elegant zu handhaben, indem Sie dem Benutzer klares Feedback senden.

#### &#x200B;6. Warum fehlt in der Vorabautorisierungsentscheidung ein Medien-Token? {#preauthorization-phase-faq6}

Der Vorautorisierungsentscheidung fehlt ein Medien-Token, da die Vorautorisierungsphase nicht zum Wiedergeben von Ressourcen verwendet werden darf, da dies der Zweck der Autorisierungsphase ist.

#### &#x200B;7. Kann die Autorisierungsphase übersprungen werden, wenn bereits eine Entscheidung vor der Autorisierung existiert? {#preauthorization-phase-faq7}

Anzahl

Die Autorisierungsphase kann auch dann nicht übersprungen werden, wenn eine Entscheidung vor der Autorisierung verfügbar ist. Entscheidungen vor der Autorisierung dienen nur zu Informationszwecken und gewähren keine tatsächlichen Wiedergaberechte. Die Vorautorisierungsphase soll eine frühe Anleitung bieten, aber die Autorisierungsphase ist weiterhin erforderlich, bevor Inhalte wiedergegeben werden können.

#### &#x200B;8. Was ist eine Ressource und welche Formate werden unterstützt? {#preauthorization-phase-faq8}

Die Ressource ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) definiert ist.

Die Ressource ist eine eindeutige Kennung, die mit MVPDs vereinbart wird und mit einem Inhalt verknüpft ist, den die Client-Anwendung streamen könnte.

Die eindeutige Kennung der Ressource kann zwei Formate aufweisen:

* Ein einfaches Zeichenfolgenformat wie eine eindeutige Kennung für einen Kanal (eine Marke).
* Ein RSS-Format für Medien (MRSS), das zusätzliche Informationen wie Titel, Bewertungen und Metadaten zur elterlichen Kontrolle enthält.

Weitere Informationen finden Sie in der Dokumentation [Geschützte Ressourcen](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) .

#### &#x200B;9. Für wie viele Ressourcen kann die Client-Anwendung gleichzeitig eine Vorabautorisierungsentscheidung erhalten? {#preauthorization-phase-faq9}

Die Client-Anwendung kann aufgrund von Bedingungen, die von MVPDs auferlegt werden, in einer einzelnen API-Anfrage eine Vorabautorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen erhalten, normalerweise bis zu 5.

Diese maximale Anzahl von Ressourcen kann angezeigt und geändert werden, nachdem eine Vereinbarung mit MVPDs über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter getroffen wurde.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### Häufig gestellte Fragen zur Genehmigungsphase {#authorization-phase-faqs-general}

+++Häufig gestellte Fragen zur Autorisierungsphase

#### &#x200B;1. Was ist der Zweck der Genehmigungsphase? {#authorization-phase-faq1}

In der Autorisierungsphase soll der Client-Anwendung die Möglichkeit gegeben werden, die von den Benutzenden angeforderten Ressourcen abzuspielen, nachdem ihre Rechte bei der MVPD validiert wurden.

#### &#x200B;2. Ist die Autorisierungsphase obligatorisch? {#authorization-phase-faq2}

Die Autorisierungsphase ist obligatorisch. Die Client-Anwendung kann diese Phase nicht überspringen, wenn sie die von der Benutzerin oder dem Benutzer angeforderten Ressourcen wiedergeben möchte, da sie bzw. er bei der MVPD überprüfen muss, ob die Benutzerin oder der Benutzer berechtigt ist, bevor der Stream freigegeben wird.

#### &#x200B;3. Was ist eine Zulassungsentscheidung und wie lange ist sie gültig? {#authorization-phase-faq3}

Der Begriff Autorisierung ist in der [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization)-Dokumentation definiert, während der Begriff Entscheidung auch im [Glossar) ](rest-api-v2-glossary.md#decision) ist.

Die Autorisierungsentscheidung speichert Informationen zum Abfrageergebnis des MVPD-Autorisierungsprozesses, die vom Entscheidungsautorisierungs-Endpunkt abgerufen werden können.

Die Client-Anwendung kann die Autorisierungsentscheidung mit einem Medien-Token verwenden, um den Ressourcen-Stream abzuspielen, falls die Entscheidung des TV-Anbieters (autorisierend) dem Benutzer den Zugriff darauf ermöglichen würde.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Abrufen der Autorisierungsentscheidungen-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Die Autorisierungsentscheidung ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Ausgabe angegeben ist. Dabei wird die Zeit angegeben, die von der Adobe Pass-Authentifizierung zwischengespeichert wird, bevor die MVPD erneut abgefragt werden muss.

Dieser begrenzte Zeitraum, der als Autorisierung (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) bezeichnet wird, kann über das Adobe Pass [TVE-Dashboard ](rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsbeauftragten angezeigt und geändert werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;4. Sollte die Client-Anwendung die Autorisierungsentscheidungen in einem persistenten Speicher zwischenspeichern? {#authorization-phase-faq4}

Die Client-Anwendung ist nicht erforderlich, um Autorisierungsentscheidungen in einem persistenten Speicher zu speichern.

#### &#x200B;5. Wie kann der Kundenantrag feststellen, warum eine Autorisierungsentscheidung verweigert wurde? {#authorization-phase-faq5}

Die Client-Anwendung kann den Grund für eine Entscheidung bezüglich der verweigerten Autorisierung ermitteln, indem sie den [Fehlercode und die Nachricht) überprüft](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) die in der Antwort vom Endpunkt Decisions Authorize enthalten sind. Diese Details geben insight den spezifischen Grund an, aus dem die Autorisierungsanfrage abgelehnt wurde. So kann das Benutzererlebnis oder der Trigger über die erforderliche Verarbeitung in der Anwendung informiert werden.

Stellen Sie sicher, dass ein zum Abrufen von Autorisierungsentscheidungen implementierter Wiederholungsmechanismus nicht zu einer Endlosschleife führt, wenn die Autorisierungsentscheidung abgelehnt wird.

Erwägen Sie, weitere Zustellversuche auf eine angemessene Anzahl zu beschränken und Ablehnungen elegant zu handhaben, indem Sie dem Benutzer klares Feedback senden.

#### &#x200B;6. Was ist ein Medien-Token und wie lange ist es gültig? {#authorization-phase-faq6}

Das Medien-Token ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token) definiert ist.

Das Medien-Token besteht aus einer signierten Zeichenfolge, die als Klartext gesendet wird und vom Entscheidungsautorisierungs-Endpunkt abgerufen werden kann.

Weitere Informationen finden Sie in der Dokumentation [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) .

Das Medien-Token ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Ausgabe angegeben ist. Es gibt die Zeitspanne an, bevor es von der Client-Anwendung überprüft und verwendet werden muss.

Die Client-Anwendung kann das Medien-Token verwenden, um einen Ressourcen-Stream abzuspielen, falls die Entscheidung des TV-Anbieters (autoritär) dem Benutzer den Zugriff darauf ermöglichen würde.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Abrufen der Autorisierungsentscheidungen-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### &#x200B;7. Soll die Client-Anwendung das Medien-Token validieren, bevor der Ressourcen-Stream wiedergegeben wird? {#authorization-phase-faq7}

Ja.

Die Client-Anwendung muss das Medien-Token validieren, bevor die Wiedergabe des Ressourcen-Streams gestartet wird. Diese Validierung sollte mit dem „Media [ Verifier“ durchgeführt ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). Durch die Überprüfung der `serializedToken` aus der zurückgegebenen `token` verhindert die Client-Anwendung nicht autorisierten Zugriff, wie z. B. das Stream-Ripping, und stellt sicher, dass nur ordnungsgemäß autorisierte Benutzer den Inhalt wiedergeben können.

#### &#x200B;8. Soll die Client-Anwendung während der Wiedergabe ein abgelaufenes Medien-Token aktualisieren? {#authorization-phase-faq8}

Anzahl

Die Client-Anwendung ist nicht erforderlich, um ein abgelaufenes Medien-Token zu aktualisieren, während der Stream aktiv wiedergegeben wird. Wenn das Medien-Token während der Wiedergabe abläuft, sollte der Stream ununterbrochen fortgesetzt werden können. Der Client muss jedoch beim nächsten Versuch, eine Ressource abzuspielen, eine neue Autorisierungsentscheidung anfordern und ein neues Medien-Token abrufen.

#### &#x200B;9. Was ist der Zweck jedes Zeitstempelattributs in der Autorisierungsentscheidung? {#authorization-phase-faq9}

Die Autorisierungsentscheidung enthält mehrere Zeitstempelattribute, die einen wesentlichen Kontext zur Gültigkeitsdauer der Autorisierung selbst und des zugehörigen Medien-Tokens bieten. Diese Zeitstempel dienen verschiedenen Zwecken, je nachdem, ob sie sich auf die Autorisierungsentscheidung oder das Medien-Token beziehen.

**Zeitstempel auf Entscheidungsebene**

Diese Zeitstempel beschreiben den Gültigkeitszeitraum der gesamten Autorisierungsentscheidung:

| Attribut | Beschreibung | Notizen |
|-------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Der Zeitpunkt in Millisekunden, zu dem die Autorisierungsentscheidung erlassen wurde. | Dies markiert den Beginn des Gültigkeitsfensters der Autorisierung. |
| `notAfter` | Der Zeitpunkt in Millisekunden, zu dem die Autorisierungsentscheidung abläuft. | Die [Time-to-Live (TTL) für die Autorisierung](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management) bestimmt, wie lange die Autorisierung gültig bleibt, bevor eine erneute Autorisierung erforderlich wird. Diese TTL wird mit MVPD-Mitarbeitern ausgehandelt. |

**Zeitstempel auf Token-Ebene**

Diese Zeitstempel beschreiben den Gültigkeitszeitraum des Medien-Tokens, das an die Autorisierungsentscheidung gebunden ist:

| Attribut | Beschreibung | Notizen |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Die Zeit in Millisekunden, zu der das Medien-Token ausgegeben wurde. | Dies wird markiert, wenn das Token für die Wiedergabe gültig wird. |
| `notAfter` | Der Zeitpunkt in Millisekunden, zu dem das Medien-Token abläuft. | Medien-Token haben eine absichtlich kurze Lebensdauer (in der Regel 7 Minuten), um Missbrauchsrisiken zu minimieren und potenzielle Uhrzeitunterschiede zwischen dem Token-generierenden Server und dem Token-verifizierenden Server zu berücksichtigen. |

#### &#x200B;10. Was ist eine Ressource und welche Formate werden unterstützt? {#authorization-phase-faq10}

Die Ressource ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) definiert ist.

Die Ressource ist eine eindeutige Kennung, die mit MVPDs vereinbart wird und mit einem Inhalt verknüpft ist, den die Client-Anwendung streamen könnte.

Die eindeutige Kennung der Ressource kann zwei Formate aufweisen:

* Ein einfaches Zeichenfolgenformat wie eine eindeutige Kennung für einen Kanal (eine Marke).
* Ein RSS-Format für Medien (MRSS), das zusätzliche Informationen wie Titel, Bewertungen und Metadaten zur elterlichen Kontrolle enthält.

Weitere Informationen finden Sie in der Dokumentation [Geschützte Ressourcen](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) .

#### &#x200B;11. Für wie viele Ressourcen kann die Client-Anwendung gleichzeitig eine Autorisierungsentscheidung erhalten? {#authorization-phase-faq11}

Die Client-Anwendung kann in einer einzelnen API-Anfrage, in der Regel bis zu 1, eine Autorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen erhalten. Dies ist auf die von MVPDs auferlegten Bedingungen zurückzuführen.

+++

### Häufig gestellte Fragen zur Abmeldephase {#logout-phase-faqs-general}

+++Häufig gestellte Fragen zur Abmeldephase

#### &#x200B;1. Was ist der Zweck der Abmeldephase? {#logout-phase-faq1}

Die Abmeldephase soll der Client-Anwendung die Möglichkeit geben, das authentifizierte Benutzerprofil innerhalb der Adobe Pass-Authentifizierung auf Benutzeranfrage zu beenden.

#### &#x200B;2. Ist die Abmeldephase obligatorisch? {#logout-phase-faq2}

Die Abmeldephase ist obligatorisch. Die Client-Anwendung muss dem Benutzer die Möglichkeit zum Abmelden bereitstellen.

+++

### Häufig gestellte Fragen zu Kopfzeilen {#headers-faqs-general}

+++Häufig gestellte Fragen zu Kopfzeilen

#### &#x200B;1. Wie wird der Wert für die Autorisierungs-Kopfzeile berechnet? {#headers-faq1}

>[!IMPORTANT]
>
> Wenn die Client-Anwendung von REST API V1 zu REST API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode verwenden, um den Wert des `Bearer` Zugriffstokens wie zuvor abzurufen.

Der [Autorisierungs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)-Anfrage-Header enthält das `Bearer` Zugriffstoken, das von der Client-Anwendung für den Zugriff auf durch Adobe Pass geschützte APIs benötigt wird.

Der Wert der Autorisierungs-Kopfzeile muss während der Registrierungsphase von der Adobe Pass-Authentifizierung abgerufen werden.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Abrufen von Client-Anmeldeinformationen API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Zugriffstoken-API abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### &#x200B;2. Wie wird der Wert für die Kopfzeile „AP-Device-Identifier“ berechnet? {#headers-faq2}

>[!IMPORTANT]
>
> Wenn die Client-Anwendung von REST API V1 zu REST API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode zur Berechnung des Gerätekennungswerts wie zuvor verwenden.

Der [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)-Anforderungsheader enthält die Streaming-Gerätekennung, wie sie von der Client-Anwendung erstellt wurde.

Die Kopfzeilendokumentation [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) enthält Beispiele für die wichtigsten Plattformen für die Berechnung des Werts, aber die Client-Anwendung kann basierend auf ihrer eigenen Geschäftslogik und ihren Anforderungen eine andere Methode verwenden.

#### &#x200B;3. Wie wird der Wert für den X-Device-Info-Header berechnet? {#headers-faq3}

>[!IMPORTANT]
>
> Wenn die Client-Anwendung von REST API V1 zu REST API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode zur Berechnung des Geräteinformationswerts wie zuvor verwenden.

Der [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)-Anfrage-Header enthält die Client-Informationen (Gerät, Verbindung und Anwendung), die sich auf das eigentliche Streaming-Gerät beziehen, und wird verwendet, um plattformspezifische Regeln zu bestimmen, die von MVPDs erzwungen werden können.

Die [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)-Header-Dokumentation enthält Beispiele für Hauptplattformen zur Berechnung des Werts, aber die Client-Anwendung kann basierend auf ihrer eigenen Geschäftslogik und ihren Anforderungen eine andere Methode verwenden.

Wenn der Header [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) fehlt oder falsche Werte enthält, kann die Anfrage als von einer `unknown` Plattform stammend klassifiziert werden.

Dies kann dazu führen, dass die Anfrage als unsicher behandelt wird und restriktiveren Regeln unterliegt, wie z. B. kürzeren Authentifizierungs-TTLs. Darüber hinaus sind einige Felder, wie die `connectionIp` und `connectionPort` des Streaming-Geräts, für Funktionen wie die „Home Base Authentication[ von Spectrum ](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md).

Selbst wenn die Anfrage im Auftrag eines Geräts von einem Server stammt, muss der [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)-Header-Wert die tatsächlichen Streaming-Geräteinformationen widerspiegeln.

+++

### Häufig gestellte Fragen (FAQ) {#misc-faqs-general}

+++Häufig gestellte Fragen zu Sonstiges

#### &#x200B;1. Kann ich REST API V2-Anfragen und -Antworten untersuchen und die API testen? {#misc-faq1}

Ja.

Sie können die REST-API V2 über unsere spezielle [Adobe Developer](https://developer.adobe.com/adobe-pass/)-Website erkunden. Die Adobe Developer-Website bietet uneingeschränkten Zugriff auf:

* [DCR-API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Um mit [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) zu interagieren, müssen Sie den [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)-Header mit einem `Bearer` Zugriffstoken einfügen, das über die [DCR-API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Für die Verwendung der [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) ist eine Software-Anweisung mit dem REST API V2-Umfang erforderlich. Weitere Informationen finden Sie in den häufig gestellten [ zur Dynamic Client-Registrierung (DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

#### &#x200B;2. Kann ich REST API V2-Anfragen und -Antworten mithilfe eines API-Entwicklungs-Tools mit OpenAPI-Unterstützung durchsuchen? {#misc-faq2}

Ja.

Sie können OpenAPI-Spezifikationsdateien für die [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) und [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) von der [Adobe Developer](https://developer.adobe.com/adobe-pass/)-Website herunterladen.

Um die OpenAPI-Spezifikationsdateien herunterzuladen, klicken Sie auf die Download-Schaltflächen, um die folgenden Dateien auf Ihrem lokalen Computer zu speichern:

* [DCR-API - JSON](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

Sie können diese Dateien dann in Ihr bevorzugtes API-Entwicklungs-Tool importieren, um REST-API-V2-Anfragen und -Antworten zu untersuchen und die API zu testen.

#### &#x200B;3. Kann ich weiterhin das vorhandene API-Test-Tool verwenden, das unter https://sp.auth-staging.adobe.com/apitest/api.html gehostet wird? {#misc-faq3}

Anzahl

Die Client-Programme, die zur REST API V2 migrieren, sollten das neue Test-Tool verwenden, das unter https://developer.adobe.com/adobe-pass/ gehostet wird. Die Adobe Developer-Website bietet uneingeschränkten Zugriff auf:

* [DCR-API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Um mit [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) zu interagieren, müssen Sie den [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)-Header mit einem `Bearer` Zugriffstoken einfügen, das über die [DCR-API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Für die Verwendung der [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) ist eine Software-Anweisung mit dem REST API V2-Umfang erforderlich. Weitere Informationen finden Sie in den häufig gestellten [ zur Dynamic Client-Registrierung (DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

+++

## Häufig gestellte Fragen zur Migration {#migration-faqs}

Fahren Sie mit diesem Abschnitt fort, wenn Sie an einem Programm arbeiten, das ein vorhandenes Programm zu REST API V2 migrieren muss.

>[!MORELIKETHIS]
>
> * [Häufig gestellte Fragen zur Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Häufig gestellte Fragen zur allgemeinen Migration {#general-migration-faqs}

+++Häufig gestellte Fragen zur allgemeinen Migration

#### &#x200B;1. Muss ich eine neue Client-Anwendung einführen, die zu REST API V2 migriert wurde, und zwar für alle Benutzer gleichzeitig? {#migration-faq1}

Anzahl

Die Client-Anwendung ist nicht erforderlich, um eine neue Version einzuführen, die die REST-API V2 gleichzeitig für alle Benutzer integriert.

Die Adobe Pass-Authentifizierung unterstützt bis Ende 2025 weiterhin ältere Client-Anwendungsversionen, die die REST-API V1 oder SDK integrieren.

#### &#x200B;2. Muss ich eine neue Client-Anwendung einführen, die zur REST-API V2 über alle APIs und Flüsse hinweg gleichzeitig migriert wurde? {#migration-faq2}

Ja.

Die Client-Anwendung ist erforderlich, um eine neue Version einzuführen, die die REST-API V2 über alle Adobe Pass-Authentifizierungs-APIs und -Flüsse hinweg gleichzeitig integriert.

Im Falle des Flusses „Authentifizierung auf dem zweiten Bildschirm“ ist die Client-Anwendung erforderlich, um eine neue Version einzuführen, die die REST-API V2 für die [primären](rest-api-v2-glossary.md#primary-application) und [sekundären](rest-api-v2-glossary.md#secondary-application)-Anwendungen gleichzeitig integriert.

Die Adobe Pass-Authentifizierung unterstützt keine „hybriden“ Implementierungen, die sowohl die REST-API V2 als auch die REST-API V1/SDK zwischen APIs und Flüssen integrieren.

#### &#x200B;3. Wird die Benutzerauthentifizierung beim Aktualisieren auf eine neue Client-Anwendung beibehalten, die zur REST API V2 migriert wurde? {#migration-faq3}

Anzahl

Die Benutzerauthentifizierung, die in älteren Client-Anwendungsversionen mit der REST-API V1 oder SDK erhalten wurde, wird nicht beibehalten.

Daher muss sich der Benutzer innerhalb der neuen Client-Anwendung, die zur REST-API V2 migriert wurde, erneut authentifizieren.

#### &#x200B;4. Sind die erweiterten Fehler-Codes in der REST-API V2 standardmäßig aktiviert? {#migration-faq4}

Ja.

Die Client-Programme, die zur REST API V2 migrieren, profitieren standardmäßig automatisch von dieser Funktion und bieten detailliertere und präzisere Fehlerinformationen.

Weitere Informationen finden Sie in der Dokumentation [Erweiterte ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)).

#### &#x200B;5. Benötigen bestehende Integrationen bei der Migration auf REST API V2 Konfigurationsänderungen? {#migration-faq5}

Anzahl

Für die Client-Programme, die zur REST API V2 migrieren, sind keine Konfigurationsänderungen für bestehende MVPD-Integrationen erforderlich. Außerdem werden sie weiterhin dieselben Kennungen für bestehende [Dienstleister](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) und [MVPDs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd) verwenden.

Darüber hinaus bleiben die Protokolle, die von der Adobe Pass-Authentifizierung zur Kommunikation mit MVPD-Endpunkten verwendet werden, unverändert.

+++

### Migration von der REST API V1 zur REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Fahren Sie mit diesem Unterabschnitt fort, wenn Sie an einem Programm arbeiten, das von REST API V1 zu REST API V2 migriert werden muss.

#### Häufig gestellte Fragen zur Registrierungsphase {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Registrierungsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Registrierungsphase erforderlich? {#registration-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 gibt es keine allgemeinen Änderungen in Bezug auf die Registrierungsphase.

Die Client-Anwendung kann denselben Fluss weiterhin verwenden, um sich über den [Dynamische Client-Registrierung (DCR) bei der Adobe Pass](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr)Authentifizierung zu registrieren und ein Zugriffstoken zu erhalten.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Abrufen von Client-Anmeldeinformationen API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Zugriffstoken-API abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### Häufig gestellte Fragen zur Konfigurationsphase {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Konfigurationsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Konfigurationsphase erforderlich? {#configuration-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Häufig gestellte Fragen zur Authentifizierungsphase {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Authentifizierungsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Authentifizierungsphase erforderlich? {#authentication-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungs-Code (Authentifizierungs-Code) abrufen | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST-<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registrierungs-Code prüfen (Authentifizierungs-Code) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Authentifizierung (MVPD) auf dem zweiten Bildschirm fortsetzen (Anwendung) | [GET <br/> /api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST-<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD)-Authentifizierung initiieren | [GET <br/> /api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [GET <br/> /api/v1/checkauthn (erster Bildschirm)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (zweiter Bildschirm)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen des Benutzerauthentifizierungs-Tokens (Profil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Vorautorisierungsphase {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Vorautorisierungsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Vorautorisierungsphase erforderlich? {#preauthorization-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen vorab autorisierter Ressourcen (Entscheidungen vor der Autorisierung) | [GET <br/> /api/v1/preauthorize (erster Bildschirm)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (zweiter Bildschirm)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Genehmigungsphase {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Autorisierungsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Autorisierungsphase erforderlich? {#authorization-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD)-Autorisierung initiieren | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Autorisierungs-Token abrufen (Autorisierungsentscheidung) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Abrufen eines kurzen Autorisierungs-Tokens (Medien-Token) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Abmeldephase {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Abmeldephase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Abmeldephase erforderlich? {#logout-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren des Abmeldevorgangs | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migration von SDK zur REST-API V2 {#migration-sdk-to-rest-api-v2}

Fahren Sie mit diesem Unterabschnitt fort, wenn Sie an einem Programm arbeiten, das von SDK zu REST API V2 migriert werden muss.

#### Häufig gestellte Fragen zur Registrierungsphase {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Häufig gestellte Fragen zur Registrierungsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Registrierungsphase erforderlich? {#registration-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in den folgenden Tabellen aufgeführt sind:

###### AccessEnabler JavaScript SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abschließen der dynamischen Client-Registrierung (DCR) | Bereitstellen einer Software-Anweisung für den Konstruktor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abschließen der dynamischen Client-Registrierung (DCR) | Bereitstellen einer Software-Anweisung für den Konstruktor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abschließen der dynamischen Client-Registrierung (DCR) | Bereitstellen einer Software-Anweisung für den Konstruktor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abschließen der dynamischen Client-Registrierung (DCR) | Bereitstellen einer Software-Anweisung für den Konstruktor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Konfigurationsphase {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Häufig gestellte Fragen zur Konfigurationsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Konfigurationsphase erforderlich? {#configuration-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in den folgenden Tabellen aufgeführt sind:

###### AccessEnabler JavaScript SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Häufig gestellte Fragen zur Authentifizierungsphase {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++Häufig gestellte Fragen zur Authentifizierungsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Authentifizierungsphase erforderlich? {#authentication-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in den folgenden Tabellen aufgeführt sind:

###### AccessEnabler JavaScript SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungs-Code (Authentifizierungs-Code) abrufen | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST-<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registrierungs-Code prüfen (Authentifizierungs-Code) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Authentifizierung (MVPD) auf dem zweiten Bildschirm fortsetzen (Anwendung) | [GET <br/> /api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST-<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungs-Code (Authentifizierungs-Code) abrufen | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST-<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registrierungs-Code prüfen (Authentifizierungs-Code) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Authentifizierung (MVPD) auf dem zweiten Bildschirm fortsetzen (Anwendung) | [GET <br/> /api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST-<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Vorautorisierungsphase {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Häufig gestellte Fragen zur Vorautorisierungsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Vorautorisierungsphase erforderlich? {#preauthorization-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in den folgenden Tabellen aufgeführt sind:

###### AccessEnabler JavaScript SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen vorab autorisierter Ressourcen (Entscheidungen vor der Autorisierung) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen vorab autorisierter Ressourcen (Entscheidungen vor der Autorisierung) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen vorab autorisierter Ressourcen (Entscheidungen vor der Autorisierung) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Umfang | SDK | REST-API V2 | Beobachtungen |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen vorab autorisierter Ressourcen (Entscheidungen vor der Autorisierung) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Genehmigungsphase {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Häufig gestellte Fragen zur Autorisierungsphase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Autorisierungsphase erforderlich? {#authorization-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in den folgenden Tabellen aufgeführt sind:

###### AccessEnabler JavaScript SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen eines kurzen Autorisierungs-Tokens (Medien-Token) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen eines kurzen Autorisierungs-Tokens (Medien-Token) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen eines kurzen Autorisierungs-Tokens (Medien-Token) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen eines kurzen Autorisierungs-Tokens (Medien-Token) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Abmeldephase {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++Häufig gestellte Fragen zur Abmeldephase

##### &#x200B;1. Welche API-Migrationen auf hoher Ebene sind für die Abmeldephase erforderlich? {#logout-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in den folgenden Tabellen aufgeführt sind:

###### AccessEnabler JavaScript SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren des Abmeldevorgangs | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren des Abmeldevorgangs | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren des Abmeldevorgangs | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren des Abmeldevorgangs | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
