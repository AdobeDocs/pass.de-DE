---
title: Integrieren von Server-seitigen Daten der Adobe Pass-Authentifizierung in Adobe Analytics
description: Integrieren von Server-seitigen Daten der Adobe Pass-Authentifizierung in Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Integrieren von Server-seitigen Daten der Adobe Pass-Authentifizierung in Adobe Analytics

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Kunden mit Adobe Pass-Authentifizierung möchten die Server-seitigen Daten der Adobe Pass-Authentifizierung (Adobe Pass) im Adobe Analytics-Dashboard anzeigen, um die Nutzung zu erleichtern.

Die Daten dienen dazu, wichtige TVE-Metriken wie Authentifizierungs-Konversionsraten pro MVPD, Unique Users basierend auf der MVPD User ID und mehr zu verfolgen.

Sie soll keine Client-seitige Implementierung ersetzen, sofern bereits eine vorhanden ist, da die Benutzeraktivität ohne Besucher-ID über die unten stehenden spezifischen Ereignisse hinaus nicht verfolgt werden kann. Wenn Kunden bei Pass-Aufrufen eine Besucher-ID angeben, können wir einen anderen Typ der Analytics-Integration entsperren - Echtzeit -, der alle Pass-Ereignisse mit den vorhandenen Kundendaten verbinden kann. Weitere Details zu diesem neuen Typ der möglichen Integration finden Sie hier: &quot;[Verwenden der Experience Cloud-ID in der Adobe Pass-Authentifizierung](/help/authentication/integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)&quot;

## Enthaltene Metriken {#metrics-included-int-authn-analyt}

| Ereignis | Beschreibung |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| Authentifizierung angefordert | Anzahl der initiierten Authentifizierungsflüsse |
| Auth. ausstehend | Anzahl der erfolgreich generierten Authentifizierungs-Token (unabhängig davon, ob der Client sie tatsächlich erhalten hat oder nicht) |
| AuthNr. OK | Anzahl der von Benutzern erfolgreich abgerufenen Authentifizierungs-Token |
| AuthZ angefordert | Anzahl der Genehmigungsversuche |
| AuthZ OK | Anzahl erfolgreicher Autorisierungen |
| AuthZ fehlgeschlagen | Anzahl der verweigerten Genehmigungen durch MVPDs auf Anwendungsebene |
| Anfrage abspielen | Anzahl der generierten Short-Media-Token (die der Anzahl der Wiedergabeanfragen entsprechen) |
| Abmelden angefordert | Anzahl der initiierten Abmeldevorgänge |
| Abmelden abgeschlossen | Anzahl der erfolgreich abgeschlossenen Abmeldevorgänge |
| Abmelden fehlgeschlagen | Anzahl der fehlgeschlagenen Abmeldevorgänge |
| Vorabautorisierung angefordert | Anzahl der initiierten Vorabautorisierungsflüsse |
| Vorautorisierung OK | Anzahl erfolgreicher Vorautorisierungsereignisse mit den vorab autorisierten Ressourcen |
| Vorautorisierung verweigert | Anzahl der Vorautorisierungsereignisse mit den Ressourcen, denen die Vorautorisierung verweigert wurde |
| Vorautorisierung fehlgeschlagen | Anzahl fehlgeschlagener Vorautorisierungs-Ereignisse |

| Adobe Analytics-Name | Beschreibung |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kanal | Die Anforderer-ID, die zum Ausführen der Berechtigungsanfrage verwendet wird |
| MVPD | Die MVPD, die für die Gewährung der Berechtigung an den Benutzer verantwortlich ist |
| Proxy | Der Proxy-MVPD (der bei direkten Integrationen „direkt“ ist) |
| SDK-Typ | Die verwendete Client-SDK (Flash, HTML5, Android nativ, iOS, clientless usw.) |
| SDK-Version | Die Version des Adobe Pass-Authentifizierungs-Clients SDK |
| Ressourcen-ID | Der tatsächlich an der Autorisierungsanfrage beteiligte Ressourcentitel (extrahiert aus der MRSS-Payload als Element/Titel, falls angegeben) |
| AuthZ-Fehlertyp | Der Grund für die Fehler, wie von Adobe Pass Authentication <br/> gemeldet. Hier finden Sie die häufigsten Werte <br/> **noAuthZ** = Der MVPD antwortete, dass der Benutzer den Kanal nicht in seinem Paket habe<br/> **Netzwerk** = Wir konnten die MVPD nicht erreichen (MVPD hat zum Zeitpunkt des Anrufs ein Problem und hat nicht geantwortet)<br/> **norefreshToken** = Dies ist ausschließlich für OAuth-Implementierungen. Dies kann vorkommen, wenn Benutzende ihr Kennwort ändern oder MVPD es aus irgendeinem Grund verweigert. Dies führt normalerweise zu einer neuen Authentifizierung<br/> **nicht übereinstimmen** = wenn die Anfrage von einem Gerät stammt, das sich von dem Gerät mit dem Authentifizierungs-Token unterscheidet. Dies kann vorkommen, wenn Benutzende versuchen, das System auszutricksen, aber die meisten davon ereigneten sich im Kontext unserer alten JavaScript SDK, wo die Geräte-ID die IP-Adresse als Teil der Berechnung verwendet hat. Wenn ein(e) Benutzende(r) TVE zu Hause und dann bei der Arbeit angeschaut hat, wird dieser Fehler ausgelöst und er/sie muss sich erneut authentifizieren<br/> **ungültig** = ungültige Anfrage, fehlende oder ungültige Parameter<br/>  **authzNone** = Programmierer können die Autorisierung für eine bestimmte channelxMVPD-Kombination verweigern. Dies wird durch eine Backend-API ausgelöst, auf die Programmierer Zugriff haben<br/> **Betrug** = das ist ein Schutzmechanismus auf unserer Seite. Wenn die Autorisierung fehlschlägt und der Benutzer sie dann mehrmals in einem kurzen Intervall (Sekunden) erneut anfordert, verweigern wir den Aufruf direkt. Dies geschieht in der Regel, wenn ein Programmierer einen Fehler in seiner Implementierung hat, der ständig um Autorisierung bittet, wenn er fehlschlägt. |
| Token-Typ | Wenn Token aufgrund von AuthZ All und AuthN All erstellt werden, müssen wir wissen, was durch eine Abbaumaßnahme verursacht wird.<br/> Sie sind:<br/> „normal“ = der Normalfall<br/> „authnall“ = Wenn AuthN Alle aktiviert ist<br/> „authzall“ = Wenn AuthZ Alle aktiviert ist<br/> „hba“ = Wenn HBA aktiviert ist |
| Clientloser Gerätetyp | Die Geräteplattform (alternativ), die derzeit für Clientless verwendet wird.<br/> Die Werte können sein: <br/> Nicht zutreffend - das Ereignis stammt nicht von einer Client-losen SDK<br/> Unbekannt - Da der deviceType-Parameter von einer **Client-API** optional ist, gibt es Aufrufe, die keinen Wert enthalten.<br/> Alle anderen Werte, die über die **Clientless-API“ gesendet**. Beispiel: xbox, appletTV und roku. |
| MVPD-Benutzer-ID | Ersetzt die Cookie-basierte Besucher-ID |


## Details {#details-int-authn-analyt}

* Die Metriken werden ereignisweise über die Adobe Analytics Data Insertion API in die jeweilige Report Suite eingefügt
* Die Einfügung wird im Batch-Modus vorgenommen und alle 30 Minuten gesendet. Aus diesem Grund muss der Bericht mit einem Zeitstempel versehen werden
* Jeder Kunde verfügt über eine oder mehrere Report Suites. Eine Anforderer-ID (Kanal) wird nur einer Report Suite zugeordnet. Mehrere Anforderer-IDs können nur einer Report Suite zugeordnet werden.
* Historische Daten können bereitgestellt werden, jedoch ist aufgrund von Traffic-/Leistungsproblemen besondere Vorsicht geboten.
* Die Unique-Visitor-Variable ist auf die MVPD-Benutzer-ID festgelegt
* Die Zuordnung von Ereignissen und eVars ist nicht konfigurierbar.


## SLA {#sla-int-authn-serv-anal}

Da es sich nicht um eine kritische Komponente handelt, gibt es keine SLA-Garantie für die Integration.

## Report Suite-Konfiguration {#report-suite-config}

Der Bericht muss mit einem Zeitstempel versehen sein, da die Ereignisse in Stapeln gesendet werden.

### -Events {#report-suite-config-events}


>[!NOTE]
>Alle sollten wie folgt festgelegt werden:
>
>* Zähler (keine Unterbeziehungen)

| Ereignis | Adobe Analytics-Ereignis |
|---------------------------------------|-----------------------|
| Authentifizierung angefordert | event1 |
| Auth. ausstehend | event2 |
| AuthNr. OK | event3 |
| AuthZ angefordert | event4 |
| AuthZ OK | event5 |
| AuthZ fehlgeschlagen | event6 |
| Anfrage abspielen | event7 |
| Authentifizierung fehlgeschlagen | event8 |
| Client-loses Authentifizierungs-Token Anfrage OK | event9 |
| Client-loses Authentifizierungs-Token fehlgeschlagen | event10 |
| Play-Anfrage fehlgeschlagen | event11 |
| Abmeldeanforderung | event12 |
| Abmelden abgeschlossen | event13 |
| Abmelden fehlgeschlagen | event14 |
| PreFlight-Anfrage | event15 |
| PreFlight fehlgeschlagen | event16 |
| PreFlight erteilt | event17 |
| PreFlight verweigert | event18 |


### eVars {#evars}


>[!NOTE]
>Alle sollten wie folgt festgelegt werden:
>
>* Zuordnung: Zuletzt verwendet (Letzte)
>* Läuft ab nach: Treffer
>* Typ: Textzeichenfolge

| Eigenschaft | eVar |
|-----------------------------------|--------------------------------|
| Kanal | EVAR1 |
| MVPD | EVAR2 |
| Proxy | EVAR3 |
| SDK-Typ | EVAR4 |
| SDK-Version | EVAR5 |
| Ressourcen-ID | EVAR6 |
| AuthZ-Fehlertyp | EVAR7 |
| Token-Typ | EVAR8 |
| Clientloser Gerätetyp | EVAR9 |
| MVPD-Benutzer-ID | visitorID (erfolgt automatisch) |
| MVPD-Benutzer-ID | eVar10 |
| Gerätetyp | eVar11 |
| Betriebssystem | eVar12 |
| Primärer Hardwaretyp | eVar13 |
| TTL | eVar14 |
| Authentifizierungstyp | eVar15 |
| Version der Serverarchitektur | eVar16 |
| Externer Authentifizierungsanbieter | eVar17 |
| Latenz | eVar18 |
| Besucher-ID | eVar19 |
| SSO-Mechanismus | eVar20 |
| Gerätemodell | eVar21 |
| Geräteversion | eVar22 |
| Geräte-Hardwaremodell | eVar23 |
| Gerätehardware-Anbieter | eVar24 |
| Gerätehardware-Hersteller | eVar25 |
| Gerätehardwareversion | eVar26 |
| Betriebssystemname des Geräts | eVar27 |
| Betriebssystemfamilie des Geräts | eVar28 |
| Anbieter des Betriebssystems für Geräte | eVar29 |
| Betriebssystemversion des Geräts | eVar30 |
| Geräte-Browser-Name | eVar31 |
| Gerätebrowser-Anbieter | eVar32 |
| Gerätebrowser-Version | eVar33 |
| Normalisierter Client-loser Gerätetyp | eVar34 |

## Preisgestaltung {#pricing}

Weitere Informationen erhalten Sie von Ihrem TAM.
