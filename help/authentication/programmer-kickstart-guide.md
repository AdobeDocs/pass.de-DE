---
title: Handbuch zum Schnellstart für Programmierer
description: Handbuch zum Schnellstart für Programmierer
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 0%

---

# Handbuch zum Schnellstart für Programmierer {#programmer-kickstart-guide}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#prog-kickstart-guide-intro}

Willkommen bei der Adobe Pass-Authentifizierung für TV überall. Wir freuen uns auf die Zusammenarbeit mit Ihnen.

>[!NOTE]
>
>Dies ist das Schnellstartanleitung für Programmierer (Content Provider). Wenn Sie mit einem Multi-Channel-Videoverzeichnungs-Distributor (MVPD) arbeiten, sehen Sie sich bitte das [MVPD-Schnellstarthandbuch](/help/authentication/mvpd-kickstart-guide.md) an.


Adobe Pass-Authentifizierungskontakte:

* Support - für alle Fragen, Vorfälle oder Funktionsanfragen, `tve-support@adobe.com`
* Bis zum Projektstart wird Ihrem Projekt ein Aktivierungskontakt zugewiesen.

Im Folgenden werden einige wichtige erste Schritte beschrieben, die uns zu einem soliden und effizienten Start verhelfen. Das Ziel besteht darin, eine Erläuterung und Erwartung darüber zu liefern, wie wir mit Partnern zusammenarbeiten werden, um Integrationen zu erreichen. Bitte beachten Sie die Abschnitte &quot;Sie werden bereitstellen&quot; / &quot;Adobe wird bereitstellen&quot; für jeden Artikel. Diese werden über eine Checkliste oder einen Leitfaden bei der Arbeit durch das Projekt aufgelistet.

In diesem Dokument wird davon ausgegangen, dass Programmierer für die Zusammenarbeit mit einem ausgewählten MVPD-Partner angemeldet sind.

## Veröffentlichungszeitplan {#release-schedule}

Der Adobe-Sprint-Entwicklungszyklus ist geplant, damit Sie sehen können, wann unsere Versionen geplant sind und wie jede Version durch unser Entwicklungssystem beworben wird.

Nach jedem Sprint ist es für QE verfügbar, oder um neue Implementierungen auf unserem UAT-Server zu sehen.

Etwa alle 6 Wochen wird eine Veröffentlichung auf dem Adobe Pre-Qualification-Server vorgenommen. (Dies ist der Server, auf dem wir unsere vorgeschlagene nächste Version halten, während wir die endgültige QE durchführen.) Diese Builds umfassen alle Arbeiten, die seit dem letzten Tropfen in Sprints abgeschlossen wurden. Partner können diese Version in einem zweiwöchigen QE-Fenster testen.

Wenn keine kritischen Probleme während des vorangegangenen zweiwöchigen Testfensters aufgetreten sind, wird die Version in die Live-Produktion weitergeleitet. Dies bedeutet, dass die Integration in der Adobe-Release-Umgebung verfügbar sein wird, Partner jedoch entscheiden, wann sie die Veröffentlichung vornehmen.

<!--For the latest release schedule information, see the Release Calendar.-->

## Support-Dokumentation {#supp-doc}

Adobe stellt Folgendes bereit:

* Implementierungshandbuch: **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Zugriff auf unser Zendesk Kundenunterstützungssystem. Hier finden Sie auch Beispiele, Informationen und Video-Tutorials zu einigen Prozessen. Um auf Zendesk auf dieses Dokument zugreifen zu können, müssen Sie sich bei `https://tve.zendesk.com/home` registrieren und ein Konto erstellen. Die Anzahl der Benutzer, die Sie registrieren können, ist nicht beschränkt.  Sie können Kommentare zu jedem eingereichten Ticket sehen und teilen. Alle Supportfragen sollten an `tve-support@adobe.com` adressiert werden.
* [Handbuch zur Programmierintegration](/help/authentication/programmer-integration-guide-overview.md)
* Media Token Verifier Library: `https://tve.zendesk.com/entries/471323-media-token-validator-library`.

## Einrichten der Testumgebung {#test-env-setup}

Adobe wird Sie zunächst mit der Adobe-Test-Site einrichten, auf der Adobe zu Testzwecken als MVPD fungiert. Ihr Team kann dann eine Test-Website einrichten, die die Adobe-API aufruft. Verwenden Sie den standardmäßigen MVPD-Selektor und wählen Sie &quot;Adobe&quot;als idP aus.

Sie stellen Folgendes bereit:

1. Anforderer-ID. Dies ist eine Zeichenfolge, die die Marke der Website oder der Anwendung eindeutig identifiziert, die Anforderungen an die Adobe Pass-Authentifizierung sendet. Die Zeichenfolge selbst ist willkürlich, muss aber zwischen der Adobe und dem Programmierer vereinbart werden
1. Kanalinformationen. Hierbei handelt es sich um einen Satz von Zeichenfolgen, die die Inhaltskanäle identifizieren, die von der Anforderer-ID angefordert werden. In vielen Fällen sind der Kanal und die Anforderer-ID identisch. Sie können jedoch über mehrere Inhaltskanäle verfügen, die von derselben ID angefordert werden können. Kanalnamenzeichenfolgen sollten den Kabelfernsehkanälen entsprechen. Einige MVPDs validieren diesen Wert über das AuthN- und/oder AuthZ-Protokoll.
1. Domänennamen (für diese Anforderungs-ID zulässig). Dies ist eine Liste der tatsächlichen Domänennamen, die von Adobe aufgelistet werden, um die Anforderer-ID zu akzeptieren. Dadurch wird sichergestellt, dass nur Ihre genehmigten Domänen mit Ihren Metadaten Zugriff auf die Adobe Pass-Authentifizierung haben. HINWEIS: Für die Produktion gültige Domänennamen können sich für Tests/Staging unterscheiden. Beide Domänennamen sollten angegeben und identifiziert werden.

Adobe richtet das Konto ein und Adobe stellt Folgendes bereit:

* Anmelden und Kennwort für den Zugriff auf die Test-Site

## Einrichtung mit MVPD {#setup-mvpd}

In diesem Abschnitt wird beschrieben, was Sie benötigen, wenn Sie von der Adobe-Test-Site zur Verwendung mit einem MVPD migrieren.

Sie stellen (über MVPD) Folgendes bereit:

* **Zwei Berechtigungssätze**:
   * AuthN + AuthZ : Anmeldung/Passwort für einen authentifizierten und autorisierten Benutzer
   * AuthN + Nicht-AuthZ : Anmeldung/Passwort für einen Benutzer, der authentifiziert, aber nicht autorisiert ist
* **Ressourcen-ID**. Dies ist eine spezifische Inhaltskennung, die mit einem MVPD über das AuthZ-Protokoll validiert wird. Dies kann sich auf der Kanal-, Anzeigen-, Episode- oder Asset-Ebene befinden; es sollte mit Ihrem MVPD vereinbart werden.

Die Adobe Pass-Authentifizierung unterstützt ein MRSS-basiertes Metadatenschema. Das bedeutet, dass Ressourcen-IDs nach Bedarf so spezifisch sein können und Identifikatoren enthalten können, die für ein bestimmtes MVPD eindeutig sein können.

**NEUE MVPD-Integration**: Es ist wichtig, sich daran zu erinnern, dass Ihr ausgewählter MVPD eine wesentliche Rolle beim Abschluss einer Integration spielt. Adobe muss Code für jedes MVPD gemäß ihren Spezifikationen schreiben. Bis diese Schritte abgeschlossen sind, können Sie diesen MVPD nicht im Dialogfeld auswählen oder Ihre Produkttests abschließen. Adobe muss diese Arbeit im Voraus planen, um sie an den nächsten verfügbaren Sprint anzupassen. (Aktuelle Planinformationen finden Sie im Release-Kalender.)

**Vorhandene MVPD-Integrationen**: Wenn Ihr ausgewählter MVPD bereits mit Adobe eingerichtet ist, sollten die Verbindungsschritte wesentlich einfacher (schneller) sein und die Konnektivität kann oft durch Konfigurationsänderungen erreicht werden.

>[!NOTE]
>
>Der MVPD muss weiterhin den Programmierer aktivieren und alle relevanten Geschäftsabschlüsse abzeichnen.

**QE mit MVPDs**: Alle Integrationen erfordern eine gemeinsame QE, und da der Endbenutzer letztendlich Kunde des MVPD ist, haben viele vor dem &quot;Live&quot;-Pushen Testzyklen festgelegt. Da dies die Planung von MVPD-Ressourcen beinhaltet, ist dies ein potenzieller Bereich für Verzögerungen.

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
