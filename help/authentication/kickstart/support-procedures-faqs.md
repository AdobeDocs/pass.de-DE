---
title: Häufig gestellte Fragen zu Support-Verfahren
description: Häufig gestellte Fragen zu Support-Verfahren
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 1b9847d8dcb078755fd68a6363972f8973290e52
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Häufig gestellte Fragen zu Support-Verfahren {#support-procedures-faqs}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

In diesem Dokument werden die häufig gestellten Fragen (FAQs) zu Support-Verfahren für schwerwiegende Vorfälle (Schweregrad 1) beschrieben, die die Adobe Pass-Authentifizierung und ihre Partner betreffen.

## Häufig gestellte Fragen {#faqs}

### Was ist ein Vorfall der Stufe SEVERITY 1? {#support-procedures-faqs-1}

Ein Vorfall der Stufe SCHWEREGRAD 1 ist eine Live-Situation in der Produktionsumgebung, die den Abschluss von Authentifizierungs- oder Autorisierungsflüssen für einen Kanal und einen MVPD verhindert, was eine große Anzahl von Abonnenten betrifft.

Beispiele für Vorfälle mit SCHWEREGRAD 1

* Während des Authentifizierungsprozesses wird der Benutzer nicht zur Anmeldeseite umgeleitet, nachdem er in einem unterstützten Browser die MVPD ausgewählt hat.

* Während des Authentifizierungsprozesses hängt der Benutzer auf einer Adobe-Fehlerseite, ohne den Authentifizierungsfluss erneut starten zu können.

* Der -Partner erhält zahlreiche Berichte, dass sich Benutzende nicht mit einer bestimmten MVPD authentifizieren oder autorisieren können.

* Der unter https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js gehostete Production Access Enabler ist nicht verfügbar.

### Was ist ein Nicht-Schweregrad-1-Vorfall?

Adobe unterstützt Untersuchungen zu diesen Problemen, sie werden jedoch nicht als Vorfälle der Stufe 1 eingestuft:

* Mindestens ein Abonnent kann sich nicht authentifizieren und auf der Anmeldeseite von MVPD bleiben.

* Mindestens ein Abonnent ist authentifiziert, kann jedoch keine Videos abspielen.

* Einige oder alle Abonnentinnen und Abonnenten sehen auf der Programmierer-Site einen JavaScript-Fehler.

### Wie werden Vorfälle der Stufe 1 gehandhabt?

Ein Vorfall der Stufe SEVERITY 1 kann entweder von Adobe oder einem Adobe Pass-Authentifizierungspartner eingeleitet werden. Die einzelnen Schritte sind unten beschrieben.

**Vom Partner initiierter Fluss**

1. Der Partner identifiziert einen Vorfall der Stufe 1, der sofortige Aufmerksamkeit der Adobe erfordert.

1. Der Partner sendet eine E-Mail an **tve-support@adobe.com**, einschließlich **DRINGEND -**) in der Betreffzeile und fügt die folgenden Informationen hinzu:
   * Titel
   * Beschreibung und Schritte zur Reproduktion
   * Betriebssystem/Browser
   * SDK und Version
   * Betroffene Geräte
   * % Benutzende betroffen
   * Das Problem demonstrierende HTTP-Trace- oder Geräteprotokolle
   * (Optional) Alle verfügbaren Screenshots oder Videoaufnahmen, die das Problem zeigen

1. Wenn Adobe nicht innerhalb eines Zeitraums auf das Ticket antwortet, kann der Partner die folgende Nummer anrufen: **1-205-693-9813**.

>[!IMPORTANT]
>
> Wenn Sie „URGENT-INCIDENT“ nicht in den Titel des Tickets aufnehmen, wird es von unserem Benachrichtigungssystem nicht abgeholt.

**Adobe-initiierter Fluss**

Bei einem Adobe Pass-Authentifizierungsproblem:

1. Adobe identifiziert ein internes Problem und öffnet ein Ticket in unserem Tracking-System.

1. Adobe benachrichtigt den Programmmanager und den technischen Ansprechpartner des Partners und gibt die Ticketnummer sowie die voraussichtliche Auswirkung des Problems an.

1. Adobe arbeitet daran, den Vorfall zu beheben und hält alle betroffenen Partner auf dem Laufenden.

Für ein Partnerproblem (Programmierer/MVPD):

1. Adobe kennzeichnet ein Problem im Zusammenhang mit der Integration mit einer MVPD oder auf einer der Sites des Programmierers.

1. Adobe benachrichtigt den betroffenen Partner nach Abschluss der Support-Verfahren, die für diesen Partner gelten, und eröffnet ein Ticket bei der Support-Organisation des Partners.

1. Wenn Adobe bei der Folgenabschätzung feststellt, dass das Problem unter eine der im Voraus vereinbarten Entscheidungen über Vorfallsszenarien fällt, wird sie entsprechend handeln, ohne auf die Beiträge des Partners zu warten.

1. Adobe wartet auf Updates des Partners und benachrichtigt Sie, wenn der Service wiederhergestellt wurde.

### Was sind vorab vereinbarte Entscheidungen über Vorfallszenarien?

Bestimmte Situationen mit Standardaktionen, die bei Eintreten des Szenarios ausgeführt werden:

|    | Szenario | Beschreibung | Aktionen |
|----|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe kennzeichnet ein Problem mit der MVPD-Integration während des normalen Produktionsbetriebs. | Bei normalen Produktionsvorgängen erkennt Adobe ein Problem mit einem der MVPDs, das die Durchführung der Authentifizierungs-/Autorisierungsflüsse unmöglich macht (z. B. abgelaufene Zertifikate, abgelaufene SAML-Antworten, geschlossene Ports, geänderte Parameter usw.) | Adobe benachrichtigt die betroffenen MVPD und Programmierer.  </br></br> Adobe deaktiviert diese MVPD für alle betroffenen Programmierer. </br></br> Adobe öffnet ein Ticket bei der MVPD nach dem vereinbarten Support-Verfahren mit dieser MVPD |
| S2 | Adobe aktiviert eine neue MVPD für einen Programmierer, und der Programmierer lässt die MVPD vor dem Launch-Datum zu. | Adobe aktiviert eine neue MVPD für die Website eines Programmierers, und die Website zeigt die neue MVPD bereits in der Auswahl an, selbst wenn sie nicht hätte angezeigt werden sollen. | Adobe wird den Programmierer vor dem geplanten Datum über die neue MVPD in der Auswahl informieren. </br></br> Programmierer wird bei Bedarf Maßnahmen ergreifen, um sie aus der Auswahl zu entfernen. |
| S3 | Adobe aktiviert eine neue MVPD für einen Programmierer, selbst wenn die MVPD nicht produktionsbereit ist | Adobe aktiviert eine neue MVPD für einen Programmierer, aber MVPD hat noch keine Unterstützung für die Integration bereitgestellt, sodass die Authentifizierungs-/Autorisierungsflüsse nicht ausgeführt werden können | Adobe führt die Bereitstellung nur durch, wenn vom Programmierer angefordert </br></br> Der Programmierer ist dafür verantwortlich, die Genehmigung der MVPD sicherzustellen, sobald alle Tests durchgeführt wurden. |
