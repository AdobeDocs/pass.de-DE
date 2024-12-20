---
title: Isolationsmodus-MVPDs
description: Erfahren Sie mehr über Isolationsmodus-MVPDs für TV Everywhere-Programmierer
exl-id: 7ffe4ce3-e9cb-4382-a421-bb57d1927b53
source-git-commit: 2bb570ab14a3295d46ee6dc0d38485697d63809c
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---

# Isolationsmodus MVPDs für TV Everywhere-Programmierer {#isolation-mode-tve}

>[!IMPORTANT]
>
> Die Beschränkung für MVPDs im Isolationsmodus gilt nur für TV Everywhere-Programmierer.

Im Isolationsmodus identifizieren MVPDs (wie Xfinity) Abonnenten konsistent geräteübergreifend basierend auf ihrer Interaktion mit bestimmten Programmierern. Im Standardmodus identifizieren MVPDs Abonnentinnen und Abonnenten konsistent geräteübergreifend, unabhängig von den beteiligten Programmierern.

Beispiel:

![](assets/isolation-diff-new.png)

*Isolationsmodus-MVPDs identifizieren vier verschiedene Abonnentinnen und Abonnenten statt zwei*

* Wenn ein Abonnent B eines Isolationsmodus-MVPD (z. B. Xfinity) auf die Inhalte zugreift, die von zwei verschiedenen Programmierern mit demselben Gerät angeboten werden, verknüpft die MVPD verschiedene Kennungen mit den beiden verschiedenen Zugriffsversuchen. Es scheint, dass zwei verschiedene Abonnentinnen und Abonnenten auf den Inhalt für die Programmierer zugreifen (L und M in der Abbildung).

* Wenn Abonnent B bei Standard-MVPDs auf Inhalte zugreift, die von zwei verschiedenen Programmierern angeboten werden, verknüpft die MVPD für beide Zugriffsversuche eine einzige Zugriffskennung.

* MVPDs (z. B. Xfinity) im Isolationsmodus identifizieren einen Abonnenten nicht konsistent, selbst wenn der Abonnent dasselbe Gerät über verschiedene Programmierer hinweg verwendet.

Um Datenverzerrungen zu vermeiden, die durch die Zählung eines einzelnen Teilnehmers als mehrere Teilnehmer aufgrund des Zugriffs auf verschiedene Programmierer verursacht werden, beschränkt der Isolationsmodus die gemeldete Aktivität über einen Programmierer auf ihre Anwendungen.

Beispielsweise kann der Programmierer L nur Daten basierend auf der Aktivität der Identitäten W und Y anzeigen, wobei die Identitäten X und Z im vorherigen Bild ignoriert werden.

>[!IMPORTANT]
>
> Der Nachteil besteht darin, dass Programmierer L aufgrund von Aktivitäten mit anderen Programmierern als L keine Informationen über die Abonnenten A und B weitergeben kann.

Im Isolationsmodus werden die Freigabewerte und zugehörigen Metriken ausschließlich anhand der Aktivität der Geräte berechnet, die von den Programmen des ausgewählten Programmierers und Kanals streamen. Die Sharing-Scores und -Wahrscheinlichkeiten werden anhand von Stream-Starts für die aktuell ausgewählten Kanäle berechnet.

Das System arbeitet automatisch im Isolationsmodus, wenn das ausgewählte Segment einen Isolationsmodus-MVPD enthält, der einzelne Abonnenten beim Streaming von verschiedenen Programmierern als mehrere Abonnenten identifiziert. Alle Diagramme und Diagramme für diese Segmente spiegeln die Ergebnisse dieses geänderten Verhaltens wider.

>[!IMPORTANT]
>
> Das Verhalten im Isolationsmodus ist nicht kompatibel mit dem Standardmodus, Isolationsmodus MVPD kann nicht mit anderen MVPDs gemischt werden und umgekehrt.

Um ein Segment zu erstellen, das im Isolationsmodus analysiert wird, ziehen Sie Isolationsmodus-MVPD, z. B **Xfinity**, in den Abschnitt „MVPDs“ der Segmentdefinition.

>[!NOTE]
>
> Da MVPDs im Isolationsmodus nicht mit anderen MVPDs gemischt werden können, lässt der Abschnitt MVPD der Segmentdefinition nicht zu, dass eine andere MVPD dorthin gezogen wird.

![](assets/xfinity-in-segment.png)

*Xfinity-Auswahl im Isolationsmodus*

>[!IMPORTANT]
>
> Die Kontofreigabe ist relevanter, wenn sie für das Streaming über alle Programme von Programmierern hinweg gemessen wird. Im Isolationsmodus werden **niedrigere Werte** Freigabe“ und einige Variationen in den Metriken erwartet.

![](assets/aggregate-sharing-isolation.png)

*Freigabe von Wahrscheinlichkeitsabständen im Isolationsmodus*

Die obigen Messwerte zeigen, dass nur 9 % aller Konten gemeinsam genutzt werden, und von diesen werden nur 11 % der Inhalte konsumiert. Aufgrund der naturgemäß niedrigeren Werte sollten die Ergebnisse im Isolationsmodus anders interpretiert werden als die Ergebnisse im Standardmodus.
