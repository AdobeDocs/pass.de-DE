---
title: Glossar zur REST-API v2
description: Glossar zur REST-API v2
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# Glossar zur REST-API v2 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Dieses Dokument enthält Definitionen für Begriffe, die bei der Integration der Adobe Pass Authentication REST API V2 verwendet werden.

>[!MORELIKETHIS]
>
> * [Glossar zur dynamischen Client-Registrierung (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## Glossarbegriffe {#glossary-terms}

### Ein {#a}

#### Authentifizierung {#authentication}

Die Authentifizierung ist ein Prozess, der es einem Benutzer ermöglicht, seine Identität gegenüber einem [Programmierer](#programmer) nachzuweisen, um Zugriff auf geschützte Inhalte ([Ressource](#resource)) zu erhalten, nachdem das Benutzerabonnement mit der [MVPD validiert ](#mvpd).

#### Authentifizierungs-Code {#code}

Der Authentifizierungs-Code ist ein Adobe Pass-Authentifizierungskonzept, das einen eindeutigen Wert speichert, der generiert wird, wenn ein Benutzer den [Authentifizierungsprozess](#authentication) initiiert, und die [Authentifizierungssitzung](#session) eines Benutzers eindeutig identifiziert, bis der Authentifizierungsprozess abgeschlossen ist.

Der Authentifizierungs-Code kann sowohl von einer [Primären (Programmierer-)](#primary-application) als auch von einer [Sekundären (Programmierer-)](#secondary-application) verwendet werden, um den [Authentifizierungs](#authentication)-Prozess abzuschließen, Informationen über die [Authentifizierungssitzung](#session) abzurufen oder auf den Benutzer [profile](#profile) zuzugreifen.

Synonym für den früheren Begriff verwendet Registrierungs-Code.

#### Authentifizierungssitzung {#session}

Die Authentifizierungssitzung ist ein Adobe Pass-Authentifizierungskonzept, das Informationen zum Authentifizierungsprozess des Benutzers speichert, der von einem [Programmierprogramm](#programmer) gestartet (oder fortgesetzt) wurde, und durch einen [Authentifizierungs-Code](#code) eindeutig identifiziert wird.

In der Authentifizierungssitzung kann auch angegeben werden[ dass die ](#programmer)-Anwendung mit dem [Autorisierungs](#authorization)-Prozess als nächste Phase des [Berechtigungs](#entitlement)-Flusses fortfahren soll, falls der Benutzer bereits authentifiziert ist.

#### Autorisierung {#authorization}

Die Autorisierung ist ein Prozess, der es einem Benutzer ermöglicht, auf geschützte Inhalte ([resource](#resource)) aus einem [Programmierer](#programmer)-Katalog zuzugreifen, der auf dem eigenen [MVPD](#mvpd)-Abonnement basiert, nachdem die Benutzerrechte mit der [MVPD validiert ](#mvpd).

### C {#c}

#### Konfiguration {#configuration}

Bei der Konfiguration handelt es sich um ein Adobe Pass-Authentifizierungskonzept, das Informationen zu den [Programmierer](#programmer)- und [MVPD](#mvpd)-Integrationseinstellungen speichert und während des [Authentifizierungs](#authentication)-Prozesses verwendet werden kann, wenn Benutzende gebeten werden, ihren [TV-Anbieter](#tv-provider) aus einer Liste aktiver Integrationen auszuwählen.

### D {#d}

#### Entscheidung {#decision}

Bei der Entscheidung handelt es sich um ein Adobe Pass-Authentifizierungskonzept, das Informationen über die Prozessabfrage [MVPD](#mvpd) [authorization](#authorization) oder [preAuthorization](#preauthorization) speichert, um Benutzenden den Zugriff auf einen durch [Programmierer](#programmer) geschützten Inhalt zu ermöglichen oder zu verweigern.

#### Degradierung {#degradation}

Bei der Beeinträchtigung handelt es sich um eine Adobe Pass-Authentifizierungsfunktion, mit der Benutzende auch dann auf geschützte Inhalte zugreifen können, wenn bei ihrer [MVPD](#mvpd) eine Service-Unterbrechung auftritt.

Weitere Informationen finden Sie in der Dokumentation [Abbaufunktion](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md).

#### Geräte-ID {#device-id}

Die Geräte-ID ist eine eindeutige Kennung, die an das Gerät des Benutzers gebunden ist und von der Anwendung [Programmierer](#programmer) in allen Phasen des Flusses [Berechtigung](#entitlement) bereitgestellt werden muss.

### E {#e}

#### Berechtigung {#entitlement}

Bei der Berechtigung handelt es sich um ein Adobe Pass-Authentifizierungskonzept, das die verfügbaren Flüsse und Funktionen umfasst, die Benutzenden dabei helfen, verschiedene Phasen zu durchlaufen, um auf geschützte Inhalte zuzugreifen, von [Authentifizierung](#authentication), [Vorabautorisierung](#preauthorization), [Autorisierung](#authorization) und schließlich [Abmelden](#logout).

#### Erweiterter Fehlercode {#enhanced-error-code}

Der erweiterte Fehler-Code ist ein Adobe Pass-Authentifizierungskonzept, das zusätzliche Informationen zu dem bei der Verarbeitung einer Anfrage aufgetretenen Fehler bereitstellt.

Weitere Informationen finden Sie in der Dokumentation [Erweiterte ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)).

### H {#h}

#### HBA {#hba}

Bei der Home-Based Authentication (HBA) handelt es sich um den Prozess, bei dem einem Verbraucher automatisch Zugriff auf [TV Everywhere (TVE)-](#tve) auf ausgewählten Geräten gewährt wird, die mit seinem Heimnetzwerk verbunden sind, das Teil des Speicherorts im Abonnementvertrag ist.

### Ich {#i}

#### Identitätsanbieter {#identity-provider}

Der Identitätsanbieter ist ein Unternehmen, das Identitätsdienste für Verbraucher über Kabel-, Satelliten- oder internetbasierte Dienste im Kontext von [TV Everywhere (TVE) bereitstellt](#tve).

Synonym für [MVPD](#mvpd) und [TV Provider](#tv-provider).

### L {#l}

#### Abmelden {#logout}

Die Abmeldung ist ein Prozess, der es einem Benutzer ermöglicht, sein authentifiziertes [Profil](#profile) innerhalb der Adobe Pass-Authentifizierung zu beenden und das [Programmierer](#programmer)-Programm zu aktualisieren, um den Status des Benutzers widerzuspiegeln.

### M {#m}

#### Medien-Token {#media-token}

Das Medien-Token ist ein Token, das von der Adobe Pass-Authentifizierung als Ergebnis einer Autorisierung ([) generiert ](#decision), um Zugriff auf geschützte Inhalte zu gewähren.

Das Medien-Token wird an den [Programmierer](#programmer) übergeben, der es dann validiert, um die Sicherheit des Zugriffs für diese [Ressource“ ](#resource).

Synonym für den früheren Begriff verwendet Short Authorization Token.

#### Medien-Token-Verifizierer {#media-token-verifier}

Der Media Token Verifier ist eine von der Adobe Pass-Authentifizierung verteilte Bibliothek, die für die Überprüfung der Authentizität eines [Medien-Tokens“ ](#media-token) ist.

Weitere Informationen finden Sie in der Dokumentation [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) .

#### MVPD {#mvpd}

Der Multichannel Video Programming Distributor (MVPD) ist ein Unternehmen, das Verbrauchern Fernsehdienste über Kabel-, Satelliten- oder Internet-basierte Dienste anbietet.

Die MVPD wird durch einen eindeutigen Wert identifiziert, der während des Onboarding-Prozesses zwischen der MVPD und der Adobe definiert wird.

Synonym für [TV Provider](#tv-provider) und [Identity Provider](#identity-provider).

### P {#p}

#### Teilhaber {#partner}

Der Partner ist ein Unternehmen, das einem Programmierer einen Service oder ein Framework [, ](#programmer) ein Single Sign-on-Benutzererlebnis zu ermöglichen.

Der Partner wird durch einen eindeutigen Wert (z. B. „apple„) identifiziert, der während des Onboarding-Prozesses zwischen dem Partner und der Adobe definiert wird.

#### Vorautorisierung {#preauthorization}

Die Vorautorisierung ist ein Prozess, der es einem Benutzer ermöglicht, eine Teilmenge von [Ressourcen](#resource) aus einem [Programmierer](#programmer)-Katalog in der Vorschau anzuzeigen, auf den er zugreifen darf, nachdem er die Benutzerrechte mit der [MVPD validiert ](#mvpd).

Synonym für [Preflight](#preflight).

#### vor dem Flug {#preflight}

Der Preflight ist ein Prozess, der es einem Benutzer ermöglicht, eine Teilmenge von [Ressourcen](#resource) aus einem [Programmierer](#programmer)-Katalog in der Vorschau anzuzeigen, auf den er zugreifen darf, nachdem er die Benutzerrechte mit der [MVPD validiert ](#mvpd).

Synonym für [Vorabautorisierung](#preauthorization).

#### Primäre (Programmierer-)Anwendung {#primary-application}

Die primäre Anwendung bezieht sich auf eine [Programmierer](#programmer)-Anwendung, die [Authentifizierung](#authentication) initiiert, aber möglicherweise nicht in der Lage ist, den Prozess mithilfe eines [Benutzeragenten](#user-agent) abzuschließen, um zur Anmeldeseite von [MVPD](#mvpd) zu navigieren.

#### Profil {#profile}

Bei dem Profil handelt es sich um ein Adobe Pass-Authentifizierungskonzept, das Informationen über das Start- und Enddatum der Authentifizierung des Benutzers, die [Metadaten des Benutzers](#user-metadata) sowie andere Felder speichert, die die Authentifizierungsmethode angeben (z. B. „normal“, „degradiert“, „temporär“, „Single Sign-on“ usw.).

Synonym für den früheren Begriff „verwendetes Authentifizierungstoken“.

#### Programmierer {#programmer}

The Programmer ist ein Unternehmen, das Verbrauchern Inhalte über eigene Kanäle (Marken) auf verschiedenen Plattformen bereitstellt.

Der Programmierer gruppiert mehrere eigene Kanäle (Marken) als [Dienstleister](#service-provider) in seiner Integration mit der Adobe Pass-Authentifizierung.

#### Proxy-MVPD {#proxy-mvpd}

Die Proxy-MVPD ist ein Unternehmen, das Identitätsdienste für andere MVPDs bereitstellt und direkt in die Adobe Pass-Authentifizierung integriert ist.

#### Proxy-MVPD {#proxied-mvpd}

Der Proxy-MVPD ist ein Unternehmen, das keine direkte Integration mit der Adobe Pass-Authentifizierung hat, aber über einen [Proxy-MVPD) ](#proxy-mvpd) wird.

#### Platform-Identität {#platform-identity}

Die Plattformidentität ist eine eindeutige Plattformkennungs-Payload, die von einem Service oder Framework (Bibliothek) generiert wird, der an das Gerät des Benutzers gebunden ist und dem [Programmierer](#programmer) bereitgestellt wird, um ein Single Sign-on-Benutzererlebnis zu ermöglichen.

Weitere Informationen finden Sie in der Dokumentation [Single Sign-on using Platform Identity Flows](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

### R {#r}

#### Ressource {#resource}

Die Ressource ist ein geschützter Inhalt, auf den eine Benutzerin oder ein Benutzer über einen [-](#programmer)-Katalog zugreifen möchte.

Die Ressource wird durch einen eindeutigen Wert identifiziert, der zwischen dem Programmierer und den MVPDs vereinbart wird.

Weitere Informationen finden Sie in der Dokumentation [Geschützte Ressourcen](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) .

### S {#s}

#### SAML {#saml}

Die Security Assertion Markup Language (SAML) ist ein offener Standard für den Austausch von Authentifizierungs- und Autorisierungsdaten zwischen Parteien, insbesondere zwischen einem [Identitätsanbieter](#identity-provider) und einem [Dienstleister](#sp).

#### Sekundäre (Programmierer-)Anwendung {#secondary-application}

Die sekundäre Anwendung bezieht sich auf eine [Programmierer](#programmer)-Anwendung, die den [Authentifizierungs](#authentication)-Prozess mithilfe eines [Benutzeragenten) abschließen kann](#user-agent) um zur Anmeldeseite von [MVPD](#mvpd) zu navigieren.

Die sekundäre Anwendung kann auf demselben Gerät wie die primäre Anwendung oder auf einem anderen (sekundären) Gerät ausgeführt werden. In diesem Fall wird die Anmeldung als Benutzererlebnis „Authentifizierung auf dem zweiten Bildschirm“ bezeichnet.

#### Service-Token {#service-token}

Das Service-Token ist eine eindeutige Benutzerkennung, die von einem Service oder einer Framework (Bibliothek) generiert wird, der/die an den Benutzer gebunden ist, und dem [Programmierer) bereitgestellt wird](#programmer) um ein Single Sign-on-Benutzererlebnis zu ermöglichen.

Weitere Informationen finden Sie in der Dokumentation [Single Sign-on using Service Token Flows](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

#### Dienstleister {#service-provider}

Der Dienstleister ist ein Kanal (eine Marke), der/die einem [Programmierer) ](#programmer).

Der Dienstleister wird durch einen eindeutigen Wert identifiziert, der während des Onboarding-Prozesses zwischen dem Programmierer und dem Adobe definiert wird.

Synonym für den früheren Begriff „Antragsteller-ID“.

#### LANGWEILIG {#slo}

Die einmalige Abmeldung (Single Logout, SLO) ist ein Prozess, der es einem Benutzer ermöglicht, sich von allen Anwendungen abzumelden, die Teil des [Single Sign-on (SSO) waren](#sso).

#### SP {#sp}

Der Dienstleister (SP) bezieht sich auf die Rolle, die die Adobe Pass-Authentifizierung für einen [Programmierer](#programmer) in einer Integration mit einer [MVPD](#mvpd) spielt.

#### SSO {#sso}

Single Sign-On (SSO) ist ein Prozess, der es einem Benutzer ermöglicht, sich einmal zu authentifizieren und Zugriff auf mehrere [Programmierer](#programmer)-Anwendungen zu erhalten, ohne dass sich für jede dieser Anwendungen authentifizieren muss.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

Der einfache TempPass ist eine Adobe Pass-Authentifizierungsfunktion, mit der ein Benutzer für eine begrenzte Zeit auf geschützte Inhalte zugreifen kann, ohne sich mit einer [MVPD authentifizieren zu ](#mvpd).

Weitere Informationen finden Sie in der Dokumentation [Basic TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass).

#### TempPass-Werbeaktion {#temp-pass-promotional}

Der Promotion-TempPass ist eine Adobe Pass-Authentifizierungsfunktion, die es einem Benutzer ermöglicht, für eine maximale Anzahl von Ressourcen und für einen begrenzten Zeitraum auf geschützte Inhalte zuzugreifen, ohne sich bei einer [MVPD authentifizieren zu ](#mvpd).

Weitere Informationen finden Sie in der Dokumentation [Promotion TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass).

#### TTL {#ttl}

Die Time-to-Live (TTL) ist ein Wert, der die Zeitdauer angibt, für die eine zugrunde liegende Entität gültig ist.

Die TTL kann für ein [Zugriffstoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token), ein [Profil](#profile), eine Autorisierung [Entscheidung](#decision) oder ein [Medien-Token](#media-token).

#### TVE {#tve}

Der TV Everywhere (TVE) ist eine Nische in der Branche, in der Verbraucher auf ihre bevorzugten Fernsehsendungen, Filme und andere Inhalte auf mehreren Geräten wie Smartphones, Tablets, Laptops und vielen mehr zugreifen können.

#### TVE-Dashboard {#tve-dashboard}

Das TV Everywhere-Dashboard (TVE) ist ein Adobe Pass-Authentifizierungstool, das [Programmierern“ zur ](#programmer) von Konfiguration und Daten bereitgestellt wird.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

#### Fernsehanbieter {#tv-provider}

Der Fernsehanbieter ist ein Unternehmen, das Verbrauchern Fernsehdienste über Kabel, Satellit oder Internet anbietet.

Der TV-Anbieter wird durch einen eindeutigen Wert identifiziert, der beim Onboarding-Prozess zwischen dem TV-Anbieter und Adobe definiert wird.

Synonym für [MVPD](#mvpd) und [Identitätsanbieter](#identity-provider).

### U-{#u}

#### Benutzeragent {#user-agent}

Der Benutzeragent verweist auf einen Browser oder eine ähnliche Komponente (plattformspezifisch), die im Web navigieren und die Anmeldeseite von [MVPD](#mvpd) rendern kann.

#### Benutzer-ID {#user-id}

Die Benutzer-ID ist eine eindeutige Kennung, die an den Benutzer gebunden ist und aus dem Authentifizierungsprozess [MVPD](#mvpd) stammt.

#### Benutzermetadaten {#user-metadata}

Die Benutzermetadaten beziehen sich auf benutzerspezifische Attribute (z. B. Postleitzahlen, elterliche Bewertungen, Benutzer-IDs usw.), die von der [MVPD](#mvpd) verwaltet und von der Adobe Pass-Authentifizierung als Teil eines [Profils](#profile) bereitgestellt werden.

Weitere Informationen finden Sie in der Dokumentation [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) .

### v {#v}

#### VSA {#vsa}

Das Videoabonnentenkonto (VSA) ist ein von Apple entwickeltes Framework, das dem [Programmierer](#programmer) bereitgestellt wird, um ein Single Sign-on-Benutzererlebnis zu ermöglichen.

Weitere Informationen finden Sie in der Dokumentation [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) und [Single Sign-on using Partner Flows](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
