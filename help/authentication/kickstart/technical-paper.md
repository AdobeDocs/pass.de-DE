---
title: Über Adobe Pass-Authentifizierung und TV Everywhere
description: Über Adobe Pass-Authentifizierung und TV Everywhere
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '6288'
ht-degree: 0%

---

# Über Adobe Pass-Authentifizierung und TV Everywhere {#about-auth-tve}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Über TV Everywhere {#about-tve}

Die Fernsehzuschauer von heute können zu jeder Zeit und an jedem Ort online gehen, und sie erwarten, dass ihre Möglichkeiten, auf Pay-TV-Inhalte zuzugreifen, genau dort sein werden, wo sie auch sind. Darüber hinaus verwenden Zielgruppen zur Anzeige von Inhalten eine ständig wachsende Auswahl an Internet-fähigen Geräten, darunter:

* Notebooks
* Tablets
* Smartphones
* Websites
* Federated Apps
* Spielekonsolen
* Set-Top-Boxen
* Smart-TVs

TV Everywhere ist die Branchenbewegung, die die Fähigkeit von Pay-TV-Abonnenten unterstützt, auf dieselben Inhalte zuzugreifen, für die sie bereits bezahlen, und zwar auf mehreren Geräten sowohl innerhalb als auch außerhalb ihres Hauses. Während der Großteil des Fernsehkonsums noch immer im konventionellen, linearen Fernsehen stattfindet, ist der Konsum bei zeitverschobenen Inhalten, Online-Videos und alternativen Bildschirmen gestiegen. Infolgedessen befindet sich der Videovertriebsmarkt heute in einer Umbruchsituation und TV Everywhere hat sich als die Lösung etabliert, die die Interessen von Programmierern, Pay-TV-Anbietern und Pay-TV-Abonnenten miteinander in Einklang bringt.


Das technische Ziel von TV Everywhere besteht darin, es Pay-TV-Kunden zu ermöglichen, auf Inhalte zuzugreifen, die sie bereits auf all ihren Geräten und Plattformen abonniert haben.


Die Geschäftsziele von TV Everywhere sind:

* **Bestehende Kundenbeziehungen beibehalten und neue aktivieren**
* Ermöglichen Sie es Programmierern und Inhaltsverantwortlichen, die breiteste Zielgruppe zu erreichen und mehr Wert aus Premium-Inhalten zu gewinnen
* Marken durch direkte Online-Interaktion mit Betrachtern erweitern

## Herausforderungen bei TV Everywhere {#tve-challenges}

Zusammen mit den Möglichkeiten von TV Everywhere kommen Herausforderungen. Im Mittelpunkt dieser Rechte steht das Recht. Bevor ein Betrachter auf Abonnementinhalte zugreift, muss jemand feststellen, ob ihm dieser Zugriff gewährt werden darf.

Besitzt der Benutzer ein Abonnement bei einem Pay-TV-Anbieter? Wenn ja, enthält dieses Abonnement den angeforderten Inhalt? Für Programmierer und Inhaltsbesitzer ist die direkte Feststellung von Berechtigungen besonders schwierig, da es die Pay-TV-Anbieter sind, die über die Identifizierungsdaten für ihre Kunden sowie über die Zugriffsberechtigungen ihrer Kunden verfügen.


Neben den Berechtigungen gibt es eine Vielzahl damit verbundener technischer und Integrationsherausforderungen, darunter:

* Entwicklung und Umsetzung einer umfassenden Strategie für mehrere Geräte
* Koordination der unzähligen Beziehungen zwischen Programmierern und Pay-TV-Anbietern
* Verhinderung von betrügerischem Zugriff oder Missbrauch von Nutzungsbedingungen
* Bereitstellen eines konsistenten und frustrierfreien Authentifizierungserlebnisses für Benutzer über Websites und Apps hinweg
* Einfache Time-to-Market, um mit Affiliate-Angeboten Schritt zu halten
* Verwaltung der Kosten für mehrere Integrationen

Diese Herausforderungen machen die Durchführung und Pflege komplexer, direkter Integrationen zwischen Programmierern und den Authentifizierungssystemen mehrerer Pay-TV-Anbieter äußerst ressourcenintensiv und erfordern Zeit und technische Raffinesse.


Die Lösung? **Adobe® Authentifizierung bestehen**.

## Einführung in die Adobe Pass-Authentifizierung {#authentication-intro}

Bei der Adobe Pass-Authentifizierung müssen Programmierer und Pay-TV-Anbieter nur eine einfache Integration durchführen, indem sie Adobe Pass-Authentifizierungs-APIs verwenden, um Zugriff auf das gesamte Ökosystem zu erhalten, einschließlich:

* Programmierer wie Turner Broadcasting (TBS, TNT, CNN), Fox Broadcast Networks und Hulu

* Alle führenden Pay-TV-Anbieter in den USA, die mehr als 90 % aller Pay-TV-Haushalte in den USA ausmachen

Darüber hinaus bietet die Adobe Pass-Authentifizierung das Framework, das die Benutzerauthentifizierung und -autorisierung einfach und sicher macht.

![](../assets/programmers-connect-authn.png)

*Abbildung 1: Nur einige der Programmierer und Pay-TV-Anbieter, die über die Adobe Pass-Authentifizierung eine Verbindung herstellen…*

Adobe Pass vermittelt Berechtigungstransaktionen sicher zwischen Programmierern und Pay-TV-Anbietern, wodurch der Zuschauerzugriff auf Abonnementinhalte erleichtert wird. Oder mit anderen Worten…

**Die Adobe Pass-Authentifizierung erleichtert und beschleunigt den Zugriff der richtigen Kundinnen und Kunden auf die richtigen Inhalte.**

**Für wen ist die Adobe Pass-Authentifizierung?**

* **Programmierer** die sich problemlos mit Pay-TV-Anbietern (auch als „MVPDs“ oder „Multichannel Video Programming Distributors“ bezeichnet) integrieren lassen möchten, um die größte Zielgruppe zu erreichen und so optimale Umsätze zu erzielen. Mithilfe der Adobe Pass-Authentifizierung können Programmierer Viewer bei allen wichtigen Anbietern authentifizieren, unabhängig von der Client-Plattform.

* **Pay-TV-Anbieter/-MVPDs** die schmerzlose Konnektivität mit mehreren Programmierern und eine höhere Kundenzufriedenheit suchen, indem sie den Zugriff auf Abonnementinhalte online erleichtern.

* **Pay-TV** Kunden, die einen einfachen Zugang zu Inhalten wünschen, die sie bereits abonniert haben, unabhängig davon, wo sie sich befinden, ohne zusätzliche Kosten. Single Sign-on bietet eine sichere Viewer-Authentifizierung im Web oder über Mobile Apps hinweg, ohne dass Client-Downloads oder wiederholte Anmeldungen erforderlich sind, sowie ein gutes Benutzererlebnis.

Für **Programmierer** bietet die Adobe Pass-Authentifizierung:

* Einfache Integration und sofortige Konnektivität mit führenden Pay-TV-Anbietern, ohne die Last mehrerer, direkter Integrationen
* Optimierung sowohl des Abonnement- (Lizenzierungs-) als auch des Werbeeinkommens durch Unterstützung der größtmöglichen Zielgruppen für Inhalte
* Sichere Authentifizierung mit Zugriff auf Premium-Inhalte, die nur autorisierten Benutzern/Geräten gewährt wird
* Ein offenes und flexibles Framework, das sowohl Player- als auch DRM-Plattformunabhängig ist. Die Wiedergabe kann auf einer Vielzahl von Plattformen erfolgen, einschließlich iOS, Android, Windows 8, Spielkonsolen, Set-Top-Boxen und mehr.
* Kompatibilität mit jeder DRM-Technologie, z. B. Adobe Flash Acceß ® oder Play Ready®.
* Unterstützung für Single-Sign-On (SSO)-Authentifizierung und -Autorisierung, sodass sich Abonnentinnen und Abonnenten nach der ersten Authentifizierung auf ihrem eigenen System nicht erneut anmelden müssen.


Für **Pay-TV-Anbieter/MVPDs** bietet die Adobe Pass-Authentifizierung:

* Einfache Integration mit Inhaltsverantwortlichen für sofortige Konnektivität mit mehreren Programmierern über eine einzige Integration
* Verbesserte Kundeninteraktion durch Unterstützung eines reibungslosen Markenerlebnisses bei der Anzeige von Inhalten auf mehreren Plattformen und Geräten
* Sichere Authentifizierung, die sicherstellt, dass nur autorisierte Benutzer/Geräte Zugriff auf Premium-Inhalte erhalten, und (optional) die Anzahl der Geräte und gleichzeitigen Streams begrenzt, die pro Haushaltskonto eine Verbindung herstellen können.


Für **Pay-TV** Kunden bietet die Adobe Pass-Authentifizierung:

* **TV Everywhere!**

Der Rest dieses Artikels bietet eine technische Einführung in die Adobe Pass-Authentifizierung.  Während sich viele der folgenden Themen auf die Programmiererintegration konzentrieren, gibt es sowohl allgemeine als auch spezifische Informationen, die auch für Pay-TV-Anbieter gelten. In diesem Dokument wird auch die Sicherheit und Integrität der Funktionsweise der Adobe Pass-Authentifizierung als Lösung für TV Everywhere hervorgehoben. Für weitere Informationen über dieses Dokument hinaus wenden Sie sich an Ihren Adobe-Support-Mitarbeiter oder füllen Sie das Informationsanfrageformular ([) ](https://www.adobe.com/).

## Bausteine {#arch-building-blocks}

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/import-pc7mz3dfnv/check.gif) Im Folgenden werden die zentralen Berechtigungstransaktionen für Authentifizierung und Autorisierung erläutert. Bei der Authentifizierung wird einem Pay-TV-Anbieter bestätigt, dass ein bestimmter Benutzer ein bekannter Kunde ist. Autorisierung ist der Prozess, bei dem ein Pay-TV-Anbieter bestätigt, dass ein authentifizierter Benutzer über ein gültiges Abonnement für eine bestimmte Ressource verfügt.
Die Adobe Pass-Authentifizierung besteht aus den folgenden Grundkomponenten:

* Client-Komponente (eine der folgenden):

   * Der Access Enabler - eine plattformspezifische Bibliothek; bietet benutzerfreundliche APIs und Code-Beispiele zur Implementierung der Berechtigungsflüsse
   * Die Clientless-API - RESTful-Web-Services; stellt Endpunkte für Berechtigungsflüsse für Plattformen ohne Website-Rendering-Funktionen bereit (z. B. Spielekonsolen, Set-Top-Boxen usw.),

* Adobe-gehostete Backend-Server
* Media Token Verifier
* Ein sicheres, zentrales Medium für den Austausch (Token)

Auf allgemeiner Ebene besteht die Adobe Pass-Authentifizierung aus drei Komponenten (dem Access Enabler, Adobe-gehosteten Backend-Servern und dem Media Token Verifier) und einem zentralen Exchange-Element (Token).

### Client-Komponenten {#client-components}

* Access Enabler
* Clientless-API

#### Access Enabler {#access-enabler}

Auf vollständig unterstützten Plattformen (einschließlich Web, iOS, Android, Windows 8) interagieren Programmierer über die Access Enabler-Client-Komponente mit der Adobe Pass-Authentifizierung. Diese Komponente erleichtert alle Authentifizierungs- und Autorisierungsinteraktionen mit dem Kunden.  Access Enabler wird lokal auf ihrem System ausgeführt. Wenn ein(e) Benutzende(r) auf eine Programmierer-Site oder ein Programm zugreift und Inhalte anfordert, wird die Adobe-gehostete/gepflegte Access Enabler-Komponente im Hintergrund geladen.

Der Access Enabler verwaltet die tatsächlichen Berechtigungs-Workflows, während der Programmierer die Verantwortung für die übergeordnete Web-Seite oder Player-Anwendung behält, die die Benutzeroberfläche implementiert und mit dem Access Enabler interagiert. Diese Interaktionen erfolgen über ein asynchrones System von Funktionen und Callbacks, die von der Access Enabler-API definiert werden.

Dies sind die grundlegenden Berechtigungsflüsse, die mithilfe der Access Enabler-API einfach zu implementieren sind:

* Anfordereridentität (Programmierer) festlegen
* Benutzerauthentifizierung mit einem bestimmten Pay-TV-Anbieter (dem „Identitätsanbieter„) prüfen/einholen
* Überprüfen/Abrufen der Benutzerautorisierung für eine bestimmte Ressource
* Abmelden des Benutzers

Access Enabler bietet außerdem die folgenden Services:

* Es validiert Abfragen vom Programmierer, einschließlich des Registrierungsstatus bestimmter Clients, ihrer Domains und ihrer Ressourcen/Kanäle.
* Es liefert die Daten, die die Liste der Pay-TV-Anbieter erstellen, aus denen der Nutzer seinen Anbieter auswählt. Diese Liste wird ebenfalls validiert und für den Programmierer, von dem die Anfrage stammt, entsprechend definiert.
* Sie initiiert für Pay-TV-Betreiber spezifische Authentifizierungs- und Autorisierungs-Workflows.
* Die erfolgreichen Autorisierungsantworten werden pro Programmiererressource/Kanal zwischengespeichert, um unnötigen Anfragedatenverkehr zu minimieren.
* Er kann für vordefinierte Workflows konfiguriert werden, die für jeden Pay-TV-Benutzer spezifisch sind, z. B. für die explizite Geräteregistrierung.

Je nach Website oder Player-Anwendung kann Access Enabler die folgenden Formen annehmen:

* Eine SWF-Datei, die die Flash Player-Laufzeit ausführen kann
* JS-Datei, die direkt vom Browser ausgeführt wird
* Ein nativer Access Enabler für unterstützte Plattformen (einschließlich iOS, Android und Windows 8)

#### Clientless-API {#clientless-api}

Der Client-lose API-Ansatz eignet sich für „intelligente Geräte“ (Spielekonsolen, Set-Top-Boxen und Smart-TVs), die keine Webbrowser unterstützen (erforderlich für die Authentifizierung mit MVPDs).  Beim Client-losen Ansatz kommunizieren Apps für intelligente Geräte direkt mit der Adobe Pass-Authentifizierung über RESTful-Webservices-APIs für alles, mit Ausnahme der Authentifizierung, die in einer zweiten Bildschirm-(Browser-)App durchgeführt wird. Mit anderen Worten, die Client-seitige Bibliothek von Access Enabler wird nicht verwendet. Stattdessen verwenden Entwickler von Apps für intelligente Geräte direkt die Web-Services-APIs für die Adobe Pass-Authentifizierung, um die Berechtigungsflüsse zu implementieren.

### Adobe-gehostete Backend-Server {#adobe-backend-servers}

Die Adobe Pass-Authentifizierungs-Backend-Server, gehostet von Adobe:

* Bereitstellen der Authentifizierungs- und Autorisierungs-Workflows mit den Pay-TV-Anbietern, die eine Server-zu-Server-Kommunikation zwischen der Adobe Pass-Authentifizierung und dem Operator erfordern.
* Verwalten Sie die Konfiguration für Programmierer-Sites und -Programme.
* Hosten Sie die herunterladbaren Komponentendateien des Access Enabler.
* Stellen Sie die RESTful-Webservice-Endpunkte für die Client-lose API-Integration bereit.
* Generieren von Authentifizierungs- und Autorisierungs-Token (und in einigen Fällen Speichern).

### Token und der Media Token Verifier {#tokens-media-token-verifier}

Die Adobe Pass-Lösung für Authentifizierungsberechtigungen konzentriert sich auf die Generierung bestimmter Datenelemente, die nach erfolgreichem Abschluss der Authentifizierungs-/Autorisierungs-Workflows abgerufen werden. Diese Daten werden als Token bezeichnet. Sie haben eine begrenzte Lebensdauer und werden sicher gespeichert, entweder an plattformabhängigen Stellen auf dem Client oder auf Adobe-Servern im Fall der Client-losen API-Lösung. Nach Ablauf müssen Token durch die erneute Initiierung der Authentifizierungs- und/oder Autorisierungs-Workflows erneut ausgestellt werden.

Es gibt drei Arten von Token, die bei der Adobe Pass-Authentifizierung während der Authentifizierungs-/Autorisierungs-Workflows auftreten. Zwei davon sind „langlebig“ und bieten Kontinuität im Anwendererlebnis. Das dritte, ein kurzlebiges Token, unterstützt die Best Practices der Branche zur Betrugsbekämpfung (wo Betrug beispielsweise Exploits wie Stream-Ripping umfasst). Die TTL-Werte (Time-to-Live) werden auf der Grundlage von Vereinbarungen zwischen Programmierern und Pay-TV-Anbietern festgelegt, die sich auf einen Wert einigen, der allen Beteiligten am besten dient.

#### (Langlebiges) Authentifizierungstoken {#long-lived-auth-token}

Die Authentifizierung wird erfolgreich durchgeführt, sobald eine Kundin oder ein Kunde die Adobe Pass-Authentifizierung verwendet, um sich erfolgreich bei ihrem Pay-TV-Konto anzumelden. Die Adobe Pass-Authentifizierung erzeugt dann ein langlebiges Authentifizierungstoken (AuthN), das mit dem anfragenden Gerät verknüpft ist, und (je nach Pay-TV-Anbieter) eine global eindeutige Kennung („GUID„), die den Benutzer anonym identifiziert.

* Die Adobe Pass-Authentifizierung speichert das AuthN-Token sicher an einem Speicherort, an dem es für alle Anwendungen verfügbar ist, die die Adobe Pass-Authentifizierung verwenden. Bei Access Enabler-Integrationen werden Token sicher auf der Client-Seite gespeichert.  Die Adobe Pass-Authentifizierung verwendet das AuthN-Token, um nachfolgende Autorisierungsabfragen im Namen der Benutzerin bzw. des Benutzers durchzuführen.
* Es wird immer nur ein Authentifizierungs-Token gespeichert. Wenn ein neues AuthN-Token ausgegeben wird und bereits ein altes vorhanden ist, überschreibt das neue Token den vorhandenen gespeicherten Wert.

#### (Langlebiges) Autorisierungs-Token {#long-lived-authriz-token}

Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung ein langlebiges Autorisierungs-Token („AuthZ„). Dieses Token ist nicht portabel, da es an das anfordernde Gerät und eine bestimmte geschützte Ressource (z. B. einen Kanal, eine Serie oder eine Folge) gebunden ist.

* Die Adobe Pass-Authentifizierung speichert das AuthZ-Token sicher zusammen mit anderen Autorisierungs-Token für andere Ressourcen.  Wie bei den AuthN-Token wird bei Plattformen, die Access Enabler verwenden, das Token lokal auf dem Client gespeichert. Bei Plattformen, die die Clientless-API verwenden, werden Token auf den Adobe Pass-Authentifizierungsservern gespeichert.
* Die Time-to-Live (TTL) des langlebigen AuthZ-Tokens wird in der Regel im Bereich von Tagen bis Wochen definiert, je nach der spezifischen Vereinbarung zwischen dem Pay-TV-Anbieter und dem Programmierer.
* Pro Ressource wird immer nur ein AuthZ-Token gespeichert. Es können mehrere Autorisierungs-Token gespeichert werden, sofern sie mit verschiedenen Ressourcen verknüpft sind. Wenn ein neues Autorisierungs-Token ausgegeben wird und bereits ein altes für dieselbe Ressource vorhanden ist, überschreibt das neue Token den vorhandenen zwischengespeicherten Wert.
* Die Adobe Pass-Authentifizierung verwendet das langlebige AuthZ-Token, um die kurzlebigen Medien-Token zu erstellen, die für den tatsächlichen Anzeigezugriff verwendet werden.

#### Kurzlebiges Medien-Token {#short-lived-media-token}

Sobald die Adobe Pass-Authentifizierung das AuthZ-Token generiert, verwendet sie dieses Token, um ein einmaliges, kurzlebiges Medien-Token zu generieren, das von Adobe signiert und verschlüsselt wird, um Manipulationen während des Austauschs zu vermeiden:

* Die TTL des kurzlebigen Tokens (Standard: 5 Minuten) ist so eingestellt, dass Probleme bei der Uhrensynchronisierung zwischen dem Server, der das Token generiert, und dem Server, der das Token validiert, möglich sind.
* Das kurzlebige Token wird auf der Einbettungs-Site verfügbar gemacht, bevor der Zugriff auf die geschützte Ressource bereitgestellt wird. Daher muss der Programmierer das Token mithilfe des Media Token Verifier für Access Enabler-Integrationen oder, im Fall von Client-losen API-Integrationen, des Token Verifier-Services validieren.

#### Medien-Token-Verifizierer {#media-token-verifier}

Programmierer sind dafür verantwortlich, die Media Token Verifier-Bibliothek in ihren vorhandenen Anwendungs-Server zu integrieren, damit der Verifier die endgültigen Benutzervalidierungen durchführen kann, bevor ein Video-Stream tatsächlich gestartet wird. Die Media Token Verifier-Bibliothek definiert:

* Eine Token-Verifizierungs-API, die Informationen aus dem Token abruft, z. B. ob es gültig ist, wann das Token ausgegeben wurde und andere relevante Daten
* Der öffentliche Adobe-Schlüssel, mit dem überprüft wird, ob das Token tatsächlich von Adobe stammt
* Eine Referenzimplementierung, die zeigt, wie die Verifier-API verwendet wird und wie der in der Bibliothek enthaltene öffentliche Adobe-Schlüssel zur Überprüfung seiner Herkunft verwendet wird

![](../assets/high-level-architecture-nflows.png)

*Abbildung 2: Allgemeine Architektur des Adobe Pass-Authentifizierungs-Ökosystems in einer Access Enabler-Integration*

## Integration mit der Adobe Pass-Authentifizierung {#integrate-auth}

Unabhängig davon, ob Sie Pay-TV-Anbieter oder Programmierer sind, erfordert der Integrationsprozess mit der Adobe Pass-Authentifizierung einen gewissen Anteil Ihrer aktiven Teilnahme. Jeder dieser Prozesse wird im Folgenden beschrieben.

### Der Prozess für Pay-TV-Anbieter

Die Hauptverantwortung des Pay-TV-Anbieters bei der Adobe Pass-Authentifizierung besteht darin, zu überprüfen, ob ein anfragender Nutzer tatsächlich ein bekannter Abonnent ist, der zum Zugriff auf die Inhalte des Programmierers berechtigt ist. Auf Hochtouren
Auf dieser Ebene erfordert der Adobe Pass-Authentifizierungsprozess für die Integration mit einem neuen Pay-TV-Anbieter die folgenden Schritte:

1. Der Anbieter unterzeichnet die Adobe Pass Authentication Non-Disclosure Agreement (NDA).
1. Der Anbieter stellt Adobe Spezifikationen für das Authentifizierungs- und Autorisierungssystem zur Verfügung. Für die einfachste Integration wird empfohlen, dass Pay-TV-Anbieter einen SAML-basierten Identitätsanbieter (IdP) für die Authentifizierung und die Möglichkeit zur Kommunikation über das SOAP-Zugriffsprotokoll zur Autorisierung haben.
1. Der Anbieter stellt die Verbindung zwischen seinen Servern und den Adobe Pass-Authentifizierungsservern her. Dazu gehören die Bereitstellung von Endpunkten und die Auflistung von IPs.
1. Präqualifikationsfreigabe und QE.
1. Produktionsfreigabe und QE.

Während die Adobe Pass-Authentifizierung bestehende Integrationen für Programmierer ersetzen kann, ist dies für Pay-TV-Anbieter in der Regel nicht erforderlich. Adobe arbeitet mit dem technischen Team des Anbieters zusammen, um die Adobe Pass-Authentifizierung so zu konfigurieren, dass sie den Anforderungen vorhandener Integrationen entspricht. Die Integration ist für Pay-TV-Anbieter kostenlos, vorausgesetzt, es handelt sich um eine „Standardintegration“ und minimale Support-Anforderungen (Dokumentation und grundlegender E-Mail-Support). Wenn ein Provider erheblichen Support oder einen eskalierten Zeitplan benötigt, kann eine Support-Gebühr erhoben werden oder der Provider möchte mit einem Drittanbieter zusammenarbeiten, der mit unserer Lösung vertraut ist, wie z. B. Synacor.


Die Adobe Pass-Authentifizierung unterstützt auch die effiziente Handhabung der Geschäftslogik von Pay-TV-Anbietern wie folgt:

* Für eine eigenständige Geschäftslogik, die vom Benutzer angewendet werden kann, wenn eine Autorisierungsanfrage eingeht, stellt Adobe die erforderlichen Daten bereit, um die Durchsetzung der Geschäftslogik zu unterstützen, wenn der Benutzer eine Autorisierungsanfrage erhält. Zu diesen Daten können u. a. die eindeutige Geräte-ID des Benutzers, der die Anfrage stellt, und die IP-Adresse des Geräts gehören.
* Für eine Geschäftslogik, die ein Eingreifen des Benutzers und/oder eine spezifische Handhabung durch die Adobe-Lösung erfordert, kann Adobe einige benutzerdefinierte Eigenschaften für jeden Pay-TV-Anbieter beibehalten. Zu diesen benutzerspezifischen Konfigurationen/Richtlinien gehört die Aktivierung vordefinierter Workflows, die an bestimmten Punkten des Workflows der obersten Ebene gestartet werden können. Weitere Informationen zur Unterstützung benutzerdefinierter Eigenschaften erhalten Sie von Ihrem Adobe-Support-Mitarbeiter.

Adobe bietet außerdem Betrugsbekämpfungsdienste an. Weitere Informationen erhalten Sie von Ihrem Adobe-Support-Mitarbeiter.

### Der Programmierprozess {#programmer-process}

Um die Adobe Pass-Authentifizierung erfolgreich zu integrieren, müssen Programmierer ihre Media Player-Anwendung oder Web-Seite so einrichten, dass sie mit der Adobe Pass-Authentifizierung bei der Verarbeitung der wichtigsten Berechtigungsprozesse zusammenarbeitet: Authentifizierung, Autorisierung und Abmeldung.


Bevor Sie eine Integration mit der Adobe Pass-Authentifizierung beginnen, sollten Programmierer über Folgendes verfügen:

* Eine vorhandene Online-Videoplattform, einschließlich eines Medien-Players, entweder als Teil einer Website oder als eigenständige Anwendung
* Ein Content Management System
* Ein Bereitstellungsmechanismus, der ein Content Delivery Network (CDN) eines Drittanbieters enthalten kann oder nicht

Programmierer sollten erwarten, einige Integrationsaufgaben als Teil der Bereitstellung von TV Everywhere-Services mit Adobe Pass-Authentifizierung durchzuführen. Zu diesen Aufgaben gehören:

* Integrieren der Access Enabler-Bibliothek der Adobe Pass-Authentifizierung in Ihre Web-Seite oder Ihren Media Player oder Implementieren der Integration mithilfe des Client-losen Ansatzes für „intelligente Geräte“, die nicht Web-fähig sind
* Serverseitige Arbeit zur Integration der Adobe Pass Authentication Token Verifier-Komponente in Ihren Video-Streaming-Workflow
* Erstellen einer Benutzeroberfläche für den Zugriffs-Workflow in Ihrer Website oder Ihrer Mobile App (einige Elemente davon, wie der eigentliche Anmeldevorgang, werden vom Pay-TV-Benutzer bereitgestellt, und einige Elemente sind optional als Teil der Adobe Pass-Authentifizierung verfügbar)

Dieses Dokument bietet einen Überblick über den Programmierprozess, und Adobe bietet zusätzliche Anleitungen bei der formellen Initiierung der Integration.

#### Einrichtung des Anforderers (Programmierers) {#requester-prog-setup}

##### Registrieren bei Adobe {#registering}

Als ersten Schritt müssen sich Programmierer bei Adobe oder einem Adobe-autorisierten Partner registrieren und die Domains angeben, die sie mit der Adobe Pass-Authentifizierung verwenden möchten. Programmierer erhalten dann für jede Sitzung, in der der Programmierer mit dem Access Enabler interagiert, eine eindeutige Anforderer-ID, die der Adobe Pass-Authentifizierung bereitgestellt wird.

##### Setup für die Integration von Initial Access Enabler {#access-enabler-int-setup}

Bevor Kunden Zugriff auf Inhalte anfordern, müssen Programmierer die Client-Komponente für die Adobe Pass-Authentifizierung - den Access Enabler - in ihre bestehende Media Player-App oder Web-Seite integrieren. Es gibt verschiedene Möglichkeiten, wie dies geschehen kann:

* Sie können die Flash-Version AccessEnabler.swf in einen Flash-basierten Videoplayer auf einer Webseite oder direkt in HTML einbetten. Sie können mit der SWF auf ActionScript oder JavaScript kommunizieren. Die Basis-API ist ActionScript, aber eine vollständige JavaScript-Wrapper-Bibliothek ist verfügbar.
* Bei Nicht-Flash-Geräten haben Sie folgende Möglichkeiten:
   * Verwenden Sie die HTML5-/JavaScript-Version, AccessEnabler.js, und kommunizieren Sie mit ihr über die JavaScript-API oder
   * Verwenden einer nativen Access Enabler-Bibliothek, z. B. für iOS, Android oder Windows 8

##### Setup für die anfängliche Client-lose API-Integration {#clientless-api-int-setup}

Bevor Kunden Zugriff auf Inhalte anfordern, müssen Programmierer die RESTful-Webservice-Aufrufe mit der Clientless-API in ihre Media Player-App implementieren und eine „Second-Screen“-Anwendung einrichten, um die Anmeldung des Benutzers bei seinem Pay-TV-Anbieter über das Web zu verarbeiten.

#### Handhabung von Authentifizierung und Autorisierung {#auth-authr-handling}

Wenn ein Kunde zum ersten Mal eine geschützte Ressource von einem Programmierer anfordert, stellt der Programmierer dem Kunden eine Liste von Pay-TV-Anbietern zur Auswahl. Wenn der Anbieter ausgewählt ist, wird der Benutzer zur ersten Benutzerauthentifizierung zu diesem Operator weitergeleitet. Sobald die Authentifizierung erfolgreich war, kommuniziert die Adobe Pass-Authentifizierung mit dem ausgewählten Pay-TV-Anbieter, um den Zugriff auf die angegebene Ressource zu autorisieren. Details zu diesen Prozessen folgen.

![](../assets/providr-selection-ui.png)


*Abbildung 3: Beispiel für die Benutzeroberfläche zur Anbieterauswahl*

>[!NOTE]
>
>* Die Authentifizierung erfolgt als SAML-Austausch zwischen der Adobe Pass-Authentifizierung als Dienstleister (oder „SP„) und einem Pay-TV-Anbieter als Identitätsanbieter (oder „IdP„).
>* Bei der Autorisierung wird ein Back-Channel-Webservice-Austausch (Server-zu-Server) zwischen der Adobe Pass-Authentifizierung (dem SP) und einem Pay-TV-Anbieter (dem IdP) verwendet.


##### Programmiererkommunikation mit dem Access Enabler

Der bidirektionale Kommunikationskanal zwischen dem Access Enabler und der Web-Seite oder der Player-App des Programmierers folgt einem vollständig asynchronen Muster. Der Programmierer sendet Nachrichten über die Methoden, die von der Access Enabler-API verfügbar gemacht werden, an den Access Enabler. Der Access Enabler antwortet über Callbacks, die mit der Access Enabler-Bibliothek registriert sind.

* Bei jeder Autorisierungsanfrage wird automatisch zuerst die Authentifizierung angefordert, wenn im lokalen System kein Authentifizierungstoken gefunden wird. Wenn die Authentifizierung erfolgreich ist, wird das Token des Kunden lokal gespeichert, sodass er sich über einen bestimmten Zeitraum nicht erneut anmelden muss. Wenn sie sich in einem anderen Kontext erfolgreich über die Adobe Pass-Authentifizierungsberechtigungslösung authentifiziert haben (z. B. über die Website des Pay-TV-Anbieters oder einen anderen Programmierer), hat der Access Enabler Zugriff auf das lokale Token und benötigt keine zusätzliche Authentifizierung.
* Wenn ein Kunde eine bestimmte Ressource anfordert, fordert der Programmierer über den Access Enabler die Autorisierung des Pay-TV-Anbieters an. Nach Überprüfung (oder Initiierung) der Authentifizierung kontaktiert der Access Enabler den Pay-TV-Anbieter (über die Adobe Pass-Authentifizierung), um zu ermitteln, ob der Kunde berechtigt ist, die Ressource anzuzeigen. Die Adobe Pass-Authentifizierung verarbeitet die Kommunikation mit dem Pay-TV-Anbieter, um eine Autorisierung zu erhalten. Der Programmierer muss lediglich die Anfrage an den Access Enabler senden und die Antwort verarbeiten (Erfolg oder Fehler bei der Autorisierung). Wenn die Autorisierung erfolgreich ist, wird ein Autorisierungs-Token auf dem Client-System gespeichert und der Callback erhält ein kurzlebiges Medien-Token.

##### Programmiererkommunikation mit der Client-losen API {#progr-comm-clientless-api}

Die Kommunikation zwischen der App des Programmierers und der Adobe Pass-Authentifizierung erfolgt über RESTful-Webservices.  Für alle API-Aufrufe an die Adobe Pass-Authentifizierungsendpunkte sind Sicherheitsprotokolle vorhanden.  Die Sicherheitsanforderungen werden in der Dokumentation zur Clientless-API beschrieben.

##### Beispiel-Workflow mit SSO-basierter SAML-Webbrowser-Authentifizierung {#sample-wf}

1. Betrachter Navigiert zu einer Website (dummy1.com) und versucht, auf berechtigte Inhalte zuzugreifen.
1. Video-Seite/Player lädt Access Enabler von adobe.com und fordert bei Aufforderung durch eine Benutzeraktion zur Autorisierung des angeforderten Inhalts auf.
1. Access Enabler führt den Anforderer und die Anforderung aus und validiert sie.
1. Access Enabler sucht im lokalen Speicher nach einem gültigen Autorisierungstoken. Wenn eine gültige Autorisierung gefunden wird, erzeugt der Access Enabler ein kurzlebiges Medien-Token (siehe Schritt 14).
1. Wenn keine gültige Autorisierung für die angeforderte Ressource gefunden wird, aber ein gültiges Authentifizierungstoken vorhanden ist, initiiert Access Enabler eine Autorisierungsanfrage beim Pay-TV-Anbieter, gegen den der Benutzer authentifiziert ist. Der Adobe-Server stellt den Autorisierungs-/Antwortaustausch mit dem Pay-TV-Anbieter bereit.
1. Wenn kein gültiges Authentifizierungstoken gefunden wird, fordert Access Enabler den Benutzer auf, seinen Pay-TV-Anbieter anzugeben. (Auswahl eines Anbieters, der die SSO-basierten SAML-Webbrowser-Authentifizierungs-Trigger unterstützt, durch einen SAML-basierten Authentifizierungs-Workflow. Bei Nicht-SAML-Anbietern verarbeitet Adobe einen ähnlichen benutzerdefinierten Workflow.)
1. Access Enabler führt den Browser zum SAML-Service (Service Provider) von Adobe und übergibt ihm alle entsprechenden Parameter.
1. Der SAML-SP ruft den entsprechenden SAML-IdP (Identity Provider) beim Pay-TV-Anbieter des Benutzers auf, wobei das SAML-Webbrowserprofil verwendet wird, wie in den IdP-Metadaten angegeben. Dadurch wird der Benutzer effektiv zur Seite des IdP (Pay-TV-Anbieter) weitergeleitet, wo er sich authentifiziert.
1. Nach erfolgreicher Authentifizierung wird der Benutzer zurück zu Adobe auf SAML SP weitergeleitet und übergibt in der SAML-Antwort eine Authentifizierungs-GUID.
1. Adobe auf SAML SP erstellt eine Server-seitige Sitzung, in der die Authentifizierungs-GUID gespeichert ist, und leitet den Benutzer zurück zur ursprünglichen Programmiererseite. (Die Serversitzung wird beim Abrufen des Authentifizierungs-Tokens durch Access Enabler gelöscht.)
1. Access Enabler ruft die Authentifizierungs-GUID vom Adobe-Server ab, um sie mit einer von der Adobe Pass-Authentifizierung verwalteten Geräte-ID in das Token aufzunehmen. Wenn sich das Flash-DRM auf dem Gerät befindet, erfolgt dies über Flash Acceß-APIs (Flash Player-DRM-Komponente), die die Bindung der GUID an die Geräte-ID ermöglichen und ein Authentifizierungstoken zurückgeben. Andernfalls erfolgt dies über JS-APIs über HTTPS unter Verwendung von HTML5-basiertem Speicher oder über bestimmte native Komponenten.
1. Das Authentifizierungstoken wird von Access Enabler verwendet, um Autorisierungsanfragen an den Pay-TV-Anbieter zu stellen. Auf Flash Acceß-fähigen Geräten werden die Anfragen immer über Flash Acceß-APIs gestellt, sodass das daraus resultierende Autorisierungs-Token an das Gerät gebunden ist. Auf Nicht-Flash Acceß-Geräten wird HTTPS für die sichere Kommunikation zwischen Client und Server verwendet.
1. Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung ein langlebiges Autorisierungs-Token („authZ„) und übergibt es an den Access Enabler, der es auf dem lokalen System speichert.
1. Der Access Enabler verwendet das AuthZ-Token, um kurzlebige Medien-Token zu erstellen, die für den tatsächlichen Anzeigezugriff verwendet werden. Aus Sicherheitsgründen müssen diese kurzlebigen Token von einer anderen Adobe Pass-Authentifizierungskomponente, dem Media Token Verifier, validiert werden.

![](../assets/authn-authz-entitlmnt-flow.png)


*Abbildung 4: Workflow „Authentication and Authorization Access Enabler“*

##### Bereitstellen einer Benutzeroberfläche für Berechtigungen {#entitlement-ui}

Programmierer müssen ihre eigene Benutzeroberfläche für den Zugriffs-Workflow auf ihre Website oder App erstellen. Einige Elemente, wie der eigentliche Anmeldevorgang, werden vom Pay-TV-Anbieter bereitgestellt, und einige Elemente sind optional als Teil der Adobe Pass-Authentifizierung verfügbar. Der Programmierer führt mindestens Folgendes aus:

* **Implementiert eine Schnittstelle zur Anbieterauswahl** über die neue Benutzende ihren Pay-TV-Anbieter identifizieren und sich erstmals anmelden können. Für die Entwicklung bietet der Access Enabler eine grundlegende Benutzeroberfläche, die dem Kunden eine Auswahl an Pay-TV-Anbietern ermöglicht und den Anmeldeprozess initiiert. In einer Produktionsumgebung müssen Programmierer ihr eigenes Dialogfeld für die Anbieterauswahl implementieren. Einige Pay-TV-Anbieter leiten zur Anmeldung zu ihrer eigenen Website weiter, und einige verlangen, dass ihre Anmeldeseiten in einem iFrame angezeigt werden. Programmierer müssen den Callback implementieren, der diesen iframe erstellt, falls der Kunde einen dieser Anbieter auswählt.
* **Identifiziert geschützte Ressourcen.** Geschützte Ressourcen sind diejenigen, für die eine Zugriffsautorisierung erforderlich ist. Beim Anbieten dieser Ressourcen sollte die Programmierschnittstelle angeben, dass vor dem Anzeigen eine Autorisierung erforderlich ist. Bei erfolgreicher Autorisierung sollte die Benutzeroberfläche zeigen, dass die Ressource jetzt autorisiert ist.
* **Erstellt und verwaltet eine Liste von Pay-TV** Anbietern, um den Benutzerzugriff nur auf die von Ihnen angegebenen Anbieter zu steuern.
* **Zeigt an, dass ein Benutzer authentifiziert ist.** Der Programmierer sollte den Authentifizierungsstatus des Kunden als Teil der Mittel angeben, die zur Identifizierung geschützter Ressourcen verwendet werden. Programmierer können den Access Enabler abfragen, um festzustellen, ob ein Kunde bereits authentifiziert wurde.

#### Unterstützen von Single Logout {#single-logout-support}

In den meisten Fällen ist der Programmierer für die Verarbeitung von Benutzerabmeldungen über einen einfachen API-Aufruf verantwortlich. Der Aufruf logout() weist die Adobe Pass-Authentifizierung an, den aktuellen Benutzer abzumelden, indem er:

* Löschen aller AuthN- und AuthZ-Token
* Löschen aller Authentifizierungs- und Autorisierungsinformationen für diesen Benutzer
* Initiieren eines anbieterspezifischen Pay-TV-Workflows zum Löschen der Authentifizierungssitzung des Benutzers beim Anbieter (wenn die Authentifizierung beispielsweise mit dem SAML-Authentifizierungsanforderungsprotokoll durchgeführt wurde, kann die Abmeldung mit dem SAML-Abmeldeprotokoll erfolgen).

Wenn der/die Benutzende den Computer so lange inaktiv lässt, dass seine/ihre Token ablaufen, kann er/sie trotzdem zu seiner/ihrer Sitzung zurückkehren und die Abmeldung erfolgreich starten. Die Adobe Pass-Authentifizierung stellt sicher, dass alle Token gelöscht werden, und benachrichtigt den Pay-TV-Anbieter, auch seine Sitzung zu löschen.


Wenn die Abmeldung von einer Site initiiert wird, die nicht in die Adobe Pass-Authentifizierung integriert ist, kann der Pay-TV-Anbieter den einmaligen Abmelde-Service der Adobe Pass-Authentifizierung über eine Browser-Umleitung aufrufen.

## Über die grundlegenden Leistungsflüsse hinaus - zusätzliche Funktionen {#beyond-basics}

Die grundlegenden Flüsse für Berechtigungen sind Start, Authentifizierung, Autorisierung und Abmelden.  Im Zuge der Reife und Entwicklung der Adobe Pass-Authentifizierung wurden und werden eine Reihe zusätzlicher Funktionen zu den grundlegenden -Flüssen hinzugefügt.  Dazu gehören:

* **Benutzermetadaten** - Abhängig von Vereinbarungen zwischen MVPDs und Programmierern können MVPDs Metadaten wie Postleitzahl, maximale Bewertung, Kanal-ID und mehr sicher austauschen. Metadaten ermöglichen verschiedene Anwendungsfälle, darunter Kindersicherung, regionale Einfrierzeiten für Sportereignisse usw.
* **Temporary Free Access** - Ermöglicht es Programmierern, temporären freien Zugriff auf ihre geschützten Inhalte anzubieten (z. B. kurze Beispiele für die tägliche Programmierung oder kostenlose Präsentation eines großen Ereignisses).
* **Proxy MVPD** - Ein MVPD kann seine eigene Integration mit der Adobe Pass-Authentifizierung verwalten und auch den Berechtigungsprozess im Namen einer Gruppe verknüpfter „ProxiedMVPDs“ verwalten.

## Sicherheit {#security}

In diesem Abschnitt wird die Sicherheit und Integrität der Authentifizierungsinfrastruktur von Adobe Pass beschrieben.

### Token-Sicherheit {#token-security}

Eines der Hauptziele der Adobe Pass-Authentifizierung besteht darin, sicherzustellen, dass das System Angriffen auf die Inhaltsberechtigungsdaten durch einen nicht autorisierten Benutzer oder einen Inhalts-Aggregator standhalten kann. Daher wird der Datenzugriff auf verschiedenen Ebenen im Workflow gesichert, wobei dem Schutz der Generierung und Verwendung der Daten des Autorisierungs-Tokens die höchste Bedeutung zukommt. Die Adobe Pass-Authentifizierungsarchitektur soll sicherstellen, dass Token-Inhalte sicher gepflegt werden und das Token auf dem Gerät, auf das es ausgegeben wurde, verbleibt.

* **Langlebige AuthN- und AuthZ-Token-**: Alle langlebigen Token werden vom Adobe Pass-Authentifizierungsserver digital signiert. Die digitale Signatur unterscheidet sich jedoch von Plattform zu Plattform insofern, als sie eine Geräte-ID verwendet, die sich hinsichtlich der Generierung, des Schutzes und der Validierung unterscheidet. In allen Fällen stellt eine Client-seitige Validierung sicher, dass die digitale Signatur intakt ist und die Integrität des Tokens erhalten bleibt. Der Access Enabler speichert die validierten Token sicher an Orten, die für die Umgebung spezifisch sind, in der er ausgeführt wird. Wenn die Überprüfung der Geräte-ID fehlschlägt, wird die Authentifizierungssitzung ungültig gemacht, Token werden zurückgesetzt und der Benutzer wird aufgefordert, sich erneut anzumelden.
* **Kurzlebige Medien-Token-**: Kurzlebige Medien-Token, die im letzten Schritt vor dem Inhaltszugriff erstellt werden, werden per Adobe signiert und verschlüsselt, um Manipulationen während des Austauschs zu vermeiden. Für kurzlebige Medien-Token ist außerdem ein zusätzlicher Validierungsschritt durch eine zusätzliche Adobe Pass-Authentifizierungskomponente, den Media Token Verifier, erforderlich. Die TTL des kurzlebigen Tokens ist auf den Standardwert von 5 Minuten eingestellt und kann bei Bedarf kürzer gestaltet werden. Das kurzlebige Medien-Token wird nie zwischengespeichert. Jedes Mal, wenn eine Autorisierungs-API aufgerufen wird, wird ein neues Token vom Server abgerufen.

### Plattformspezifische Gerätesicherheit {#platform-sp-security}

Die von der Adobe Pass-Authentifizierung verwendeten Sicherheitsmaßnahmen variieren je nach Plattform, sind jedoch alle robust und auf dem neuesten Stand der Technik.

* **Flash-fähige Geräte** - Wenn sich Flash Player 10.1+ oder AIR 2.5+ auf dem Gerät befindet, verwendet die Adobe Pass-Authentifizierung zum Schutz die Flash Player-DRM-Funktion, auch als Flash Acceß bezeichnet. Flash bietet ein zusätzliches Schutzniveau. Die hohe Sicherheit der Gerätebindung für Flash-basierte Token bedeutet, dass die Lebensdauer in den meisten Fällen länger sein kann, Benutzende sich nicht so oft anmelden müssen und das Benutzererlebnis im Allgemeinen reibungsloser ist.
* **Browser-interne Erlebnisse auf HTML5-fähigen Geräten**- Auf Nicht-Flash-Geräten mit HTML5-Browser-Funktion bietet die Adobe Pass-Authentifizierung eine alternative Möglichkeit zum eingeschränkten Schutz für Browser-basierte Integrationen. Da die Gerätebindung für HTML5 jedoch nicht so stark ist, ist die Time-to-Live (TTL) für Token auf HTML5-Plattformen in der Regel kürzer.
* **Native App-Unterstützung für In- und Out-of-Home-Geräte** - Adobe bietet native SDKs pro Betriebssystem (iOS, Android, Windows 8 usw.), die verbesserte Sicherheit gegenüber der HTML5-Lösung bieten. Diese SDKs verwenden native APIs, um eine Geräte-ID abzurufen und sie sicher an den Adobe Pass-Authentifizierungsserver zu übergeben.
* **Clientless** - Die Adobe Pass-Authentifizierung verwendet das HTTPS-Protokoll für eine sichere Kommunikation. Darüber hinaus müssen alle Aufrufe von einem intelligenten Gerät digital signiert werden.

## FAQs {#faqs}

**Was ist TV Everywhere?**
Die als TV Everywhere bekannte Branchenbewegung ermöglicht es Pay-TV-Kunden, auf Premium-Inhalte zuzugreifen, die sie bereits für eine Vielzahl an mit dem Internet verbundenen Geräten abonniert haben, darunter PCs, Tablets, Smartphones, Spielekonsolen, Set-Top-Boxen und „Smart-TVs“. Die Herausforderung dieser Initiative besteht darin, den Authentifizierungsprozess so einfach und schmerzlos wie möglich zu gestalten, sodass Kunden reibungslos und ohne untragbare Hindernisse und mit mehreren Anmeldungen auf ihren Abonnementinhalt zugreifen können.


**Was ist die Adobe Pass-Authentifizierung und wie steht sie in Bezug zu TV Everywhere?**
Mit der Adobe Pass-Authentifizierung wird TV Everywhere von Konzept zu Realität, indem die Berechtigung von Benutzenden für Inhalte auf einfache und sichere Weise überprüft wird. Die Adobe Pass-Authentifizierung ist ein gehosteter Service, der eine schnelle Backend-Integration ermöglicht, die auf den von Programmierern und Pay-TV-Anbietern geforderten Geschäftsregeln basiert. Dies bedeutet für alle Beteiligten eine kurze Markteinführungszeit, eine sicherere Umgebung zur Verhinderung von Betrug und eine überlegene Kundenerfahrung, wobei mehr TV-Inhalte für mehr Menschen plattformübergreifend verfügbar sind.


**Wie wird die Adobe Pass-Authentifizierung angeboten/bereitgestellt?**
Die Adobe Pass-Authentifizierung wird über das SaaS-Modell (Software as a Service) angeboten. Dies ermöglicht eine sicherere Kommunikation zwischen Endnutzern, Programmierern und Pay-TV-Anbietern, um die Berechtigung auf Inhalte zu überprüfen. Zu den Kernkomponenten des Service gehören der Client-seitige Access Enabler (oder bei einigen Geräten die Client-lose API) und der gehostete Adobe Pass-Authentifizierungsserver. Der Access Enabler ist eine kleine Datei, die in die Web-Seite oder Player-Anwendung eines Programmierers geladen wird. Sie kommuniziert mit den Adobe Pass-Authentifizierungsservern, die ihrerseits über Verbindungen verfügen, die in die Authentifizierungssysteme verschiedener Pay-TV-Anbieter integriert sind. Die Adobe Pass-Authentifizierung bietet auch einen Client-losen API-Integrationsansatz für einige „intelligente Geräte“, die nicht Web-fähig sind (Smart-TVs, Set-Top-Boxen, Spielekonsolen usw.). Der Client-lose Ansatz bietet RESTful-Web-Services, mit denen Entwicklerinnen und Entwickler die Berechtigungsflüsse für die Adobe Pass-Authentifizierung auf diesen Geräten implementieren können.


**Wie unterscheidet sich die Adobe Pass-Authentifizierung von anderen TV Everywhere-Lösungen?**
Die Adobe Pass-Authentifizierung bietet gegenüber alternativen TV Everywhere-Lösungen deutliche Vorteile. Direkte Integrationen mit einzelnen Anbietern bieten nicht die Flexibilität einer einzigen, persistenten Anmeldung (SSO), da Benutzer über das Internet von einer Site zu einer anderen reisen. Die Adobe Pass-Authentifizierung hat auch eine bemerkenswerte Marktdurchdringung: Sobald ein Programmierer mit der Adobe Pass-Authentifizierung integriert ist, wird er sofort mit Pay-TV-Anbietern verbunden, die mehr als 90 % der Haushalte in den Vereinigten Staaten bedienen. Darüber hinaus nutzt die Adobe Pass-Authentifizierung einzigartige Sicherheitsfunktionen, die in die Flash-Laufzeit (sofern verfügbar) integriert sind, um Betrug zu reduzieren. Gleichzeitig werden SDKs bereitgestellt, damit Programmierer dieselbe TV Everywhere-Funktion in native Apps für Mobilgeräte oder Heimgeräte integrieren können, auf denen Flash nicht verfügbar ist. Und schließlich: Während die Adobe Pass-Authentifizierung als eigenständiger Service verfügbar ist, bieten wir auch die Möglichkeit einer engen Integration mit anderen Adobe-Produkten und -Services (einschließlich Adobe Pass und Adobe Analytics) im Zusammenhang mit der Bereitstellung, dem Schutz und der Monetarisierung von TV Everywhere-Inhalten.

**Wie sicher ist die Adobe Pass-Authentifizierung?**
Die Adobe Pass-Authentifizierungsarchitektur muss in erster Linie sicherstellen, dass nur autorisierte Viewer authentifiziert werden und Zugriff auf Premium-Inhalte erhalten. Die Adobe Pass-Authentifizierung bindet den Zugriff auf das Anzeigegerät fest und kann dazu beitragen, Streams, Sitzungen und/oder Geräte für einen bestimmten Haushalt zu beschränken.


**Ist Flash Player erforderlich?**
Adobe Flash Player 11.x oder höher ist für die strengste Gerätebindungssicherheit erforderlich. Adobe Pass-Authentifizierung für TV Everywhere ist jedoch player- und plattformunabhängig und lässt sich mit jeder Wiedergabeanwendung integrieren, einschließlich Silverlight und HTML5. Darüber hinaus bietet die Adobe Pass-Authentifizierung native Unterstützung für Geräte wie iOS, Android und Xbox, auf denen keine Flash Player verfügbar ist.  Schließlich bietet die Adobe Pass-Authentifizierung einen Client-losen Ansatz für Geräte, die keine Web-Seite rendern können (Spielekonsolen, Smart-TVs, Set-Top-Boxen).


**Welche Geräte werden von der Adobe Pass-Authentifizierung unterstützt?**
Die Adobe Pass-Authentifizierung wird von praktisch jedem Gerät mit dem HTML5 Web Kit für Browser-basiertes Anwendererlebnis unterstützt. Darüber hinaus führt die Adobe Pass-Authentifizierung weiterhin native Software-Entwicklungs-Kits (SDKs) für verschiedene gerätespezifische Plattformen wie iOS, Android™ und Windows 8 ein. Die Adobe Pass-Authentifizierung unterstützt einige nicht webfähige Geräte (Smart-TVs, Set-Top-Boxen, Spielekonsolen usw.) über ihre RESTful-Webservice-APIs teilweise.

**Unterstützt die Adobe Pass-Authentifizierung die neuen Standards für TV Everywhere?**
Die Adobe Pass-Authentifizierung entspricht der **CableLabs OLCA (Online Content Access)** [Spezifikation](https://www.cablelabs.com/specifications), die technische Anforderungen und Architektur für die Bereitstellung von Videos an einen Pay-TV-Kunden aus Online-Quellen bereitstellt. Adobe nahm im Juni 2011 am gemeinsamen Interopt-Testprojekt von CableLabs teil und durchlief den Testprozess für eine Service Provider-Implementierung. Die Adobe Pass-Authentifizierung wird anhand der OLCA-Authentifizierungsspezifikationen überprüft (vollständig und getestet). Die Autorisierungskomponente ist abgeschlossen, aber die Testverifizierung wartet auf die Veröffentlichung der CableLabs-Testumgebung (ETA Nov 2011).

Adobe ist auch aktives Mitglied des **OATC (Open Authentication Technical Consortium)** und beteiligt sich als Teil dieses Gremiums an mehreren Spezifikationsentwürfen der Unterausschüsse.

**Wie handhabt die Adobe Pass-Authentifizierung die Federated Identity Management/Single Sign-on (SSO)?**
Mit der Adobe Pass-Authentifizierung können Sie Kundinnen und Kunden mithilfe der Back-Channel-Kommunikation (Server-zu-Server) zwischen der Adobe Pass-Authentifizierung und den beteiligten Pay-TV-Anbietern Authentifizierung und Autorisierung über Single Sign-on (SSO) bereitstellen. Bei der Adobe Pass-Authentifizierung müssen sich Abonnentinnen und Abonnenten also nicht nach ihrer ersten Authentifizierung erneut anmelden, solange diese Authentifizierung vom Pay-TV-Anbieter zugelassen wird, um fortzubestehen. Diese Beschränkung ist in der Regel auf 30 Tage festgelegt. Zu diesem Zweck stellt die Adobe Pass-Authentifizierung eine gemeinsame Domain für Authentifizierungs-Token für unsere Kunden bereit. Diese Authentifizierungszustandsinformationen stehen allen teilnehmenden Websites zur Verfügung, die mit einem bestimmten Pay-TV-Anbieter integriert sind.

Derzeit verwenden die meisten Adobe Pass-Authentifizierungsintegrationen mit Pay-TV-Anbietern das SAML-Protokoll, einen der wichtigsten Authentifizierungsstandards. Die Adobe Pass-Authentifizierung fungiert als Proxy-Service-Provider in der SAML-Architektur und behält die SAML-Authentifizierungsantwort als sicheres Token in der gemeinsamen Adobe-Domain bei. Die Adobe Pass-Authentifizierung entspricht SAML 2.0.

Während die Adobe Pass-Authentifizierung derzeit normalerweise mit SAML-SSO-Lösungen verwendet wird, abstrahiert die Adobe Pass-Authentifizierungsarchitektur alle Protokollspezifikationen von der Programmierer-Integration. Daher kann die Unterstützung für neue Protokolle - z. B. eines, das auf OAuth 2.0 oder benutzerdefinierten Protokollen basiert - im Laufe der Zeit hinzugefügt werden.

**Wie viel kostet die Adobe Pass-Authentifizierung für TV Everywhere für Endbenutzer?**
Endbenutzern entstehen durch die Verwendung der Adobe Pass-Authentifizierung keine zusätzlichen Kosten.

>[!NOTE]
>
>**Nächste Schritte:** Weitere Informationen erhalten Sie von Ihrem Adobe-Support-Mitarbeiter oder füllen Sie das Informationsanfrageformular ([) ](https://www.adobe.com/cfusion/mmform/index.cfm?name=adobepass_rfi).
>
