---
title: REST API - Übersicht
description: Übersicht über REST-APIs
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 0%

---

# REST API - Übersicht {#rest-api-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Übersicht {#over}

Die Adobe Pass Authentication REST API bietet direkten Zugriff auf die Authentifizierungs- und Autorisierungsdienste von TV Anywhere (TVE). Diese API unterstützt zwei Hauptarchitekturen: Server-zu-Server oder verbundene Geräte (z. B. Spielekonsolen, Smart-TVs, Set-Top-Boxen usw.) Anwendungen ohne Webbrowsing-Funktionen.

### Drosselmechanismus

Die Adobe Pass-Authentifizierungs-REST-API wird von einem [Drosselmechanismus](/help/authentication/throttling-mechanism.md) verwaltet.


### Server-zu-Server

Server-zu-Server-Lösungen beinhalten Programmierer-Client-Anwendungen, die in Programmiererdienste integriert sind, die mit Adobe Pass-Authentifizierungsdiensten für TVE-Flüsse verbunden sind. Dieser Ansatz verschiebt den Großteil der TVE-Implementierung vom Client auf den Server, auf dem ein einzelnes, einheitliches Autorisierungsmodul erstellt und gepflegt werden kann. Die verbleibende Hauptverantwortung der Clientanwendungen besteht in der Verwaltung einer Webansicht für die Benutzerauthentifizierung.



### Angeschlossene Geräte

Connected Devices-Apps kommunizieren über die REST-APIs direkt mit der Adobe Pass-Authentifizierung, um Konfigurations-, Registrierungs-, Authentifizierungsstatus- und Autorisierungsflüsse durchzuführen, während für den Authentifizierungsfluss eine zweite Bildschirmanwendung (Browser) erforderlich ist. Daher werden keine nativen SDKs verwendet.



### Sonstige Architekturen

Neben den beiden primären REST API-basierten Architekturen, Server-zu-Server- und Direct-Client-Lösungen für intelligente Geräte, gibt es auch andere Architekturen.  Primär ist unter anderem die SDK-Architektur, die eine Client-Komponente namens Access Enabler verwendet, die die Adobe Pass-Authentifizierung für Programmierer bereitstellt.  Das Programm verwendet Access Enabler-APIs, um Start, Authentifizierung, Autorisierung und Abmeldung zu verarbeiten.  Die gesamte Kommunikation zwischen der App des Programmierers und den Adobe Pass-Authentifizierungsservern erfolgt über den Access Enabler.  Für die folgenden Plattformen ist Access Enabler in einer anderen Variante verfügbar: JavaScript, iOS, tvOS, Android und FireTV.

Obwohl es möglich ist, die REST-API direkt auf Client-Plattformen zu verwenden, die native SDKs außerhalb einer Server-zu-Server-Lösung unterstützen, wird dieser Ansatz nicht empfohlen.



## REST API Pros und Nachteile {#ProsAndCons}

Die Adobe Pass Authentication REST API wurde erstellt, um eine TV Anywhere (TVE)-Lösung für Geräte ohne Web-Browsing-Funktionen oder beständigen Speicher bereitzustellen. Die REST-API bietet Unterstützung für alle Authentifizierungs- und Autorisierungsflüsse, da sie jedoch keine native SDK-Komponente enthält. Die von der Adobe Pass-Authentifizierung bereitgestellten und gepflegten SDK enthalten vordefinierte Funktionen, die Geschäftsregeln implementieren, die im Falle der REST-API von den Programmierern implementiert und gepflegt werden müssen. In der folgenden Tabelle mit den Programmiererverantwortlichkeiten werden die Einschränkungen der aktuellen REST-API beschrieben, die von Programmierern angegangen werden müssen.



### Server-zu-Server im Vergleich zu Client-basierten Vor- und Nachteile

Eine Server-zu-Server-Architektur bietet eine Möglichkeit, den Großteil der authentifizierungs- und autorisierungsbezogenen Logik in einer logischen Einheit oder Implementierung zu bündeln.  Dieser Ansatz hat Vor- und Nachteile.  Die Vorteile umfassen:

* Einzelimplementierung für die Authentifizierung und Autorisierung der Geschäftslogik.
* Vermeiden Sie die Implementierung dieser Logik auf jeder unterstützten Plattform mit diesen nativen Tools.
* Möglichkeit, Funktionen zu aktualisieren, ohne Clients mit allen zugehörigen Anforderungen (z. B. App Store-Updates) zu aktualisieren.
* **Einfacher** Erweitern und erweitern Sie benutzerdefinierte Funktionen von authN und authZ (z. B. Hinzufügen von D2C).
* Direktes Management des damit verbundenen Traffics für eine bessere Kontrolle, Qualität und Überwachung.



Auch hier sind die Symbole in den Verantwortlichkeiten des Programmierers aufgeführt, umfassen jedoch Folgendes:

* SSO muss für jeden Client für Plattformen ohne Platform SSO implementiert werden.
* Programmierer müssen bei Bedarf MVPD-spezifische Logik implementieren.
* Alle Plattformen, die die REST-API verwenden, verwenden eine einzige Konfiguration, die Eigenschaften wie Authentifizierungs-TTLs regelt.



### Angeschlossene Geräte

Bei den meisten vernetzten Geräten muss die REST-API auf die eine oder andere Weise verwendet werden, da ein SDK nicht verfügbar ist. Das angeschlossene Gerät verwendet entweder die REST-API direkt oder integriert in eine Server-zu-Server-Lösung, die die REST-API verwendet.

## Programmiereraufgaben {#programmer-responsibilities}

Folgendes gilt sowohl für Server-zu-Server- als auch für Connected Device-Anwendungen.

| **Funktionalität** | **Verantwortlich für die Verarbeitung der Funktion** | **Beschreibung der Einschränkungen der aktuellen clientlosen API und Unterschiede zu nativen SDKs** |
| --- | --- | --- |
| Von Platform angewendete Konfigurationseinstellungen | Adobe | Eine **wesentliche Einschränkung** bei der Verwendung der REST-API für alle Plattformen (einschließlich Mobilgeräten wie iOS und Android) besteht darin, dass die Konfigurationseinstellungen, die der REST-API in unserem TVE Dashboard-Konfigurationstool entsprechen, auf allen Geräten angewendet werden (selbst wenn ein iOS-Gerät vorhanden ist, auf dem eine native Anwendung zusätzlich zu unserer REST-API implementiert ist). Diese Beschränkung **kann** die vereinbarten TTLs und vereinbarten Plattformeinstellungen mit den MVPDs unterbrechen - wenn diese je nach Plattform unterschiedlich sind. [1](#1) |
| Single Sign-on | Programmierer | Bei Verwendung der REST-API ist SSO nur auf Plattformen verfügbar, die Platform SSO unterstützen (z. B. Apple, Roku, Amazon), während SSO bei Verwendung der REST-API für andere Plattformen nicht garantiert werden kann. Die SDK speichern Daten Site-/App-übergreifend zwischen. Das bedeutet, dass sich der Benutzer einmal auf einer Site/App anmeldet und bereits auf den teilnehmenden Sites angemeldet ist, ohne dass eine Benutzerinteraktion erforderlich ist. [2](#2) |
| Single Logout | Programmierer | In einem nativen SDK-SSO-Szenario meldet sich der Benutzer bei einer teilnehmenden Anwendung von überall ab. Bei der aktuellen REST-API unterstützen wir SLO nicht. Wenn Sie sich von einer Anwendung abmelden, wird der Benutzer nur für diese Anwendung abgemeldet. |
| Zwischenspeicherung | Programmierer | Die REST-API-Implementierungen müssen einen eigenen Caching-Mechanismus für geschäftsvereinbarte Datenelemente implementieren. Die SDKs speichern unter Berücksichtigung verschiedener Geschäftsregeln automatisch verschiedene Datenelemente zwischen. Beispielsweise werden die Benutzermetadaten mit derselben TTL wie das Authentifizierungstoken zwischengespeichert, während einige Elemente programmgesteuert vom Caching ausgeschlossen werden können (Preflight). |
| Detaillierter Fehlermeldungsmechanismus | Programmierer | Die REST-API stützt sich bei der Berichterstellung von Anwendungsfehlern hauptsächlich auf HTTP-Fehlercodes, während SDKs über einen detaillierten Fehlermeldungsmechanismus verfügen, der den Anwendungsentwicklern dabei hilft, besser zu verstehen, was passiert. |
| Wiederherstellung von Anwendungsfehlern (erneuter Versuch bei Fehlern, Erkennung von Schleifen usw.) | Programmierer | REST-API-Implementierungen müssen eigene Anwendungswiederherstellungssysteme erstellen, während Implementierungen zusätzlich zu SDKs vom Fehlerbehebungssystem der SDKs profitieren: Wiederherstellung von vorübergehenden Netzwerkfehlern durch Wiederholung bestimmter Netzwerkaufrufe mit einer vorhandenen Logik, um &quot;Schleifen&quot;zu verhindern. |
| Authentifizierung pro Anfrage | Programmierer | Einige MVPDs erfordern eine Authentifizierung für jede Site/App, entweder aus geschäftlichen Gründen oder aufgrund technischer Herausforderungen. Die SDKs erzwingen dies automatisch anhand der TVE-Dashboard-Konfiguration. Die REST-API-Implementierer müssen dies selbst implementieren, um Geschäftsvereinbarungen nicht zu verletzen oder um die Autorisierung für die MVPDs, die ihre Authentifizierungsdaten pro Anwendung abdecken, abzuschließen. |
| Passive Authentifizierung | Programmierer | Einige MVPDs unterstützen die &quot;passive&quot;Authentifizierung, bei der der Benutzer die Anmeldeinformationen nicht eingeben muss und der Versuch, den Benutzer zu authentifizieren, automatisch erfolgt. Dies ist besonders für MVPDs nützlich, die auch &quot;Authentifizierung pro Anfragender&quot;als Anforderung aufweisen. In einem solchen Fall ist die von den SDKs aktivierte UX nahtlos, wenn sich der Benutzer nur einmal in einer Anwendung authentifiziert und das SDK eine &quot;passive&quot;Authentifizierung für andere Anwendungen im Ökosystem durchführt. Der Benutzer sieht die zusätzlichen passiven Aufrufe nicht, er wird einfach bereits authentifiziert sein, während der MVPD das Ziel erreicht, separate Authentifizierungssitzungen für jede Anwendung zu haben. |
| Implizite und einheitliche Geräteerkennung | Programmierer | Die SDKs erkennen den Gerätetyp automatisch und senden diese Informationen standardmäßig. REST-API-Implementierungen neigen dazu, je nach Implementor unterschiedliche Informationen zu senden, wodurch Geschäftsregeln und Statistiken über Sites hinweg verzerrt/verletzt werden. **Programmierer müssen sicherstellen, dass sie uns die richtigen Geräteinformationen** in jeder ihrer Apps geschickt haben. Bei Server-zu-Server-Implementierungen identifiziert die REST-API die IP-Adresse des Endbenutzers nicht ordnungsgemäß, es sei denn, es werden zusätzliche Schritte unternommen. Die IP-Adresse ist in bestimmten Anwendungsfällen wichtig, wie z.B. bei der Betrugsbekämpfung oder HBA. |
| Tamper-Testversand TempPass | Programmierer | Die Durchsetzung von TempPass basiert auf einer stabilen Geräte-ID. Die SDK verwenden Hardware-Info-/Fingerabdrucktechniken, um das Gerät zu identifizieren. Dieser Mechanismus ist nicht öffentlich, daher sicherer und kollisionsfrei. Für REST-API-Implementierungen müssen die Programmierer eigene Algorithmen für die Geräteerkennung/den Fingerabdruck implementieren. |
| Gerätebindung | Programmierer | Token, die auf einem Gerät mit einer Anwendung über SDK generiert werden, sind nicht portabel. Dies erschwert es böswilligen Benutzern, ihre Token zu teilen und anderen Benutzern Zugriff zu gewähren. Dies basiert auf demselben Geräte-ID-Mechanismus wie der &quot;manipulationssichere TempPass&quot;. Bei der REST-API bleiben die Token in der Cloud, was bedeutet, dass zwei Geräte mit denselben Geräte-IDs, die dieselben Aufrufe ausführen, dieselben Antworten/Zugriffe erhalten. Programmierer müssen sicherstellen, dass sie über einen robusten, sicheren und kollisionsfreien Mechanismus zum Generieren/Zuweisen von Geräte-IDs verfügen. |
| Vorhersehbare und zertifizierte Funktionen | Programmierer | Die vorhandenen Funktionen in jedem SDK sind konsistent, vorhersehbar und vollständig zertifiziert und getestet. Einige Geschäftsanforderungen werden über die SDKs durchgesetzt, wodurch es für den Programmierer riskant wird, Verträge bei Verwendung der REST-API zu brechen. Während die REST-API möglicherweise flexibler ist, garantiert Adobe, dass die SDK-Implementierungen Geschäftsentscheidungen und Einstellungen über das TVE-Dashboard erzwingen. Im Falle von ClientLess implementiert jeder Programmierer seine eigene Untergruppe der Geschäftsregeln, die seine Anwendungen zu einem bestimmten Zeitpunkt anordnen. |


1. Im Rahmen unserer neuen One API-Initiative planen wir, diese Einschränkung zu beheben und die Möglichkeit zu bieten, Regeln pro Plattform auf der Grundlage der Geräterkennung anzuwenden.

2. Adobe arbeitet weiterhin mit allen wichtigen Plattformen zusammen, um Platform SSO zu implementieren, die mit unserer REST-API verwendet werden kann. Unsere One API-Initiative bietet SSO-Unterstützung zwischen Apps, die mit nativen SDKs und Apps implementiert wurden, die mithilfe der REST-API implementiert wurden.

## Mindestanforderungen an Geräte {#min_reqs}

Um die Adobe Pass Authentication REST API verwenden zu können, müssen Geräte die im Abschnitt REST API des Dokuments [Adobe Pass Authentication Platform/Device/Tools Requirements](#general_clientless_reqs) der Dokumentation Authentication Platform/Tools Requirements‘ aufgelisteten technischen Mindestanforderungen erfüllen oder überschreiten.
