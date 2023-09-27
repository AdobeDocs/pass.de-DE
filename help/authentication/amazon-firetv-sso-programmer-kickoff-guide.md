---
title: Amazon fireTV SSO - Leitfaden zum Programmstart
description: Amazon fireTV SSO - Leitfaden zum Programmstart
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---

# Amazon fireTV SSO - Leitfaden zum Programmstart {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Einführung {#intro}

In diesem Dokument werden die für die Integration der neuen **Adobe Pass Authentication&#39;s fireTV SDK** in der Anwendung &quot;fireTV&quot;. Dieses neue SDK nutzt die Integration auf Betriebssystemebene auf der Amazon-BrandTV-Plattform und bietet so **Single Sign-On** unterstützen. Um Single Sign-On zu nutzen, ist von Ihrer Seite ein wenig Aufwand erforderlich, um Ihre Anwendung von der Client-losen API in das neue fireTV-SDK zu migrieren. Es gibt einige Änderungen in den Authentifizierungsflüssen, die nachfolgend beschrieben werden.

## Integration auf hoher Ebene in Architektur und Betriebssysteme {#high}

Um Single-Sign-On zwischen TV-Programmen überall auf der Amazon-BrandTV-Plattform zu erreichen und um das Gesamterlebnis auf dieser Plattform zu verbessern, haben wir beschlossen, unser Core-SDK auf der Ebene von fireTV OS zu integrieren. Programmierer müssen mit einer von Adobe bereitgestellten Stub-Bibliothek kompilieren. Die tatsächliche Funktionalität wird von der Adobe-Bibliothek bereitgestellt, die im BrandTV-Betriebssystem von Amazon vorhanden ist.

Solange Amazon nicht über einen &quot;fireTV&quot;-Simulator verfügt, der unsere Bibliothek auf Betriebssystemebene integriert, wäre die Entwicklung nur mit echten &quot;fireTV&quot;-Geräten möglich.

## Vorteile {#bene}

* Single-Sign-On zwischen allen Adobe-TV-Anwendungen auf der Amazon-BrandTV-Plattform mit allen integrierten MVPDs.
* Möglichkeit, von HBA (mit unterstützten MVPDs) zu profitieren.
* Möglichkeit, das neueste fireTV-SDK zu verwenden, ohne dass Ihre Anwendungen jedes Mal aktualisiert werden müssen, wenn eine neue SDK-Version veröffentlicht wird.
* Alle TVE-Apps profitieren von der Verwendung der freigegebenen Systembibliothek, da keine lokale Kopie der AccessEnabler-Bibliothek erforderlich ist. Dadurch wird auch sichergestellt, dass alle Anwendungen dieselbe SDK-Version verwenden.
* Authentifizierung mit einem Bildschirm - kein Registrierungs-Code und Workflows mit dem zweiten Bildschirm erforderlich.

## Migration von einer clientless API-basierten App zu einer App, die auf dem TV SDK basiert {#migra1}

Für die Migration von der ClientLess-API zum fireTV-SDK müssen Sie die Codebase entfernen, die mit der ClientLess-API verknüpft ist, und das neue fireTV-SDK integrieren.

Im Vergleich zur clientless-API-basierten App ist beim neuen fireTV-SDK die Authentifizierung beim ersten Bildschirm nicht mehr erforderlich, da eine zweite Bildschirmanauthentifizierung nicht mehr erforderlich ist.

Dazu müssen Programmierer einen MVPD-Picker in ihre Apps einfügen, damit Benutzer ihren TV-Anbieter direkt auf dem fireTV-Gerät auswählen können. Bei Auswahl des MVPD wird dem Benutzer die MVPD-Anmeldeseite auf dem Gerät fireTV angezeigt.

Wireframes der Benutzerflüsse, die die regulären, HBA- und SSO-Szenarien bei fireTV beschreiben, finden Sie unter [Amazon Fire TV - MVVPD-Anmeldungs-Benutzerfluss](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migration von einer Android SDK-basierten App zu einer App, die auf dem SDK basiert {#migra2}

Dieses neue fireTV-SDK ähnelt unserem vorhandenen Android-SDK und der aktuellen Dokumentation, für die wir arbeiten **Integrieren unseres Android-SDK** <!--http://tve.helpdocsonline.com/android-technical-overview-->kann verwendet werden, bis die fireTV SDK-Dokumente bereit sind. Wenn Sie bereits über Android-Anwendungen verfügen, die unser Android-SDK verwenden, sollte die Integration des fireTV-SDK in Ihre fireTV-Anwendung unkompliziert sein.

Im Vergleich zum vorhandenen Android SDK wird der Authentifizierungsprozess beim fireTV-SDK einfacher zu entwickeln sein, da die Aufgaben zur Verwaltung/Präsentation der MVPD-Anmeldeseite und zum Abrufen des AuthN-Tokens intern von der AccessEnabler-Bibliothek ausgeführt werden.

## FAQs {#faq}

1. Wie wird die **SSO** arbeiten?

   * SSO arbeitet für alle Programmer-Anwendungen mit Adobe Pass-Authentifizierung, die das neue fireTV-SDK auf demselben Amazon-Feuerfernsehgerät verwenden
   * SSO zwischen Programmer-Apps, die in der clientlosen REST-API implementiert sind, und Apps, die im fireTV-SDK implementiert sind **wird NICHT unterstützt**

1. Wie hoch ist die MVPD-Abdeckung der fireTV SSO?

   * **Alle MVPDs** Wird durch die Adobe Pass-Authentifizierung integriert, wird SSO technisch auf dem fireTV-SDK unterstützt.

1. Neben der Verwendung des neuen SDK, was andere **Workflow-Änderungen** sollten Programmierer sich dessen bewusst sein?

   * Programmierer müssen einen MVPD-Picker für die BrandTV-Plattform implementieren.

1. Wird die Authentifizierung geändert? **TTLs**?

   * Es gibt keine Verhaltensänderung in Bezug auf Authentifizierungs-TTLs.
   * Das erste gültige Authentifizierungstoken wird für die Durchführung der einmaligen Anmeldung verwendet. In diesem Fall verwenden alle anderen Anwendungen, die über SSO authentifiziert werden, dieselbe TTL, bis sie abläuft. Wenn Sie also von einer Anwendung zur anderen navigieren, teilt die zweite Anwendung die TTL der ersten authentifizierten Anwendung.

1. Wie die **AbbauAPI** arbeiten?

   * Für die Abbau-API sind keine Änderungen erforderlich. Das Benutzererlebnis entspricht dem auf Android-Geräten.

1. How **TempPass** sind betroffen?

   * TempPass-Flüsse sind einzelne Bildschirme und verhalten sich wie auf anderen nativen Geräten.

1. Funktionieren andere Adobe-Funktionen wie bisher?

   * Alle Adobe Pass-Authentifizierungsfunktionen funktionieren auf fireTV wie auf Android-Geräten.
