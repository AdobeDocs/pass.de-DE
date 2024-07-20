---
title: Entscheidungspunkt für politische Maßnahmen
description: Entscheidungspunkt für politische Maßnahmen
exl-id: 94bc638c-bef8-45ea-b20a-9b7038adecdd
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# Entscheidungspunkt für politische Maßnahmen {#policy-desc-pt}

## Domänenmodell {#domain-model}

Diese Seite dient als Referenz für verschiedene Anwendungsfälle und Implementierungen von Richtlinien. Wir empfehlen Ihnen, auch den Abschnitt [Glossar](/help/concurrency-monitoring/cm-glossary.md) der Dokumentation zu Begriffsdefinitionen zu lesen.

Ein **Mandant** besitzt **Anwendungen**, für die er **Richtlinien** erzwingen möchte. **Client-Anwendungen** müssen mit der (von Adobe bereitgestellten) **Anwendungs-ID** konfiguriert werden.

Der Mandant verknüpft dann jede Anwendung mit einer oder mehreren Richtlinien, die er erstellt oder von anderen erstellt und freigegeben hat. Richtlinien können zwischen mehreren Mandanten verknüpft werden.

Die **Betreffaktivität** besteht aus allen Streams (unabhängig von der Anwendung), die für einen bestimmten Betreff der Überwachung der Parallelität gemeldet werden.

Wenn ein Stream für einen bestimmten Betreff autorisiert werden soll, prüft das System zunächst alle Richtlinien, die für die Anwendung definiert sind, die den Stream erstellt hat.

Für jede anwendbare Richtlinie müssen wir dann alle **relevanten Aktivitäten** erfassen, die an die Regel übergeben werden. Die **relevante Aktivität** für eine Richtlinie P enthält nur einen Stream S, wenn die folgende Bedingung erfüllt ist:

**Stream &quot;S&quot;wird von einer Anwendung gestartet, die unter ihren Richtlinien die Richtlinie &quot;P&quot;enthält.**

![Stream &quot;S&quot;wird von einer Anwendung gestartet, die unter ihren Richtlinien die Richtlinie &quot;P&quot;enthält.](assets/pdp-domain-model.png)

## Anwendungsfälle für Trockenausführung {#dry-run-use-cases}

Die folgende exemplarische Vorgehensweise dient der Validierung des Modells anhand einiger Anwendungsfälle. Wir werden dies schrittweise tun, indem wir mit einem grundlegenden Setup beginnen und Komplexität auf verschiedene Arten hinzufügen.

### 1. Ein Mieter. Eine Anwendung. Eine Richtlinie. Ein Stream {#onetenant-oneapp-onepolicy-onestream}

Wir beginnen mit einem einzelnen Mandanten, mit einer einzigen Anwendung und einer einzigen Richtlinie, die verknüpft ist. Nehmen wir einmal an, dass die Richtlinie besagt, dass es für jeden Benutzer maximal einen aktiven Stream geben kann (der neueste Stream darf wiedergegeben werden).

Sobald ein Stream gestartet wurde, besteht die Aktivität nur aus diesem Stream und kann wiedergegeben werden.

![Ein Mandant. Eine Anwendung. Eine Richtlinie. Ein Stream](assets/onetenant-app-policy-stream.png)


### 2. Ein Mieter. Eine Anwendung. Eine Richtlinie. Zwei Streams. {#onetenant-oneapp-onepolicy-twostreams}

Sobald ein zweiter Stream gestartet wird (durch denselben Betreff mit derselben Anwendung), besteht die für die Validierung verwendete Aktivität aus sowohl **s1** als auch **s2**.

Der Grenzwert wird überschritten, da die Richtlinie besagt, dass nur ein Stream wiedergegeben werden darf. Daher werden wir nur die Wiedergabe des neuesten Streams (**s2**) zulassen.

![Ein Mandant. Eine Anwendung. Eine Richtlinie. Zwei Streams.](assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>Die Diagramme stellen die Systemansicht der Benutzeraktivität dar. Bei Stream-Initialisierungsversuchen wird die Zugriffsentscheidung in die Antwort aufgenommen. Bei aktiven Streams wird die Entscheidung in der Heartbeat-Antwort zurückgegeben.

### 3. Zwei Mieter. Zwei Anwendungen. Eine Richtlinie. Zwei Streams. {#twotenant-twoapp-onepolicy-twostreams}

Nehmen wir nun an, dass ein neuer Mandant dieselbe Richtlinie in seinen Anwendungen durchsetzen möchte:

![Zwei Mandanten. Zwei Anwendungen. Eine Richtlinie. Zwei Streams.](assets/onepolicy-twotenant-app-stream.png)

Da die beiden Mandanten durch dieselbe Richtlinie verknüpft sind, gilt hier die in Nutzungsszenario 2 beschriebene Situation und **s3** darf wiedergegeben werden, da es sich um den neuesten Stream handelt.

### 4. Zwei Mieter. Drei Anträge. Zwei Strategien. Zwei Streams. {#twotenants-threeapps-twopolicies-twostreams}

Nehmen wir nun an, der zweite Mandant stellt eine neue Anwendung bereit und möchte eine neue Richtlinie definieren, die zwischen **app2** und **app3** freigegeben wird.

![Zwei Mandanten. Drei Anträge. Zwei Strategien. Zwei Streams.](assets/twotenant-policies-streams-threeapps.png)

Derzeit sind die aktiven Streams **s3** und **s4** beide zulässig. Wenn für **s3** die Richtlinie **P1** ausgewertet wird, zählt das System nur **s3**, da **relevante Aktivität** (**s4** in keiner Weise mit der Richtlinie **P1** in Zusammenhang steht, sodass kein Verstoß vorliegt.

Die Richtlinie **P2** wird auf beide Streams angewendet und enthält sowohl **s3** als auch **s4** als relevante Aktivität. Da diese Aktivität innerhalb der Grenzen von zwei Streams liegt, sind beide Streams zulässig.

### 5. Zwei Mieter. Drei Anträge. Zwei Strategien. Drei Ströme. {#twotenants-threeapps-twopolicies-threestreams}

Jetzt vorausgesetzt, dass ein neuer Stream-Initialisierungsversuch mit **app2** durchgeführt wird:

![Zwei Mandanten. Drei Anträge. Zwei Strategien. Drei Streams.](assets/twotenants-policies-threeapps-streams.png)

**s5** darf mit **P1** beginnen (wodurch neuere Streams übernommen werden können), wird jedoch von **P2** verweigert, sodass es nicht gestartet wird.

Dasselbe geschieht, wenn ein Stream mit app3 versucht wird: Dieselbe Richtlinie, die P2 durchführt, verweigert den Zugriff für diesen Stream.

![](assets/stream-init-attempted-app3.png)

Sehen wir uns nun an, was passieren würde, wenn der Benutzer versucht, einen neuen Stream mit App1 zu erstellen:

![](assets/new-stream-with-app1.png)

Die Anwendungs-App1 steht in keiner Weise in Zusammenhang mit der Richtlinie **P2**, daher wird nur die Richtlinie **P1** angewendet: , die es dem neuen Stream ermöglicht, zu starten und den älteren Stream zu verweigern (**s3** in diesem Fall).
