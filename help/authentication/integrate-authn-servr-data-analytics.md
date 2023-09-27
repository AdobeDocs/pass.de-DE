---
title: Integrieren von serverseitigen Adobe Pass-Authentifizierungsdaten in Adobe Analytics
description: Integrieren von serverseitigen Adobe Pass-Authentifizierungsdaten in Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1138'
ht-degree: 4%

---

# Integrieren von serverseitigen Adobe Pass-Authentifizierungsdaten in Adobe Analytics

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Kunden mit Adobe Pass-Authentifizierung möchten die serverseitigen Daten der Adobe Pass-Authentifizierung (Adobe Pass) im Adobe Analytics-Dashboard sehen, um die Nutzung zu erleichtern.

Die Daten dienen dazu, wichtige TVE-Metriken wie Authentifizierungskonversionsraten pro MVPD, Unique Users basierend auf der MVPD-Benutzer-ID und mehr zu verfolgen.

Es soll keine clientseitige Implementierung ersetzen, wenn bereits eine vorhanden ist, da die Benutzeraktivität ohne Besucher-ID nicht über die spezifischen Ereignisse unten hinaus verfolgt werden kann. Wenn die Kunden eine Besucher-ID für Pass-Aufrufe bereitstellen, können wir einen anderen Analytics-Integrationstyp - in Echtzeit - entsperren, der alle Pass-Ereignisse mit den vorhandenen Kundendaten verbinden kann. Weitere Informationen zu diesem neuen Integrationstyp finden Sie hier: &quot;[Verwenden der Experience Cloud-ID in der Adobe Pass-Authentifizierung](/help/authentication/exp-cloud-id-authn.md)&quot;

## Einbezogene Metriken {#metrics-included-int-authn-analyt}

| Ereignis | Beschreibung |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| AuthN angefordert | Anzahl der initiierten Authentifizierungsflüsse |
| AuthN ausstehend | Anzahl der erfolgreich generierten Authentifizierungstoken (unabhängig davon, ob der Client sie tatsächlich erhalten hat oder nicht) |
| AuthN OK | Anzahl der Authentifizierungstoken, die von Benutzern erfolgreich abgerufen wurden |
| AuthZ angefordert | Anzahl der erteilten Genehmigungen |
| AuthZ OK | Anzahl erfolgreicher Zulassungen |
| AuthZ fehlgeschlagen | Anzahl der verweigerten Genehmigungen durch MVPDs auf Anwendungsebene |
| Play-Anfrage | Anzahl der generierten Kurzmedia-Token (die mit der Anzahl der Wiedergabeanforderungen übereinstimmen) |
| Abmelden angefordert | Anzahl der gestarteten Abmeldevorgänge |
| Abgemeldet abgeschlossen | Anzahl erfolgreich abgeschlossener Abmeldevorgänge |
| Abmeldefehler | Anzahl fehlgeschlagener Abmeldevorgänge |
| Vorabgenehmigung beantragt | Anzahl der initiierten Vorabgenehmigungsströme |
| Vorautorisierung OK | Anzahl erfolgreicher Vorautorisierungsereignisse mit den Ressourcen, die vorautorisiert wurden |
| Vorabgenehmigung verweigert | Anzahl der Ereignisse vor der Autorisierung mit den Ressourcen, denen die Vorabgenehmigung verweigert wurde |
| Vorautorisierung fehlgeschlagen | Anzahl fehlgeschlagener Vorautorisierungsereignisse |

| Adobe Analytics-Name | Beschreibung |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kanal | Die Anfragende-ID, die zum Ausführen der Berechtigungsanforderung verwendet wird |
| MVPD | Der MVPD, der für die Gewährung der Berechtigung an den Benutzer verantwortlich ist |
| Proxy | Der Proxy-MVPD (der für direkte Integrationen &quot;Direkt&quot;sein wird) |
| SDK-Typ | Das verwendete Client-SDK (Flash, HTML5, natives Android-SDK, iOS, Clientless usw.) |
| SDK-Version | Die Version des Adobe Pass Authentication Client SDK |
| Ressourcen-ID | Der tatsächliche Ressourcentitel, der an der Autorisierungsanforderung beteiligt ist (aus der MRSS-Payload als Element/Titel extrahiert, sofern angegeben) |
| AuthZ-Fehlertyp | Der Grund für Fehler, wie von der Adobe Pass-Authentifizierung gemeldet <br/> Die häufigsten Werte sind: <br/> **noAuthZ** = der MVPD antwortete, dass der Kanal nicht in seinem Paket enthalten sei.<br/> **network** = wir konnten den MVPD nicht erreichen (MVPD hat zum Zeitpunkt des Anrufs ein Problem und hat nicht geantwortet)<br/> **norefreshToken** = dies gilt ausschließlich für OAuth-Implementierungen und es kann dazu führen, dass der Benutzer sein Kennwort geändert oder der MVPD es aus irgendeinem Grund verweigert hat. Dies führt normalerweise zu einer neuen Authentifizierung<br/> **nicht übereinstimmend** = wenn die Anfrage von einem Gerät stammt, das sich von dem mit dem Authentifizierungstoken unterscheidet. Dies kann dazu führen, dass Benutzer versuchen, das System zu betreiben, aber die meisten dieser Fälle erfolgten im Kontext unseres alten JavaScript-SDK, wo die Geräte-ID die IP-Adresse als Teil der Berechnung verwendete. Wenn ein Benutzer TVE zu Hause und dann am Arbeitsplatz angesehen hat, wird dieser Fehler ausgelöst und er muss sich erneut authentifizieren<br/> **ungültig** = ungültige Anforderung, fehlende oder ungültige Parameter<br/>  **authzNone** = Programmierer können Berechtigungen für eine bestimmte channelxMVPD-Kombination verweigern. Dies wird durch eine Backend-API ausgelöst, auf die Programmierer Zugriff haben<br/> **Betrug** = es ist ein Schutzmechanismus auf unserer Seite. Wenn der Benutzer die Autorisierung nicht erfolgreich durchführt und diese dann innerhalb kurzer Zeit (in Sekunden) wiederholt anfordert, wird der Aufruf direkt verweigert. Dies geschieht normalerweise, wenn ein Programmierer einen Fehler in seiner Implementierung hat, der ständig eine Autorisierung verlangt, wenn er fehlschlägt. |
| Token-Typ | Wenn Token aufgrund von AuthZ All und AuthN All erstellt werden, müssen wir wissen, was durch eine Abbaumaßnahme verursacht wird.<br/> Sie sind:<br/> &quot;normal&quot;= der Normalfall<br/> &quot;authnall&quot; = Wenn AuthN All aktiviert ist<br/> &quot;authzall&quot; = Wenn AuthZ All aktiviert ist<br/>  &quot;hba&quot; = Wenn HBA aktiviert ist |
| Clientloser Gerätetyp | Die Geräteplattform (Alternative), die derzeit für ClientLess verwendet wird.<br/> Die Werte können wie folgt lauten:<br/> K/A - das Ereignis stammt nicht von einem Client-losen SDK<br/> Unbekannt - Seit dem Parameter deviceType von einer **Clientlose API** optional ist, gibt es Aufrufe, die keinen Wert enthalten.<br/> Jeder andere Wert, der über den **Clientlose API**. Beispiel: xbox, appletv und roku. |
| MVPD User ID | Ersetzt Cookie-basierte Besucher-ID |


## Details {#details-int-authn-analyt}

* Die Metriken werden über die Adobe Analytics Data Insertion API (Dateneinfüge-API) jedes Ereignis in die jeweilige Report Suite eingefügt
* Die Einfügung erfolgt in Batches und wird alle 30 Minuten gesendet. Aus diesem Grund muss der Bericht mit einem Zeitstempel versehen werden
* Jeder Kunde verfügt über eine oder mehrere Report Suites. Eine Anfrage-ID (Kanal) wird nur einer Report Suite zugeordnet. Mehrere Anforderer-IDs können nur einer Report Suite zugeordnet werden.
* Historische Daten können zur Verfügung gestellt werden, aber aufgrund von Verkehrs-/Leistungsproblemen ist besondere Vorsicht geboten.
* Die Unique Visitor-Variable ist auf die MVPD-Benutzer-ID festgelegt.
* Die Zuordnung von Ereignissen und eVars kann nicht konfiguriert werden.


## SLA {#sla-int-authn-serv-anal}

Da es sich nicht um eine kritische Komponente handelt, gibt es keine SLA-Garantie für die Integration.

## Report Suite-Konfiguration {#report-suite-config}

Der Bericht muss mit einem Zeitstempel versehen werden, da die Ereignisse in Batches gesendet werden.

### Veranstaltungen {#report-suite-config-events}


>[!NOTE]
>Alle sollten wie folgt festgelegt werden:
>
>* Zähler (keine Unterbeziehungen)

| Ereignis | Adobe Analytics-Ereignis |
|---------------------------------------|-----------------------|
| AuthN angefordert | event1 |
| AuthN ausstehend | event2 |
| AuthN OK | event3 |
| AuthZ angefordert | event4 |
| AuthZ OK | event5 |
| AuthZ fehlgeschlagen | event6 |
| Play-Anfrage | event7 |
| AuthN fehlgeschlagen | event8 |
| Clientlose AuthN-Token-Anfrage OK | event9 |
| Clientlose AuthN-Token-Anfrage fehlgeschlagen | event10 |
| Abspielanforderung fehlgeschlagen | event11 |
| Abmeldeanforderung | event12 |
| Abgemeldet abgeschlossen | event13 |
| Abmeldefehler | event14 |
| Preflight-Anfrage | event15 |
| Preflight fehlgeschlagen | event16 |
| Vorfeld gewährt | event17 |
| Preflight verweigert | event18 |


### eVars {#evars}


>[!NOTE]
>Alle sollten wie folgt festgelegt werden:
>
>* Zuordnung: Zuletzt verwendet (letzte)
>* Läuft ab nach: Treffer
>* Typ: Textzeichenfolge

| Eigenschaft | eVar |
|-----------------------------------|--------------------------------|
| Kanal | eVar1 |
| MVPD | eVar2 |
| Proxy | eVar3 |
| SDK-Typ | eVar4 |
| SDK-Version | eVar5 |
| Ressourcen-ID | eVar6 |
| AuthZ-Fehlertyp | eVar7 |
| Token-Typ | eVar8 |
| Clientloser Gerätetyp | eVar9 |
| MVPD User ID | visitorID (automatisch durchgeführt) |
| MVPD User ID | eVar10 |
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
| Gerätemaschinenmodell | eVar23 |
| Gerätehardware-Anbieter | eVar24 |
| Hersteller von Gerätehardware | eVar25 |
| Hardware-Version | eVar26 |
| Betriebssystemname des Geräts | eVar27 |
| Device OS-Familie | eVar28 |
| Betriebssystemanbieter des Geräts | eVar29 |
| Betriebssystemversion des Geräts | eVar30 |
| Browser-Name des Geräts | eVar31 |
| Anbieter des Geräte-Browsers | eVar32 |
| Gerätebrowser-Version | eVar33 |
| Normalisierter Clientloser Gerätetyp | eVar34 |

## Preise {#pricing}

Weitere Informationen erhalten Sie von Ihrem TAM.
