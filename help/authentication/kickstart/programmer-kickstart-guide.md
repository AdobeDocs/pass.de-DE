---
title: Anleitung für Programmierer
description: Anleitung für Programmierer
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 0%

---

# Anleitung für Programmierer {#programmer-kickstart-guide}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Einführung {#prog-kickstart-guide-intro}

Willkommen bei der Adobe Pass-Authentifizierung für TV Everywhere. Wir freuen uns auf die Zusammenarbeit mit Ihnen.

>[!NOTE]
>
>Dies ist das Kickstart-Handbuch für Programmierer (Inhaltsanbieter). Wenn Sie mit einem Multi-Channel-Videoprogrammierungs-Distributor (MVPD) arbeiten, lesen Sie bitte das [MVPD-Kickstart-Handbuch](/help/authentication/kickstart/mvpd-kickstart-guide.md).


Adobe Pass-Authentifizierungskontakte:

* Support - für alle Fragen, Vorfälle oder Funktionsanfragen, `tve-support@adobe.com`
* Zum Zeitpunkt des Projektstarts wird Ihrem Projekt ein Kontakt zur Aktivierung zugewiesen.

Die folgenden Informationen beschreiben einige wichtige erste Schritte, um einen soliden und effizienten Start zu ermöglichen. Ziel ist es, eine Erklärung und Erwartung darüber zu liefern, wie wir mit Partnern zusammenarbeiten werden, um Integrationen zu erreichen. Bitte beachten Sie für jeden Artikel die folgenden Abschnitte „Sie werden bereitstellen“ / &quot;Adobe wird bereitstellen“. Diese werden während der Bearbeitung des Projekts als Checkliste oder Leitfaden aufgeführt.

In diesem Dokument wird davon ausgegangen, dass Programmierer angemeldet sind, um mit einem ausgewählten MVPD-Partner zu arbeiten.

## Veröffentlichungszeitplan {#release-schedule}

Der Entwicklungszyklus des Adobe-Sprints ist so geplant, dass Sie sehen können, wann unsere Versionen geplant sind und wie jede Version durch unser Entwicklungssystem beworben wird.

Nach jedem Sprint ist er für QE verfügbar oder um neue Implementierungen auf unserem UAT-Server zu sehen.

Etwa alle 6 Wochen wird eine Veröffentlichung auf dem Adobe-Vorbequalifizierungsserver vorgenommen. (Dies ist der Server, auf dem wir unsere vorgeschlagene nächste Version halten, während wir die endgültige QE durchführen.) Diese Builds enthalten alle seit dem letzten Abwurf in Sprints abgeschlossenen Arbeiten. Zurzeit steht den Partnern ein zweiwöchiges QE-Fenster zur Verfügung, in dem sie diese Version testen können.

Wenn im vorausgegangenen zweiwöchigen Testfenster keine kritischen Probleme aufgetreten sind, wird die Version zur Live-Produktion weitergeleitet. Das bedeutet, dass die Integration in der Adobe-Veröffentlichungsumgebung verfügbar ist, die Partner jedoch selbst entscheiden, wann sie die Veröffentlichung durchführen.

<!--For the latest release schedule information, see the Release Calendar.-->

## Support-Dokumentation {#supp-doc}

Adobe bietet:

* Bereitstellungshandbuch: **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Zugriff auf unser Zendesk-Kundensupportsystem. Hier finden Sie auch Beispiele, Informationen und Video-Tutorials zu einigen der Prozesse. Um auf dieses Dokument auf Zendesk und andere dort veröffentlichte Dokumente zugreifen zu können, müssen Sie sich bei `https://tve.zendesk.com/home` registrieren und ein Konto erstellen. Die Anzahl der Benutzer, die Sie registrieren können, ist unbegrenzt.  Sie können Kommentare zu jedem abgelegten Ticket anzeigen und freigeben. Alle Support-Fragen sollten an `tve-support@adobe.com` gerichtet werden.
* [Integrationshandbuch für Programmierer](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)
* Media Token Verifier-Bibliothek: `https://tve.zendesk.com/entries/471323-media-token-validator-library`.

## Einrichtung der Testumgebung {#test-env-setup}

Adobe richtet Sie zunächst auf der Adobe-Test-Site ein, wo Adobe zu Testzwecken als MVPD fungiert. Ihr Team kann dann eine Test-Website einrichten, die die Adobe-API aufruft. Verwenden Sie den standardmäßigen MVPD-Selektor und wählen Sie als ID &quot;Adobe&quot; aus.

Sie geben Folgendes an:

1. Antragsteller-ID. Dies ist eine Zeichenfolge, die die Marke der Website oder der Anwendung, die Anfragen an die Adobe Pass-Authentifizierung sendet, eindeutig identifiziert. Die Zeichenkette selbst ist willkürlich, muss aber zwischen Adobe und dem Programmierer vereinbart werden
1. Kanalinformationen. Dies ist ein Satz von Zeichenfolgen, der die Inhaltskanäle identifiziert, die von der Anforderer-ID angefordert werden. In vielen Fällen sind der Kanal und die Anforderer-ID identisch. Sie können jedoch über mehrere Inhaltskanäle verfügen, die von derselben ID angefordert werden können. Die Zeichenfolge des Kanalnamens sollte mit den Kabelfernsehkanälen übereinstimmen. Einige MVPDs überprüfen diesen Wert über das AuthN- und/oder AuthZ-Protokoll.
1. Domain-Namen (für diese Anforderer-ID zulässig). Dies ist eine Liste der tatsächlichen Domain-Namen, die von Adobe aufgelistet werden, um die Anforderer-ID zu akzeptieren. Dadurch wird sichergestellt, dass nur Ihre genehmigten Domains mit Ihren Metadaten Zugriff auf die Adobe Pass-Authentifizierung haben. HINWEIS: Die für die Produktion gültigen Domain-Namen können für Tests/Staging unterschiedlich sein und sollten angegeben und identifiziert werden.

Adobe richtet das Konto ein und Adobe stellt Folgendes bereit:

* Anmelden und Kennwort für den Zugriff auf die Test-Site

## Einrichten mit MVPD {#setup-mvpd}

In diesem Abschnitt wird beschrieben, was Sie benötigen, wenn Sie von der Adobe-Test-Site zu einer MVPD migrieren.

Sie geben Folgendes an (über MVPD):

* **Zwei Sätze von Anmeldeinformationen**:
   * AuthN + AuthZ : Anmelden/Kennwort eines Benutzers, der authentifiziert und autorisiert ist
   * AuthN + Nicht-AuthZ : Anmelden/Kennwort eines Benutzers, der authentifiziert, aber nicht autorisiert ist
* **Ressourcen-ID**. Dies ist eine spezifische Inhaltskennung, die mit einem MVPD über das AuthZ-Protokoll validiert wird. Dies kann auf Kanal-, Show-, Folge- oder Asset-Ebene erfolgen; es sollte mit Ihrem MVPD abgestimmt werden.

Die Adobe Pass-Authentifizierung unterstützt ein MRSS-basiertes Metadatenschema, d. h. Ressourcen-IDs können bei Bedarf spezifisch sein und Kennungen enthalten, die für eine bestimmte MVPD eindeutig sein können.

**NEW MVPD-Integration**: Beachten Sie, dass die von Ihnen gewählte MVPD bei jeder Integration eine wesentliche Rolle spielt. Adobe muss Code für jede MVPD gemäß ihren Spezifikationen schreiben. Solange diese Schritte nicht abgeschlossen sind, können Sie diese MVPD nicht aus dem Dialogfeld auswählen oder Ihre Produkttests abschließen. Adobe muss diese Arbeit im Voraus planen, damit sie zum nächsten verfügbaren Sprint passt. (Aktuelle Zeitplaninformationen finden Sie im Veröffentlichungskalender.)

**Bestehende MVPD-Integrationen**: Wenn Ihre ausgewählte MVPD bereits mit Adobe eingerichtet ist, sollten die Konnektivitätsschritte viel einfacher (schneller) sein und häufig kann die Konnektivität durch Konfigurationsänderungen erreicht werden.

>[!NOTE]
>
>Die MVPD muss weiterhin den Programmierer aktivieren und alle relevanten Geschäftsabschlüsse abzeichnen.

**QE mit MVPDs**: Alle Integrationen beinhalten gemeinsame QE, und da der Endanwender letztendlich ein Kunde der MVPD ist, haben viele Testzyklen festgelegt, bevor sie „live“ pushen. Da dies die Planung von MVPD-Ressourcen beinhaltet, ist dies ein potenzielles Verzugsgebiet.

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
