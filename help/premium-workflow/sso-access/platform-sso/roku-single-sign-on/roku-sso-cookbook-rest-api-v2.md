---
title: Roku SSO Cookbook (REST API v2)
description: Roku SSO Cookbook (REST API v2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Roku SSO Cookbook (REST API v2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Die Adobe Pass-Authentifizierungs-REST-API V2 unterstützt Platform Single Sign-On (SSO) für Endbenutzer von Client-Anwendungen, die auf RokuOS ausgeführt werden.

Dieses Dokument dient als Erweiterung für die vorhandene [REST API V2 - Übersicht](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md), die eine allgemeine Ansicht bietet, sowie für das Dokument, in dem die Implementierung von [Single Sign-on mithilfe von Platform-Identitätsflüssen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) beschrieben wird.

## Roku-Single-Sign-on mit Platform-Identitätsflüssen {#cookbook}

Die Adobe Pass-Authentifizierung arbeitet mit Roku zusammen, um das Benutzererlebnis bei der Anmeldung zu verbessern und die einmalige Anmeldung (Single Sign-On, SSO) für TV Everywhere-Programme für TV-Abonnenten zu erleichtern.

### Voraussetzungen {#prerequisites}

Bevor Sie mit dem Roku-Single-Sign-on unter Verwendung von Platform-Identitätsflüssen fortfahren, stellen Sie sicher, dass Roku SSO aktiviert ist. Roku SSO ist standardmäßig aktiviert, es sei denn, die Programmierer- oder MVPD-Anfrage für SSO wird deaktiviert.

Jeder Programmierer kann das Single Sign-On (SSO) auf der Roku-Plattform für bestimmte Integrationen über das [ Adobe Pass TVE Dashboard aktivieren oder ](https://experience.adobe.com/pass/authentication).

### Workflow {#workflow}

**Client-zu-Server**

Für Programmieranwendungen, die eine Client-zu-Server-Architektur zur Integration der REST-API V2 verwenden, funktioniert Roku SSO nahtlos und ohne Änderungen.

RokuOS hängt automatisch zwei HTTP-Header an alle Anfragen an, die an Authentifizierungsendpunkte von Adobe Pass gesendet werden.

**Server-zu-Server**

Für Programmieranwendungen, die eine Server-zu-Server-Architektur zur Integration der REST-API V2 verwenden, muss sich der Programmierer mit dem Roku-Team abstimmen, um diese Kopfzeilen zu konfigurieren, damit sie in alle API-Flüsse aufgenommen werden, die an ihre Domain gerichtet sind.

Um programmübergreifendes und geräteübergreifendes SSO zu aktivieren, sollte die von Roku bereitgestellte Teilnehmer-ID anstelle der Geräte-ID verwendet werden, wenn sie von der Anwendung übergeben wird.

Weitere Informationen finden Sie in der folgenden Dokumentation:

* [Kopfzeile - x-roku-reserved-roku-connect-token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [Header - AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

Weitere Informationen zum Format der erforderlichen Kopfzeilen erhalten Sie von Ihrem Adobe-Support-Mitarbeiter.

### Häufig gestellte Fragen {#faqs}

* **Wie wird das SSO funktionieren?**

  SSO funktioniert in allen Programmierprogrammen, die von der Adobe Pass-Authentifizierung unterstützt werden, auf allen Roku-Geräten, die mit demselben Roku-Benutzer verknüpft sind. Nicht alle MVPDs erlauben Roku SSO.


* **Werden die Authentifizierungs-TTLs geändert?**

  Das erste gültige Authentifizierungstoken wird zur Durchführung von SSO verwendet, und in diesem Fall verwenden alle anderen Anwendungen, die über SSO authentifiziert werden, dieselbe TTL, bis sie abläuft. Wenn Sie also von einer Anwendung zur anderen navigieren, teilt die zweite Anwendung die TTL der ersten Anwendung, die sich authentifiziert.


* **Funktionieren andere Adobe-Funktionen wie bisher?**

  Alle Adobe Pass-Authentifizierungsfunktionen funktionieren wie zuvor.


* **Gibt es auf der Roku-Plattform einen Opt-in-/Opt-out-Prozess für Programmierer, die von SSO profitieren?**

  Dies ist eine Konfigurationsänderung im TVE-Dashboard von Adobe. Jeder Programmierer kann SSO auf der Roku-Plattform für bestimmte Integrationen aktivieren oder deaktivieren.


* **Was sind einige häufige Probleme?**

  Programmierer sollten sicherstellen, dass ihre aktuellen Implementierungen, die auf Adobes REST-API basieren, Rokus Plattform-SSO nicht behindern.

  Nachfolgend finden Sie eine Liste möglicher Probleme und deren Lösung.

| Problem | Mögliche Ursache | Mögliche Lösungen |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Kein Roku-SSO-Header an Adobe gesendet | Verwendung von HTTP anstelle von HTTPS für Aufrufe an Adobe Pass-Authentifizierungs-Domains | HTTPS verwenden |
| MVPD-Logo für SSO-Token nicht angezeigt/nicht aktualisiert | Benutzeroberfläche nutzt lokalen Speicher | Anwendungen sollten die Benutzeroberfläche (und ggf. den lokalen Speicher) nach der Überprüfung der Authentifizierung aktualisieren |
| Abmeldung bei nicht erfolgter Authentifizierung ausgelöst | Anwendungsdesign | Die Anwendung sollte so aktualisiert werden, dass eine Abmeldung nie hinter den Kulissen erfolgt |
