---
title: Anwendungsszenarien
description: Anwendungsfälle bei der Überwachung von gleichzeitigen Aufrufen.
exl-id: 6cc30bb6-e985-4d9a-9f99-a7f04ae8deb7
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Anwendungsfälle {#use-cases}

Der Hauptanwendungsfall des Stream Counting Service besteht darin, die Anzahl der gleichzeitigen Videostreams zu zählen, die von einer Benutzerin oder einem Benutzer angesehen werden, und eine Entscheidung über die gleichzeitige Verwendung für dieselbe Konto-ID zu treffen.

Um die Nutzung durch Abonnenten zu überwachen, ist ein zentralisierter Service erforderlich, der Benutzeraktivitäten unabhängig davon aggregieren kann, ob sie auf der Website oder in der Anwendung des Programmierers, im Inhaltsportal von MVPD oder in einer Syndication-Eigenschaft stattfinden.

Die wichtigsten Anwendungsfälle, die von diesem zentralisierten Service unterstützt werden, sollten sein:

1. Sobald ein Abonnent beginnt, ein Video anzusehen, kann die Anwendung **Streaming-Sitzung initialisieren** und Daten **Berichtsaktivität** starten.
1. Im selben zentralen Service erhält eine andere Instanz ***CM-Entscheidungen***. Wenn die Anwendung eine oder mehrere im CM-Service registrierte Richtlinien hat, antwortet der Service mit einer Zugriffsentscheidung, die auf der aktuellen Aktivität basiert.

## Häufige Anwendungsfälle {#common-use-cases}

### Einfache Strombegrenzung

Begrenzen Sie die Anzahl der gleichzeitigen Streams pro Abonnent in all Ihren Programmen.

### Gerätebasierte Einschränkungen

Lassen Sie nur eine bestimmte Anzahl von Streams pro Gerätetyp (Smartphone, Tablet, TV usw.) zu.

### Inhaltsspezifische Regeln

Wenden Sie unterschiedliche Beschränkungen für Live-Inhalte und VOD-Inhalte an.

### Standortbasierte Richtlinien

Streaming auf Grundlage des geografischen Standorts oder des Netzwerktyps einschränken.

## Erstellen einer Sitzung {#create-session}

Dieser API-Aufruf ermöglicht es dem Client, eine neue CM-Sitzung zu erstellen, wenn der Benutzer die Schaltfläche „Abspielen“ drückt, um sich Inhalte anzusehen. Die Server-Antwort enthält die neue Stream-URL (mit der Stream-ID), um sie am Leben zu erhalten, und den Zeitpunkt, zu dem die Zeitüberschreitung des Streams eintritt. Es wird erwartet, dass die Client-Anwendung die Aktivität über Heartbeats meldet. Der Sitzungsinitialisierungsaufruf muss Metadaten in Form von Schlüssel/Wert-Paaren enthalten, die als Formulardaten (oder Abfragezeichenfolgenparameter) gesendet werden. Darüber hinaus enthält die Antwort auch eine Markierung, die anzeigt, ob die Wiedergabe richtlinienkonform ist. Wenn dies nicht der Fall ist, ist die Wiedergabe nicht zulässig.

## Berichtsaktivität {#reporting-activity}

Nachdem eine Sitzung erstellt wurde, muss die Anwendung regelmäßig Heartbeats senden, damit dieser Stream aktiv bleibt. Außerdem wird empfohlen, dass die Client-App den Stream stoppt, sobald der Benutzer die Wiedergabe stoppt, sodass der Stream bis zur Zeitüberschreitung nicht als aktiv zählt.

Die Antwort auf den Heartbeat-Aufruf kann der Client-Anwendung erlauben, die Videowiedergabe fortzusetzen (wenn sie richtlinienkonform ist), oder sie kann anweisen, die Videowiedergabe anzuhalten. Wenn der Video-Stream nicht kompatibel ist, muss er von der Client-Anwendung angehalten werden. Die Antwort enthält Informationen, damit die Client-Anwendung eine Fehlermeldung und/oder verfügbare Aktionen anzeigt, mit denen der Benutzer die Wiedergabe fortsetzen kann.
