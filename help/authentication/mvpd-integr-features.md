---
title: MVPD-Integrationsfunktionen
description: MVPD-Integrationsfunktionen
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1711'
ht-degree: 2%

---

# MVPD-Integrationsfunktionen

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#mvpd-int-features-overview}

Adobe Pass Authentication unterstützt die neuen Standards für Fernsehen überall. Sie entspricht dem **Spezifikation für CableLabs OLCA (Online Content Access)**, die technische Anforderungen und Architektur für die Bereitstellung von Videos von Online-Quellen an einen Pay TV-Kunden bietet. Adobe nahm im Juni 2011 am gemeinsamen CableLabs-Interopt-Test-Projekt teil und hat den Testprozess für eine Service Provider-Implementierung bestanden.  Adobe ist auch ein aktives Mitglied der **OATC (Open Authentication Technical Consortium)** und nimmt an mehreren Projekten der Fachausschüsse teil, die in diesem Gremium ausgearbeitet werden.

Nachdem Sie die Einhaltung der Authentifizierungs-OLCA-Konformität und die Teilnahme von Adobe Pass an OATC beschrieben haben, ist es auch wichtig, darauf hinzuweisen, dass die Authentifizierung tatsächlich &quot;stilistisch&quot;ist.  Aber in dieser Phase der TVE-Ära orientiert sich die Adobe Pass-Authentifizierung definitiv an den OLCA-Standards.  In den Standards werden derzeit vereinbarte Methoden zur Implementierung von TVE-Funktionen durch die verschiedenen TVE-Player (Programmierer, MVPDs und Service Provider) definiert. Viele dieser Funktionen sind in den folgenden Tabellen aufgeführt, einschließlich Links zu verwandten Seiten, die Details und Beispiele zur Implementierung der Funktionen enthalten.

Die Informationen in den Tabellen sollen den Integrationsprozess der Adobe Pass-Authentifizierung zu konsistenten Funktionen für alle MVPD-Integrationen führen. Die Prioritätsspalte ordnet die Funktionen in A, B und C zu:

* A - Dies sind Funktionen, die für das erstmalige Rollout einer Integration erforderlich sind.
* B - Dies sind wichtige Verbesserungen der ersten Integration, die nach dem ersten Rollout hinzugefügt werden.
* C - Dies sind weitere Verbesserungen der Integration, die nach den &quot;B&quot;-Anforderungen implementiert werden können.


## 1. Hauptfunktionen {#core-func-features}


| Anzahl | Funktion | Beschreibung | Priorität | Hinweise |
|------|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.1 | [Vom Programmierer initiierte Anmeldung](/help/authentication/authn-oauth2-protocol.md) | Der Viewer wählt den MVPD aus und initiiert den Authentifizierungsfluss (AuthN) von der Website oder Anwendung der Programmierermarke. | A+ |                                                                                                                                                         |
| 1.2 | [Kanalbasierte Autorisierung](/help/authentication/authz-usecase.md) | Nach der Authentifizierung kann die AuthZ-Autorisierung im Hintergrund erfolgen, wobei nur eine Netzwerkkanal-ID und eine Benutzer-ID an den MVPD übergeben werden. | A+ |                                                                                                                                                         |
| 1.3 | UserID-Persistenz | Der MVPD bietet eine verschleierte, beständige UserID. | A |                                                                                                                                                         |
| 1.4 | [Single-Sign-On-Support](/help/authentication/sso-support.md) | Der Viewer meldet sich mit dem MVPD auf der Site einer Marke an und kann dann zu einer anderen Marke wechseln und nicht aufgefordert werden, sich erneut anzumelden. Die AuthN-Sitzung wird von den Marken gemeinsam genutzt. SSO-Unterstützung ist sowohl für Websites als auch für Mobile-/Device-Anwendungen verfügbar.  Bei MVPDs muss entweder eine Benutzer-ID oder ein anderes Benutzer-Token zurückgegeben werden, das für AuthZ über verschiedene Marken hinweg verwendet werden kann. | A |                                                                                                                                                         |
| 1.5 | Geräteoptimiertes Benutzererlebnis bei der Anmeldung (UX) | Der MVPD unterstützt die Größenanpassung des Anmeldebildschirms, sodass er den Abmessungen des von der Ansicht verwendeten Geräts entspricht. | A |                                                                                                                                                         |
| 1.6 | Standard-MVPD-Picker-Logo | Der MVPD bietet eine URL zu einem Standardlogo mit entsprechenden Abmessungen (112 x 33 Pixel). | A | Das Logo sollte vom MVPD gehostet werden und sollte von CDN zwischengespeichert werden. |
| 1.7 | [Umfang des Dienstleisters](/help/authentication/serv-provider-scoping.md) | MVPD unterstützt die Übergabe der Markenkennung (Anforderungswert) in der AuthN-Anfrage. | A- | Dies ermöglicht eine dienstanbieterspezifische Anmeldung. |
| 1.8 | [Mehrkanal-Autorisierung](/help/authentication/mvpd-preflight-authz.md#preflight-multich-authz) | Der MVPD unterstützt einen Programmierer, der mehrere Kanäle in einer einzigen Autorisierungsanfrage sendet. | B+ |                                                                                                                                                         |
| 1.9 | iFrame- oder JS-Popup-basierte Authentifizierung | Der Programmierer kann den Login-Fluss in ein iFrame- oder Popup-Erlebnis integrieren, anstatt eine HTTP-Umleitung durchzuführen. | B |                                                                                                                                                         |
| 1.10 | [Vom Programmierer initiierte Abmeldung](/help/authentication/usecase-mvpd-logout.md) | Der Viewer verfügt über eine authentifizierte Sitzung sowohl auf der Website oder in der App des Programmierers als auch mit dem MVPD. Der Betrachter kann eine föderierte Abmeldung von der Programmer-Site auslösen, und die Abmeldung löscht auch die Sitzung im MVPD-Portal. | B | Stellt sicher, dass freigegebene Computer aus Sicht des Programmierers besser vor Missbrauch geschützt sind. |
| 1.11 | [Fehlermeldung bezüglich benutzerdefinierter Autorisierung](/help/authentication/error-reporting.md) | Der MVPD übergibt eine eigene benutzerdefinierte Fehlerzeichenfolge, die für die verknüpfte Site oder App des Programmierers geeignet ist, um sie dem Benutzer anzuzeigen. | B | Aktiviert Upsell-Szenarien |
| 1.12 | **Benutzermetadaten in der Authentifizierungsantwort** | Die MVPD AuthN-Antwort kann Benutzermetadaten enthalten, die während des Berechtigungsflusses als Hinweis für die Personalisierung des Benutzererlebnisses dienen. Diese Anforderung ermöglicht elterliche Kontrollhinweise vom MVPD zum Programmierer. | B- |                                                                                                                                                         |
| 1.13 | MVPD-initiierte Authentifizierung | Der Viewer schließt eine erfolgreiche AuthN-Sitzung im MVPD-Portal ab und navigiert dann zur TVE-Site des Programmierers. Der Benutzer wird nicht zur MVPD-Auswahl aufgefordert und wird automatisch authentifiziert. | B- |                                                                                                                                                         |
| 1.14 | UserID-Scoping | Die MVPD UserID sollte in zwei Formen auftreten - eine auf Programmierer übertragen und die andere auf Betrügereien.  Dies ermöglicht es Adobe, ohne weitere Verschlüsselung/Verschleierung die programmgesteuerte MVPD UserID freizugeben. | C |                                                                                                                                                         |
| 1.15 | Asset-basierte Autorisierung | Sobald AuthN abgeschlossen ist, kann AuthZ im Hintergrund auftreten, indem strukturierte Daten übergeben werden, die Netzwerk-, Anzeigen-, Asset-, elterliche Kontrollbewertung und mehr nach Bedarf umfassen können. Dies ermöglicht die elterliche Kontrolle bei jedem AuthZ-Aufruf vom Programmierer zum MVPD. | C |                                                                                                                                                         |
| 1.16 | MVPD-initiierte Abmeldung | Der Viewer verfügt über eine authentifizierte Sitzung sowohl auf der Website oder in der App des Programmierers als auch mit dem MVPD. Der Betrachter kann eine föderierte Abmeldung von der MVPD-Site starten, die auch die Sitzung auf allen Federated Programmer-Sites löscht. | C |                                                                                                                                                         |
| 1.17 | Zulassungspflichten | Der MVPD bietet zusätzliche Bedingungen in der AuthZ-Antwort, wie z. B. Protokollierung oder eine aktualisierte maximale elterliche Kontrollberechtigung (OLCA). | C |                                                                                                                                                         |
| 1.18 | IP-Adressenkontext | Der MVPD erfordert die explizite Übergabe der IP-Adresse. Bei AuthZ, bei dem die Aufrufe serverseitig sind, erhalten Sie so Informationen zum MVPD-Betrug-Tracking darüber, woher der Benutzer beim AuthZ-Aufruf stammt. | C |                                                                                                                                                         |
| 1.19 | Dynamisch definierter TTL-Wert (Token Persistence Time-to-Live) | Der MVPD kann die TTL des Adobe Pass-Authentifizierungstokens dynamisch über Eigenschaften in der Antwort festlegen, sodass Adobe bei TTL-Änderungen nicht in der Warteschlange ist. | C- |                                                                                                                                                         |
| 1.20 | Gerätetyp | Der MVPD unterstützt die Übergabe des Gerätetyps in der AuthN- oder AuthZ-Anfrage. Diese Eigenschaft informiert den MVPD über die Art des Geräts, auf dem der Inhalt verwendet wird, sodass er die TTL des AuthN- oder AuthZ-Tokens entsprechend seinen eigenen Sicherheitsüberlegungen für das Gerät anpassen kann. | C- | Dies ist für die clientlose Plattform hilfreich, bei der es schwierig sein kann, jeden Gerätetyp als Konfigurationseinstellung auf der Seite &quot;Adobe Pass-Authentifizierung&quot;anzuzeigen. |



## 2. Betriebsfunktionen {#operational-features}

| Nein | Funktion | Beschreibung | Priorität | Hinweise |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2.1 | 24/7 Zeitaufwand | Eine MVPD-Vereinbarung, die eine 24/7-Zeit-Anlaufzeit anstrebt und diese mit der Zeit misst. | A |       |
| 2.2 | Testen der Anmeldeinformationen für jeden Programmierer | Der MVPD stellt für jeden Programmierer separate Testkonten bereit. | A |       |
| 2.3 | Ablaufplan für Zertifikate und Zeitplan für Erneuerung | Ein MVPD-dokumentierter Plan mit einem Zeitplan, um sicherzustellen, dass SAML-Zertifikate aktuell sind. | A- |       |
| 2.4 | Durchschnittliche Latenz unter 250 MS/Antwort | Eine MVPD-Vereinbarung, die auf niedrige Latenzzeiten abzielt und mit der Zeit dagegen zu messen. | A- |       |
| 2.5 | Hosten eines eigenen MVPD-Picker-Logos | Der MVPD muss sein eigenes Logo hosten und sollte durch CDN-Caching geschützt werden. | B |       |
| 2.6 | Support-Eskalationsplan | Ein MVPD-dokumentierter Eskalationsplan, der auf dem neuesten Stand gehalten und mindestens vierteljährlich überprüft wird. | A |       |
| 2.7 | Plan zum Schutz von Krankheitsausbrüchen | Ein MVPD-Co-Plan mit Adobe zu Abbaumaßnahmen, die im Allgemeinen bei Bedarf verwendet werden können. | B |       |
| 2.8 | Separate Staging- und Produktions-Endpunkte verwalten | Ein MVPD-Integrationstest, für den kein Hostdatei-Spoofing erforderlich ist, um die Staging-Integration zu testen. | B |       |
| 2.9 | Zusätzlicher QA-Endpunkt | Der MVPD unterhält eine zusätzliche QS-IdP-Integration für die gemeinsame Entwicklung neuer Funktionen.  Wenn Sie dies unterstützen, ist es weniger wahrscheinlich, dass wir UAT-spezielle Anforderungen für das Testen von Appstore-Zertifikaten benötigen. | C |       |





## 3. Authentifizierungs-Erlebnisfunktionen {#authn-exp-features}


| Nein | Funktion | Beschreibung | Priorität | Hinweise |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3.1 | Authentifizierungskonversion über Minimalerwartungen | Der MVPD gewährleistet eine minimale Konversionsrate, um die Funktionsfähigkeit (5 %) und die Verlässlichkeit (30 %) zu belegen. | A |                                                                                                                                                           |
| 3.2 | Inline-Passwortwiederherstellung | Der MVPD bietet die Möglichkeit, Passwörter inline zum Federated AuthN-Fluss zu übermitteln. | A |                                                                                                                                                           |
| 3.3 | Inline-Kontoregistrierung | Der MVPD bietet die Möglichkeit, ein neues Konto inline zu Federated AuthN Flow zu erstellen. | A |                                                                                                                                                           |
| 3.4 | Inline-Hilfe/Support | Der MVPD bietet eine Möglichkeit, während des Federated AuthN Flusses Hilfe zu leisten. | A |                                                                                                                                                           |
| 3.5 | Modembasierte In-Home-Authentifizierung | Der MVPD authentifiziert ein Gerät automatisch, wenn es sich im lokalen Netzwerk eines registrierten Modells befindet (nur ISP MVPD). | B | Dies hat eine geringere Priorität, da es sich um eine Optimierung handelt, die von vielen noch nicht unterstützt werden kann, und einige Herausforderungen für die Betrugsbekämpfung und die elterliche Kontrolle mit sich bringt |
Sie können jetzt den Markdown-Tabellencode direkt über das Dialogfeld Datei/Tabellendaten einfügen.. importieren.



## 4. Analytics-Funktionen {#analytics-features}


| Nein | Funktion | Beschreibung | Priorität |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4.1 | Ausrichtung des Authentifizierungs-Konversionstrichter | Der MVPD verfügt über eine Metrik für AuthN-Anforderungen, die mit der SAML-AuthN-Anfrage beginnt - um sie an die Adobe Pass-Authentifizierungs- und -AuthN-Anforderungsmetriken für Programmierer anzupassen. | A |
| 4.2 | Unique Users | Benutzer, die erfolgreich authentifiziert und in einer monatlichen Datenaggregation über mehrere Tage hinweg dedupliziert wurden. | A |
| 4.3 | Zeitzonenrechnung | Berichte enthalten Zeitzone für den Tageswechsel. | A |





## 5. Funktionen zur Betrugsbekämpfung {#fraud-mitgn-features}


| Nein | Funktion | Beschreibung | Priorität | Hinweise |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5.1 | Validierung von Videoabonnenten bei Authentifizierung | Der MVPD stellt sicher, dass der Benutzer während des AuthN-Flusses ein gültiger Videoabonnent ist. | A |                                                                                                |
| 5.2 | Ratenbegrenzung für Authentifizierung/Autorisierung | Der MVPD unterstützt Einschränkungen in ihren Webdiensten, um die Nutzung von einem bestimmten Benutzerkonto zu begrenzen, falls dies erforderlich ist. | B |                                                                                                |
| 5.3 | Persistente Benutzer-ID mit globaler Reichweite zum Adobe | Der MVPD stellt sicher, dass Adobe über eine UserID verfügt, die von Programmierern aus auf Betrug hin verfolgt werden kann. Dies muss dem Kunden nicht direkt bereitgestellt werden. | B | Spezifikation des Online Multimedia Authorization Protocol (OMAP) und Echtzeitüberwachung (RUM). |
| 5.4 | Validierung der gleichzeitigen Verwendung | Der MVPD bietet die Möglichkeit, die gleichzeitige Nutzung des Abonnentenkontos über einen Geschäftsschwellenwert hinaus zu verfolgen und zu begrenzen. | B |                                                                                                |

## P1. Proxy-spezifische Funktionen {#proxy-sp-func-features}

| Nein | Beschreibung | Priorität | Hinweise |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| P 1.1 | Proxy, der für die Aufrechterhaltung der Genauigkeit der aktuellen Liste von SubMVPDs verantwortlich ist | A |                                                   |
| P 1.2 | Proxy liefert für jeden subMVPD das entsprechende Logo | A | Einige Live-SubMVPDs haben nicht die richtige Logogröße |
| P 1.3 | Proxy stellt für jeden subMVPD eine passende Branded-Anmeldeseite bereit | A |                                                   |
| P 1.4 | Proxy, der für das Targeting bestimmter Service Provider-Marken mit SubMVPD-Liste verantwortlich ist | B |                                                   |
| P 1.5 | Proxy gibt alle subMVPD-Eigenschaften korrekt an (iFrame-Größe, TTL, Logo usw.) | A |                                                   |
| P 1.6 | Proxy gibt separate entityID an | B |                                                   |

## P2. Proxy-Betriebsfunktionen {#proxy-op-features}

| Nein | Beschreibung | Priorität |
|-------|----------------------------------------------------------------------------------------|----------|
| P 2.1 | API-Schlüssel ist gesichert | A |
| P 2.2 | Testen Sie die Anmeldeinformationen für das erste in der Live-Integration verwendete subMVPD für jeden neuen Anfragenden. | A |
| P 2.3 | subMVPD-Listen sind präzise und vollständig pro Anforderer | A |
