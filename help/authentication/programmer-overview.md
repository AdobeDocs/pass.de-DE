---
title: Übersicht für Programmierer
description: Übersicht für Programmierer
exl-id: 64a12e49-0ecb-4b81-977d-60c10925bb59
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '4274'
ht-degree: 0%

---

# Übersicht für Programmierer {#programmers-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#introduction}

Diese Übersicht richtet sich an den Inhaltsanbieter (Programmer), der plant, Adobe® Pass in seine Website oder Anwendung zu integrieren. Weitere Dokumentationen, einschließlich Schnellstart- und Integrationsleitfäden, finden Sie unten unter Verwandte Informationen .

Heute können Ihre Betrachter jederzeit und überall im Internet surfen und direkt von Ihnen, dem Programmierer, den Zugriff auf geschützte Inhalte anfordern. Sie möchten vielleicht eine einmalige Veranstaltung sehen, oder sie suchen nach Anzeigerechten für eine ganze Fernsehserie, die Sie ausstrahlen.

Bevor Sie jedoch den Zugriff auf Ihre geschützten Inhalte zulassen, müssen Sie festlegen, ob ein Kunde berechtigt ist, diese Inhalte anzuzeigen. Haben sie ein Abonnement mit einem Multichannel Video Programming Distributor (MVPD)? Wenn ja, beinhaltet dieses Abonnement Ihre Programmierung?

Die Bestimmung der Berechtigung eines Betrachters ist für einen Programmierer nicht immer einfach. Die MVPDs verfügen über die identifizierenden Daten und Zugriffsberechtigungen für ihre Kunden.  Fügen Sie hinzu, dass Betrachter, die versuchen, auf Ihren geschützten Inhalt zuzugreifen, sich für eine Vielzahl von MVPDs anmelden, von denen jede über verschiedene Systeme verfügt. Es ist einfach festzustellen, dass die Bestimmung des Anspruchs des Betrachters auf geschützte Inhalte schnell kompliziert und technisch schwierig werden kann:

![](assets/user-ent-by-progr.png)

*Abbildung: Direkt vom Programmierer bestimmte Benutzerberechtigungen*

Adobe Pass-Authentifizierung für TV Überall vermittelt diese Berechtigungstransaktionen zwischen Programmierern und MVPDs sicher. Mit der Adobe Pass-Authentifizierung können Programmierer für gültige Kunden einfache, schnelle und sichere Inhalte bereitstellen:

![](assets/user-ent-mediatedby-authn.png)

*Abbildung: Durch Adobe Pass-Authentifizierung vermittelte Benutzerberechtigungen*

Adobe Pass Authentication fungiert als Proxy im Austausch mit teilnehmenden MVPDs, sodass Sie Ihre Viewer mit einer konsistenten siteübergreifenden Oberfläche präsentieren können. Mit der Adobe Pass-Authentifizierung können Sie Ihren Viewern auch die Authentifizierung und Autorisierung für Single Sign-On (SSO) bereitstellen. Authentifizierung und Autorisierung werden für alle beteiligten Dienste verfolgt, sodass sich ein Abonnent nach seiner ersten Authentifizierung auf seinem eigenen System nicht mehr anmelden muss.

* **Authentifizierung** - Der Prozess, mit einem MVPD zu bestätigen, dass ein bestimmter Benutzer ein bekannter Kunde ist.
* **Autorisierung** - Der Prozess der Bestätigung mit einem MVPD, dass ein authentifizierter Benutzer über ein gültiges Abonnement für eine bestimmte Ressource verfügt.

### Funktionsweise der Adobe Pass-Authentifizierung {#HowItWorks}

Die Content-Viewing-Anwendung des Programmierers interagiert mit der Adobe Pass-Authentifizierung entweder über die Access Enabler-Client-Komponente oder die RESTful-Webdienste der Client-losen API (für nicht webfähige Geräte wie Smart TVs, Spielekonsolen, Set-Top-Boxen usw.). Der Access Enabler wird auf dem System des Benutzers ausgeführt, wodurch alle Berechtigungs-Workflows vereinfacht werden.  Die Komponente Access Enabler wird von der Hosting-Website unter Adobe heruntergeladen, wenn Kunden auf Ihre Site zugreifen und geschützte Inhalte anfordern.  Adobe Pass-Authentifizierungsserver hosten die RESTful-Webdienste, die in der Clientless-Lösung verwendet werden.

Die Adobe Pass-Authentifizierung verarbeitet die tatsächlichen Berechtigungs-Workflows und stellt Primitive bereit, die Sie für Folgendes verwenden:

* Festlegen Ihrer Identität. (Der Programmierer ist der &quot;Anforderer&quot;im Berechtigungsfluss für die Adobe Pass-Authentifizierung.)
* Authentifizieren Sie einen Benutzer mit einem bestimmten MVPD.  (Der MVPD ist der &quot;Identitäts-Provider&quot;oder &quot;IdP&quot;im Berechtigungsfluss der Adobe Pass-Authentifizierung.)
* Autorisieren Sie einen Benutzer mit dem MVPD für eine bestimmte Ressource.
* Melden Sie den Benutzer ab.

Der Programmierer ist für seine übergeordnete Web-Seite oder Player-Anwendung verantwortlich, die Folgendes durchführt:

* Implementiert die Benutzeroberfläche
* Interagiert mit den Web-Services Access Enabler oder ClientLess API

Ziel der Adobe Pass-Authentifizierung ist es, eine einfache, modulare Methode zu erstellen, mit der sowohl Programmierer als auch MVPDs die Berechtigungsprüfung durchführen können.

## Grundlagen zu Token {#understanding-tokens}

Die Berechtigungslösung für die Adobe Pass-Authentifizierung konzentriert sich auf die Generierung bestimmter Datenteile, die nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungsarbeitsabläufe erstellt werden. Diese Daten werden als Token bezeichnet. Token haben eine begrenzte Lebensdauer. Wenn sie ablaufen, müssen Token erneut ausgegeben werden, indem die Authentifizierungs- und Autorisierungs-Workflows neu gestartet werden.

Weitere Informationen zu Tokens finden Sie in den folgenden Abschnitten:

* [Typen von Token](#token-types)
* [Token-Speicher](#token-storage)
* [Token-Sicherheit](#token-security)
* [Token-Freigabe](#token-sharing)

### Token-Typen {#token-types}

Während der Authentifizierungs- und Autorisierungs-Workflows werden drei Typen von Token ausgegeben. Die AuthN- und AuthZ-Token sind &quot;langlebig&quot;, was Kontinuität im Anzeigeerlebnis des Benutzers bietet. Das Media Token ist ein kurzlebiges Token, das Best Practices der Branche zur Verhinderung von Betrug durch Stream-Ripping unterstützt. Programmierer geben die TTL-Werte (Time-to-Live) für jeden Token-Typ basierend auf Vereinbarungen mit MVPDs an. Programmierer entscheiden über einen TTL-Wert, der am besten für Ihr Unternehmen und Ihre Kunden geeignet ist.

* **AuthN-Token** (&quot;langlebig&quot;): Bei erfolgreicher Authentifizierung erstellt die Adobe Pass-Authentifizierung ein AuthN-Token, das sowohl dem anfordernden Gerät als auch einer global eindeutigen Kennung (GUID) zugeordnet ist.
   * Adobe Pass Authentication sendet das AuthN-Token an den Access Enabler, der es sicher auf dem Client-System zwischenspeichert.  Während das AuthN-Token vorhanden und nicht abgelaufen ist, ist es für alle Anwendungen verfügbar, die die Adobe Pass-Authentifizierung verwenden. Der Access Enabler verwendet das AuthN-Token für den Autorisierungsfluss.
   * Zu jedem Zeitpunkt wird nur ein AuthN-Token zwischengespeichert. Wenn ein neues AuthN-Token ausgegeben und bereits ein altes vorhanden ist, überschreibt die Adobe Pass-Authentifizierung das zwischengespeicherte Token.
* **AuthZ-Token** (&quot;langlebig&quot;): Bei erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung ein AuthZ-Token, das mit dem anfordernden Gerät verknüpft ist, sowie eine bestimmte geschützte Ressource.  Die geschützte Ressource wird durch eine eindeutige Ressourcen-ID identifiziert.
   * Adobe Pass Authentication sendet das AuthZ-Token an den Access Enabler, der es sicher auf dem lokalen System zwischenspeichert. Der Access Enabler verwendet dann das AuthZ-Token, um das kurzlebige Medien-Token zu erstellen, das für den tatsächlichen Anzeigezugriff verwendet wird.
   * Zu jedem Zeitpunkt wird nur ein AuthZ-Token pro Ressource zwischengespeichert. Adobe Pass-Authentifizierung kann mehrere AuthZ-Token zwischenspeichern, sofern sie mit verschiedenen Ressourcen verknüpft sind. Wenn ein neues AuthZ-Token ausgegeben wird und bereits ein altes für dieselbe Ressource existiert, überschreibt die Adobe Pass-Authentifizierung das zwischengespeicherte Token.
* **Medien-Token** (&quot;short-Lived&quot;): Der Access Enabler verwendet das AuthZ-Token, um ein kurzlebiges (Standard: 7 Minuten) Medien-Token zu generieren. Dies ist der Punkt, an dem eine erfolgreiche Wiedergabeanforderung als aufgetreten gilt.
   * Bevor Sie Zugriff auf die geschützte Ressource gewähren, muss Ihr Medienserver zur Validierung des Medien-Tokens eine Adobe Pass-Authentifizierungskomponente (den Media Token Verifier) verwenden.
   * Da das Medien-Token nicht an das Gerät gebunden ist, ist seine Lebensdauer deutlich kürzer (Standard: 7 Minuten) als die der lang gelebten AuthN- und AuthZ-Token.
   * Das kurzlebige Medien-Token ist auf die einmalige Verwendung beschränkt und wird nie zwischengespeichert. Er wird jedes Mal, wenn eine Autorisierungs-API aufgerufen wird, vom Adobe Pass-Authentifizierungsserver abgerufen.

### Token-Speicher {#token-storage}

Der Access Enabler speichert langlebige Token (AuthN und AuthZ) an Orten, die für die jeweilige Umgebung spezifisch sind:

* **Flash 10.1** (oder höher): Die langlebigen Token werden als &quot;Lokale freigegebene Objekte&quot;gespeichert.
* **HTML5**: Die langlebigen Token werden sicher im lokalen Speicher des HTML5-Browsers aufbewahrt.
* **iOS**: Die langlebigen Token werden auf einer beständigen Whiteboard gespeichert, auf die sie von anderen Adobe Pass-Authentifizierungs-Client-Anwendungen aufgerufen werden können.
* **Android**: Die langlebigen Token werden in einer freigegebenen Datenbankdatei gespeichert, auf die sie von anderen Adobe Pass-Authentifizierungs-Client-Anwendungen zugegriffen werden können.
* **Clientlose API-Geräte**: Token werden auf den Adobe Pass-Authentifizierungsservern gespeichert.

### Token-Sicherheit {#token-security}

Der Adobe Pass-Authentifizierungsserver signiert alle langlebigen Token digital mit der Geräte-ID (abgeleitet aus den Hardwareeigenschaften des Geräts). Die digitale Signatur unterscheidet sich je nach Umgebung in ihrer Erstellung, ihrem Schutz und ihrer Validierung:

* **Flash 10.1** (oder höher) - Die Geräte-ID basiert auf der Geräteberechtigung, einem eindeutigen Zertifikat, das vom Adobe-Individualisierungsserver ausgestellt wird. Diese Sicherheit entspricht der FAXS DRM-Technologie. Bei dieser serverseitigen Validierung wird die eindeutige Geräte-ID im Token mit der Geräteberechtigung verglichen (die sicher von der Flash Player an die Adobe Pass-Authentifizierung übermittelt wird). Die Geräteberechtigung gibt auch die FAXS-Client-Version und die Flash Player (oder AIR)-Version an, für die sie ausgestellt wurde. Die Gerätebindung ist stärker als bei HTML5, sodass die Time-to-Live (TTL) für Token normalerweise länger mit Flash ist.
* **HTML5** - Das Gerät wird clientseitig individualisiert. Es verwendet über JavaScript verfügbare Eigenschaften, um eine Pseudo-Geräte-ID zu erstellen, die Browser- und Betriebssystemversionen, eine IP-Adresse und eine GUID für Browsercookies (global eindeutige Kennung) enthält. Diese Token-Geräte-ID wird mit der aktuellen Pseudo-Geräte-ID für das Gerät verglichen. Da sich die IP-Adresse während der normalen Verwendung ändern kann, auch in derselben Sitzung, speichert die Adobe Pass-Authentifizierung HTML5-Token an zwei Speicherorten: localStorage und sessionStorage. Wenn sich die IP ändert und das sessionStorage-Token ansonsten weiterhin gültig ist, wird die Sitzung beibehalten. Bei HTML5 ist die Gerätebindung nicht so stark, daher ist die TTL für Token in der Regel kleiner als für Flash.
* **Native Clients** (iOS und Android) - Die langlebigen Token enthalten native ID-Individualisierungsinformationen für Geräte und sind daher an das anfordernde Gerät gebunden. Die Authentifizierungs- und Autorisierungsanfragen werden über HTTPS gesendet und die Geräte-ID-Informationen werden digital von der Access Enabler-Bibliothek signiert, bevor sie an die Backend-Server gesendet werden. Auf der Serverseite werden die Geräte-ID-Informationen anhand der zugehörigen digitalen Signatur validiert.
* **Clientlose API-Clients** - Die clientlose API-Lösung verfügt über einen Satz von Sicherheitsprotokollen, die das digitale Signieren aller API-Aufrufe umfassen. Während der Berechtigungsflüsse generierte Token werden sicher auf den Adobe Pass-Authentifizierungsservern gespeichert.

Die Adobe Pass-Authentifizierung validiert jedes langlebige Token, um sicherzustellen, dass das Gerät, das auf den Inhalt zugreift, mit dem Gerät übereinstimmt, das das Token ausgegeben hat. Bei allen Token stellt eine clientseitige Validierung sicher, dass die digitale Signatur intakt ist und dass die Integrität des Tokens erhalten bleibt. Wenn die Überprüfung der Geräte-ID fehlschlägt, wird die Authentifizierungssitzung ungültig gemacht und der Benutzer wird aufgefordert, sich erneut anzumelden, wodurch die Token zurückgesetzt werden.

### Token-Freigabe {#token-sharing}

Anwendungen auf verschiedenen Plattformen verwenden keine Token. Hierfür gibt es eine Reihe von Gründen, darunter die folgenden:

* Wie in [Token-Speicher](#token-storage) beschrieben, ist die Methode zum Speichern von Token von Plattform zu Plattform unterschiedlich (z. B. Lokale freigegebene Objekte für Flash, WebStorage für JavaScript).
* Der Grad der Token-Sicherheit unterscheidet sich zwischen Plattformen. Beispielsweise sind Flash-Token mit FAXS stark an ein Gerät gebunden. Token in einer reinen JavaScript-Umgebung verfügen nicht über die gleiche DRM-Unterstützung wie Flash.  Die Freigabe von JS-Token mit Flash-Anwendungen würde die Wahrscheinlichkeit verringern, dass weniger sichere Token eine sicherere Umgebung nutzen.

## Lebenszyklus der Programmierintegration {#prog-integ-lifecycle}

![](assets/progr-flow-int-lifecycle.png)

*Abbildung: Integrieren der Authentifizierung mit der Website und Anwendung des Programmierers*


## Logische Flüsse {#logical-flows}

### Entitäts-Flussdiagramm {#chart}

Das folgende Flussdiagramm zeigt den Gesamtprozess der Bestätigung der Berechtigung (mithilfe der Client-Komponente Adobe Pass Authentication Access Enabler ):

![](assets/ent-flowchart.png)

*Abbildung: Verfahren zur Bestätigung der Berechtigung*

### Authentifizierungsschritte {#authn-steps}

Die folgenden Schritte zeigen ein Beispiel für den Authentifizierungsfluss der Adobe Pass-Authentifizierung.  Dies ist der Teil des Berechtigungsprozesses, bei dem ein Programmierer feststellt, ob der Benutzer ein gültiger Kunde eines MVPD ist.  In diesem Szenario ist der Benutzer ein gültiger Abonnent eines MVPD.  Der Benutzer versucht, geschützte Inhalte mithilfe der Flash-Anwendung eines Programmierers anzuzeigen:

1. Der Benutzer besucht die Webseite des Programmierers, auf der die Flash-Applikation des Programmierers und die Adobe Pass Authentication Access Enabler-Komponenten auf den Computer des Benutzers geladen werden. Die Flash-Applikation verwendet Access Enabler, um die Identifikation des Programmierers mit der Adobe Pass-Authentifizierung festzulegen, und die Adobe Pass-Authentifizierung führt den Access Enabler mit Konfigurations- und Statusdaten für diesen Programmierer (den &quot;Anforderer&quot;) an. Der Access Enabler muss diese Daten vom Server empfangen, bevor er andere API-Aufrufe durchführt. Technischer Hinweis: Der Programmierer legt seine Identität mit der `setRequestor()` -Methode des Access Enabler fest. Weitere Informationen finden Sie im [Integrationsleitfaden für Programmierer](/help/authentication/programmer-integration-guide-overview.md).
1. Wenn der Benutzer versucht, den geschützten Inhalt des Programmierers anzuzeigen, zeigt die Programmer-Anwendung dem Benutzer eine Liste von MVPDs an, aus denen der Benutzer einen Anbieter auswählt.
1. Der Benutzer wird an einen Adobe Pass-Authentifizierungsserver weitergeleitet, auf dem eine verschlüsselte [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) -Anfrage für den vom Benutzer ausgewählten MVPD erstellt wird. Diese Anfrage wird als Authentifizierungsanfrage im Namen des Programmierers an den MVPD gesendet. Je nach System des MVPD wird der Browser des Benutzers dann entweder zur MVPD-Site weitergeleitet, um sich anzumelden, oder ein Anmelde-iFrame wird in der App des Programmierers erstellt.
1. In beiden Fällen (Umleitung oder iFrame) akzeptiert der MVPD die Anfrage und zeigt die Anmeldeseite an.
1. Der Benutzer meldet sich mit dem MVPD an, der MVPD validiert den Status des Benutzers als zahlender Kunde und dann erstellt der MVPD seine eigene HTTP-Sitzung.
1. Wenn der Benutzer validiert wird, erstellt der MVPD eine Antwort (SAML und verschlüsselt), die der MVPD an die Adobe Pass-Authentifizierung zurücksendet.
1. Adobe Pass Authentication empfängt die MVPD-Antwort, erkennt, dass eine Adobe Pass Authentication HTTP-Sitzung geöffnet ist, validiert die SAML-Antwort vom MVPD und leitet zurück zur Programmer-Site.
1. Die Seite des Programmierers wird neu geladen, der Access Enabler wird neu geladen und der Programmierer ruft erneut setRequestor() auf.  Der zweite Aufruf von setRequestor() ist erforderlich, da die aktuelle Konfiguration geändert wurde. Es ist jetzt eine Markierung vorhanden, die den Access Enabler informiert, dass ein AuthN-Token auf dem Server auf die Generierung wartet.
1. Der Access Enabler erkennt, dass eine ausstehende Authentifizierung vorliegt, und fordert das Token vom Adobe Pass-Authentifizierungsserver an. Das Token wird vom Server abgerufen, indem die Flash Player DRM-Funktionen aufgerufen werden.
1. Das AuthN-Token wird im Flash Player-LSO-Cache des Programmierers gespeichert. Die Authentifizierung ist jetzt abgeschlossen und die Sitzung wird auf dem Adobe Pass-Authentifizierungsserver zerstört.

### Autorisierungsschritte {#authz-steps}

Die folgenden Schritte werden in den [Authentifizierungsschritten](#authn-steps) fortgesetzt:

1. Wenn der Benutzer versucht, auf den geschützten Inhalt des Programmierers zuzugreifen, sucht die Anwendung des Programmierers zunächst auf dem lokalen Computer oder Gerät des Benutzers nach einem AuthN-Token.  Wenn dieses Token nicht vorhanden ist, werden die obigen [Authentifizierungsschritte](#authn-steps) befolgt.  Wenn das AuthN-Token vorhanden ist, wird der Autorisierungsfluss mit der Anwendung des Programmierers fortgesetzt, die einen Aufruf an den Access Enabler mit einer Anfrage zum Abrufen der Anzeigerechte des Benutzers für ein bestimmtes Element geschützter Inhalte initiiert.
1. Das spezifische Element geschützter Inhalte wird durch eine &quot;Ressourcen-ID&quot;dargestellt.  Dies kann eine einfache Zeichenfolge oder eine komplexere Struktur sein, aber in jedem Fall wird die Art der Ressourcenkennung im Voraus zwischen dem Programmierer und dem MVPD vereinbart.  Die Anwendung des Programmierers übergibt die Kennung der Ressource an den Access Enabler.  Der Access Enabler sucht auf dem lokalen Computer oder Gerät des Benutzers nach einem AuthZ-Token.  Wenn das AuthZ-Token nicht vorhanden ist, übergibt der Access Enabler die Anfrage an den Backend-Adobe Pass-Authentifizierungsserver.
1. Der Adobe Pass-Authentifizierungsserver kommuniziert mithilfe standardisierter Protokolle mit dem MVPDs-Autorisierungsendpunkt.  Wenn die Antwort des MVPD anzeigt, dass der Benutzer berechtigt ist, den geschützten Inhalt anzuzeigen, erstellt der Adobe Pass-Authentifizierungsserver ein AuthZ-Token und übergibt es an den Access Enabler, der das AuthZ-Token auf dem Computer des Benutzers speichert.
1. Wenn ein AuthZ-Token auf dem Computer oder Gerät des Benutzers gespeichert ist, ruft die Anwendung des Programmierers den Access Enabler auf, um ein Media Token vom Adobe Pass-Authentifizierungsserver abzurufen, und stellt dieses Token für die Anwendung des Programmierers bereit.
1. Schließlich verwendet die Anwendung des Programmierers die Komponente Media Token Verifier , um zu bestätigen, dass der richtige Benutzer den richtigen Inhalt anzeigt, und mit dem vorhandenen Media Token kann der Benutzer den geschützten Inhalt anzeigen.


## Registrierung und Initialisierung {#reg-and-init}

Der erste Schritt bei der Arbeit mit der Adobe Pass-Authentifizierung besteht darin, sich bei Adobe oder einem Partner für die Adobe Pass-Authentifizierung zu registrieren.

Bei der Registrierung geben Sie eine Liste der Domänen an, von denen Sie mit der Adobe Pass-Authentifizierung kommunizieren. Die Turner Broadcasting System-Domänen umfassen beispielsweise tbs.com und tnt.tv als mit Adobe Pass-Authentifizierung registrierte Domänen. Jede dieser Inhalts-Sites erhält Zugriff auf die Adobe Pass-Authentifizierung und erhält eine eigene Anforderungs-ID (z. B. &quot;TBS&quot;und &quot;TNT&quot;). Wenn Sie sich dafür entscheiden, weitere Sites hinzuzufügen, müssen Sie die Adobe über die zusätzlichen Domänennamen informieren, damit sie in die Liste der zulässigen Domänen aufgenommen werden, und zusätzliche Anforderer-IDs erhalten.

>[!WARNING]
>
>Stellen Sie beim Testen Ihrer Integration sicher, dass sich Ihr Testserver in einer registrierten Domäne befindet, die Sie in der Produktion verwenden möchten. Anforderungen, auch Testanforderungen, die von Domänen stammen, die nicht auf der Whitelist stehen, werden ignoriert. Wenn Sie beispielsweise domain.com für die Verwendung in der Produktion angefordert haben, stellen Sie sicher, dass Sie Ihre Testintegration unter domain.com, test.domain.com und staging.domain.com bereitstellen.
>
>Anforderungen, die Benutzernamen und/oder Kennwort in der URL enthalten, werden ignoriert, selbst wenn Domänen auf der Whitelist stehen. Beispiel: `//username@registered-domain/`

Die Anforderer-ID identifiziert den Client des Programmierers in allen Kommunikationen mit der Client-Komponente Access Enabler der Adobe Pass-Authentifizierung eindeutig. Alle statischen Adobe Pass-Authentifizierungsdaten, die mit dem Programmierer verknüpft sind, werden dieser ID zugewiesen.

>[!TIP]
>
>Zusätzlich zu Ihrer Anforderer-ID erhalten Sie bei der Registrierung auch funktionale URLs für die Client-Komponente Access Enabler und den Media Token Verifier.

## Integrationsschritte {#integration-steps}

>[!TIP]
>
>Wenn Sie die Adobe-Open Source Media Framework (&quot;OSMF&quot;) für Ihre Medienplayer-Entwicklung verwenden, besteht die schnellste Möglichkeit zur Verwendung der Adobe Pass-Authentifizierung darin, das OSMF-Plugin *(nicht mehr unterstützt)* in den Code Ihres Players zu integrieren.
>
><!--For details, see [Adobe Pass Authentication Plugin For OSMF](https://tve.helpdocsonline.com/9-2-2) in the Programmer Integration Guide.-->

1. [Request-Einrichtung](#requestor)
1. [Umgang mit Authentifizierung und Autorisierung](#authn-authz)
1. [Unterstützende einmalige Abmeldung](#ssl)

### 1. Einrichtung des Anforderers {#requestor}

#### (1a) Registrieren bei Adobe

Ihr erster Schritt besteht darin, sich bei Adobe oder einem autorisierten Partner für die Adobe Pass-Authentifizierung zu registrieren.  Bei der Registrierung erhalten Sie eine oder mehrere globale eindeutige Kennungen (GUIDs). Jede ausgestellte GUID ist mit einer Domäne verknüpft, von der aus der Zugriff auf die Adobe Pass-Authentifizierung möglich ist. Sie übergeben eine GUID (die Anforderer-ID) für die anfordernde Domäne, um Ihre Identität für jede Sitzung zu registrieren, in der Sie mit dem Access Enabler interagieren. Weitere Informationen finden Sie unter [Registrierung und Initialisierung](#reg-and-init) im Programmierer-Integrationsleitfaden.

#### 1b. Integration von Initial Access Enabler

Der nächste Schritt besteht darin, den Access Enabler in Ihre vorhandene Medienplayer-App oder -Webseite zu integrieren:

* Sie können die Flash-Version `AccessEnabler.swf` in einen Flash-basierten Videoplayer einbetten oder sie direkt in die HTML Ihrer Web-Seite einbetten. Sie können mit der Access Enabler-SWF entweder in ActionScript oder JavaScript kommunizieren. Die Basis-API lautet ActionScript, aber wenn Sie es vorziehen, mit JavaScript zu arbeiten, steht eine vollständige Wrapper-Bibliothek zur Verfügung, die in Ihre Seiten aufgenommen werden kann.
* Bei Nicht-Flash-Umgebungen haben Sie folgende Möglichkeiten:
   * Verwenden Sie die HTML5/JavaScript-Version AccessEnabler.js und kommunizieren Sie mit ihr über die JavaScript-API.
   * Verwenden Sie die native AIR-Erweiterung für die Adobe Pass-Authentifizierung, um nativen Code mit integrierten ActionScript-Klassen zu kombinieren.
   * Verwenden Sie eine der nativen Clientversionen der Access Enabler Library (iOS oder Android)

### 2. Umgang mit Authentifizierung und Autorisierung {#authn-authz}

#### 2a. Kommunikation mit dem Access Enabler

Die Kommunikation zwischen dem Access Enabler und Ihrer Web- oder Player-App ist asynchron. Ihre Anwendung ruft Access Enabler-Methoden auf und der Access Enabler antwortet über Callbacks, die Sie bei der Access Enabler-Bibliothek registrieren.

* Wenn Ihre Anwendung eine Autorisierungsanfrage stellt, initiiert der Access Enabler automatisch eine Authentifizierungsanfrage, wenn sich ein gültiges AuthN-Token noch nicht im lokalen System befindet. Wenn die Authentifizierung erfolgreich ist, wird das AuthN-Token des Benutzers lokal gespeichert, sodass er sich nicht erneut anmelden muss. Wenn sie sich erfolgreich über die Adobe Pass-Authentifizierung in einem anderen Kontext authentifiziert haben (z. B. über die MVPD-Website oder mit einem anderen Programmierer), hat der Access Enabler Zugriff auf das lokale AuthN-Token und eine zusätzliche Authentifizierung ist nicht erforderlich.
* Wenn ein Benutzer versucht, auf Ihren geschützten Inhalt zuzugreifen, senden Sie eine Autorisierungsanfrage an den Access Enabler. Nach der Überprüfung (oder Initiierung) der Authentifizierung kontaktiert der Access Enabler das MVPD (über den Adobe Pass-Authentifizierungsserver), um festzustellen, ob der Kunde berechtigt ist, den geschützten Inhalt anzuzeigen. Ihre Anwendung muss nur die Anfrage an den Access Enabler senden und dann die Antwort verarbeiten (Autorisierungserfolg oder -fehler). Wenn die Autorisierung erfolgreich ist, wird ein AuthZ-Token auf dem Clientsystem gespeichert.  Schließlich erhält Ihre Anwendung ein kurzlebiges Medien-Token zur Verwendung in Ihrem eigenen Genehmigungsverfahren.

>[!NOTE]
>
>* Die Authentifizierung erfolgt als SAML-Austausch zwischen der Adobe Pass-Authentifizierung als Service Provider (SP) und dem MVPD als Identitäts-Provider (IdP).
>
>* Die Autorisierung verwendet einen Back-Channel-Webdienstaustausch (Server-zu-Server) zwischen der Adobe Pass-Authentifizierung (der SP) und einem MVPD (der IdP).


#### 2b. Bereitstellen einer Entitäts-Benutzeroberfläche {#entitlement-ui}

Sie stellen eine eigene Benutzeroberfläche für den Benutzerzugriff auf Ihre Inhalte bereit. Einige Elemente, wie der eigentliche Anmeldeprozess, werden vom MVPD bereitgestellt und einige Elemente sind optional als Teil der Adobe Pass-Authentifizierung verfügbar. Führen Sie mindestens die folgenden Schritte aus:

* **Implementieren Sie eine MVPD-Auswahlschnittstelle, über die ein neuer Benutzer seinen MVPD identifizieren und sich zum ersten Mal anmelden kann.** Für die Entwicklung bietet der Access Enabler eine einfache Benutzeroberfläche, über die der Kunde MVPDs auswählen und den Anmeldeprozess starten kann. Für die Produktion müssen Sie Ihr eigenes MVPD-Auswahldialogfeld implementieren. Einige MVPDs leiten zur Anmeldung zu ihrer eigenen Site um und einige verlangen, dass ihre Anmeldeseiten in einem iFrame angezeigt werden. Sie müssen einen Callback implementieren, der diesen iFrame erstellt, um die Fälle zu verarbeiten, in denen der MVPD des Benutzers seine Anmeldeseite in einem iFrame anzeigt.
* **geschützten Inhalt identifizieren**. Für geschützten Inhalt ist eine Zugriffsberechtigung erforderlich. Auf Ihrer Benutzeroberfläche sollte angegeben werden, welcher Inhalt geschützt ist und welcher Inhalt autorisiert wurde.  Der Autorisierungsstatus wird häufig mit den Symbolen &quot;entsperrt&quot;und &quot;gesperrt&quot;angezeigt.
* **Anzeigen, dass ein Benutzer authentifiziert ist**. Sie sollten den Authentifizierungsstatus eines Benutzers im Rahmen beliebiger Methoden zur Identifizierung geschützter Inhalte angeben. Sie können den Access Enabler abfragen, um festzustellen, ob der Kunde bereits authentifiziert wurde.

#### 2c. Integrieren des Medien-Token-Verifikators {#int-media-token-ver}

Sie müssen die Komponente Adobe Pass Authentication Media Token Verifier in Ihren Medienserver integrieren.  Dadurch kann Ihr vorhandener Token-Prüfer die kurzlebigen Medien-Token erkennen, die von der Adobe Pass-Authentifizierung mit einer erfolgreichen Autorisierung bereitgestellt werden. Der Media Token Verifier überprüft die Medien-Token als letzten Sicherheitsschritt, bevor Sie dem Benutzer Zugriff auf geschützte Inhalte gewähren. Sie erhalten den Speicherort, von dem Sie den Media Token Verifier herunterladen können, wenn Sie sich bei Adobe registrieren.

### 3. Unterstützung der einmaligen Abmeldung {#ssl}

In den meisten Fällen ist Ihr Medienplayer für die Handhabung von Benutzerabfragen über eine einfache Access Enabler API verantwortlich. Wenn Sie logout() aufrufen, führt der Access Enabler Folgendes aus:

* Melden Sie den aktuellen Benutzer ab
* Löscht alle Authentifizierungs- und Autorisierungsinformationen für den abgemeldeten Benutzer
* Löscht alle AuthN- und AuthZ-Token aus dem lokalen System des Benutzers

Wenn der Benutzer seinen Computer so lange im Leerlauf lässt, dass seine Token ablaufen, kann der Benutzer dennoch zur Sitzung zurückkehren und die Abmeldung erfolgreich starten. Die Adobe Pass-Authentifizierung stellt sicher, dass alle Token gelöscht werden, und benachrichtigt den MVPD, auch ihre Sitzung zu löschen.

Wenn die Abmeldung von einer Site initiiert wird, die nicht mit der Adobe Pass-Authentifizierung integriert ist, kann der MVPD den Adobe Pass-Authentifizierungs-Single-Logout-Dienst über eine Browser-Umleitung aufrufen.

## Grundlegendes zu Benutzer-IDs {#user-ids}

Grundsätzlich ist jeder Benutzer, der einen Berechtigungsfluss initiiert, einer einzelnen eindeutigen Benutzer-ID zugeordnet.  Im Zuge eines Berechtigungsflusses kann jedoch eine Benutzer-ID auf unterschiedliche Weise angezeigt werden, je nachdem, von welcher API Sie die ID erhalten.

Die sessionGUID im Short Media Token ist die sichere Form der UserID, die über den sendTrackingData() -Aufruf verfügbar ist.   In allen aktuellen Integrationen ist dies eine beständige GUID für den Benutzer über Zeit und Geräte hinweg, aber die Quelle der GUID beginnt mit der UserID in der SAML-Antwort vom MVPD.   Einige MVPDs könnten sich jedoch in Zukunft anders entscheiden und eine vorübergehende GUID senden.  Wenn ein Programmierer sicherstellen möchte, dass die MVPD-Quell-UserID in der AuthN-Antwort persistent ist, sollte er dies in seinen Vereinbarungen mit MVPDs anordnen.

Im Folgenden finden Sie die verschiedenen Möglichkeiten, wie die Benutzer-ID in den Adobe Pass-Authentifizierungs-APIs dargestellt wird:

* `sendTrackingData()` GUID-Eigenschaft - Dies ist die Adobe-Hash-Version der MVPD UserID.  Es wird gehasht, sodass diese Benutzer-ID nicht von der MVPD an die Quelle zurückverfolgt werden kann.   Diese ID ist eindeutig und in der Regel persistent, kann jedoch nicht mit dem MVPD geteilt werden, um das spezifische Nutzungsverhalten mit dem zu vergleichen, was MVPDs auf ihrer Seite haben.   Es ist nicht digital unterzeichnet, daher nicht unbeschädigt für die Betrugsbekämpfung, aber es ist gut genug für Analysen.  Dieses Formular der Benutzer-ID wird clientseitig für alle Ereignisse bereitgestellt, die die Adobe Pass-Authentifizierung im AuthN/AuthZ-Fluss generiert.
* Eigenschaft `sessionGUID` des Short Media Token - Dies entspricht der UserID über `sendTrackingData()`. Diese Eigenschaft ist jedoch digital signiert, um ihre Integrität zu schützen.  Dadurch ist dieser Wert ausreichend für die Verfolgung von Betrug bei gleichzeitiger Nutzung. Er soll nach Verwendung unserer Validator-Bibliothek serverseitig verarbeitet werden und kann auf Betrugsmuster analysiert werden, bevor der Video-Stream an den Client freigegeben wird.  Die Durchführung dieser Aufgaben obliegt dem Programmierer.
* `getMetadata() userID `property - Diese Eigenschaft ermöglicht es Adobe, die eigentliche MVPD UserID-Quell-MVPD für den Programmierer verfügbar zu machen. Sie wird mit dem öffentlichen Schlüssel aus dem Zertifikat, das wir vom Programmierer haben, verschlüsselt, sodass sie dem Client nicht im Klartext angezeigt wird. Dadurch erhält der Programmierer die eigentliche UserID aus dem MVPD, also kann sie direkt mit dem MVPD für die Kontoverknüpfung oder Betrugsuntersuchung verwendet werden.

**Schlussfolgerung**

* Die MVPD-Benutzer-ID ist eine allgemeine, aber nicht garantierte, beständige eindeutige ID, die **aus den MVPDs generiert und bei erfolgreicher Authentifizierung an Adobe übergeben wird**. Es ist im Allgemeinen in allen Netzwerken mit einigen Ausnahmen konsistent.
* Die MVPD-Benutzer-ID enthält keine personenbezogenen Daten und ist KEINE Kontonummer. Es muss nicht in verschlüsselter Form offen gelegt werden, da wir mit allen MVPDs überprüft haben, dass keine PII gesendet werden.


Wie Sie die Benutzer-ID verwenden, hängt vom Anwendungsfall ab:

* Wenn Sie es für Tracking/Analysen benötigen, ist es am praktischsten, es von `sendTrackingData()` zu erhalten.
* Wenn Sie es Server-seitig für Stream-Veröffentlichung, Betrug oder operative Daten benötigen, können Sie es vom Media Token Validator abrufen.
* Wenn Sie es für Kontoverknüpfung und tieferen Betrug benötigen, wenden Sie sich an Ihren Adobe-Ansprechpartner, um die Verfügbarkeit zu erhalten.

<!--
>[!RELATEDINFORMATION]
>
>* **Kickstart Guides** These guides explain the initial steps to take once you have decided to begin integrating with Adobe Pass Authentication.
>* **Programmer Integration Guide** This is a lower level technical guide for Programmers, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>* [Overview For MVPDs](/help/authentication/mvpd-overview.md) This provides a similar level of conceptual information as in this Programmer overview, but is directed toward MVPDs.
-->
