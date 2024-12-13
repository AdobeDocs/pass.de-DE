---
title: Amazon fireTV SSO - Programmer-Kickoff-Handbuch
description: Amazon fireTV SSO - Programmer-Kickoff-Handbuch
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# (Legacy) Amazon fireTV SSO - Anleitung zum Programmer-Start {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>

## Einführung {#intro}

In diesem Dokument werden die Informationen beschrieben, die zum Integrieren des neuen FireTV SDK **der** Adobe Pass-Authentifizierung in Ihre FireTV-Anwendung erforderlich sind. Dieser neue SDK nutzt die Integration auf Betriebssystemebene auf Amazons FireTV-Plattform und bietet somit **Single Sign-On**-Unterstützung. Um von Single Sign-On zu profitieren, ist ein geringer Aufwand Ihrerseits erforderlich, um Ihre Anwendung von der Clientless-API auf die neue FireTV-SDK zu migrieren. Die Authentifizierungsabläufe haben einige Änderungen, die unten beschrieben werden.

## Hochrangige Architektur und Integration auf Betriebssystemebene {#high}

Um Single Sign On zwischen den Anwendungen von TV Everywhere auf der Amazon fireTV-Plattform zu erreichen und das Gesamterlebnis auf dieser Plattform zu verbessern, haben wir uns entschieden, unser Core-SDK auf der Ebene des fireTV-Betriebssystems zu integrieren. Programmierer müssen mit einer Stub-Bibliothek kompilieren, die von Adobe bereitgestellt wird. Die eigentliche Funktionalität wird von der Adobe-Bibliothek bereitgestellt, die sich im FireTV-Betriebssystem von Amazon befindet.

Bis Amazon einen FireTV-Simulator bereitstellt, der unsere Bibliothek auf Betriebssystemebene enthält, ist die Entwicklung nur mit echten FireTV-Geräten möglich.

## Vorteile {#bene}

* Single Sign On zwischen allen Adobe-gestützten TV Everywhere-Anwendungen auf der Amazon fireTV-Plattform mit allen integrierten MVPDs.
* Möglichkeit, von HBA zu profitieren (mit unterstützten MVPDs).
* Möglichkeit zur Verwendung der neuesten FireTV-SDK, ohne dass Ihre Programme bei jeder Veröffentlichung einer neuen SDK-Version aktualisiert werden müssen.
* Alle TVE-Anwendungen profitieren von der Verwendung der freigegebenen Systembibliothek, da keine lokale Kopie der AccessEnabler-Bibliothek mehr erforderlich ist. Dadurch wird auch sichergestellt, dass alle Anwendungen dieselbe SDK-Version verwenden.
* Authentifizierung über einen einzigen Bildschirm - kein Registrierungs-Code und zweite Bildschirm-Workflows erforderlich.

## Migration von einer Client-losen API-basierten App zu einer auf FireTV SDK basierenden App {#migra1}

Für die Migration von der Clientless-API zur FireTV-SDK müssen Sie die Codebasis für die Clientless-API entfernen und die neue FireTV-SDK integrieren.

Verglichen mit der Client-losen API-basierten App wird bei der neuen FireTV SDK die Authentifizierung auf den ersten Bildschirm verschoben, sodass keine zweite Bildschirmauthentifizierung mehr erforderlich ist.

Programmierer müssen dazu eine MVPD-Auswahl in ihren Apps hinzufügen, damit die Nutzer ihren Fernsehanbieter direkt auf dem FireTV-Gerät auswählen können. Nach der Auswahl von MVPD wird dem Benutzer die MVPD-Anmeldeseite auf dem FireTV-Gerät angezeigt.

Wireframes der Benutzerflüsse, die die regulären, HBA- und SSO-Szenarien auf fireTV beschreiben, finden Sie unter [Amazon Fire TV - MVVPD Sign-in User Flow](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migration von der Android SDK-basierten App zur FireTV SDK-basierten App {#migra2}

Dieser neue FireTV SDK ähnelt unserem bestehenden Android SDK und die aktuelle Dokumentation zur **Integration unseres Android SDK** <!--http://tve.helpdocsonline.com/android-technical-overview-->kann verwendet werden, bis wir die FireTV SDK-Dokumente fertig haben. Wenn Sie bereits über Android-Anwendungen verfügen, die unsere Android SDK verwenden, sollte die Integration von fireTV SDK in Ihre fireTV-Anwendung unkompliziert sein.

Im Vergleich zu Android SDK ist der Authentifizierungsprozess in FireTV SDK einfacher zu entwickeln, da die Aufgaben der Verwaltung/Präsentation der MVPD-Anmeldeseite und des Abrufs des AuthN-Tokens intern von der AccessEnabler-Bibliothek ausgeführt werden.

## Häufig gestellte Fragen {#faq}

1. Wie wird die **SSO** funktionieren?

   * SSO funktioniert in allen Programmieranwendungen, die von der Adobe Pass-Authentifizierung unterstützt werden und die die neue FireTV-SDK auf demselben Amazon FireTV-Gerät verwenden
   * SSO zwischen Programmieranwendungen, die in der Clientless-REST-API implementiert sind, und Apps, die in der FireTV-SDK implementiert **werden NICHT unterstützt**

1. Was ist die MVPD-Abdeckung von fireTV SSO?

   * **Alle MVPDs** die über die Adobe Pass-Authentifizierung integriert sind, werden von FireTV SDK technisch unterstützt.

1. Welche anderen **Workflow-Änderungen** sollten Programmierer neben der Verwendung der neuen SDK beachten?

   * Programmierer müssen eine MVPD-Auswahl für die FireTV-Plattform implementieren.

1. Gibt es Änderungen an den **TTLs**?

   * Das Verhalten in Bezug auf Authentifizierungs-TTLs ändert sich nicht.
   * Das erste gültige Authentifizierungstoken wird für die Durchführung des SSO verwendet, und in diesem Fall verwenden alle anderen Anwendungen, die über SSO authentifiziert werden, dieselbe TTL, bis sie abläuft. Wenn Sie also von einem Programm zu einem anderen navigieren, teilt das zweite Programm die TTL des ersten Programms, das sich authentifiziert.

1. Wie funktioniert **Degradation-**?

   * Es sind keine Änderungen für die Beeinträchtigungs-API erforderlich. Das Benutzererlebnis ist dasselbe wie auf Android-Geräten.

1. Wie **TempPass**-Flüsse betroffen?

   * TempPass-Flüsse sind Einzelbildschirm und verhalten sich wie auf anderen nativen Geräten.

1. Funktionieren andere Adobe-Funktionen wie bisher?

   * Alle Adobe Pass-Authentifizierungsfunktionen funktionieren auf FireTV wie auf Android-Geräten.
