---
title: Übersicht für Programmierer
description: Übersicht für Programmierer
exl-id: 64a12e49-0ecb-4b81-977d-60c10925bb59
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '4274'
ht-degree: 0%

---

# Übersicht für Programmierer {#programmers-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Einführung {#introduction}

Diese Übersicht richtet sich an Content Provider (Programmierer), die Adobe® Pass in ihre Website oder Applikation integrieren wollen. Weitere Dokumentationen, einschließlich Kickstart- und Integrationshandbüchern, finden Sie weiter unten unter „Weitere Informationen“.

Heute können Ihre Zuschauer jederzeit und überall ins Internet gehen und den Zugriff auf geschützte Inhalte direkt von Ihnen, dem Programmierer, anfordern. Vielleicht wollen sie ein einmaliges Ereignis sehen oder sie suchen die Fernsehrechte für eine ganze Fernsehserie, die Sie ausstrahlen.

Bevor Sie jedoch den Zugriff auf Ihre geschützten Inhalte zulassen, müssen Sie festlegen, ob ein Kunde berechtigt ist, diese Inhalte anzuzeigen. Haben sie ein Abonnement bei einem Multichannel Video Programming Distributor (MVPD)? Wenn ja, beinhaltet dieses Abonnement Ihre Programmierung?

Die Bestimmung der Berechtigung eines Viewers ist für einen Programmierer nicht immer einfach. Die MVPDs verfügen über die identifizierenden Daten und Zugriffsberechtigungen für ihre Kunden.  Wenn man bedenkt, dass Zuschauer, die auf Ihre geschützten Inhalte zugreifen möchten, eine Vielzahl von MVPDs abonnieren, von denen jedes über verschiedene Systeme verfügt, wird schnell ersichtlich, dass die Bestimmung der Zuschauerberechtigung für geschützte Inhalte kompliziert und technisch schwierig werden kann:

![](../assets/user-ent-by-progr.png)

*Abbildung: Benutzerberechtigungen, die direkt vom Programmierer bestimmt werden*

Die Adobe Pass-Authentifizierung für TV Everywhere vermittelt diese Berechtigungstransaktionen sicher zwischen Programmierern und MVPDs. Mit der Adobe Pass-Authentifizierung können Programmierer bzw. Programmiererinnen geschützte Inhalte für gültige Kundinnen und Kunden einfach, schnell und sicher bereitstellen:

![](../assets/user-ent-mediatedby-authn.png)

*Abbildung: Benutzerberechtigung, vermittelt durch Adobe Pass-Authentifizierung*

Bei Austauschen mit teilnehmenden MVPDs fungiert die Adobe Pass-Authentifizierung als Proxy, sodass Sie Ihren Viewern eine konsistente Site-übergreifende Oberfläche bieten können. Mit der Adobe Pass-Authentifizierung können Sie Ihren Viewern auch Single-Sign-on (SSO)-Authentifizierung und -Autorisierung bereitstellen. Authentifizierung und Autorisierung werden für alle teilnehmenden Dienste verfolgt, sodass sich ein Abonnent nach der ersten Authentifizierung auf seinem eigenen System nicht erneut anmelden muss.

* **Authentifizierung** - Der Prozess, mit einer MVPD zu bestätigen, dass eine bestimmte Benutzerin oder ein bestimmter Benutzer eine bekannte Kundin oder ein bekannter Kunde ist.
* **Autorisierung** - Der Prozess, mit einem MVPD zu bestätigen, dass ein authentifizierter Benutzer über ein gültiges Abonnement für eine bestimmte Ressource verfügt.

### Funktionsweise der Adobe Pass-Authentifizierung {#HowItWorks}

Die Anwendung zum Anzeigen von Inhalten des Programmierers interagiert mit der Adobe Pass-Authentifizierung entweder über die Access Enabler-Client-Komponente oder die RESTful-Web-Services der Clientless-API (für nicht webfähige Geräte wie Smart-TVs, Spielekonsolen, Set-Top-Boxen usw.). Der Access Enabler wird auf dem System des Benutzers ausgeführt, wo er alle Berechtigungs-Workflows erleichtert.  Die Access Enabler-Komponente wird von der Hosting-Site auf Adobe heruntergeladen, wenn Kunden auf Ihre Site zugreifen und geschützte Inhalte anfordern.  Adobe Pass-Authentifizierungsserver hosten die in der Client-losen Lösung verwendeten RESTful-Webservices.

Die Adobe Pass-Authentifizierung verarbeitet die eigentlichen Berechtigungs-Workflows und stellt dabei Primitive bereit, die Sie für Folgendes verwenden:

* Identität festlegen. (Der Programmierer ist der „Anfragende“ im Berechtigungsfluss der Adobe Pass-Authentifizierung.)
* Authentifizieren Sie einen Benutzer mit einer bestimmten MVPD.  (Die MVPD ist der „Identitätsanbieter“ oder die „IDp“ im Berechtigungsfluss der Adobe Pass-Authentifizierung.)
* Autorisieren Sie einen Benutzer mit der MVPD für eine bestimmte Ressource.
* Melden Sie den Benutzer ab.

Der Programmierer ist für die übergeordnete Web-Seite oder Player-Anwendung verantwortlich, die Folgendes tut:

* Implementiert die Benutzeroberfläche
* Interagiert mit Access Enabler oder Client-losen API-Web-Services

Ziel der Adobe Pass-Authentifizierung ist es, eine einfache, modulare Methode für Programmierer und MVPDs zur Überprüfung von Berechtigungen zu schaffen.

## Grundlegendes zu Token {#understanding-tokens}

Die Adobe Pass-Lösung für Authentifizierungsberechtigungen konzentriert sich auf die Generierung bestimmter Datenelemente, die nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungs-Workflows erstellt werden. Diese Daten werden als Token bezeichnet. Token haben eine begrenzte Lebensdauer. Wenn sie ablaufen, müssen Token durch die erneute Initiierung der Authentifizierungs- und Autorisierungs-Workflows erneut ausgestellt werden.

Weitere Informationen zu Token finden Sie in den folgenden Abschnitten:

* [Typen von Token](#token-types)
* [Token-Speicher](#token-storage)
* [Token-Sicherheit](#token-security)
* [Token-Freigabe](#token-sharing)

### Typen von Token {#token-types}

Während der Authentifizierungs- und Autorisierungs-Workflows werden drei Arten von Token ausgegeben. Die AuthN- und AuthZ-Token sind „langlebig“ und bieten Kontinuität beim Anwendererlebnis. Das Medien-Token ist ein kurzlebiges Token, das die Best Practices der Branche unterstützt, um Betrug durch Stream-Ripping zu verhindern. Programmierer geben die Time-to-Live (TTL)-Werte für jeden Token-Typ basierend auf Vereinbarungen mit MVPDs an. Programmierer entscheiden sich für einen TTL-Wert, der Ihrem Unternehmen und Ihren Kunden am besten dient.

* **AuthN Token** („Langlebig„): Bei erfolgreicher Authentifizierung erstellt die Adobe Pass-Authentifizierung ein AuthN-Token, das sowohl dem anfragenden Gerät als auch einer global eindeutigen Kennung (GUID) zugeordnet ist.
   * Die Adobe Pass-Authentifizierung sendet das AuthN-Token an Access Enabler, der es sicher auf dem System des Clients zwischenspeichert.  Das AuthN-Token ist zwar vorhanden und nicht abgelaufen, ist aber für alle Anwendungen verfügbar, die die Adobe Pass-Authentifizierung verwenden. Der Access Enabler verwendet das Authentifizierungs-Token für den Autorisierungsfluss.
   * Es wird immer nur ein Authentifizierungs-Token zwischengespeichert. Wenn ein neues Authentifizierungs-Token ausgegeben wird und bereits ein altes vorhanden ist, überschreibt die Adobe Pass-Authentifizierung das zwischengespeicherte Token.
* **AuthZ Token** („Langlebig„): Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung ein AuthZ-Token, das mit dem anfragenden Gerät und einer bestimmten geschützten Ressource verknüpft ist.  Die geschützte Ressource wird durch eine eindeutige Ressourcen-ID identifiziert.
   * Die Adobe Pass-Authentifizierung sendet das AuthZ-Token an Access Enabler, der es sicher auf dem lokalen System zwischenspeichert. Der Access Enabler verwendet dann das AuthZ-Token, um das kurzlebige Medien-Token zu erstellen, das für den tatsächlichen Anzeigezugriff verwendet wird.
   * Es wird immer nur ein AuthZ-Token pro Ressource zwischengespeichert. Die Adobe Pass-Authentifizierung kann mehrere AuthZ-Token zwischenspeichern, sofern sie mit verschiedenen Ressourcen verknüpft sind. Wenn ein neues AuthZ-Token ausgegeben wird und bereits ein altes Token für dieselbe Ressource vorhanden ist, überschreibt die Adobe Pass-Authentifizierung das zwischengespeicherte Token.
* **Medien-Token** („kurzlebig„): Der Access Enabler verwendet das AuthZ-Token, um ein kurzlebiges (Standard: 7 Minuten) Medien-Token zu generieren. Dies ist der Punkt, an dem eine erfolgreiche Wiedergabeanforderung als erfolgt gilt.
   * Bevor Sie Zugriff auf die geschützte Ressource gewähren, muss Ihr Medienserver eine Adobe Pass-Authentifizierungskomponente, den Media Token Verifier, verwenden, um das Medien-Token zu validieren.
   * Da das Medien-Token nicht an das Gerät gebunden ist, ist seine Lebensdauer erheblich kürzer (Standard: 7 Minuten) als die der langlebigen AuthN- und AuthZ-Token.
   * Das kurzlebige Medien-Token ist auf die einmalige Verwendung beschränkt und wird nie zwischengespeichert. Sie wird jedes Mal vom Adobe Pass-Authentifizierungsserver abgerufen, wenn eine Autorisierungs-API aufgerufen wird.

### Token-Speicher {#token-storage}

Der Access Enabler speichert langlebige Token (AuthN und AuthZ) an umgebungsspezifischen Speicherorten:

* **Flash 10.1** (oder höher): Die langlebigen Token werden als lokale freigegebene Objekte gespeichert.
* **HTML5**: Die langlebigen Token werden sicher im lokalen Speicher des HTML5-Browsers aufbewahrt.
* **iOS**: Die langlebigen Token werden in einer persistenten Zwischenablage gespeichert, wo sie von anderen Client-Anwendungen für die Adobe Pass-Authentifizierung aufgerufen werden können.
* **Android**: Die langlebigen Token werden in einer freigegebenen Datenbankdatei gespeichert, wo sie von anderen Adobe Pass-Authentifizierungs-Client-Anwendungen aufgerufen werden können.
* **Client-lose API** Geräte: Token werden auf den Adobe Pass-Authentifizierungsservern gespeichert.

### Token-Sicherheit {#token-security}

Der Adobe Pass-Authentifizierungsserver signiert alle langlebigen Token digital mit der Geräte-ID (abgeleitet von den Hardwareeigenschaften des Geräts). Die digitale Signatur unterscheidet sich in der Art und Weise, wie sie generiert, geschützt und validiert wird, je nach Umgebung:

* **Flash 10.1** (oder höher) - Die Geräte-ID beruht auf den Geräteanmeldeinformationen, einem eindeutigen Zertifikat, das vom Adobe-Individualisierungsserver ausgestellt wird. Diese Sicherheit entspricht der FAXS DRM-Technologie. Bei dieser Server-seitigen Validierung wird die eindeutige Geräte-ID im Token mit den Geräteanmeldeinformationen verglichen (sicher von Flash Player an Adobe Pass-Authentifizierung übermittelt). Mit den Geräteanmeldeinformationen werden auch die FAXS-Client-Version und die Flash Player-Version (oder AIR) identifiziert, für die sie ausgegeben wurde. Die Gerätebindung ist stärker als bei HTML5, sodass die Time-to-Live (TTL) für Token beim Flash in der Regel länger ist.
* **HTML5** - Das Gerät ist Client-seitig individualisiert. Sie verwendet über JavaScript verfügbare Merkmale, um eine Pseudo-Geräte-ID zu erstellen, die Browser- und Betriebssystemversionen, eine IP-Adresse und eine Browser-Cookie-GUID (Globally Unique Identifier) enthält. Diese Token-Geräte-ID wird mit der aktuellen Pseudo-Geräte-ID für das Gerät verglichen. Da sich die IP-Adresse während der normalen Verwendung ändern kann, auch in derselben Sitzung, speichert die Adobe Pass-Authentifizierung HTML5-Token an zwei Orten: localStorage und sessionStorage. Wenn sich die IP ändert und das sessionStorage-Token ansonsten weiterhin gültig ist, wird die Sitzung gepflegt. Bei HTML5 ist die Gerätebindung nicht so stark, sodass die TTL für Token normalerweise kürzer ist als für Flash.
* **Native Clients** (iOS und Android) - Die langlebigen Token enthalten native Informationen zur Individualisierung der Geräte-ID und sind daher an das anfragende Gerät gebunden. Die Authentifizierungs- und Autorisierungsanfragen werden über HTTPS gesendet und die Geräte-ID-Informationen werden von der Access Enabler-Bibliothek digital signiert, bevor sie an die Backend-Server gesendet werden. Auf der Serverseite werden die Geräte-ID-Informationen anhand der zugehörigen digitalen Signatur überprüft.
* **Client-lose API-**: Die Client-lose API-Lösung verfügt über einen Satz von Sicherheitsprotokollen, die das digitale Signieren aller API-Aufrufe beinhalten. Token, die während der Berechtigungsflüsse generiert wurden, werden sicher auf den Adobe Pass-Authentifizierungsservern gespeichert.

Die Adobe Pass-Authentifizierung validiert jedes langlebige Token, um sicherzustellen, dass das Gerät, das auf den Inhalt zugreift, mit dem Gerät identisch ist, das das Token ausgestellt hat. Bei allen Token stellt eine Client-seitige Validierung sicher, dass die digitale Signatur intakt ist und dass die Integrität des Tokens erhalten bleibt. Wenn die Überprüfung der Geräte-ID fehlschlägt, wird die Authentifizierungssitzung ungültig gemacht, und der Benutzer wird aufgefordert, sich erneut anzumelden, wodurch die Token zurückgesetzt werden.

### Token-Freigabe {#token-sharing}

Anwendungen auf verschiedenen Plattformen geben keine Token frei. Dafür gibt es eine Reihe von Gründen, darunter die folgenden:

* Wie unter [Token-Speicher](#token-storage) beschrieben, variiert die Methode zum Speichern von Token zwischen Plattformen (z. B. Local Shared Objects for Flash, WebStorage for JavaScript).
* Der Grad der Token-Sicherheit ist je nach Plattform unterschiedlich. Flash-Token sind beispielsweise per FAX stark an ein Gerät gebunden. Token in einer JavaScript-Umgebung verfügen nicht über dieselbe DRM-Unterstützung wie in Flash.  Die Freigabe von JS-Token mit Flash-Anwendungen würde die Möglichkeit erhöhen, dass weniger sichere Token eine sicherere Umgebung nutzen.

## Lebenszyklus der Programmiererintegration {#prog-integ-lifecycle}

![](../assets/progr-flow-int-lifecycle.png)

*Abbildung: Integrieren der Authentifizierung mit der Website und Anwendung des Programmierers*


## Logische Flüsse {#logical-flows}

### Ablaufdiagramm für Berechtigungen {#chart}

Das folgende Diagramm zeigt den allgemeinen Prozess der Bestätigung von Berechtigungen (mithilfe der Client-Komponente &quot;Adobe Pass Authentication Access Enabler„):

![](../assets/ent-flowchart.png)

*Abbildung: Verfahren zur Bestätigung der Berechtigung*

### Authentifizierungsschritte {#authn-steps}

Die folgenden Schritte zeigen ein Beispiel für den Adobe Pass-Authentifizierungsfluss.  Dies ist der Teil des Berechtigungsprozesses, in dem ein Programmierer bestimmt, ob der Benutzer ein gültiger Kunde einer MVPD ist.  In diesem Szenario ist der Benutzer ein gültiger Abonnent einer MVPD.  Der/die Benutzende versucht, geschützte Inhalte mithilfe einer Flash-Anwendung eines Programmierers anzuzeigen:

1. Der/die Benutzende navigiert zur Programmierer-Webseite, auf der die Flash-Anwendung des/die Programmierers und die Adobe Pass Authentication Access Enabler-Komponenten auf den Computer des/der Benutzenden geladen werden. Die Flash-Anwendung verwendet Access Enabler, um die Identität des Programmierers mit der Adobe Pass-Authentifizierung festzulegen, und die Adobe Pass-Authentifizierung stellt dem Access Enabler die Konfigurations- und Statusdaten für diesen Programmierer (den „Anforderer„) zur Verfügung. Der Access Enabler muss diese Daten vom Server erhalten, bevor andere API-Aufrufe durchgeführt werden können. Technischer Hinweis: Der Programmierer legt seine Identität mit der `setRequestor()`-Methode des Access Enablers fest. Weitere Informationen finden Sie im [Programmierer-Integrationshandbuch](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md).
1. Wenn der Benutzer versucht, die geschützten Inhalte des Programmierers anzuzeigen, stellt das Programm des Programmierers dem Benutzer eine Liste von MVPDs zur Verfügung, aus denen der Benutzer einen Anbieter auswählt.
1. Der Benutzer wird an einen Adobe Pass-Authentifizierungsserver weitergeleitet, auf dem eine verschlüsselte [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language)-Anfrage für den vom Benutzer ausgewählten MVPD erstellt wird. Diese Anfrage wird als Authentifizierungsanfrage im Namen des Programmierers an MVPD gesendet. Abhängig vom MVPD-System wird der Browser des Benutzers entweder zur MVPD-Site weitergeleitet, um sich anzumelden, oder es wird ein Anmelde-iFrame in der App des Programmierers erstellt.
1. In beiden Fällen (Redirect oder iFrame) akzeptiert die MVPD die Anfrage und zeigt ihre Anmeldeseite an.
1. Der Benutzer meldet sich bei der MVPD an, die MVPD überprüft den Benutzerstatus als zahlender Kunde und die MVPD erstellt dann eine eigene HTTP-Sitzung.
1. Wenn der Benutzer validiert wird, erstellt MVPD eine Antwort (SAML und verschlüsselt), die der MVPD an die Adobe Pass-Authentifizierung zurücksendet.
1. Die Adobe Pass-Authentifizierung erhält die MVPD-Antwort, stellt fest, dass eine Adobe Pass-Authentifizierungs-HTTP-Sitzung geöffnet ist, validiert die SAML-Antwort von der MVPD und leitet zurück zur Website des Programmierers.
1. Die Website des Programmierers wird neu geladen, der Access Enabler wird neu geladen, und der Programmierer ruft setRequestor() erneut auf.  Der zweite Aufruf von setRequestor() ist erforderlich, da sich die aktuelle Konfiguration geändert hat. Jetzt ist ein Flag vorhanden, das dem Access Enabler mitteilt, dass ein Authentifizierungs-Token auf die Generierung auf dem Server wartet.
1. Der Access Enabler erkennt, dass eine Authentifizierung aussteht und fordert das Token vom Adobe Pass-Authentifizierungsserver an. Das Token wird vom Server abgerufen, indem die DRM-Funktionen der Flash Player aufgerufen werden.
1. Das AuthN-Token wird im Flash Player-LSO-Cache des Programmierers gespeichert. Die Authentifizierung ist jetzt abgeschlossen und die Sitzung wird auf dem Adobe Pass-Authentifizierungsserver zerstört.

### Autorisierungsschritte {#authz-steps}

Die folgenden Schritte werden im Abschnitt [Authentifizierungsschritte“ ](#authn-steps):

1. Wenn Benutzende versuchen, auf den geschützten Inhalt des Programmierers zuzugreifen, prüft die Anwendung des Programmierers zunächst auf dem lokalen Computer oder Gerät der Benutzenden auf ein AuthN-Token.  Wenn dieses Token nicht vorhanden ist, [ die oben genannten ](#authn-steps) befolgt.  Wenn das AuthN-Token vorhanden ist, fährt der Autorisierungsfluss mit dem Aufruf des Access Enabler durch das Programm des Programmierers fort, wobei die Anfrage gestellt wird, die Anzeigerechte des Benutzers für ein bestimmtes Element geschützten Inhalts abzurufen.
1. Das spezifische Element geschützter Inhalte wird durch eine „Ressourcenkennung“ dargestellt.  Dies kann eine einfache Zeichenfolge oder eine komplexere Struktur sein, aber in jedem Fall wird die Art der Ressourcenkennung vorab zwischen dem Programmierer und dem MVPD vereinbart.  Die Anwendung des Programmierers übergibt die Ressourcenkennung an den Access Enabler.  Der Access Enabler sucht auf dem lokalen Computer oder Gerät des Benutzers nach einem AuthZ-Token.  Wenn das AuthZ-Token nicht vorhanden ist, übergibt Access Enabler die Anfrage an den Backend-Adobe Pass-Authentifizierungsserver.
1. Der Adobe Pass-Authentifizierungsserver kommuniziert mithilfe standardisierter Protokolle mit dem MVPDs-Autorisierungsendpunkt.  Wenn die Antwort von MVPD anzeigt, dass die Benutzerin bzw. der Benutzer berechtigt ist, die geschützten Inhalte anzuzeigen, erstellt der Adobe Pass-Authentifizierungsserver ein AuthZ-Token und übergibt es zurück an Access Enabler, der das AuthZ-Token auf dem Computer der Benutzerin bzw. des Benutzers speichert.
1. Mit einem auf dem Computer oder Gerät des Benutzers gespeicherten AuthZ-Token ruft die Anwendung des Programmierers den Access Enabler auf, um ein Medien-Token vom Adobe Pass-Authentifizierungsserver zu erhalten, und stellt dieses Token der Anwendung des Programmierers zur Verfügung.
1. Schließlich verwendet die Anwendung des Programmierers die Komponente „Media Token Verifier“, um zu bestätigen, dass der richtige Benutzer den richtigen Inhalt anzeigt, und wenn das Medien-Token eingerichtet ist, kann der Benutzer den geschützten Inhalt anzeigen.


## Registrierung und Initialisierung {#reg-and-init}

Der erste Schritt bei der Arbeit mit der Adobe Pass-Authentifizierung besteht darin, sich bei Adobe oder einem von der Adobe Pass-Authentifizierung autorisierten Partner zu registrieren.

Bei der Registrierung geben Sie eine Liste der Domains an, von denen aus Sie mit der Adobe Pass-Authentifizierung kommunizieren. Die Domains des Turner Broadcasting Systems enthalten beispielsweise tbs.com und tnt.tv als authentifizierungsregistrierte Domains von Adobe Pass. Jede dieser Inhalts-Sites erhält Zugriff auf die Adobe Pass-Authentifizierung und erhält eine eigene Anforderer-ID (z. B. „TBS“ und „TNT„). Wenn Sie sich entscheiden, weitere Sites hinzuzufügen, müssen Sie Adobe über die zusätzlichen Domain-Namen informieren, damit sie in die Liste der zulässigen Domains aufgenommen werden und zusätzliche Anforderer-IDs erhalten.

>[!WARNING]
>
>Stellen Sie beim Testen der Integration sicher, dass sich Ihr Test-Server in einer registrierten Domain befindet, die Sie in der Produktion verwenden möchten. Anfragen, sogar Testanfragen, die von nicht auf der Zulassungsliste stehenden Domains stammen, werden ignoriert. Wenn Sie beispielsweise angefordert haben, domain.com in der Produktion zu verwenden, stellen Sie sicher, dass Sie Ihre Testintegration unter domain.com, test.domain.com und staging.domain.com bereitstellen.
>
>Anfragen, die den Benutzernamen und/oder das Passwort in der URL enthalten, werden ignoriert, selbst wenn Domains auf der Whitelist stehen. Beispiel: `//username@registered-domain/`

Die Anforderer-ID identifiziert den Client des Programmierers bei allen Kommunikationen mit der Access Enabler-Client-Komponente der Adobe Pass-Authentifizierung eindeutig. Alle dem Programmierer zugeordneten statischen Adobe Pass-Authentifizierungsdaten werden für diese ID verschlüsselt.

>[!TIP]
>
>Bei der Registrierung erhalten Sie neben Ihrer Anforderer-ID auch funktionale URLs für die Access Enabler-Client-Komponente und den Media Token Verifier.

## Integrationsschritte {#integration-steps}

>[!TIP]
>
>Wenn Sie die Adobe-Open Source Media Framework („OSMF„) für die Entwicklung Ihres Media Players verwenden, ist die schnellste Möglichkeit, die Adobe Pass-Authentifizierung zu verwenden, die OSMF-Plug-in-*(nicht mehr unterstützt* in den Code Ihres Players zu integrieren.
>
><!--For details, see [Adobe Pass Authentication Plugin For OSMF](https://tve.helpdocsonline.com/9-2-2) in the Programmer Integration Guide.-->

1. [Antragsteller-Setup](#requestor)
1. [Handhabung von Authentifizierung und Autorisierung](#authn-authz)
1. [Unterstützen von Single Logout](#ssl)

### 1. Einrichtung des Antragstellers {#requestor}

#### 1a. Registrieren bei Adobe

Zunächst müssen Sie sich bei Adobe oder einem autorisierten Adobe Pass-Authentifizierungspartner registrieren.  Bei der Registrierung erhalten Sie einen oder mehrere GUIDs (Globally Unique Identifiers). Jede GUID, die Sie erstellen, ist mit einer Domain verknüpft, von der aus der Zugriff auf die Adobe Pass-Authentifizierung zulässig ist. Sie übergeben eine GUID (die als Anforderer-ID bezeichnet wird) für die anfragende Domain, um Ihre Identität für jede Sitzung zu registrieren, in der Sie mit dem Access Enabler interagieren. Weitere Informationen finden Sie unter [Registrierung und Initialisierung](#reg-and-init) im Handbuch zur Programmiererintegration.

#### 1b. Integration von Initial Access Enabler

Der nächste Schritt besteht darin, den Access Enabler in Ihre vorhandene Media Player-App oder Web-Seite zu integrieren:

* Sie können die Flash-Version `AccessEnabler.swf` in einen Flash-basierten Videoplayer einbetten oder direkt in die HTML Ihrer Webseite einbetten. Sie können mit der Access Enabler-SWF entweder auf ActionScript oder in JavaScript kommunizieren. Die Basis-API ist ActionScript. Wenn Sie jedoch lieber mit JavaScript arbeiten, steht eine vollständige Wrapper-Bibliothek zur Verfügung, die Sie in Ihre Seiten aufnehmen können.
* Für Nicht-Flash-Umgebungen haben Sie folgende Möglichkeiten:
   * Verwenden Sie die HTML5-/JavaScript-Version, AccessEnabler.js, und kommunizieren Sie über die JavaScript-API mit ihr
   * Verwenden Sie die native AIR-Erweiterung für die Adobe Pass-Authentifizierung, um nativen Code mit integrierten ActionScript-Klassen zu kombinieren
   * Verwenden Sie eine der nativen Client-Versionen der Access Enabler-Bibliothek (iOS oder Android).

### 2. Handhabung von Authentifizierung und Autorisierung {#authn-authz}

#### 2a. Kommunikation mit dem Access Enabler

Die Kommunikation zwischen dem Access Enabler und Ihrer Web-Seite oder Player-App ist asynchron. Ihre Anwendung ruft Access Enabler-Methoden auf, und der Access Enabler antwortet über Callbacks, die Sie bei der Access Enabler-Bibliothek registrieren.

* Wenn Ihre Anwendung eine Autorisierungsanfrage stellt, initiiert Access Enabler automatisch eine Authentifizierungsanfrage, wenn sich noch kein gültiges Authentifizierungs-Token auf dem lokalen System befindet. Wenn die Authentifizierung erfolgreich ist, wird das Authentifizierungs-Token des Benutzers lokal gespeichert, sodass er sich nicht erneut anmelden muss. Wenn sie sich über die Adobe Pass-Authentifizierung in einem anderen Kontext erfolgreich authentifiziert haben (z. B. über die MVPD-Website oder mit einem anderen Programmierer), hat der Access Enabler Zugriff auf das lokale Authentifizierungstoken, und eine zusätzliche Authentifizierung ist nicht erforderlich.
* Wenn ein(e) Benutzende(r) versucht, auf Ihre geschützten Inhalte zuzugreifen, senden Sie eine Autorisierungsanfrage an Access Enabler. Nach der Überprüfung (oder Initiierung) der Authentifizierung kontaktiert Access Enabler die MVPD (über den Adobe Pass-Authentifizierungsserver), um festzustellen, ob der Kunde berechtigt ist, die geschützten Inhalte anzuzeigen. Ihre Anwendung muss nur die Anfrage an den Access Enabler senden und dann die Antwort verarbeiten (Autorisierungserfolg oder -fehler). Wenn die Autorisierung erfolgreich ist, wird ein AuthZ-Token auf dem Client-System gespeichert.  Schließlich erhält Ihre Anwendung ein kurzlebiges Medien-Token zur Verwendung in Ihrem eigenen Autorisierungsverfahren.

>[!NOTE]
>
>* Die Authentifizierung erfolgt als SAML-Austausch zwischen Adobe Pass Authentication as the Service Provider (SP) und MVPD as the Identity Provider (IDp).
>
>* Bei der Autorisierung wird ein Backchannel (Server-zu-Server)-Webservice-Austausch zwischen der Adobe Pass-Authentifizierung (dem SP) und einem MVPD (dem IDp) verwendet.


#### 2b. Bereitstellen einer Benutzeroberfläche für Berechtigungen {#entitlement-ui}

Sie stellen eine eigene Benutzeroberfläche für den Benutzerzugriff auf Ihre Inhalte bereit. Einige Elemente, z. B. der eigentliche Anmeldeprozess, werden von MVPD bereitgestellt. Einige Elemente sind optional als Teil der Adobe Pass-Authentifizierung verfügbar. Sie haben mindestens folgende Möglichkeiten:

* **Implementieren Sie eine MVPD-Auswahlschnittstelle, über die neue Benutzende ihre MVPD identifizieren und sich erstmals anmelden können**. Für die Entwicklung bietet Access Enabler eine grundlegende Benutzeroberfläche, die dem Kunden eine Auswahl an MVPDs gibt und den Anmeldeprozess initiiert. Für die Produktion müssen Sie Ihr eigenes MVPD-Selektor-Dialogfeld implementieren. Einige MVPDs werden zur Anmeldung auf ihre eigene Website umgeleitet, andere erfordern, dass ihre Anmeldeseiten in einem iFrame angezeigt werden. Sie müssen einen Callback implementieren, der diesen iFrame erstellt, um die Fälle zu handhaben, in denen die MVPD des Benutzers seine Anmeldeseite in einem iFrame anzeigt.
* **Identifizieren geschützter Inhalte**. Geschützte Inhalte erfordern eine Zugriffsberechtigung. Ihre Benutzeroberfläche sollte angeben, welche Inhalte geschützt sind und welche Inhalte autorisiert wurden.  Der Autorisierungsstatus wird oft mit Symbolen „entsperrt“ und „gesperrt“ angezeigt.
* **Anzeigen, dass ein Benutzer authentifiziert ist**. Sie sollten den Authentifizierungsstatus eines Benutzers als Teil der Mittel angeben, die Sie zum Identifizieren geschützter Inhalte verwenden. Sie können den Access Enabler abfragen, um festzustellen, ob der Kunde bereits authentifiziert wurde.

#### 2c. Integrieren von Media Token Verifier {#int-media-token-ver}

Sie müssen die Komponente Media Token Verifier für die Adobe Pass-Authentifizierung in Ihren Medienserver integrieren.  Auf diese Weise kann Ihr vorhandener Token-Verifizierer die von der Adobe Pass-Authentifizierung bereitgestellten kurzlebigen Medien-Token bei erfolgreicher Autorisierung erkennen. Der Media Token Verifier validiert die Medien-Token als letzten Sicherheitsschritt, bevor Sie den Benutzenden Zugriff auf geschützte Inhalte gewähren. Sie erhalten den Speicherort, von dem Sie den Media Token Verifier herunterladen können, wenn Sie sich bei Adobe registrieren.

### 3. Unterstützen von Single Logout {#ssl}

In den meisten Fällen ist Ihr Medien-Player für die Verarbeitung von Benutzerabmeldungen über eine einfache Access Enabler-API verantwortlich. Wenn Sie logout() aufrufen, führt der Access Enabler Folgendes aus:

* Meldet den aktuellen Benutzer ab
* Löscht alle Authentifizierungs- und Autorisierungsinformationen für den abgemeldeten Benutzer
* Löscht alle AuthN- und AuthZ-Token aus dem lokalen System des Benutzers

Wenn der Benutzer den Computer lange genug inaktiv lässt, damit seine Token ablaufen, kann der Benutzer weiterhin zur Sitzung zurückkehren und die Abmeldung erfolgreich initiieren. Die Adobe Pass-Authentifizierung stellt sicher, dass alle Token gelöscht werden, und benachrichtigt die MVPD, auch ihre Sitzung zu löschen.

Wenn die Abmeldung von einer Site initiiert wird, die nicht in die Adobe Pass-Authentifizierung integriert ist, kann die MVPD den Adobe Pass-Authentifizierungsdienst für die einmalige Abmeldung über eine Browser-Umleitung aufrufen.

## Grundlegendes zu Benutzer-IDs {#user-ids}

Konzeptionell wird jeder Benutzer, der einen Berechtigungsfluss initiiert, mit einer einzelnen, eindeutigen Benutzer-ID verknüpft.  Im Verlauf eines Berechtigungsflusses kann diese eine Benutzer-ID jedoch auf unterschiedliche Weise dargestellt werden, je nachdem, von welcher API Sie die ID erhalten.

Die sessionGUID im Short Media Token ist die sichere Form der UserID, die über den sendTrackingData()-Aufruf verfügbar ist.   Bei allen aktuellen Integrationen ist dies eine persistente GUID für den Benutzer über Zeit und Geräte hinweg, aber die Quelle der GUID beginnt mit der Benutzer-ID in der SAML-Antwort von MVPD.   Einige MVPDs könnten jedoch in Zukunft ihre Meinung ändern und eine vorübergehende GUID senden.  Wenn ein Programmierer sicherstellen möchte, dass die MVPD-Quell-Benutzer-ID in der AuthN-Antwort persistent ist, sollte er dies in seinen Vereinbarungen mit MVPDs arrangieren.

Im Folgenden werden die verschiedenen Arten der Darstellung der Benutzer-ID in den Adobe Pass-Authentifizierungs-APIs beschrieben:

* `sendTrackingData()` GUID-Eigenschaft - Dies ist die Adobe-Hash-Version der MVPD UserID.  Sie wird gehasht, sodass diese Benutzer-ID nicht von der MVPD zur Quelle zurückverfolgt werden kann.   Diese ID ist eindeutig und normalerweise persistent, kann jedoch nicht mit der MVPD geteilt werden, um ein bestimmtes Nutzungsverhalten mit dem zu vergleichen, was MVPDs auf ihrer Seite haben.   Es ist nicht digital signiert, also nicht uneinsehbar für die Betrugsbekämpfung, aber es ist gut genug für Analysen.  Diese Form der Benutzer-ID wird Client-seitig für alle Ereignisse bereitgestellt, die von der Adobe Pass-Authentifizierung im AuthN-/AuthZ-Fluss generiert werden.
* Die `sessionGUID`-Eigenschaft des Short Media Token - diese ist mit der Benutzer-ID über `sendTrackingData()` identisch, diese ist jedoch digital signiert, um ihre Integrität zu schützen.  Dadurch ist dieser Wert gut genug, um Betrug und gleichzeitige Nutzung zu verfolgen. Es ist für die Verarbeitung auf der Server-Seite nach Verwendung unserer Validator-Bibliothek vorgesehen und kann auf Betrugsmuster analysiert werden, bevor der Video-Stream an den Client freigegeben wird.  Die Ausführung einer dieser Aufgaben liegt beim Programmierer.
* `getMetadata() userID `property - Diese Eigenschaft ermöglicht dem Adobe das Anzeigen der tatsächlichen Quell-MVPD-Benutzer-ID beim Programmierer. Sie wird mit dem öffentlichen Schlüssel aus dem Zertifikat, das wir vom Programmierer haben, verschlüsselt, sodass sie für den Client nicht in der unverschlüsselten Datei verfügbar ist. Dadurch erhält der Programmierer die tatsächliche Benutzer-ID aus der MVPD, sodass sie direkt mit der MVPD für die Kontoverknüpfung oder Betrugsermittlung verwendet werden kann.

**Zum Schluss**

* Die MVPD-Benutzer-ID ist eine im Allgemeinen, jedoch nicht garantierte, beständige eindeutige ID, die **aus den MVPDs generiert und bei erfolgreicher Authentifizierung an Adobe übergeben**. Sie ist mit einigen Ausnahmen im Allgemeinen über alle Netzwerke hinweg konsistent.
* Die MVPD-Benutzer-ID enthält keine personenbezogenen Daten und ist KEINE Kontonummer. Es muss nicht in verschlüsselter Form verfügbar gemacht werden, da wir mit allen MVPDs validiert haben, dass keine PII gesendet werden.


Wie Sie die Benutzer-ID verwenden, hängt vom Anwendungsfall ab:

* Wenn Sie es für Tracking/Analysen benötigen, ist der praktischste Ort, es von `sendTrackingData()` zu erhalten.
* Wenn Sie sie Server-seitig für Stream-Freigabe, Betrug oder Betriebsdaten benötigen, können Sie sie vom Media Token Validator erhalten.
* Wenn Sie es für Kontoverknüpfung und tieferen Betrug benötigen, fragen Sie Ihren Adobe-Ansprechpartner nach der Verfügbarkeit.

<!--
>[!RELATEDINFORMATION]
>
>* **Kickstart Guides** These guides explain the initial steps to take once you have decided to begin integrating with Adobe Pass Authentication.
>* **Programmer Integration Guide** This is a lower level technical guide for Programmers, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>* [Overview For MVPDs](/help/authentication/mvpd-overview.md) This provides a similar level of conceptual information as in this Programmer overview, but is directed toward MVPDs.
-->
