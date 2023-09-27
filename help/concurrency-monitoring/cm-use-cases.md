---
title: Anwendungsbeispiele
description: Anwendungsfälle bei der Überwachung der Parallelität.
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---


# Nutzungsszenarios {#use-cases}

Der Hauptanwendungsfall des Stream-Zählungs-Service besteht darin, die Anzahl der gleichzeitigen Videostreams zu zählen, die von einem Benutzer angesehen werden, und eine Entscheidung über die gleichzeitige Verwendung dieser Video-Streams für dieselbe Konto-ID zu treffen.

Um die Nutzung durch Abonnenten zu überwachen, ist ein zentralisierter Dienst erforderlich, der Benutzeraktivitäten aggregieren kann, unabhängig davon, ob sie auf der Website oder in der Anwendung des Programmierers, im Inhaltsportal des MVPD oder in einer syndizierten Eigenschaft stattfinden.

Die wichtigsten Anwendungsfälle, die von diesem zentralen Dienst unterstützt werden, sind:

1. Sobald ein Abonnent mit der Wiedergabe eines Videos beginnt, kann die Anwendung **Initialisieren einer Streaming-Sitzung** und starten **Berichtsaktivität** Daten.
1. Im selben zentralen Dienst erhält eine andere Instanz ***CM-Entscheidungen*** - Wenn die Anwendung eine oder mehrere Richtlinien im CM-Dienst registriert hat, antwortet der Dienst mit der Zugriffsentscheidung auf der Grundlage der aktuellen Aktivität.


## Sitzungserstellung {#create-session}

Mit diesem API-Aufruf kann der Client eine neue CM-Sitzung erstellen, wenn der Benutzer zum Ansehen von Inhalten die Schaltfläche &quot;Abspielen&quot;drückt. Die Server-Antwort enthält die neue Stream-URL (die die Stream-ID enthält), um sie aktiv zu halten, und den Zeitpunkt, zu dem der Stream mit einer Zeitüberschreitung beendet wird. Es wird erwartet, dass die Client-Anwendung die Aktivität über Heartbeats meldet. Der Aufruf zur Sitzungsinitialisierung muss Metadaten in Form von Schlüssel/Wert-Paaren enthalten, die als Formulardaten (oder Abfragezeichenfolgenparameter) gesendet werden. Darüber hinaus enthält die Antwort auch eine Markierung, die angibt, ob die Wiedergabe &quot;richtlinienkonform&quot;ist. Ist dies nicht der Fall, ist die Wiedergabe nicht zulässig.

## Berichtsaktivität {#reporting-activity}

Nachdem eine Sitzung erstellt wurde, muss die Anwendung regelmäßig Heartbeats senden, damit dieser Stream aktiv bleibt. Außerdem wird empfohlen, dass die Client-App den Stream stoppt, sobald der Benutzer die Wiedergabe stoppt, damit der Stream erst nach einer Zeitüberschreitung als aktiv gezählt wird.

Die Antwort des Heartbeat-Aufrufs kann der Clientanwendung erlauben, die Videowiedergabe fortzusetzen (wenn sie richtlinienkonform ist) oder sie kann sie anweisen, die Videowiedergabe zu stoppen. Wenn der Video-Stream nicht kompatibel ist, muss die Client-Anwendung ihn stoppen. Die Antwort enthält Informationen, damit die Client-Anwendung eine Fehlermeldung und/oder verfügbare Aktionen anzeigt, damit der Benutzer die Wiedergabe fortsetzen kann.
