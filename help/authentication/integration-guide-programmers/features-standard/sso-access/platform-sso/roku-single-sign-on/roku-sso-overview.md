---
title: Roku SSO - Übersicht
description: Über Roku SSO
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# Roku SSO - Übersicht {#overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

In diesem Dokument werden die Informationen beschrieben, die erforderlich sind, um die Single Sign-On (SSO)-Funktionen auf Roku-Geräten nutzen zu können. Die Adobe Pass-Authentifizierung arbeitet mit Roku zusammen, um das Benutzererlebnis bei der Anmeldung zu verbessern und die einmalige Anmeldung für TV Everywhere-Anwendungen für TV-Abonnenten zu erleichtern.

Die Lösung basiert auf der REST-API der Adobe Pass-Authentifizierung, sodass die meisten Programmierer ihre Anwendungen nicht aktualisieren müssen, um von Single Sign-On zu profitieren.

## Aktivieren von Roku SSO {#enable-roku-sso}

Roku SSO ist standardmäßig aktiviert, es sei denn, die Programmierer- oder MVPD-Anfrage für SSO wird deaktiviert.

Jeder Programmierer kann SSO auf der Roku-Plattform für bestimmte Integrationen aktivieren/deaktivieren.

### Was ein Programmierer tun sollte, damit Roku SSO funktioniert {#make-roku-sso-work}

Für Programmieranwendungen, die die REST-API auf Client-Geräten implementieren, funktioniert das Roku-SSO ohne Änderungen. Roku OS fügt bei jeder Anfrage an die Authentifizierungsendpunkte von Adobe Pass zwei HTTP-Header hinzu.

Für Programmierprogramme, die eine Server-zu-Server-Lösung für die REST-API implementieren, sollte sich der Programmierer an das Roku-Team wenden und um eine Konfiguration bitten, bei der diese beiden Header bei allen API-Flüssen an ihre Domain gesendet werden.

Von Roku bereitgestellte Teilnehmer-ID, die anstelle der Geräte-ID verwendet werden soll, die von der Anwendung übergeben wird, um anwendungsübergreifendes (und geräteübergreifendes) SSO sicherzustellen.

Weitere Informationen zum Format der erforderlichen Kopfzeilen erhalten Sie von Ihrem Adobe-Support-Mitarbeiter.

### Mögliche Probleme {#possible-issues}

Programmierer sollten sicherstellen, dass ihre aktuellen Implementierungen, die auf der Adobe-REST-API basieren, Rokus Plattform-SSO nicht behindern. Nachfolgend finden Sie eine Liste möglicher Probleme und deren Lösung.

| Problem | Mögliche Ursache | Mögliche Lösungen |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Kein Roku SSO-Header an Adobe gesendet | Verwendung von HTTP anstelle von HTTPS für Aufrufe an Adobe Pass-Authentifizierungs-Domains | HTTPS verwenden |
| MVPD-Logo für SSO-Token nicht angezeigt/nicht aktualisiert | Benutzeroberfläche nutzt lokalen Speicher | Anwendungen sollten die Benutzeroberfläche (und ggf. den lokalen Speicher) nach der Überprüfung der Authentifizierung aktualisieren |
| Abmeldung bei nicht erfolgter Authentifizierung ausgelöst | Anwendungsdesign | Die Anwendung sollte so aktualisiert werden, dass eine Abmeldung nie hinter den Kulissen erfolgt |

## FAQs {#faq}

* **Wie wird das SSO funktionieren?**

  SSO funktioniert in allen Programmierprogrammen, die von der Adobe Pass-Authentifizierung unterstützt werden, auf allen Roku-Geräten, die mit demselben Roku-Benutzer verknüpft sind. Nicht alle MVPDs erlauben Roku SSO.


* **Werden die Authentifizierungs-TTLs geändert?**

  Das erste gültige Authentifizierungstoken wird zur Durchführung von SSO verwendet, und in diesem Fall verwenden alle anderen Anwendungen, die über SSO authentifiziert werden, dieselbe TTL, bis sie abläuft. Wenn Sie also von einer Anwendung zur anderen navigieren, teilt die zweite Anwendung die TTL der ersten Anwendung, die sich authentifiziert.


* **Werden andere Adobe-Funktionen wie bisher funktionieren?**

  Alle Adobe Pass-Authentifizierungsfunktionen funktionieren wie zuvor.


* **Gibt es auf der Roku-Plattform einen Opt-in-/Opt-out-Prozess für Programmierer, die von SSO profitieren?**

  Dies ist eine Konfigurationsänderung im TVE-Dashboard von Adobe. Jeder Programmierer kann SSO auf der Roku-Plattform für bestimmte Integrationen aktivieren/deaktivieren.
