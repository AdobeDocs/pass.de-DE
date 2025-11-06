---
title: Technische Übersicht zu Amazon FireOS
description: Technische Übersicht zu Amazon FireOS
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '2189'
ht-degree: 0%

---

# (Legacy) Amazon FireOS - Technische Übersicht {#amazon-fireos-technical-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>

## Einführung {#intro}

Amazon FireOS AccessEnabler wird durch zwei Komponenten dargestellt: eine AccessEnabler-Stubbibliothek, die von der Anwendung verwendet wird, und eine Java Android-Bibliothek auf Systemebene, die es mobilen Apps ermöglicht, die Adobe Pass-Authentifizierung für die Berechtigungsdienste von TV Everywhere zu verwenden. Eine Android-Implementierung für Amazon FireOS besteht aus der AccessEnabler-Schnittstelle, die die Berechtigungs-API definiert, und einem EntitlementDelegate-Protokoll, das die Callbacks beschreibt, die die Bibliothek Trigger. Die AccessEnabler Android-Bibliothek auf Systemebene ermöglicht den Zugriff auf Amazon-Services, um Single Sign-On auf Plattformebene zu ermöglichen.

## Grundlegendes zu nativen Client-Workflows {#native_client_workflows}

Native Client-Workflows sind in der Regel mit denen browserbasierter Adobe Pass-Authentifizierungs-Clients identisch oder ihnen sehr ähnlich. Es gibt jedoch einige Ausnahmen, wie unten beschrieben.


### Workflow nach der Initialisierung {#post-init}

Bei allen vom AccessEnabler unterstützten Berechtigungs-Workflows wird davon ausgegangen, dass Sie [`setRequestor()`](#setRequestor) zuvor aufgerufen haben, um Ihre Identität festzulegen. Sie führen diesen Aufruf aus, um Ihre Anforderer-ID nur einmal anzugeben, normalerweise während der Initialisierungs-/Einrichtungsphase Ihrer Anwendung.

Bei den nativen Clients (z. B. AmazonFireOS) haben Sie nach Ihrem ersten Aufruf an [`setRequestor()`](#setRequestor) die Wahl, wie Sie fortfahren möchten:

- Sie können sofort mit Berechtigungsaufrufen beginnen und bei Bedarf zulassen, dass sie ohne Nachfrage in die Warteschlange gestellt werden.
- Sie können auch eine Bestätigung über den Erfolg/Misserfolg von [`setRequestor()`](#setRequestor) erhalten, indem Sie den Callback setRequestorComplete() implementieren.
- Oder beides.

Es liegt an Ihnen zu entscheiden, ob Sie auf die Benachrichtigung über den Erfolg von [`setRequestor()`](#setRequestor) warten oder sich auf den Call Queue-Mechanismus von AccessEnabler verlassen sollen. Da alle nachfolgenden Autorisierungs- und Authentifizierungsanfragen die Anforderer-ID und die zugehörigen Konfigurationsinformationen benötigen, blockiert die [`setRequestor()`](#setRequestor)-Methode effektiv alle Authentifizierungs- und Autorisierungs-API-Aufrufe, bis die Initialisierung abgeschlossen ist.

### Allgemeiner anfänglicher Authentifizierungs-Workflow {#generic}

Dieser Workflow dient der Anmeldung von Benutzenden mit ihrer MVPD.  Nach erfolgreicher Anmeldung gibt der Backend-Server dem Benutzer ein Authentifizierungstoken aus. Während die Authentifizierung in der Regel als Teil des Autorisierungsprozesses erfolgt, wird im Folgenden beschrieben, wie die Authentifizierung isoliert funktionieren kann und keine Autorisierungsschritte enthält.

Beachten Sie, dass sich der folgende native Client-Workflow zwar vom typischen browserbasierten Authentifizierungs-Workflow unterscheidet, die Schritte 1 bis 5 jedoch für native Clients und browserbasierte Clients identisch sind:

1. Ihre Seite oder Ihr Player initiiert den Authentifizierungs-Workflow mit einem Aufruf von [getAuthentication()](#getAuthN), der auf ein gültiges zwischengespeichertes Authentifizierungstoken prüft. Diese Methode verfügt über einen optionalen `redirectURL`. Wenn Sie keinen Wert für `redirectURL` angeben, wird der Benutzer nach einer erfolgreichen Authentifizierung an die URL zurückgegeben, von der die Authentifizierung initialisiert wurde.
1. AccessEnabler bestimmt den aktuellen Authentifizierungsstatus. Wenn der Benutzer derzeit authentifiziert ist, ruft AccessEnabler Ihre `setAuthenticationStatus()` Callback-Funktion auf und übergibt einen Authentifizierungsstatus, der auf Erfolg hinweist (Schritt 7 unten).
1. Wenn der Benutzer nicht authentifiziert ist, setzt AccessEnabler den Authentifizierungsfluss fort, indem festgestellt wird, ob der letzte Authentifizierungsversuch des Benutzers mit einer bestimmten MVPD erfolgreich war. Wenn eine MVPD-ID zwischengespeichert wird UND das `canAuthenticate`-Flag „true“ lautet ODER eine MVPD mithilfe von [`setSelectedProvider()`](#setSelectedProvider) ausgewählt wurde, wird der/die Benutzende nicht über das MVPD-Auswahldialogfeld aufgefordert. Der Authentifizierungsfluss verwendet weiterhin den zwischengespeicherten Wert der MVPD (d. h. dieselbe MVPD, die bei der letzten erfolgreichen Authentifizierung verwendet wurde). Ein Netzwerkaufruf erfolgt an den Backend-Server und der Anwender wird zur Anmeldeseite von MVPD weitergeleitet (Schritt 6 unten).
1. Wenn keine MVPD-ID zwischengespeichert ist UND keine MVPD mithilfe von [`setSelectedProvider()`](#setSelectedProvider) ausgewählt wurde ODER das `canAuthenticate`-Flag auf „false“ gesetzt ist, wird der [`displayProviderDialog()`](#displayProviderDialog)-Callback aufgerufen. Dieser Rückruf leitet Ihre Seite oder Ihren Player zur Erstellung der Benutzeroberfläche an, die dem Benutzer eine Liste von MVPDs zur Auswahl bietet. Es wird ein Array von MVPD-Objekten bereitgestellt, das die erforderlichen Informationen zum Erstellen des MVPD-Selektors enthält. Jedes MVPD-Objekt beschreibt eine MVPD-Entität und enthält Informationen wie die ID der MVPD (z. B. XFINITY, AT\&amp;T usw.) und die URL, unter der sich das MVPD-Logo befindet.
1. Nachdem eine bestimmte MVPD ausgewählt wurde, muss Ihre Seite oder Ihr Player den AccessEnabler über die Auswahl des Benutzers informieren. Bei Nicht-Flash-Clients informieren Sie den AccessEnabler über die Benutzerauswahl, sobald der Anwender die gewünschte MVPD auswählt, über einen Aufruf der [`setSelectedProvider()`](#setSelectedProvider). Flash-Clients senden stattdessen einen freigegebenen `MVPDEvent` vom Typ &quot;`mvpdSelection`&quot; und übergeben den ausgewählten Provider.
1. Bei Amazon-Programmen wird der [`navigateToUrl()`](#navigagteToUrl)-Callback ignoriert. Die Access Enabler-Bibliothek erleichtert den Zugriff auf ein gemeinsames WebView-Steuerelement zum Authentifizieren von Benutzern.
1. Über die `WebView` gelangt der/die Benutzende auf die Anmeldeseite von MVPD und gibt seine/ihre Anmeldeinformationen ein. Beachten Sie, dass während dieser Übertragung mehrere Umleitungsvorgänge stattfinden.
1. Sobald WebView die Authentifizierung abgeschlossen hat, wird sie geschlossen und der AccessEnabler wird informiert, dass sich der Benutzer erfolgreich angemeldet hat. AccessEnabler ruft das tatsächliche Authentifizierungstoken vom Backend-Server ab. Der AccessEnabler ruft den [`setAuthenticationStatus()`](#setAuthNStatus)-Callback mit dem Status-Code 1 auf und zeigt damit an, dass er erfolgreich war. Wenn während der Ausführung dieser Schritte ein Fehler auftritt, wird der [`setAuthenticationStatus()`](#setAuthNStatus)-Callback mit dem Status-Code 0 zusammen mit einem entsprechenden Fehler-Code ausgelöst, was darauf hinweist, dass der Benutzer nicht authentifiziert ist.

### Abmelde-Workflow {#logout}

Für native Clients werden Logouts ähnlich wie beim oben beschriebenen Authentifizierungsprozess gehandhabt. Nach diesem Muster erstellt AccessEnabler ein `WebView` und lädt das Steuerelement mit der URL des Abmeldeendpunkts auf den Backend-Server. Sobald der Abmeldevorgang abgeschlossen ist, werden die Token gelöscht.

Beachten Sie, dass sich der Abmeldefluss vom Authentifizierungsfluss insofern unterscheidet, als der Benutzer in keiner Weise mit dem `WebView` interagieren muss. Nach Abschluss des Abmeldevorgangs ruft AccessEnabler den `setAuthenticationStatus()`-Callback mit dem Status-Code 0 auf, was anzeigt, dass der Benutzer nicht authentifiziert ist.

## Token {#tokens}

### Definitionen und Verwendung {#definitions}

Die Lösung für die Berechtigung für die Adobe Pass-Authentifizierung umfasst die Generierung bestimmter Datenelemente (Token), die von der Adobe Pass-Authentifizierung nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungs-Workflows generiert werden. Diese Token werden lokal auf dem Amazon FireOS-Gerät des Clients gespeichert.

Token haben eine begrenzte Lebensdauer. Nach Ablauf müssen Token durch die erneute Initiierung der Authentifizierungs- und/oder Autorisierungs-Workflows erneut ausgestellt werden.

Es gibt drei Arten von Token, die während der Berechtigungs-Workflows ausgegeben werden:

- **Authentifizierungstoken** - Das Endergebnis des Benutzerauthentifizierungs-Workflows ist eine Authentifizierungs-GUID, mit der AccessEnabler Autorisierungsabfragen im Namen des Benutzers durchführen kann. Diese Authentifizierungs-GUID hat einen zugehörigen TTL-Wert (Time-to-Live), der sich von der Authentifizierungssitzung des Benutzers selbst unterscheiden kann. Die Adobe Pass-Authentifizierung generiert ein Authentifizierungstoken, indem sie die Authentifizierungs-GUID an das Gerät bindet, das die Authentifizierungsanfragen initiiert.
- **Autorisierungs-Token** - Gewährt Zugriff auf eine bestimmte geschützte Ressource, die durch eine eindeutige `resourceID` identifiziert wird. Sie besteht aus einer von der genehmigenden Partei zusammen mit dem ursprünglichen `resourceID` ausgestellten Genehmigung. Diese Informationen sind an das Gerät gebunden, das die Anfrage initiiert.
- **Kurzlebiges Medien-Token** - AccessEnabler gewährt durch Rückgabe eines kurzlebigen Medien-Tokens Zugriff auf die Hosting-Anwendung für eine bestimmte Ressource. Dieses Token wird basierend auf dem Autorisierungs-Token generiert, das zuvor für diese bestimmte Ressource abgerufen wurde. Außerdem ist dieses Token nicht an das Gerät gebunden und die zugehörige Lebensdauer ist erheblich kürzer (Standard: 5 Minuten).

Nach erfolgreicher Authentifizierung und Autorisierung gibt die Adobe Pass-Authentifizierung Authentifizierungs-, Autorisierungs- und kurzlebige Medien-Token aus. Diese Token sollten auf dem Gerät des Benutzers zwischengespeichert und für die Dauer der zugehörigen Lebensdauer verwendet werden.

### Richtlinien für das Zwischenspeichern {#caching}


#### Authentifizierungstoken

- **AccessEnabler 1.10.1 für FireOS** basiert auf AccessEnabler für Android 1.9.1 - Diese SDK führt eine neue Methode der Token-Speicherung ein, die mehrere Programmer-MVPD-Buckets und daher mehrere Authentifizierungstoken ermöglicht.

#### Autorisierungs-Token

Es wird immer nur EIN Autorisierungs-Token pro Ressource vom AccessEnabler zwischengespeichert. Es können mehrere Autorisierungs-Token zwischengespeichert werden, sie sind jedoch mit verschiedenen Ressourcen verknüpft. Wenn ein neues Autorisierungs-Token ausgegeben wird und bereits ein altes für dieselbe Ressource vorhanden ist, überschreibt das neue Token den vorhandenen zwischengespeicherten Wert.

#### Medien-Token

Das kurzlebige Medien-Token sollte ÜBERHAUPT NICHT zwischengespeichert werden. Das Medien-Token sollte jedes Mal vom Server abgerufen werden, wenn eine Autorisierungs-API aufgerufen wird, da es auf die einmalige Verwendung beschränkt ist.

### Persistenz {#persistence}

Token müssen über mehrere aufeinander folgende Ausführungen desselben Programms hinweg persistent sein. Das bedeutet, dass nach dem Abrufen der Authentifizierungs- und Autorisierungs-Token und dem Schließen der Anwendung durch den Benutzer dieselben Token für die Anwendung verfügbar sind, wenn der Benutzer die Anwendung erneut öffnet. Darüber hinaus ist es wünschenswert, dass diese Token über mehrere Anwendungen hinweg persistent sind. Mit anderen Worten: Wenn ein Benutzer eine Anwendung verwendet, um sich bei einem bestimmten Identitätsanbieter anzumelden (und Authentifizierungs- und Autorisierungs-Token erfolgreich erhalten hat), können dieselben Token über eine andere Anwendung verwendet werden, und der Benutzer wird bei der Anmeldung über denselben Identitätsanbieter nicht mehr zur Eingabe von Anmeldeinformationen aufgefordert.

Diese Art von nahtlosem Authentifizierungs-/Autorisierungs-Workflow macht die Adobe Pass-Authentifizierungslösung zu einer echten TV-Everywhere-Implementierung. Aus rein technischer Sicht umgeht die Android AccessEnabler-Bibliothek die Probleme der programmübergreifenden Datenfreigabe, indem die Token-Daten in einer Datenbankdatei gespeichert werden, die sich auf einem externen Speicher befindet. Diese freigegebenen Ressourcen auf Systemebene bieten die wichtigsten Bestandteile, die die Implementierung des gewünschten Anwendungsfalls für persistente Token ermöglichen:

- Unterstützung für strukturierten Speicher - Der Adobe Pass-Authentifizierungstoken-Speicher ist nicht nur eine einfache, lineare, pufferähnliche Speicherstruktur. Es bietet einen wörterbuchähnlichen Speichermechanismus, der die Indizierung von Daten auf der Grundlage benutzerdefinierter Schlüsselwerte ermöglicht.
- Unterstützung der Datenpersistenz mit dem zugrunde liegenden Dateisystem - Der Inhalt der Datenbankdatei wird standardmäßig beibehalten und die Daten werden im externen Speicher des Geräts gespeichert.

Sobald ein bestimmtes Token im Token-Cache platziert ist, wird seine Gültigkeit von der AccessEnabler-Bibliothek zu verschiedenen Zeiten überprüft.  Ein gültiges Token wird wie folgt definiert:

- Die TTL des Tokens ist nicht abgelaufen
- Der Aussteller des Tokens ist in der Liste der zulässigen Identitätsanbieter enthalten

Der Token-Speicher kann mehrere Programmer-MVPD-Kombinationen unterstützen, wobei eine mehrstufige verschachtelte Zuordnungsstruktur verwendet wird, die mehrere Authentifizierungstoken enthalten kann. Dieser neue Speicher hat keinerlei Auswirkungen auf die öffentliche AccessEnabler-API und erfordert keine Änderungen seitens des Programmierers. Im Folgenden finden Sie ein Beispiel für diese neuere Funktion:

1. Öffnen Sie App1 (entwickelt von Programmer1).
1. Authentifizierung mit MVPD1 (in Programmer1 integriert).
1. Aussetzen/Schließen Sie die aktuelle Anwendung und öffnen Sie App2 (entwickelt von Programmer2).
1. Nehmen wir an, dass Programmer2 nicht in MVPD2 integriert ist. Daher wird der Benutzer NICHT in App2 authentifiziert.
1. Authentifizierung bei MVPD2 (das mit Programmer2 integriert ist) in App2.
1. Wechseln Sie zurück zu App1. Der Benutzer wird weiterhin mit Programmer1 authentifiziert.

### Format {#format}

#### Authentifizierungstoken {#authn_token}

In der folgenden Liste ist das Format des Authentifizierungstokens aufgeführt:

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


#### Autorisierungs-Token {#authz_token}

In der folgenden Liste ist das Format des Autorisierungs-Tokens aufgeführt:

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


#### Kurzes Medien-Token {#short_media_token}

In der folgenden Liste wird das Format des Kurzwahltokens angezeigt.  Dieses Token wird für die Anwendung des Programmierers verfügbar gemacht.  Sie wird am Ende eines erfolgreichen Berechtigungsprozesses an das Programm des Programmierers übergeben:

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

Beachten Sie in den obigen XML-Auflistungen das Tag mit dem Titel `simpleTokenFingerprint`. Der Zweck dieses Tags ist es, native Geräte-ID-Individualisierungsinformationen zu enthalten. Die AccessEnabler-Bibliothek kann diese Personalisierungsinformationen abrufen und während der Berechtigungsaufrufe für die Adobe Pass-Authentifizierungsdienste verfügbar machen. Der Service verwendet diese Informationen und bettet sie in die tatsächlichen Token ein, wodurch die Token effektiv an ein bestimmtes Gerät gebunden werden. Das Endziel besteht darin, die Token nicht geräteübergreifend übertragbar zu machen.

Beachten Sie in den obigen XML-Listen das Tag „simpleTokenFingerprint“. Der Zweck dieses Tags ist es, native Geräte-ID-Individualisierungsinformationen zu enthalten. Die AccessEnabler-Bibliothek kann diese Personalisierungsinformationen abrufen und während der Berechtigungsaufrufe für die Adobe Pass-Authentifizierungsdienste verfügbar machen. Der Service verwendet diese Informationen und bettet sie in die tatsächlichen Token ein, wodurch die Token effektiv an ein bestimmtes Gerät gebunden werden. Das Endziel besteht darin, die Token nicht geräteübergreifend übertragbar zu machen.

Da es sich hierbei offensichtlich um eine sicherheitsbezogene Funktion handelt, sind diese Informationen aus Sicherheitssicht von Natur aus „sensibel“. Daher müssen diese Informationen sowohl vor Manipulationen als auch vor Abhörmaßnahmen geschützt werden. Das Problem des Abhörens wird gelöst, indem die Authentifizierungs-/Autorisierungsanfragen über das HTTPS-Protokoll gesendet werden. Der Manipulationsschutz wird durch eine digitale Signatur der Geräterkennungsinformationen gehandhabt. Die AccessEnabler-Bibliothek berechnet eine Geräte-ID aus den vom Gerät bereitgestellten Informationen und sendet dann die Geräte-ID „im Klartext“ als Abfrageparameter an die Adobe Pass-Authentifizierungsserver.  Die Adobe Pass-Authentifizierungsserver signieren die Geräte-ID digital mit dem privaten Schlüssel von Adobe und fügen sie dem Authentifizierungstoken hinzu, das an AccessEnabler zurückgegeben wird. Daher ist die Geräte-ID an das Authentifizierungs-Token gebunden.  Während des Autorisierungsflusses sendet der AccessEnabler die Geräte-ID erneut als gelöscht zusammen mit dem Authentifizierungstoken.  Ein Fehlschlagen des Validierungsprozesses führt automatisch zum Fehlschlagen der Authentifizierungs-/Autorisierungs-Workflows.  Die Adobe Pass-Authentifizierungsserver wenden den privaten Schlüssel auf die Geräte-ID an und vergleichen ihn mit dem Wert im Authentifizierungs-Token.  Wenn sie nicht übereinstimmen, schlägt dieser Berechtigungsfluss fehl.
