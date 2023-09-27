---
title: Technische Übersicht über Amazon FireOS
description: Technische Übersicht über Amazon FireOS
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2154'
ht-degree: 0%

---

# Technische Übersicht über Amazon FireOS {#amazon-fireos-technical-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Einführung {#intro}

Amazon FireOS AccessEnabler wird durch zwei Komponenten repräsentiert: eine AccessEnabler -Stub-Bibliothek, die von der Anwendung verwendet wird, und eine Java-Android-Bibliothek auf Systemebene, mit der mobile Apps die Adobe Pass-Authentifizierung für die Berechtigungsdienste von TV Anywhere verwenden können. Eine Android-Implementierung für Amazon FireOS besteht aus der AccessEnabler-Schnittstelle, die die Berechtigungs-API definiert, und einem EntitlementDelegate-Protokoll, das die Rückrufe beschreibt, die von der -Bibliothek Trigger werden. Die AccessEnabler Android-Bibliothek auf Systemebene ermöglicht den Zugriff auf Amazon-Dienste, um Single Sing On auf Plattformebene zu aktivieren.

## Grundlegendes zu nativen Client-Workflows {#native_client_workflows}

Native Client-Workflows sind in der Regel mit denen von browserbasierten Adobe Pass-Authentifizierungs-Clients identisch oder sehr ähnlich. Es gibt jedoch einige Ausnahmen, wie unten beschrieben.


### Nachinitialisierungs-Workflow {#post-init}

Alle vom AccessEnabler unterstützten Berechtigungs-Workflows setzen voraus, dass Sie zuvor [`setRequestor()`](#setRequestor) , um Ihre Identität festzustellen. Sie führen diesen Aufruf aus, um Ihre Anforderer-ID nur einmal bereitzustellen, normalerweise während der Initialisierungs-/Setup-Phase Ihrer Anwendung.

Bei den nativen Clients (z. B. AmazonFireOS) nach dem ersten Aufruf an [`setRequestor()`](#setRequestor), können Sie entscheiden, wie Sie vorgehen:

- Sie können sofort mit Berechtigungsaufrufen beginnen und diese bei Bedarf still in die Warteschlange stellen.
- Sie können auch eine Bestätigung des Erfolgs/Misserfolgs von [`setRequestor()`](#setRequestor) durch Implementierung des Rückrufs setRequestorComplete() .
- Oder machen Sie beides.

Es liegt an Ihnen, auf die Benachrichtigung über den Erfolg der [`setRequestor()`](#setRequestor) oder sich auf den Aufruf-Warteschlangenmechanismus von AccessEnabler zu verlassen. Da alle nachfolgenden Autorisierungs- und Authentifizierungsanfragen die Anforderer-ID und die zugehörigen Konfigurationsinformationen benötigen, muss die [`setRequestor()`](#setRequestor) -Methode blockiert effektiv alle Authentifizierungs- und Autorisierungs-API-Aufrufe, bis die Initialisierung abgeschlossen ist.

### Allgemeiner Workflow für die anfängliche Authentifizierung {#generic}

Dieser Workflow dient der Anmeldung eines Benutzers mit seinem MVPD.  Nach erfolgreicher Anmeldung gibt der Backend-Server dem Benutzer ein Authentifizierungstoken aus. Während die Authentifizierung in der Regel im Rahmen des Autorisierungsprozesses erfolgt, wird im Folgenden beschrieben, wie die Authentifizierung isoliert funktioniert, und es sind keine Autorisierungsschritte enthalten.

Beachten Sie, dass sich der folgende native Client-Workflow vom typischen browserbasierten Authentifizierungs-Workflow unterscheidet, die Schritte 1 bis 5 jedoch für native Clients und browserbasierte Clients gleich sind:

1. Ihre Seite oder Ihr Player initiiert den Authentifizierungs-Workflow mit einem Aufruf von [getAuthentication()](#getAuthN), das nach einem gültigen zwischengespeicherten Authentifizierungstoken sucht. Diese Methode verfügt über eine optionale `redirectURL` Parameter; wenn Sie keinen Wert für `redirectURL`nach erfolgreicher Authentifizierung wird der Benutzer an die URL zurückgegeben, von der aus die Authentifizierung initialisiert wurde.
1. Der AccessEnabler bestimmt den aktuellen Authentifizierungsstatus. Wenn der Benutzer derzeit authentifiziert ist, ruft AccessEnabler Ihre `setAuthenticationStatus()` Callback-Funktion, die einen Authentifizierungsstatus übergibt, der auf Erfolg hinweist (Schritt 7 unten).
1. Wenn der Benutzer nicht authentifiziert ist, setzt der AccessEnabler den Authentifizierungsfluss fort, indem er bestimmt, ob der letzte Authentifizierungsversuch des Benutzers mit einem bestimmten MVPD erfolgreich war. Wenn eine MVPD-ID zwischengespeichert wird UND die `canAuthenticate` Flag ist wahr ODER ein MVPD wurde mit [`setSelectedProvider()`](#setSelectedProvider), wird der Benutzer nicht mit dem MVPD-Auswahldialogfeld aufgefordert. Der Authentifizierungsfluss wird mit dem zwischengespeicherten Wert des MVPD fortgesetzt (d. h. mit dem MVPD, der bei der letzten erfolgreichen Authentifizierung verwendet wurde). Ein Netzwerkaufruf erfolgt an den Backend-Server und der Benutzer wird zur MVPD-Anmeldeseite weitergeleitet (Schritt 6 unten).
1. Wenn keine MVPD-ID zwischengespeichert wurde UND kein MVPD mit [`setSelectedProvider()`](#setSelectedProvider) ODER `canAuthenticate` Flag auf &quot;false&quot;festgelegt ist, wird die [`displayProviderDialog()`](#displayProviderDialog) Callback wird aufgerufen. Dieser Rückruf leitet Ihre Seite oder Ihren Player an, um die Benutzeroberfläche zu erstellen, über die der Benutzer eine Liste mit MVPDs zur Auswahl hat. Es wird ein Array von MVPD-Objekten bereitgestellt, die die erforderlichen Informationen zum Erstellen des MVPD-Selektors enthalten. Jedes MVPD-Objekt beschreibt eine MVPD-Entität und enthält Informationen wie die Kennung des MVPD (z. B. XFINITY, AT\&amp;T usw.) und die URL, unter der das MVPD-Logo zu finden ist.
1. Sobald ein bestimmter MVPD ausgewählt ist, muss Ihre Seite oder Ihr Player den AccessEnabler über die Auswahl des Benutzers informieren. Bei Nicht-Flash-Clients informieren Sie den AccessEnabler über die Benutzerauswahl, sobald der Benutzer den gewünschten MVPD auswählt. [`setSelectedProvider()`](#setSelectedProvider) -Methode. Flash-Clients senden stattdessen eine freigegebene `MVPDEvent` des Typs &quot;`mvpdSelection`&quot;, wobei der ausgewählte Provider übergeben wird.
1. Bei Amazon-Anwendungen muss die Variable [`navigateToUrl()`](#navigagteToUrl) -Rückruf wird ignoriert. Die Access Enabler-Bibliothek erleichtert den Zugriff auf ein gemeinsames WebView-Steuerelement zum Authentifizieren von Benutzern.
1. Über die `WebView`, gelangt der Benutzer auf die Anmeldeseite des MVPD und gibt seine Anmeldeinformationen ein. Beachten Sie, dass während dieser Übertragung mehrere Umleitungsvorgänge auftreten.
1. Sobald die WebView-Authentifizierung abgeschlossen ist, wird sie geschlossen und der AccessEnabler informiert, dass sich der Benutzer erfolgreich angemeldet hat. Der AccessEnabler ruft das tatsächliche Authentifizierungstoken vom Backend-Server ab. AccessEnabler ruft die [`setAuthenticationStatus()`](#setAuthNStatus) Callback mit Status-Code 1, der auf Erfolg hinweist. Wenn bei der Ausführung dieser Schritte ein Fehler auftritt, wird die [`setAuthenticationStatus()`](#setAuthNStatus) -Callback wird mit dem Status-Code 0 zusammen mit einem entsprechenden Fehlercode ausgelöst, der angibt, dass der Benutzer nicht authentifiziert ist.

### Workflow abmelden {#logout}

Bei nativen Clients werden Logouts ähnlich wie der oben beschriebene Authentifizierungsprozess verarbeitet. Auf diese Weise erstellt der AccessEnabler einen `WebView` steuert und lädt das Steuerelement mit der URL des Abmelde-Endpunkts auf den Backend-Server. Sobald der Abmeldevorgang abgeschlossen ist, werden die Token gelöscht.

Beachten Sie, dass sich der Abmeldefluss vom Authentifizierungsfluss unterscheidet, da der Benutzer nicht mit der `WebView` in irgendeiner Weise. Sobald die Abmeldung abgeschlossen ist, ruft der AccessEnabler die `setAuthenticationStatus()` Callback mit Status-Code 0, der angibt, dass der Benutzer nicht authentifiziert ist.

## Token {#tokens}

### Definitionen und Verwendung {#definitions}

Die Berechtigungslösung für die Adobe Pass-Authentifizierung umfasst die Generierung bestimmter Datenteile (Token), die die Adobe Pass-Authentifizierung nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungsarbeitsabläufe generiert. Diese Token werden lokal auf dem Amazon FireOS-Gerät des Clients gespeichert.

Token haben eine begrenzte Lebensdauer. Nach Ablauf der Gültigkeit müssen Token erneut ausgestellt werden, indem die Authentifizierungs- und/oder Autorisierungs-Workflows neu gestartet werden.

Während der Berechtigungs-Workflows werden drei Typen von Token ausgegeben:

- **Authentifizierungstoken** - Das Endergebnis des Workflows zur Benutzerauthentifizierung ist eine Authentifizierungs-GUID, die der AccessEnabler verwenden kann, um Autorisierungsabfragen im Namen des Benutzers durchzuführen. Diese Authentifizierungs-GUID weist einen zugehörigen TTL-Wert (Time-to-Live) auf, der sich von der Authentifizierungssitzung des Benutzers selbst unterscheiden kann. Die Adobe Pass-Authentifizierung generiert ein Authentifizierungstoken, indem sie die Authentifizierungs-GUID an das Gerät bindet, das die Authentifizierungsanfragen initiiert.
- **Autorisierungstoken** - Gewährt Zugriff auf eine spezifische geschützte Ressource, die durch eine eindeutige `resourceID`. Sie besteht aus einer von der autorisierenden Partei erteilten Ermächtigung zusammen mit dem Original `resourceID`. Diese Informationen sind an das Gerät gebunden, das die Anfrage initiiert.
- **Kurzlebige Medien-Token** - Der AccessEnabler gewährt Zugriff auf die Hosting-Anwendung für eine bestimmte Ressource, indem er ein kurzlebiges Medien-Token zurückgibt. Dieses Token wird basierend auf dem Autorisierungstoken generiert, das zuvor für diese bestimmte Ressource erworben wurde. Außerdem ist dieses Token nicht an das Gerät gebunden und die zugehörige Lebensdauer ist deutlich kürzer (Standard: 5 Minuten).

Nach erfolgreicher Authentifizierung und Autorisierung stellt die Adobe Pass-Authentifizierung Authentifizierungs-, Autorisierungs- und kurzlebige Medien-Token aus. Diese Token sollten auf dem Gerät des Benutzers zwischengespeichert und für die Dauer der zugehörigen Lebensdauer verwendet werden.

### Caching-Richtlinien {#caching}


#### Authentifizierungstoken

- **AccessEnabler 1.10.1 für FireOS** basiert auf AccessEnabler für Android 1.9.1 - Mit diesem SDK wird eine neue Methode der Token-Speicherung eingeführt, die mehrere Programmer-MVPD-Buckets und damit mehrere Authentifizierungstoken ermöglicht.

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

Der Token-Speicher kann mehrere Programmer-MVPD-Kombinationen unterstützen, die auf einer mehrstufigen verschachtelten Zuordnungsstruktur basieren, die mehrere Authentifizierungstoken enthalten kann. Dieser neue Speicher wirkt sich in keiner Weise auf die öffentliche AccessEnabler-API aus und erfordert keine Änderungen auf der Seite des Programmierers. Im Folgenden finden Sie ein Beispiel, das diese neuere Funktion veranschaulicht:

1. Öffnen Sie App1 (entwickelt von Programmer1).
1. Authentifizieren Sie sich mit MVPD1 (integriert in Programmer1).
1. Aussetzen/Schließen Sie die aktuelle Anwendung und öffnen Sie App2 (entwickelt von Programmer2).
1. Nehmen wir an, dass Programmer2 nicht in MVPD2 integriert ist. Daher wird der Benutzer NICHT in App2 authentifiziert.
1. Authentifizieren Sie sich mit MVPD2 (das mit Programmer2 integriert ist) in App2.
1. Wechseln Sie zurück zu App1. Der Benutzer wird weiterhin mit Programmer1 authentifiziert.

### Format {#format}

#### Authentifizierungstoken {#authn_token}

Die folgende Liste zeigt das Format des Authentifizierungstokens:

```JSON
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

```JSON
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

```JSON
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

Beachten Sie in den oben aufgeführten XML-Listen das Tag mit dem Namen `simpleTokenFingerprint`. Der Zweck dieses Tags besteht darin, native ID-Individualisierungsinformationen für Geräte zu speichern. Die AccessEnabler-Bibliothek kann diese Individualisierungsinformationen abrufen und den Adobe Pass-Authentifizierungsdiensten während der Berechtigungsaufrufe zur Verfügung stellen. Der Dienst verwendet diese Informationen und bettet sie in die tatsächlichen Token ein, wodurch die Token effektiv an ein bestimmtes Gerät gebunden werden. Das Endziel besteht darin, die Token geräteübergreifend nicht übertragbar zu machen.

Notieren Sie sich in den oben aufgeführten XML-Listen das Tag simpleTokenFingerprint. Der Zweck dieses Tags besteht darin, native ID-Individualisierungsinformationen für Geräte zu speichern. Die AccessEnabler-Bibliothek kann diese Individualisierungsinformationen abrufen und den Adobe Pass-Authentifizierungsdiensten während der Berechtigungsaufrufe zur Verfügung stellen. Der Dienst verwendet diese Informationen und bettet sie in die tatsächlichen Token ein, wodurch die Token effektiv an ein bestimmtes Gerät gebunden werden. Das Endziel besteht darin, die Token geräteübergreifend nicht übertragbar zu machen.

Da dies offensichtlich eine sicherheitsrelevante Funktion ist, ist diese Information von Natur aus &quot;sensibel&quot; aus sicherheitspolitischer Sicht. Daher müssen diese Informationen sowohl vor Manipulation als auch vor Abhören geschützt werden. Das Abhörproblem wird gelöst, indem die Authentifizierungs-/Autorisierungsanfragen über das HTTPS-Protokoll gesendet werden. Der Manipulationsschutz wird durch digitale Signatur der Identifizierungsinformationen des Geräts erreicht. Die AccessEnabler-Bibliothek berechnet eine Geräte-ID aus den vom Gerät bereitgestellten Informationen und sendet dann die Geräte-ID &quot;in the clear&quot;als Anfrageparameter an die Adobe Pass-Authentifizierungsserver.  Die Adobe Pass-Authentifizierungsserver signieren die Geräte-ID digital mit dem privaten Adobe-Schlüssel und fügen sie dem Authentifizierungstoken hinzu, das an den AccessEnabler zurückgegeben wird. Daher ist die Geräte-ID mit dem Authentifizierungstoken verknüpft.  Während des Autorisierungsflusses sendet der AccessEnabler erneut die Geräte-ID in der -Datei zusammen mit dem Authentifizierungstoken.  Wenn der Validierungsprozess fehlschlägt, schlagen die Authentifizierungs-/Autorisierungs-Workflows automatisch fehl.  Die Adobe Pass-Authentifizierungsserver wenden den privaten Schlüssel auf die Geräte-ID an und vergleichen ihn mit dem Wert im Authentifizierungstoken.  Wenn sie nicht übereinstimmen, schlägt dieser Berechtigungsfluss fehl.
