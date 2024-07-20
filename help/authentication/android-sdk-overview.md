---
title: Übersicht über das Android SDK
description: Übersicht über das Android SDK
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '2731'
ht-degree: 0%

---

# Übersicht über das Android SDK {#android-sdk-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#intro}

Android AccessEnabler ist eine Java Android-Bibliothek, mit der mobile Apps die Adobe Pass-Authentifizierung für die Berechtigungsdienste von TV Anywhere verwenden können. Eine Android-Implementierung besteht aus der AccessEnabler-Schnittstelle, die die Berechtigungs-API definiert, und einem EntitlementDelegate-Protokoll, das die Rückrufe beschreibt, die die Bibliotheks-Trigger ausführen. Die Schnittstelle wird zusammen mit dem Protokoll unter einem gemeinsamen Namen genannt: die AccessEnabler Android-Bibliothek.

## Android-Anforderungen {#reqs}

Aktuelle technische Anforderungen in Bezug auf die Android-Plattform und die Adobe Pass-Authentifizierung finden Sie unter [Anforderungen an Plattform/Gerät/Tool](#android) oder in den Versionshinweisen zum Android SDK-Download.

## Grundlegendes zu nativen Client-Workflows {#native_client_workflows}

Native Client-Workflows sind in der Regel mit denen von browserbasierten Adobe Pass-Authentifizierungs-Clients identisch oder sehr ähnlich. Es gibt jedoch einige Ausnahmen, wie unten beschrieben.

- [Nachinitialisierungs-Workflow](#post-init)
- [Allgemeiner Workflow für die anfängliche Authentifizierung](#generic)
- [Workflow abmelden](#logout)

### Nachinitialisierungs-Workflow {#post-init}

Bei allen vom AccessEnabler unterstützten Berechtigungs-Workflows wird davon ausgegangen, dass Sie zuvor [`setRequestor()`](#setRequestor) aufgerufen haben, um Ihre Identität zu ermitteln. Sie führen diesen Aufruf aus, um Ihre Anforderer-ID nur einmal bereitzustellen, normalerweise während der Initialisierungs-/Setup-Phase Ihrer Anwendung.


Bei den nativen Clients (z. B. Android) haben Sie nach dem ersten Aufruf von [`setRequestor()`](#setRequestor) die Wahl, wie Sie vorgehen sollen:

- Sie können sofort mit Berechtigungsaufrufen beginnen und diese bei Bedarf still in die Warteschlange stellen.

- Alternativ können Sie eine Bestätigung des Erfolgs/Fehlschlagens von [`setRequestor()`](#setRequestor) erhalten, indem Sie den Rückruf setRequestorComplete() implementieren.

- Oder machen Sie beides.

Sie können entscheiden, ob Sie auf die Benachrichtigung über den Erfolg von [`setRequestor()`](#setRequestor) warten oder sich auf den Warteschlangenmechanismus von AccessEnabler verlassen möchten. Da alle nachfolgenden Autorisierungs- und Authentifizierungsanfragen die Anforderungs-ID und die zugehörigen Konfigurationsinformationen benötigen, blockiert die [`setRequestor()`](#setRequestor) -Methode effektiv alle Authentifizierungs- und Autorisierungs-API-Aufrufe, bis die Initialisierung abgeschlossen ist.

### Allgemeiner Workflow für die anfängliche Authentifizierung {#generic}

Dieser Workflow dient der Anmeldung eines Benutzers mit seinem MVPD.  Nach erfolgreicher Anmeldung gibt der Backend-Server dem Benutzer ein Authentifizierungstoken aus. Während die Authentifizierung in der Regel im Rahmen des Autorisierungsprozesses erfolgt, wird im Folgenden beschrieben, wie die Authentifizierung isoliert funktioniert, und es sind keine Autorisierungsschritte enthalten.

Beachten Sie, dass sich der folgende native Client-Workflow vom typischen browserbasierten Authentifizierungs-Workflow unterscheidet, die Schritte 1 bis 5 jedoch für native Clients und browserbasierte Clients gleich sind:

1. Ihre Seite oder Ihr Player initiiert den Authentifizierungs-Workflow mit einem Aufruf von [getAuthentication()](#getAuthN) , der nach einem gültigen zwischengespeicherten Authentifizierungstoken sucht. Diese Methode weist einen optionalen Parameter `redirectURL` auf. Wenn Sie keinen Wert für `redirectURL` angeben, wird der Benutzer nach einer erfolgreichen Authentifizierung an die URL zurückgegeben, von der aus die Authentifizierung initialisiert wurde.
1. Der AccessEnabler bestimmt den aktuellen Authentifizierungsstatus. Wenn der Benutzer derzeit authentifiziert ist, ruft AccessEnabler Ihre `setAuthenticationStatus()` -Rückruffunktion auf und übergibt einen Authentifizierungsstatus, der auf Erfolg hinweist (Schritt 7 unten).
1. Wenn der Benutzer nicht authentifiziert ist, setzt der AccessEnabler den Authentifizierungsfluss fort, indem er bestimmt, ob der letzte Authentifizierungsversuch des Benutzers mit einem bestimmten MVPD erfolgreich war. Wenn eine MVPD-ID zwischengespeichert wird UND das `canAuthenticate`-Flag wahr ist ODER ein MVPD mit [`setSelectedProvider()`](#setSelectedProvider) ausgewählt wurde, wird der Benutzer nicht mit dem MVPD-Auswahldialogfeld aufgefordert. Der Authentifizierungsfluss wird mit dem zwischengespeicherten Wert des MVPD fortgesetzt (d. h. mit dem MVPD, der bei der letzten erfolgreichen Authentifizierung verwendet wurde). Ein Netzwerkaufruf erfolgt an den Backend-Server und der Benutzer wird zur MVPD-Anmeldeseite weitergeleitet (Schritt 6 unten).
1. Wenn keine MVPD-ID zwischengespeichert wird UND kein MVPD mit [`setSelectedProvider()`](#setSelectedProvider) ausgewählt wurde ODER das Flag `canAuthenticate` auf &quot;false&quot;gesetzt ist, wird der Rückruf [`displayProviderDialog()`](#displayProviderDialog) aufgerufen. Dieser Rückruf leitet Ihre Seite oder Ihren Player an, um die Benutzeroberfläche zu erstellen, über die der Benutzer eine Liste mit MVPDs zur Auswahl hat. Es wird ein Array von MVPD-Objekten bereitgestellt, die die erforderlichen Informationen zum Erstellen des MVPD-Selektors enthalten. Jedes MVPD-Objekt beschreibt eine MVPD-Entität und enthält Informationen wie die Kennung des MVPD (z. B. XFINITY, AT\&amp;T usw.) und die URL, unter der das MVPD-Logo zu finden ist.
1. Sobald ein bestimmter MVPD ausgewählt ist, muss Ihre Seite oder Ihr Player den AccessEnabler über die Auswahl des Benutzers informieren. Bei Nicht-Flash-Clients informieren Sie den AccessEnabler über die Benutzerauswahl, sobald der Benutzer den gewünschten MVPD auswählt, über einen Aufruf der [`setSelectedProvider()`](#setSelectedProvider) -Methode. Flash-Clients senden stattdessen eine freigegebene `MVPDEvent` vom Typ &quot;`mvpdSelection`&quot;, wobei der ausgewählte Provider übergeben wird.
1. Wenn für Android-Anwendungen com.android.chrome verfügbar ist, wird die Authentifizierungs-URL in benutzerdefinierte Chrome-Registerkarten geladen.
1. Über benutzerdefinierte Chrome-Registerkarten gelangt der Benutzer auf die Anmeldeseite des MVPD und gibt seine Anmeldeinformationen ein. Beachten Sie, dass während dieser Übertragung mehrere Umleitungsvorgänge auftreten.
1. Wenn benutzerdefinierte Chrome-Registerkarten erkennen, dass eine URL mit dem Schema (adobepass://) und dem Deep-Link aus der Ressource &quot;redirect\_uri&quot;(d. h. adobepass://com.adobepass ) übereinstimmt, ruft der AccessEnabler das tatsächliche Authentifizierungstoken von den Backend-Servern ab. Beachten Sie, dass die endgültigen Umleitungs-URLs ungültig sind und nicht für benutzerdefinierte Chrome-Registerkarten zum Laden vorgesehen sind. Sie dürfen vom SDK nur als Signal interpretiert werden, dass der Authentifizierungsvorgang abgeschlossen ist.
1. Der AccessEnabler informiert Ihre Anwendung darüber, dass der Authentifizierungsvorgang abgeschlossen ist. Der AccessEnabler ruft den Rückruf [`setAuthenticationStatus()`](#setAuthNStatus) mit dem Status-Code 1 auf und zeigt den Erfolg an. Wenn bei der Ausführung dieser Schritte ein Fehler auftritt, wird der Rückruf [`setAuthenticationStatus()`](#setAuthNStatus) mit dem Statuscode 0 und einem entsprechenden Fehlercode ausgelöst, der auf einen Authentifizierungsfehler hinweist.

### Workflow abmelden {#logout}

Bei nativen Clients werden Logouts ähnlich wie der oben beschriebene Authentifizierungsprozess verarbeitet. Auf diese Weise öffnen AccessEnabler benutzerdefinierte Chrome-Registerkarten und laden die URL des Abmelde-Endpunkts auf den Backend-Server.



**Hinweis:** Durch Abmelden von einer Programmierer-/MVPD-Sitzung wird die
den zugrunde liegenden Speicher für diesen spezifischen MVPD, einschließlich des gesamten
andere Programmierer-Authentifizierungstoken, die über SSO abgerufen werden
das Gerät. Token, die für andere MVPDs oder nicht über SSO erhalten werden, werden nicht
gelöscht werden.


## Token {#tokens}

- [Definitionen und Verwendung](#definitions)
- [Caching-Richtlinien](#caching)
- [Persistenz](#persistence)
- [Format](#format)
- [Gerätebindung](#device_binding)



### Definitionen und Verwendung {#definitions}

Die Berechtigungslösung für die Adobe Pass-Authentifizierung umfasst die Generierung bestimmter Datenteile (Token), die die Adobe Pass-Authentifizierung nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungsarbeitsabläufe generiert. Diese Token werden lokal auf dem Android-Gerät des Clients gespeichert.

Token haben eine begrenzte Lebensdauer. Nach Ablauf der Gültigkeit müssen Token erneut ausgestellt werden, indem die Authentifizierungs- und/oder Autorisierungs-Workflows neu gestartet werden.

Während der Berechtigungs-Workflows werden drei Typen von Token ausgegeben:

- **Authentifizierungstoken** - Das Endergebnis des Workflows zur Benutzerauthentifizierung ist eine Authentifizierungs-GUID, die der AccessEnabler verwenden kann, um Autorisierungsabfragen im Namen des Benutzers durchzuführen. Diese Authentifizierungs-GUID weist einen zugehörigen TTL-Wert (Time-to-Live) auf, der sich von der Authentifizierungssitzung des Benutzers selbst unterscheiden kann. Die Adobe Pass-Authentifizierung generiert ein Authentifizierungstoken, indem sie die Authentifizierungs-GUID an das Gerät bindet, das die Authentifizierungsanfragen initiiert.
- **Autorisierungstoken** - Gewährt Zugriff auf eine bestimmte geschützte Ressource, die durch eine eindeutige `resourceID` identifiziert wird. Es handelt sich dabei um eine von der autorisierenden Partei erteilte Ermächtigung zusammen mit dem Original `resourceID`. Diese Informationen sind an das Gerät gebunden, das die Anfrage initiiert.
- **Kurzlebige Medien-Token** - AccessEnabler gewährt Zugriff auf die Hosting-Anwendung für eine bestimmte Ressource, indem ein kurzlebiges Medien-Token zurückgegeben wird. Dieses Token wird basierend auf dem Autorisierungstoken generiert, das zuvor für diese bestimmte Ressource erworben wurde. Außerdem ist dieses Token nicht an das Gerät gebunden und die zugehörige Lebensdauer ist deutlich kürzer (Standard: 5 Minuten).

Nach erfolgreicher Authentifizierung und Autorisierung stellt die Adobe Pass-Authentifizierung Authentifizierungs-, Autorisierungs- und kurzlebige Medien-Token aus. Diese Token sollten auf dem Gerät des Benutzers zwischengespeichert und für die Dauer der zugehörigen Lebensdauer verwendet werden.



### Caching-Richtlinien {#caching}

- Authentifizierungstoken
- Autorisierungstoken
- Medien-Token


#### Authentifizierungstoken

- **AccessEnabler 1.6 und älter** - Die Art und Weise, wie Authentifizierungstoken auf dem Gerät zwischengespeichert werden, hängt von der &quot;**Authentifizierung pro Anforderer&quot;** -Markierung ab, die mit dem aktuellen MVPD verknüpft ist:


1. Wenn die Funktion &quot;Authentifizierung pro Anforderer&quot;auf *disabled* gesetzt ist, wird ein einzelnes Authentifizierungstoken lokal auf der globalen Zwischenablage gespeichert. Dieses Token wird für alle Anwendungen freigegeben, die in das aktuelle MVPD integriert sind.
1. Wenn die Funktion &quot;Authentifizierung pro Anforderer&quot;auf *enabled* gesetzt ist, wird ein Token explizit mit dem Programmierer verknüpft, der den Authentifizierungsfluss durchgeführt hat (das Token wird nicht in der globalen Zwischenablage, sondern in einer privaten Datei gespeichert, die nur für die Anwendung dieses Programmierers sichtbar ist). Genauer gesagt wird Single Sign-On (SSO) zwischen verschiedenen Anwendungen deaktiviert. Der Benutzer muss den Authentifizierungsfluss beim Wechsel zu einer neuen App explizit durchführen (vorausgesetzt, der Programmierer der zweiten App ist mit dem aktuellen MVPD integriert und für diesen Programmierer ist kein Authentifizierungstoken im lokalen Cache vorhanden).

   **Hinweis:** AEM 1.6 Google GSON Tech Hinweis: [So lösen Sie DSGVO-Abhängigkeiten auf](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** - Mit diesem SDK wird eine neue Methode der Tokenspeicherung eingeführt, die mehrere Programmer-MVPD-Buckets und damit mehrere Authentifizierungstoken ermöglicht. Ab AEM 1.7 wird dasselbe Speicherlayout sowohl für das Szenario &quot;Authentifizierung pro Anforderer&quot;als auch für den normalen Authentifizierungsfluss verwendet. Der einzige Unterschied zwischen den beiden besteht in der Art und Weise, wie die Authentifizierung durchgeführt wird: &quot;Authentifizierung pro Anforderer&quot;enthält eine neue Verbesserung (passive Authentifizierung), die es dem AccessEnabler ermöglicht, eine Back-Channel-Authentifizierung durchzuführen, basierend auf dem Vorhandensein eines Authentifizierungstokens im Speicher (für einen anderen Programmierer). Der Benutzer muss sich nur einmal authentifizieren. Diese Sitzung wird verwendet, um Authentifizierungstoken in nachfolgenden Apps zu erhalten. Dieser Backchannel-Fluss erfolgt während des [`setRequestor()`](#setRequestor) -Aufrufs und ist größtenteils für den Programmierer transparent. Es gibt jedoch eine wichtige Anforderung: Der Programmierer MUSS [`setRequestor()`](#setRequestor) aus dem Haupt-UI-Thread und aus einer Aktivität aufrufen.


#### Autorisierungstoken

Jederzeit wird nur ein ONE-Autorisierungstoken pro Ressource vom AccessEnabler zwischengespeichert. Es können mehrere Autorisierungstoken zwischengespeichert werden, sie sind jedoch mit verschiedenen Ressourcen verknüpft. Wenn ein neues Autorisierungstoken ausgegeben wird und bereits ein altes für dieselbe Ressource existiert, überschreibt das neue Token den vorhandenen zwischengespeicherten Wert.



#### Medien-Token

Das Token für kurzlebige Medien sollte überhaupt NICHT zwischengespeichert werden. Das Medien-Token sollte jedes Mal, wenn eine Autorisierungs-API aufgerufen wird, vom Server abgerufen werden, da es auf die einmalige Verwendung beschränkt ist.



### Persistenz {#persistence}

Token müssen in aufeinander folgenden Ausführungen derselben Anwendung persistent sein. Das bedeutet, dass nach dem Erwerb der Authentifizierungs- und Autorisierungstoken und dem Schließen der Anwendung durch den Benutzer dieselben Token für die Anwendung verfügbar sind, wenn der Benutzer die Anwendung erneut öffnet. Darüber hinaus ist es wünschenswert, dass diese Token über mehrere Anwendungen hinweg persistent sind. Anders ausgedrückt: Wenn ein Benutzer eine Anwendung verwendet, um sich bei einem bestimmten Identitätsanbieter anzumelden (um Authentifizierungs- und Autorisierungstoken erfolgreich zu erhalten), können dieselben Token über eine andere Anwendung verwendet werden. Der Benutzer wird bei der Anmeldung über denselben Identitätsanbieter nicht mehr zur Eingabe von Anmeldeinformationen aufgefordert.



Dieser nahtlose Authentifizierungs-/Autorisierungs-Workflow macht die Adobe Pass-Authentifizierungslösung zu einer echten TV-Überall-Implementierung. Aus rein technischer Sicht umgeht die Android AccessEnabler-Bibliothek die Probleme der anwendungsübergreifenden Datenfreigabe, indem sie die Token-Daten in einer Datenbankdatei speichert, die sich im externen Speicher befindet. Diese freigegebenen Ressourcen auf Systemebene bieten die wichtigsten Komponenten, die die Implementierung des gewünschten Anwendungsfalls für beständige Token ermöglichen:

- Unterstützung für strukturierten Speicher - Der Speicher des Adobe Pass-Authentifizierungstokens ist nicht nur eine einfache lineare Pufferspeicherstruktur. Es bietet einen wörterbuchähnlichen Speichermechanismus, der eine Datenindizierung basierend auf benutzerdefinierten Schlüsselwerten ermöglicht.
- Unterstützung der Datenpersistenz mithilfe des zugrunde liegenden Dateisystems - Der Inhalt der Datenbankdatei wird standardmäßig beibehalten und die Daten werden im externen Speicher des Geräts gespeichert.



Sobald ein bestimmtes Token im Token-Cache platziert wurde, wird seine Gültigkeit von der AccessEnabler-Bibliothek zu unterschiedlichen Zeitpunkten überprüft.  Ein gültiges Token wird wie folgt definiert:

- Die TTL des Tokens ist nicht abgelaufen
- Der Aussteller des Tokens ist in der Liste der zugelassenen Identitätsanbieter enthalten



Ab AccessEnabler 1.7 kann der Token-Speicher mehrere Programmer-MVPD-Kombinationen unterstützen, die auf einer mehrstufigen verschachtelten Kartenstruktur basieren, die mehrere Authentifizierungstoken enthalten kann. Dieser neue Speicher wirkt sich in keiner Weise auf die öffentliche AccessEnabler-API aus und erfordert keine Änderungen auf der Seite des Programmierers. Im Folgenden finden Sie ein Beispiel, das diese neuere Funktion veranschaulicht:

1. Öffnen Sie App1 (entwickelt von Programmer1).
1. Authentifizieren Sie sich mit MVPD1 (integriert in Programmer1).
1. Aussetzen/Schließen Sie die aktuelle Anwendung und öffnen Sie App2 (entwickelt von Programmer2).
1. Nehmen wir an, dass Programmer2 nicht in MVPD2 integriert ist. Daher wird der Benutzer NICHT in App2 authentifiziert.
1. Authentifizieren Sie sich mit MVPD2 (das mit Programmer2 integriert ist) in App2.
1. Wechseln Sie zurück zu App1. Der Benutzer wird weiterhin mit Programmer1 authentifiziert.

In älteren Versionen von AccessEnabler würde Schritt 6 den Benutzer als nicht authentifiziert rendern, da der Token-Speicher zuvor nur ein Authentifizierungstoken unterstützte.


**HINWEIS:** Durch Abmelden von einer Programmer-/MVPD-Sitzung wird der zugrunde liegende Speicher gelöscht, einschließlich aller anderen Programmierer-Authentifizierungstoken auf dem Gerät mit SSO. Token, die für andere MVPDs oder nicht über SSO erhalten wurden, werden nicht gelöscht. Durch Abbruch des Authentifizierungsflusses (Aufrufen von [`setSelectedProvider(null)`](#setSelectedProvider)) wird der zugrunde liegende Speicher NICHT gelöscht, sondern nur der aktuelle Programmierer/MVPD-Authentifizierungsversuch (durch Löschen des MVPD für den aktuellen Programmierer).


Eine weitere speicherbezogene Funktion, die in AccessEnabler 1.7 enthalten ist, ermöglicht den Import von Authentifizierungstoken aus älteren Speicherbereichen. Dieser &quot;Token Importer&quot;trägt dazu bei, die Kompatibilität zwischen aufeinander folgenden AccessEnabler-Versionen zu erreichen, wodurch der SSO-Status auch bei Aktualisierung der Speicherversion beibehalten wird.

Das Importtool wird während des [`setRequestor()`](#setRequestor)-Flusses ausgeführt und in den folgenden beiden Szenarien ausgeführt (vorausgesetzt, im aktuellen Speicher ist kein gültiges Authentifizierungstoken für den aktuellen Programmierer vorhanden):

- Die erste Installation einer 1.7-App, die von einem bestimmten Programmierer entwickelt wurde
- Pfad zu einem zukünftigen AccessEnabler aktualisieren, der einen neuen Speicher verwendet

Der Importvorgang ist für den Programmierer transparent und erfordert keine Codeänderungen in der Clientanwendung.



### Format {#format}

- [Authentifizierungstoken](#authn_token)
- [Autorisierungstoken](#authz_token)
- [Short Media Token](#short_media_token)
- [Gerätebindung](#device_binding)

#### Authentifizierungstoken {#authn_token}

Die folgende Liste zeigt das Format des Authentifizierungstokens:

```XML
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthenticationToken>
        <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
        <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>   
    </simpleAuthenticationToken>
```


#### Autorisierungstoken {#authz_token}

Die folgende Liste zeigt das Format des Autorisierungstokens:

```XML
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthorizationToken>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
        <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>
    </simpleAuthorizationToken>
```


#### Short Media Token {#short_media_token}

Die folgende Liste zeigt das Format des Short-Media-Tokens.  Dieses Token wird der Anwendung des Programmierers zur Verfügung gestellt.  Er wird am Ende eines erfolgreichen Berechtigungsprozesses an die Anwendung des Programmierers übergeben:

```XML
    <signatureInfo>signature<signatureInfo>
    <shortAuthorizationToken>
      <sessionGUID>session_guid</sessionGUID>
      <requestorID>requestor_id</requestorID>
      <resourceID>resource_id</resourceID>
      <ttl>ttl_in_ms</ttl>
      <issueTime>issue_time</issueTime>
      <mvpdId>mvpd_id</mvpdId>
      <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
    </shortAuthorizationToken>
```


#### Gerätebindung {#device_binding}

Beachten Sie in den oben aufgeführten XML-Listen das Tag `simpleTokenFingerprint`. Der Zweck dieses Tags besteht darin, native ID-Individualisierungsinformationen für Geräte zu speichern. Die AccessEnabler-Bibliothek kann diese Individualisierungsinformationen abrufen und den Adobe Pass-Authentifizierungsdiensten während der Berechtigungsaufrufe zur Verfügung stellen. Der Dienst verwendet diese Informationen und bettet sie in die tatsächlichen Token ein, wodurch die Token effektiv an ein bestimmtes Gerät gebunden werden. Das Endziel besteht darin, die Token geräteübergreifend nicht übertragbar zu machen.



Notieren Sie sich in den oben aufgeführten XML-Listen das Tag simpleTokenFingerprint. Der Zweck dieses Tags besteht darin, native ID-Individualisierungsinformationen für Geräte zu speichern. Die AccessEnabler-Bibliothek kann diese Individualisierungsinformationen abrufen und den Adobe Pass-Authentifizierungsdiensten während der Berechtigungsaufrufe zur Verfügung stellen. Der Dienst verwendet diese Informationen und bettet sie in die tatsächlichen Token ein, wodurch die Token effektiv an ein bestimmtes Gerät gebunden werden. Das Endziel besteht darin, die Token geräteübergreifend nicht übertragbar zu machen.



Da dies offensichtlich eine sicherheitsrelevante Funktion ist, ist diese Information von Natur aus &quot;sensibel&quot; aus sicherheitspolitischer Sicht. Daher müssen diese Informationen sowohl vor Manipulation als auch vor Abhören geschützt werden. Das Abhörproblem wird gelöst, indem die Authentifizierungs-/Autorisierungsanfragen über das HTTPS-Protokoll gesendet werden. Der Manipulationsschutz wird durch digitale Signatur der Identifizierungsinformationen des Geräts erreicht. Die AccessEnabler-Bibliothek berechnet eine Geräte-ID aus den vom Gerät bereitgestellten Informationen und sendet dann die Geräte-ID &quot;in the clear&quot;als Anfrageparameter an die Adobe Pass-Authentifizierungsserver.  Die Adobe Pass-Authentifizierungsserver signieren die Geräte-ID digital mit dem privaten Adobe-Schlüssel und fügen sie dem Authentifizierungstoken hinzu, das an den AccessEnabler zurückgegeben wird. Daher ist die Geräte-ID mit dem Authentifizierungstoken verknüpft.  Während des Autorisierungsflusses sendet der AccessEnabler erneut die Geräte-ID in der -Datei zusammen mit dem Authentifizierungstoken.  Wenn der Validierungsprozess fehlschlägt, schlagen die Authentifizierungs-/Autorisierungs-Workflows automatisch fehl.  Die Adobe Pass-Authentifizierungsserver wenden den privaten Schlüssel auf die Geräte-ID an und vergleichen ihn mit dem Wert im Authentifizierungstoken.  Wenn sie nicht übereinstimmen, schlägt dieser Berechtigungsfluss fehl.


<!--
## Related Information {#related}

- [Android Integration Cookbook](#)
- [Android API](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass Authentication native SDK (Tech Note)](#)
- [AE 1.6: How to resolve Gson dependencies (Tech Note)](#)
-->
