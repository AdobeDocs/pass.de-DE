---
title: Häufig gestellte Fragen zur dynamischen Client-Registrierung (DCR)
description: Häufig gestellte Fragen zur dynamischen Client-Registrierung (DCR)
exl-id: 12268163-632e-4884-b35d-a29cc8ef45bf
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 0%

---

# Häufig gestellte Fragen zur dynamischen Client-Registrierung (DCR) {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Dieses Dokument bietet einen Überblick über Antworten auf häufig gestellte Fragen zur Verwendung der dynamischen Client-Registrierung (DCR) für die Adobe Pass-Authentifizierung.

Weitere Informationen zur gesamten dynamischen Client-Registrierung (DCR) finden Sie in der Dokumentation [Übersicht über die dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

## Allgemeine FAQs {#general-faqs}

Beginnen Sie mit diesem Abschnitt, wenn Sie an einem Programm arbeiten, das die dynamische Client-Registrierung (DCR) integrieren muss, unabhängig davon, ob es sich um ein neues oder ein bestehendes Programm handelt, das von einem der vorherigen Mechanismen migriert wird.

>[!MORELIKETHIS]
>
> * [Häufig gestellte Fragen zur REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#general-faqs)

### Häufig gestellte Fragen zum Zugriff auf die REST-API v2 {#rest-api-v2-access-faqs}

+++REST API v2 - Zugriff auf häufig gestellte Fragen

#### 1. Was ist der Zweck der Registrierungsphase? {#rest-api-v2-access-faq1}

In der Registrierungsphase wird die Client-Anwendung über den DCR-Prozess (Dynamic Client Registration[&#x200B; für die Adobe Pass-Authentifizierung &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr).

Für den Prozess der dynamischen Client-Registrierung (DCR) muss die Client-Anwendung ein Paar von Client-Anmeldeinformationen abrufen und ein Zugriffstoken als Endziel der Registrierungsphase abrufen.

Weitere Informationen finden Sie in der Dokumentation [Übersicht über die Dynamic Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 2. Ist die Registrierungsphase obligatorisch? {#rest-api-v2-access-faq2}

Die Registrierungsphase ist obligatorisch, aber die Client-Anwendung kann diese Phase überspringen, wenn sie ein zwischengespeichertes Paar von Client-Anmeldeinformationen und ein Zugriffstoken hat, die noch gültig sind.

#### 3. Was ist eine Software-Erklärung und wie lange ist sie gültig? {#rest-api-v2-access-faq3}

Die Software-Anweisung ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement) definiert ist.

Die Software-Anweisung besteht aus einem JSON Web Token (JWT), das von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter [&#128279;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) Adobe Pass TVE Dashboard) generiert und heruntergeladen werden kann.

Die Software-Erklärung ist für einen unbegrenzten Zeitraum gültig, Sie können jedoch einen Adobe Pass-Authentifizierungsbeauftragten bitten, sie jederzeit zu widerrufen.

Die Client-Anwendung muss die Software-Anweisung speichern und sie verwenden, wenn Client-Anmeldeinformationen abgerufen werden müssen.

Weitere Informationen finden Sie in der Dokumentation [Übersicht über die Dynamic Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

#### 4. Wie kann ich ein Software-Statement erstellen und herunterladen? {#rest-api-v2-access-faq4}

Dieser Vorgang kann über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter ausgeführt werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Channels User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) oder [TVE Dashboard Programmers User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

#### 5. Was passiert, wenn eine Software-Erklärung widerrufen wird? {#rest-api-v2-access-faq5}

Wenn die Software-Anweisung widerrufen wird, gibt es eine wichtige Konsequenz zu beachten:

* Die Client-Programme, die die widerrufene Software-Anweisung verwenden, können die Flüsse für [Berechtigungen“ nicht mehr durchlaufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement) was bedeutet, dass Benutzende vom Abspielen von Inhalten ausgeschlossen werden.

#### 6. Was sind Client-Anmeldeinformationen und wie lange sind sie gültig? {#rest-api-v2-access-faq6}

Die Client-Anmeldeinformationen sind ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials) definiert ist.

Die Client-Anmeldeinformationen bestehen aus einer Client-Kennung und einem geheimen Client-Schlüsselpaar, die vom Endpunkt der Clientregistrierung abgerufen werden können.

Die Client-Anmeldedaten sind für einen unbegrenzten Zeitraum gültig.

Die Client-Anwendung muss die Client-Anmeldeinformationen speichern und sie unbegrenzt verwenden, wenn ein Zugriffs-Token abgerufen werden muss.

Weitere Informationen finden Sie in der Dokumentation [Abrufen von Client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)Anmeldeinformationen.

#### 7. Wie werden Client-Anmeldeinformationen verwaltet? {#rest-api-v2-access-faq7}

Wir empfehlen der Client-Anwendung, für jede Benutzeranwendungsinstanz ein eindeutiges Paar von Client-Anmeldeinformationen zu verwalten, sowohl bei Client-zu-Server- als auch bei Server-zu-Server-Integrationen mit der Adobe Pass-Authentifizierung.

#### 8. Sollte die Client-Anwendung die Client-Anmeldeinformationen in einem persistenten Speicher zwischenspeichern? {#rest-api-v2-access-faq8}

Die Client-Anwendung muss die Client-Anmeldeinformationen speichern und sie unbegrenzt verwenden, wenn ein Zugriffs-Token abgerufen werden muss.

#### 9. Was passiert, wenn zwischengespeicherte Client-Anmeldeinformationen verloren gehen? {#rest-api-v2-access-faq9}

Wenn zwischengespeicherte Client-Anmeldeinformationen verloren gehen, sind drei wichtige Folgen zu berücksichtigen:

* Die Client-Anwendung muss ein neues Paar von Client-Anmeldeinformationen abrufen.
* Die Client-Anwendung muss mithilfe des neuen Paars von Client-Anmeldeinformationen ein neues Zugriffstoken abrufen.
* Die Client-Anwendung muss den Benutzer zur erneuten Authentifizierung auffordern, da die Client-Anwendung den Zugriff auf die zuvor erhaltenen authentifizierten Profile verliert.

#### 10. Was ist ein Zugriffs-Token und wie lange ist es gültig? {#rest-api-v2-access-faq10}

Das Zugriffstoken ist ein Begriff, der in der Dokumentation [Glossar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token) definiert ist.

Das Zugriffstoken besteht aus einem [Bearer-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) das vom Client-Token-Endpunkt abgerufen werden kann.

Das Zugriffs-Token ist für einen begrenzten und kurzen Zeitraum gültig, der zum Zeitpunkt der Ausgabe angegeben ist.

Die Client-Anwendung muss das Zugriffstoken speichern und verwenden, bis es beim Targeting der REST-API V2 abläuft.

Die Client-Anwendung muss vor Ablauf des aktuellen Zugriffstokens ein neues Zugriffstoken erhalten, um nicht autorisierte Anforderungen zu verhindern.

Weitere Informationen finden Sie in der Dokumentation [Zugriffstoken abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) .

#### 11. Sollte die Client-Anwendung das Zugriffstoken in einem persistenten Speicher zwischenspeichern? {#rest-api-v2-access-faq11}

Die Client-Anwendung muss das Zugriffstoken speichern und verwenden, bis es abläuft. Dann muss es verworfen und ein neues abgerufen werden.

#### 12. Wie kann die Client-Anwendung ein Zugriffstoken aktualisieren? {#rest-api-v2-access-faq12}

Die Client-Anwendung muss ein Zugriffstoken auf die gleiche Weise aktualisieren wie ein neues Zugriffstoken, jedoch unter Verwendung zwischengespeicherter Client-Anmeldeinformationen.

Die Client-Anwendung darf sich nicht erneut registrieren, um ein Zugriffstoken zu aktualisieren. Stattdessen muss sie die gespeicherten Client-Anmeldeinformationen verwenden, da sich die Benutzer andernfalls erneut authentifizieren müssen.

Weitere Informationen finden Sie in der Dokumentation [Zugriffstoken abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) .

+++

## Häufig gestellte Fragen zur Migration {#migration-faqs}

Fahren Sie mit diesem Abschnitt fort, wenn Sie an einem Programm arbeiten, das ein vorhandenes Programm migrieren muss, um die dynamische Client-Registrierung (DCR) zu verwenden.

>[!MORELIKETHIS]
>
> * [Häufig gestellte Fragen zur REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#migration-faqs)

### Häufig gestellte Fragen zur Migration zur REST API v2 {#rest-api-v2-migration-faqs}

+++Häufig gestellte Fragen zur REST API v2-Migration

#### 1. Kann die Client-Anwendung die vorhandenen registrierten Anwendungen (Software-Anweisungen) wiederverwenden? {#rest-api-v2-migration-faq1}

Die Client-Anwendung kann die vorhandenen registrierten Anwendungen nicht wiederverwenden (Software-Anweisungen). Daher muss sie neue registrierte Anwendungen (Software-Anweisungen) generieren und herunterladen, die speziell für die Nutzung der REST-API V2 vorgesehen sind.

Dieser Vorgang kann über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter ausgeführt werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Channels User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) oder [TVE Dashboard Programmers User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

Zunächst müssen Sie einen Adobe Pass-Authentifizierungsbeauftragten bitten, die Verwendung der REST-API V2 für Ihre neu registrierten Anwendungen (Software-Anweisungen) zu aktivieren, bis das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) aktualisiert wird, um die Selbstverwaltung dieses Vorgangs zu ermöglichen.

Um die registrierten Anwendungen (Software-Anweisungen) zu unterscheiden, die in Client-Anwendungen verwendet werden, die REST API V2 nutzen, müssen Sie dem registrierten Anwendungsnamen ein spezifisches Suffix hinzufügen, z. B. „RESTV2“.

#### 2. Kann die Client-Anwendung die vorhandenen benutzerdefinierten Schemata wiederverwenden? {#rest-api-v2-migration-faq2}

Die Client-Anwendung kann die vorhandenen benutzerdefinierten Schemata wiederverwenden, die über das Adobe Pass-[TVE-Dashboard) generiert &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard).

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Channels User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes) oder [TVE Dashboard Programmers User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes).

+++
