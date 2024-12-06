---
title: Roku SSO-Übersicht
description: Über Roku SSO
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# Roku SSO-Übersicht {#overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

In diesem Dokument werden die Informationen beschrieben, die erforderlich sind, um die SSO-Funktion (Single Sign-On) auf Roku-Geräten nutzen zu können. Adobe Pass Authentication arbeitet mit Roku zusammen, um die Benutzererfahrung bei der Anmeldung zu verbessern und Single-Sign-On für TV-Abonnenten überall in der Welt zu ermöglichen.

Die Lösung basiert auf der REST-API der Adobe Pass-Authentifizierung, sodass die meisten Programmierer ihre Anwendungen nicht aktualisieren müssen, um Single Sign-On nutzen zu können.

## Aktivieren von Roku SSO {#enable-roku-sso}

Roku SSO ist standardmäßig aktiviert, es sei denn, der Programmierer oder MVPD fordert die Deaktivierung von SSO an.

Jeder Programmierer kann SSO für bestimmte Integrationen auf der Roku-Plattform aktivieren/deaktivieren.

### Was ein Programmierer tun sollte, damit Roku SSO funktioniert {#make-roku-sso-work}

Für Programmiereranwendungen, die die REST-API auf Clientgeräten implementieren, funktioniert die Roku-SSO ohne Änderungen. Roku OS fügt bei jeder Anfrage zwei HTTP-Header zu Adobe Pass-Authentifizierungsendpunkten hinzu.

Bei Programmiereranwendungen, die eine Server-zu-Server-Lösung für REST-API implementieren, sollte sich der Programmierer an das Roku-Team wenden und nach einer Konfiguration fragen, bei der diese beiden Header bei allen API-Flüssen an ihre Domäne gesendet werden.

Von Roku bereitgestellte Abonnenten-ID, die anstelle der von der Anwendung übergebenen Geräte-ID verwendet werden soll, um eine anwendungsübergreifende (und geräteübergreifende) SSO zu gewährleisten.

Wenden Sie sich an Ihren Adobe-Kundenbetreuer, um genauere Informationen zum Format der benötigten Kopfzeilen zu erhalten.

### Mögliche Probleme {#possible-issues}

Programmierer sollten überprüfen, ob ihre aktuellen Implementierungen basierend auf der Adobe REST API die Plattform-SSO von Roku nicht behindern. Unten finden Sie eine Liste möglicher Probleme und wie diese gelöst werden sollten.

| Problem | Mögliche Ursache | Mögliche Lösungen |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Kein Roku-SSO-Header an Adobe gesendet | Verwenden von HTTP anstelle von HTTPS für Aufrufe an Adobe Pass-Authentifizierungsdomänen | HTTPS verwenden |
| MVPD-Logo wird für SSO-Token nicht angezeigt/nicht aktualisiert | Die Benutzeroberfläche basiert auf lokalem Speicher | Die Anwendungen sollten die Benutzeroberfläche (und ggf. den lokalen Speicher) aktualisieren, nachdem die Authentifizierung überprüft wurde |
| Abmeldeauslösung bei keiner AuthZ | Anwendungsdesign | Die Anwendung sollte so aktualisiert werden, dass sie sich nie hinter den Kulissen abmelden kann |

## FAQs {#faq}

* **Wie funktioniert die einmalige Anmeldung?**

  SSO funktioniert für alle Programmierer-Anwendungen, die mit Adobe Pass-Authentifizierung betrieben werden, auf allen Roku-Geräten, die mit demselben Roku-Benutzer verknüpft sind. Nicht alle MVPDs ermöglichen Roku SSO.


* **Gibt es eine Änderung an den Authentifizierungs-TTLs?**

  Das erste gültige Authentifizierungstoken wird für die Durchführung der einmaligen Anmeldung verwendet. In diesem Fall verwenden alle anderen Anwendungen, die über SSO authentifiziert werden, dieselbe TTL, bis sie abläuft. Wenn Sie also von einer Anwendung zur anderen navigieren, teilt die zweite Anwendung die TTL der ersten authentifizierten Anwendung.


* **Funktionieren andere Adobe-Funktionen wie zuvor?**

  Alle Adobe Pass-Authentifizierungsfunktionen funktionieren wie bisher.


* **Gibt es einen Opt-in-/Opt-out-Prozess für Programmierer, der von SSO auf der Roku-Plattform profitiert?**

  Dies ist eine Konfigurationsänderung im Adobe TVE Dashboard. Jeder Programmierer kann SSO für bestimmte Integrationen auf der Roku-Plattform aktivieren/deaktivieren.
