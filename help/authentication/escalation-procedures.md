---
title: Eskalationsverfahren
description: Eskalationsverfahren
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# Eskalationsverfahren {#escalation-procedures}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
> 
>Rufen Sie die Hotline auf: **+1-205-693-9813** und senden Sie eine E-Mail an **tve-support@adobe.com** einschließlich **DRINGEND - INCIDENT** in der Betreffzeile.

## Einführung {#introduction}

In diesem Dokument werden die Supportverfahren für größere Vorfälle (Ebene **SEVERITY 1**) beschrieben, die sich auf die Adobe Pass-Authentifizierung, die Adobe Pass-Überwachung der Parallelität und ihre Partner auswirken.


## Definition eines Ereignisses der Stufe 1 der SEVERITY-Stufe {#definition-of-a-severity-1-level-incident}

Ein Vorfall auf Ebene **SEVERITY 1** ist eine **LIVE**-Situation, **die in der Produktionsumgebung stattfindet**, die den Abschluss der Authentifizierungs- und/oder Autorisierungsflüsse für einen Kanal und einen MVPD nicht zulässt, was eine große Anzahl von Abonnenten des MVPD betrifft, der den Fluss durchführt.


## Beispiele für Vorfälle von SEVERITY 1 {#examples-of-severity-1-incidentcs}

* Der unter `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (oder `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) gehostete Produktions-Access-Enabler ist nicht verfügbar.

* Bei einem bestimmten MVPD leitet Adobe die Anmeldeseite nicht mehr weiter/zeigt sie an, nachdem der Benutzer das MVPD (in einem der unterstützten Browser) ausgewählt hat.

* Der Partner erhält eine große Anzahl von Berichten, denen zufolge Benutzer sich nicht mit einem bestimmten MVPD authentifizieren/autorisieren können.

* Während des Authentifizierungsprozesses hängt der Benutzer auf einer Adobe-Fehlerseite, ohne den Authentifizierungs-/Autorisierungsfluss neu starten zu können.


| Beispiele für den Vorfall vom Typ **NOT** wegen Schweregrad 1 |
|---|
| Bei Problemen dieser Art wird Adobe Untersuchungen unterstützen, es handelt sich jedoch nicht um Fälle von Schweregrad 1:<ul><li>Ein oder mehrere Abonnenten können den Ablauf aufgrund eines Flash-Versionsfehlers (fehlende Flash, Flash-Blocker, falsche Flash-Version) nicht durchführen.</li><li>Ein oder mehrere Abonnenten können sich nicht authentifizieren und bleiben auf der MVPD-Anmeldeseite.</li><li>Ein oder mehrere Abonnenten sind authentifiziert, können jedoch keine Videos abspielen.</li><li>Bei einem/mehreren Abonnenten tritt auf der Programmierer-Site ein JavaScript-Fehler auf.</li></ul> |

## Eskalationsflüsse des Schweregrads 1 {#severity-1-escalation-flows}

Vorfälle des Schweregrads 1 können entweder von Adobe oder einem Adobe Pass-Authentifizierungspartner eingeleitet werden. Die einzelnen Schritte werden nachfolgend beschrieben.

### Vom Partner initiierter Fluss {#partner-initiated-flow}

1. Der Partner identifiziert einen Vorfall des Schweregrads 1 (wie oben beschrieben), der eine sofortige Aufmerksamkeit der Adobe erfordert.
1. Der Partner sendet eine E-Mail an &quot;**tve-support@adobe.com**&quot;, die in der Betreffzeile den Eintrag **URGENT - INCIDENT** enthält, und fügt die folgenden Informationen hinzu:
   * Titel
   * Beschreibung und Schritte zum Reproduzieren
   * Betriebssystem/Browser
   * SDK und Version
   * Betroffene Geräte
   * % der betroffenen Verwender
   * HTTP-Ablaufverfolgungs- oder Geräteprotokolle, die das Problem zeigen
   * (Optional) Alle verfügbaren Screenshots oder Videoaufnahmen, die das Problem demonstrieren
1. Wenn Adobe nicht innerhalb von 30 Minuten auf das Ticket antwortet, ruft der Partner die folgende Nummer auf:
   **1-205-693-9813**
   >[!IMPORTANT]
   >Wenn Sie &quot;URGENT-INCIDENT&quot;nicht in den Titel des Tickets aufnehmen, wird dies nicht von unserem Benachrichtigungssystem abgerufen.

### Adobe-initiierter Fluss {#adobe-initiated-flow}

#### ...bei einem Adobe Pass-Authentifizierungsproblem {#adobe-initiated-flow-authn-issue}

1. Adobe identifiziert ein internes Problem und öffnet ein Ticket mit unserem Tracking-System.

1. Adobe informiert den Programmmanager und den technischen Ansprechpartner des Partners über die Ticketnummer und die voraussichtliche Auswirkung des Problems.

1. Adobe arbeitet an der Lösung des Vorfalls und informiert alle betroffenen Partner.

#### ...für ein Partnerproblem (Programmierer/MVPD) {#adobe-initiated-flow-partner-issue}

1. Adobe identifiziert ein Problem im Zusammenhang mit der Integration mit einem MVPD oder auf einer der Sites des Programmierers.

1. Adobe benachrichtigt den betroffenen Partner <u>über die Einhaltung der mit diesem Partner bestehenden Supportverfahren</u> und öffnet ein Ticket bei der Support-Organisation des Partners.

1. Wenn Adobe bei der Folgenabschätzung feststellt, dass das Problem zu einer der vorab vereinbarten Entscheidungen über Störungsszenarien gehört, siehe **Vorab vereinbarte Entscheidungen über Unfallszenarien**, wird es entsprechend gehandelt, ohne auf den Beitrag des Partners zu warten.

1. Adobe wartet auf Updates vom Partner und eine Benachrichtigung vom Partner, sobald der Dienst wiederhergestellt wurde.

## Vorab vereinbarte Beschlüsse über Unfallszenarien {#pre-agreed-descn}

Es gibt einige Situationen, in denen eine Standardaktion ausgeführt wird, falls dieses Szenario auftritt:

|   | Szenario | Beschreibung | Aktionen |
|---|---|---|---|
| S1 | Adobe identifiziert ein Problem mit der Integration eines MVPD während normaler Produktionsvorgänge. | Bei normalen Produktionsvorgängen erkennt Adobe ein Problem mit einem der MVPDs, das die Ausführung der Authentifizierungs-/Autorisierungsflüsse unmöglich macht (z. B. abgelaufene Zertifikate, abgelaufene SAML-Antworten, geschlossene Ports, geänderte Parameter usw.) | - Adobe benachrichtigt die betroffenen MVPD- und Programmierer.  </br> </br> - Adobe deaktiviert diesen MVPD für alle betroffenen Programmierer. </br> </br> - Adobe öffnet ein Ticket beim MVPD nach dem vereinbarten Supportverfahren mit diesem MVPD. |
| S2 | Adobe aktiviert einen neuen MVPD für einen Programmierer, und der Programmierer lässt den MVPD vor dem Startdatum zu. | Adobe aktiviert einen neuen MVPD für die Seite eines Programmierers, und die Site zeigt den neuen MVPD bereits in der Auswahl an, selbst wenn dies nicht vorgesehen war. | - Adobe wird den Programmierer vor dem geplanten Datum über das neue MVPD in der Auswahl informieren. </br> </br> - Der Programmierer ergreift Maßnahmen, um ihn bei Bedarf aus der Auswahl zu entfernen. |
| S3 | Adobe aktiviert einen neuen MVPD für einen Programmierer, selbst wenn der MVPD nicht bereit ist, in die Produktion zu gehen | Adobe aktiviert einen neuen MVPD für einen Programmierer, aber der MVPD hat die Unterstützung für die Integration noch nicht bereitgestellt, sodass die Authentifizierungs-/Autorisierungsflüsse nicht ausgeführt werden können | - Adobe führt die Bereitstellung nur durch, wenn der Programmierer </br> darum ersucht. </br> - Der Programmierer ist dafür verantwortlich, die Genehmigung des MVPD sicherzustellen, sobald alle Tests durchgeführt wurden. |

## Reaktionserwartungen bei Schweregraden 1 Unfälle {#response-expectations-for-severity-one-incidents}

* Erstes Ansprechen: 30 Minuten (24/7)
* Aktionsplan: 1 Stunde (24/7)
* Lösung: ASAP (24/7)
