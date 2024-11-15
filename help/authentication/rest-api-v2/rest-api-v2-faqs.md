---
title: Häufig gestellte Fragen zur REST API V2
description: Häufig gestellte Fragen zur REST API V2
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# Häufig gestellte Fragen zur REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Dieses Dokument bietet einen umfassenden Überblick über die Antworten auf häufig gestellte Fragen zur Übernahme der Adobe Pass Authentication REST API V2.

Weitere Informationen zur REST API V2 finden Sie in der Dokumentation zur [REST API V2 Overview](./rest-api-v2-overview.md) .

## Allgemeine häufig gestellte Fragen {#general-faqs}

Beginnen Sie mit diesem Abschnitt, wenn Sie an einer Anwendung arbeiten, die die REST-API V2 integrieren muss, unabhängig davon, ob es sich um eine neue oder eine vorhandene Anwendung handelt, die von der [REST-API V1](#migrate-rest-api-v1-to-rest-api-v2) oder dem [SDK](#migrate-sdk-to-rest-api-v2) migriert wird.

Weitere Informationen zu Migrationsdetails und Schritten finden Sie auch in den nächsten Abschnitten.

Häufig gestellte Fragen zur ++ Registrierungsphase

### 1. Welchen Zweck hat die Registrierungsphase? {#registration-phase-faq1}

Die Registrierungsphase dient der Registrierung der Clientanwendung für die Adobe Pass-Authentifizierung über den Prozess [Dynamische Client-Registrierung (DCR)](./rest-api-v2-glossary.md#dcr) .

Für den Dynamische Client-Registrierung (DCR)-Prozess muss die Client-Anwendung ein Paar Client-Anmeldeinformationen abrufen und ein Zugriffstoken als Endziel der Registrierungsphase abrufen.

Weitere Informationen finden Sie in der Dokumentation [Übersicht über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md) .

### 2. Ist die Registrierungsphase obligatorisch? {#registration-phase-faq2}

Die Registrierungsphase ist obligatorisch. Die Client-Anwendung kann diese Phase jedoch überspringen, wenn sie über ein zwischengespeichertes Paar Client-Anmeldeinformationen und ein Zugriffstoken verfügt, die noch gültig sind.

### 3. Was ist eine Software-Aussage und wie lange ist sie gültig? {#registration-phase-faq3}

Die Softwareanweisung ist ein in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#software-statement) definierter Begriff.

Die Softwareanweisung besteht aus einem JSON-Web-Token (JWT), das von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen tätigen Adobe Pass-Authentifizierungsbeauftragten vom Adobe Pass [TVE-Dashboard](./rest-api-v2-glossary.md#tve-dashboard) generiert und heruntergeladen werden kann.

Die Softwareanweisung ist für einen unbegrenzten Zeitraum gültig. Sie können jedoch einen Adobe Pass-Authentifizierungsbeauftragten bitten, sie jederzeit zu widerrufen.

Die Clientanwendung muss die Softwareanweisung speichern und verwenden, wenn Client-Anmeldeinformationen abgerufen werden müssen.

Weitere Informationen finden Sie in der Dokumentation zur [Übersicht über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md) .

### 4. Wie generiere und lade ich eine Software-Anweisung herunter? {#registration-phase-faq4}

Dieser Vorgang kann über das Adobe Pass [TVE-Dashboard](./rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder von einem für Sie zuständigen Adobe Pass-Authentifizierungsbeauftragten ausgeführt werden.

Weitere Informationen finden Sie in der Dokumentation zum [TVE Dashboard Channels User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) oder dem [TVE Dashboard-Programmierhandbuch](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) .

### 5. Was passiert, wenn eine Softwareanweisung widerrufen wird? {#registration-phase-faq5}

Wenn die Softwareanweisung widerrufen wird, muss Folgendes berücksichtigt werden:

* Die Clientanwendungen, die die gesperrte Softwareanweisung verwenden, können die Flüsse [Berechtigung](./rest-api-v2-glossary.md#entitlement) nicht mehr durchlaufen. Das bedeutet, dass Benutzer von der Wiedergabe von Inhalten ausgeschlossen werden.

### 6. Was sind Client-Zugangsdaten und wie lange sind sie gültig? {#registration-phase-faq6}

Die Client-Anmeldeinformationen sind ein in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#client-credentials) definierter Begriff.

Die Client-Anmeldeinformationen bestehen aus einer Client-Kennung und einem Client-Geheimpaar, die vom Client-Register-Endpunkt abgerufen werden können.

Die Client-Anmeldeinformationen sind für einen unbegrenzten Zeitraum gültig.

Die Clientanwendung muss die Client-Anmeldeinformationen unbegrenzt speichern und verwenden, wenn ein Zugriffstoken abgerufen werden muss.

Weitere Informationen finden Sie in der Dokumentation zum [Abrufen von Client-Anmeldeinformationen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) .

### 7. Wie werden Client-Anmeldeinformationen verwaltet? {#registration-phase-faq7}

Es wird empfohlen, dass die Clientanwendung für jede Benutzeranwendungsinstanz ein eindeutiges Paar von Client-Anmeldeinformationen verwaltet, sowohl bei Client-zu-Server- als auch bei Server-zu-Server-Integrationen mit der Adobe Pass-Authentifizierung.

Die Clientanwendung muss die Client-Anmeldeinformationen unbegrenzt speichern und verwenden, wenn ein Zugriffstoken abgerufen werden muss.

### 8. Was passiert, wenn zwischengespeicherte Client-Anmeldeinformationen verloren gehen? {#registration-phase-faq8}

Wenn zwischengespeicherte Client-Anmeldeinformationen verloren gehen, sind drei wichtige Konsequenzen zu beachten:

* Die Clientanwendung muss ein neues Paar Client-Anmeldeinformationen abrufen.
* Die Clientanwendung muss ein neues Zugriffstoken mit dem neuen Client-Anmeldedatenpaar abrufen.
* Die Clientanwendung muss den Benutzer auffordern, sich erneut zu authentifizieren, da die Clientanwendung den Zugriff auf die authentifizierten Profile verliert, die zuvor abgerufen wurden.

### 9. Was ist ein Zugriffstoken und wie lange ist es gültig? {#registration-phase-faq9}

Das Zugriffstoken ist ein in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#access-token) definierter Begriff.

Das Zugriffstoken besteht aus einem [Bearer-Token](./appendix/headers/rest-api-v2-appendix-headers-authorization.md) , das vom Client-Token-Endpunkt abgerufen werden kann.

Das Zugriffstoken ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt des Problems angegeben wird.

Die Clientanwendung muss das Zugriffstoken speichern und verwenden, bis es bei der Zielgruppenbestimmung der REST-API V2 abläuft.

Die Clientanwendung muss vor Ablauf des aktuellen Zugriffstokens ein neues Zugriffstoken abrufen, um nicht autorisierte Anfragen zu verhindern.

Weitere Informationen finden Sie in der Dokumentation zum [Zugriffstoken abrufen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) .

### 10. Wie kann die Client-Anwendung ein Zugriffstoken aktualisieren? {#registration-phase-faq10}

Die Clientanwendung muss ein Zugriffstoken auf dieselbe Weise aktualisieren wie ein neues Zugriffstoken abgerufen werden, jedoch mit zwischengespeicherten Client-Anmeldeinformationen.

Die Clientanwendung darf sich nicht erneut registrieren, um ein Zugriffstoken zu aktualisieren, sondern muss stattdessen die gespeicherten Client-Anmeldeinformationen verwenden. Andernfalls müssen sich Benutzer erneut authentifizieren.

Weitere Informationen finden Sie in der Dokumentation zum [Zugriffstoken abrufen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) .

+++

Häufig gestellte Fragen zur ++- Konfigurationsphase

### 1. Welchen Zweck hat die Konfigurationsphase? {#configuration-phase-faq1}

Die Konfigurationsphase dient dazu, der Clientanwendung die Liste der MVPDs bereitzustellen, mit denen sie aktiv integriert ist, sowie Konfigurationsdetails bereitzustellen, die von der Adobe Pass-Authentifizierung für jeden MVPD gespeichert werden.

Die Konfigurationsphase dient als Voraussetzung für die Authentifizierungsphase, wenn die Client-Anwendung den Benutzer auffordern muss, seinen TV-Anbieter auszuwählen.

### 2. Ist die Konfigurationsphase obligatorisch? {#configuration-phase-faq2}

Die Konfigurationsphase ist nicht obligatorisch. Die Client-Anwendung kann diese Phase in den folgenden Szenarien überspringen:

* Der Benutzer ist bereits authentifiziert.
* Dem Benutzer wird über eine einfache oder Werbe-TempPass-Funktion vorübergehender Zugriff angeboten.
* Die Benutzerauthentifizierung ist abgelaufen, aber die Client-Anwendung hat das zuvor ausgewählte MVPD als Benutzererlebnis einer begründeten Entscheidung zwischengespeichert und fordert den Benutzer lediglich auf, zu bestätigen, dass er immer noch Abonnent dieses MVPD ist.

### 3. Was ist eine Konfiguration und wie lange ist sie gültig? {#configuration-phase-faq3}

Die Konfiguration ist ein in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#configuration) definierter Begriff.

Die Konfiguration besteht aus einer Liste von MVPDs, die vom Configuration-Endpunkt abgerufen werden können.

Die Clientanwendung kann die Konfiguration verwenden, um eine UI-Komponente namens &quot;Picker&quot;darzustellen, wenn der Benutzer aufgefordert wird, sein MVPD auszuwählen.

Die Clientanwendung sollte die Konfiguration aktualisieren, bevor der Benutzer die Authentifizierungsphase durchläuft.

Weitere Informationen finden Sie in der Dokumentation zum [Abrufen der Konfiguration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) .

### 4. Kann die Client-Anwendung ihre eigene Liste von MVPDs verwalten? {#configuration-phase-faq4}

Die Clientanwendung kann ihre eigene Liste von MVPDs verwalten. Es wird jedoch empfohlen, die von der Adobe Pass-Authentifizierung bereitgestellte Konfiguration zu verwenden, um sicherzustellen, dass die Liste aktuell und korrekt ist.

Die Clientanwendung erhält einen [Fehler](/help/authentication/enhanced-error-codes.md) von der Adobe Pass Authentication REST API V2, wenn der ausgewählte Benutzer MVPD nicht aktiv mit dem angegebenen [Dienstleister](./rest-api-v2-glossary.md#service-provider) integriert hat.

### 5. Kann die Client-Anwendung die Liste der MVPDs filtern? {#configuration-phase-faq5}

Die Client-Anwendung kann die Liste der in der Konfigurationsantwort bereitgestellten MVPDs filtern, indem sie einen benutzerdefinierten Mechanismus implementiert, der auf einer eigenen Geschäftslogik und Anforderungen wie dem Standort des Benutzers oder dem Benutzerverlauf der vorherigen Auswahl basiert.

### 6. Was passiert, wenn die Integration mit einem MVPD deaktiviert und als inaktiv markiert ist? {#configuration-phase-faq6}

Wenn die Integration mit einem MVPD deaktiviert und als inaktiv markiert ist, wird der MVPD aus der Liste der MVPDs entfernt, die in weiteren Konfigurationsantworten bereitgestellt werden, und es gibt zwei wichtige Konsequenzen, die zu berücksichtigen sind:

* Die nicht authentifizierten Benutzer dieses MVPD können die Authentifizierungsphase nicht mehr mit diesem MVPD abschließen.
* Die authentifizierten Benutzer dieses MVPD können die Vorautorisierungs-, Autorisierungs- oder Abmeldephasen mit diesem MVPD nicht mehr abschließen.

Die Clientanwendung erhält einen [Fehler](/help/authentication/enhanced-error-codes.md) von der Adobe Pass Authentication REST API V2, wenn der ausgewählte Benutzer MVPD nicht mehr aktiv mit dem angegebenen [Dienstleister](./rest-api-v2-glossary.md#service-provider) integriert hat.

### 7. Was passiert, wenn die Integration mit einem MVPD wieder aktiviert und als aktiv markiert wird? {#configuration-phase-faq7}

Wenn die Integration mit einem MVPD wieder aktiviert und als aktiv markiert wird, wird der MVPD wieder in die Liste der MVPDs aufgenommen, die in weiteren Konfigurationsantworten bereitgestellt werden. Es gibt zwei wichtige Konsequenzen, die zu beachten sind:

* Die nicht authentifizierten Benutzer dieses MVPD können die Authentifizierungsphase mit diesem MVPD erneut abschließen.
* Die authentifizierten Benutzer dieses MVPD können die Vorautorisierungs-, Autorisierungs- oder Abmeldephasen mit diesem MVPD erneut abschließen.

### 8. Wie kann ich die Integration mit einem MVPD aktivieren oder deaktivieren? {#configuration-phase-faq8}

Dieser Vorgang kann über das Adobe Pass [TVE-Dashboard](./rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder von einem für Sie zuständigen Adobe Pass-Authentifizierungsbeauftragten ausgeführt werden.

Weitere Informationen finden Sie im [TVE Dashboard Integrations User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#disable-integration) -Handbuch.

+++

Häufig gestellte Fragen zur ++- Authentifizierungsphase

### 1. Welchen Zweck hat die Authentifizierungsphase? {#authentication-phase-faq1}

In der Authentifizierungsphase soll die Client-Anwendung die Möglichkeit erhalten, die Identität des Benutzers zu überprüfen und Metadateninformationen von Benutzern zu erhalten.

Die Authentifizierungsphase dient als Voraussetzung für die Phase der Vorabgenehmigung oder der Autorisierung, wenn die Clientanwendung Inhalte abspielen muss.

### 2. Wie kann die Client-Anwendung erkennen, ob der Benutzer bereits authentifiziert ist? {#authentication-phase-faq2}

Die Clientanwendung kann einen der folgenden Endpunkte abfragen, mit denen überprüft werden kann, ob ein Benutzer bereits authentifiziert ist, und Profilinformationen zurückgeben:

* [Profil-Endpunkt-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profil-Endpunkt für bestimmte MVPD-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profil-Endpunkt für spezifische (Authentifizierungs-)Code-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Durchfluss von Standardprofilen innerhalb der primären Anwendung](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Durchgang der einfachen Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3. Wie kann die Client-Anwendung die Metadateninformationen des Benutzers abrufen? {#authentication-phase-faq3}

Die Clientanwendung kann einen der folgenden Endpunkte abfragen, mit denen [Benutzermetadaten](/help/authentication/user-metadata-feature.md) als Teil der Profilinformationen zurückgegeben werden können:

* [Profil-Endpunkt-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profil-Endpunkt für bestimmte MVPD-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profil-Endpunkt für spezifische (Authentifizierungs-)Code-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Durchfluss von Standardprofilen innerhalb der primären Anwendung](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Durchgang der einfachen Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4. Was ist eine Authentifizierungssitzung und wie lange ist sie gültig? {#authentication-phase-faq4}

Die Authentifizierungssitzung ist ein Begriff, der in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#session) definiert ist.

Die Authentifizierungssitzung speichert Informationen über den initiierten Authentifizierungsprozess, die vom Sitzungsendpunkt abgerufen werden können.

Die Authentifizierungssitzung ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt des Problems festgelegt wird. Dies gibt an, wie lange der Benutzer den Authentifizierungsprozess abschließen muss, bevor er den Ablauf neu starten muss.

Die Clientanwendung kann die Authentifizierungssitzungsantwort verwenden, um zu erfahren, wie der Authentifizierungsprozess fortgesetzt werden soll. Beachten Sie, dass es Fälle gibt, in denen der Benutzer nicht authentifiziert werden muss, z. B. die Bereitstellung von temporärem Zugriff, eingeschränktem Zugriff oder wenn der Benutzer bereits authentifiziert ist.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Authentifizierungssitzungs-API erstellen](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Fortsetzen der Authentifizierungssitzungs-API](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundlegender Authentifizierungsfluss, der in der sekundären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5. Was ist ein Authentifizierungscode und wie lange ist er gültig? {#authentication-phase-faq5}

Der Authentifizierungscode ist ein Begriff, der in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#code) definiert ist.

Der Authentifizierungscode speichert einen eindeutigen Wert, der generiert wird, wenn ein Benutzer den Authentifizierungsprozess startet, und identifiziert eindeutig die Authentifizierungssitzung eines Benutzers, bis der Prozess abgeschlossen ist oder die Authentifizierungssitzung abläuft.

Der Authentifizierungscode gilt für einen begrenzten und kurzen Zeitraum, der zum Zeitpunkt der Initiierung der Authentifizierungssitzung festgelegt wird. Er gibt an, wie viel Zeit der Benutzer zum Abschluss des Authentifizierungsprozesses benötigt, bevor er den Ablauf neu starten muss.

Die Clientanwendung kann den Authentifizierungscode verwenden, um dem Benutzer zu ermöglichen, den Authentifizierungsprozess entweder auf demselben Gerät oder auf einem zweiten Gerät abzuschließen oder fortzusetzen, da die Authentifizierungssitzung nicht abgelaufen ist.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Authentifizierungssitzungs-API erstellen](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Fortsetzen der Authentifizierungssitzungs-API](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundlegender Authentifizierungsfluss, der in der sekundären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6. Wie kann die Client-Anwendung feststellen, ob der Benutzer einen gültigen Authentifizierungscode eingegeben hat und dass die Authentifizierungssitzung noch nicht abgelaufen ist? {#authentication-phase-faq6}

Die Clientanwendung kann den Authentifizierungscode überprüfen, der vom Benutzer in einer sekundären (Bildschirm-)Anwendung eingegeben wird, indem sie eine Anfrage an den Sitzungsendpunkt sendet, der für das Abrufen der mit dem Authentifizierungscode verknüpften Sitzungsinformationen zuständig ist.

Die Clientanwendung erhält einen [Fehler](/help/authentication/enhanced-error-codes.md) , wenn der angegebene Authentifizierungscode falsch eingegeben wurde oder die Authentifizierungssitzung abgelaufen wäre.

Weitere Informationen finden Sie in der Dokumentation zum [Abrufen einer Authentifizierungssitzung](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) .

### 7. Was ist ein Profil und wie lange ist es gültig? {#authentication-phase-faq7}

Das Profil ist ein in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#profile) definierter Begriff.

Das Profil speichert Informationen über die Authentifizierungsvalidität des Benutzers, Metadateninformationen und vieles mehr, die vom Endpunkt Profile abgerufen werden können.

Die Clientanwendung kann das Profil verwenden, um den Authentifizierungsstatus des Benutzers zu erfahren, auf Benutzermetadaten-Informationen zuzugreifen oder die Authentifizierungsmethode zu finden.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Profil-Endpunkt-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profil-Endpunkt für bestimmte MVPD-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profil-Endpunkt für spezifische (Authentifizierungs-)Code-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Durchfluss von Standardprofilen innerhalb der primären Anwendung](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Durchgang der einfachen Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Das Profil ist für einen begrenzten Zeitraum gültig, der bei der Abfrage angegeben wird. Dies gibt an, wie lange der Benutzer über eine gültige Authentifizierung verfügt, bevor er die Authentifizierungsphase erneut durchlaufen muss.

Dieser begrenzte Zeitraum, der als Authentifizierung (authN) [TTL](./rest-api-v2-glossary.md#ttl) bezeichnet wird, kann über das Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder von einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsbeauftragten angezeigt und geändert werden.

Weitere Informationen finden Sie im [TVE Dashboard Integrations User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) -Handbuch.

+++

+++Häufig gestellte Fragen zur Vorautorisierungsphase

### 1. Welchen Zweck hat die Phase der Vorabgenehmigung? {#preauthorization-phase-faq1}

In der Phase der Vorabgenehmigung soll der Clientanwendung die Möglichkeit gegeben werden, eine Untergruppe von Ressourcen aus ihrem Katalog zu präsentieren, auf die der Benutzer Zugriff haben könnte.

Die Phase der Vorabautorisierung kann das Benutzererlebnis verbessern, wenn der Benutzer die Clientanwendung zum ersten Mal öffnet oder zu einem neuen Abschnitt navigiert.

### 2. Ist die Phase der Vorabgenehmigung obligatorisch? {#preauthorization-phase-faq2}

Die Phase der Vorabautorisierung ist nicht obligatorisch. Die Clientanwendung kann diese Phase überspringen, wenn sie einen Ressourcenkatalog präsentieren möchte, ohne ihn zuerst basierend auf der Berechtigung des Benutzers zu filtern.

### 3. Was ist eine Vorabgenehmigung? {#preauthorization-phase-faq3}

Die Vorabautorisierung ist ein Begriff, der in der Dokumentation [Glossar](./rest-api-v2-glossary.md#preauthorization) definiert ist, während der Entscheidungsbegriff auch im [Glossar](./rest-api-v2-glossary.md#decision) zu finden ist.

Die Vorautorisierungsentscheidung speichert Informationen über das Ergebnis der Anfrage zum MVPD-Vorabgenehmigungsverfahren, die vom Entscheidungsvorautorisierungsendpunkt abgerufen werden können.

Die Client-Anwendung kann die Entscheidungen zur Vorabgenehmigung verwenden, um eine Untergruppe von Ressourcen darzustellen, auf die die Entscheidung des Fernsehveranstalters (informativ) den Zugriff des Benutzers ermöglicht.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [API für Vorabentscheidungen abrufen](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Grundlegender Ablauf der Vorautorisierung innerhalb der Hauptanwendung](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4. Warum fehlt in der Vorautorisierungsentscheidung ein Medien-Token? {#preauthorization-phase-faq4}

Der Entscheidung über die Vorabgenehmigung fehlt ein Medien-Token, da die Phase der Vorabgenehmigung nicht dazu verwendet werden darf, Ressourcen abzuspielen, da dies der Zweck der Genehmigungsphase ist.

### 5. Was ist eine Ressource und welche Formate werden unterstützt? {#preauthorization-phase-faq5}

Die Ressource ist ein in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#resource) definierter Begriff.

Die Ressource ist eine eindeutige Kennung, die mit MVPDs vereinbart wird und mit einem Inhalt verknüpft ist, den die Client-Anwendung streamen kann.

Die eindeutige Kennung der Ressource kann zwei Formate aufweisen:

* Ein einfaches Zeichenfolgenformat, z. B. eine eindeutige Kennung für einen Kanal (Marke).
* Ein Medien-RSS-Format (MRSS) mit zusätzlichen Informationen wie Titel, Bewertungen und Metadaten zur elterlichen Kontrolle.

Weitere Informationen finden Sie in der Dokumentation [Identifizieren geschützter Ressourcen](/help/authentication/identify-protected-resources.md) .

### 6. Für wie viele Ressourcen kann die Clientanwendung gleichzeitig eine Vorabgenehmigung erhalten? {#preauthorization-phase-faq6}

Die Clientanwendung kann aufgrund von durch MVPDs auferlegten Bedingungen eine Vorabautorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen in einer einzelnen API-Anfrage (in der Regel bis zu 5) erhalten.

Diese maximale Anzahl von Ressourcen kann nach Absprache mit MVPDs über das Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) durch einen Ihrer Organisationsadministratoren oder durch einen in Ihrem Namen tätigen Adobe Pass-Authentifizierungsbeauftragten angezeigt und geändert werden.

Weitere Informationen finden Sie im [TVE Dashboard Integrations User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) -Handbuch.

+++

++ + Häufig gestellte Fragen zur Autorisierungsphase

### 1. Welchen Zweck hat die Genehmigungsphase? {#authorization-phase-faq1}

Die Autorisierungsphase dient dazu, der Clientanwendung die Möglichkeit zu geben, Ressourcen abzuspielen, die der Benutzer nach der Validierung seiner Rechte mit dem MVPD anfordert.

### 2. Ist die Genehmigungsphase obligatorisch? {#authorization-phase-faq2}

Die Autorisierungsphase ist obligatorisch. Die Client-Anwendung kann diese Phase nicht überspringen, wenn sie Ressourcen abspielen möchte, die der Benutzer anfordert, da sie mit dem MVPD überprüfen muss, ob der Benutzer berechtigt ist, bevor der Stream veröffentlicht wird.

### 3. Was ist eine Genehmigungsentscheidung und wie lange ist sie gültig? {#authorization-phase-faq3}

Die Autorisierung ist ein Begriff, der in der Dokumentation [Glossar](./rest-api-v2-glossary.md#authorization) definiert ist, während der Entscheidungsbegriff auch im [Glossar](./rest-api-v2-glossary.md#decision) zu finden ist.

Die Genehmigungsentscheidung speichert Informationen über das Ergebnis der MVPD-Autorisierungsprozess-Anfrage, die vom Entscheidungsvalidierungs-Endpunkt abgerufen werden können.

Die Client-Anwendung kann die Autorisierungsentscheidung mit einem Medien-Token verwenden, um den Ressourcen-Stream wiederzugeben, falls die (verbindliche) Entscheidung des TV-Anbieters dem Benutzer den Zugriff auf diesen ermöglicht.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [API für Abrufen von Autorisierungsentscheidungen](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundlegender Autorisierungsfluss innerhalb der Hauptanwendung](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Die Autorisierungsentscheidung gilt für einen begrenzten und kurzen Zeitraum, der zum Zeitpunkt der Ausstellung festgelegt wird, und gibt an, wie lange sie von der Adobe Pass-Authentifizierung zwischengespeichert werden soll, bevor eine erneute Abfrage des MVPD erforderlich ist.

Dieser begrenzte Zeitraum, der als Autorisierung (authZ) [TTL](./rest-api-v2-glossary.md#ttl) bezeichnet wird, kann über das Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder von einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsbeauftragten angezeigt und geändert werden.

Weitere Informationen finden Sie im [TVE Dashboard Integrations User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) -Handbuch.

### 4. Was ist ein Medien-Token und wie lange ist es gültig? {#authorization-phase-faq4}

Das Medien-Token ist ein in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#media-token) definierter Begriff.

Das Medien-Token besteht aus einer signierten Zeichenfolge, die in unverschlüsseltem Text gesendet wird und vom Entscheidungsvalidierungs-Endpunkt abgerufen werden kann.

Weitere Informationen finden Sie in der Dokumentation [Integrieren des Medien-Token-Verifikators](/help/authentication/media-token-verifier-int.md) .

Das Medien-Token ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Ausgabe angegeben wird. Dies gibt die Zeit an, die die Client-Anwendung verwenden muss, bevor der Entscheidungsvalidierungs-Endpunkt erneut abgefragt werden muss.

Die Clientanwendung kann das Medien-Token verwenden, um einen Ressourcen-Stream abzuspielen, falls die (autoritative) Entscheidung des TV-Anbieters dem Benutzer den Zugriff auf diesen ermöglicht.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [API für Abrufen von Autorisierungsentscheidungen](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundlegender Autorisierungsfluss innerhalb der Hauptanwendung](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5. Was ist eine Ressource und welche Formate werden unterstützt? {#authorization-phase-faq5}

Die Ressource ist ein in der Dokumentation zu [Glossar](./rest-api-v2-glossary.md#resource) definierter Begriff.

Die Ressource ist eine eindeutige Kennung, die mit MVPDs vereinbart wird und mit einem Inhalt verknüpft ist, den die Client-Anwendung streamen kann.

Die eindeutige Kennung der Ressource kann zwei Formate aufweisen:

* Ein einfaches Zeichenfolgenformat, z. B. eine eindeutige Kennung für einen Kanal (Marke).
* Ein Medien-RSS-Format (MRSS) mit zusätzlichen Informationen wie Titel, Bewertungen und Metadaten zur elterlichen Kontrolle.

Weitere Informationen finden Sie in der Dokumentation [Identifizieren geschützter Ressourcen](/help/authentication/identify-protected-resources.md) .

### 6. Wie viele Ressourcen kann die Clientanwendung gleichzeitig eine Autorisierungsentscheidung erhalten? {#authorization-phase-faq6}

Die Clientanwendung kann eine Autorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen in einer einzelnen API-Anfrage erhalten, in der Regel bis zu 1 aufgrund von durch MVPDs auferlegten Bedingungen.

+++

Häufig gestellte Fragen zur +++ Abmeldephase

### 1. Welchen Zweck hat die Abmeldephase? {#logout-phase-faq1}

In der Abmeldephase soll der Clientanwendung die Möglichkeit gegeben werden, das authentifizierte Profil des Benutzers in der Adobe Pass-Authentifizierung auf Benutzeranfrage zu beenden.

+++

+++Kopfzeilen - Häufig gestellte Fragen

### 1. Wie wird der Wert für den Autorisierungs-Header berechnet? {#headers-faq1}

Der Anforderungsheader [Autorisierung](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) enthält das Zugriffstoken `Bearer` , das von der Clientanwendung für den Zugriff auf Adobe Pass-geschützte APIs benötigt wird.

Der Header-Wert für die Autorisierung muss während der Registrierungsphase von der Adobe Pass-Authentifizierung abgerufen werden.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Überblick über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Abrufen der Client-Anmeldeinformationen-API](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Zugriffstoken-API abrufen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamischer Ablauf zur Kundenregistrierung](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

Wenn die Client-Anwendung von der REST-API V1 zu der REST-API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode verwenden, um das Zugriffstoken `Bearer` wie zuvor zu erhalten.

### 2. Wie wird der Wert für die Kopfzeile &quot;AP-Device-Identifier&quot;berechnet? {#headers-faq2}

Die Anfragekopfzeile [AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) enthält die Streaming-Geräte-ID, wie sie von der Clientanwendung erstellt wurde.

Die Kopfzeilendokumentation [AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) enthält einige Beispiele für die Berechnung des Werts für verschiedene Plattformen. Die Clientanwendung kann jedoch eine andere Methode basierend auf ihrer eigenen Geschäftslogik und ihren Anforderungen verwenden.

Wenn die Client-Anwendung von der REST-API V1 zu der REST-API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode zur Berechnung der Gerätekennung verwenden wie zuvor.

### 3. Wie wird der Wert für die Kopfzeile &quot;X-Device-Info&quot;berechnet? {#headers-faq3}

Der Anforderungsheader [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) enthält die Client-Informationen (Gerät, Verbindung und Anwendung), die sich auf das eigentliche Streaming-Gerät beziehen.

Die Kopfzeilendokumentation [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) enthält einige Beispiele für die Berechnung des Werts für verschiedene Plattformen. Die Clientanwendung kann jedoch eine andere Methode basierend auf ihrer eigenen Geschäftslogik und ihren Anforderungen verwenden.

Wenn die Client-Anwendung von der REST-API V1 zu der REST-API V2 migriert, kann die Client-Anwendung weiterhin dieselbe Methode zur Berechnung der Geräteinformationen verwenden wie zuvor.

+++

## FAQs zur Migration {#migration-faqs}

Fahren Sie mit diesem Abschnitt fort, wenn Sie an einer Anwendung arbeiten, die eine vorhandene Anwendung in die REST-API V2 migrieren muss.

+++Häufig gestellte Fragen zur allgemeinen Migration

### 1. Muss ich eine neue Client-Anwendung einführen, die auf die REST-API V2 migriert ist und gleichzeitig an alle Benutzer gesendet wird? {#migration-faq1}

Anzahl

Die Clientanwendung ist nicht erforderlich, um eine neue Version mit der Integration der REST-API V2 für alle Benutzer gleichzeitig bereitzustellen.

Die Adobe Pass-Authentifizierung unterstützt bis Ende 2025 weiterhin ältere Clientanwendungsversionen, die die REST-API V1 oder das SDK integrieren.

### 2. Muss ich eine neue Client-Anwendung einführen, die über alle APIs und Datenflüsse hinweg auf die REST-API V2 migriert wird? {#migration-faq2}

Ja.

Die Clientanwendung ist erforderlich, um eine neue Version einzuführen, in der die REST-API V2 über alle Adobe Pass-Authentifizierungs-APIs und -Flüsse hinweg gleichzeitig integriert wird.

Im Fall der &quot;zweiten Bildschirmauthentifizierung&quot;muss die Client-Anwendung eine neue Version einführen, in der die REST-API V2 für die Anwendungen [primary](./rest-api-v2-glossary.md#primary-application) und [secondary](./rest-api-v2-glossary.md#secondary-application) gleichzeitig integriert wird.

Die Adobe Pass-Authentifizierung unterstützt keine &quot;hybriden&quot;Implementierungen, die sowohl die REST API V2 als auch die REST API V1/SDK zwischen APIs und Flüssen integrieren.

### 3. Wird die Benutzerauthentifizierung bei der Aktualisierung auf eine neue Client-Anwendung beibehalten, die auf die REST-API V2 migriert wird? {#migration-faq3}

Anzahl

Die Benutzerauthentifizierung, die in älteren Clientanwendungsversionen mit Integration der REST API V1 oder des SDK erhalten wurde, wird nicht beibehalten.

Daher muss sich der Benutzer innerhalb der neuen Client-Anwendung, die auf die REST-API V2 migriert wird, erneut authentifizieren.

### 4. Kann die Clientanwendung die vorhandenen registrierten Anwendungen (Softwareanweisungen) verwenden? {#migration-faq4}

Die Clientanwendung kann die vorhandenen registrierten Anwendungen (Softwareanweisungen) nicht wiederverwenden. Daher muss sie neue registrierte Anwendungen (Softwareanweisungen) generieren und herunterladen, die für die Nutzung der REST-API V2 vorgesehen sind.

Dieser Vorgang kann über das Adobe Pass [TVE-Dashboard](./rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder von einem für Sie zuständigen Adobe Pass-Authentifizierungsbeauftragten ausgeführt werden.

Weitere Informationen finden Sie in der Dokumentation zum [TVE Dashboard Channels User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) oder dem [TVE Dashboard-Programmierhandbuch](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) .

Derzeit müssen Sie einen Adobe Pass-Authentifizierungsbeauftragten bitten, die Verwendung der REST API V2 für Ihre neuen registrierten Anwendungen (Softwareanweisungen) zu aktivieren, bis das Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) aktualisiert wird, um die Selbstverwaltung dieses Vorgangs zu ermöglichen.

Um zwischen registrierten Anwendungen (Softwareanweisungen) zu unterscheiden, die in Clientanwendungen verwendet werden, die die REST API V2 nutzen, müssen Sie dem registrierten Anwendungsnamen ein bestimmtes Suffix hinzufügen, z. B. &quot;RESTV2&quot;.

### 5. Kann die Clientanwendung die vorhandenen benutzerdefinierten Schemas verwenden? {#migration-faq5}

Die Clientanwendung kann die vorhandenen benutzerdefinierten Schemas wiederverwenden, die über das Adobe Pass [TVE-Dashboard](./rest-api-v2-glossary.md#tve-dashboard) generiert wurden.

Weitere Informationen finden Sie in der Dokumentation zum [TVE Dashboard Channels User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#custom-schemes) oder dem [TVE Dashboard-Programmierhandbuch](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) .

### 6. Sind die erweiterten Fehlercodes in REST API V2 standardmäßig aktiviert? {#migration-faq6}

Ja.

Die Client-Anwendungen, die die REST-API V2 integrieren, profitieren von der standardmäßig aktivierten erweiterten Funktion für Fehlercodes.

Weitere Informationen finden Sie in der Dokumentation zu [erweiterten Fehlercodes](/help/authentication/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) .

+++

### Migration von REST API V1 zu REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Fahren Sie mit diesem Unterabschnitt fort, wenn Sie an einer Anwendung arbeiten, die von der REST-API V1 zu der REST-API V2 migriert werden muss.

Häufig gestellte Fragen zur ++ Registrierungsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Registrierungsphase erforderlich? {#registration-phase-v1-to-v2-faq1}

Bei der Migration von der REST-API V1 zu der REST-API V2 gibt es keine Änderungen auf hoher Ebene in Bezug auf die Registrierungsphase.

Die Clientanwendung kann denselben Ablauf weiterhin verwenden, um sich über den Prozess [Dynamische Client-Registrierung (DCR)](./rest-api-v2-glossary.md#dcr) bei der Adobe Pass-Authentifizierung zu registrieren und ein Zugriffstoken zu erhalten.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Überblick über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Abrufen der Client-Anmeldeinformationen-API](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Zugriffstoken-API abrufen](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamischer Ablauf zur Kundenregistrierung](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

+++

Häufig gestellte Fragen zur ++- Konfigurationsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Konfigurationsphase erforderlich? {#configuration-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST API V1 | REST API V2 | Bemerkungen |
|------------------------------------------------|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

Häufig gestellte Fragen zur ++- Authentifizierungsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Authentifizierungsphase erforderlich? {#authentication-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST API V1 | REST API V2 | Bemerkungen |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungscode abrufen (Authentifizierungscode) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Prüfen Sie den Registrierungs-Code (Authentifizierungscode). | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Fortsetzen der Authentifizierung (MVPD) auf dem zweiten Bildschirm (Anwendung) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiieren der Authentifizierung (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [GET <br/> /api/v1/checkauthn (erster Bildschirm)](/help/authentication/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (zweiter Bildschirm)](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen des Benutzerauthentifizierungstokens (profile) | [GET <br/> /api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++Häufig gestellte Fragen zur Vorautorisierungsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Vorabgenehmigungsphase erforderlich? {#preauthorization-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST API V1 | REST API V2 | Bemerkungen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen von vorab autorisierten Ressourcen (Vorabentscheidungen zur Autorisierung) | [GET <br/> /api/v1/preauthorize (erster Bildschirm)](/help/authentication/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (zweiter Bildschirm)](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

++ + Häufig gestellte Fragen zur Autorisierungsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Autorisierungsphase erforderlich? {#authorization-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST API V1 | REST API V2 | Bemerkungen |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiate-Autorisierung (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Clientanwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Initiate-Autorisierung (MVPD)</li><li>Autorisierungsentscheidung abrufen</li><li>Abruf des Short Media Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der Hauptanwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Autorisierungstoken abrufen (Autorisierungsentscheidung) | [GET <br/> /api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Clientanwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Initiate-Autorisierung (MVPD)</li><li>Autorisierungsentscheidung abrufen</li><li>Abruf des Short Media Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der Hauptanwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Abrufen eines kurzen Autorisierungstokens (Medien-Token) | [GET <br/> /api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Die Clientanwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Initiate-Autorisierung (MVPD)</li><li>Autorisierungsentscheidung abrufen</li><li>Abruf des Short Media Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der Hauptanwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

Häufig gestellte Fragen zur +++ Abmeldephase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Abmeldephase erforderlich? {#logout-phase-v1-to-v2-faq1}

Bei der Migration von REST API V1 zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in der folgenden Tabelle aufgeführt sind:

| Umfang | REST API V1 | REST API V2 | Bemerkungen |
|-----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abmeldung starten | [GET <br/> /api/v1/logout](/help/authentication/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migration vom SDK zur REST API V2 {#migrate-sdk-to-rest-api-v2}

Fahren Sie mit diesem Unterabschnitt fort, wenn Sie an einer Anwendung arbeiten, die von SDK zu REST API V2 migriert werden muss.

Häufig gestellte Fragen zur ++ Registrierungsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Registrierungsphase erforderlich? {#registration-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in den folgenden Tabellen dargestellt werden:

##### AccessEnabler JavaScript SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vollständige dynamische Kundenregistrierung (DCR) | Bereitstellung der Softwareanweisung für den Konstruktor | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Überblick über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Dynamischer Ablauf der Client-Registrierung](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vollständige dynamische Kundenregistrierung (DCR) | Bereitstellung der Softwareanweisung für den Konstruktor | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Überblick über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Dynamischer Ablauf der Client-Registrierung](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vollständige dynamische Kundenregistrierung (DCR) | Bereitstellung der Softwareanweisung für den Konstruktor | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Überblick über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Dynamischer Ablauf der Client-Registrierung](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler FireOS-SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vollständige dynamische Kundenregistrierung (DCR) | Bereitstellung der Softwareanweisung für den Konstruktor | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Überblick über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Dynamischer Ablauf der Client-Registrierung](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

Häufig gestellte Fragen zur ++- Konfigurationsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Konfigurationsphase erforderlich? {#configuration-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in den folgenden Tabellen dargestellt werden:

##### AccessEnabler JavaScript SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|------------------------------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler Android SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler FireOS-SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Liste der MVPDs mit aktiver Integration abrufen | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

Häufig gestellte Fragen zur ++- Authentifizierungsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Authentifizierungsphase erforderlich? {#authentication-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in den folgenden Tabellen dargestellt werden:

##### AccessEnabler JavaScript SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren der Authentifizierung (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/javascript-sdk-api-reference.md#checkAuthN) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/javascript-sdk-api-reference.md#getMeta) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler iOS SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren der Authentifizierung (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler tvOS-SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungscode abrufen (Authentifizierungscode) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Prüfen Sie den Registrierungs-Code (Authentifizierungscode). | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Fortsetzen der Authentifizierung (MVPD) auf dem zweiten Bildschirm (Anwendung) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiieren der Authentifizierung (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiieren der Authentifizierung (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/android-sdk-api-reference.md#checkAuthN) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/android-sdk-api-reference.md#getMetadata) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS-SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registrierungscode abrufen (Authentifizierungscode) | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Prüfen Sie den Registrierungs-Code (Authentifizierungscode). | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Fortsetzen der Authentifizierung (MVPD) auf dem zweiten Bildschirm (Anwendung) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiieren der Authentifizierung (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Authentifizierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundlegender Authentifizierungsfluss, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Überprüfen des Benutzerauthentifizierungsstatus | [AccessEnabler.checkAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthN) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Abrufen von Benutzermetadaten-Informationen | [AccessEnabler.getMetadata](/help/authentication/amazon-fireos-native-client-api-reference.md#getMetadata) | Verwenden Sie einen der folgenden Schritte: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Die Clientanwendung kann die Antwort dieser APIs für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Überprüfen des Benutzerauthentifizierungsstatus</li><li>Benutzerprofil abrufen</li><li>Abrufen von Benutzermetadaten-Informationen</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Ablauf von Profilen, der innerhalb der primären Anwendung ausgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundlegender Ablauf der Profile, der innerhalb der sekundären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++Häufig gestellte Fragen zur Vorautorisierungsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Vorabgenehmigungsphase erforderlich? {#preauthorization-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in den folgenden Tabellen dargestellt werden:

##### AccessEnabler JavaScript SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen von vorab autorisierten Ressourcen (Vorabentscheidungen zur Autorisierung) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen von vorab autorisierten Ressourcen (Vorabentscheidungen zur Autorisierung) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen von vorab autorisierten Ressourcen (Vorabentscheidungen zur Autorisierung) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Umfang | SDK | REST API V2 | Bemerkungen |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abrufen von vorab autorisierten Ressourcen (Vorabentscheidungen zur Autorisierung) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Vorautorisierungsfluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

++ + Häufig gestellte Fragen zur Autorisierungsphase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Autorisierungsphase erforderlich? {#authorization-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in den folgenden Tabellen dargestellt werden:

##### AccessEnabler JavaScript SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Abrufen eines kurzen Autorisierungstokens (Medien-Token) | [AccessEnabler.checkAuthorization](/help/authentication/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | Die Clientanwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Initiate-Autorisierung (MVPD)</li><li>Autorisierungsentscheidung abrufen</li><li>Abruf des Short Media Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der Hauptanwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Abrufen eines kurzen Autorisierungstokens (Medien-Token) | [AccessEnabler.checkAuthorization](/help/authentication/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | Die Clientanwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Initiate-Autorisierung (MVPD)</li><li>Autorisierungsentscheidung abrufen</li><li>Abruf des Short Media Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der Hauptanwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Abrufen eines kurzen Autorisierungstokens (Medien-Token) | [AccessEnabler.checkAuthorization](/help/authentication/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | Die Clientanwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Initiate-Autorisierung (MVPD)</li><li>Autorisierungsentscheidung abrufen</li><li>Abruf des Short Media Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der Hauptanwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS-SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Abrufen eines kurzen Autorisierungstokens (Medien-Token) | [AccessEnabler.checkAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/entscheidungen/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | Die Clientanwendung kann die Antwort dieser API für mehrere Zwecke gleichzeitig verwenden: <br/> <ul><li>Initiate-Autorisierung (MVPD)</li><li>Autorisierungsentscheidung abrufen</li><li>Abruf des Short Media Tokens</li></ul> <br/> Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Autorisierungsfluss, der innerhalb der Hauptanwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

Häufig gestellte Fragen zur +++ Abmeldephase

#### 1. Welche API-Migrationen auf hoher Ebene sind für die Abmeldephase erforderlich? {#logout-phase-sdk-to-v2-faq1}

Bei der Migration von SDKs zu REST API V2 sind Änderungen auf hoher Ebene zu beachten, die in den folgenden Tabellen dargestellt werden:

##### AccessEnabler JavaScript SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|-----------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Abmeldung starten | [AccessEnabler.logout](/help/authentication/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Abmeldung starten | [AccessEnabler.logout](/help/authentication/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Abmeldung starten | [AccessEnabler.logout](/help/authentication/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS-SDK

| Umfang | SDK | REST API V2 | Bemerkungen |
|-----------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Abmeldung starten | [AccessEnabler.logout](/help/authentication/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Weitere Informationen finden Sie in den folgenden Dokumenten: <br/> <ul><li>[Grundlegender Abmeldefluss, der innerhalb der primären Anwendung durchgeführt wird](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
