---
title: MVPD Direct Integration Plan
description: MVPD Direct Integration Plan
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# MVPD-Schnellstartanleitung: MVPD-Direkt-Integrationsplan {#mvpd-dir-int-plan}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#mvpd-kickstart-intro}

Willkommen bei der Adobe Pass-Authentifizierung für TV überall.  Wir freuen uns auf die Zusammenarbeit mit Ihnen.

>[!NOTE]
>
>Dies ist die Schnellstartanleitung für Multichannel Video Programming Distributors (MVPDs). Wenn Sie ein Programmierer sind (Inhaltsanbieter), lesen Sie den Abschnitt [Schnellstartanleitung für Programmierer](/help/authentication/programmer-kickstart-guide.md).

Der Support ist jederzeit über das Ticketsystem Adobe Pass-Authentifizierung auf Zendesk verfügbar. Hier finden Sie auch Beispiele, Dokumentationen und Videoschulungen zu unseren Prozessen. Zur Verwendung [Zendesk](https://adobeprimetime.zendesk.com/)müssen Sie sich registrieren und ein Konto unter https://tve.zendesk.com/home erstellen. Es gibt keine Beschränkung für die Anzahl der Benutzer, die Sie registrieren können und die Kommentare zu einem ausgefüllten Ticket sehen oder posten können. Alle Support-Fragen sollten an: tve-support unter adobe.com gerichtet werden.

**Teamkontakte**:

**Support** - Bei allen Fragen, Vorfällen oder Funktionsanfragen **tve-support@adobe.com**.

## 1. Kick-Off-Sitzungen {#kickoff-meetings}

Diese Treffen sind der Beginn technischer Diskussionen zwischen der Adobe und dem MVPD. An dieser Stelle des Prozesses muss die Dokumentation von beiden Seiten freigegeben werden. Anschließend muss Adobe ein Ticket in unserem Ticketsystem (https://tve.zendesk.com/) öffnen, um den Integrationsstatus zu verfolgen.

## 2. Funktionen {#features}

Adobe richtet einen wöchentlichen Statusaufruf ein, um den Gesamtplan, die Schritte, den Zeitplan und die Implementierungsdetails der Integration zu besprechen und zu verfolgen. In dieser Phase überprüft Adobe die Spezifikationen des MVPD. Das Ergebnis sollte eine spezifische Seite sein, auf der alle vom MVPD benötigten Funktionen beschrieben werden. Der MVPD sendet der Adobe ein Spezifikationsdokument, in dem die Authentifizierungs-/Autorisierungsimplementierung des MVPD detailliert beschrieben wird.

Zu klärende Punkte finden Sie unter [MVPD-Integrationsfunktionen](/help/authentication/mvpd-integr-features.md).

Es gibt verschiedene Einstellungen, die zu diesem Zeitpunkt detailliert beschrieben werden müssen:

* **Logo-URL von MVPD** - Dies ist eine Datei mit den folgenden Abmessungen: 112 x 33 Pixel. Das Logo wird von Programmierern auf ihren Websites angezeigt, wenn der Benutzer auf die Schaltfläche &quot;Anmelden&quot;klickt, um seinen Pay TV-Anbieter auszuwählen.
* **TTL-Werte (Time-to-Live)** - Die TTL wird normalerweise vom MVPD während des Authentifizierungs-/Autorisierungsprozesses festgelegt. Adobe kann diese TTL-Werte jedoch außer Kraft setzen und je nach Vereinbarung des Programmierers und des MVPD unterschiedliche Werte bereitstellen.
* **Anzeigename** - Dies wird von Programmierern auf ihren Websites angezeigt, wenn der Benutzer auf die Schaltfläche &quot;Anmelden&quot;klickt, um seinen Pay TV-Anbieter auszuwählen.
* **Testen von Anmeldeinformationen** - Beide Profile (Staging und Produktion) sollten über eine Liste mit Testberechtigungen verfügen.

>[!IMPORTANT]
>
>Jedes Mal, wenn ein Benutzer einen Berechtigungsfluss initiiert, wird er einer einzelnen, undurchsichtigen, eindeutigen Benutzer-ID zugeordnet.  Die Benutzer-ID wird verwendet, um den Benutzer der App eines Programmierers zu identifizieren, stammt jedoch aus dem MVPD.

>[!CAUTION]
>
>Ein wichtiger Hinweis für jeden MVPD: Die Benutzer-ID sollte KEINE personenbezogenen Daten (PII), Informationen, die allein oder mit anderen Informationen verwendet werden können, um den Benutzer zu identifizieren, zu kontaktieren oder zu finden, enthalten.

## 2. Metadatenaustausch {#metadata-ex}

Beide Seiten müssen die Metadaten für alle beteiligten Umgebungen (Produktion, Staging usw.) austauschen.

* **Adobe**
   * Für die Staging-Umgebung können Adobe-SP-Metadaten abgerufen werden von: [Authentifizierungs-Staging-sp-Metadaten](https://sp.auth-staging.adobe.com/sp/metadata)
   * Für die Produktionsumgebung können Adobe-SP-Metadaten abgerufen werden von: [Authentifizierungs-Produktions-sp-Metadaten](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * Hinzufügen von Metadaten (Staging/Produktion).

## 4. IP-Liste zulassen {#allow-ip-list}

Die folgenden IPs sollten in der Firewall des MVPD auf die Whitelist gesetzt werden. Kontaktieren Sie Adobe für die Liste der IPs.

* Für die Adobe Pass-Authentifizierung müssen Firewalls an den Ports 80 und 443 geöffnet werden, um den Zugriff auf eingeschränkte Ressourcen zu ermöglichen.

* Der MVPD muss eine Liste von IP-Adressen für Authentifizierungs- und Autorisierungsserver hinzufügen (falls dies der Fall ist).

## 5. Entwicklung {#deve}

Die Dauer der Entwicklungsphase wird nach Überprüfung der Spezifikationen und unter Berücksichtigung der Elemente bestimmt, die beide Seiten signalisieren. Adobe muss benutzerdefinierten Code für den Autorisierungsteil schreiben.

>[!NOTE]
>
>Beachten Sie, dass die Autorisierung pro Ressource erfolgt. Die Autorisierung wird normalerweise mit einer ID-Zeichenfolge ausgeführt, die von der Website des Programmierers übergeben wird und den Kanal darstellt, für den der Benutzer eine Autorisierung anfordert. Diese Ressourcen-ID wird zwischen dem Programmierer und MVPD festgelegt und kann so detailliert wie nötig sein.

## 6. Adobe-Umgebungen {#adobe-env}

Adobe bietet verschiedene Umgebungen für verschiedene Phasen des Entwicklungsprozesses:

* **Vorqualifizierung** (PRE-QUAL): Die Umgebung PRE-QUAL enthält den Kandidaten für die nächste Version. Adobe integriert zunächst neue Partner in diese Umgebung, bevor die Integration in die Versionsumgebung aktualisiert wird. Partner haben zwei Wochen Zeit, um die PRE-QUAL-Umgebung zu testen, und müssen explizit Änderungen an der PRE-QUAL-Konfiguration anfordern (kontaktieren Sie Ihren Adobe-Support-Mitarbeiter, um Einzelheiten zum Änderungsanfrageprozess zu erhalten). Fehlerkorrektur - In dieser Umgebung werden neue Trigger-Implementierungen behoben.
* **Version** (RELEASE): Der aktuelle Produktionsaufbau wird hier in einer Live-Umgebung bereitgestellt.

Weitere Informationen zur Verwendung von Adobe-Umgebungen finden Sie unter [Grundlagen zu Adobe-Umgebungen](/help/authentication/understanding-the-adobe-environments.md)

## 7. Staging-Bereitstellung {#stag-env}

Basierend auf den Metadaten, die vom MVPD empfangen wurden, erstellt und konfiguriert Adobe ein neues MVPD im Adobe Pass-Authentifizierungssystem. Diese wird in der Adobe Prequal Staging-Umgebung bereitgestellt und mit unserem Testprogrammierer (TestDistributors) konfiguriert.

Der MVPD muss dieselbe Implementierung in seiner QA-/Staging-/Testumgebung durchführen.

## 8. Testen und Fehlerbehebung {#tes-troubleshoot}

In dieser Phase führen Adobe und der MVPD-Test durch und führen eine Fehlerbehebung für die Integration durch. Um die Integration zu testen, kann das Adobe Pass-Authentifizierungsteam die Adobe API-Test-Site verwenden. Weitere Informationen zur Verwendung der Adobe API-Test-Site finden Sie unter [Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Adobe API-Test-Site](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).

Wenn die Tests und Fehlerbehebung erfolgreich abgeschlossen wurden, ist die Integration in der Adobe Release-Staging-Umgebung aktiviert. An dieser Stelle kann Adobe das MVPD mit einem tatsächlichen Programmierer integrieren.

## 9. Produktionsbereitstellung {#prod-dep}

* Der MVPD muss zunächst im Produktionsprofil bereitgestellt werden, um die Konnektivität zu testen.

* Adobe wird in der Produktion bereitgestellt.

* Alle Parteien können nun im Produktionsprofil testen.

* Wenn an dieser Stelle alles in Ordnung ist, kann Adobe die Integration in die Release-Produktionsumgebung (&quot;live&quot;) verschieben, die für alle Benutzer verfügbar ist.

## 10. Eskalationsverfahren {#esc-proc}

Sobald die Integration in Produktion ist, ist es wichtig, das beste Kundenerlebnis zu bieten. Um im Falle eines Serverausfalls die beste Reaktion sicherzustellen, müssen MVPDs eine Eskalationsdokumentation für Probleme bereitstellen, die mit der Aufmerksamkeit der Adobe in Verbindung gebracht werden.

Adobe stellt wiederum MVPDs mit dem aktuellsten Adobe Pass-Authentifizierungs-Eskalationsprozess bereit.


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
