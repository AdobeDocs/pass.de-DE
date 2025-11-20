---
title: Benutzerdefinierte Metadaten
description: Benutzerdefinierte Metadaten
exl-id: 0cfd1158-8c6c-47c2-b838-5490ff4bf0ce
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# Benutzerdefinierte Metadaten {#cm}

>[!NOTE]
>
> Diese Seite ist veraltet, da sie für die vorherige Version der API gilt, die für neue Integrationen nicht mehr empfohlen wird

Der Service ermöglicht es Kunden, sowohl standardmäßige als auch benutzerdefinierte Felder in Abfragen und bei der Entscheidungsfindung zu verwenden. Die Standardfelder sind immer für jeden Stream verfügbar (da sie entweder für die Stream-Erstellung erforderlich oder vom Server generiert werden):

* Anwendung
* mvpd
* accountId
* streamId
* streamStart
* Initiator


Die benutzerdefinierten Felder sind alle Schlüssel/Wert-Paare, die bei der Stream-Initialisierung als Formular- oder Abfragezeichenfolgenparameter übergeben werden. Sowohl die standardmäßigen als auch die benutzerdefinierten Felder können dann in den folgenden Szenarien verwendet werden (Einzelheiten finden Sie in der entsprechenden Dokumentation für die betroffenen API-Ressourcen weiter unten):

* Beim Abrufen der Stream-Liste für ein Konto (die Ressource /streams) werden zusätzliche Felder (über den Abfragezeichenfolgenparameter fields) angefordert.
* Aufschlüsselung der Kontoaktivität durch Angabe der zu gruppierenden Dimensionen (Ressource /activity)
* Definieren von Server-seitigen Richtlinien basierend auf Feldwerten oder Kardinalitäten (in den Beispielen wird nur aus Gründen der Klarheit Pseudo-SQL verwendet):
* Konfigurieren Sie eine Richtlinie, die nur auf bestimmte Feldwerte angewendet werden soll (z. B. eine dedizierte iOS-Richtlinie: WO osType &quot;iOS&quot; IST).
* Anzahl der eindeutigen Werte für ein bestimmtes Feld begrenzen (z. B. nicht mehr als X verschiedene Geräte: HAVING DISTINCT COUNT(deviceId) >= 2)
* Anzahl der aktiven Streams pro Feldwert begrenzen (z. B. nicht mehr als X aktive Streams für einen einzelnen Gerätetyp: GROUP BY deviceType HAVING COUNT(streamId) >= 3)


Basierend auf diesen gesendeten Schlüsseln/Werten können verschiedene Regeln festgelegt werden. Dies könnte in etwa so aussehen:

1. Der Kunde entscheidet, dass er die Parametergruppe senden möchte, die die Werte „SPORTS“ und „KIDS“ hat.
1. Dann müsste die App Folgendes tun:
   * Bei Sportkanälen würde die App bei Stream-Initialisierung ***type=SPORTS*** als Abfrageparameter senden
   * Bei Kanälen mit kinderbezogenem Inhalt würde die App bei der Stream-Initialisierung ***type=KIDS*** als Abfrageparameter senden
1. Dann könnte eine Richtlinie wie die folgende definiert werden:
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. Dies würde im Grunde bedeuten, dass ein Benutzer, der Sport ansieht, dies nicht auf mehr als einem Gerät tun kann. Wenn der Benutzer jedoch Inhalte für Kinder ansieht, ist das Anzeigen auf maximal drei Geräten erlaubt.
