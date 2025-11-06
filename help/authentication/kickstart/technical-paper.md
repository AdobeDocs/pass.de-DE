---
title: Über die Adobe Pass-Authentifizierung
description: Über die Adobe Pass-Authentifizierung
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# Über die Authentifizierung mit Adobe® Pass {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Über TV Everywhere {#about-tv-everywhere}

Fernsehzuschauer erwarten heute einen nahtlosen Zugang zu Pay-TV-Inhalten - jederzeit und überall. Mit dem Aufkommen von Geräten, die mit dem Internet verbunden sind, konsumieren Zielgruppen Inhalte über eine wachsende Anzahl von Plattformen, darunter:

* Notebooks
* Tablets
* Smartphones
* Websites
* Federated Apps
* Spielekonsolen
* Set-Top-Boxen
* Smart-TVs

TV Everywhere ist eine Initiative der Branche, die dafür sorgt, dass Pay-TV-Abonnenten auf Inhalte zugreifen können, für die sie bereits bezahlen, und zwar über mehrere Geräte hinweg, sowohl innerhalb als auch außerhalb ihres Hauses.

Während das traditionelle (lineare) Fernsehen stark bleibt, sind die am schnellsten wachsenden Segmente des Videokonsums zeitverschobene Inhalte, Online-Streaming und alternative Bildschirme. Dieser Wandel hat den Videovertrieb gestört, sodass TV Everywhere zu einer wichtigen Lösung geworden ist, durch die die Interessen von **Programmierern, Pay-TV-Anbietern und Abonnenten“ in Einklang gebracht werden**

### Ziele von TV Everywhere {#goals-tv-everywhere}

**Technisches Ziel**

* Bezahlfernsehkunden den nahtlosen Zugriff auf abonnierte Inhalte über alle Geräte und Plattformen hinweg ermöglichen.

**Geschäftsziele**

* Bewahren und stärken Sie bestehende Kundenbeziehungen und eröffnen Sie gleichzeitig neue Chancen.
* Ermöglichen Sie es Programmierern und Inhaltsverantwortlichen, breitere Zielgruppen zu erreichen und den Wert von Premium-Inhalten zu maximieren.
* Marken durch direkte Online-Interaktion mit Betrachtern erweitern.

### Herausforderungen des Fernsehens überall {#challenges-tv-everywhere}

Angesichts der Möglichkeiten, die das Fernsehen „Überall“ bietet, stehen die Anspruchsberechtigungen im Vordergrund. Bevor ein Betrachter auf Abonnementinhalte zugreifen kann, muss ein System seine Berechtigung überprüfen.

Zu den wichtigsten Fragen gehören:

* Verfügt der Benutzer über ein aktives Abonnement bei einem Pay-TV-Anbieter?
* Enthält ihr Abonnement die angeforderten Inhalte?

Für Programmierer und Inhaltsinhaber ist es besonders schwierig, eine Berechtigung zu bestimmen, da Pay-TV-Anbieter Kundendaten und Zugriffsberechtigungen kontrollieren.

Neben den Berechtigungen stellen sich verschiedene technische und Integrationsprobleme, darunter:

* Entwicklung einer Strategie mit mehreren Geräten, die den nahtlosen Zugriff sicherstellt.
* Verwaltung komplexer Beziehungen zwischen Programmierern und Pay-TV-Anbietern.
* Verhinderung von betrügerischem Zugriff und Durchsetzung von Nutzungsbedingungen.
* Bereitstellen eines konsistenten, benutzerfreundlichen Authentifizierungserlebnisses für Websites und Apps.
* Gewährleistung einer schnellen Markteinführungszeit, um sich an Affiliate-Vereinbarungen anzupassen.
* Kostenkontrolle im Zusammenhang mit mehreren Integrationen.

Diese Herausforderungen machen die direkte Integration zwischen Programmierern und mehreren Pay-TV-Authentifizierungssystemen äußerst ressourcenintensiv und erfordern sowohl Zeit als auch technisches Know-how.

Um diese Hürden zu überwinden, vereinfacht und optimiert die **Adobe® Pass Authentication** die Überprüfung von Berechtigungen und ermöglicht so einen nahtlosen, sicheren Zugriff auf TV Everywhere-Inhalte.

## Einführung in die Adobe Pass-Authentifizierung {#introduction-adobe-pass-authentication}

Die Adobe Pass-Authentifizierung vermittelt auf sichere Weise Berechtigungstransaktionen zwischen Programmierern und Pay-TV-Anbietern, um sicherzustellen, dass die richtigen Kunden mühelos auf die richtigen Inhalte zugreifen können.

![](../assets/programmers-connect-authn.png)

*Einige der Programmierer und Pay-TV-Anbieter, die eine Verbindung über die Adobe Pass-Authentifizierung herstellen*

**Wer profitiert von der Adobe Pass-Authentifizierung**

* **Programmierer**

  Wer problemlos mit Pay-TV-Anbietern, auch bekannt als MVPDs (Multichannel Video Programming Distributors), integrieren möchte, um die breiteste Zielgruppe zu erreichen und die Umsätze zu optimieren.

* **Pay-TV-Anbieter (MVPDs)**

  Personen, die über eine einzige Integration mit mehreren Inhaltsinhabern (Programmierern) in Kontakt treten möchten, um die Kundenzufriedenheit zu steigern, indem sie den Online-Zugriff auf Abonnementinhalte erleichtern.

* **Pay-TV-Kunden**

  Wer die Inhalte sehen möchte, für die er bereits bezahlt, jederzeit, überall und auf jedem Gerät.

**Programmierer**

Programmierer, die die Reichweite und den Umsatz ihrer Zielgruppe maximieren möchten, können:

* Einfache Integration mit führenden Pay-TV-Anbietern ohne Verwaltung mehrerer Direktverbindungen.
* Erweitern Sie Ihre Zielgruppe, indem Sie die Authentifizierung über alle wichtigen Anbieter und Plattformen hinweg sicherstellen.
* Schützen Sie Premium-Inhalte durch sichere Authentifizierung, die den Zugriff auf autorisierte Benutzer und Geräte einschränkt.
* Single Sign-On (SSO) aktivieren, um das Benutzererlebnis auf Apps und Websites zu verbessern.

**Pay-TV-Anbieter (MVPDs)**

Pay-TV-Anbieter können das Kundenerlebnis verbessern und den Betrieb optimieren durch:

* Herstellen einer Verbindung zu mehreren Inhaltsinhabern über eine einzige Integration.
* Bietet ein markenübergreifendes, nahtloses Anwendererlebnis auf allen Geräten und Plattformen.
* Sichere Authentifizierung, um nicht autorisierten Zugriff zu verhindern und gleichzeitige Datenströme pro Haushalt zu verwalten

**Pay-TV-Kunden**

Abonnentinnen und Abonnenten profitieren von TV Everywhere , sodass sie jederzeit, überall und auf jedem Gerät Inhalte sehen können, für die sie bereits bezahlen.

### Integrieren mit der Adobe Pass-Authentifizierung {#integrating-adobe-pass-authentication}

Unabhängig davon, ob Sie Pay-TV-Anbieter oder Programmierer sind, erfordert die Integration in die Adobe Pass-Authentifizierung eine aktive Beteiligung. Nachfolgend finden Sie einen Überblick über den Prozess für beide Rollen.

#### Integrationsprozess für Programmierer {#programmer-integration-process}

Zusätzliche Anleitungen sind verfügbar, sobald die Integration formell eingeleitet wurde, aber der Prozess umfasst in der Regel die folgenden Schritte:

**Voraussetzungen**

* Ein Content Management System (CMS).
* Ein Mechanismus zur Bereitstellung von Inhalten, der ein Content Delivery Network (CDN) eines Drittanbieters enthalten kann.
* Eine vorhandene Online-Videoplattform mit einem in eine Website eingebetteten Medien-Player oder einer eigenständigen Anwendung.

**Integrationsaufgaben**

* Integrieren Sie die Adobe Pass-Authentifizierung [REST-API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).
* Integrieren Sie die Adobe Pass-Authentifizierung [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md).
* Integrieren Sie die Adobe Pass-Authentifizierung [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).
* Entwickeln Sie eine Benutzeroberfläche für den Authentifizierungs-, Autorisierungs- und Abmelde-Workflow.

Weitere Informationen zum Programmierer-Integrationsprozess finden Sie im [Programmierer-Schnellstartanleitung](/help/authentication/kickstart/programmer-kickstart-guide.md) und [Programmierer-Integrationshandbuch](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md).

#### Integrationsprozess des Pay-TV-Anbieters {#pay-tv-provider-integration-process}

Zusätzliche Anleitungen sind verfügbar, sobald die Integration formell eingeleitet wurde, aber der Prozess umfasst in der Regel die folgenden Schritte:

**Voraussetzungen**

* Unterschreiben Sie die Adobe Pass Authentication Non-Disclosure Agreement (NDA).
* Geben Sie dem Adobe Pass Authentication Engineering-Team Spezifikationen für das Authentifizierungs- und Autorisierungssystem. Für die einfachste Integration werden ein SAML-basierter Identitätsanbieter (IdP) für die Authentifizierung und ein SOAP-basiertes Autorisierungssystem empfohlen.

**Integrationsaufgaben**

* Herstellen der Verbindung mit Adobe Pass-Authentifizierungsservern.
* Vollständige Staging-Version und Qualitätssicherung.
* Vollständige Produktionsfreigabe und Sicherstellung der Qualitätssicherung.

Die Standardintegration ist für Pay-TV-Anbieter kostenlos, einschließlich Dokumentation und grundlegender E-Mail-Support. Anbieter, die umfangreiche Unterstützung oder beschleunigte Zeitpläne benötigen, können jedoch eine Support-Gebühr entrichten oder sich dafür entscheiden, mit einem erfahrenen Drittanbieter wie Synacor zusammenzuarbeiten.

Die Adobe Pass-Authentifizierung kann die für Pay-TV-Anbieter spezifische Geschäftslogik auf zwei Arten effizient unterstützen:

* Für eine Geschäftslogik, die eigenständig ist und vom Pay-TV-Anbieter angewendet wird, wenn eine Autorisierungsanfrage eingeht, stellt Adobe die erforderlichen Daten (z. B. eindeutige Geräte-ID, IP-Adresse) bereit, um die Durchsetzung zu unterstützen.
* Für eine Geschäftslogik, die ein Eingreifen des Benutzers oder eine spezifische Handhabung durch Adobe erfordert, können für jeden Pay-TV-Anbieter benutzerdefinierte Eigenschaften verwaltet werden. Diese Konfigurationen können vordefinierte Workflows enthalten, die an bestimmten Punkten des Authentifizierungsprozesses ausgelöst werden.

Weitere Informationen zum Integrationsprozess für Pay-TV-Anbieter finden Sie im [Handbuch zur MVPD-](/help/authentication/kickstart/mvpd-kickstart-guide.md) und im [Handbuch zur MVPD-Integration](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md).

### Berechtigungsfluss {#entitlement-flow}

Die Adobe Pass-Authentifizierung dient als Proxy und erleichtert den Berechtigungsfluss zwischen Programmierern und MVPDs, indem sie sichere und konsistente Schnittstellen für beide Parteien bietet.

Für Programmierer stellt die Adobe Pass-Authentifizierung APIs als Teil einer **Standard**- oder **Premium**-Ebene bereit:

* Standard-APIs zur Adobe Pass-Authentifizierung:
   * [REST-API-DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST-API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium Adobe Pass-Authentifizierungs-APIs:
   * [Temporäre Pass-API zurücksetzen](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass-Funktion](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [Abbauungs-API](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [Abbaumerkmal](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [Berechtigungs-Service-Überwachungs-API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

Weitere Informationen zum Ablauf von Berechtigungen finden Sie in der Dokumentation [Handbuch zur Programmiererintegration](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow).

#### Grundlegendes zu Berechtigungen {#understanding-entitlements}

Die Adobe Pass-Authentifizierungslösung umfasst die Erstellung von Berechtigungen - spezifische Datenelemente, die nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungs-Workflows generiert werden. Diese Berechtigungen gewähren Zugriff auf geschützte Inhalte, haben jedoch eine begrenzte Lebensdauer. Sobald eine Berechtigung abläuft, muss sie erneuert werden, indem die Authentifizierungs- oder Autorisierungsprozesse erneut initiiert werden.

Weitere Informationen zu Berechtigungen finden Sie in den folgenden Dokumenten:

* **Profile**

  Bei erfolgreicher Authentifizierung erstellt die Adobe Pass-Authentifizierung ein authentifiziertes Profil („langlebig„), das mit der ID der anfragenden Anwendung, des Geräts und des Dienstleisters (Anfordererkennung) verknüpft ist.

* **[Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Nach erfolgreicher Authentifizierung (und in einigen Fällen auch nach der Autorisierung) empfängt die Adobe Pass-Authentifizierung Benutzermetadaten von der MVPD, die sie für die anfragende Anwendung verfügbar machen.

* **[Entscheidungen](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung eine Autorisierungsentscheidung („langlebig„), die mit der anfragenden Anwendung, dem Gerät, der Kennung des Dienstleisters (Anfordererkennung) und einer bestimmten geschützten Ressource (Ressourcenkennung) verknüpft ist.

* **[Medien-Token](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung ein Medien-Token („kurzlebig„), das mit einer erfolgreichen Wiedergabeanfrage verknüpft ist, und bietet Unterstützung für Best Practices der Branche zur Risikominderung (z. B. Stream-Ripping).

Die TTL-Werte (Time-to-Live) für Profile und Entscheidungen werden auf der Grundlage von Vereinbarungen zwischen Programmierern und Pay-TV-Anbietern festgelegt, die sich auf einen Wert einigen, der allen Beteiligten am besten dient.

#### Erstellen der Benutzeroberfläche {#building-user-interface}

Programmierer sind für die Gestaltung und Implementierung der Benutzeroberfläche (User Interface, UI) für den Berechtigungs-Workflow innerhalb ihrer Website oder App verantwortlich, während bestimmte Elemente, wie der Anmeldeprozess, vom Pay-TV-Anbieter verwaltet werden.

Programmierer müssen mindestens:

* **Implementieren einer Schnittstelle zur Anbieterauswahl**
   * Ermöglichen Sie es neuen Nutzern, ihren Pay-TV-Anbieter zu identifizieren und sich zum ersten Mal anzumelden.
   * Einige Pay-TV-Anbieter leiten Benutzer zu einer externen Anmeldeseite weiter, während andere die Anmeldung innerhalb eines iFrames erfordern. Programmierer müssen eine Callback-Funktion implementieren, um bei Bedarf den iframe zu generieren.

* **Verwalten einer Liste unterstützter Pay-TV-Anbieter**
   * Stellen Sie sicher, dass Benutzer nur über genehmigte Anbieter auf Inhalte zugreifen können.

* **Authentifizierungsstatus angeben**
   * Anzeigen, wenn ein Benutzer in der App oder auf der Website authentifiziert wird.

* **Identifizieren geschützter Ressourcen**
   * Geben Sie klar an, welche Inhalte autorisiert werden müssen, bevor sie angezeigt werden.
   * Aktualisieren Sie die Benutzeroberfläche, um die erfolgreiche Autorisierung widerzuspiegeln, sobald der Zugriff gewährt wurde.

## FAQs {#faqs}

**Was ist TV Everywhere?**

TV Everywhere ist eine Initiative der Branche, die es Pay-TV-Kunden ermöglicht, auf Premium-Inhalte zuzugreifen, die sie bereits auf verschiedenen mit dem Internet verbundenen Geräten abonniert haben. Dazu gehören PCs, Tablets, Smartphones, Spielekonsolen, Set-Top-Boxen und Smart-TVs. Die größte Herausforderung besteht darin, einen nahtlosen und benutzerfreundlichen Authentifizierungsprozess sicherzustellen, der es Kunden ermöglicht, ohne mehrere Anmeldungen oder technische Hindernisse auf ihre Abonnementinhalte zuzugreifen.

**Was ist die Adobe Pass-Authentifizierung und wie unterstützt sie TV Everywhere?**

Die Adobe Pass-Authentifizierung erweckt TV Everywhere zum Leben, indem sie das Recht eines Nutzers auf Inhalte sicher und effizient verifiziert. Es handelt sich dabei um einen gehosteten Dienst, der eine schnelle Backend-Integration auf der Grundlage von Geschäftsregeln ermöglicht, die sowohl von Programmierern als auch von Pay-TV-Anbietern festgelegt werden. Dies führt zu einer kürzeren Markteinführungszeit für alle Beteiligten, einer sichereren Umgebung, die Betrug minimiert, und einem besseren Benutzererlebnis mit mehr TV-Inhalten, die über mehrere Plattformen zugänglich sind.

**Wie wird die Adobe Pass-Authentifizierung bereitgestellt?**

Die Adobe Pass-Authentifizierung wird als SaaS-Lösung (Software as a Service) bereitgestellt. Dieser Ansatz gewährleistet eine sichere Kommunikation zwischen Endbenutzern, Programmierern und Pay-TV-Anbietern für die Validierung von Inhaltsberechtigungen.

**Was unterscheidet die Adobe Pass-Authentifizierung von anderen TV Everywhere-Lösungen?**

Die Adobe Pass-Authentifizierung bietet mehrere Vorteile gegenüber alternativen Lösungen:

* **Nahtloses Single Sign-On (SSO)** - Im Gegensatz zu direkten Integrationen mit einzelnen Anbietern ermöglicht die Adobe Pass-Authentifizierung ein persistentes Anmeldeerlebnis, wenn Benutzende zwischen verschiedenen Websites und Programmen wechseln.
* **Weit reichende Marktdurchdringung** - Sobald ein Programmierer mit der Adobe Pass-Authentifizierung integriert ist, erhält er sofort Zugriff auf Pay-TV-Anbieter, die über 90 % der US-Haushalte abdecken.
* **Integration mit dem Adobe-Ökosystem** - arbeitet nahtlos mit anderen Adobe-Lösungen für die Bereitstellung, den Schutz und die Monetarisierung von Inhalten zusammen, einschließlich Adobe Analytics.

**Wie sicher ist die Adobe Pass-Authentifizierung?**

Sicherheit hat oberste Priorität. Die Adobe Pass-Authentifizierung stellt sicher, dass nur autorisierte Benutzerinnen und Benutzer auf Premium-Inhalte zugreifen können, indem der Zugriff an das Gerät der Benutzerin oder des Benutzers gebunden wird. Es bietet außerdem Optionen zum Begrenzen der Anzahl gleichzeitiger Streams, Sitzungen oder Geräte pro Haushalt.

**Welche Geräte unterstützt die Adobe Pass-Authentifizierung?**

Die Adobe Pass-Authentifizierung wurde für die Verwendung auf einer Vielzahl von Geräten entwickelt, z. B. Web-basierten Geräten, Mobilgeräten oder mit dem Fernsehgerät verbundenen Geräten.

**Kostet die Adobe Pass-Authentifizierung Endbenutzern etwas?**

Anzahl Die Adobe Pass-Authentifizierung ist für Endbenutzer kostenlos.
