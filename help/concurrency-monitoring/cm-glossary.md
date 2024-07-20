---
title: Glossar
description: Glossar der Begriffe in der Überwachung der Parallelität
exl-id: 3b3b36fe-9f04-4de9-bd84-9f8d766bbc71
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Glossar {#glossary}

## Konto-ID {#accid-defn}

* MVPD-Konto eines Abonnenten, das normalerweise dem tatsächlichen Abrechnungskonto entspricht. Dieses Konto muss vom MVPD in seinem eigenen System identifiziert werden können.

## Aktion {#action-defn}

* Der Zugriffstyp, den der Betreff anfordert; die möglichen Werte für CM sind ***Initiator*** oder ***continue*** für eine Streaming-Sitzung.

## Aktiver Stream {#active-stream-defn}

* Ein Stream, der in den letzten 90 Sekunden mindestens 1 Ereignis (Heartbeat) erhalten hat.

* ***Hinweis:*** Wenn das letzte Ereignis im Stream vom Typ &quot;stop&quot;(`?event=stop`) ist, wird es nicht gezählt. Dies ist eine Optimierung, die es einem Player ermöglicht, einen Stream explizit zu schließen, sodass er nicht mehr als &quot;aktiv&quot;betrachtet wird.

## Anwendung {#application-defn}

* Vom Mandanten für den Videoinhaltszugriff entwickelt
* Erzwingt Entscheidungen über den Inhaltszugriff anhand von Informationen, die vom Concurrency Monitoring Service bereitgestellt werden (gilt für den Fall [Policy Information Point](/help/concurrency-monitoring/policy-info-pt-versionone.md) ).
* wird über eine eindeutige **Anwendungs-ID** verfügen, die von Adobe bereitgestellt wird.

## Dienst zur Überwachung von Parallelen {#cm-service-defn}

* fungiert als Überwachungssystem für die Abonnenten und unterstützt die MVPDs und Programmierer bei ihren anwendungsübergreifenden Richtliniendurchsetzungsanforderungen.
* Ruft Heartbeats auf, die auf Stream-Aktivitäten hinweisen.
* fungiert als _Policy Decisioning Point_, indem Autorisierungsanfragen anhand der Benutzeraktivität bewertet und eine Antwort auf &quot;allow/deny&quot;bereitgestellt werden.
* fungiert als _Richtlinieninformationspunkt_, indem die Anzahl der aktiven Streams (und zusätzlichen Stream-Metadaten) für einen Abonnenten gemeldet wird.

## Umgebung {#env-defn}

* Zusätzliche Informationen, die für die Anfrage relevant sind, z. B. Konfigurationseinstellungen oder Systemzeit.

## MVPD {#mvpd-defn}

* Multichannel Video Programming Distributor.
* fungiert als Authentifizierungs- und Autorisierungsanbieter, kann aber auch Service oder Content Provider sein.

## Politik {#policy-defn}

* Das zentrale Zugriffskontrollkonzept in CM definiert als Zielgruppe und mindestens eine Regel, die unter einem eindeutigen Namen gruppiert ist.

## Policy Administration Point (PAP) {#policy-admin-pt-defn}

* Point, der Zugriffsberechtigungsrichtlinien verwaltet. Dies wird hier nicht dokumentiert, aber schließlich werden wir eine Self-Service-Konsole bereitstellen, über die Kunden ihre Zugriffsrichtlinien verwalten können.

## Policy Decision Point (PDP) {#policy-decn-pt-defn}

* Punkt, der Zugriffsanfragen anhand von Autorisierungsrichtlinien bewertet, bevor Zugriffsentscheidungen getroffen werden.

## Policy Enforcement Point (PEP) {#policy-ef-pt-defn}

* Punkt, der die Zugriffsanfrage des Benutzers auf eine Ressource abfängt, eine Entscheidungsanfrage an die PDP sendet und diese Entscheidung über die Anfrage erzwingt. Dies ist derzeit die Client-Anwendung und es gibt keinen Plan, diese Rolle zur Überwachung der Parallelität zu übertragen.

## Policy Information Point (PIP) {#policy-info-pt-defn}

* Eine Quelle von Attributwerten. Die Überwachung der Parallelität fungiert als Informationspunkt, indem sie Folgendes bereitstellt:
   * Übergabe-Stream-Metadaten.
   * Aktivitätsmetriken zu den gleichzeitigen Streams.

## Programmierer {#programmer-defn}

* fungiert als Dienst- und Inhaltsanbieter.
* Verwendet die bereitgestellte Client-Anwendung, die in den Concurrency Monitoring-Dienst integriert ist, um die definierten Sicherheitsrichtlinien basierend auf den oben genannten Dienstdaten durchzusetzen.
* Muss den MVPD bei der Erfassung von Abonnentenaktivitäten und der Durchsetzung der Begrenzungsregeln bei ihren Eigenschaften unterstützen.
* Sie können auch daran interessiert sein, den gleichzeitigen Zugriff auf ihre Inhalte über alle Zielportale hinweg zu beschränken, als separate Regel.

  *Q: Warum die Programmierer- und nicht die Anforderer-ID wie bei der übrigen Adobe Pass-Authentifizierung?*

  *A: Der Grund besteht darin, dass Programmierer diesen Parameter flexibel verwenden können, um Daten zwischen ihren Eigenschaften je nach Anwendungsfall weiterzugeben oder zu isolieren.*

## Ressource {#resource-defn}

* Der tatsächliche Inhalt, den ein Betreff verwenden möchte. Die Ressource enthält in der Regel Attribute, die mit dem Eigentümer (Herausgeber) in Verbindung stehen, und kann auch zusätzliche Informationen wie Genre oder was auch immer bereitstellen.

## Regel {#rule-defn}

* Eine boolesche Funktion, die anhand eines bestimmten Streams und der relevanten Benutzeraktivität bewertet wird, um zu bestimmen, ob der Zugriff für diesen Stream zulässig oder verweigert werden soll.

## Streaming-Sitzung {#streaming-session-defn}

* Eine Streaming-Videositzung, die von einem Betreff für die Nutzung einer bestimmten Ressource initiiert wird.

## Betreff {#subj-defn}

* Der Nutzer des (Video-)Inhalts über das Internet. Wir vermeiden bewusst den Begriff _**Benutzer**_, da sich die Überwachung der Parallelität normalerweise mit MVPD-Konto-IDs befasst (an denen mehrere tatsächliche Benutzer beteiligt sind, die denselben Vertrag teilen, z. B. Familienmitglieder für einen Haushalt).

* Für jeden Stream kann der Betreff durch Attribute erweitert werden, die sich auf die tatsächliche Person beziehen, die den Dienst verwendet, ihr netzwerkgebundenes Gerät usw.

## Abonnent {#subscriber-defn}

* Der zahlende Kunde eines MVPD oder eine Person, die die Anmeldeinformationen eines zahlenden Kunden teilt
* Kann durch den Dienst zur Überwachung der Parallelität von Inhalten durch die Clientanwendung, die den oben genannten Dienst verwendet, verhindert werden.
* Im besten Fall erkennt er niemals die Existenz des Concurrency Monitoring Service

## Target {#target-defn}

* Ein Stream-Prädikat, das zurückgibt, ob die Regel auf einen bestimmten Stream anwendbar ist. Das implizite Ziel in CM ist jeder Stream, der von einer Anwendung erstellt wird, die auf die betreffende Richtlinie verweist. Darüber hinaus können Attributwertbedingungen hinzugefügt werden, um die Aktivitätsfilterung vor Anwendung der Regeln zu optimieren.

## Mandant {#tenant-defn}

* Eine Organisation zur Überwachung der Parallelität des Kunden.
