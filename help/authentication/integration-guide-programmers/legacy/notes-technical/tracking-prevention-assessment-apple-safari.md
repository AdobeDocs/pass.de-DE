---
title: Tracking Prevention Assessment Apple Safari
description: Tracking Prevention Assessment Apple Safari
exl-id: a3362020-92ff-4232-b923-e462868730d5
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1849'
ht-degree: 0%

---

# Bewertung der (veralteten) Tracking-Prävention - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Safari 10 {#safari10}

**Details**

Ab Safari 10 führen die standardmäßigen Browser-Datenschutzeinstellungen dazu, dass die Funktionen für Single Sign-On (SSO), Single Logout (SLO) und passive Authentifizierung nicht mehr funktionieren. Single Sign-On (SSO) und passive Authentifizierung funktionieren auch in
die gleiche Sitzung zwischen mehreren Registerkarten oder Browser-Fenstern.

Diese Änderungen wirken sich auf die Adobe Pass-Authentifizierungsprozesse aus und wirken sich darauf aus
für die folgenden Versionen von AccessEnabler JavaScript SDK: v2 (Versionen 2.x), v3 (Versionen 3.x), v4 (Versionen 4.x).

### Milderung {#mitigation-safari10}

Um diese Einschränkungen abzumildern, können Sie den Benutzer anweisen, die Datenschutzeinstellungen des Safari 10-Browsers zu ändern, und die Option &quot;**Immer zulassen** für den Eintrag &quot;**Cookies und Website-Daten**&quot; auf der Registerkarte „Datenschutz“ des Browsers unter Voreinstellungen verwenden, wie in der folgenden Abbildung dargestellt.

![](../../../assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Details**

>[!IMPORTANT]
>
>Alle oben genannten Angaben aus Abschnitt Safari 10 gelten auch im Falle von Safari 11.

Beginnend mit Safari 11 führt der Browser den Mechanismus [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/)(ITP) ein, eine Technologie, die Heuristiken verwendet, um Website-übergreifendes Tracking zu verhindern. Diese Heuristiken beeinflussen die Art und Weise, wie Cookies von Drittanbietern bei Netzwerkaufrufen gespeichert und wiedergegeben werden, was bedeutet, dass der Safari-Browser je nach Aktivierung des ITP-Mechanismus Cookies von Drittanbietern in der Kommunikation zwischen Client und Server blockiert.

Der Adobe Pass-Authentifizierungs-Service verwendet Cookies als Teil des Authentifizierungsprozesses und verlässt sich darauf **um zu**. In Situationen, in denen der Authentifizierungsprozess automatisch stattfindet (z. B. bei Temp Pass) oder in Implementierungen, die iFrames oder „refressless“ Funktionen verwenden, werden Adobe-Cookies als Drittanbieter-Cookies betrachtet und standardmäßig blockiert. In allen anderen Fällen verwendet Safari einen Algorithmus für maschinelles Lernen, der alle Cookies des Adobe-Authentifizierungsdienstes als Tracking-Cookies kennzeichnen kann und daher von ITP blockiert wird.

Abschließend kann gesagt werden, dass ein Benutzer des Safari 11-Browsers nach der Aktivierung des Intelligent Tracking Prevention-Mechanismus (ITP) möglicherweise nicht in der Lage ist, sich auf einer Website mit aktivierter Adobe Pass-Authentifizierung zu authentifizieren, insbesondere wenn der Benutzer mehrere Websites mit aktivierter Adobe Pass-Authentifizierung verwendet. Daher kann das Authentifizierungserlebnis des Benutzers unerwartet und undefiniert sein, von der Unfähigkeit zur Anmeldung bis hin zu einer kürzeren als erwarteten Authentifizierungsdauer.

Diese Änderungen wirken sich auf die Adobe Pass-Authentifizierungsprozesse für die folgenden Versionen von AccessEnabler JavaScript SDK aus: v2 (Version 2.x), v3 (Version 3.x).

### Milderung {#mitigation-safari11}

Sowohl für AccessEnabler JavaScript SDK v3 (Version 3.x) als auch für AccessEnabler JavaScript SDK v4 (Version 4.x) enthält die -Bibliothek einen Mechanismus, der Situationen identifizieren kann, in denen die Authentifizierung des Benutzers aufgrund fehlender erforderlicher Cookies blockiert wurde. In diesen Situationen senden die Bibliotheksbenutzer einen spezifischen Fehler-Callback [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference), der zurück an die Website mit aktivierter Adobe Pass-Authentifizierung übergeben wird, um als Signal verwendet zu werden, um den Trigger anzuweisen, Maßnahmen zu ergreifen, die das Problem beheben können. Um von diesem Mechanismus zu profitieren, muss die Website die Spezifikation [Fehlerberichterstattung](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) implementieren.

Für AccessEnabler JavaScript SDK v2 (Version 2.x) bietet die -Bibliothek den oben beschriebenen Mechanismus nicht. Daher kann die Website mit aktivierter Adobe Pass-Authentifizierung nicht signalisiert werden, wenn der Benutzer angewiesen wird, Maßnahmen zur Behebung des Problems zu ergreifen.

Die Liste der Aktionen, die die oben genannten Probleme beheben können **gilt für alle drei Versionen** von AccessEnabler JavaScript SDK.

Wenn [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference)-Fehler-Callback von der Website des Implementierers empfangen wird, sollte der Benutzer angewiesen werden, die intelligente Tracking-Prävention (ITP) zu deaktivieren und Drittanbieter-Cookies zu aktivieren, indem er:

* Im Fall von Mac OS X High Sierra und höher: Deaktivieren Sie die Option &quot;**Prevent cross-site tracking**&quot; für den Eintrag &quot;**Website tracking**&quot; auf der Registerkarte „Datenschutz“ des Browsers unter „Voreinstellungen“, wie in der folgenden Abbildung dargestellt.

  ![](../../../assets/uncheck-prvnt-cr-st-tr-safari11.png)


* Im Fall von Mac OS X Sierra und vorherigen: Aktivieren Sie die Option &quot;**Immer zulassen** für den Eintrag &quot;**Cookies und Website-Daten**&quot; auf der Registerkarte „Datenschutz“ des Browsers unter „Voreinstellungen“, wie in der folgenden Abbildung dargestellt.

  ![](../../../assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Details**

>[!IMPORTANT]
>
>Alle oben genannten Angaben aus Abschnitt Safari 10 und Abschnitt Safari 11 gelten auch für Safari 12.

In diesem Abschnitt werden die Kompatibilitätsprobleme von **AccessEnabler JavaScript SDK Version 4.x** auf Safari 12 beschrieben.

>[!NOTE]
>
>Beachten Sie, dass im Fall von AccessEnabler JavaScript SDK Version 2.x und AccessEnabler JavaScript SDK Version 3.x beide Drittanbieter-Cookies für die Authentifizierungsprozesse verwenden. Aufgrund der ITP- und Drittanbieter-Cookie-Richtlinien, die mit Safari 11 beginnen, kann das Authentifizierungserlebnis des Benutzers unerwartet und undefiniert sein, von der Unfähigkeit zur Anmeldung bis zur kürzeren als erwarteten Authentifizierungsdauer.


### Zertifizierte Funktionalität von AccessEnabler JavaScript SDK v4 (Version 4.x) auf Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

**Authentifizierung** Flüsse, die Benutzerinteraktionen nutzen, funktionieren immer, auch wenn die Drittanbieter-Cookies im Browser des Benutzers deaktiviert sind, da ab Version 4.0 die AccessEnabler JavaScript SDK keine Drittanbieter-Cookies mehr für die Authentifizierungsprozesse verwendet.

>[!NOTE]
>
>Der Benutzer MUSS mit der Website interagieren, um Anmelde-Popups zu öffnen und/oder mit der MVPD-Anmeldeseite zu interagieren.

**Authorization/Preflight/User Metadata**-Vorgänge sind voll funktionsfähig, vorausgesetzt, der Benutzer ist bereits authentifiziert.

### Bekannte Probleme mit AccessEnabler JavaScript SDK v4 (Version 4.x) in Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO und SLO

   * Aufgrund der Implementierung von localStorage in Safari ab Safari 10 kann der JS-SDK den Anmeldestatus nicht mehr über einen gemeinsamen Domain-iFrame teilen. Das bedeutet, dass sich der Benutzer auf jeder Site anmelden muss, die AccessEnabler JavaScript SDK verwendet. Die Abmeldung löscht auch keine Authentifizierungstoken zwischen Sites. Daher muss sich der Benutzer von jeder Website mit aktivierter Adobe Pass-Authentifizierung abmelden.

* Temp Pass

   * Für temporäre Durchläufe verwendet AccessEnabler JavaScript SDK einen Individualisierungsmechanismus, um ein Authentifizierungstoken auf einem bestimmten Gerät (Browser-Instanz) zu sperren. Aufgrund der neuen Mechanismen in Safari 12, die das Tracking verhindern sollen, ist der Fingerabdruck, den wir im Individualisierungsmechanismus berechnen und verwenden **für alle Benutzer mit derselben IP-Adresse gleich**. Wir berücksichtigen die Client-IP für Individualisierungszwecke, aber trotzdem hat dies Auswirkungen auf Benutzer, die dieselbe öffentliche IP-Adresse haben. Für diese Benutzer berechnen wir dieselbe Individualisierungs-ID und der temporäre Pass ist daran gebunden. Das bedeutet, dass niemand mehr darauf zugreifen kann, sobald ein solcher Benutzer einen temporären Pass verwendet \! Dies wirkt sich insbesondere auf Unternehmensbenutzer, Bildungseinrichtungen oder andere Organisationen aus, die mehrere Benutzer haben, die NAT oder einen gemeinsamen Proxy für den Zugriff auf das Internet verwenden.

>[!NOTE]
>
>Dieses Problem betrifft Benutzende nur, wenn das Implementierungs-Tool aufgrund der Benutzerinteraktion „Temp Pass“ verwendet. Andernfalls unterliegt die Authentifizierung „Temp Pass **den folgenden**.

* Automatische Flüsse

   * Der Versuch, Authentifizierungsflüsse im automatisierten Modus ohne Benutzerinteraktion durchzuführen, ist in Safari 12 bei Verwendung von JS SDK 4.0 nicht erfolgreich. Beachten Sie, dass in der kommenden Version 4.1 von JS SDK alle Probleme mit automatisierten Flüssen behoben werden.

Von diesem Problem betroffene Anwendungsfälle:

* Automatische TempPass-Authentifizierung (kostenlose Vorschau) - für solche Flüsse gibt der SDK einen N130-Fehler aus.

* Passiv-Authentifizierung (schlägt leise fehl) - Der Benutzer wird aufgefordert, diese MVPD auszuwählen und Anmeldeinformationen einzugeben

### Milderung {#mitigation-safari12}

**SSO und SLO**

Zum Zeitpunkt der Veröffentlichung ist keine Abmilderung verfügbar oder möglich. Apple hat in Safari 12 (`https://webkit.org/blog/8124/introducing-storage-access-api`) eine „Speicherzugriffs-API“ eingeführt, aber die aktuelle Implementierung gilt nicht für localStorage, sondern nur für Cookies. Darüber hinaus erfordert die API eine Benutzerinteraktion, um verwendet werden zu können. Sobald Sie sie verwenden, wird der Benutzer auch mit einem Berechtigungsdialogfeld ähnlich dem folgenden aufgefordert.

![](../../../assets/permission-dialog-apple.png)


Zu diesem Zeitpunkt entsprechen diese Safari-Anforderungen/-Aufforderungen nicht unseren UX-Anforderungen und wir haben kein konsistentes Verhalten wie bei anderen Browsern, bei denen SSO „einfach funktioniert“, sobald wir ein Token in einer gemeinsamen lokalen Domain gespeichert haben.

**Temp Pass**

Um die Individualisierungsprobleme abzumildern und eine Benutzerinteraktion zu ermöglichen, empfehlen wir, **[Werbe-Temporärpass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)** interaktiv zu verwenden und mindestens eine zusätzliche Information über den Benutzer bereitzustellen (z. B. E-Mail-Adresse).

## Safari 13 {#safari13}

**Details**

>[!IMPORTANT]
>
>Alle oben genannten Angaben von Abschnitt Safari 10 bis Abschnitt Safari 12 gelten auch für Safari 13.


Beginnend mit Safari 13 führt der Browser neue Änderungen am [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP) ein, wodurch die Heuristiken hinter dem Mechanismus beim Kennzeichnen von Drittanbieter-Cookies als Tracking-Cookies strenger werden, um Website-übergreifendes Tracking zu verhindern.

Wie in den vorherigen Abschnitten beschrieben, verwendet der Adobe Pass-Authentifizierungs-Service Drittanbieter-Cookies und verlässt sich darauf als Teil der Authentifizierungsprozesse, wenn Implementierungsprogramme die AccessEnabler JavaScript SDK v2 (Versionen 2.x) und AccessEnabler JavaScript SDK v3 (Versionen 3.x) verwenden. Im Vergleich zu früheren Versionen des Safari-Browsers, als ITP nach einiger Zeit aktiv wurde, um „etwas über die Interaktion zwischen dem Benutzer und den beteiligten Parteien (Websites und Adobe des Programmierers) zu erfahren, blockiert der Safari 13-Browser von Anfang an Drittanbieter-Cookies, die in der Kommunikation zwischen Client und Server als Tracking-Cookies gelten.

Abschließend ist festzustellen, dass ein Benutzer des Safari 13-Browsers höchstwahrscheinlich nicht in der Lage sein wird, neue Authentifizierungen auf einer Website mit aktivierter Adobe Pass-Authentifizierung zu initiieren, die eine ältere Version von AccessEnabler JavaScript SDK, v2 (Version 2.x) oder v3 (Version 3.x) verwendet. Dies geschieht, weil alle erforderlichen Adobe-Cookies des Primetime-Authentifizierungsdienstes von ITP blockiert werden, sodass der Dienst die Authentifizierungsanfrage nicht erfüllen kann.

Die AccessEnabler JavaScript SDK v4-Bibliothek (Version 4.x) verwendet keine Drittanbieter-Cookies für den Authentifizierungsprozess. Daher sind ihre Vorgänge durch die Safari 13-Änderungen in keiner Weise betroffen.

### Milderung {#mitigation-safari13}

Zunächst einmal empfehlen wir dringend **Migration auf AccessEnabler JavaScript SDK Version 4.x**, um ein stabiles und vorhersehbares Verhalten im Safari-Browser zu erzielen.

Zweitens enthält die Bibliothek für AccessEnabler JavaScript SDK v3 (Version 3.x) einen Mechanismus, der die Situationen erkennt, in denen die Benutzerauthentifizierung aufgrund fehlender erforderlicher Cookies blockiert wurde. In diesen Situationen senden die Bibliotheksbenutzer einen spezifischen Fehler-Callback ([N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference)), der zurück an die Website mit aktivierter Adobe Pass-Authentifizierung übergeben wird, um als Signal verwendet zu werden, um den Trigger anzuweisen, Maßnahmen zu ergreifen, die das Problem beheben können. Um von diesem Mechanismus zu profitieren, muss die Website die Spezifikation [Fehlerberichterstattung](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) implementieren.

Für AccessEnabler JavaScript SDK v2 (Version 2.x) bietet die -Bibliothek den oben beschriebenen Mechanismus nicht. Daher kann die Website mit aktivierter Adobe Pass-Authentifizierung nicht signalisiert werden, wenn der Benutzer angewiesen wird, Maßnahmen zur Behebung des Problems zu ergreifen.

Wenn [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference)-Fehler-Callback von der Website des Implementierers empfangen wird, sollte der Benutzer angewiesen werden, die intelligente Tracking-Prävention (ITP) zu deaktivieren und Drittanbieter-Cookies zu aktivieren, indem er:

* Im Fall von Mac OS X High Sierra und höher: Deaktivieren Sie die Option &quot;**Prevent cross-site tracking**&quot; für den Eintrag &quot;**Website tracking**&quot; auf der Registerkarte „Datenschutz“ des Browsers unter „Voreinstellungen“, wie in der folgenden Abbildung dargestellt.

  ![](../../../assets/prvnt-cross-site-tr-safari13.png)

* Im Fall von Mac OS X Sierra und höher: Aktivieren Sie </span> Option &quot;**Immer zulassen** für den Eintrag &quot;**Cookies und Website-Daten**&quot; auf der Registerkarte „Datenschutz“ des Browsers unter „Voreinstellungen“, wie in der folgenden Abbildung dargestellt.

  ![](../../../assets/always-allow-safari13.png)
