---
title: Grundlegendes zu serverseitigen Metriken
description: Grundlegendes zu serverseitigen Metriken
exl-id: 516884e9-6b0b-451a-b84a-6514f571aa44
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2232'
ht-degree: 0%

---

# Grundlegendes zu serverseitigen Metriken {#understanding-server-side-metrics}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Einführung {#intro}

In diesem Dokument werden die serverseitigen Metriken der Adobe Pass-Authentifizierung beschrieben, die vom Entitlement Service Monitoring (ESM)-Dienst generiert wurden. Es werden nicht die Ereignisse beschrieben, die aus der Client-seitigen Perspektive betrachtet werden (was die Programmierer sehen würden, wenn sie einen Messungsdienst wie Adobe Analytics auf ihrer Seite/Anwendung implementieren würden).

## Ereigniszusammenfassung {#events_summary}

Vom Server-seitigen Standpunkt der Adobe Pass-Authentifizierung werden die folgenden Ereignisse generiert:

* **Im Authentifizierungsfluss generierte Ereignisse** (tatsächliche Anmeldung mit dem MVPD)

   * Benachrichtigung über AuthN-Versuch - Dies wird generiert, wenn der Benutzer an die MVPD-Anmeldeseite gesendet wird.
   * Benachrichtigung über &quot;AuthN ausstehend&quot;- Wenn es dem Benutzer gelingt, sich mit seinem MVPD anzumelden, wird dies generiert, wenn der Benutzer        zurück zur Adobe Pass-Authentifizierung umgeleitet.
   * Benachrichtigung über AuthN Granted (AuthN gewährt): Diese wird generiert, wenn der Benutzer wieder auf der Website des Programmierers ist und das Authentifizierungstoken erfolgreich aus der Adobe Pass-Authentifizierung abgerufen hat.
* **Autorisierungsfluss** (Nur Prüfung auf Autorisierung mit einer
MVPD)\
  *Voraussetzung:* Ein gültiges AuthN-Token
   * Benachrichtigung über AuthZ-Versuch
   * Benachrichtigung über die Gewährung von AuthZ
* **Anfrage zur erfolgreichen Wiedergabe**\
  *Voraussetzung:* Gültige AuthN- und AuthZ-Token
   * Benachrichtigung über einen Check mit Adobe Pass-Authentifizierung
   * Eine Wiedergabeanforderung erfordert sowohl eine erteilte Authentifizierung als auch eine erteilte Genehmigung


Die Anzahl der Unique Users wird im Abschnitt [Unique Users](#unique-users) unten ausführlich beschrieben. Da die Antworten auf erteilte Authentifizierung und Autorisierung in der Regel zwischengespeichert werden, gelten in der Regel die folgenden Formeln:

* Anzahl der AuthN-Versuche \> Anzahl der gewährten AuthN
* Anzahl der AuthZ-Versuche \> Anzahl der gewährten AuthZ
* Anzahl der AuthZ-Versuche \> Anzahl der gewährten AuthN (normalerweise)
* Anzahl erfolgreicher Wiedergabeanforderungen \> Anzahl der erteilten AuthZ


### Beispiel {#example}

Das folgende Beispiel zeigt die serverseitigen Metriken für einen Monat für
eine Marke:

| Metrik | MVPD 1 | MVPD 2 | ... | MVPD n | Ingesamt |
| -------------------------- | ------ | ------ | - | ------ | ---------------------------------------------- |
| Erfolgreiche Authentifizierungen | 1125 | 2892 |   | 2203 | SUM(MVP1+...MVPD n) |
| Erfolgreiche Berechtigungen | 2527 | 5603 |   | 5904 | SUM(MVP1+...MVPD n) |
| Erfolgreiche Abspielanforderungen | 4201 | 10518 |   | 10737 | SUM(MVP1+...MVPD n) |
| Unique Users | 1375 | 2400 |   | 2890 | SUM aller Benutzer für alle deduplizierten MVPDs\* |
| versucht Authentifizierungen | 2147 | 3887 |   | 3108 | SUM(MVP1+...MVPD n) |
| Versuchsberechtigungen | 2889 | 6139 |   | 6039 | SUM(MVP1+...MVPD n) |

</br>

Eine Deduplizierung in diesem Fall sollte keine Auswirkungen haben, da verschiedene MVPD-Benutzer nicht dieselbe Benutzer-ID erhalten sollten. Bei der Summe für zwei verschiedene Marken, aber für denselben MVPD sollte die Deduplizierungswirkung viel größer sein.

## Ereignis-Trigger {#event_triggers}

### Neuer Benutzer - Vollständiger Fluss {#new-user-full-flow}

In der folgenden Tabelle werden die Ereignisse und Schritte für einen Benutzer ohne Authentifizierungstoken beschrieben (ein neuer Benutzer oder ein Benutzer, dessen Authentifizierungstoken abgelaufen ist):



![](../../../assets/ae-flow-with-events.png)



Der Fluss umfasst Rundfahrten zu MVPDs für die Authentifizierung (#5 bis \#7) und die Autorisierung (\#11).



Nach Abschluss des Workflows werden die Authentifizierungs- und Autorisierungstoken auf dem Gerät des Benutzers zwischengespeichert. Die TTL-Werte (Time-to-Live) für Authentifizierungstoken liegen zwischen 6 Stunden und 90 Tagen. Ein AuthN-Token-Ablauf erzwingt automatisch den Ablauf eines AuthZ-Tokens. Der TTL-Wert für das Autorisierungstoken beträgt in der Regel 24 Stunden.

| Ausgelöste serverseitige Ereignisse | <ul><li>Authentifizierungsversuch, Authentifizierung ausstehend, Authentifizierung gewährt</li><li>Autorisierungsversuch, Autorisierung erteilt</li><li>Erfolgreiche Play-Anfrage</li></ul> |
|---|---|


### Wiederkehrender Benutzer - AuthZ- und AuthN-Token zwischengespeichert

Für Benutzer mit gültigen AuthZ- und AuthN-Token im Cache:
-Schritte ausgeführt werden:


![](../../../assets/ae-flow-tokens-cached-web.png)



Dies wird beim Aufruf von `getAuthorization()` automatisch ausgelöst und umfasst nur Prüfungen mit Adobe Pass-Authentifizierung. Der MVPD ist an diesem Fluss nicht beteiligt.


| Ausgelöste serverseitige Ereignisse | * Anfrage zur erfolgreichen Wiedergabe |
|---|---|


### Wiederkehrender Benutzer - AuthN-Token zwischengespeichert, AuthZ-Token abgelaufen

Für Benutzer, die noch gültige AuthN-Token haben, werden die folgenden Schritte ausgeführt:

![](../../../assets/ae-flow-authn-token-cached.png)


Dieser Fluss beinhaltet eine Hin- und Rückfahrt zum MVPD.


| Ausgelöste serverseitige Ereignisse | <ul><li>Autorisierungsversuch, Autorisierung OK</li><li>Erfolgreiche Play-Anfrage</li> |
|---|---|

## Authentifizierungsereignisse {#authn_events}

### Authentifizierungsversuch {#authentication-attempt}

Wie im obigen Diagramm dargestellt, werden die Authentifizierungsereignisse nur ausgelöst, wenn der Benutzer einen Hin- und Rücklauf zum MVPD durchführt. Authentifizierungs-Ereignisse enthalten keine zwischengespeicherten Token-Authentifizierungen.

Das Authentifizierungsversuch-Ereignis wird ausgelöst, nachdem der Benutzer in der Auswahl auf einen bestimmten MVPD geklickt hat.

* Das erste Ereignis auf der MVPD-Seite, das dieser Seite nahe liegt, ist das Laden der Seite
* Die Adobe Pass-Authentifizierung zählt keine wiederholten Anmeldeversuche des Benutzers auf der MVPD-Seite (falsches Kennwort, versuchen Sie es erneut).
* Mehrere Versuche werden als ein Versuch gezählt
* Einige MVPDs führen die Autorisierung auch im Schritt Authentifizierung durch und der Benutzer wird nicht zurückgeleitet, wenn die Autorisierung fehlschlägt.

### Authentifizierung ausstehend {#authentication-pending}

Dieses Ereignis tritt auf, wenn der Umleitungsprozess zur Adobe Pass-Authentifizierung gestartet wurde.

### Authentifizierung gewährt {#authentication-granted}

Der Benutzer ist ein bekannter Abonnent des MVPD, in der Regel mit einem Pay TV-Abonnement, manchmal aber nur mit Internetzugang. Eine erfolgreiche Authentifizierung kann entweder dadurch erfolgen, dass der Benutzer explizit gültige Anmeldeinformationen mit seinem MVPD eingegeben hat oder weil er zuvor gültige Anmeldeinformationen eingegeben und &quot;mich speichern&quot;aktiviert hatte (und die vorherige Sitzung noch nicht abgelaufen war).

Der MVPD sendet daher eine positive Antwort auf die Authentifizierungsanforderung an die Adobe Pass-Authentifizierung und die Adobe Pass-Authentifizierung erstellt ein *AuthN-Token*.

* Die Authentifizierung wird normalerweise über einen langen Zeitraum (einen Monat oder länger) zwischengespeichert. Aus diesem Grund sind Authentifizierungsereignisse erst dann mehr vorhanden, wenn das Token abläuft und der Fluss erneut gestartet wird.
* Wenn Sie von einer anderen Site/App über Single Sign-On eingehen, werden keine Authentifizierungsereignisse für den Trigger angezeigt.



### Comcast-Authentifizierung {#comcast-authentication}

Comcast weist einen anderen AuthN-Fluss auf als der Rest der MVPDs.

Die folgenden Funktionen beschreiben die Unterschiede:

* **Sitzungs-Cookie-Verhalten**: Dies führt dazu, dass alle Authentifizierungstoken vollständig entfernt werden, nachdem der Benutzer den Browser geschlossen hat. Diese Funktion ist nur im Internet verfügbar. Der Hauptzweck besteht darin sicherzustellen, dass Ihre Comcast-Sitzung nicht auf unsicheren/freigegebenen Computern beibehalten wird. Die Auswirkung besteht darin, dass es mehr Authentifizierungsversuche/genehmigte Flüsse als für den Rest der MVPDs geben wird.

* **AuthN pro Anfragende-ID**: Der Comcast lässt nicht zu, dass der AuthN-Status von einer Anfragenkennung an eine andere zwischengespeichert wird. Daher muss jede Website/App zu Comcast gehen, um ein Authentifizierungstoken zu erhalten. Abgesehen von den Überlegungen zum Benutzererlebnis wird wie oben gezeigt, dass mehr Authentifizierungsversuche/gewährte Ereignisse generiert werden.

* **Passive Authentifizierung**: Um das Benutzererlebnis zu verbessern, aber
weiterhin die AuthN-Funktionalität pro Anfrage-ID beibehalten, wird ein passiver Authentifizierungsfluss in einem ausgeblendeten iFrame durchgeführt. Der Benutzer sieht nichts, aber die Ereignisse werden weiterhin wie zuvor ausgelöst.

Wenn der Benutzer auf der Anmeldeseite Comcast auf &quot;Angaben speichern&quot;klickt, werden nachfolgende Besuche auf dieser Seite (in einem Zeitraum von 2 Wochen) nur eine kurze Umleitung ermöglichen. Andernfalls müssen sich Benutzer tatsächlich auf der Seite authentifizieren.

### Nicht erfolgreiche Authentifizierung {#unsuccessful-authentication}

Eine nicht erfolgreiche Authentifizierung ist kein Ereignis pro Person in der Adobe Pass-Authentifizierung, sondern kann als Unterschied zwischen Versuchen und Erfolgen berechnet werden.

In der Version vom Mai 2013 fügt die Adobe Pass-Authentifizierung Fehlercodes für nicht erfolgreiche Authentifizierungen hinzu, die auf System- oder Netzwerkfehler zurückzuführen sind, einschließlich DRM-Fehlern (Token-Bindung fehlgeschlagen) und LSO-Fehlern (kein Leerzeichen zum Schreiben des Tokens usw.).

### Authentifizierungskonversionsrate {#authenitication-conversion-rate}

Eine interessante Metrik, die Programmierer verfolgen können, ist die Authentifizierungskonversionsrate, berechnet als (AuthN-Anfragen / AuthN gewährt)%.

Einige Hinweise zu den Metriken:

* Da es sich um eine ereignisbasierte Metrik handelt, spiegelt sie nicht wirklich die Unique User-Konversionsrate wider - wenn ein Benutzer acht Mal versucht und das neunte Mal erfolgreich ist - spiegelt sich dies sehr schlecht in der obigen Konversionsrate wider.
* In der Adobe Pass-Authentifizierung (serverseitig) gibt es (noch) keine Möglichkeit, eine eindeutige basierte Authentifizierungskonvertierung zu berechnen.
* Wenn automatische AuthN-Zustellversuche auf der Site/in der App vorhanden sind, wirkt sich dies auch auf die obige Metrik aus.

## Autorisierungsereignisse {#authorization_events}

### Autorisierungsversuch {#authorization_attempt}

Zusätzlich zum Abrufen eines Authentifizierungstokens müssen Benutzer vor dem Abspielen von Inhalten auch ein Autorisierungstoken erhalten. Dies geschieht normalerweise nach der Authentifizierung oder nach Ablauf des Autorisierungstokens. Da diese Prüfung Server-seitig durchgeführt wird (von den Adobe Pass-Authentifizierungsservern zu den MVPD-Servern), ist der Benutzer nicht verpflichtet, irgendetwas zu tun.

### Genehmigte Genehmigung erteilt {#authorization-granted}

Eine &quot;Autorisierung erteilt&quot;signalisiert, dass das Abonnement des authentifizierten Benutzers die angeforderte Programmierung enthält.

Beachten Sie, dass nicht alle MVPDs einen separaten Autorisierungsschritt unterstützen. Für einige Authentifizierung ist mit Autorisierung gleichgesetzt. Der MVPD sendet Adobe Pass-Authentifizierung eine erfolgreiche Antwort auf die Backchannel-AuthZ-Anfrage, und die Adobe Pass-Authentifizierung erstellt ein AuthZ-Token.

* Das AuthZ-Token wird für einen bestimmten Zeitraum zwischengespeichert, in der Regel 24 Stunden Während dieses Zeitraums werden keine AuthZ-Ereignisse ausgelöst.
* Manche MVPDs funktionieren mit Asset-Level-Berechtigungen, andere arbeiten mit Kanalebene-Berechtigungen. Je nachdem, welche Option verwendet wird, werden mehr oder weniger AuthZ-Ereignisse ausgelöst. Selbst für die Autorisierung auf Kanalebene ist das Caching vorhanden. Wenn dasselbe Asset also in weniger als 24 Stunden angefordert wird, werden keine Ereignisse ausgelöst.

### Genehmigung verweigert {#authorization-denied}

Wenn eine Autorisierung verweigert wird, verfügt der authentifizierte Benutzer nicht über ein bestätigtes Abonnement für die angeforderte Programmierung. Die wahrscheinlichste Ursache ist, dass der Kanal nicht Teil des Abonnementpakets des Benutzers ist, aber dies kann auch für einen Benutzer gelten, der nur über einen Internetzugang von MVPD verfügt.

Bei einigen MVPDs sind Benutzer erfolgreich authentifiziert, obwohl sie nur über ein Internet-Abonnement des MVPD verfügen (kein Pay-TV-Abonnement). In diesem Fall wird die Autorisierung verweigert, auch wenn der Kanal, für den der Benutzer die Autorisierung anfordert, im Basispaket enthalten ist.

Einige MVPDs bieten benutzerdefinierte Fehlermeldungen für AuthZ-Ablehnungen, die Angebote zur Aktualisierung ihres Pakets enthalten können.


### Autorisierungs-Konversionsrate {#authorization-conversion-rate}

Die Konversionsrate der Authentifizierung kann als (AuthZ-Anfragen / AuthZ gewährt)% berechnet werden.

### Erfolgreiche Play-Anfrage {#successful-play-request}

Ein Benutzer, der sowohl authentifiziert als auch autorisiert ist, darf geschützte Inhalte anzeigen.

Bei einer erfolgreichen Wiedergabeanforderung generiert die Adobe Pass-Authentifizierung ein kurzlebiges Medien-Token, das bestätigt, dass der Benutzer berechtigt ist, das angeforderte Video anzuzeigen. Der Programmierer verwendet dieses Media Token zur weiteren Validierung des potenziellen Betrachters. Media Tokens werden als erfolgreiche Wiedergabeanforderungen verfolgt.

* Die Adobe Pass-Authentifizierung verfolgt *nicht*, ob die Videowiedergabe nach der Generierung des Medien-Tokens tatsächlich begonnen hat. Wenn es beispielsweise eine Geo-Beschränkung für den Inhalt gibt, zählt die Transaktion weiterhin als erfolgreiche Wiedergabeanforderung, auch wenn der Stream nie tatsächlich gestartet wird.
* Da AuthN- und AuthZ-Token die MVPD-Antwort für einen bestimmten Zeitraum zwischenspeichern, ist das erfolgreiche Wiedergabeanforderungsereignis das häufigste Ereignis in den Metriken.

## Unique Users {#unique-users}

### Definition {#definition}

Bei erfolgreicher Authentifizierung verfolgt die Adobe Pass-Authentifizierung die Existenz eines eindeutigen Benutzers basierend auf dem zurückgegebenen MVPD-Benutzer-ID-Wert.  Dieser Wert basiert auf den Anmeldeinformationen des Benutzers, enthält jedoch keine personenbezogenen Informationen.

Dieser Wert wird auch im Rückruf sendTrackingData an die Site/App übergeben.

Dieser Wert kann geräteübergreifend persistent sein (der MVPD erzeugt für einen bestimmten Benutzer denselben Wert, unabhängig davon, wo die Anmeldung erfolgt) oder transient (für jede Anmeldung wird ein neuer Wert generiert, den der MVPD in seinem Backend zuordnet. In der Regel sind die Werte, die von MVPDs für die Adobe Pass-Authentifizierung bereitgestellt werden, sitzungs- und geräteübergreifend persistent, aber wie angegeben, wird die Persistenz weder garantiert noch validiert.

Dieser Wert dient zur Berechnung der Unique Users. Der gemeldete Wert (pro Anfrage-ID/Intervall/MVPD) wird für das jeweilige Intervall dedupliziert. Die Summe der Unique Users pro Tag unterscheidet sich also normalerweise vom Monatswert, wobei der Monatswert den niedrigeren Wert aufweist.

Diese Zahl enthält alle Ereignisse aus der Adobe Pass-Authentifizierung abzüglich Authentifizierungsversuche (die keine Benutzer-ID aufweisen), jedoch einschließlich der versucht wurde (und möglicherweise fehlgeschlagenen) Berechtigungen.

### Beispiele {#examples}

#### Tag 1 {#day1}

Benutzer XYZ besucht die Site, um sich ein Video anzusehen.

Ausgelöste Ereignisse:

* AuthN-Versuch (noch kein eindeutiger Benutzer)
* AuthN erteilt
   * an diesem Punkt identifizieren wir den Benutzer eindeutig anhand dessen, was der MVPD zurückgibt - sodass die tägliche Unique User-Anzahl um 1 erhöht wird.
   * Das AuthN-Token wird 30 Tage lang zwischengespeichert.
* AuthZ-Versuch/gewährtes Ereignis
   * AuthZ-Token 1 Tag lang zwischengespeichert
* Erfolgreiches Wiedergabeanforderungsereignis

#### Tag 1 (später am) {#day1-later-on}

Benutzer XYZ sieht sich ein weiteres Video an.

Ausgelöste Ereignisse:

* Erfolgreiches Wiedergabeanforderungsereignis (der Rest wird zwischengespeichert)
* Keine Erhöhung der individuellen Tageswerte oder Monatswerte

#### Tag 3 {#day3}

Benutzer XYZ sieht sich ein weiteres Video an.

Ausgelöste Ereignisse:

* AuthZ-Versuch/gewährtes Ereignis
   * Seit 1 Tag ist die Zwischenspeicherung ab Tag 1 abgelaufen
* Erfolgreiches Wiedergabeanforderungsereignis (der Rest wird zwischengespeichert)
* Unique Users pro Tag erhöht um 1 - Unique Visitors pro Monat sind immer noch 1

#### Tag 31 {#day31}

Benutzer XYZ sieht sich ein weiteres Video an.

Wie an Tag 1, seit die AuthN-Zwischenspeicherung abgelaufen ist.

Wenn derselbe Benutzer die Autorisierung nicht erfolgreich abschließen sollte, wird die Anzahl der Unique Users pro Monat dennoch um 1 erhöht, da es zwei Ereignisse gibt, die die Benutzer-ID enthalten: Authentifizierung gewährt und Autorisierungsversuch.

### Single-Sign-On (SSO) {#single-sign-on-sso}

In einigen Fällen kann die Anzahl der Unique Users größer sein als die Anzahl der erfolgreichen Authentifizierungen. Dies ist normalerweise der Fall, wenn viele Benutzer von anderen Sites/Apps per SSO eingehen und nur eine Autorisierung für die aktuelle Site/App benötigen.

### Vergleich zwischen Client- und Server-seitigen Unique Users {#comparing-client-side-and-server-side-unique-users}

Wenn der Benutzer-ID-Wert von `sendTrackingData()` clientseitig verwendet wird, um Unique Users zu zählen, sollten die clientseitigen und serverseitigen Zahlen übereinstimmen.

Wenn es sich bei den Unterschieden um wesentliche Unterschiede handelt, sind in der Regel die folgenden Gründe für die
difference:

* Eindeutige Videowiedergabe im Vergleich zu allen Ereignissen. Wie bereits erwähnt, zählt die Adobe Pass-Authentifizierung Unique Users für alle Ereignisse außer AuthN-Versuchen. Das bedeutet, dass eine Erhöhung der Unique User-Anzahl immer noch ausgelöst wird, wenn sich der Benutzer nur authentifiziert (auf der Seite), aber kein Video anzeigt.

* Zählung von Benutzern, die die Autorisierung fehlgeschlagen haben - Adobe Pass-Authentifizierung zählt diese Benutzer auch in der gemeldeten Zahl.

<!--
## Related Information {#related-information}

- [Entitlement Service Monitoring API](/help/authentication/entitlement-service-monitoring-api.md)

-->
