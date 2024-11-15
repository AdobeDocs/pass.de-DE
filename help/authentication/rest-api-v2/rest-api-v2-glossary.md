---
title: REST API V2-Glossar
description: REST API V2-Glossar
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '1964'
ht-degree: 0%

---

# REST API V2-Glossar {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Dieses Dokument enthält Definitionen für Begriffe, die bei der Integration der Adobe Pass Authentication REST API V2-Dokumentation verwendet werden, und dient als Überschreibung für unser veraltetes [Glossar](/help/authentication/glossary.md).

## Glossarbegriffe {#glossary-terms}

### A {#a}

#### Zugriffstoken {#access-token}

Das Zugriffstoken ist ein Token, das von der Adobe Pass-Authentifizierung als Ergebnis des Prozesses [Dynamische Client-Registrierung (DCR)](#dcr) generiert wird, der den Zugriff auf geschützte APIs sicherstellen soll.

#### Authentifizierung {#authentication}

Die Authentifizierung ist ein Prozess, der es einem Benutzer ermöglicht, seine Identität an einen [Programmierer](#programmer) zu beweisen, um Zugriff auf geschützte Inhalte ([resource](#resource)) zu erhalten, nachdem das Benutzerabonnement mit dem [MVPD](#mvpd) validiert wurde.

#### Authentifizierungscode {#code}

Der Authentifizierungscode ist ein Adobe Pass-Authentifizierungskonzept, das einen eindeutigen Wert speichert, der generiert wird, wenn ein Benutzer den [Authentifizierungsprozess](#authentication) startet, und die [Authentifizierungssitzung eines Benutzers](#session) eindeutig identifiziert, bis der Authentifizierungsprozess abgeschlossen ist.

Der Authentifizierungscode kann sowohl von einer [Primären (Programmierer-)Anwendung](#primary-application) als auch von einer [Sekundären (Programmierer-)Anwendung](#secondary-application) verwendet werden, um den [Authentifizierungsprozess](#authentication) abzuschließen, Informationen zur [Authentifizierungssitzung](#session) abzurufen oder auf den Benutzer [profile](#profile) zuzugreifen.

Synonym mit dem ehemaligen Begriff verwendet Registrierungscode.

#### Authentifizierungssitzung {#session}

Die Authentifizierungssitzung ist ein Adobe Pass-Authentifizierungskonzept, das Informationen über den Authentifizierungsprozess des Benutzers speichert, der von einer [Programmierer](#programmer) -Anwendung gestartet (oder fortgesetzt) wurde, und durch einen [Authentifizierungscode](#code) eindeutig identifiziert wird.

Die Authentifizierungssitzung kann auch die Anwendung [Programmierer](#programmer) angeben, um mit dem Prozess [Autorisierung](#authorization) fortzufahren, da dies die nächste Phase des Flusses [Berechtigung](#entitlement) ist, falls der Benutzer bereits authentifiziert ist.

#### Autorisierung {#authorization}

Bei der Autorisierung handelt es sich um einen Prozess, der Benutzern den Zugriff auf geschützte Inhalte ([resource](#resource)) aus einem [Programmer](#programmer) -Katalog auf Grundlage des im Besitz befindlichen [MVPD](#mvpd) -Abonnements ermöglicht, nachdem die Benutzerrechte mit dem [MVPD](#mvpd) validiert wurden.

### C {#c}

#### Client-Anmeldedaten {#client-credentials}

Die Client-Anmeldeinformationen sind ein Satz eindeutiger Werte, die während des Prozesses [Dynamische Client-Registrierung (DCR)](#dcr) generiert werden und zum Abrufen eines [Zugriffstokens](#access-token) verwendet werden sollen.

#### Konfiguration {#configuration}

Bei der Konfiguration handelt es sich um ein Adobe Pass-Authentifizierungskonzept, das Informationen über die Integrationseinstellungen [Programmierer](#programmer) und [MVPD](#mvpd) speichert und während des [Authentifizierungsprozesses](#authentication) verwendet werden kann, wenn der Benutzer aufgefordert wird, seinen [TV-Anbieter](#tv-provider) aus einer Liste aktiver Integrationen auszuwählen.

#### Benutzerdefiniertes Schema {#custom-scheme}

Das benutzerdefinierte Schema ist ein eindeutiger Wert, der auf eine [Programmer](#programmer) -Anwendung verweist, die aus dem Adobe Pass [TVE Dashboard](#tve-dashboard) generiert und heruntergeladen werden kann und als endgültige Umleitung in Anwendungen verwendet werden soll, die auf iOS-Geräten ausgeführt werden.

### D {#d}

#### DCR {#dcr}

Die dynamische Client-Registrierung (DCR) ist ein von [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) definierter Autorisierungsmechanismus und basiert auf dem OAuth 2.0-Autorisierungsframework, das von [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) beschrieben wird.

Das DCR wird als Adobe Pass-Authentifizierungsdienst an einen [Programmierer](#programmer) bereitgestellt, der den Zugriff auf geschützte APIs weiter aktivieren kann.

Weitere Informationen finden Sie in der Dokumentation [Übersicht über die dynamische Kundenregistrierung](/help/authentication/dcr-api/dynamic-client-registration-overview.md) .

#### Entscheidung {#decision}

Bei der Entscheidung handelt es sich um ein Adobe Pass-Authentifizierungskonzept, das Informationen zur Prozessabfrage [MVPD](#mvpd) [authorization](#authorization) oder [preauthorization](#preauthorization) speichert, um den Benutzerzugriff auf geschützte Inhalte durch [Programmierer](#programmer) zu ermöglichen oder zu verweigern.

#### Abbau {#degradation}

Der Abbau ist eine Adobe Pass-Authentifizierungsfunktion, mit der ein Benutzer auf geschützte Inhalte zugreifen kann, selbst wenn sein [MVPD](#mvpd) eine Dienstunterbrechung durchläuft.

Weitere Informationen finden Sie in der Dokumentation zur [Übersicht über die Abbauungs-API](/help/authentication/degradation-api-overview.md) .

### E {#e}

#### Berechtigung {#entitlement}

Die Berechtigung ist ein Adobe Pass-Authentifizierungskonzept, das die verfügbaren Datenflüsse und Funktionen enthält, mit denen Benutzer in verschiedenen Phasen auf geschützten Inhalt zugreifen können, von [Authentifizierung](#authentication), [Vorautorisierung](#preauthorization), [Autorisierung](#authorization) und schließlich [Abmeldung](#logout).

#### Verbesserter Fehlercode {#enhanced-error-code}

Der erweiterte Fehlercode ist ein Adobe Pass-Authentifizierungskonzept, das zusätzliche Informationen über den Fehler bereitstellt, der bei der Verarbeitung einer Anfrage aufgetreten ist.

Weitere Informationen finden Sie in der Dokumentation zu [erweiterten Fehlercodes](/help/authentication/enhanced-error-codes.md) .

### H {#h}

#### HBA {#hba}

Die Home-basierte Authentifizierung (HBA) ist der Prozess, bei dem einem Verbraucher automatisch Zugriff auf [TV Anywhere (TVE)](#tve) -Inhalte auf ausgewählten Geräten gewährt wird, die mit seinem Heimnetzwerk verbunden sind, das Teil des Standorts im Abonnementvertrag ist.

### I {#i}

#### Identitäts-Provider {#identity-provider}

Der Identitätsanbieter ist ein Unternehmen, das Identitätsdienste für Verbraucher über Kabel, Satellit oder Internet-basierte Dienste im Kontext von [TV Anywhere (TVE)](#tve) bereitstellt.

Synonym mit [MVPD](#mvpd) und [TV-Anbieter](#tv-provider).

### L {#l}

#### Abmelden {#logout}

Die Abmeldung ist ein Prozess, der es einem Benutzer ermöglicht, sein authentifiziertes [Profil](#profile) innerhalb der Adobe Pass-Authentifizierung zu beenden und die [Programmierer](#programmer)-Anwendung zu aktualisieren, um den Benutzerstatus widerzuspiegeln.

### M {#m}

#### Medien-Token {#media-token}

Das Medien-Token ist ein Token, das von der Adobe Pass-Authentifizierung aufgrund einer Autorisierung von [Entscheidung](#decision) generiert wird, die den Zugriff auf geschützte Inhalte ermöglichen soll.

Das Medien-Token wird an den [Programmierer](#programmer) übergeben, der es dann validiert, um die Sicherheit des Zugriffs für diese [Ressource](#resource) sicherzustellen.

Synonym mit dem früheren Begriff verwendet kurze Autorisierungstoken.

#### Media Token Verifier {#media-token-verifier}

Der Media Token Verifier ist eine von der Adobe Pass-Authentifizierung verteilte Bibliothek, die für die Überprüfung der Authentizität eines [Medien-Tokens](#media-token) verantwortlich ist.

Weitere Informationen finden Sie in der Dokumentation [Integrieren des Medien-Token-Verifikators](/help/authentication/media-token-verifier-int.md) .

#### MVPD {#mvpd}

Der Multichannel Video Programming Distributor (MVPD) ist ein Unternehmen, das Fernsehdienste für Verbraucher über Kabel-, Satelliten- oder Internet-basierte Dienste anbietet.

Der MVPD wird durch einen eindeutigen Wert identifiziert, der während des Onboarding-Prozesses zwischen dem MVPD und der Adobe definiert wird.

Synonym mit [TV-Anbieter](#tv-provider) und [Identitätsanbieter](#identity-provider).

### P {#p}

#### Partner {#partner}

Der Partner ist ein Unternehmen, das einem [Programmierer](#programmer) einen Dienst oder ein Framework bereitstellt, um ein Single Sign-on-Benutzererlebnis zu ermöglichen.

Der Partner wird durch einen eindeutigen Wert (z. B. &quot;Apfel&quot;) identifiziert, der beim Einstieg zwischen Partner und Adobe definiert wird.

#### Vorabgenehmigung {#preauthorization}

Die Vorabautorisierung ist ein Prozess, der es einem Benutzer ermöglicht, eine Untergruppe von [Ressourcen](#resource) aus einem [Programmer](#programmer)-Katalog in der Vorschau anzuzeigen, auf den er zugreifen kann, nachdem er die Benutzerrechte mit dem [MVPD](#mvpd) überprüft hat.

Synonym mit [Preflight](#preflight).

#### Preflight {#preflight}

Der Preflight-Vorgang ist ein Prozess, der es einem Benutzer ermöglicht, eine Untergruppe von [Ressourcen](#resource) aus einem [Programmer](#programmer)-Katalog in der Vorschau anzuzeigen, auf den er zugreifen kann, nachdem er die Benutzerrechte mit dem [MVPD](#mvpd) überprüft hat.

Synonym mit [Vorautorisierung](#preauthorization).

#### Primäre (Programmierer-)Anwendung {#primary-application}

Die primäre Anwendung bezieht sich auf eine [Programmierer](#programmer) -Anwendung, die die [Authentifizierung](#authentication) initiiert, den Prozess jedoch möglicherweise nicht abschließen kann, indem ein [Benutzeragent](#user-agent) verwendet wird, um zur Anmeldeseite [MVPD](#mvpd) zu navigieren.

#### Profil {#profile}

Das Profil ist ein Adobe Pass-Authentifizierungskonzept, das Informationen über das Beginndatum und das Enddatum der Authentifizierung des Benutzers, die Metadaten des [Benutzers](#user-metadata) sowie weitere Felder speichert, die die Methode zum Abrufen der Authentifizierung angeben (z. B. &quot;normal&quot;, &quot;beschädigt&quot;, &quot;temporär&quot;, &quot;Single Sign-on&quot; usw.).

Synonym mit dem früher verwendeten Authentifizierungstoken.

#### Programmierer {#programmer}

Der Programmierer ist ein Unternehmen, das Kunden über eigene Kanäle (Marken) über verschiedene Plattformen hinweg Inhalte bereitstellt.

Der Programmierer gruppiert mehrere eigene Kanäle (Marken) als [Dienstleister](#service-provider) in seiner Integration mit der Adobe Pass-Authentifizierung.

#### Proxy-MVPD {#proxy-mvpd}

Das Proxy-MVPD ist ein Unternehmen, das Identitätsdienste für andere MVPDs bereitstellt und direkt mit der Adobe Pass-Authentifizierung integriert.

#### Proximiertes MVPD {#proxied-mvpd}

Der proximierte MVPD ist ein Unternehmen, das keine direkte Integration mit der Adobe Pass-Authentifizierung hat, aber über einen [Proxy-MVPD](#proxy-mvpd) integriert wird.

#### Platform Identity {#platform-identity}

Die Plattformidentität ist eine eindeutige Plattform-ID-Payload, die von einem Dienst oder Framework (Bibliothek) generiert wird, der an das Gerät des Benutzers gebunden ist und dem [Programmierer](#programmer) bereitgestellt wird, um ein Single-Sign-On-Benutzererlebnis zu ermöglichen.

Weitere Informationen finden Sie in der Dokumentation [Single Sign-on mit Platform-Identitätsflüssen](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) .

### R {#r}

#### Registrierte Anwendung {#registered-application}

Die registrierte Anwendung ist ein Adobe Pass-Authentifizierungskonzept, das Informationen zur [Programmer](#programmer) -Anwendung speichert, die erforderlich ist, um mit dem Prozess zur [dynamischen Client-Registrierung (DCR)](#dcr) fortzufahren.

#### Ressource {#resource}

Die Ressource ist ein geschützter Inhalt, auf den ein Benutzer über einen [Programmierer](#programmer) -Katalog zugreifen möchte.

Die Ressource wird durch einen eindeutigen Wert identifiziert, der zwischen dem Programmierer und den MVPDs vereinbart wird.

Weitere Informationen finden Sie in der Dokumentation zum [Identifizieren geschützter Ressourcen](/help/authentication/identify-protected-resources.md) .

### S {#s}

#### SAML {#saml}

Die Security Assertion Markup Language (SAML) ist ein offener Standard für den Austausch von Authentifizierungs- und Autorisierungsdaten zwischen Parteien, insbesondere zwischen einem [Identitätsanbieter](#identity-provider) und einem [Dienstleister](#sp).

#### Sekundäre (Programmierer-)Anwendung {#secondary-application}

Die sekundäre Anwendung bezieht sich auf eine [Programmierer](#programmer) -Anwendung, die den [Authentifizierungsprozess](#authentication) abschließen kann, indem ein [Benutzeragent](#user-agent) verwendet wird, um zur Anmeldeseite [MVPD](#mvpd) zu navigieren.

Die sekundäre Anwendung kann auf demselben Gerät wie die primäre Anwendung oder auf einem anderen (sekundären) Gerät ausgeführt werden. In diesem Fall wird das Anmeldeerlebnis als Benutzererlebnis mit &quot;zweiter Bildschirmauthentifizierung&quot;bezeichnet.

#### Service Token {#service-token}

Das Service-Token ist eine eindeutige Benutzer-ID, die von einem Dienst oder Framework (Bibliothek) generiert wird, das an den Benutzer gebunden ist und dem [Programmierer](#programmer) bereitgestellt wird, um ein Single Sign-On-Benutzererlebnis zu ermöglichen.

Weitere Informationen finden Sie in der Dokumentation [Single Sign-on using Service-Token-Flüsse](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) .

#### Dienstleister {#service-provider}

Der Dienstleister ist ein Kanal (Marke), der einem [Programmierer](#programmer) gehört.

Der Dienstleister wird durch einen eindeutigen Wert identifiziert, der während des Onboarding-Prozesses zwischen dem Programmierer und Adobe definiert wird.

Synonym mit dem ehemaligen Begriff [Anforderer-ID](/help/authentication/glossary.md#requestor-id).

#### Softwareanweisung {#software-statement}

Die Softwareanweisung ist ein JSON-Web-Token (JWT), das vom Adobe Pass [TVE-Dashboard](#tve-dashboard) heruntergeladen werden kann und im Rahmen des Prozesses [Dynamische Client-Registrierung (DCR)](#dcr) verwendet werden soll.

#### SLO {#slo}

Die einmalige Abmeldung (SLO) ist ein Prozess, der es einem Benutzer ermöglicht, sich von allen Anwendungen abzumelden, die Teil der [einmaligen Anmeldung (SSO)](#sso) waren.

#### SP {#sp}

Der Dienstanbieter (SP) bezieht sich auf die Rolle, die die Adobe Pass-Authentifizierung im Namen eines [Programmierers](#programmer) in einer Integration mit einem [MVPD](#mvpd) spielt.

#### SSO {#sso}

Single Sign-On (SSO) ist ein Prozess, der es einem Benutzer ermöglicht, sich einmal zu authentifizieren und Zugriff auf mehrere [Programmer](#programmer)-Anwendungen zu erhalten, ohne dass er sich für jede dieser Anwendungen authentifizieren muss.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

Der grundlegende TempPass ist eine Adobe Pass-Authentifizierungsfunktion, mit der ein Benutzer für eine begrenzte Zeit auf geschützte Inhalte zugreifen kann, ohne sich mit einem [MVPD](#mvpd) authentifizieren zu müssen.

Weitere Informationen finden Sie in der Dokumentation zum [Temp Pass](/help/authentication/temp-pass.md) .

#### TempPass Promotional {#temp-pass-promotional}

Die Werbeaktion &quot;TempPass&quot;ist eine Adobe Pass-Authentifizierungsfunktion, mit der ein Benutzer für eine maximale Anzahl von Ressourcen und eine begrenzte Zeit auf geschützte Inhalte zugreifen kann, ohne sich mit einem [MVPD](#mvpd) authentifizieren zu müssen.

Weitere Informationen finden Sie in der Dokumentation zum [Vorübergehenden Weiterleitungstyp-Pass](/help/authentication/promotional-temp-pass.md) .

#### TTL {#ttl}

Die &quot;Time to Live&quot;(TTL) ist ein Wert, der angibt, für wie viel Zeit eine zugrunde liegende Entität gültig ist.

Die TTL kann für ein [Zugriffstoken](#access-token), ein [Profil](#profile), eine Autorisierungsentscheidung [Entscheidung](#decision) oder ein [Medien-Token](#media-token) erwähnt werden.

#### TVE {#tve}

Der TV Anywhere (TVE) ist eine Industrieschneide, die es Verbrauchern ermöglicht, auf ihre Lieblingsfernsehsendungen, -filme und andere Inhalte auf verschiedenen Geräten wie Smartphones, Tablets, Laptops und vielem mehr zuzugreifen.

#### TVE Dashboard {#tve-dashboard}

Das Dashboard TV Anywhere (TVE) ist ein Adobe Pass-Authentifizierungstool, das [Programmierern](#programmer) zur Verwaltung ihrer Konfiguration und Daten bereitgestellt wird.

Weitere Informationen finden Sie in der Dokumentation zum [TVE Dashboard-Benutzerhandbuch](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md) .

#### TV-Anbieter {#tv-provider}

Der Fernsehanbieter ist ein Unternehmen, das Fernsehdienste für Verbraucher über Kabel, Satelliten oder Internet anbietet.

Der TV-Anbieter wird durch einen eindeutigen Wert identifiziert, der während des Onboarding-Vorgangs zwischen dem TV-Anbieter und der Adobe definiert wird.

Synonym mit [MVPD](#mvpd) und [Identitätsanbieter](#identity-provider).

### U {#u}

#### Benutzeragent {#user-agent}

Der Benutzeragent bezieht sich auf einen Browser oder eine ähnliche (plattformspezifische) Komponente, die in der Lage ist, im Internet zu navigieren und die Anmeldeseite [MVPD](#mvpd) zu rendern.

#### Benutzermetadaten {#user-metadata}

Die Benutzermetadaten beziehen sich auf benutzerspezifische Attribute (z. B. Postleitzahlen, elterliche Bewertungen, Benutzer-IDs usw.), die von der [MVPD](#mvpd) verwaltet und von der Adobe Pass-Authentifizierung als Teil eines [Profils](#profile) bereitgestellt werden.

Weitere Informationen finden Sie in der Dokumentation zu [Benutzermetadaten](/help/authentication/user-metadata-feature.md) .

### V {#v}

#### VSA {#vsa}

Das Video Subscriber Account (VSA) ist ein von Apple entwickeltes Framework, das dem [Programmierer](#programmer) zur Verfügung gestellt wird, um ein Single Sign-On-Benutzererlebnis zu ermöglichen.

Weitere Informationen finden Sie in der Dokumentation zum [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) und zum [Single Sign-on mit Partner-Flüssen](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) .
