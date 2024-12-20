---
title: Verstehen von Server-seitigen Metriken
description: Verstehen von Server-seitigen Metriken
exl-id: 516884e9-6b0b-451a-b84a-6514f571aa44
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2232'
ht-degree: 0%

---

# Verstehen von Server-seitigen Metriken {#understanding-server-side-metrics}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.


## Einführung {#intro}

In diesem Dokument werden die Server-seitigen Metriken der Adobe Pass-Authentifizierung beschrieben, die vom ESM-Service (Berechtigungs-Service-Überwachung) generiert wurden. Es beschreibt nicht die gleichen Ereignisse, die aus Client-seitiger Perspektive betrachtet werden (was Programmierer sehen würden, wenn sie einen Mess-Service wie Adobe Analytics auf ihrer Seite / Anwendung implementieren würden).

## Ereigniszusammenfassung {#events_summary}

Aus der Server-seitigen Sicht der Adobe Pass-Authentifizierung werden die folgenden Ereignisse generiert:

* **Im Authentifizierungsfluss generierte Ereignisse**(tatsächliche Anmeldung bei MVPD)

   * Benachrichtigung über Authentifizierungsversuch - Diese wird generiert, wenn Benutzende an die MVPD-Anmeldeseite gesendet werden.
   * Benachrichtigung über ausstehende Authentifizierung : Wenn sich der Benutzer erfolgreich mit seiner MVPD anmeldet, wird dies generiert, wenn der Benutzer        Zurück zur Adobe Pass-Authentifizierung.
   * Benachrichtigung über AuthN-Genehmigung erteilt : Diese Benachrichtigung wird generiert, wenn sich der/die Benutzende wieder auf der Programmierer-Site befindet und das Authentifizierungstoken erfolgreich von der Adobe Pass-Authentifizierung abgerufen hat.
* **Autorisierungsfluss** (Nur eine Prüfung auf Autorisierung mit einem
MVPD)\
  *Voraussetzung:* Ein gültiges Authentifizierungs-Token
   * Benachrichtigung über AuthZ-Versuch
   * Benachrichtigung über erteilte AuthZ
* **Erfolgreiche Play-Anfrage**\
  *Voraussetzung:* Authentifizierungs- und AuthZ-Token
   * Benachrichtigung über eine Prüfung mit Adobe Pass-Authentifizierung
   * Eine Play-Anfrage erfordert sowohl eine erteilte Authentifizierung als auch eine erteilte Autorisierung


Die Anzahl der eindeutigen Benutzer wird im Abschnitt [Eindeutige Benutzer](#unique-users) unten detailliert behandelt. Da die Antworten auf die Authentifizierung und Autorisierung normalerweise zwischengespeichert werden, gelten für gewöhnlich die folgenden Formeln:

* Anzahl der Authentifizierungsversuche \> Anzahl der gewährten Authentifizierung
* Anzahl der AuthZ-Versuche \> Anzahl der gewährten AuthZ
* Anzahl der AuthZ-Versuche \> Anzahl der gewährten AuthZ (normalerweise)
* Anzahl erfolgreicher Wiedergabeanforderungen \> Anzahl der gewährten Authentifizierungen


### Beispiel {#example}

Das folgende Beispiel zeigt die Server-seitigen Metriken für einen Monat
Eine Marke:

| Metrik | MVPD 1 | MVPD 2 | … | MVPD n | Gesamt |
| -------------------------- | ------ | ------ | - | ------ | ---------------------------------------------- |
| Erfolgreiche Authentifizierungen | 1125 | 2892 |   | 2203 | SUM(MVP1+…MVPD n) |
| Erfolgreiche Berechtigungen | 2527 | 5603 |   | 5904 | SUM(MVP1+…MVPD n) |
| Erfolgreiche Wiedergabeanfragen | 4201 | 10518 |   | 10737 | SUM(MVP1+…MVPD n) |
| Unique Users | 1375 | 2400 |   | 2890 | Summe aller Benutzer für alle deduplizierten MVPDs\* |
| Authentifizierungsversuche | 2147 | 3887 |   | 3108 | SUM(MVP1+…MVPD n) |
| Autorisierungsversuch | 2889 | 6139 |   | 6039 | SUM(MVP1+…MVPD n) |

</br>

Die Deduplizierung sollte in diesem Fall keine Auswirkungen haben, da verschiedene MVPD-Benutzende nicht dieselbe Benutzer-ID erhalten sollten. Wenn Sie eine Summe für zwei verschiedene Marken, aber denselben MVPD erstellen, sollte der Deduplizierungseffekt viel größer sein.

## Ereignis-Trigger {#event_triggers}

### Neuer Benutzer - Vollständiger Fluss {#new-user-full-flow}

Das folgende Diagramm beschreibt die Ereignisse und Schritte für eine Benutzerin oder einen Benutzer ohne Authentifizierungs-Token (ein neuer Benutzer oder ein Benutzer mit abgelaufenem Authentifizierungs-Token):



![](../../../assets/ae-flow-with-events.png)



Der Fluss umfasst Roundtrips zu MVPDs sowohl für die Authentifizierung (#5 zu \#7) als auch für die Autorisierung (\#11).



Nach Abschluss des Flusses werden die Authentifizierungs- und Autorisierungs-Token auf dem Gerät des Benutzers zwischengespeichert. Die TTL-Werte (Time-to-Live) für Authentifizierungs-Token liegen zwischen 6 Stunden und 90 Tagen. Eine AuthN-Token-Gültigkeit erzwingt automatisch eine AuthZ-Token-Gültigkeit. Der TTL-Wert für das Autorisierungs-Token beträgt in der Regel 24 Stunden.

| Ausgelöste Server-seitige Ereignisse | <ul><li>Authentifizierungsversuch, ausstehende Authentifizierung, Authentifizierung erteilt</li><li>Autorisierungsversuch, Autorisierung erteilt</li><li>Erfolgreiche Play-Anfrage</li></ul> |
|---|---|


### Wiederkehrender Benutzer - AuthZ- und AuthN-Token zwischengespeichert

Für Benutzer, die gültige AuthZ- und AuthN-Token zwischengespeichert haben, gilt Folgendes
Schritte passieren:


![](../../../assets/ae-flow-tokens-cached-web.png)



Dies wird beim Aufruf von `getAuthorization()` automatisch ausgelöst und umfasst nur Prüfungen mit der Adobe Pass-Authentifizierung. Der MVPD ist an diesem Fluss nicht beteiligt.


| Ausgelöste Server-seitige Ereignisse | * Erfolgreiche Play-Anfrage |
|---|---|


### Wiederkehrender Benutzer - AuthZ-Token zwischengespeichert, AuthZ-Token abgelaufen

Für Benutzer, die noch gültige Authentifizierungs-Token haben, passieren die folgenden Schritte:

![](../../../assets/ae-flow-authn-token-cached.png)


Dieser Fluss beinhaltet einen Hin- und Rückflug zur MVPD.


| Ausgelöste Server-seitige Ereignisse | <ul><li>Autorisierungsversuch, Autorisierung OK</li><li>Erfolgreiche Play-Anfrage</li> |
|---|---|

## Authentifizierungsereignisse {#authn_events}

### Authentifizierungsversuch {#authentication-attempt}

Wie im obigen Diagramm dargestellt, werden die Authentifizierungsereignisse nur ausgelöst, wenn der/die Benutzende einen Roundtrip zur MVPD durchführt. Authentifizierungsereignisse beinhalten keine Authentifizierungen von zwischengespeicherten Token.

Das Authentifizierungsversuch-Ereignis wird ausgelöst, nachdem der Benutzer in der Auswahl auf eine bestimmte MVPD geklickt hat.

* Das erste Ereignis auf der MVPD-Seite, das diesem Wert nahe kommt, ist das Laden der Seite
* Die Adobe Pass-Authentifizierung zählt nicht die wiederholten Anmeldeversuche der Benutzenden auf der MVPD-Seite (falsches Kennwort, versuchen Sie es erneut)
* Mehrere Versuche werden als ein Versuch gezählt
* Einige MVPDs führen auch eine Autorisierung im Authentifizierungsschritt durch, und der Benutzer wird nicht zurückgeleitet, wenn die Autorisierung fehlschlägt.

### Authentifizierung ausstehend {#authentication-pending}

Dieses Ereignis tritt auf, wenn der Umleitungsprozess zur Adobe Pass-Authentifizierung gestartet wurde.

### Authentifizierung erteilt {#authentication-granted}

Der Nutzer ist ein bekannter MVPD-Abonnent, typischerweise mit einem Pay-TV-Abonnement, manchmal jedoch nur mit Internetzugang. Eine erfolgreiche Authentifizierung kann entweder dadurch erfolgen, dass der/die Benutzende explizit gültige Anmeldeinformationen für seine/ihre MVPD eingegeben hat, oder dadurch, dass er/sie zuvor gültige Anmeldeinformationen eingegeben und „mich erinnern“ aktiviert hat (und die vorherige Sitzung nicht abgelaufen war).

Daher sendet die MVPD eine positive Antwort auf die Authentifizierungsanfrage an die Adobe Pass-Authentifizierung, und die Adobe Pass-Authentifizierung erstellt ein *AuthN-Token*.

* Die Authentifizierung wird in der Regel über einen langen Zeitraum zwischengespeichert (einen Monat oder länger). Aus diesem Grund sind Authentifizierungsereignisse erst dann vorhanden, wenn das Token abläuft und der Fluss erneut gestartet wird.
* Wenn Sie über Single Sign-On von einer anderen Site/App eingehen, werden keine Trigger-Authentifizierungsereignisse ausgelöst.



### Comcast-Authentifizierung {#comcast-authentication}

Comcast hat einen anderen AuthN-Fluss als die übrigen MVPDs.

Die folgenden Funktionen beschreiben die Unterschiede:

* **Verhalten von Sitzungs-**: Dadurch werden alle Authentifizierungs-Token entfernt, nachdem der Browser geschlossen wurde. Diese Funktion ist nur im Internet verfügbar. Der Hauptzweck besteht darin, sicherzustellen, dass Ihre Comcast-Sitzung nicht auf unsicheren/freigegebenen Computern persistiert wird. Dies hat zur Folge, dass es mehr Authentifizierungsversuche/gewährte Flüsse geben wird als für den Rest der MVPDs.

* **AuthN pro RequestorID**: Bei Comcast kann der AuthN-Status nicht von einer Requestor-ID in eine andere zwischengespeichert werden. Aus diesem Grund muss jede Site/App zu Comcast gehen, um ein Authentifizierungs-Token zu erhalten. Neben Überlegungen zum Benutzererlebnis hat dies, wie oben beschrieben, zur Folge, dass mehr Authentifizierungsversuche/gewährte Ereignisse generiert werden.

* **Passive Authentifizierung**: Zur Verbesserung des Benutzererlebnisses, aber
die AuthN-Funktion pro RequestorID beibehalten wird, findet ein passiver Authentifizierungsfluss in einem ausgeblendeten iFrame statt. Der/die Benutzende sieht nichts, aber die Ereignisse werden trotzdem wie zuvor ausgelöst.

Wenn der Benutzer auf der Comcast-Anmeldeseite auf „Mich erinnern“ klickt, sind nachfolgende Besuche auf dieser Seite (in einem Zeitraum von 2 Wochen) nur eine schnelle Umleitung zurück. Andernfalls müssen sich Benutzer tatsächlich auf der Seite authentifizieren.

### Erfolglose Authentifizierung {#unsuccessful-authentication}

Eine nicht erfolgreiche Authentifizierung ist kein Ereignis an sich in der Adobe Pass-Authentifizierung, kann aber als Unterschied zwischen Versuchen und Erfolgen berechnet werden.

In der Version vom Mai 2013 fügt Adobe Pass Authentication Fehler-Codes für fehlgeschlagene Authentifizierungen hinzu, die auf System- oder Netzwerkfehler zurückzuführen sind, einschließlich DRM-Fehler (Token-Bindung fehlgeschlagen) und LSO-Fehler (kein Speicherplatz zum Schreiben des Tokens usw.).

### Authentifizierungs-Konversionsrate {#authenitication-conversion-rate}

Eine interessante Metrik, die Programmierer verfolgen können, ist die Authentifizierungs-Konversionsrate, berechnet als (AuthN-Anfragen / AuthN gewährt)%.

Einige Hinweise zu den Metriken:

* Da es sich um eine ereignisbasierte Metrik handelt, spiegelt sie nicht die Konversionsrate der einzelnen Benutzer wider. Wenn ein Benutzer acht Mal versucht und dies zum neunten Mal erfolgreich ist, spiegelt dies sehr schlecht die Konversionsrate oben wider.
* Es gibt (noch) keine Möglichkeit, bei der Adobe Pass-Authentifizierung (serverseitig) eine eindeutige Authentifizierungskonvertierung zu berechnen.
* Wenn in der Site/App automatische Authentifizierungsversuche vorhanden sind, verzerrt dies auch die obige Metrik.

## Autorisierungsereignisse {#authorization_events}

### Autorisierungsversuch {#authorization_attempt}

Benutzer müssen nicht nur ein Authentifizierungs-Token erhalten, sondern auch ein Autorisierungs-Token, bevor sie Inhalte wiedergeben können. Dies geschieht normalerweise nach der Authentifizierung oder wenn das Autorisierungs-Token abläuft. Da diese Prüfung Server-seitig durchgeführt wird (von den Adobe Pass-Authentifizierungsservern bis zu den MVPD-Servern), ist der Benutzer nicht verpflichtet, etwas zu tun.

### Genehmigung erteilt {#authorization-granted}

Eine „Autorisierung erteilt“ signalisiert, dass das Abonnement des authentifizierten Benutzers die angeforderte Programmierung enthält.

Beachten Sie, dass nicht alle MVPDs einen separaten Autorisierungsschritt unterstützen. Für einige Authentifizierungen wird dies mit einer Autorisierung gleichgesetzt. Die MVPD Adobe Pass-Authentifizierung sendet eine erfolgreiche Antwort auf die Backchannel AuthZ-Anfrage und die Adobe Pass-Authentifizierung erstellt ein AuthZ-Token.

* Das AuthZ-Token wird für einen bestimmten Zeitraum zwischengespeichert, in der Regel nach 24 Stunden. Während dieses Zeitraums werden keine AuthZ-Ereignisse ausgelöst.
* Einige MVPDs arbeiten mit Berechtigungen auf Asset-Ebene, andere mit Berechtigungen auf Kanalebene. Je nachdem, welche verwendet werden, werden mehr oder weniger AuthZ-Ereignisse ausgelöst. Selbst für die Autorisierung auf Kanalebene ist ein Caching vorhanden - wenn dasselbe Asset in weniger als 24 Stunden angefordert wird, werden keine Ereignisse ausgelöst.

### Autorisierung verweigert {#authorization-denied}

Wenn eine Autorisierung verweigert wird, verfügt der authentifizierte Benutzer nicht über ein bestätigtes Abonnement für die angeforderte Programmierung. Die wahrscheinlichste Ursache ist, dass der Kanal nicht Teil des Abonnementpakets des Benutzers ist. Dies kann jedoch auch darauf zurückzuführen sein, dass ein Benutzer nur über Internetzugriff von MVPD verfügt.

Bei einigen MVPDs werden Benutzer erfolgreich authentifiziert, obwohl sie nur über ein Internetabonnement von der MVPD verfügen (kein Pay-TV-Abonnement). In diesem Fall wird die Autorisierung verweigert, obwohl sich der Kanal, für den der Benutzer die Autorisierung anfordert, im Basispaket befindet.

Einige MVPDs bieten benutzerdefinierte Fehlermeldungen für AuthZ-Ablehnungen, die Angebote für ein Upgrade ihres Pakets enthalten können.


### Konversionsrate der Autorisierung {#authorization-conversion-rate}

Die Konversionsrate der Authentifizierung kann als (AuthZ-Anfragen / AuthZ gewährt)% berechnet werden.

### Erfolgreiche Play-Anfrage {#successful-play-request}

Ein Benutzer, der sowohl authentifiziert als auch autorisiert ist, darf geschützte Inhalte anzeigen.

Bei einer erfolgreichen Wiedergabeanfrage generiert die Adobe Pass-Authentifizierung ein kurzlebiges Medien-Token, das bestätigt, dass der Benutzer berechtigt ist, das angeforderte Video anzuzeigen. Der Programmierer verwendet dieses Medien-Token für die weitere Validierung des potenziellen Betrachters. Medien-Token werden als erfolgreiche Wiedergabeanfragen verfolgt.

* Bei der Adobe Pass *Authentifizierung wird nicht*, ob die Videowiedergabe nach der Generierung des Medien-Tokens tatsächlich begonnen hat. Wenn es beispielsweise eine Geo-Einschränkung für den Inhalt gibt, zählt die Transaktion weiterhin als erfolgreiche Wiedergabeanfrage, obwohl der Stream tatsächlich nie gestartet wird.
* Da AuthN- und AuthZ-Token die MVPD-Antwort über einen bestimmten Zeitraum zwischenspeichern, ist das erfolgreiche Wiedergabeanfrageereignis das häufigste Ereignis in den Metriken.

## Unique Users {#unique-users}

### Definition {#definition}

Bei erfolgreicher Authentifizierung verfolgt die Adobe Pass-Authentifizierung die Existenz eines eindeutigen Benutzers basierend auf dem zurückgegebenen Wert der MVPD-Benutzer-ID.  Dieser Wert basiert auf den Anmeldeinformationen des Benutzers, enthält jedoch keine persönlich identifizierbaren Informationen.

Dieser Wert wird auch im sendTrackingData-Callback an die Site/App übergeben.

Dieser Wert kann geräteübergreifend persistent sein (die MVPD erzeugt für eine bestimmte Benutzerin oder einen bestimmten Benutzer denselben Wert, unabhängig davon, wo die Anmeldung stattfindet) oder transient (für jede Anmeldung wird ein neuer Wert generiert, den die MVPD in ihrem Backend zuordnet. Normalerweise sind die Werte, die von MVPDs für die Adobe Pass-Authentifizierung bereitgestellt werden, sitzungs- und geräteübergreifend persistent. Wie jedoch bereits erwähnt, wird die Persistenz weder garantiert noch validiert.

Dieser Wert wird zur Berechnung der eindeutigen Benutzer verwendet. Der gemeldete Wert (pro Anforderer-ID/Intervall/MVPD) wird für das jeweilige Intervall dedupliziert. Die Summe der Unique Users pro Tag unterscheidet sich daher für gewöhnlich vom Monatswert, wobei der Monatswert den niedrigeren Wert aufweist.

Diese Zahl enthält alle Ereignisse aus der Adobe Pass-Authentifizierung abzüglich Authentifizierungsversuche (die keine Benutzer-ID haben), aber einschließlich versuchter (und möglicherweise fehlgeschlagener) Autorisierungen.

### Beispiele {#examples}

#### Tag 1 {#day1}

Benutzer XYZ geht auf die Website, um ein Video anzusehen.

Ausgelöste Ereignisse:

* Authentifizierungsversuch (noch kein eindeutiger Benutzer)
* Auth. erteilt
   * An dieser Stelle identifizieren wir den Benutzer eindeutig anhand dessen, was MVPD zurückgibt - sodass die tägliche Anzahl eindeutiger Benutzer um 1 erhöht wird
   * Das AuthN-Token wird 30 Tage lang zwischengespeichert
* AuthZ-Versuch/erteiltes Ereignis
   * AuthZ-Token für 1 Tag zwischengespeichert
* Erfolgreiches Wiedergabeanfrageereignis

#### Tag 1 (später) {#day1-later-on}

Benutzer XYZ sieht sich ein anderes Video an.

Ausgelöste Ereignisse:

* Erfolgreiches Wiedergabeanforderungsereignis (die restlichen Daten werden zwischengespeichert)
* Keine Zunahme der Unique-Werte pro Tag oder pro Monat

#### Tag 3 {#day3}

Benutzer XYZ sieht sich ein anderes Video an.

Ausgelöste Ereignisse:

* AuthZ-Versuch/erteiltes Ereignis
   * Seit 1 Tag, an dem das Caching ab Tag 1 abgelaufen ist
* Erfolgreiches Wiedergabeanforderungsereignis (die restlichen Daten werden zwischengespeichert)
* Täglich Unique Users um 1 erhöht - Unique-Werte pro Monat sind immer noch 1

#### Tag 31 {#day31}

Benutzer XYZ sieht sich ein anderes Video an.

Wie in Tag 1 seit dem Ablauf der AuthN-Zwischenspeicherung.

Wenn die Autorisierung bei demselben Benutzer fehlschlägt, wird die monatliche Anzahl der Unique Users weiterhin um 1 erhöht, da es zwei Ereignisse gibt, die die Benutzer-ID enthalten - „Authentifizierung erteilt“ und „Autorisierungsversuch“.

### Single-Sign-On (SSO) {#single-sign-on-sso}

In einigen Fällen kann die Anzahl der eindeutigen Benutzer größer sein als die Anzahl der erfolgreichen Authentifizierungen. Dies ist in der Regel der Fall, wenn viele Benutzer über SSO von anderen Sites/Programmen aus eintreten und nur eine Autorisierung für die aktuelle Site/App benötigen.

### Vergleich von clientseitigen und serverseitigen eindeutigen Benutzern {#comparing-client-side-and-server-side-unique-users}

Wenn der Benutzer-ID-Wert aus `sendTrackingData()` Client-seitig verwendet wird, um eindeutige Benutzer zu zählen, sollten die Client- und Server-seitigen Zahlen übereinstimmen.

Wenn die Unterschiede erheblich sind, sind die folgenden Gründe in der Regel für Folgendes verantwortlich
Unterschied:

* Eindeutige Videowiedergabe im Vergleich zu allen einzigartigen Ereignissen. Wie bereits erwähnt, zählt die Adobe Pass-Authentifizierung eindeutige Benutzende für alle Ereignisse außer Authentifizierungsversuchen. Wenn sich der/die Benutzende also nur authentifiziert (auf der Seite), aber kein Video anzeigt, wird weiterhin eine Zunahme der Unique User Count ausgelöst.

* Zählen der Benutzer, die bei der Autorisierung fehlgeschlagen sind - Bei der Adobe Pass-Authentifizierung werden diese Benutzer ebenfalls in der gemeldeten Zahl gezählt.

<!--
## Related Information {#related-information}

- [Entitlement Service Monitoring API](/help/authentication/entitlement-service-monitoring-api.md)

-->
