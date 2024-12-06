---
title: Tracking Prevention Assessment Apple Safari
description: Tracking Prevention Assessment Apple Safari
exl-id: a3362020-92ff-4232-b923-e462868730d5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1826'
ht-degree: 0%

---

# Tracking Prevention Assessment - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Safari 10 {#safari10}

**Details**

Ab Safari 10 funktionieren die standardmäßigen Datenschutzeinstellungen des Browsers nicht mehr mit Single Sign-On (SSO), Single Logout (SLO) und passiven Authentifizierungsfunktionen. Single Sign-On (SSO) und passive Authentifizierung funktionieren auch in
die gleiche Sitzung zwischen mehreren Registerkarten oder Browserfenstern.

Diese Änderungen wirken sich auf Adobe Pass-Authentifizierungsprozesse aus und wirken sich darauf aus
für die folgenden Versionen des AccessEnabler JavaScript SDK: v2 (Versionen 2.x), v3 (Versionen 3.x), v4 (Versionen 4.x).

### Abmilderung {#mitigation-safari10}

Um diese Einschränkungen zu vermeiden, können Sie den Benutzer anweisen, die Datenschutzeinstellungen des Safari 10-Browsers zu ändern, und die Option &quot;**Immer zulassen**&quot;für den Eintrag &quot;**Cookies und Website-Daten**&quot;auf der Registerkarte Datenschutz des Browsers verwenden, wie in unten abgebildeten Bildern dargestellt.

![](../assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Details**

>[!IMPORTANT]
>
>Alle oben genannten Details aus Abschnitt Safari 10 gelten weiterhin für Safari 11.

Ab Safari 11 führt der Browser den Mechanismus [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/)(ITP) ein, eine Technologie, die heuristische Verfahren verwendet, um Site-übergreifendes Tracking zu verhindern. Diese Heuristik wirkt sich auf die Art und Weise aus, wie Drittanbieter-Cookies bei Netzwerkaufrufen gespeichert und wiedergegeben werden. Das bedeutet, dass der Safari-Browser je nach Aktivierung des ITP-Mechanismus Drittanbieter-Cookies in der Kommunikation zwischen Client und Server blockiert.

Der Adobe Pass-Authentifizierungsdienst verwendet Cookies als Teil des Authentifizierungsprozesses **für die Funktion** und verlässt sich darauf. In Fällen, in denen der Authentifizierungsprozess automatisch stattfindet (z. B. Temp Pass) oder in Implementierungen, die iFrames oder die Funktion &quot;refressless&quot;verwenden, werden Adobe-Cookies als Drittanbieter-Cookies betrachtet und standardmäßig blockiert. Für alle anderen Fälle verwendet Safari einen maschinellen Lernalgorithmus, der alle Cookies des Adobe Pass Authentication-Dienstes als Tracking-Cookies kennzeichnen könnte, sodass ITP für die Blockierung des Dienstes verantwortlich ist.

Schließlich kann sich ein Benutzer des Safari 11-Browsers nach Aktivierung des ITP-Mechanismus (Intelligent Tracking Prevention), insbesondere wenn der Benutzer mehrere Websites mit aktivierter Adobe Pass-Authentifizierung verwendet, nicht bei einer Website authentifizieren, die für die Adobe Pass-Authentifizierung aktiviert ist. Das Authentifizierungs-Erlebnis des Benutzers kann daher unerwartet und nicht definiert sein, von der Unfähigkeit zur Anmeldung bis hin zu einer kürzeren Authentifizierungsdauer als erwartet.

Diese Änderungen wirken sich auf die Adobe Pass-Authentifizierungsprozesse der folgenden Versionen des AccessEnabler JavaScript SDK aus: v2 (Versionen 2.x), v3 (Versionen 3.x).

### Abmilderung {#mitigation-safari11}

Sowohl für AccessEnabler JavaScript SDK v3 (Versionen 3.x) als auch für AccessEnabler JavaScript SDK v4 (Versionen 4.x) enthält die Bibliothek einen Mechanismus, mit dem die Situationen identifiziert werden können, in denen die Authentifizierung des Benutzers aufgrund fehlender erforderlicher Cookies blockiert wurde. In diesen Fällen Trigger die Bibliothek einen bestimmten Fehler-Callback [N130](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md#advanced-error-codes-reference), der an die Website zurückgegeben wird, auf der die Adobe Pass-Authentifizierung aktiviert ist, um als Signal verwendet zu werden, um den Benutzer anzuweisen, Maßnahmen zu ergreifen, die das Problem beheben können. Um von diesem Mechanismus profitieren zu können, muss die Website die Spezifikation [Fehlerberichterstellung](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) implementieren.

Für AccessEnabler JavaScript SDK v2 (Versionen 2.x) bietet die Bibliothek den oben beschriebenen Mechanismus nicht. Daher kann die Website mit aktivierter Adobe Pass-Authentifizierung nicht signalisiert werden, wann der Benutzer angewiesen werden soll, Maßnahmen zur Behebung des Problems zu ergreifen.

Die Liste der Aktionen, die die oben genannten Probleme beheben können **gilt für alle drei Versionen** des AccessEnabler JavaScript SDK.

Wenn der Fehler-Rückruf [N130](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md#advanced-error-codes-reference) von der Website des Implementors empfangen wird, sollte der Benutzer angewiesen werden, Intelligent Tracking Prevention (ITP) zu deaktivieren und Drittanbieter-Cookies zu aktivieren, indem:

* Bei Mac OS X High Sierra und höher: Deaktivieren Sie die Option &quot;**Prevent cross-site tracking**&quot; für den Eintrag &quot;**Website tracking**&quot; auf der Registerkarte Datenschutz des Browsers in den Einstellungen, wie unten dargestellt.

  ![](../assets/uncheck-prvnt-cr-st-tr-safari11.png)


* Im Falle von Mac OS X Sierra und früher: Aktivieren Sie die Option &quot;**Immer zulassen**&quot;für den Eintrag &quot;**Cookies und Website-Daten**&quot; auf der Registerkarte Datenschutz des Browsers unter Voreinstellungen, wie unten dargestellt.

  ![](../assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Details**

>[!IMPORTANT]
>
>Alle oben genannten Details aus Abschnitt Safari 10 und Abschnitt Safari 11 gelten weiterhin für Safari 12.

In diesem Abschnitt werden die Kompatibilitätsprobleme der **AccessEnabler JavaScript SDK-Versionen 4.x** in Safari 12 beschrieben.

>[!NOTE]
>
>Beachten Sie, dass bei den AccessEnabler JavaScript SDK-Versionen 2.x und AccessEnabler JavaScript SDK Version 3.x sowohl Drittanbieter-Cookies für die Authentifizierungsprozesse verwendet werden. Aufgrund von ITP- und Drittanbieter-Cookies-Richtlinien, die mit Safari 11 beginnen, kann die Authentifizierungserfahrung des Benutzers unerwartet und undefiniert sein, von der Unfähigkeit bis zur erwarteten Authentifizierungsdauer.


### Zertifizierte Funktionalität von AccessEnabler JavaScript SDK v4 (Versionen 4.x) in Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

**Authentifizierungsflüsse**, die Benutzerinteraktionen nutzen, funktionieren immer, auch wenn die Drittanbieter-Cookies im Browser des Benutzers deaktiviert sind, da das AccessEnabler JavaScript SDK ab Version 4.0 keine Drittanbieter-Cookies mehr für die Authentifizierungsprozesse verwendet.

>[!NOTE]
>
>Der Benutzer MUSS mit der Site interagieren, um Anmelde-Popup zu öffnen und/oder mit der MVPD-Anmeldeseite zu interagieren.

Die Vorgänge **Autorisierung/Preflight/Benutzermetadaten** sind voll funktionsfähig, sofern der Benutzer bereits authentifiziert ist.

### Bekannte Probleme mit AccessEnabler JavaScript SDK v4 (Versionen 4.x) in Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO und SLO

   * Aufgrund der Implementierung von localStorage in Safari ab Safari 10 kann das JS-SDK den Anmeldestatus nicht mehr über einen gemeinsamen Domänen-iFrame freigeben. Das bedeutet, dass sich der Benutzer auf jeder Site anmelden muss, auf der AccessEnabler JavaScript SDK verwendet wird. Beim Abmelden werden auch Authentifizierungstoken nicht siteübergreifend gelöscht. Daher muss sich der Benutzer von jeder Website mit aktivierter Adobe Pass-Authentifizierung abmelden.

* Temporärer Pass

   * Für temporäre Übermittlungen verwendet das AccessEnabler JavaScript SDK einen Individualisierungsmechanismus, um ein Authentifizierungstoken an ein bestimmtes Gerät (Browserinstanz) zu sperren. Aufgrund neuer Mechanismen in Safari 12, die das Tracking verhindern sollen, ist der Fingerabdruck, den wir berechnen und im Individualisierungsmechanismus verwenden, **für alle Benutzer identisch, die dieselbe IP-Adresse haben**. Wir berücksichtigen die Client-IP für Individualisierungszwecke, aber dennoch wirkt sich dies auf Benutzer aus, die dieselbe öffentliche IP-Adresse teilen. Für diese Benutzer berechnen wir dieselbe Individualisierungs-ID und der temporäre Pass wird mit ihr verknüpft. Das bedeutet, dass ein Benutzer, der einen temporären Pass verwendet, sonst niemand Zugriff darauf hat \! Dies betrifft insbesondere Unternehmensbenutzer, Bildungseinrichtungen oder andere Organisationen, die über mehrere Benutzer verfügen, die NAT oder einen gemeinsamen Proxy für den Internetzugang nutzen.

>[!NOTE]
>
>Dieses Problem betrifft Benutzer nur, wenn der Implementor aufgrund von Benutzerinteraktionen den Temp Pass verwendet. Andernfalls unterliegt die Authentifizierung beim Temp Pass dem nachstehenden Wert **Automatische Flüsse** .

* Automatische Flüsse

   * In einem automatisierten Modus versucht werden Authentifizierungsflüsse ohne Benutzerinteraktionen in Safari 12 bei Verwendung von JS SDK 4.0. Beachten Sie, dass das bevorstehende JS-SDK 4.1 alle Probleme mit automatisierten Flüssen behebt.

Anwendungsfälle, die von diesem Problem betroffen sind:

* Automatische Authentifizierung mit dem TempPass (Free Preview): Für solche Flüsse gibt das SDK einen N130-Fehler aus.

* Passive Authentifizierung (Fehler unbeaufsichtigt) - Benutzer wird aufgefordert, diesen MVPD auszuwählen und Anmeldeinformationen einzugeben.

### Abmilderung {#mitigation-safari12}

**SSO und SLO**

Es gibt keine bekannte Abschwächung, die zur Zeit dieser Schrift verfügbar oder möglich ist. Apple hat in Safari 12 eine &quot;Speicherzugriffs-API&quot;eingeführt (`https://webkit.org/blog/8124/introducing-storage-access-api`), die aktuelle Implementierung gilt jedoch nicht für localStorage, sondern nur für Cookies. Darüber hinaus erfordert die API eine Benutzerinteraktion, damit sie verwendet werden kann. Sobald Sie sie verwenden, wird der Benutzer auch mit einem Berechtigungsdialogfeld wie dem unten stehenden aufgefordert.

![](../assets/permission-dialog-apple.png)


An dieser Stelle stimmen diese Safari-Anforderungen/Eingabeaufforderungen nicht mit unseren UX-Anforderungen überein und wir haben kein konsistentes Verhalten wie bei anderen Browsern, bei denen SSO &quot;nur funktioniert&quot;, wenn wir ein Token in einer gemeinsamen Domäne localStorage gespeichert haben.

**Temp Pass**

Um die Individualisierungsprobleme zu vermeiden und eine Benutzerinteraktion zu haben, empfehlen wir, **[Werbezeitpass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)** interaktiv zu verwenden und mindestens eine zusätzliche Information über den Benutzer (z. B. E-Mail-Adresse) bereitzustellen.

## Safari 13 {#safari13}

**Details**

>[!IMPORTANT]
>
>Alle oben genannten Details aus Abschnitt Safari 10 bis Abschnitt Safari 12 gelten weiterhin für Safari 13.


Ab Safari 13 führt der Browser neue Änderungen an der [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP) ein, wodurch die Heuristik hinter dem Mechanismus beim Kennzeichnen von Drittanbieter-Cookies als Tracking-Cookies strenger wird, um Site-übergreifendes Tracking zu verhindern.

Wie in den vorherigen Abschnitten beschrieben, verwendet der Adobe Pass-Authentifizierungsdienst Drittanbieter-Cookies als Teil der Authentifizierungsprozesse, wenn Implementoren das AccessEnabler JavaScript SDK v2 (Versionen 2.x) und AccessEnabler JavaScript SDK v3 (Versionen 3.x) verwenden. Im Vergleich zu früheren Versionen des Safari-Browsers, als ITP nach einer gewissen Zeit eintrat, um mehr über die Interaktion zwischen dem Benutzer und den beteiligten Parteien zu erfahren (Websites und Adobe des Programmierers), blockiert der Safari 13-Browser die Start-Drittanbieter-Cookies, die in der Kommunikation zwischen Client und Server als Tracking-Cookies betrachtet werden.

Abschließend möchte ich sagen, dass ein Benutzer von Safari 13-Browsern höchstwahrscheinlich nicht in der Lage sein wird, neue Authentifizierungen auf einer Website mit aktivierter Adobe Pass-Authentifizierung zu initiieren, auf der eine ältere Version von AccessEnabler JavaScript SDK, v2 (Versionen 2.x) oder v3 (Versionen 3.x) verwendet wird. Dies geschieht aufgrund der Tatsache, dass alle erforderlichen Adobe-Primetime-Authentifizierungsdienst-Cookies von ITP blockiert werden, sodass der Dienst die Authentifizierungsanforderung nicht erfüllen kann.

AccessEnabler JavaScript SDK v4 (Version 4.x) verwendet keine Drittanbieter-Cookies für den Authentifizierungsprozess. Daher sind die Vorgänge von Safari 13 in keiner Weise betroffen.

### Abmilderung {#mitigation-safari13}

Zunächst empfehlen wir dringend die **Migration auf AccessEnabler JavaScript SDK-Versionen 4.x**, um ein stabiles und vorhersehbares Verhalten im Safari-Browser zu erzielen.

Zweitens enthält die Bibliothek für AccessEnabler JavaScript SDK v3 (Version 3.x) einen Mechanismus, der die Situationen identifizieren kann, in denen die Benutzerauthentifizierung aufgrund fehlender erforderlicher Cookies blockiert wurde. In diesen Fällen Trigger die Bibliothek einen bestimmten Fehler-Callback ([N130](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md#advanced-error-codes-reference)), der an die Website zurückgegeben wird, auf der die Adobe Pass-Authentifizierung aktiviert ist, um als Signal verwendet zu werden, um den Benutzer anzuweisen, Maßnahmen zu ergreifen, die das Problem beheben können. Um von diesem Mechanismus profitieren zu können, muss die Website die Spezifikation [Fehlerberichterstellung](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) implementieren.

Für AccessEnabler JavaScript SDK v2 (Versionen 2.x) bietet die Bibliothek den oben beschriebenen Mechanismus nicht. Daher kann die Website mit aktivierter Adobe Pass-Authentifizierung nicht signalisiert werden, wann der Benutzer angewiesen werden soll, Maßnahmen zur Behebung des Problems zu ergreifen.

Wenn der Fehler-Rückruf [N130](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md#advanced-error-codes-reference) von der Website des Implementors empfangen wird, sollte der Benutzer angewiesen werden, Intelligent Tracking Prevention (ITP) zu deaktivieren und Drittanbieter-Cookies zu aktivieren, indem:

* Bei Mac OS X High Sierra und höher: Deaktivieren Sie die Option &quot;**Prevent cross-site tracking**&quot; für den Eintrag &quot;**Website tracking**&quot; auf der Registerkarte Datenschutz des Browsers in den Einstellungen, wie unten dargestellt.

  ![](../assets/prvnt-cross-site-tr-safari13.png)

* Im Falle von Mac OS X Sierra und früher: Überprüfen Sie die Option &quot;**Immer zulassen**&quot;für den Eintrag &quot;**Cookies und Website-Daten**&quot;auf der Registerkarte Datenschutz des Browsers über Voreinstellungen, wie unten dargestellt.</span>

  ![](../assets/always-allow-safari13.png)
