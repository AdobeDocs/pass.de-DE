---
title: MVPD Direct-Integrationsplan
description: MVPD Direct-Integrationsplan
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 0%

---

# Schnellstartanleitung für MVPD: Plan für die MVPD-Direktintegration {#mvpd-dir-int-plan}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Einführung {#mvpd-kickstart-intro}

Willkommen bei der Adobe Pass-Authentifizierung für TV Everywhere.  Wir freuen uns auf die Zusammenarbeit mit Ihnen.

>[!NOTE]
>
>Dies ist das Schnellstartanleitung für Multichannel Video Programming Distributors (MVPDs). Wenn Sie ein Programmierer (Inhaltsanbieter) sind, lesen Sie das [Schnellstarthandbuch für Programmierer](/help/authentication/kickstart/programmer-kickstart-guide.md).

Der Support ist jederzeit über das Adobe Pass-Authentifizierungs-Ticketsystem auf Zendesk verfügbar. Hier finden Sie auch Beispiele, Dokumentationen und Video-Tutorials zu unseren Prozessen. Um [Zendesk](https://adobeprimetime.zendesk.com/) nutzen zu können, müssen Sie sich registrieren und ein Konto bei https://tve.zendesk.com/home erstellen. Die Anzahl der Benutzer, die Sie registrieren können und die Kommentare zu einem abgelegten Ticket sehen oder posten können, ist unbegrenzt. Alle Support-Fragen richten Sie bitte an: tve-support unter adobe.com

**Teamkontakte**:

**Support** - Für alle Fragen, Vorfälle oder Funktionsanfragen **tve-support@adobe.com**.

## 1. Auftaktbesprechungen {#kickoff-meetings}

Der Rahmen für diese Treffen ist der Beginn technischer Gespräche zwischen Adobe und MVPD. An dieser Stelle des Prozesses muss die Dokumentation von beiden Seiten gemeinsam genutzt werden. Als Folgemaßnahme muss Adobe ein Ticket in unserem Ticketing-System (https://tve.zendesk.com/) öffnen, um den Status der Integration zu verfolgen.

## 2. Funktionen {#features}

Adobe richtet einen wöchentlichen Statusaufruf ein, um den Zeitplan, die Schritte, den Zeitplan und die Implementierungsdetails der Integration zu besprechen und zu verfolgen. In dieser Phase überprüft Adobe die MVPD-Spezifikationen. Das Ergebnis sollte eine Spezifikationsseite sein, die alle für die MVPD erforderlichen Funktionen detailliert beschreibt. Die MVPD sendet dem Adobe ein Spezifikationsdokument, in dem die Authentifizierungs-/Autorisierungsimplementierung der MVPD detailliert beschrieben wird.

Zu klärende Elemente, siehe [MVPD-Integrationsfunktionen](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md).

Es gibt mehrere Einstellungen, die an dieser Stelle detailliert beschrieben werden müssen:

* **MVPDs Logo-URL** - Dies ist eine Datei mit den folgenden Abmessungen: 112 x 33 Pixel. Das Logo wird von Programmierern auf ihren Sites angezeigt, wenn der Benutzer auf die Schaltfläche „Anmelden“ klickt, um seinen Pay-TV-Anbieter auszuwählen.
* **TTL-Werte (Time-to-Live**: Die TTL wird normalerweise von MVPD während des Authentifizierungs-/Autorisierungsprozesses festgelegt. Adobe kann diese TTL-Werte jedoch überschreiben und unterschiedliche Werte bereitstellen, je nachdem, was sowohl vom Programmierer als auch von MVPD vereinbart wurde.
* **Anzeigename** - Wird von Programmierern auf ihren Sites angezeigt, wenn der Benutzer auf die Schaltfläche „Anmelden“ klickt, um seinen Pay-TV-Anbieter auszuwählen.
* **Testanmeldeinformationen** - Beide Profile (Staging und Produktion) sollten über eine Liste von Testanmeldeinformationen verfügen.

>[!IMPORTANT]
>
>Jedes Mal, wenn ein Benutzer einen Berechtigungsfluss initiiert, wird er mit einer einzigen, undurchsichtigen, eindeutigen Benutzer-ID verknüpft.  Die Benutzer-ID wird verwendet, um den Benutzer einer Programmierer-App zu identifizieren, stammt jedoch von der MVPD.

>[!CAUTION]
>
>Ein wichtiger Hinweis für jede MVPD: Die Benutzer-ID sollte KEINE PII (Personally Identifiable Information) enthalten, d. h. Informationen, die Sie allein oder mit anderen Informationen zur Identifizierung, zum Kontakt oder zum Auffinden des Benutzers verwenden können.

## 2. Austausch von Metadaten {#metadata-ex}

Beide Seiten müssen die Metadaten für alle beteiligten Umgebungen (Produktion, Staging usw.) austauschen.

* **Adobe**
   * Adobe Für die Staging-Umgebung können SP-Metadaten von abgerufen werden: [Authentifizierungs-Staging-SP-Metadaten](https://sp.auth-staging.adobe.com/sp/metadata)
   * Für die Produktionsumgebung können SP-Metadaten von Adobe abgerufen werden unter: [Authentifizierung ProduktionsSP-Metadaten](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * So fügen Sie Metadaten hinzu (Staging/Produktion).

## 4. IP-Zulassungsliste {#allow-ip-list}

Die folgenden IPs sollten in der MVPD-Firewall auf die Whitelist gesetzt werden. Bitte kontaktieren Sie Adobe für die Liste der IPs.

* Die Adobe Pass-Authentifizierung erfordert, dass Firewalls an den Ports 80 und 443 geöffnet werden, um den Zugriff auf eingeschränkte Ressourcen zu ermöglichen.

* Der MVPD muss eine Liste von IP-Adressen für Authentifizierungs- und Autorisierungsserver hinzufügen (falls dies der Fall ist).

## 5. Entwicklung {#deve}

Die Dauer der Entwicklungsphase wird nach Überprüfung der Spezifikationen und unter Berücksichtigung der Punkte, die beide Seiten abzeichnen, festgelegt. Adobe muss benutzerdefinierten Code für den Autorisierungsteil schreiben.

>[!NOTE]
>
>Beachten Sie, dass die Autorisierung pro Ressource durchgeführt wird. Die Autorisierungstransaktion wird in der Regel mit einer ID-Zeichenfolge ausgeführt, die von der Website des Programmierers übergeben wird und den Kanal darstellt, für den der Benutzer die Autorisierung anfordert. Diese Ressourcen-ID wird zwischen dem Programmierer und MVPD festgelegt und kann bei Bedarf detailliert sein.

## 6. Adobe-Umgebungen {#adobe-env}

Adobe bietet verschiedene Umgebungen für verschiedene Phasen des Entwicklungsprozesses:

* **Präqualifikation** (PRE-QUAL): Die PRE-QUAL-Umgebung enthält den nächsten Release-Kandidaten. Adobe integriert zunächst neue Partner in diese Umgebung, bevor die Integration in die Veröffentlichungsumgebung aktualisiert wird. Die Partner haben zwei Wochen Zeit, um die PRE-QUAL-Umgebung zu testen, und müssen alle Änderungen an der PRE-QUAL-Konfiguration explizit anfordern (kontaktieren Sie Ihren Adobe-Support-Mitarbeiter, um weitere Informationen zum Änderungsanforderungsprozess zu erhalten). Fehlerbehebungen für neue Trigger-Bereitstellungen in dieser Umgebung.
* **Version** (VERSION): Der aktuelle Produktions-Build von Adobe wird hier in einer Live-Umgebung bereitgestellt.

Weitere Informationen zur Verwendung von Adobe-Umgebungen finden Sie unter [Grundlagen zu Adobe-Umgebungen](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md)

## 7. Staging-Bereitstellung {#stag-env}

Basierend auf den von der MVPD empfangenen Metadaten erstellt und konfiguriert Adobe im Adobe Pass-Authentifizierungssystem eine neue MVPD. Dieser wird in der PreQual-Staging-Umgebung von Adobe bereitgestellt und mit unserem Testprogrammierer (TestDistributors) konfiguriert.

Die MVPD muss dieselbe Bereitstellung in ihrer QA-/Staging-/Testumgebung durchführen.

## 8. Testen und Fehlerbehebung {#tes-troubleshoot}

In dieser Phase testen und beheben Adobe und MVPD die Integration. Um die Integration zu testen, kann das Adobe Pass-Authentifizierungsteam die Adobe-API-Test-Site verwenden. Weitere Informationen zur Verwendung der Adobe-API-Test-Site finden Sie [Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Adobe-API-Test-Site](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).

Nach erfolgreichem Abschluss der Tests und der Fehlerbehebung wird die Integration in der Staging-Umgebung der Adobe-Version aktiviert. An dieser Stelle kann Adobe die MVPD mit einem eigentlichen Programmierer integrieren.

## 9. Produktionsbereitstellung {#prod-dep}

* MVPD muss zuerst im Produktionsprofil bereitstellen, um die Konnektivität zu testen.

* Adobe wird in der Prequal-Produktion bereitgestellt.

* Alle Parteien können jetzt im Produktionsprofil testen.

* Wenn jetzt alles in Ordnung ist, kann Adobe die Integration in die Release-Produktionsumgebung („Live„) verschieben, die für alle Anwender verfügbar ist.

## 10. Eskalationsverfahren {#esc-proc}

Sobald die Integration in der Produktion verfügbar ist, ist es wichtig, das beste Kundenerlebnis zu bieten. Um im Falle eines Serverausfalls die beste Reaktion zu gewährleisten, müssen MVPDs eine Eskalationsdokumentation für Probleme bereitstellen, die der Adobe zur Kenntnis gebracht werden.

Adobe wiederum stellt MVPDs mit dem aktuellen Eskalationsprozess für die Adobe Pass-Authentifizierung bereit.


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
