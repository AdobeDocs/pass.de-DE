---
title: Übersicht für MVPDs
description: Übersicht für MVPDs
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2734'
ht-degree: 0%

---

# Ein Überblick über MVPDs {#mvpd-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Einführung {#intro}

Diese Übersicht richtet sich an Multi-Channel Video Programming Distributors (MVPDs). Weitere Dokumentationen, einschließlich Kickstart- und Integrationshandbüchern, finden Sie im Abschnitt „Verwandte Informationen“ am Ende dieses Dokuments.



TV Everywhere (TVE) ist die inzwischen bekannte Branchenbewegung, die es Pay-TV-Abonnenten ermöglicht, auf die Inhalte zuzugreifen, für die sie bereits bezahlen, und zwar über mehrere Geräte hinweg, sowohl innerhalb als auch außerhalb ihres Hauses.  Für Pay-TV-Anbieter schafft TVE neue Möglichkeiten, sowohl bestehende Kundenbeziehungen zu erhalten als auch neue zu ermöglichen. Doch neben diesen Chancen ergeben sich auch Herausforderungen. In der TVE-Landschaft stellen Programmierer die Inhalte bereit, aber die MVPDs enthalten die Kundeninformationen, um zu überprüfen, ob potenzielle Zuschauer gültige Abonnenten sind.



Die Koordination der Zuschauerauthentifizierung und -autorisierung mit einem Programmierer mag einfach sein, aber die Koordination mit Dutzenden oder Hunderten von verschiedenen Programmierern wird immer komplexer. Mit Adobe® Pass müssen MVPDs jedoch nur eine einzige, einfache Integration implementieren, um Zugriff auf das gesamte TVE-Ökosystem zu erhalten, einschließlich Programmierern wie NBCUniversal Media, Turner Broadcasting (TBS, TNT, CNN), Fox Broadcast Networks, Hulu usw.  Die Adobe Pass-Authentifizierung bietet ein Integrations-Framework, das die Bestimmung von Benutzerberechtigungen einfach und sicher macht.



Fazit: Die Adobe Pass-Authentifizierung vermittelt auf sichere Weise Berechtigungstransaktionen zwischen Programmierern und MVPDs, was den Viewer-Zugriff auf Abonnementinhalte erleichtert. Mit anderen Worten: Die Adobe Pass-Authentifizierung erleichtert und beschleunigt den Zugriff der richtigen Kundinnen und Kunden auf die richtigen Inhalte.


Bei der Adobe Pass-Authentifizierung erhalten MVPDs:

Einfache Integration mit Programmierern.  Bieten Sie sofortige Konnektivität von mehreren Inhaltsverantwortlichen mit einer einzigen Integration.

Verbesserte Kundeninteraktion.  Unterstützen Sie ein reibungsloses Markenerlebnis, wenn Ihre Kunden Inhalte über mehrere Plattformen und Geräte hinweg anzeigen.

Sichere Authentifizierung.  Stellen Sie sicher, dass nur autorisierten Benutzenden und Geräten Zugriff auf Premium-Inhalte gewährt wird, und beschränken Sie (optional) die Anzahl der Geräte und gleichzeitigen Streams, die pro Haushaltskonto eine Verbindung herstellen können.

## FAQs {#faq}

Wie sicher ist die Adobe Pass-Authentifizierung? Die Adobe Pass-Authentifizierungsarchitektur muss in erster Linie sicherstellen, dass nur autorisierte Viewer authentifiziert werden und Zugriff auf Premium-Inhalte erhalten. Die Adobe Pass-Authentifizierung bindet den Zugriff auf Anzeigegeräte fest und kann dazu beitragen, Streams, Sitzungen und/oder Geräte für einen bestimmten Haushalt zu beschränken.


Ist Flash Player erforderlich? Adobe Pass-Authentifizierung für TV Everywhere ist Player- und Plattformunabhängig und lässt sich mit jeder Wiedergabeanwendung, einschließlich Silverlight und HTML5, integrieren. Darüber hinaus bietet die Adobe Pass-Authentifizierung native Unterstützung für Geräte wie Smartphones und Tablets mit iOS und Android.


Welche Geräte werden von der Adobe Pass-Authentifizierung unterstützt? Die Adobe Pass-Authentifizierung wird von praktisch jedem Gerät mit dem HTML5 Web Kit für Browser-basiertes Anwendererlebnis unterstützt. Darüber hinaus führt die Adobe Pass-Authentifizierung weiterhin native Software-Entwicklungs-Kits (SDKs) für verschiedene gerätespezifische Plattformen aus, einschließlich iOS, Android™, Xbox360 (nicht mehr unterstützt) und Adobe Air® (nicht mehr unterstützt). Zuletzt hat die Adobe Pass-Authentifizierung eine Client-lose Lösung für Geräte herausgegeben, die keine Browser-Seiten rendern können (z. B. „Smart-TVs“, Set-Top-Boxen und Spielekonsolen).  Die Möglichkeit, Browser-Seiten zu rendern, ist eine Voraussetzung für die Authentifizierung von Benutzern mit MVPDs.


Unterstützt die Adobe Pass-Authentifizierung die neuen Standards für TV Everywhere? Die Adobe Pass-Authentifizierung entspricht der CableLabs OLCA-Spezifikation (Online Content Access), die technische Anforderungen und Architektur für die Bereitstellung von Videos aus Online-Quellen an einen Pay-TV-Kunden bereitstellt. Adobe nahm im Juni 2011 am gemeinsamen Interopt-Testprojekt von CableLabs teil und durchlief den Testprozess für eine Service Provider-Implementierung. Die Adobe Pass-Authentifizierung wird anhand der OLCA-Authentifizierungsspezifikationen überprüft (vollständig und getestet). Die Autorisierungskomponente ist abgeschlossen, aber die Testverifizierung wartet derzeit auf die Veröffentlichung der CableLabs-Testumgebung. Adobe ist auch aktives Mitglied des OATC (Open Authentication Technical Consortium) und beteiligt sich als Teil dieses Gremiums an mehreren Spezifikationsentwürfen der Unterausschüsse.



Was ist Authentifizierung? Authentifizierung ist der Prozess, bei dem eine MVPD bestätigt, dass eine bestimmte Benutzerin bzw. ein bestimmter Benutzer eine bekannte Kundin oder ein bekannter Kunde ist.



Was ist Autorisierung? Autorisierung ist der Prozess, bei dem ein MVPD bestätigt, dass ein authentifizierter Benutzer über ein gültiges Abonnement für eine bestimmte Ressource verfügt.



## Architektur {#architecture}

Die Adobe Pass-Authentifizierung ist ein gehosteter Service, der eine schnelle Backend-Integration (Server-zu-Server) ermöglicht, basierend auf den Geschäftsregeln, die sowohl von MVPDs als auch von Programmierern benötigt werden. Dies bedeutet für alle Beteiligten eine kurze Markteinführungszeit, eine sicherere Umgebung zur Verhinderung von Betrug und eine überlegene Kundenerfahrung, wobei mehr TV-Inhalte für mehr Menschen plattformübergreifend verfügbar sind.


Die Adobe Pass-Authentifizierung wird über das Software-as-a-Service (SaaS)-Modell angeboten und ermöglicht eine sicherere Kommunikation zwischen Endbenutzern, MVPDs und Programmierern, um die Berechtigung für Inhalte zu überprüfen. Der Service umfasst folgende Kernkomponenten:

Serverseitig - Der gehostete Adobe Pass-Authentifizierungsserver. Hierbei handelt es sich um einen Anwendungs-Server, der mit den Authentifizierungssystemen von MVPDs über einen Back-Channel (Server-zu-Server) kommuniziert.
Client-Seite:
Client-seitiger Access Enabler - Der Access Enabler ist eine kleine Datei, die in die Web-Seite oder Player-Anwendung eines Programmierers geladen wird. Es stellt Berechtigungs-APIs für das Programm zum Anzeigen von Inhalten des Programmierers bereit und kommuniziert mit dem Adobe Pass-Authentifizierungsserver.
Clientlose Webdienste (für nicht webfähige Geräte) - RESTful-Webdienste, die Berechtigungs-APIs für Geräte wie Smart-TVs, Spielekonsolen und Set-Top-Boxen bereitstellen.

>[!NOTE]
>
>Als MVPD müssen Ihre Web-Services Authentifizierungs- und Autorisierungsanfragen der Adobe Pass-Authentifizierung erkennen und mit den erforderlichen Daten im erwarteten Format antworten können.
>

Mit der Adobe Pass-Authentifizierung können Sie Kundinnen und Kunden eine Federated Identity Management (Federated Identity Management) bereitstellen, die auch als Single Sign-on (SSO)-Authentifizierung und -Autorisierung bezeichnet wird. Bei der Adobe Pass-Authentifizierung besteht keine Notwendigkeit, dass sich Abonnentinnen und Abonnenten nach ihrer ersten Authentifizierung erneut anmelden, solange diese Authentifizierung durch die MVPD zulässig ist, beizubehalten. (In der Regel 30 Tage.) Zu diesem Zweck stellt die Adobe Pass-Authentifizierung eine gemeinsame Domain für Authentifizierungs-Token für unsere Kunden bereit. Diese Authentifizierungszustandsinformationen stehen allen teilnehmenden Websites zur Verfügung, die mit einer bestimmten MVPD integriert sind.


Derzeit verwenden die meisten Adobe Pass-Authentifizierungsintegrationen mit MVPDs das SAML-Protokoll, eines der wichtigsten Authentifizierungsstandards. Die Adobe Pass-Authentifizierung fungiert als Proxy-Service-Provider in der SAML-Architektur und behält die SAML-Authentifizierungsantwort als sicheres Token in der gemeinsamen Adobe-Domain bei. Die Adobe Pass-Authentifizierung entspricht SAML 2.0. Während die Adobe Pass-Authentifizierung derzeit jedoch normalerweise mit SAML-SSO-Lösungen verwendet wird, ist die Adobe Pass-Authentifizierungsarchitektur an kein bestimmtes Protokoll gebunden. Daher kann die Unterstützung für neue Protokolle - z. B. eines, das auf OAuth 2.0 oder benutzerdefinierten Protokollen basiert - im Laufe der Zeit hinzugefügt werden.


Adobe arbeitet mit einem technischen Team von MVPD zusammen, um die Adobe Pass-Authentifizierung so zu konfigurieren, dass sie den Anforderungen bestehender Integrationen entspricht. Die Integration ist für MVPDs kostenlos, vorausgesetzt, es handelt sich um eine „standardmäßige“ Integration und minimale Support-Anforderungen (Dokumentation und grundlegender E-Mail-Support). Wenn ein MVPD erhebliche Unterstützung oder einen eskalierten Zeitplan erfordert, kann eine Support-Gebühr erhoben werden oder der Anbieter möchte möglicherweise mit einem Drittanbieter zusammenarbeiten, der mit unserer Lösung vertraut ist, z. B. Synacor.


Die Adobe Pass-Authentifizierung unterstützt auch die effiziente Handhabung der MVPD-Geschäftslogik wie folgt:

Für eine Geschäftslogik, die eigenständig ist und von der MVPD angewendet werden kann, wenn eine Autorisierungsanfrage eingeht, stellt Adobe die erforderlichen Daten bereit, um die Durchsetzung der Geschäftslogik zu unterstützen, wenn die MVPD eine Autorisierungsanfrage erhält. Diese Daten können u. a. die eindeutige Geräte-ID des anfragenden Benutzers und die IP-Adresse des Geräts umfassen.

Für Geschäftslogiken, die ein Eingreifen des Benutzers und/oder eine spezifische Handhabung durch die Adobe-Lösung erfordern, kann Adobe für jede MVPD einige benutzerdefinierte Eigenschaften beibehalten. Diese MVPD-spezifischen Konfigurationen/Richtlinien umfassen die Aktivierung vordefinierter Workflows, die an bestimmten Stellen des Workflows der obersten Ebene gestartet werden können. Weitere Informationen zur Unterstützung benutzerdefinierter Eigenschaften erhalten Sie von Ihrem Adobe-Support-Mitarbeiter.

Die folgende Abbildung zeigt die Beziehung von MVPD und Programmierer zu diesen Adobe Pass-Authentifizierungskomponenten:

![](../assets/high-level-architecture-nflows.png)

*Abbildung: Allgemeine Architektur und Flüsse*

## Adobe Pass-Authentifizierungskomponenten {#components}

Im Folgenden finden Sie einen Überblick über einige der Hauptkomponenten des Adobe Pass-Authentifizierungs-Ökosystems. Dazu gehören:

* [Der Access Enabler/Client-lose Web-Services](#ae)
* [Der Adobe-gehostete Backend-Server](#backend)
* [Token](#tokens)

### Zugriff auf Enabler/Client-lose Web-Services {#ae}

Der Access Enabler erleichtert alle Authentifizierungs- und Autorisierungsinteraktionen mit dem Benutzer und wird lokal auf dessen System ausgeführt. Es ist der Access Enabler, der die tatsächlichen Berechtigungs-Workflows mit der MVPD verarbeitet, während der Programmierer die Verantwortung für die Web-Seite oder das Player-Programm auf höherer Ebene behält.

Clientlose Webservices werden von der Adobe Pass-Authentifizierung für Geräte bereitgestellt, die keine Web-Seiten rendern können.  Bei diesen Geräten wird der Berechtigungsprozess initiiert und die Inhalte auf dem intelligenten Gerät angezeigt, während die Authentifizierung mit einem MVPD auf einem webfähigen Gerät (PC, Smartphone und Tablet) erfolgt.

Der Access Enabler:

* Startet MVPD-spezifische Authentifizierungs- und Autorisierungs-Workflows.
* Zwischenspeichert die erfolgreichen Autorisierungsantworten pro Programmiererressource/Kanal, um unnötigen Anfragedatenverkehr zu minimieren.
* Kann für vordefinierte Workflows konfiguriert werden, die für jede MVPD spezifisch sind, z. B. für die explizite Geräteregistrierung.
* Es ist in folgenden Formen verfügbar:
   * Eine SWF-Datei, die die Flash Player-Laufzeit ausführen kann
   * JS-Datei, die direkt vom Browser ausgeführt wird
   * Ein nativer Access Enabler für verschiedene Plattformen, einschließlich iOS, Android und Xbox.

### Adobe-gehosteter Backend-Server {#backend}

Der Backend-Server für die Adobe Pass-Authentifizierung, gehostet von Adobe:

* Stellt die Authentifizierungs- und Autorisierungs-Workflows mit den MVPDs bereit, die eine Server-zu-Server-Kommunikation zwischen der Adobe Pass-Authentifizierung und dem Operator erfordern.
* Behält die Konfiguration für Programmierer-Sites und -Programme bei.
* hostet die herunterladbaren Komponentendateien des Access Enabler.
* Erzeugt Authentifizierungs- und Autorisierungstoken.

### Token {#tokens}

Die Adobe Pass-Lösung für Authentifizierungsberechtigungen konzentriert sich auf die Generierung bestimmter Datenelemente, die nach erfolgreichem Abschluss der Authentifizierungs-/Autorisierungs-Workflows abgerufen werden. Diese Daten werden als Token bezeichnet. Sie haben eine begrenzte Lebensdauer und werden an plattformabhängigen Stellen sicher gespeichert. Nach Ablauf müssen Token durch die erneute Initiierung der Authentifizierungs- und/oder Autorisierungs-Workflows erneut ausgestellt werden.

Es gibt drei Arten von Token, die während der Authentifizierungs-/Autorisierungs-Workflows ausgegeben werden. Zwei davon sind „langlebig“ und bieten Kontinuität im Anwendererlebnis. Das dritte Token, ein kurzlebiges Token, unterstützt Best Practices für die Branche, um Betrug durch Stream-Ripping zu reduzieren. Die Time-to-Live („TTL„)-Werte für Token werden auf der Grundlage von Vereinbarungen zwischen MVPDs und Programmierern festgelegt. Sie entscheiden sich für einen TTL-Wert, der Ihrem Unternehmen und Ihren Kunden am besten dient.

**Das langlebige Authentifizierungstoken**. Die Authentifizierung ist erfolgreich, wenn eine Kundin oder ein Kunde die Adobe Pass-Authentifizierung verwendet, um sich erfolgreich bei ihrem MVPD-Konto anzumelden. Die Adobe Pass-Authentifizierung erzeugt dann ein langlebiges Authentifizierungstoken („authN„), das mit dem anfragenden Gerät verknüpft ist, und (je nach MVPD) eine global eindeutige Kennung („GUID„), die den Benutzer anonym identifiziert.

**Das langlebige Autorisierungs-Token**. Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung ein langlebiges Autorisierungs-Token („authZ„). Dieses Token ist nicht portabel, da es an das anfordernde Gerät und eine bestimmte geschützte Ressource (z. B. einen Kanal, eine Serie oder eine Folge) gebunden ist. Der Access Enabler verwendet das langlebige AuthZ-Token, um die kurzlebigen Medien-Token zu erstellen, die für den tatsächlichen Anzeigezugriff verwendet werden.

**Das kurzlebige Medien-Token**. Sobald der Benutzer autorisiert ist, generiert die Adobe Pass-Authentifizierung ein authZ-Token und verwendet dieses Token, um ein einmaliges, kurzlebiges Medien-Token zu generieren, das von Adobe signiert und verschlüsselt wird, um Manipulationen während des Austauschs zu vermeiden. Da das kurzlebige Token entweder über die Access Enabler-API oder Client-lose Webservices für die Einbettungssite verfügbar gemacht wird, muss der Medienserver des Programmierers vor dem Zugriff auf die geschützte Ressource eine Adobe Pass-Authentifizierungskomponente, den Media Token Verifier, verwenden, um das Token zu validieren.

## MVPD Integration Lifecycle {#lifecycle}

Die folgende Abbildung zeigt den Lebenszyklus der Integration zwischen der Adobe Pass-Authentifizierung und einer MVPD.

![](../assets/mvpd-int-lifecycle.png)

*Abbildung: MVPD-Integrationslebenszyklus*

## Ablaufdiagramm für Berechtigungen {#chart}

Das folgende Diagramm zeigt den gesamten Prozess der Bestätigung von Berechtigungen mithilfe der Adobe Pass-Authentifizierung:

![](../assets/authn-authz-entitlmnt-flow.png)

*Abbildung: Bestätigungsprozess der Berechtigung mithilfe der Adobe Pass-Authentifizierung*

## Authentifizierungsschritte {#authn-steps}

Die folgenden Schritte zeigen ein Beispiel für den Adobe Pass-Authentifizierungsfluss.  Dies ist der Teil des Berechtigungsprozesses, in dem ein Programmierer bestimmt, ob der Benutzer ein gültiger Kunde einer MVPD ist.  In diesem Szenario ist der Benutzer ein gültiger Abonnent einer MVPD.  Der/die Benutzende versucht, geschützte Inhalte mithilfe einer Flash-Anwendung eines Programmierers anzuzeigen:

1. Der/die Benutzende navigiert zur Programmierer-Webseite, auf der die Flash-Anwendung des/die Programmierers und die Adobe Pass Authentication Access Enabler-Komponenten auf den Computer des/der Benutzenden geladen werden. Die Flash-Anwendung verwendet Access Enabler, um die Identität des Programmierers mit der Adobe Pass-Authentifizierung festzulegen, und die Adobe Pass-Authentifizierung stellt dem Access Enabler die Konfigurations- und Statusdaten für diesen Programmierer (den „Anforderer„) zur Verfügung. Der Access Enabler muss diese Daten vom Server erhalten, bevor andere API-Aufrufe durchgeführt werden können.  Technischer Hinweis: Der Programmierer legt seine Identität mit der `setRequestor()`-Methode des Access Enablers fest. Weitere Informationen finden Sie im [Programmierer-Integrationshandbuch](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md).
1. Wenn der Benutzer versucht, die geschützten Inhalte des Programmierers anzuzeigen, stellt das Programm des Programmierers dem Benutzer eine Liste von MVPDs zur Verfügung, aus denen der Benutzer einen Anbieter auswählt.
1. Der Benutzer wird an einen Adobe Pass-Authentifizierungsserver umgeleitet, auf dem eine verschlüsselte SAML-Anfrage für den vom Benutzer ausgewählten MVPD erstellt wird. Diese Anfrage wird als Authentifizierungsanfrage im Namen des Programmierers an MVPD gesendet. Abhängig vom MVPD-System wird der Browser des Benutzers entweder zur MVPD-Site weitergeleitet, um sich anzumelden, oder es wird ein Anmelde-iFrame in der App des Programmierers erstellt.
1. In beiden Fällen (Redirect oder iFrame) akzeptiert die MVPD die Anfrage und zeigt ihre Anmeldeseite an.
1. Der Benutzer meldet sich bei der MVPD an, die MVPD überprüft den Benutzerstatus als zahlender Kunde und die MVPD erstellt dann eine eigene HTTP-Sitzung.
1. Wenn der Benutzer validiert wird, erstellt MVPD eine Antwort (SAML und verschlüsselt), die der MVPD an die Adobe Pass-Authentifizierung zurücksendet.
1. Die Adobe Pass-Authentifizierung erhält die MVPD-Antwort, stellt fest, dass eine Adobe Pass-Authentifizierungs-HTTP-Sitzung geöffnet ist, validiert die [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language)-Antwort von der MVPD und leitet zurück zur Website des Programmierers.
1. Die Website des Programmierers wird neu geladen, der Access Enabler wird neu geladen, und der Programmierer ruft setRequestor() erneut auf.  Der zweite Aufruf von setRequestor() ist erforderlich, da sich die aktuelle Konfiguration geändert hat. Jetzt ist ein Flag vorhanden, das dem Access Enabler mitteilt, dass ein Authentifizierungs-Token auf die Generierung auf dem Server wartet.
1. Der Access Enabler erkennt, dass eine Authentifizierung aussteht und fordert das Token vom Adobe Pass-Authentifizierungsserver an. Das Token wird vom Server abgerufen, indem die DRM-Funktionen der Flash Player aufgerufen werden.
1. Das AuthN-Token wird im Flash Player-LSO-Cache des Programmierers gespeichert. Die Authentifizierung ist jetzt abgeschlossen und die Sitzung wird auf dem Adobe Pass-Authentifizierungsserver zerstört.

## Autorisierungsschritte {#authz-steps}

Die folgenden Schritte werden im vorherigen Abschnitt fortgesetzt ([Authentifizierungsschritte](#authn-steps)):

1. Wenn Benutzende versuchen, auf den geschützten Inhalt des Programmierers zuzugreifen, prüft die Anwendung des Programmierers zunächst auf dem lokalen Computer oder Gerät der Benutzenden auf ein AuthN-Token.  Wenn dieses Token nicht vorhanden ist, [ die oben genannten ](#authn-steps) befolgt.  Wenn das AuthN-Token vorhanden ist, fährt der Autorisierungsfluss mit dem Aufruf des Access Enabler durch das Programm des Programmierers fort, wobei die Anfrage gestellt wird, die Anzeigerechte des Benutzers für ein bestimmtes Element geschützten Inhalts abzurufen.
1. Das spezifische Element geschützter Inhalte wird durch eine „Ressourcenkennung“ dargestellt.  Dies kann eine einfache Zeichenfolge oder eine komplexere Struktur sein, aber in jedem Fall wird die Art der Ressourcenkennung vorab zwischen dem Programmierer und dem MVPD vereinbart.  Die Anwendung des Programmierers übergibt die Ressourcenkennung an den Access Enabler.  Der Access Enabler sucht auf dem lokalen Computer oder Gerät des Benutzers nach einem AuthZ-Token.  Wenn das AuthZ-Token nicht vorhanden ist, übergibt Access Enabler die Anfrage an den Backend-Adobe Pass-Authentifizierungsserver.
1. Der Adobe Pass-Authentifizierungsserver kommuniziert mithilfe standardisierter Protokolle mit dem MVPDs-Autorisierungsendpunkt.  Wenn die Antwort von MVPD anzeigt, dass die Benutzerin bzw. der Benutzer berechtigt ist, die geschützten Inhalte anzuzeigen, erstellt der Adobe Pass-Authentifizierungsserver ein AuthZ-Token und übergibt es zurück an Access Enabler, der das AuthZ-Token auf dem Computer der Benutzerin bzw. des Benutzers speichert.
1. Mit einem auf dem Computer oder Gerät des Benutzers gespeicherten AuthZ-Token ruft die Anwendung des Programmierers den Access Enabler auf, um ein Medien-Token vom Adobe Pass-Authentifizierungsserver zu erhalten, und stellt dieses Token der Anwendung des Programmierers zur Verfügung.
1. Schließlich verwendet die Anwendung des Programmierers die Komponente „Media Token Verifier“, um zu bestätigen, dass der richtige Benutzer den richtigen Inhalt anzeigt, und wenn das Medien-Token eingerichtet ist, kann der Benutzer den geschützten Inhalt anzeigen.

<!--
>![RELATEDINFORMATION]
>
>*   Kickstart Guides, [MVPD kickstart](/help/authentication/mvpd-kickstart-guide.md) and [programmer kickstart](/help/authentication/programmer-kickstart-guide.md). These guides explain the initial steps to take to begin integrating with Adobe Pass Authentication.
>
>*   [MVPD Integration Guide](/help/authentication/mvpd-kickstart-guide.md). This is a lower level technical guide for MVPDs, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>
>*   [Overview For Programmers](/help/authentication/programmer-overview.md). The same high level of conceptual information as in this MVPD overview, but directed toward the content providers (Programmers).
-->
