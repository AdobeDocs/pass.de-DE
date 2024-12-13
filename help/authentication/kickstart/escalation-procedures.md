---
title: Eskalationsverfahren
description: Eskalationsverfahren
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# Eskalationsverfahren {#escalation-procedures}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
> 
>Rufen Sie die Hotline an: **+1-205-693-9813** und senden Sie eine E-Mail an **tve-support@adobe.com** einschließlich **DRINGEND - VORFALL** in der Betreffzeile.

## Einführung {#introduction}

In diesem Dokument werden die Support-Verfahren für schwerwiegende Vorfälle (**Schweregrad 1** Ebene) beschrieben, die die Adobe Pass-Authentifizierung, die Adobe Pass-Parallelitätsüberwachung und deren Partner betreffen.


## Definition eines Vorfalls mit Schweregrad 1 {#definition-of-a-severity-1-level-incident}

Ein Vorfall **SCHWEREGRAD 1** ist eine **LIVE**-Situation, **in der Produktionsumgebung**, die den Abschluss der Authentifizierungs- und/oder Autorisierungsflüsse für einen Kanal und einen MVPD nicht zulässt und eine große Anzahl von Abonnentinnen und Abonnenten des MVPD, die den Fluss durchführen, betrifft.


## Beispiele für Vorfälle mit SCHWEREGRAD 1 {#examples-of-severity-1-incidentcs}

* Der auf `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (oder `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) gehostete Produktionszugriffs-Enabler ist nicht verfügbar.

* Für eine bestimmte MVPD leitet Adobe die Anmeldeseite nicht mehr um bzw. zeigt diese an, nachdem der Benutzer die MVPD ausgewählt hat (in einem der unterstützten Browser).

* Der -Partner erhält eine große Anzahl von Berichten, die die Benutzerinnen und Benutzer nicht mit einer bestimmten MVPD authentifizieren/autorisieren können.

* Während des Authentifizierungsprozesses wird der Anwender auf einer Adobe-Fehlerseite festgehalten, ohne dass der Authentifizierungs-/Autorisierungsfluss erneut initiiert werden kann.


| Beispiele für &quot;**&quot;** Vorfall vom Schweregrad 1 |
|---|
| Bei Problemen dieser Art bietet Adobe Unterstützung für Ermittlungen, es handelt sich jedoch nicht um Vorfälle mit Schweregrad 1:<ul><li>Ein oder mehrere Abonnenten können den Vorgang aufgrund eines Flash-Versionsproblems nicht ausführen (fehlende Flash, Flash-Blocker, falsche Flash-Version).</li><li>Mindestens ein Abonnent kann sich nicht authentifizieren und auf der Anmeldeseite von MVPD bleiben.</li><li>Ein oder mehrere Abonnenten sind authentifiziert, können jedoch keine Videos abspielen.</li><li>Ein Abonnent/wenige Abonnenten/alle Abonnenten sehen auf der Programmierer-Site einen JavaScript-Fehler</li></ul> |

## Eskalationsflüsse von Schweregrad 1 {#severity-1-escalation-flows}

Vorfälle des Schweregrads 1 können entweder von einer Adobe oder einem Adobe Pass-Authentifizierungspartner initiiert werden. Die einzelnen Schritte werden im Folgenden beschrieben.

### Vom Partner initiierter Fluss {#partner-initiated-flow}

1. Der Partner identifiziert einen Vorfall mit Schweregrad 1 (wie oben beschrieben), der sofortiges Eingreifen der Adobe erfordert.
1. Der Partner sendet eine E-Mail an **tve-support@adobe.com**, einschließlich **DRINGEND -**) in der Betreffzeile und fügt die folgenden Informationen hinzu:
   * Titel
   * Beschreibung und Schritte zur Reproduktion
   * Betriebssystem/Browser
   * SDK und Version
   * Betroffene Geräte
   * % Benutzende betroffen
   * Das Problem demonstrierende HTTP-Trace- oder Geräteprotokolle
   * (Optional) Alle verfügbaren Screenshots oder Videoaufnahmen, die das Problem zeigen
1. Wenn Adobe nicht innerhalb von 30 Minuten auf das Ticket antwortet, ruft der Partner die folgende Nummer an:
   **-205-693-9813**
   >[!IMPORTANT]
   >Wenn Sie „URGENT-INCIDENT“ nicht in den Titel des Tickets aufnehmen, wird es von unserem Benachrichtigungssystem nicht abgeholt**.

### Adobe-initiierter Fluss {#adobe-initiated-flow}

#### …für ein Problem mit der Adobe Pass-Authentifizierung {#adobe-initiated-flow-authn-issue}

1. Adobe identifiziert ein internes Problem und öffnet ein Ticket mit unserem Tracking-System.

1. Adobe benachrichtigt den Programmmanager und den technischen Ansprechpartner des Partners und gibt die Ticketnummer sowie die voraussichtliche Auswirkung des Problems an.

1. Adobe arbeitet an der Behebung des Vorfalls und hält alle betroffenen Partner auf dem Laufenden.

#### …für ein Partnerproblem (Programmierer/MVPD) {#adobe-initiated-flow-partner-issue}

1. Adobe kennzeichnet ein Problem im Zusammenhang mit der Integration mit einer MVPD oder auf einer der Sites des Programmierers.

1. Adobe benachrichtigt den betroffenen Partner <u>nach den bei diesem Partner eingerichteten Support-Verfahren</u> und eröffnet ein Ticket bei der Support-Organisation des Partners.

1. Wenn Adobe während der Folgeanalyse feststellt, dass das Problem zu einer der im Voraus vereinbarten Entscheidungen zu Vorfallsszenarien gehört, siehe **Vorab vereinbarte Entscheidungen zu Vorfallsszenarien**, handelt sie entsprechend, ohne auf die Eingabe des Partners zu warten.

1. Adobe wartet auf Updates des Partners und auf eine Benachrichtigung des Partners, wenn der Service wiederhergestellt wurde.

## Vorab vereinbarte Entscheidungen über Vorfallsszenarien {#pre-agreed-descn}

Es gibt einige Situationen, in denen im Falle des Auftretens dieses Szenarios eine Standardaktion ausgeführt wird:

|   | Szenario | Beschreibung | Aktionen |
|---|---|---|---|
| S1 | Adobe kennzeichnet ein Problem mit der MVPD-Integration während des normalen Produktionsbetriebs. | Bei normalen Produktionsvorgängen erkennt Adobe ein Problem mit einem der MVPDs, das die Durchführung der Authentifizierungs-/Autorisierungsflüsse unmöglich macht (z. B. abgelaufene Zertifikate, abgelaufene SAML-Antworten, geschlossene Ports, geänderte Parameter usw.) | - Adobe benachrichtigt die betroffenen MVPD und Programmierer.  </br> </br> - Adobe deaktiviert diese MVPD für alle betroffenen Programmierer. </br> </br> - Adobe öffnet ein Ticket bei der MVPD nach dem vereinbarten Support-Verfahren mit dieser MVPD |
| S2 | Adobe aktiviert eine neue MVPD für einen Programmierer, und der Programmierer lässt die MVPD vor dem Launch-Datum zu. | Adobe aktiviert eine neue MVPD für die Website eines Programmierers, und die Website zeigt die neue MVPD bereits in der Auswahl an, selbst wenn sie nicht hätte angezeigt werden sollen. | - Adobe wird den Programmierer vor dem geplanten Datum über die neue MVPD in der Auswahl informieren. </br> </br> : Der Programmierer ergreift bei Bedarf Maßnahmen, um sie aus der Auswahl zu entfernen. |
| S3 | Adobe aktiviert eine neue MVPD für einen Programmierer, selbst wenn die MVPD nicht produktionsbereit ist | Adobe aktiviert eine neue MVPD für einen Programmierer, aber MVPD hat noch keine Unterstützung für die Integration bereitgestellt, sodass die Authentifizierungs-/Autorisierungsflüsse nicht ausgeführt werden können | - Adobe führt die Bereitstellung nur durch, wenn vom Programmierer </br> angefordert </br> - Der Programmierer ist dafür verantwortlich, die Genehmigung der MVPD sicherzustellen, sobald alle Tests durchgeführt wurden. |

## Erwartete Reaktionen auf Vorfälle mit Schweregrad 1 {#response-expectations-for-severity-one-incidents}

* Erste Reaktion: 30 Minuten (24/7)
* Aktionsplan: 1 Stunde (24/7)
* Lösung: so bald wie möglich (24/7)
