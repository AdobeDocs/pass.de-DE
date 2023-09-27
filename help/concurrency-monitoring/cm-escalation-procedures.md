---
title: Eskalationsverfahren für die gleichzeitige Überwachung
description: Eskalationsverfahren für die gleichzeitige Überwachung
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '823'
ht-degree: 0%

---


# Eskalationsverfahren für die gleichzeitige Überwachung {#esc-procedures}

>[!NOTE]
>
>Rufen Sie die Hotline an: +1-205-693-9813 und senden Sie eine E-Mail an `tve-support@adobe.com` einschließlich &quot;DRINGEND - INCIDENT&quot;in der Betreffzeile.


## Einführung {#cm-escalation-intro}

In diesem Dokument werden die Unterstützungsverfahren für größere Zwischenfälle beschrieben (**SCHWERE 1** Ebene), die sich auf die Adobe Pass-Authentifizierung, die Adobe Pass-Überwachung der Parallelität und ihre Partner auswirken.

## Definition der Eskalation Schweregrad 1 Level {#defn-escl-sevrityone-level}

A **SCHWERE 1** Level-Vorfall ist **LIVE** Situation, **in der Produktionsumgebung**, das nicht den Abschluss der Authentifizierungs- und/oder Autorisierungsflüsse für einen Kanal und einen MVPD zulässt, was eine große Anzahl von Abonnenten des MVPD betrifft, der den Fluss durchführt.

## Beispiele für Fälle von Schweregrad 1 {#exampl-sevone-incident}

* Der Produktions-Access-Enabler, der unter <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> ist nicht verfügbar.

* Bei einem bestimmten MVPD leitet Adobe die Anmeldeseite nicht mehr weiter/zeigt sie an, nachdem der Benutzer das MVPD (in einem der unterstützten Browser) ausgewählt hat.

* Der Partner erhält eine große Anzahl von Berichten, denen zufolge Benutzer sich nicht mit einem bestimmten MVPD authentifizieren/autorisieren können.

* Während des Authentifizierungsprozesses hängt der Benutzer auf einer Adobe-Fehlerseite, ohne den Authentifizierungs-/Autorisierungsfluss neu starten zu können.


## Beispiele für *NOT* Schweregrad 1 {#exampl-not-sev1}

*Bei Problemen dieser Art wird Adobe Untersuchungen unterstützen, es handelt sich jedoch nicht um Fälle von Schweregrad 1:*

* Ein oder mehrere Abonnenten können den Ablauf aufgrund eines Flash-Versionsfehlers (fehlende Flash, Flash-Blocker, falsche Flash-Version) nicht durchführen.
* Ein oder mehrere Abonnenten können sich nicht authentifizieren und bleiben auf der MVPD-Anmeldeseite.
* Ein oder mehrere Abonnenten sind authentifiziert, können jedoch keine Videos abspielen.
* Bei einem/mehreren Abonnenten tritt auf der Programmierer-Site ein JavaScript-Fehler auf.

## Eskalationsflüsse des Schweregrads 1 {#sevone-escalation-flows}

Vorfälle des Schweregrads 1 können entweder von Adobe oder einem Adobe Pass-Authentifizierungspartner eingeleitet werden. Die einzelnen Schritte werden nachfolgend beschrieben.

### Vom Partner initiierter Fluss {#partner-initiated-flow}

1. Der Partner identifiziert einen Vorfall des Schweregrads 1 (wie oben beschrieben), der eine sofortige Aufmerksamkeit der Adobe erfordert.

1. Der Partner sendet eine E-Mail an tve-support@adobe.com , in der in der Betreffzeile &quot;URGENT - INCIDENT&quot;enthalten ist und folgende Informationen hinzugefügt werden:

   * Titel
   * Beschreibung und Schritte zum Reproduzieren
   * BS
   * Browser
   * Flash-Version
   * (Optional) Alle verfügbaren Screenshots oder Videoaufnahmen, die das Problem demonstrieren

1. Wenn Adobe nicht innerhalb von 30 Minuten auf das Ticket antwortet, ruft der Partner die unten stehende Nummer auf:

   * **1-205-693-9813**


**Wenn Sie &quot;URGENT-INCIDENT&quot;nicht in den Titel des Tickets aufnehmen, wird dies nicht von unserem Benachrichtigungssystem übernommen.**

### Adobe-initiierter Fluss {#adobe-initiated-flow}

**...bei einem Adobe Pass-Authentifizierungsproblem**

1. Adobe identifiziert ein internes Problem und öffnet ein Ticket mit unserem Tracking-System.

1. Adobe informiert den Programmmanager und den technischen Ansprechpartner des Partners unter Angabe der Ticketnummer und der geschätzten Auswirkungen des Problems.

1. Adobe arbeitet an der Lösung des Vorfalls und informiert alle betroffenen Partner.


**...für ein Partnerproblem (Programmierer/MVPD)**

1. Adobe identifiziert ein Problem im Zusammenhang mit der Integration mit einem MVPD oder auf einer der Sites des Programmierers.

1. Adobe benachrichtigt den betroffenen Partner **nach den mit diesem Partner bestehenden Unterstützungsverfahren** und öffnet ein Ticket bei der Support-Organisation des Partners.

1. Wenn Adobe bei der Folgenabschätzung feststellt, dass das Problem zu einer der im Voraus vereinbarten Entscheidungen über Störungsszenarien gehört (siehe Abschnitt &quot;Vorab vereinbarte Entscheidungen über Störungsszenarien&quot;unten), wird es entsprechend handeln, ohne auf den Partner1 zu warten. -Eingabe.

1. Adobe wartet auf Updates vom Partner und eine Benachrichtigung vom Partner, sobald der Dienst wiederhergestellt wurde.

### Vorab vereinbarte Beschlüsse über Unfallszenarien {#pre-agreed-decisions}

Es gibt einige Situationen, in denen eine Standardaktion ausgeführt wird, falls dieses Szenario auftritt:

|    | Szenario | Beschreibung | Aktionen |
|:---:|:---|:---|:---|
| S1 | Adobe identifiziert ein Problem mit der Integration eines MVPD während normaler Produktionsvorgänge. | Bei normalen Produktionsvorgängen erkennt Adobe ein Problem mit einem der MVPDs, das die Ausführung der Authentifizierungs-/Autorisierungsflüsse unmöglich macht (z. B. abgelaufene Zertifikate, abgelaufene SAML-Antworten, geschlossene Ports, geänderte Parameter usw.) | Adobe benachrichtigt die betroffenen MVPD- und Programmierer. Adobe deaktiviert diesen MVPD für alle betroffenen Programmierer. Adobe öffnet ein Ticket mit dem MVPD nach dem vereinbarten Supportverfahren mit diesem MVPD |
| S2 | Adobe aktiviert einen neuen MVPD für einen Programmierer, und der Programmierer weiß den MVPD vor dem Startdatum. | Adobe aktiviert einen neuen MVPD für die Seite eines Programmierers, und die Site zeigt den neuen MVPD bereits in der Auswahl an, selbst wenn dies nicht vorgesehen war. | Adobe wird den Programmierer vor dem geplanten Datum über das neue MVPD in der Auswahl informieren. Programmierer ergreifen Maßnahmen, um sie bei Bedarf aus der Auswahl zu entfernen. |
| S3 | Adobe aktiviert einen neuen MVPD für einen Programmierer, selbst wenn der MVPD nicht bereit ist, in die Produktion zu gehen | Adobe aktiviert einen neuen MVPD für einen Programmierer, aber der MVPD hat die Unterstützung für die Integration noch nicht bereitgestellt, sodass die Authentifizierungs-/Autorisierungsflüsse nicht ausgeführt werden können | Adobe wird die Bereitstellung nur durchführen, wenn der Programmierer dies verlangt. Der Programmierer ist dafür verantwortlich, die Whitelist des MVPD zu gewährleisten, sobald alle Tests durchgeführt wurden. |

### Reaktionserwartungen bei Schweregraden 1 Unfälle {#response-expectations}

* Erstes Ansprechen: 30 Minuten (24/7)
* Aktionsplan: 1 Stunde (24/7)
* Lösung: ASAP (24/7)
