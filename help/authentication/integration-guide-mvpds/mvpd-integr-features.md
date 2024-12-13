---
title: MVPD-Integrationsfunktionen
description: MVPD-Integrationsfunktionen
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1733'
ht-degree: 3%

---

# MVPD-Integrationsfunktionen

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#mvpd-int-features-overview}

Die Adobe Pass-Authentifizierung unterstützt die neuen Standards für TV Everywhere. Es entspricht der **CableLabs OLCA (Online Content Access) -Spezifikation** die technische Anforderungen und Architektur für die Bereitstellung von Video aus Online-Quellen an einen Pay-TV-Kunden stellt. Adobe nahm im Juni 2011 am gemeinsamen Interopt-Testprojekt von CableLabs teil und durchlief den Testprozess für eine Service Provider-Implementierung.  Adobe ist auch aktives Mitglied des **OATC (Open Authentication Technical Consortium)** und beteiligt sich als Teil dieses Gremiums an mehreren Spezifikationsentwürfen der Unterausschüsse.

Nachdem die Adobe Pass-Authentifizierung, die OLCA-Konformität und die Teilnahme der Adobe an OATC beschrieben wurde, ist es auch wichtig zu beachten, dass die Adobe Pass-Authentifizierung tatsächlich „protokollunabhängig“ ist.  Aber in dieser Phase der TVE-Ära ist die Adobe Pass-Authentifizierung definitiv auf die OLCA-Standards ausgerichtet.  Die Standards beschreiben derzeit vereinbarte Wege für die verschiedenen TVE-Player (Programmierer, MVPDs und Dienstleister), TVE-Funktionen zu implementieren. Viele dieser Funktionen sind in den folgenden Tabellen aufgeführt, mit Links zu verwandten Seiten, die Details und Beispiele zur Implementierung der Funktionen enthalten.

Die Informationen in den Tabellen sollen den Integrationsprozess der Adobe Pass-Authentifizierung vorantreiben, sodass er über alle MVPD-Integrationen hinweg konsistente Funktionen bietet. Die Spalte Priorität ordnet die Funktionen in A, B und C ein:

* A - Diese Funktionen sind für den ersten Rollout einer Integration „unverzichtbar“.
* B - Dies sind wichtige Verbesserungen der ursprünglichen Integration, die nach dem ersten Rollout hinzugefügt werden.
* C - Dies sind weitere Verbesserungen der Integration, die nach den Anforderungen des „B“ implementiert werden können.


## 1. Hauptfunktionalitäten {#core-func-features}


| Anzahl | Funktion | Beschreibung | Priorität | Notizen |
|------|---------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1,1 | [Vom Programmierer initiierte Anmeldung](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md) | Der Viewer wählt die MVPD aus und initiiert den Authentifizierungsfluss (AuthN) über die Website oder das Programm der Marke Programmierer. | A+ |                                                                                                                                                          |
| 1,2 | [Kanalbasierte Autorisierung](/help/authentication/integration-guide-mvpds/authz-usecase.md) | Nach der Authentifizierung kann die Autorisierung (AuthZ) im Hintergrund erfolgen, wobei nur eine Netzwerkkanal-Kennung und eine Benutzerkennung an die MVPD übergeben werden. | A+ |                                                                                                                                                          |
| 1,3 | UserID-Persistenz | Die MVPD bietet eine verschleierte, persistente Benutzer-ID. | A |                                                                                                                                                          |
| 1,4 | [Unterstützung für Single Sign-on](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-support.md) | Der Betrachter meldet sich mit der MVPD auf der Website einer Marke an und kann dann zu einer anderen Marke wechseln, ohne erneut aufgefordert zu werden. Die AuthN-Sitzung wird von allen Marken gemeinsam genutzt. SSO-Unterstützung ist sowohl für Websites als auch für mobile / Geräte-Anwendungen.  Bei MVPDs ist es erforderlich, entweder eine Benutzer-ID oder ein anderes Benutzer-Token zurückzugeben, das markenübergreifend für AuthZ verwendet werden kann. | A |                                                                                                                                                          |
| 1,5 | Geräteoptimierte Anmeldung - Benutzererlebnis (UX) | MVPD unterstützt die Größenanpassung des Anmeldebildschirms so, dass er den Abmessungen des Geräts entspricht, das die Ansicht verwendet. | A |                                                                                                                                                          |
| 1,6 | Standardlogo für die MVPD-Auswahl | Der MVPD bietet eine URL zu einem Standardlogo mit entsprechenden Abmessungen (112 x 33 Pixel). | A | Das Logo sollte vom MVPD gehostet und vom CDN zwischengespeichert werden. |
| 1,7 | [Service-Provider-Umfang](/help/authentication/integration-guide-mvpds/serv-provider-scoping.md) | MVPD unterstützt die Übergabe der Markenkennung (Wert des Anforderers) in der Authentifizierungsanfrage. | A- | Dies ermöglicht eine für den Dienstleister spezifische Anmeldung. |
| 1,8 | [Mehrkanalautorisierung](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#preflight-multich-authz) | MVPD unterstützt einen Programmierer, der mehrere Kanäle in einer einzigen Autorisierungsanfrage sendet. | B+ |                                                                                                                                                          |
| 1,9 | iFrame- oder „JS Pop-up“-basierte Authentifizierung | Der Programmierer kann den Anmeldefluss in ein iFrame- oder Popup-Erlebnis integrieren, anstatt eine HTTP-Umleitung durchzuführen. | B |                                                                                                                                                          |
| 1,10 | [Programmierinitiierte Abmeldung](/help/authentication/integration-guide-mvpds/usecase-mvpd-logout.md) | Der Viewer verfügt über eine authentifizierte Sitzung sowohl auf der Website oder in der App des Programmierers als auch mit der MVPD. Der Viewer kann auf der Website des Programmierers eine Federated Logout initiieren, und die Abmeldung löscht auch die Sitzung auf dem MVPD-Portal. | B | Stellt sicher, dass freigegebene Computer aus Sicht der Programmierer besser vor Missbrauch geschützt sind. |
| 1,11 | [Benutzerdefinierte Autorisierungsfehlermeldung](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) | Der MVPD übergibt eine eigene benutzerdefinierte Fehlerzeichenfolge, die für die föderierte Site oder App des Programmierers geeignet ist, um sie dem Benutzer anzuzeigen. | B | Ermöglicht Upsell-Szenarien |
| 1,12 | **Benutzermetadaten in der Authentifizierungsantwort** | Die MVPD-AuthN-Antwort kann Benutzermetadaten enthalten, die als Hinweis für die Personalisierung des Benutzererlebnisses während des Berechtigungsflusses dienen. Diese Anforderung ermöglicht elterliche Kontrollhinweise vom MVPD an den Programmierer. | B- |                                                                                                                                                          |
| 1,13 | MVPD-initiierte Authentifizierung | Der Viewer schließt eine erfolgreiche AuthN-Sitzung im MVPD-Portal ab und navigiert dann zur TVE-Site des Programmierers. Der/die Benutzende wird nicht zur MVPD-Auswahl aufgefordert und automatisch authentifiziert. | B- |                                                                                                                                                          |
| 1,14 | UserID Scoping | Die MVPD-Benutzer-ID sollte in zwei Formen vorliegen - eine für Programmierer und die andere für Adobe-weit für Betrug.  Dadurch kann Adobe die MVPD-Benutzer-ID im Programmiererbereich ohne weitere Verschlüsselung/Verschleierung freigeben. | C |                                                                                                                                                          |
| 1,15 | Asset-basierte Autorisierung | Sobald AuthN abgeschlossen ist, kann AuthZ im Hintergrund erfolgen, indem strukturierte Daten übergeben werden, die Netzwerk, Show, Asset, elterliche Kontrolle und mehr nach Bedarf umfassen können. Dies ermöglicht die elterliche Kontrolle bei jedem AuthZ-Aufruf vom Programmierer an MVPD. | C |                                                                                                                                                          |
| 1,16 | MVPD-initiierte Abmeldung | Der Viewer verfügt über eine authentifizierte Sitzung sowohl auf der Website oder in der App des Programmierers als auch mit der MVPD. Der Viewer kann einen Federated Logout von der MVPD-Site initiieren, wodurch auch die Sitzung auf allen Federated Programmer-Sites gelöscht wird. | C |                                                                                                                                                          |
| 1,17 | Zulassungspflichten | Die MVPD bietet zusätzliche Bedingungen in der AuthZ-Antwort, z. B. Protokollierung oder aktualisierte maximale Kindersicherungsbewertung (OLCA). | C |                                                                                                                                                          |
| 1,18 | IP-Adresskontext | MVPD erfordert die explizite Übergabe der IP-Adresse. Bei AuthZ, wo die Aufrufe Server-seitig sind, erhält die MVPD Informationen zur Betrugs-Verfolgung, wo der Benutzer beim AuthZ-Aufruf herkommt. | C |                                                                                                                                                          |
| 1,19 | Dynamisch definierter TTL-Wert (Time-to-Live) für Token-Persistenz | MVPD kann die TTL des Adobe Pass-Authentifizierungstokens dynamisch über die Eigenschaften in der Antwort festlegen, sodass Adobe für TTL-Änderungen nicht in der Schleife ist. | C- |                                                                                                                                                          |
| 1,20 | Gerätetyp | MVPD unterstützt die Übergabe des Gerätetyps in der AuthN- oder AuthZ-Anfrage. Diese Eigenschaft informiert die MVPD über die Art des Geräts, auf dem die Inhalte genutzt werden. Daher können sie die TTL des AuthN- oder AuthZ-Tokens an ihre eigenen Sicherheitsüberlegungen für das Gerät anpassen. | C- | Dies ist für die Client-lose Plattform hilfreich, bei der es schwierig sein kann, jeden Gerätetyp als Konfigurationseinstellung auf der Seite &quot;Adobe Pass-Authentifizierung“ anzuzeigen. |



## 2. Operative Merkmale {#operational-features}

| Nein | Funktion | Beschreibung | Priorität | Notizen |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2,1 | 24/7 Betriebszeit | Ein MVPD-Abkommen, um die 24/7-Laufzeit zu erreichen und im Laufe der Zeit dagegen vorzugehen. | A |       |
| 2,2 | Testen der Anmeldeinformationen für jeden Programmierer | MVPD bietet für jeden Programmierer separate Testkonten. | A |       |
| 2,3 | Plan für Zertifikatablauf und -erneuerung | Ein von MVPD dokumentierter Plan mit einem Zeitplan, um sicherzustellen, dass SAML-Zertifikate auf dem neuesten Stand sind. | A- |       |
| 2,4 | Durchschnittliche Latenz unter 250 ms / Antwort | Eine MVPD-Vereinbarung, um eine niedrige Latenz anzustreben und mit der Zeit daran zu messen. | A- |       |
| 2,5 | Hosten eines eigenen MVPD-Auswahllogos | MVPD muss ein eigenes Logo hosten und sollte durch CDN-Caching geschützt sein. | B |       |
| 2,6 | Support-Eskalationsplan | Ein von MVPD dokumentierter Eskalationsplan, der auf dem neuesten Stand gehalten und mindestens vierteljährlich überprüft wird. | A |       |
| 2,7 | Ausfallsicherungsplan | Ein MVPD-Co-Plan mit Adobe über Abbaumaßnahmen, die bei Bedarf allgemein angewendet werden können. | B |       |
| 2,8 | separate Staging- und Produktions-Endpunkte verwalten | Ein MVPD-Integrationstest, der kein Hostdatei-Spoofing zum Testen der Staging-Integration erfordert. | B |       |
| 2,9 | Zusätzlicher QA-Endpunkt | Der MVPD verwaltet eine zusätzliche QA-IDp-Integration für die gemeinsame Entwicklung neuer Funktionen.  Wenn wir dies unterstützen, wäre es unwahrscheinlicher, dass wir spezielle UAT-Anfragen für App Store-Zertifikatstests benötigen. | C |       |





## 3. Authentifizierungserlebnis-Funktionen {#authn-exp-features}


| Nein | Funktion | Beschreibung | Priorität | Notizen |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3,1 | Konversion der Authentifizierung über die Mindestvoraussetzung | Der MVPD stellt eine minimale Konversionsrate sicher, um sich als funktionsfähig (5 %) und angemessen (30 %) zu erweisen. | A |                                                                                                                                                           |
| 3,2 | Inline-Kennwortwiederherstellung | MVPD bietet eine Möglichkeit, Kennwörter inline im Federated AuthN-Fluss wiederherzustellen. | A |                                                                                                                                                           |
| 3,3 | Inline-Kontoregistrierung | MVPD bietet die Möglichkeit, ein neues Konto inline zum Federated AuthN-Fluss zu erstellen. | A |                                                                                                                                                           |
| 3,4 | Inline-Hilfe/Support | Die MVPD bietet eine Möglichkeit, während des zusammengeführten AuthN-Flusses Hilfe zu bieten. | A |                                                                                                                                                           |
| 3,5 | Modembasierte Authentifizierung zu Hause | MVPD authentifiziert ein Gerät automatisch, wenn es sich im lokalen Netzwerk eines registrierten Modells befindet (nur ISP MVPD). | B | Dies hat niedrigere Priorität, da es sich um eine Optimierung handelt, die viele noch nicht unterstützen können, und stellt einige Herausforderungen für die Betrugsbekämpfung und die elterliche Kontrolle dar |

Sie können jetzt Markdown-Tabellen-Code direkt mit dem Dialogfeld „Tabellendaten einfügen…“ importieren.


## 4. Analytics-Funktionen {#analytics-features}


| Nein | Funktion | Beschreibung | Priorität |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4,1 | Ausrichtung des Authentifizierungs-Konversionstrichters | Der MVPD verfügt über eine Metrik für AuthN-Anfragen, die mit der SAML-AuthN-Anfrage beginnt - zur Abstimmung mit der Metrik für Adobe Pass-Authentifizierung und Programmierer-AuthN-Anfragen . | A |
| 4,2 | Unique Users | Benutzer, die erfolgreich authentifiziert und in einer monatlichen Datenaggregation über Tage hinweg dedupliziert wurden. | A |
| 4,3 | Zeitzonenbuchhaltung | Berichte enthalten die Zeitzone für den Umschwung des Tages. | A |





## 5. Betrugsbekämpfungsfunktionen {#fraud-mitgn-features}


| Nein | Funktion | Beschreibung | Priorität | Notizen |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5,1 | Validierung von Videoabonnenten bei der Authentifizierung | Der MVPD stellt sicher, dass der Benutzer während des AuthN-Flusses ein gültiger Videoabonnent ist. | A |                                                                                                |
| 5,2 | Ratenbegrenzung für Authentifizierung/Autorisierung | Die MVPD unterstützt Einschränkungen bei ihren Web-Services, um die Nutzung eines bestimmten Benutzerkontos bei Bedarf zu beschränken. | B |                                                                                                |
| 5,3 | Persistente Benutzer-ID mit globalem Gültigkeitsbereich für Adobe | Der MVPD stellt sicher, dass Adobe über eine Benutzer-ID verfügt, die über Programmierer hinweg zur Betrugsprüfung verfolgt werden kann. Dies muss dem Kunden nicht direkt bereitgestellt werden. | B | OMAP-Spezifikation (Online Multimedia Authorization Protocol) und Real User Monitoring (RUM). |
| 5,4 | Validierung gleichzeitiger Nutzung | Die MVPD verfügt über eine Möglichkeit, die gleichzeitige Nutzung des Abonnentenkontos über einen geschäftlichen Schwellenwert hinaus zu verfolgen und zu begrenzen. | B |                                                                                                |

## P1. Proxy-spezifische Funktionsmerkmale {#proxy-sp-func-features}

| Nein | Beschreibung | Priorität | Notizen |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| S. 1.1 | Proxy, der für die Aufrechterhaltung der Genauigkeit der aktuellen Liste von SubMVPDs verantwortlich ist | A |                                                   |
| S. 1.2 | Proxy liefert passendes Logo für jedes SubMVPD | A | Einige Live-Sub-MVPDs haben nicht die richtige Logogröße |
| S. 1.3 | Proxy stellt für jedes subMVPD eine passende Anmeldeseite mit Branding bereit | A |                                                   |
| S. 1.4 | Proxy, der für das Targeting bestimmter Dienstleister-Marken mit SubMVPD-Liste verantwortlich ist | B |                                                   |
| S. 1.5 | Proxy legt alle SubMVPD-Eigenschaften korrekt fest (iFrame-Größe, TTL, Logo usw.) | A |                                                   |
| S. 1.6 | Proxy gibt separate entityID an | B |                                                   |

## P2. Proxy-Betriebsfunktionen {#proxy-op-features}

| Nein | Beschreibung | Priorität |
|-------|----------------------------------------------------------------------------------------|----------|
| S. 2.1 | API-Schlüssel ist gesichert | A |
| S. 2.2 | Testen der Anmeldeinformationen für das erste in der Live-Integration verwendete Sub-MVPD für jeden neuen Anforderer | A |
| S. 2.3 | subMVPD-Listen sind genau und vollständig pro Anfragenden | A |
