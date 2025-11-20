---
title: Glossar
description: Glossar der Begriffe bei der Überwachung gleichzeitiger Nutzung
exl-id: 3b3b36fe-9f04-4de9-bd84-9f8d766bbc71
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Glossar {#glossary}

## Konto-ID {#accid-defn}

* MVPD-Konto eines Abonnenten, das normalerweise dem tatsächlichen Abrechnungskonto entspricht. Dieses Konto muss von der MVPD in ihrem eigenen System identifiziert werden können.

## Aktion {#action-defn}

* Die Art des Zugriffs, den der Betreff anfordert. Die möglichen Werte für CM sind ***Initiieren*** oder ***Fortsetzen*** einer Streaming-Sitzung.

## Aktiver Stream {#active-stream-defn}

* Ein Stream, der in den letzten 90 Sekunden mindestens 1 Ereignis (Heartbeat) erhalten hat.

* ***Hinweis:*** Wenn das letzte Ereignis im Stream vom Typ stop (`?event=stop`) ist, wird es nicht gezählt. Dies ist eine Optimierung, die es einem Player ermöglicht, einen Stream explizit zu schließen, sodass er nicht mehr als „aktiv“ betrachtet wird.

## Anwendung {#application-defn}

* Entwickelt vom Mandanten für den Zugriff auf Videoinhalte
* Macht Entscheidungen über den Zugriff auf Inhalte basierend auf den vom Parallelitätsüberwachungs-Service bereitgestellten Informationen und erzwingt sie (dies gilt für den Fall [Policy Information Point](/help/concurrency-monitoring/technical/policy-info-pt-versionone.md))
* Sie verfügen über eine eindeutige **Anwendungs-ID**, die von Adobe bereitgestellt wird.

## Parallelitätsüberwachungs-Service {#cm-service-defn}

* Fungiert als Überwachungssystem für die Abonnenten und unterstützt die MVPDs und Programmierer bei der Durchsetzung programmübergreifender Richtlinien.
* Empfängt Heartbeats, die auf eine Stream-Aktivität hinweisen.
* Dient als _Entscheidungspunkt für Richtlinien_ indem Autorisierungsanfragen basierend auf Benutzeraktivitäten bewertet und eine Antwort zum Zulassen/Ablehnen bereitgestellt wird.
* Fungiert als _Policy Information Point_ indem die Anzahl der aktiven Streams (und zusätzlichen Stream-Metadaten) für einen Abonnenten gemeldet wird.

## Umgebung {#env-defn}

* Zusätzliche für die Anfrage relevante Informationen wie Konfigurationseinstellungen oder Systemzeit.

## MVPD {#mvpd-defn}

* Mehrkanal-Video-Programmierverteiler.
* Fungiert als Authentifizierungs- und Autorisierungsanbieter, kann aber auch Service- oder Content-Anbieter sein.

## Richtlinie {#policy-defn}

* Das Konzept der zentralen Zugriffssteuerung in CM, definiert als Ziel und eine oder mehrere Regeln unter einem eindeutigen Namen.

## Policy Administration Point (PAP) {#policy-admin-pt-defn}

* Punkt, der die Zugriffsautorisierungsrichtlinien verwaltet. Dies wird hier nicht dokumentiert, aber letztendlich stellen wir eine Self-Service-Konsole für Kunden bereit, um ihre Zugriffsrichtlinien zu verwalten.

## Policy Decision Point (PDP) {#policy-decn-pt-defn}

* Stelle, die Zugriffsanfragen anhand von Autorisierungsrichtlinien bewertet, bevor Zugriffsentscheidungen erlassen werden.

## Policy Enforcement Point (PEP) {#policy-ef-pt-defn}

* Punkt, der die Zugriffsanfrage eines Benutzers auf eine Ressource abfängt, eine Entscheidungsanfrage an die PDP sendet und diese Entscheidung bei der Anfrage durchsetzt. Dies ist derzeit die Client-Anwendung, und es gibt keinen Plan für die Übertragung dieser Rolle zur Parallelitätsüberwachung.

## Policy Information Point (PIP) {#policy-info-pt-defn}

* Eine Quelle von Attributwerten. Die Überwachung von gleichzeitigen Aufrufen dient als Informationspunkt und bietet folgende Informationen:
   * Pass-Through-Stream-Metadaten.
   * Aktivitätsmetriken bezüglich der gleichzeitigen Streams.

## Programmierer {#programmer-defn}

* Fungiert als Service- und Content-Anbieter.
* Nutzt die bereitgestellte Client-Anwendung, die mit dem Parallelitätsüberwachungs-Service integriert ist, um die definierten Sicherheitsrichtlinien auf der Grundlage der oben genannten Service-Daten durchzusetzen.
* Muss die MVPD bei der Erfassung der Abonnentenaktivität und der Durchsetzung der Beschränkungsregeln in ihren Eigenschaften unterstützen.
* Ist möglicherweise auch daran interessiert, den gleichzeitigen Zugriff auf ihre Inhalte als separate Regel auf alle Zielportale zu beschränken.

  *F: Warum sollte ich Programmierer sein und nicht Anforderer-IDs wie im Rest der Adobe Pass-Authentifizierung?*

  *A: Der Grund dafür ist, es Programmierern zu ermöglichen, diesen Parameter flexibel zu verwenden, um Daten zwischen ihren Eigenschaften je nach Anwendungsfällen weiterzugeben oder zu isolieren.*

## Ressource {#resource-defn}

* Der tatsächliche Inhalt, den ein Subjekt konsumieren möchte. Die Ressource enthält in der Regel Attribute, die sich auf den Eigentümer (Herausgeber) beziehen, und kann auch zusätzliche Informationen wie Genre oder was auch immer bereitstellen.

## Regel {#rule-defn}

* Eine boolesche Funktion, die anhand eines bestimmten Streams und der relevanten Benutzeraktivität ausgewertet wird, um zu bestimmen, ob der Zugriff für diesen Stream zugelassen oder verweigert werden soll.

## Streaming-Sitzung {#streaming-session-defn}

* Eine von einem Subjekt initiierte Streaming-Videositzung zum Nutzen einer bestimmten Ressource.

## Subjekt {#subj-defn}

* Der Verbraucher der (Video-)Inhalte über das Internet. Wir vermeiden bewusst den Begriff _**Benutzer**_, da die gleichzeitige Überwachung normalerweise MVPD-Konto-IDs betrifft (bei denen mehrere tatsächliche Benutzende denselben Vertrag teilen, z. B. Familienmitglieder eines Haushalts).

* Für jeden Stream kann das Subjekt mit Attributen erweitert werden, die sich auf die tatsächliche Person beziehen, die den Service verwendet, auf ihr netzwerkgebundenes Gerät und so weiter.

## Abonnentin oder Abonnent {#subscriber-defn}

* Der zahlende Kunde einer MVPD oder eine Person, die die Anmeldeinformationen eines zahlenden Kunden teilt
* Kann durch den Parallelitätsüberwachungs-Service von der Client-Anwendung mithilfe des oben genannten Service daran gehindert werden, Inhalte zu sehen.
* Im besten Fall bemerkt er nie die Existenz des Parallelitätsüberwachungs-Service

## Zielgruppe {#target-defn}

* Ein Stream-Prädikat, das zurückgibt, ob die Regel für einen bestimmten Stream anwendbar ist. Das implizite Ziel in CM ist jeder Stream, der von einer Anwendung erstellt wird, die auf die betreffende Richtlinie verweist. Darüber hinaus können Bedingungen für Attributwerte hinzugefügt werden, um die Aktivitätsfilterung vor der Anwendung der Regeln zu optimieren.

## Mandant {#tenant-defn}

* Eine Organisation zur Überwachung gleichzeitiger Kunden.
