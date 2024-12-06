---
title: Tracking Prevention Assessment Google Chrome
description: Tracking Prevention Assessment Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---

# Tracking Prevention Assessment - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht

In diesem Dokument werden nützliche Ressourcen zusammengefasst und die bevorstehenden Änderungen bewertet, die von Google Chrome im Rahmen ihrer Initiative zur schrittweisen Abschaffung von Drittanbieter-Cookies geplant sind.

Die Prüfung wird für TV Anywhere-(TVE-)Anwendungen durchgeführt, die im Google Chrome-Browser ausgeführt werden und die Adobe Pass Access Enabler JavaScript SDK v4 zur Integration in die Adobe Pass Authentication Backend-Dienste verwenden.

## Öffentliche Ressourcen

Unten finden Sie eine Liste der Ressourcen, die von der Entwickler-Website von Google und auch von ihrem offiziellen Blog aggregiert wurden. Wir empfehlen unseren Kunden, Folgendes zu konsultieren:

* [Der nächste Schritt zum Auslaufen von Drittanbieter-Cookies in Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Entwicklerdokumentation für Datenschutz-Sandbox](https://developers.google.com/privacy-sandbox)
* [Vorbereiten auf Beschränkungen für Drittanbieter-Cookies](https://developers.google.com/privacy-sandbox/3pcd)
* [Vorbereiten des Auslaufens des Drittanbieter-Cookies ](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Vorbereiten des Endes von Drittanbieter-Cookies](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Drittanbieter-Cookies sind standardmäßig für 1 % der Chrome-Benutzer eingeschränkt](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Timeline

In einer kurzen Zusammenfassung begann Google Chrome mit dem Testen des [Trackingschutzes](https://privacysandbox.com/), einer neuen Funktion, die das Site-übergreifende Tracking einschränkt, das sich auf alle Drittanbieter-Cookies auswirkt.

Anfang 2024 hatte dies Auswirkungen auf etwa 1 % ihrer Nutzer, und der (vorläufige) Plan besteht darin, diese Zahl ab dem dritten Quartal 2024 auf bis zu 100 % der Nutzer zu verlängern.

## Bewertung

Google hat ein Dokument veröffentlicht, in dem die empfohlene Playbook-Funktion zur Vorbereitung auf die Drittanbieter-Cookies unter folgendem Link zusammengefasst ist: https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

Wir haben dieses Playbook für die Bewertung von TV Anywhere (TVE)-Anwendungen im Google Chrome-Browser befolgt, die Adobe Pass Access Enabler JavaScript SDK v4 zur Integration in die Adobe Pass Authentication Backend-Dienste verwenden.

### Schlussfolgerungen

Basierend auf unseren Tests und der Simulation der anstehenden Aktualisierungen von Google Chrome funktionieren die primären TVE-Geschäftsabläufe **weiterhin erwartungsgemäß**.

Es ist jedoch von entscheidender Bedeutung, die umfassendere Strategie von Google anzuerkennen, die nicht nur die Einstellung von Drittanbieter-Cookies, sondern auch die Partitionierung von Drittanbieterspeichern umfasst.

Daher treten Chrome-Benutzer bei Single Sign-On (SSO), Single Logout (SLO) und passiven Authentifizierungsfunktionen auf, was separate Anmelde-/Abmeldeaktionen für jede verwendete TVE-Anwendung erfordert (Abstimmung mit dem aktuellen Erlebnis in Safari).

## Selbstbewertung

Wir fordern unsere Kunden dringend auf, proaktiv ähnliche Evaluierungen durchzuführen, um potenzielle Probleme rechtzeitig im Voraus zu erkennen und sich mit dem überarbeiteten Google Chrome-Benutzererlebnis vertraut zu machen.

Diese Bewertung sollte sowohl Erstanbieterdienste als auch Drittanbieterdienste umfassen, insbesondere in Bezug auf die Integration des Adobe Pass Access Enabler JavaScript SDK v4.

Sollten Sie Schwierigkeiten im Zusammenhang mit TVE-Geschäftsabläufen haben, wie Authentifizierung, Vorabautorisierung, Autorisierung, Benutzermetadaten oder Abmeldung, laden wir Sie ein, einen Bericht über ein Zendesk-Ticket bei unserem Kundenbetreuungsteam einzureichen.

Hilfe bei der Entwicklung Ihres Selbstbewertungsplans finden Sie in den folgenden Abschnitten.

### Verwendung von Cookies prüfen

Ab Chrome 118 zeigt die Registerkarte [DevTools-Probleme](https://developer.chrome.com/docs/devtools/issues/) potenziell betroffene Cookies mit der folgenden Meldung an: `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Cookies, die für die Verwendung durch Drittanbieter markiert sind, können durch ihren Attributwert `SameSite=None` identifiziert werden.

Klicken Sie auf diesen Link, um mehr zu erfahren: https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Prüfung auf Beschädigung

Um den Ausfall zu testen, starten Sie Chrome entweder mit der Befehlszeilenkennzeichnung `--test-third-party-cookie-phaseout` oder aktivieren Sie in Chrome 118 die Option `#test-third-party-cookie-phaseout` in `chrome://flags/`.

Dadurch wird Google Chrome eingerichtet, um Drittanbieter-Cookies zu blockieren und sicherzustellen, dass zukünftige Funktionen aktiv sind, damit der Status nach dem Auslaufen am besten simuliert werden kann.

Es lohnt sich, einen tiefen Einblick in die technischen Spezifikationen für die folgenden Chrome-Flaggen zu erhalten:

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Klicken Sie auf diesen Link, um mehr zu erfahren: https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Andere Browser

### Firefox

Firefox hatte vor ein paar Jahren ein Rollout ihres Mechanismus namens &quot;`Enhanced Tracking Protection`&quot;.

Siehe unten einige nützliche Ressourcen von Firefox:

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari hatte vor ein paar Jahren einen Rollout ihres Mechanismus namens &quot;`Intelligent Tracking Prevention`&quot;.

Unten finden Sie einige nützliche Ressourcen von Safari:

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Unten finden Sie einige nützliche Ressourcen von Adobe Pass:

* [Tracking Prevention Assessment - Apple Safari](tracking-prevention-assessment-apple-safari.md)
