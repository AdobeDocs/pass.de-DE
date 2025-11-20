---
title: Informationspunkt für Richtlinien
description: Informationspunkt für Richtlinien
exl-id: 964bb28d-cfef-4a37-b6c4-10cc59be0b47
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Informationspunkt für Richtlinien {#pip}

>[!NOTE]
>
>Diese Seite ist veraltet, da sie für die vorherige Version der API gilt, die für neue Integrationen nicht mehr empfohlen wird

Das folgende Diagramm zeigt den Ablauf für den Fall, dass der Kunde sich für den **Policy Information Point** entscheidet. In diesem Fall wird CM nur für die Abfrage der Aktivität verwendet und die gesamte Zugriffslogik ist in die Client-Anwendung eingebettet):

![](../assets/pip-workflow.png)



Das folgende Diagramm zeigt, wie die Stream-Zählung für einen Benutzer funktioniert, der Inhalte von zwei Geräten aus überwacht.

![](../assets/pip-sequence.png)

Kurz gesagt, der übliche Nachrichtenfluss sieht wie folgt aus:

1. Zunächst wird für einen Benutzer, der den Service nie verwendet hat, eine Web-Seite geladen/eine Anwendung wird geöffnet, bei der eine instrumentierte Anwendung des Services für die gleichzeitige Überwachung einen Sitzungsinitialisierungsaufruf ausführt.
1. Der Dienst zur Überwachung gleichzeitiger Zugriffe gibt die neue Stream-Ressource für Heartbeats sowie die aktuelle Benutzeraktivität zurück.
1. Während der Videowiedergabe führt die instrumentierte Anwendung Heartbeat-Aufrufe an den Parallelitätsüberwachungs-Service durch und zeigt an, dass der Benutzer derzeit ein Video verwendet.
1. Zu jedem anderen Zeitpunkt können andere instrumentierte Anwendungen Statusabfrageaufrufe an den Parallelitätsüberwachungs-Service durchführen, der die aktuelle Benutzeraktivität zurückgibt.
1. Am Ende der Videowiedergabe kann die instrumentierte Anwendung einen Heartbeat-Aufruf mit „event=stop“ ausführen, was bedeutet, dass das Video angehalten wurde und der aktuelle Stream nicht mehr als aktiver Stream gezählt werden sollte.
