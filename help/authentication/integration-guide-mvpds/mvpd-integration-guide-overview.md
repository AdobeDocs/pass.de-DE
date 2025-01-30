---
title: MVPD-Integrationshandbuch
description: MVPD-Integrationshandbuch
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 0%

---

# MVPD-Integrationshandbuch {#mvpd-integration-guide}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Dieses Integrationshandbuch ist für Multichannel Video Programming Distributors (MVPDs) gedacht, die eine Integration mit der Adobe® Pass-Authentifizierung planen.

TV Everywhere (TVE) ist eine transformative Initiative in der Pay-TV-Branche, durch die Abonnentinnen und Abonnenten auf Inhalte zugreifen können, für die sie bereits bezahlen, und zwar über mehrere Geräte hinweg, ob zu Hause oder unterwegs. Für Pay-TV-Anbieter bietet TVE erhebliche Möglichkeiten, unter anderem die bestehenden Kundenbeziehungen zu stärken und Türen für neue zu öffnen. Diese Chancen sind jedoch mit Herausforderungen verbunden.

Im TVE-Ökosystem liefern **Programmierer** die Inhalte, während **MVPDs** (Multichannel Video Programming Distributors) die Kundendaten verwalten, die benötigt werden, um zu verifizieren, ob Betrachter berechtigte Abonnenten sind. Während die Koordination der Authentifizierung und Autorisierung mit einem einzelnen Programmierer verwaltbar sein kann, führt dies mit Dutzenden oder sogar Hunderten von Programmierern zu erheblicher Komplexität.

Hier vereinfacht die Authentifizierung mit **Adobe® Pass** den Prozess. MVPDs müssen nur eine einzige, optimierte Integration mit Adobe Pass implementieren, um Zugriff auf das gesamte TVE-Ökosystem zu erhalten. Das bereitgestellte Integrations-Framework beschleunigt die Time-to-Market, bietet eine sichere Umgebung zur Betrugsbekämpfung und verbessert das Kundenerlebnis, indem es mehr TV-Inhalte über mehrere Plattformen hinweg bereitstellt.

## Adobe Pass-Authentifizierung für TV Everywhere {#adobe-pass-authentication-for-tv-everywhere}

Die Adobe Pass-Authentifizierung erfolgt als SaaS-Lösung (Software as a Service), die eine schnelle Backend-Integration (Server-zu-Server) ermöglicht und die Geschäftsregeln von Multichannel Video Programming Distributors (MVPDs) und Programmierern einhält.

### Standards und Protokolle {#standards-protocols}

Die Adobe Pass-Authentifizierung ist vollständig an die neuen Standards und Protokolle für TV Everywhere (TVE) angepasst und unterstützt eine nahtlose Integration und Compliance im gesamten Ökosystem.

* **CableLabs OLCA (Online Content Access) Spezifikation**\
  Die Adobe Pass-Authentifizierung entspricht der CableLabs-OLCA-Spezifikation, die die technischen Anforderungen und die Architektur für die Bereitstellung von Videoinhalten aus Online-Quellen an Pay-TV-Kunden definiert. Adobe nahm im Juni 2011 aktiv am CableLabs-Interoperabilitätstest teil und hat den Testprozess für eine Service Provider-Implementierung erfolgreich bestanden.

Die Adobe Pass-Authentifizierung wurde entwickelt, um mehrere Protokolle zu unterstützen (z. B. SAML, OAuth 2.0 usw.). Diese Flexibilität ermöglicht zukünftige Erweiterungen, einschließlich benutzerdefinierter Protokolle, basierend auf sich verändernden Anforderungen.

Die meisten Integrationen verwenden das SAML-Protokoll (Security Assertion Markup Language), einen primären Standard für die Authentifizierung. Die Adobe Pass-Authentifizierung fungiert als Proxy-Service-Provider innerhalb des SAML-Frameworks und behält die SAML-Authentifizierungsantwort als sicheres Token in der gemeinsamen Adobe-Domain bei.

Im Endeffekt ist die Adobe Pass-Authentifizierung protokollunabhängig und wurde entwickelt, um sie eng an die OLCA-Standards anzupassen. Diese Standards bilden ein gemeinsames Framework für Programmierer, MVPDs und Dienstleister, wodurch ein kohärenter Ansatz zur Implementierung von TVE-Funktionen gewährleistet wird.

### Integration und Support {#integration-support}

Die Adobe Pass-Authentifizierung arbeitet wie folgt mit den technischen MVPD-Teams zusammen, um Integrationen zu konfigurieren, die auf ihre spezifischen Anforderungen zugeschnitten sind:

* **Standardintegrationen**\
  Kostenlos, einschließlich Dokumentation und E-Mail-Support.

* **Verbesserter Support**\
  Für umfangreiche Anpassungen oder beschleunigte Zeitpläne kann eine Support-Gebühr erhoben werden.

Die Adobe Pass-Authentifizierung unterstützt die effiziente Handhabung der Geschäftslogik von MVPD wie folgt:

* **eigenständige Geschäftslogik**\
  Für Geschäftslogiken, die während Autorisierungsanfragen vollständig von MVPD erzwungen werden, stellt Adobe die erforderlichen Daten bereit. Zu diesen Daten können unter anderem die eindeutige Geräte-ID und die IP-Adresse des Geräts des Benutzers gehören, das die Anfrage stellt.

* **Unterstützung benutzerdefinierter Eigenschaften**\
  Für Geschäftslogiken, die ein Eingreifen des Benutzers oder eine Adobe-spezifische Handhabung erfordern, kann Adobe benutzerdefinierte Konfigurationen für jede MVPD verwalten. Diese Richtlinien ermöglichen vordefinierte Workflows, die an bestimmten Punkten im Berechtigungs-Workflow ausgelöst werden können. Weitere Informationen erhalten Sie von Ihrem Adobe-Support-Mitarbeiter.

## Berechtigungsfluss {#entitlement-flow}

Der Berechtigungsfluss ist eine Reihe von Schritten, die ein Programmierprogramm (TVE) durchführen muss, um geschützte Inhalte zu streamen. Der Fluss umfasst mehrere Phasen, die Interaktionen mit dem MVPD beinhalten:

* [Authentifizierungsphase](#authentication-phase)
* (Optional) Phase vor der Autorisierung
* [Autorisierungsphase](#authorization-phase)
* Abmeldephase

Beim ersten Besuch eines Benutzers bei einem Programmierer-Programm (TVE) folgt der Berechtigungsfluss der beschriebenen Reihenfolge. Bei nachfolgenden Besuchen kann die Anwendung jedoch bestimmte Schritte je nach Status der Authentifizierung und den geltenden Anzeigerichtlinien umgehen.

>[!NOTE]
>
> Programmer-Anwendung (TVE) wird in diesem Dokument verwendet, um kollektiv auf die Anwendungstypen zu verweisen, die auf verschiedenen Plattformen ausgeführt werden (Browser, Mobilgeräte, an Fernsehgeräte angeschlossene Geräte usw.), die von der Adobe Pass-Authentifizierung unterstützt werden.

### Authentifizierungsphase {#authentication-phase}

In den folgenden Schritten werden die allgemeinen Schritte für den Fall einer SAML-Integration beschrieben:

1. **Laden der Anwendung (Website) des Programmierers**\
   Der/die Benutzende navigiert zur Programmieranwendung (Website), die die Adobe Pass-Authentifizierung ([-API V2) ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Geschützte Inhaltsanfrage**\
   Wenn Benutzende versuchen, auf geschützte Inhalte zuzugreifen, zeigt die Anwendung des Programmierers eine Liste von MVPDs an, aus denen die Benutzenden auswählen können.

1. **Initialisierung der Authentifizierungsanfrage**\
   Nach Auswahl von MVPD wird der Benutzer an einen Adobe Pass-Authentifizierungsserver umgeleitet. Hier wird im Falle einer SAML-Integration eine verschlüsselte SAML-Authentifizierungsanfrage für den ausgewählten MVPD generiert. Diese Anfrage wird im Namen des Programmierers an die MVPD gesendet. Je nach System von MVPD wird der Browser des Benutzers entweder zur Anmeldeseite von MVPD umgeleitet oder ein Anmelde-iFrame ist in die Anwendung des Programmierers eingebettet.

1. **MVPD-Anmeldung**\
   Der MVPD akzeptiert die Anfrage und stellt seine Anmeldeschnittstelle bereit, entweder über Umleitung oder iFrame.

1. **Benutzeranmeldung und -validierung**\
   Der Benutzer meldet sich mit seinen MVPD-Anmeldeinformationen an. MVPD überprüft den Abonnementstatus des Benutzers und richtet eine eigene HTTP-Sitzung ein.

1. **MVPD-Antwort auf Adobe Pass-Authentifizierung**\
   Nach Abschluss der Validierung generiert der MVPD eine SAML-Antwort (verschlüsselt) und sendet sie zurück an die Adobe Pass-Authentifizierung.

1. **Profilgenerierung**\
   Die Adobe Pass-Authentifizierung überprüft die SAML-Antwort, generiert ein Benutzerprofil, das zwischengespeichert wird, und leitet den Benutzer zurück zur Anwendung des Programmierers (Website).

### Autorisierungsphase {#authorization-phase}

**Allgemeine Schritte**

Die folgenden Schritte beschreiben die allgemeinen Schritte:

1. **Umgang mit Ressourcenkennungen**\
   Der geschützte Inhalt wird durch eine [Ressourcenkennung“ identifiziert](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier) die eine einfache Zeichenfolge oder eine komplexere Struktur sein kann. Diese Kennung ist vordefiniert und wird vom Programmierer und der MVPD vereinbart. Die Anwendung des Programmierers sendet die Ressourcenkennung an die Adobe Pass-Authentifizierung [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **MVPD-Autorisierungsprüfung**\
   Der Adobe Pass-Authentifizierungsserver kommuniziert mithilfe standardisierter Protokolle mit dem Autorisierungsendpunkt von MVPD.

1. **MVPD-Antwort auf Adobe Pass-Authentifizierung**\
   Nach Abschluss der Validierung bestätigt der MVPD, dass der Benutzer berechtigt ist (oder nicht), auf den Inhalt zuzugreifen, und sendet eine Antwort zurück an die Adobe Pass-Authentifizierung.

1. **Generieren von Entscheidungs- und Medien-Token**\
   Die Adobe Pass-Authentifizierung überprüft die Antwort, generiert eine [Entscheidung](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) die zwischengespeichert wird und gibt die Entscheidung mit einem Medien-Token zurück an die Anwendung des Programmierers (Website).

1. **Überprüfung des Inhaltszugriffs**\
   Das Programm des Programmierers verwendet den [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) um zu bestätigen, dass der richtige Benutzer auf den richtigen Inhalt zugreift. Nach der Validierung erhält der Benutzer Zugriff, um die geschützten Inhalte anzuzeigen.

## Grundlegendes zu Berechtigungen {#understanding-entitlements}

Die Adobe Pass-Authentifizierungslösung umfasst die Erstellung von Berechtigungen - spezifische Datenelemente, die nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungs-Workflows generiert werden. Diese Berechtigungen gewähren Zugriff auf geschützte Inhalte, haben jedoch eine begrenzte Lebensdauer. Sobald eine Berechtigung abläuft, muss sie erneuert werden, indem die Authentifizierungs- oder Autorisierungsprozesse erneut initiiert werden.

Weitere Informationen zu Berechtigungen finden Sie in den folgenden Dokumenten:

* **Profile**

  Bei erfolgreicher Authentifizierung erstellt die Adobe Pass-Authentifizierung ein authentifiziertes Profil („langlebig„), das mit der ID der anfragenden Anwendung, des Geräts und des Dienstleisters (Anfordererkennung) verknüpft ist.

* **[Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Nach erfolgreicher Authentifizierung (und in einigen Fällen auch nach der Autorisierung) empfängt die Adobe Pass-Authentifizierung Benutzermetadaten von der MVPD, die sie für die anfragende Anwendung verfügbar machen.

* **[Entscheidungen](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung eine Autorisierungsentscheidung („langlebig„), die mit der anfragenden Anwendung, dem Gerät, der Kennung des Dienstleisters (Anfordererkennung) und einer bestimmten geschützten Ressource (Ressourcenkennung) verknüpft ist.

* **[Medien-Token](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung ein Medien-Token („kurzlebig„), das mit einer erfolgreichen Wiedergabeanfrage verknüpft ist.
