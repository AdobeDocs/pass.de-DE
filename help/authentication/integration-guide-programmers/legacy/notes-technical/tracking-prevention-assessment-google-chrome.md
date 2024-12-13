---
title: Tracking Prevention Assessment Google Chrome
description: Tracking Prevention Assessment Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '673'
ht-degree: 0%

---

# (Legacy) Bewertung der Tracking-Prävention - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Übersicht

In diesem Dokument werden nützliche Ressourcen zusammengefasst und die bevorstehenden Änderungen bewertet, die Google Chrome im Rahmen seiner Initiative zur schrittweisen Abschaffung von Drittanbieter-Cookies plant.

Die Bewertung wird für TV Everywhere (TVE)-Anwendungen durchgeführt, die im Google Chrome-Browser ausgeführt werden und Adobe Pass Access Enabler JavaScript SDK v4 zur Integration mit den Adobe Pass Authentication Backend-Services verwenden.

## Öffentliche Ressourcen

Unten finden Sie eine Liste der Ressourcen, die auf der Google-Entwicklerwebsite und auf dem offiziellen-Blog zusammengefasst sind und die wir unseren Kunden empfehlen, zu konsultieren:

* [Der nächste Schritt zur schrittweisen Abschaffung von Drittanbieter-Cookies in Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Entwicklerdokumentation für Privacy Sandbox](https://developers.google.com/privacy-sandbox)
* [Vorbereiten auf Beschränkungen für Drittanbieter-Cookies](https://developers.google.com/privacy-sandbox/3pcd)
* [Vorbereiten der Einstellung des Drittanbieter-Cookies](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Vorbereitung auf das Ende von Drittanbieter-Cookies](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Third-Party-Cookies, die standardmäßig für 1 % der Chrome-Benutzer eingeschränkt sind](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Timeline

Als kurze Zusammenfassung hat Google Chrome mit dem Testen von [Tracking Protection](https://privacysandbox.com/) begonnen, einer neuen Funktion, die das Site-übergreifende Tracking einschränkt, das sich auf alle Drittanbieter-Cookies auswirkt.

Dies begann Anfang 2024 und betraf etwa 1 % der Nutzer. Der (vorläufige) Plan sieht vor, dies ab dem dritten Quartal 2024 für bis zu 100 % der Nutzer zu verlängern.

## Bewertung

Google hat ein Dokument mit den empfohlenen Playbooks zur Vorbereitung auf die schrittweise Abschaffung von Drittanbieter-Cookies unter folgendem Link veröffentlicht: https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

Wir haben uns an dieses Playbook für die Bewertung von TV Everywhere (TVE)-Anwendungen gehalten, die auf dem Google Chrome-Browser ausgeführt werden und Adobe Pass Access Enabler JavaScript SDK v4 zur Integration mit den Adobe Pass Authentication Backend-Services verwenden.

### Schlussfolgerungen

Auf der Grundlage unserer Tests und der Simulation der bevorstehenden Updates von Google Chrome **die primären TVE-Geschäftsflüsse weiterhin erwartungsgemäß**.

Es ist jedoch wichtig, die umfassendere Strategie von Google zu erkennen, die nicht nur die Einstellung von Drittanbieter-Cookies umfasst, sondern auch die Partitionierung der Speicherung von Drittanbietern.

Infolgedessen treten bei Chrome-Benutzern Störungen mit Single Sign-On (SSO), Single Logout (SLO) und passiven Authentifizierungsfunktionen auf, die separate Anmelde-/Abmeldeaktionen für jede TVE-Anwendung erfordern, die sie verwenden (Abstimmung mit dem aktuellen Erlebnis auf Safari).

## Aufruf zur Selbstbewertung

Wir bitten unsere Kunden dringend, proaktiv ähnliche Bewertungen durchzuführen, um potenzielle Probleme rechtzeitig im Voraus zu identifizieren und sich mit dem überarbeiteten Google Chrome-Benutzererlebnis vertraut zu machen.

Diese Bewertung sollte sowohl Erstanbieter-Services als auch Drittanbieter-Services umfassen, insbesondere in Bezug auf die Integration von Adobe Pass Access Enabler JavaScript SDK v4.

Falls Sie auf Schwierigkeiten im Zusammenhang mit TVE-Geschäftsabläufen stoßen, einschließlich Authentifizierung, Vorautorisierung, Autorisierung, Benutzermetadaten oder Abmeldung, laden wir Sie ein, einen Bericht über ein Zendesk-Ticket bei unserem Kundenunterstützungs-Team einzureichen.

Hilfe bei der Erstellung Ihres Selbstbewertungsplans finden Sie in den folgenden Abschnitten.

### Verwendung von Cookies überprüfen

Ab Chrome 118 werden auf der Registerkarte [DevTools-Probleme](https://developer.chrome.com/docs/devtools/issues/) potenziell betroffene Cookies mit der folgenden Meldung hervorgehoben: `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Cookies, die für die Verwendung durch Dritte markiert sind, können anhand ihres `SameSite=None` Attributwerts identifiziert werden.

Mehr dazu unter diesem Link: https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Bruchprüfung

Um zu testen, ob es zu einer Beschädigung kommt, starten Sie Chrome mithilfe der `--test-third-party-cookie-phaseout`-Befehlszeilenmarkierung oder aktivieren Sie `#test-third-party-cookie-phaseout` in Chrome 118 `chrome://flags/`.

Dadurch wird Google Chrome so eingerichtet, dass Cookies von Drittanbietern blockiert werden und sichergestellt ist, dass zukünftige Funktionen aktiv sind, um den Status nach der schrittweisen Einstellung bestmöglich zu simulieren.

Es lohnt sich, einen tiefen Einblick in die technischen Spezifikationen der folgenden Chrome-Flags zu erhalten:

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Mehr dazu unter diesem Link: https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Andere Browser

### Firefox

Firefox hat vor einigen Jahren einen Rollout ihrer Methode namens `Enhanced Tracking Protection` durchgeführt.

Unten finden Sie einige nützliche Ressourcen von Firefox:

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari hat vor ein paar Jahren einen Rollout seines Mechanismus namens `Intelligent Tracking Prevention`.

Unten finden Sie einige nützliche Ressourcen aus Safari:

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Unten finden Sie einige nützliche Ressourcen aus Adobe Pass:

* [Bewertung der Tracking-Prävention - Apple Safari](tracking-prevention-assessment-apple-safari.md)
