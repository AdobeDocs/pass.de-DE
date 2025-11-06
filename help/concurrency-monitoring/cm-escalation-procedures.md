---
title: Eskalationsverfahren zur Überwachung von gleichzeitigen Aufrufen
description: Eskalationsverfahren zur Überwachung von gleichzeitigen Aufrufen
exl-id: eb110465-3a74-489e-a521-0e17f5aeecb8
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Eskalationsverfahren zur Überwachung von gleichzeitigen Aufrufen {#esc-procedures}

>[!NOTE]
>
>Rufen Sie die Hotline unter +1-205-693-9813 an und senden Sie eine E-Mail an `tve-support@adobe.com`, die in der Betreffzeile „DRINGEND - VORFALL“ enthält.


## Einführung {#cm-escalation-intro}

In diesem Dokument werden die Support-Verfahren für schwerwiegende Vorfälle (**SCHWEREGRAD 1** Ebene) beschrieben, die die Adobe Pass-Authentifizierung, die Adobe Pass-Gleichzeitigkeitsüberwachung und die -Partner betreffen.

## Definition der Eskalationsstufe 1 {#defn-escl-sevrityone-level}

Ein Vorfall **SCHWEREGRAD 1** ist eine **LIVE**-Situation, **in der Produktionsumgebung**, die den Abschluss der Authentifizierungs- und/oder Autorisierungsflüsse für einen Kanal und einen MVPD nicht zulässt und eine große Anzahl von Abonnentinnen und Abonnenten des MVPD, die den Fluss durchführen, betrifft.

## Beispiele für Vorfälle mit Schweregrad 1 {#exampl-sevone-incident}

* Der auf <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> gehostete Produktionszugriffs-Enabler ist nicht verfügbar.

* Für eine bestimmte MVPD leitet Adobe die Anmeldeseite nicht mehr um bzw. zeigt diese an, nachdem der Benutzer die MVPD ausgewählt hat (in einem der unterstützten Browser).

* Der -Partner erhält eine große Anzahl von Berichten, die die Benutzerinnen und Benutzer nicht mit einer bestimmten MVPD authentifizieren/autorisieren können.

* Während des Authentifizierungsprozesses bleibt der Benutzer auf einer Adobe-Fehlerseite stecken, ohne dass der Authentifizierungs-/Autorisierungsfluss erneut initiiert werden kann.


## Beispiele für &quot;*&quot;* Vorfall vom Schweregrad 1 {#exampl-not-sev1}

*Bei Problemen dieser Art bietet Adobe Unterstützung für Ermittlungen, es handelt sich jedoch nicht um Vorfälle mit Schweregrad 1:*

* Ein oder mehrere Abonnenten können den Fluss aufgrund eines Flash-Versionsproblems nicht ausführen (fehlende Flash, Flash-Blocker, falsche Flash-Version).
* Mindestens ein Abonnent kann sich nicht authentifizieren und auf der Anmeldeseite von MVPD bleiben.
* Ein oder mehrere Abonnenten sind authentifiziert, können jedoch keine Videos abspielen.
* Ein Abonnent/wenige Abonnenten/alle Abonnenten bemerken einen JavaScript-Fehler auf der Programmierer-Site.

## Eskalationsflüsse von Schweregrad 1 {#sevone-escalation-flows}

Vorfälle des Schweregrads 1 können entweder von Adobe oder einem Adobe Pass-Authentifizierungspartner initiiert werden. Die einzelnen Schritte werden im Folgenden beschrieben.

### Vom Partner initiierter Fluss {#partner-initiated-flow}

1. Der Partner identifiziert einen Vorfall mit Schweregrad 1 (wie oben beschrieben), der ein sofortiges Eingreifen von Adobe erfordert.

1. Der Partner sendet eine E-Mail an tve-support@adobe.com, die in der Betreffzeile „DRINGEND - VORFALL“ enthält und die folgenden Informationen hinzufügt:

   * Titel
   * Beschreibung und Schritte zur Reproduktion
   * Betriebssystem
   * Browser
   * Flash-Version
   * (Optional) Alle verfügbaren Screenshots oder Videoaufnahmen, die das Problem zeigen

1. Wenn Adobe nicht innerhalb von 30 Minuten auf das Ticket antwortet, ruft der Partner die unten stehende Nummer an:

   * **-205-693-9813**


**Wenn Sie „URGENT-INCIDENT“ nicht in den Titel des Tickets aufnehmen, wird es von unserem Benachrichtigungssystem nicht abgeholt.**

### Adobe-initiierter Fluss {#adobe-initiated-flow}

**…bei einem Adobe Pass-Authentifizierungsproblem**

1. Adobe identifiziert ein internes Problem und öffnet ein Ticket mit unserem Tracking-System.

1. Adobe benachrichtigt den Programmmanager und den technischen Ansprechpartner des Partners und gibt die Ticketnummer sowie die voraussichtliche Auswirkung des Problems an.

1. Adobe arbeitet an der Behebung des Vorfalls und hält alle betroffenen Partner auf dem Laufenden.


**…für ein Partnerproblem (Programmierer/MVPD)**

1. Adobe kennzeichnet ein Problem im Zusammenhang mit der Integration mit einer MVPD oder auf einer der Sites des Programmierers.

1. Adobe benachrichtigt den betroffenen Partner **nach den bei diesem Partner** Support-Verfahren) und eröffnet ein Ticket bei der Support-Organisation des Partners.

1. Wenn Adobe während der Folgeanalyse feststellt, dass das Problem zu einer der im Voraus vereinbarten Entscheidungen über Vorfallsszenarien gehört (siehe Abschnitt „Vorab vereinbarte Entscheidungen über Vorfallsszenarien“ unten), handelt es entsprechend, ohne auf den Partner zu warten1. s Eingabe.

1. Adobe wartet auf Updates des Partners und auf eine Benachrichtigung des Partners, wenn der Service wiederhergestellt wurde.

### Vorab vereinbarte Entscheidungen über Vorfallsszenarien {#pre-agreed-decisions}

Es gibt einige Situationen, in denen im Falle des Auftretens dieses Szenarios eine Standardaktion ausgeführt wird:

|    | Szenario | Beschreibung | Aktionen |
|:---:|:---|:---|:---|
| S1 | Adobe identifiziert ein Problem mit der MVPD-Integration während des normalen Produktionsbetriebs. | Während des normalen Produktionsbetriebs erkennt Adobe ein Problem mit einem der MVPDs, das die Durchführung der Authentifizierungs-/Autorisierungsflüsse unmöglich macht (z. B. abgelaufene Zertifikate, abgelaufene SAML-Antworten, geschlossene Ports, geänderte Parameter usw.) | Adobe benachrichtigt die betroffenen MVPD und Programmierer. Adobe deaktiviert diese MVPD für alle betroffenen Programmierer. Adobe eröffnet ein Ticket mit der MVPD nach dem vereinbarten Support-Verfahren mit dieser MVPD |
| S2 | Adobe aktiviert eine neue MVPD für einen Programmierer, und der Programmierer erstellt vor dem Launch-Datum eine Whitelist für MVPD. | Adobe aktiviert eine neue MVPD für die Site eines Programmierers, und die Site zeigt die neue MVPD bereits in der Auswahl an, selbst wenn sie nicht vorgesehen war. | Adobe benachrichtigt den Programmierer, dass die neue MVPD vor dem geplanten Datum in der Auswahl angezeigt wird. Der Programmierer wird bei Bedarf Maßnahmen ergreifen, um sie aus der Auswahl zu entfernen. |
| S3 | Adobe aktiviert eine neue MVPD für einen Programmierer, selbst wenn die MVPD nicht produktionsbereit ist | Adobe aktiviert eine neue MVPD für einen Programmierer, aber die MVPD hat die Unterstützung für die Integration noch nicht bereitgestellt, sodass die Authentifizierungs-/Autorisierungsflüsse nicht ausgeführt werden können | Adobe führt die Bereitstellung nur durch, wenn vom Programmierer dazu aufgefordert wird. Der Programmierer ist dafür verantwortlich, die Whitelist der MVPD sicherzustellen, sobald alle Tests durchgeführt wurden. |

### Erwartete Reaktionen auf Vorfälle mit Schweregrad 1 {#response-expectations}

* Erste Reaktion: 30 Minuten (24/7)
* Aktionsplan: 1 Stunde (24/7)
* Lösung: so bald wie möglich (24/7)
