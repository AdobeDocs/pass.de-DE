---
title: REST-API - Übersicht
description: Übersicht über REST-APIs
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1635'
ht-degree: 0%

---

# (Legacy) REST-API - Übersicht {#rest-api-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Überblick {#over}

Die Adobe Pass-Authentifizierungs-REST-API bietet direkten Zugriff auf die Authentifizierungs- und Autorisierungsdienste von TV Everywhere (TVE). Diese API unterstützt zwei primäre Architekturen: Server-zu-Server oder verbundene Geräte (z. B. Spielekonsolen, Smart-TVs, Set-Top-Boxen usw.), die keine Webbrowsing-Funktionen haben.

### Drosselmechanismus

Die Adobe Pass-Authentifizierungs-REST-API wird von einem [Einschränkungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gesteuert.


### Server-zu-Server

Server-zu-Server-Lösungen beinhalten Programmierer-Client-Anwendungen, die mit Programmierer-Services integriert sind, die eine Verbindung mit Adobe Pass-Authentifizierungs-Services für TVE-Flüsse herstellen. Dieser Ansatz verlagert den Großteil der TVE-Implementierung vom Client auf den Server, wo ein einziges, einheitliches Autorisierungsmodul erstellt und gepflegt werden kann. Die primäre verbleibende Verantwortung der Client-Anwendungen ist die Verwaltung einer Web-Ansicht für die Benutzerauthentifizierung.



### Verbundene Geräte

Apps mit Connected Devices kommunizieren direkt über die REST-APIs mit der Adobe Pass-Authentifizierung, um Konfigurations-, Registrierungs-, Authentifizierungsstatusprüfungen und Autorisierungsflüsse durchzuführen, während eine zweite Bildschirm-App (Browser) für den Authentifizierungsfluss erforderlich ist. Daher werden keine nativen SDKs verwendet.



### Andere Architekturen

Neben den beiden primären REST-API-basierten Architekturen, Server-zu-Server- und Direct-Client-Lösungen für intelligente Geräte, gibt es weitere Architekturen.  Primär dazu gehört die SDK-Architektur, die eine Client-Komponente namens Access Enabler verwendet, den die Adobe Pass-Authentifizierung Programmierern bereitstellt.  Die App verwendet Access Enabler-APIs für die Verarbeitung von Start, Authentifizierung, Autorisierung und Abmeldung.  Die gesamte Kommunikation zwischen der App des Programmierers und den Adobe Pass-Authentifizierungsservern erfolgt über Access Enabler.  Eine andere Variante von Access Enabler ist für die folgenden Plattformen verfügbar: JavaScript, iOS, tvOS, Android und FireTV.

Obwohl es möglich ist, die REST-API direkt auf Client-Plattformen zu verwenden, die native SDKs außerhalb einer Server-zu-Server-Lösung unterstützen, wird dieser Ansatz nicht empfohlen.



## Vor- und Nachteile der REST-API {#ProsAndCons}

Die Adobe Pass-Authentifizierungs-REST-API wurde erstellt, um eine TVE-Lösung (TV Everywhere) für Geräte bereitzustellen, die keine Webbrowsing-Funktionen oder persistenten Speicher aufweisen. Die REST-API unterstützt alle Authentifizierungs- und Autorisierungsflüsse, da ihr jedoch eine native SDK-Komponente fehlt. Die von der Adobe Pass-Authentifizierung bereitgestellten und gepflegten SDKs verfügen über vordefinierte Funktionen, mit denen Geschäftsregeln implementiert werden, die im Fall der REST-API von den Programmierern implementiert und gepflegt werden müssen. In der folgenden Tabelle zu den Programmiererverantwortlichkeiten beschreiben wir die Einschränkungen der aktuellen REST-API, die von Programmierern behoben werden müssen.



### Server-zu-Server vs. Client-basierte Vor- und Nachteile

Eine Server-zu-Server-Architektur bietet die Möglichkeit, den Großteil der Authentifizierungs- und Autorisierungslogik in einer einzigen logischen Einheit oder Implementierung zu konsolidieren.  Dieser Ansatz hat Vor- und Nachteile.  Zu den Vorteile gehören:

* Einzelne Implementierung für Business-Logik zur Authentifizierung und Autorisierung.
* Vermeiden Sie die Notwendigkeit, diese Logik auf jeder unterstützten Plattform mit den nativen Tools dieser Plattformen zu implementieren.
* Die Möglichkeit, Funktionen zu aktualisieren, ohne die Clients mit allen zugehörigen Anforderungen (z. B. App Store-Aktualisierungen) aktualisieren zu müssen.
* **Einfachere Erweiterung** benutzerdefinierten AuthN- und AuthZ-Funktionen (z. B. Hinzufügen von D2C).
* Direktes Management des zugehörigen Traffics für mehr Kontrolle, Qualität und Überwachung.



Auch hier sind die Nachteile in den Verantwortlichkeiten des Programmierers aufgeführt, beinhalten jedoch Folgendes:

* SSO muss für jeden Client für Plattformen ohne Platform SSO implementiert werden.
* Programmierer müssen bei Bedarf MVPD-spezifische Logik implementieren.
* Alle Plattformen, die die REST-API verwenden, nutzen eine einzige Konfiguration, die Eigenschaften wie Authentifizierungs-TTLs steuert.



### Verbundene Geräte

Für die meisten angeschlossenen Geräte muss die REST-API auf die eine oder andere Weise verwendet werden, da eine SDK nicht verfügbar ist. Das verbundene Gerät verwendet entweder die REST-API direkt oder integriert sich in eine Server-zu-Server-Lösung, die die REST-API verwendet.

## Zuständigkeiten des Programmierers {#programmer-responsibilities}

Folgendes gilt sowohl für Server-zu-Server- als auch für Connected Device-Anwendungen.

| **Funktionalität** | **Verantwortlich für die Handhabung der Funktion** | **Beschreibung der Einschränkungen der aktuellen Clientless-API und der Unterschiede zu nativen SDKs** |
| --- | --- | --- |
| Angewendete Konfigurationseinstellungen pro Plattform | Adobe | Eine **Haupteinschränkung** bei der Verwendung der REST-API auf allen Plattformen (einschließlich Mobilgeräten wie iOS und Android) besteht darin, dass die Konfigurationseinstellungen, die der REST-API in unserem TVE Dashboard-Konfigurationstool entsprechen, auf allen Geräten angewendet werden (auch wenn es ein iOS-Gerät gibt, auf dem eine native Anwendung ausgeführt wird, die auf unserer REST-API implementiert ist). Diese Einschränkung **kann die** TTLs und die vereinbarten Plattformeinstellungen mit den MVPDs aufheben, wenn diese je Plattform unterschiedlich sind. [1](#1) |
| Single Sign-On | Programmierer | Bei Verwendung der REST-API ist SSO nur auf Plattformen verfügbar, die Platform SSO unterstützen (z. B. Apple, Roku, Amazon), während SSO bei Verwendung der REST-API für andere Plattformen nicht garantiert werden kann. Die SDKs speichern Daten Site-/App-übergreifend. Das bedeutet, dass sich der Benutzer einmal auf einer Site/in einer App anmeldet und bereits auf den teilnehmenden Sites angemeldet ist, ohne dass eine Benutzerinteraktion erforderlich ist. [2](#2) |
| Einzelne Abmeldung | Programmierer | In einem nativen SDK SSO-Szenario wird der Benutzer bei der Abmeldung von einer teilnehmenden Anwendung von überall abgemeldet. In der aktuellen REST-API unterstützen wir SLO nicht. Wenn Sie sich von einer Anwendung abmelden, wird der Benutzer nur für diese bestimmte Anwendung abgemeldet. |
| Caching | Programmierer | Die REST-API-Implementierungen müssen einen eigenen Caching-Mechanismus für geschäftlich vereinbarte Datenelemente implementieren. Die SDKs speichern automatisch verschiedene Datenelemente unter Berücksichtigung verschiedener Geschäftsregeln. Beispielsweise werden die Benutzermetadaten mit derselben TTL wie das Authentifizierungstoken zwischengespeichert, während einige Elemente programmgesteuert aus der Zwischenspeicherung ausgeschlossen werden können (Preflight). |
| Detaillierter Mechanismus zur Fehlerberichterstattung | Programmierer | Die REST-API nutzt hauptsächlich HTTP-Fehler-Codes für die Meldung von Anwendungsfehlern, während SDKs über einen detaillierten Mechanismus zur Fehlerberichterstattung verfügen, der den Anwendungsentwicklern hilft, die Geschehnisse besser zu verstehen. |
| Wiederherstellung von Anwendungsfehlern (Wiederholung bei Fehler, Schleifenerkennung usw.) | Programmierer | REST-API-Implementierungen müssen ihre eigenen Anwendungs-Wiederherstellungssysteme erstellen, während Implementierungen, die auf SDKs aufbauen, vom SDK-Fehlerbehebungssystem profitieren: Wiederherstellung nach vorübergehenden Netzwerkfehlern, indem bestimmte Netzwerkaufrufe mit vorhandener Logik wiederholt werden, um „Schleifen“ zu verhindern. |
| Authentifizierung pro Antragsteller | Programmierer | Einige MVPDs erfordern eine Authentifizierung für jede Site/App, entweder aus geschäftlichen Gründen oder aufgrund technischer Herausforderungen. Die SDKs erzwingen dies automatisch basierend auf der TVE-Dashboard-Konfiguration. Die REST-API-Implementierer müssen dies selbst implementieren, um keine Geschäftsvereinbarungen zu verletzen oder die Autorisierung für die MVPDs abzuschließen, die ihre Authentifizierungsdaten pro Anwendung nutzen. |
| Passive Authentifizierung | Programmierer | Einige MVPDs unterstützen eine „passive“ Authentifizierung, bei der der Benutzer die Anmeldeinformationen nicht eingeben muss und automatisch versucht wird, den Benutzer zu authentifizieren. Dies ist besonders für MVPDs nützlich, die auch eine „Authentifizierung pro Anforderer“ als Anforderung haben. In einem solchen Fall ist die durch die SDKs aktivierte Benutzeroberfläche nahtlos, wobei sich der Benutzer nur einmal in einer Anwendung authentifiziert und die SDK für andere Anwendungen im Ökosystem eine „passive“ Authentifizierung durchführt. Der Anwender sieht die zusätzlichen passiven Aufrufe nicht, er wird einfach bereits authentifiziert, während der MVPD das Ziel erreicht, separate Authentifizierungssitzungen für jede Anwendung zu haben. |
| Implizite und einheitliche Geräteerkennung | Programmierer | Die SDKs erkennen automatisch den Gerätetyp und senden diese Informationen auf standardmäßige Weise. REST-API-Implementierungen neigen je nach Implementierer dazu, unterschiedliche Informationen zu senden, wodurch Geschäftsregeln und Statistiken über Sites hinweg verzerrt/beschädigt werden. **Programmierer müssen sicherstellen, dass sie uns richtige Geräteinformationen** jede ihrer Apps geschickt haben. Bei Server-zu-Server-Implementierungen identifiziert die REST-API die IP-Adresse des Endbenutzers nicht korrekt, es sei denn, es werden zusätzliche Schritte unternommen. Die IP-Adresse ist in bestimmten Anwendungsfällen, wie z. B. bei der Betrugsbekämpfung oder bei HBA, wichtig. |
| Manipulationssichere TempPass | Programmierer | Das Erzwingen von TempPass basiert auf einer stabilen Geräte-ID. Die SDKs verwenden Hardware-Info-/Fingerabdrucktechniken, um das Gerät zu identifizieren. Dieser Mechanismus ist nicht öffentlich und daher sicherer und kollisionsfreier. Bei REST-API-Implementierungen müssen die Programmierer ihre eigenen Algorithmen für die Gerätekennung/den Fingerabdruck implementieren. |
| Gerätebindung | Programmierer | Auf einem Gerät mit einer Anwendung über SDK generierte Token sind nicht portabel, sodass es für böswillige Benutzende schwierig ist, ihre Token freizugeben und anderen Benutzenden Zugriff zu gewähren. Dies basiert auf demselben Geräte-ID-Mechanismus wie „Tamper Proof TempPass“. Bei der REST-API bleiben die Token in der Cloud, was bedeutet, dass zwei Geräte mit denselben Geräte-IDs, die dieselben Aufrufe ausführen, dieselben Antworten/Zugriffe erhalten. Programmierer müssen sicherstellen, dass sie über einen robusten, sicheren und kollisionsfreien Mechanismus zum Erzeugen/Zuweisen von Geräte-IDs verfügen. |
| Berechenbarer und zertifizierter Funktionssatz | Programmierer | Die in jedem SDK vorhandenen Funktionen sind konsistent, vorhersehbar und vollständig zertifiziert und getestet. Einige Geschäftsanforderungen werden über die SDKs durchgesetzt, sodass es für den Programmierer riskant ist, bei der Verwendung der REST-API Verträge zu brechen. Während die REST-API möglicherweise flexibler ist, garantiert Adobe, dass die SDK-Implementierungen geschäftliche Entscheidungen und Einstellungen über das TVE-Dashboard durchsetzen. Bei ClientLess implementiert jeder Programmierer seine eigene Untergruppe der Geschäftsregeln, die zu einem bestimmten Zeitpunkt zu seinen Anwendungen passen. |


1. Im Rahmen unserer neuen One-API-Initiative planen wir, diese Einschränkung zu beheben und in der Lage zu sein, Regeln pro Plattform basierend auf der Gerätekennung anzuwenden.

2. Adobe arbeitet weiterhin mit allen Hauptplattformen zusammen, um Platform SSO zu implementieren, das mit unserer REST-API verwendet werden kann. Unsere One-API-Initiative bietet SSO-Unterstützung zwischen Apps, die mit nativen SDKs implementiert wurden, und Apps, die mit der REST-API implementiert wurden.

## Mindestanforderungen an das Gerät {#min_reqs}

Um die Adobe Pass-Authentifizierungs-REST-API verwenden zu können, müssen Geräte die technischen Mindestanforderungen erfüllen oder überschreiten, die im Abschnitt REST-API des Dokuments [Adobe Pass-Authentifizierungsplattform / Geräte-/Tools-Anforderungen aufgeführt ](#general_clientless_reqs).
