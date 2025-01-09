---
title: Häufig gestellte Fragen zur REST API V2
description: Häufig gestellte Fragen zur REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '6668'
ht-degree: 0%

---

# Häufig gestellte Fragen zur REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Dieses Dokument bietet allgemeine Antworten auf häufig gestellte Fragen zur Verwendung der Adobe Pass Authentication REST API V2.

Weitere Informationen zur REST API V2 insgesamt finden Sie in der Dokumentation [REST API V2 - Übersicht](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) .

>[!MORELIKETHIS]
>
> * [Häufig gestellte Fragen zur Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

## Allgemeine FAQs {#general-faqs}

Beginnen Sie mit diesem Abschnitt, wenn Sie an einem Programm arbeiten, das die REST-API V2 integrieren muss, unabhängig davon, ob es sich um ein neues oder ein bestehendes Programm handelt, das von [REST-API V1](#migration-rest-api-v1-to-rest-api-v2) oder [SDK migriert](#migration-sdk-to-rest-api-v2).

Weitere Informationen zu Migrationsdetails und -schritten finden Sie auch in den nächsten Abschnitten.

### Häufig gestellte Fragen zur Registrierungsphase {#registration-phase-faqs-general}

+++Häufig gestellte Fragen zur Registrierungsphase

Weitere Informationen finden Sie in der [Häufig gestellte Fragen zur Dynamic Client-Registrierung (DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

+++

### Häufig gestellte Fragen zur Konfigurationsphase {#configuration-phase-faqs-general}

+++Häufig gestellte Fragen zur Konfigurationsphase

#### 1. Was ist der Zweck der Konfigurationsphase? {#configuration-phase-faq1}

Die Konfigurationsphase dient dazu, der Client-Anwendung die Liste der MVPDs bereitzustellen, mit denen sie aktiv integriert ist, sowie die Konfigurationsdetails, die von der Adobe Pass-Authentifizierung für jede MVPD gespeichert wurden.

Die Konfigurationsphase dient als erforderlicher Schritt für die Authentifizierungsphase, wenn das Client-Programm den Benutzer bzw. die Benutzerin auffordern muss, seinen/ihren TV-Anbieter auszuwählen.

#### 2. Ist die Konfigurationsphase obligatorisch? {#configuration-phase-faq2}

Die Konfigurationsphase ist nicht obligatorisch. Die Client-Anwendung kann diese Phase in den folgenden Szenarien überspringen:

* Der Benutzer ist bereits authentifiziert.
* Dem Benutzer wird temporärer Zugriff über die einfache oder Werbe-TempPass-Funktion angeboten.
* Die Benutzerauthentifizierung ist abgelaufen, aber die Client-Anwendung hat die zuvor ausgewählte MVPD als benutzererlebnismotivierte Auswahl zwischengespeichert und fordert die Benutzenden lediglich auf zu bestätigen, dass sie weiterhin Abonnenten dieser MVPD sind.

#### 3. Was ist eine Konfiguration und wie lange ist sie gültig? {#configuration-phase-faq3}

Die Konfiguration ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration) definiert ist.

Die Konfiguration besteht aus einer Liste von MVPDs, die vom Konfigurationsendpunkt abgerufen werden können.

Die Client-Anwendung kann die Konfiguration verwenden, um eine UI-Komponente namens „Auswahl“ darzustellen, wenn Benutzende ihre MVPD auswählen müssen.

Die Client-Anwendung sollte die Konfiguration aktualisieren, bevor der Benutzer die Authentifizierungsphase durchläuft.

Weitere Informationen finden Sie in der Dokumentation [Konfiguration abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) .

#### 4. Kann die Client-Anwendung eine eigene Liste von MVPDs verwalten? {#configuration-phase-faq4}

Die Client-Anwendung kann ihre eigene Liste von MVPDs verwalten. Es wird jedoch empfohlen, die von der Adobe Pass-Authentifizierung bereitgestellte Konfiguration zu verwenden, um sicherzustellen, dass die Liste aktuell und korrekt ist.

Die Client-Anwendung erhält einen [Fehler](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) von der Adobe Pass-Authentifizierungs-REST-API V2, wenn der ausgewählte Benutzer in MVPD keine aktive Integration mit dem angegebenen [Dienstleister](rest-api-v2-glossary.md#service-provider) hat.

#### 5. Kann die Client-Anwendung die Liste der MVPDs filtern? {#configuration-phase-faq5}

Die Client-Anwendung kann die Liste der in der Konfigurationsantwort bereitgestellten MVPDs filtern, indem ein benutzerdefinierter Mechanismus implementiert wird, der auf eigener Geschäftslogik und Anforderungen wie dem Benutzerstandort oder dem Benutzerverlauf der vorherigen Auswahl basiert.

#### 6. Was passiert, wenn die Integration mit einer MVPD deaktiviert und als inaktiv markiert ist? {#configuration-phase-faq6}

Wenn die Integration mit einer MVPD deaktiviert und als inaktiv markiert ist, wird die MVPD aus der Liste der MVPDs entfernt, die in weiteren Konfigurationsantworten bereitgestellt werden, und es sind zwei wichtige Folgen zu berücksichtigen:

* Die nicht authentifizierten Benutzer dieser MVPD können die Authentifizierungsphase mit dieser MVPD nicht mehr abschließen.
* Die authentifizierten Benutzenden dieser MVPD können die Vorautorisierungs-, Autorisierungs- oder Abmeldephasen mit dieser MVPD nicht mehr abschließen.

Die Client-Anwendung erhält einen [Fehler](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) von der Adobe Pass-Authentifizierungs-REST-API V2, wenn der ausgewählte Benutzer in MVPD keine aktive Integration mehr mit dem angegebenen [Dienstleister](rest-api-v2-glossary.md#service-provider) hat.

#### 7. Was passiert, wenn die Integration mit MVPD wieder aktiviert und als aktiv markiert wird? {#configuration-phase-faq7}

Wenn die Integration mit einer MVPD wieder aktiviert und als aktiviert markiert ist, wird die MVPD wieder in die Liste der MVPDs aufgenommen, die in weiteren Konfigurationsantworten bereitgestellt werden, und es sind zwei wichtige Folgen zu berücksichtigen:

* Die nicht authentifizierten Benutzer dieser MVPD können die Authentifizierungsphase mithilfe dieser MVPD erneut abschließen.
* Die authentifizierten Benutzer dieser MVPD können die Vorautorisierungs-, Autorisierungs- oder Abmeldephasen mit dieser MVPD erneut abschließen.

#### 8. Wie kann ich die Integration mit MVPD aktivieren oder deaktivieren? {#configuration-phase-faq8}

Dieser Vorgang kann über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter ausgeführt werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Häufig gestellte Fragen zur Authentifizierungsphase {#authentication-phase-faqs-general}

+++Häufig gestellte Fragen zur Authentifizierungsphase

#### 1. Was ist der Zweck der Authentifizierungsphase? {#authentication-phase-faq1}

Der Zweck der Authentifizierungsphase besteht darin, der Client-Anwendung die Möglichkeit zu geben, die Identität des Benutzers zu überprüfen und Benutzermetadaten-Informationen abzurufen.

Die Authentifizierungsphase fungiert als erforderlicher Schritt für die Vorautorisierungsphase oder Autorisierungsphase, wenn die Client-Anwendung Inhalte wiedergeben muss.

#### 2. Wie kann die Client-Anwendung erkennen, ob der Benutzer bereits authentifiziert ist? {#authentication-phase-faq2}

Die Client-Anwendung kann einen der folgenden Endpunkte abfragen, die überprüfen können, ob ein Benutzer bereits authentifiziert ist, und Profilinformationen zurückgeben:

* [Profil-Endpunkt-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profile-Endpunkt für bestimmte MVPD-APIs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profile-Endpunkt für bestimmte (Authentifizierungs-)Code-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 3. Wie kann die Client-Anwendung die Metadateninformationen des Benutzers abrufen? {#authentication-phase-faq3}

Die Client-Anwendung kann einen der folgenden Endpunkte abfragen, die [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md) als Teil der Profilinformationen zurückgeben können:

* [Profil-Endpunkt-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profile-Endpunkt für bestimmte MVPD-APIs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profile-Endpunkt für bestimmte (Authentifizierungs-)Code-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 4. Was ist eine Authentifizierungssitzung und wie lange ist sie gültig? {#authentication-phase-faq4}

Die Authentifizierungssitzung ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session) definiert ist.

Die Authentifizierungssitzung speichert Informationen zum initiierten Authentifizierungsprozess, die vom Sitzungs-Endpunkt abgerufen werden können.

Die Authentifizierungssitzung ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt des Problems angegeben ist. Dies gibt an, wie lange der Benutzer bis zum Abschluss des Authentifizierungsprozesses muss, bevor der Fluss neu gestartet werden muss.

Die Client-Anwendung kann die Antwort der Authentifizierungssitzung verwenden, um zu erfahren, wie mit dem Authentifizierungsprozess fortgefahren werden soll. Beachten Sie, dass es Fälle gibt, in denen der Benutzer sich nicht authentifizieren muss, z. B. bei Bereitstellung eines temporären Zugriffs, eingeschränktem Zugriff oder wenn der Benutzer bereits authentifiziert ist.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Authentifizierungssitzung-API erstellen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Authentifizierungssitzung-API fortsetzen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundlegender Authentifizierungsfluss innerhalb der sekundären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 5. Was ist ein Authentifizierungs-Code und wie lange ist er gültig? {#authentication-phase-faq5}

Der Authentifizierungs-Code ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code) definiert ist.

Der Authentifizierungs-Code speichert einen eindeutigen Wert, der generiert wird, wenn ein Benutzer den Authentifizierungsprozess initiiert, und identifiziert die Authentifizierungssitzung eines Benutzers eindeutig, bis der Prozess abgeschlossen ist oder die Authentifizierungssitzung abläuft.

Der Authentifizierungs-Code ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Initiierung der Authentifizierungssitzung angegeben wird. Er gibt an, wie lange der Benutzer den Authentifizierungsprozess abschließen muss, bevor der Fluss neu gestartet werden muss.

Die Client-Anwendung kann den Authentifizierungs-Code verwenden, damit der Benutzer den Authentifizierungsprozess entweder auf demselben Gerät oder auf einem zweiten Gerät abschließen oder fortsetzen kann, da die Authentifizierungssitzung noch nicht abgelaufen ist.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Authentifizierungssitzung-API erstellen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Authentifizierungssitzung-API fortsetzen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundlegender Authentifizierungsfluss innerhalb der sekundären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 6. Wie kann die Client-Anwendung erkennen, ob der Benutzer einen gültigen Authentifizierungscode eingegeben hat und dass die Authentifizierungssitzung noch nicht abgelaufen ist? {#authentication-phase-faq6}

Die Client-Anwendung kann den Authentifizierungs-Code, den der Benutzer in eine sekundäre (Bildschirm-)Anwendung eingibt, überprüfen, indem sie eine Anfrage an den Sitzungs-Endpunkt sendet, der für das Abrufen der mit dem Authentifizierungs-Code verknüpften Authentifizierungssitzungsinformationen verantwortlich ist.

Die Client-Anwendung erhält einen [Fehler](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md), wenn der angegebene Authentifizierungscode falsch eingegeben wird oder wenn die Authentifizierungssitzung abgelaufen ist.

Weitere Informationen finden Sie in der Dokumentation [Authentifizierungssitzung abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) .

#### 7. Was ist ein Profil und wie lange ist es gültig? {#authentication-phase-faq7}

Das Profil ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile) definiert ist.

Das Profil speichert Informationen über die Authentifizierungsgültigkeit des Benutzers, Metadateninformationen und viele mehr, die vom Endpunkt „Profiles“ abgerufen werden können.

Die Client-Anwendung kann das Profil verwenden, um den Authentifizierungsstatus des Benutzers zu ermitteln, auf Benutzermetadaten-Informationen zuzugreifen oder die Authentifizierungsmethode zu finden.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Profil-Endpunkt-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profile-Endpunkt für bestimmte MVPD-APIs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profile-Endpunkt für bestimmte (Authentifizierungs-)Code-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Das Profil ist für einen begrenzten Zeitraum gültig, der bei der Abfrage angegeben wird. Dies gibt an, wie lange der Benutzer über eine gültige Authentifizierung verfügt, bevor er die Authentifizierungsphase erneut durchlaufen muss.

Dieser begrenzte Zeitraum, der als Authentifizierung (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) bezeichnet wird, kann über das Adobe Pass [TVE-Dashboard ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsbeauftragten angezeigt und geändert werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

+++

### Häufig gestellte Fragen zur Vorautorisierungsphase {#preauthorization-phase-faqs-general}

+++Häufig gestellte Fragen zur Vorautorisierungsphase

#### 1. Was ist der Zweck der Vorabgenehmigungsphase? {#preauthorization-phase-faq1}

Der Zweck der Vorautorisierungsphase besteht darin, der Client-Anwendung die Möglichkeit zu geben, eine Untergruppe von Ressourcen aus ihrem Katalog darzustellen, auf die der Benutzer zugreifen könnte.

Die Vorautorisierungsphase kann das Benutzererlebnis verbessern, wenn der Benutzer die Client-Anwendung zum ersten Mal öffnet oder zu einem neuen Abschnitt navigiert.

#### 2. Ist die Phase vor der Autorisierung obligatorisch? {#preauthorization-phase-faq2}

Die Vorautorisierungsphase ist nicht obligatorisch. Die Client-Anwendung kann diese Phase überspringen, wenn sie einen Katalog von Ressourcen präsentieren möchte, ohne sie zuerst auf der Grundlage der Benutzerberechtigung zu filtern.

#### 3. Was ist eine Entscheidung vor der Zulassung? {#preauthorization-phase-faq3}

Die Vorautorisierung ist ein Begriff, der in der [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization)-Dokumentation definiert ist, während der Entscheidungsbegriff auch im [Glossar) ](rest-api-v2-glossary.md#decision) ist.

Die Vorautorisierungsentscheidung speichert Informationen zum Abfrageergebnis des MVPD-Vorautorisierungsprozesses, die vom Endpunkt Decisions Preauthorize abgerufen werden können.

Die Client-Anwendung kann die Entscheidungen vor der Autorisierung verwenden, um eine Untergruppe von Ressourcen darzustellen, auf die der TV-Anbieter (informative) Entscheidungen dem Benutzer Zugriff gewähren würde.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Abrufen der Vorabautorisierungsentscheidungen-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Grundlegender Vorautorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4. Warum fehlt in der Vorabautorisierungsentscheidung ein Medien-Token? {#preauthorization-phase-faq4}

Der Vorautorisierungsentscheidung fehlt ein Medien-Token, da die Vorautorisierungsphase nicht zum Wiedergeben von Ressourcen verwendet werden darf, da dies der Zweck der Autorisierungsphase ist.

#### 5. Was ist eine Ressource und welche Formate werden unterstützt? {#preauthorization-phase-faq5}

Die Ressource ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) definiert ist.

Die Ressource ist eine eindeutige Kennung, die mit MVPDs vereinbart wird und mit einem Inhalt verknüpft ist, den die Client-Anwendung streamen könnte.

Die eindeutige Kennung der Ressource kann zwei Formate aufweisen:

* Ein einfaches Zeichenfolgenformat wie eine eindeutige Kennung für einen Kanal (eine Marke).
* Ein RSS-Format für Medien (MRSS), das zusätzliche Informationen wie Titel, Bewertungen und Metadaten zur elterlichen Kontrolle enthält.

Weitere Informationen finden Sie in der Dokumentation [Identifizieren geschützter Ressourcen](/help/authentication/integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md) .

#### 6. Für wie viele Ressourcen kann die Client-Anwendung gleichzeitig eine Vorabautorisierungsentscheidung erhalten? {#preauthorization-phase-faq6}

Die Client-Anwendung kann aufgrund von Bedingungen, die von MVPDs auferlegt werden, in einer einzelnen API-Anfrage eine Vorabautorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen erhalten, normalerweise bis zu 5.

Diese maximale Anzahl von Ressourcen kann angezeigt und geändert werden, nachdem eine Vereinbarung mit MVPDs über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter getroffen wurde.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### Häufig gestellte Fragen zur Genehmigungsphase {#authorization-phase-faqs-general}

+++Häufig gestellte Fragen zur Autorisierungsphase

#### 1. Was ist der Zweck der Genehmigungsphase? {#authorization-phase-faq1}

In der Autorisierungsphase soll der Client-Anwendung die Möglichkeit gegeben werden, die von den Benutzenden angeforderten Ressourcen abzuspielen, nachdem ihre Rechte bei der MVPD validiert wurden.

#### 2. Ist die Autorisierungsphase obligatorisch? {#authorization-phase-faq2}

Die Autorisierungsphase ist obligatorisch. Die Client-Anwendung kann diese Phase nicht überspringen, wenn sie die von der Benutzerin oder dem Benutzer angeforderten Ressourcen wiedergeben möchte, da sie bzw. er bei der MVPD überprüfen muss, ob die Benutzerin oder der Benutzer berechtigt ist, bevor der Stream freigegeben wird.

#### 3. Was ist eine Zulassungsentscheidung und wie lange ist sie gültig? {#authorization-phase-faq3}

Der Begriff Autorisierung ist in der [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization)-Dokumentation definiert, während der Begriff Entscheidung auch im [Glossar) ](rest-api-v2-glossary.md#decision) ist.

Die Autorisierungsentscheidung speichert Informationen zum Abfrageergebnis des MVPD-Autorisierungsprozesses, die vom Entscheidungsautorisierungs-Endpunkt abgerufen werden können.

Die Client-Anwendung kann die Autorisierungsentscheidung mit einem Medien-Token verwenden, um den Ressourcen-Stream abzuspielen, falls die Entscheidung des TV-Anbieters (autorisierend) dem Benutzer den Zugriff darauf ermöglichen würde.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Abrufen der Autorisierungsentscheidungen-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Die Autorisierungsentscheidung ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Ausgabe angegeben ist. Dabei wird die Zeit angegeben, die von der Adobe Pass-Authentifizierung zwischengespeichert wird, bevor die MVPD erneut abgefragt werden muss.

Dieser begrenzte Zeitraum, der als Autorisierung (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) bezeichnet wird, kann über das Adobe Pass [TVE-Dashboard ](rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsbeauftragten angezeigt und geändert werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### 4. Was ist ein Medien-Token und wie lange ist es gültig? {#authorization-phase-faq4}

Das Medien-Token ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token) definiert ist.

Das Medien-Token besteht aus einer signierten Zeichenfolge, die als Klartext gesendet wird und vom Entscheidungsautorisierungs-Endpunkt abgerufen werden kann.

Weitere Informationen finden Sie in der Dokumentation [Integrieren des Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md).

Das Medien-Token ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Ausgabe angegeben ist. Es gibt die Zeitdauer an, die von der Client-Anwendung verwendet werden muss, bevor der Entscheidungs-Autorisierungs-Endpunkt erneut abgefragt werden muss.

Die Client-Anwendung kann das Medien-Token verwenden, um einen Ressourcen-Stream abzuspielen, falls die Entscheidung des TV-Anbieters (autoritär) dem Benutzer den Zugriff darauf ermöglichen würde.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Abrufen der Autorisierungsentscheidungen-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 5. Was ist eine Ressource und welche Formate werden unterstützt? {#authorization-phase-faq5}

Die Ressource ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) definiert ist.

Die Ressource ist eine eindeutige Kennung, die mit MVPDs vereinbart wird und mit einem Inhalt verknüpft ist, den die Client-Anwendung streamen könnte.

Die eindeutige Kennung der Ressource kann zwei Formate aufweisen:

* Ein einfaches Zeichenfolgenformat wie eine eindeutige Kennung für einen Kanal (eine Marke).
* Ein RSS-Format für Medien (MRSS), das zusätzliche Informationen wie Titel, Bewertungen und Metadaten zur elterlichen Kontrolle enthält.

Weitere Informationen finden Sie in der Dokumentation [Identifizieren geschützter Ressourcen](/help/authentication/integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md) .

#### 6. Für wie viele Ressourcen kann die Client-Anwendung gleichzeitig eine Autorisierungsentscheidung erhalten? {#authorization-phase-faq6}

Die Client-Anwendung kann in einer einzelnen API-Anfrage, in der Regel bis zu 1, eine Autorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen erhalten. Dies ist auf die von MVPDs auferlegten Bedingungen zurückzuführen.

+++

### Häufig gestellte Fragen zur Abmeldephase {#logout-phase-faqs-general}

+++Häufig gestellte Fragen zur Abmeldephase

#### 1. Was ist der Zweck der Abmeldephase? {#logout-phase-faq1}

Die Abmeldephase soll der Client-Anwendung die Möglichkeit geben, das authentifizierte Benutzerprofil innerhalb der Adobe Pass-Authentifizierung auf Benutzeranfrage zu beenden.

+++

### Häufig gestellte Fragen zu Kopfzeilen {#headers-faqs-general}

+++Häufig gestellte Fragen zu Kopfzeilen

#### 1. Wie wird der Wert für die Autorisierungs-Kopfzeile berechnet? {#headers-faq1}

Der [Autorisierungs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)-Anfrage-Header enthält das `Bearer` Zugriffstoken, das von der Client-Anwendung für den Zugriff auf durch Adobe Pass geschützte APIs benötigt wird.

Der Wert der Autorisierungs-Kopfzeile muss während der Registrierungsphase von der Adobe Pass-Authentifizierung abgerufen werden.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Abrufen von Client-Anmeldeinformationen API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Zugriffstoken-API abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Wenn die Client-Anwendung von REST API V1 zu REST API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode verwenden, um das `Bearer` Zugriffstoken wie zuvor abzurufen.

#### 2. Wie wird der Wert für die Kopfzeile „AP-Device-Identifier“ berechnet? {#headers-faq2}

Der [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)-Anforderungsheader enthält die Streaming-Gerätekennung, wie sie von der Client-Anwendung erstellt wurde.

Die Kopfzeilendokumentation [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) enthält einige Beispiele für die Berechnung des Werts für verschiedene Plattformen, aber die Client-Anwendung kann basierend auf ihrer eigenen Geschäftslogik und ihren Anforderungen eine andere Methode verwenden.

Wenn die Client-Anwendung von REST API V1 zu REST API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode zur Berechnung der Geräte-ID wie zuvor verwenden.

#### 3. Wie wird der Wert für den X-Device-Info-Header berechnet? {#headers-faq3}

Der [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)-Anfrage-Header enthält die Client-Informationen (Gerät, Verbindung und Anwendung), die sich auf das eigentliche Streaming-Gerät beziehen.

Die Kopfzeilendokumentation [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) enthält einige Beispiele zur Berechnung des Werts für verschiedene Plattformen, aber die Client-Anwendung kann basierend auf ihrer eigenen Geschäftslogik und ihren Anforderungen eine andere Methode verwenden.

Wenn die Client-Anwendung von REST API V1 zu REST API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode zur Berechnung der Geräteinformationen wie zuvor verwenden.

+++

## Häufig gestellte Fragen zur Migration {#migration-faqs}

Fahren Sie mit diesem Abschnitt fort, wenn Sie an einem Programm arbeiten, das ein vorhandenes Programm zu REST API V2 migrieren muss.

### Häufig gestellte Fragen zur allgemeinen Migration {#general-migration-faqs}

+++Häufig gestellte Fragen zur allgemeinen Migration

#### 1. Muss ich eine neue Client-Anwendung einführen, die zu REST API V2 migriert wurde, und zwar für alle Benutzer gleichzeitig? {#migration-faq1}

Anzahl

Die Client-Anwendung ist nicht erforderlich, um eine neue Version einzuführen, die die REST-API V2 gleichzeitig für alle Benutzer integriert.

Die Adobe Pass-Authentifizierung unterstützt bis Ende 2025 weiterhin ältere Client-Anwendungsversionen, die die REST-API V1 oder SDK integrieren.

#### 2. Muss ich eine neue Client-Anwendung einführen, die zur REST-API V2 über alle APIs und Flüsse hinweg gleichzeitig migriert wurde? {#migration-faq2}

Ja.

Die Client-Anwendung ist erforderlich, um eine neue Version einzuführen, die die REST-API V2 über alle Adobe Pass-Authentifizierungs-APIs und -Flüsse hinweg gleichzeitig integriert.

Im Falle des Flusses „Authentifizierung auf dem zweiten Bildschirm“ ist die Client-Anwendung erforderlich, um eine neue Version einzuführen, die die REST-API V2 für die [primären](rest-api-v2-glossary.md#primary-application) und [sekundären](rest-api-v2-glossary.md#secondary-application)-Anwendungen gleichzeitig integriert.

Die Adobe Pass-Authentifizierung unterstützt keine „hybriden“ Implementierungen, die sowohl die REST-API V2 als auch die REST-API V1/SDK zwischen APIs und Flüssen integrieren.

#### 3. Wird die Benutzerauthentifizierung beim Aktualisieren auf eine neue Client-Anwendung beibehalten, die zur REST API V2 migriert wurde? {#migration-faq3}

Anzahl

Die Benutzerauthentifizierung, die in älteren Client-Anwendungsversionen mit der REST-API V1 oder SDK erhalten wurde, wird nicht beibehalten.

Daher muss sich der Benutzer innerhalb der neuen Client-Anwendung, die zur REST-API V2 migriert wurde, erneut authentifizieren.

#### 4. Kann die Client-Anwendung die vorhandenen registrierten Anwendungen (Software-Anweisungen) nutzen? {#migration-faq4}

Die Client-Anwendung kann die vorhandenen registrierten Anwendungen nicht wiederverwenden (Software-Anweisungen). Daher muss sie neue registrierte Anwendungen (Software-Anweisungen) generieren und herunterladen, die speziell für die Nutzung der REST-API V2 vorgesehen sind.

Dieser Vorgang kann über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter ausgeführt werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Channels User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) oder [TVE Dashboard Programmers User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

Zunächst müssen Sie einen Adobe Pass-Authentifizierungsbeauftragten bitten, die Verwendung der REST-API V2 für Ihre neu registrierten Anwendungen (Software-Anweisungen) zu aktivieren, bis das Adobe Pass [TVE-Dashboard](rest-api-v2-glossary.md#tve-dashboard) aktualisiert wird, um die Selbstverwaltung dieses Vorgangs zu ermöglichen.

Um die registrierten Anwendungen (Software-Anweisungen) zu unterscheiden, die in Client-Anwendungen verwendet werden, die REST API V2 nutzen, müssen Sie dem registrierten Anwendungsnamen ein spezifisches Suffix hinzufügen, z. B. „RESTV2“.

#### 5. Kann die Client-Anwendung die vorhandenen benutzerdefinierten Schemata verwenden? {#migration-faq5}

Die Client-Anwendung kann die vorhandenen benutzerdefinierten Schemata wiederverwenden, die über das Adobe Pass-[TVE-Dashboard) generiert ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard).

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Channels User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes) oder [TVE Dashboard Programmers User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes).

#### 6. Sind die erweiterten Fehler-Codes in der REST-API V2 standardmäßig aktiviert? {#migration-faq6}

Ja.

Die Client-Programme, die die REST-API V2 integrieren, profitieren von der erweiterten Fehlercode-Funktion, die standardmäßig aktiviert ist.

Weitere Informationen finden Sie in der Dokumentation [Erweiterte ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)).

+++

### Migration von der REST API V1 zur REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Fahren Sie mit diesem Unterabschnitt fort, wenn Sie an einem Programm arbeiten, das von REST API V1 zu REST API V2 migriert werden muss.

#### Häufig gestellte Fragen zur Registrierungsphase {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Registrierungsphase

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Registrierungsphase erforderlich? {#registration-phase-v1-to-v2-faq1}

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

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Konfigurationsphase erforderlich? {#configuration-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Häufig gestellte Fragen zur Authentifizierungsphase {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Authentifizierungsphase

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Authentifizierungsphase erforderlich? {#authentication-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungs-Code (Authentifizierungs-Code) abrufen | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registrierungs-Code prüfen (Authentifizierungs-Code) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Authentifizierung (MVPD) auf dem zweiten Bildschirm fortsetzen (Anwendung) | [GET <br/> /api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD)-Authentifizierung initiieren | [GET <br/> /api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [GET <br/> /api/v1/checkauthn (erster Bildschirm)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (zweiter Bildschirm)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen des Benutzerauthentifizierungs-Tokens (Profil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Vorautorisierungsphase {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Vorautorisierungsphase

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Vorautorisierungsphase erforderlich? {#preauthorization-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen vorab autorisierter Ressourcen (Entscheidungen vor der Autorisierung) | [GET <br/> /api/v1/preauthorize (erster Bildschirm)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (zweiter Bildschirm)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Genehmigungsphase {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Autorisierungsphase

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Autorisierungsphase erforderlich? {#authorization-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD)-Autorisierung initiieren | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Autorisierungs-Token abrufen (Autorisierungsentscheidung) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Abrufen eines kurzen Autorisierungs-Tokens (Medien-Token) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Client-Anwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>(MVPD)-Autorisierung initiieren</li><li>Autorisierungsentscheidung abrufen</li><li>Abrufen eines kurzen Medien-Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Abmeldephase {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Häufig gestellte Fragen zur Abmeldephase

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Abmeldephase erforderlich? {#logout-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST-API V1 | REST-API V2 | Beobachtungen |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren des Abmeldevorgangs | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migration von SDK zur REST-API V2 {#migration-sdk-to-rest-api-v2}

Fahren Sie mit diesem Unterabschnitt fort, wenn Sie an einem Programm arbeiten, das von SDK zu REST API V2 migriert werden muss.

#### Häufig gestellte Fragen zur Registrierungsphase {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Häufig gestellte Fragen zur Registrierungsphase

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Registrierungsphase erforderlich? {#registration-phase-sdk-to-v2-faq1}

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

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Konfigurationsphase erforderlich? {#configuration-phase-sdk-to-v2-faq1}

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

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Authentifizierungsphase erforderlich? {#authentication-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind wichtige Änderungen zu berücksichtigen, die in den folgenden Tabellen aufgeführt sind:

###### AccessEnabler JavaScript SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungs-Code (Authentifizierungs-Code) abrufen | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registrierungs-Code prüfen (Authentifizierungs-Code) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Authentifizierung (MVPD) auf dem zweiten Bildschirm fortsetzen (Anwendung) | [GET <br/> /api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Umfang | SDK | REST-API V2 | Beobachtungen |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungs-Code (Authentifizierungs-Code) abrufen | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Registrierungs-Code prüfen (Authentifizierungs-Code) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Authentifizierung (MVPD) auf dem zweiten Bildschirm fortsetzen (Anwendung) | [GET <br/> /api/v1/Authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| (MVPD)-Authentifizierung initiieren | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/Authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Standardauthentifizierungsfluss, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Standardauthentifizierungsfluss, der innerhalb der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Verwenden Sie eine der folgenden Möglichkeiten: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Client-Anwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Abrufen von Benutzerprofilen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Fluss der grundlegenden Profile, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Häufig gestellte Fragen zur Vorautorisierungsphase {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Häufig gestellte Fragen zur Vorautorisierungsphase

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Vorautorisierungsphase erforderlich? {#preauthorization-phase-sdk-to-v2-faq1}

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

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Autorisierungsphase erforderlich? {#authorization-phase-sdk-to-v2-faq1}

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

##### 1. Welche API-Migrationen auf hoher Ebene sind für die Abmeldephase erforderlich? {#logout-phase-sdk-to-v2-faq1}

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
