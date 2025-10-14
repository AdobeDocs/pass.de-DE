---
title: Fehlerberichterstattung
description: Fehlerberichterstattung
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3012'
ht-degree: 0%

---

# (Veraltete) Fehlerberichte {#error-reporting}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Übersicht {#overview}

Die Fehlerberichterstattung in der Adobe Pass-Authentifizierung wird derzeit auf zwei verschiedene Arten implementiert:

* **Erweiterte**: Das Implementierungsprogramm registriert einen Fehler-Callback im Fall des [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk) oder implementiert eine Schnittstellenmethode namens &quot;`status`&quot; im Fall des [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk) und [AccessEnabler Android SDK](#accessenabler-android-sdk), um erweiterte Fehlerberichte zu erhalten. Fehler werden in die Typen **Information**, **Warnung** und **Fehler** kategorisiert. Dieses Berichtssystem ist **asynchron** da **es keine Garantie für die Reihenfolge gibt, in der mehrere Fehler ausgelöst werden**.  Weitere Informationen zum erweiterten Fehlerberichterstattungssystem finden Sie im Abschnitt [Erweiterte &#x200B;](#advanced-error-reporting)).

* **Original Error Reporting -** Ein statisches Berichtssystem, in dem Fehlermeldungen an bestimmte Callback-Funktionen übergeben werden, wenn bestimmte Anfragen fehlschlagen. Fehler werden in generische, Authentifizierungs- und Autorisierungstypen unterteilt. Eine Liste der im Originalsystem gemeldeten Fehler finden Sie im Abschnitt [Original-](#original-error-reporting)).


## Erweiterte Fehlerberichterstattung {#advanced-error-reporting}

* [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [AccessEnabler Android SDK](#accessenabler-android-sdk)
* [AccessEnabler FireOS SDK](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>Die alte [Original Error Reporting](#original-error-reporting)-API funktioniert weiterhin wie zuvor, die erweiterte Fehlerberichterstattung beeinträchtigt die Funktionalität nicht, aber die ursprüngliche Fehlerberichterstattung erhält KEINE Aktualisierungen mehr. Alle neuen Fehler und Aktualisierungen werden im erweiterten Fehlerberichterstattungssystem vorgenommen.

### AccessEnabler JavaScript SDK {#accessenabler-javascript-sdk}

Das neue System zur Fehlerberichterstattung bleibt optional. Daher kann das Implementierungsprogramm einen Fehler-Handler-Callback explizit registrieren, um erweiterte Fehlerberichte zu erhalten. Das System bietet die Möglichkeit, mehrere Fehler-Callbacks dynamisch zu registrieren und ihre Registrierung aufzuheben. Darüber hinaus können Sie alle neuen Fehler-Callbacks registrieren, sobald die AccessEnabler JavaScript SDK geladen ist, ohne dass eine andere Initialisierung durchgeführt werden muss (vor dem Aufruf von `setRequestor()`), was bedeutet, dass Sie erweiterte Berichte über Initialisierungs- und Konfigurationsfehler erhalten können.


#### Implementierung {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


Ihre Fehler-Handler-Rückruffunktion erhält ein einzelnes Objekt (eine Zuordnung) mit der folgenden Struktur:

```JavaScript
    {
      errorId: "CFG410",
      level: "error",
      message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

### 1. Bindung {#bind}

**`.bind(eventType:String, handlerName:String):void`**

Fügt einen Handler für ein Ereignis an.

**`eventType`** - NUR der Wert &quot;`errorEvent`&quot; führt dazu, dass die AccessEnabler JavaScript SDK erweiterte Fehlerberichte auslöst.

**`handlerName`** - eine Zeichenfolge, die den Namen der Fehler-Handler-Funktion angibt.


Beide Bindungsparameter dürfen nur Zeichen aus dem folgenden Satz verwenden: `[0-9a-zA-Z][-._a-zA-Z0-9]`. Das heißt, Parameter müssen mit einer Zahl oder einem Buchstaben beginnen und dürfen dann nur Bindestriche, Punkte, Unterstriche und alphanumerische Zeichen enthalten.  Darüber hinaus dürfen Parameter 1024 Zeichen nicht überschreiten.

**Beispiel** von Handlern für Bindungsfehler:

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

Aufgrund technischer Einschränkungen ist es nicht möglich, einen Verschluss oder eine anonyme Funktion zu binden. Sie müssen den Namen der Methode im zweiten Parameter angeben.


### 2. Bindung aufheben {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

Entfernt einen zuvor angehängten Ereignishandler.

**`eventType`** - NUR der Wert &quot;`errorEvent`&quot; führt dazu, dass die AccessEnabler JavaScript SDK erweiterte Rückrufe von Fehlerberichten auslöst.

**`handlerName`** - Eine Zeichenfolge, die den Namen der Fehler-Handler-Funktion angibt, wenn null ist oder alle angehängten Handler für die angegebene `eventType` fehlen, wird entfernt.

Beide Bindungsparameter dürfen nur Zeichen aus dem folgenden Satz verwenden: `[0-9a-zA-Z][-._a-zA-Z0-9]`. Das heißt, Parameter müssen mit einer Zahl oder einem Buchstaben beginnen und dürfen dann nur Bindestriche, Punkte, Unterstriche und alphanumerische Zeichen enthalten.  Darüber hinaus dürfen Parameter 1024 Zeichen nicht überschreiten.

**Beispiel** Entfernen eines einzelnen Fehler-Handlers:

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**Beispiel** Entfernen aller Fehler-Handler:

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

Das neue Fehlermeldesystem ist obligatorisch, daher muss der Implementierer sich explizit an das neue Ziel-C-Protokoll „EntitlementStatus“ halten. Dieser neue Ansatz ermöglicht es Programmierern, erweiterte Fehlerberichte zu erhalten.

#### Implementierung {#accessenab-ios-tvossdk-imp}

Eine Implementierung muss dem folgenden &quot;**&quot;-** entsprechen:

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

Ihre **status**-Funktion erhält ein einzelnes Objekt (einen `NSDictionary`) mit der folgenden Struktur:

```OBJ-C
    {
          errorId: "CFG410",
          level: "error",
          message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

**1. Deklaration**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2. Implementierung**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### AccessEnabler Android SDK {#accessenabler-android-sdk}

Das neue Fehlerberichtssystem ist obligatorisch, da der Implementor explizit dem `IAccessEnablerDelegate` Schnittstellenprotokoll entsprechen muss. Dieser neue Ansatz ermöglicht es Programmierern, erweiterte Fehlerberichte zu erhalten.

#### Implementierung {#access-enablr-androidsdk-imp}

Ein Implementor muss die neue `status`-Methode von der -Schnittstelle aus `IAccessEnablerDelegate`. Die **`status`** erhält ein einzelnes **`AdvancedStatus`** mit dem folgenden Modell:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Beispiel**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

### AccessEnabler FireOS SDK {#accessenabler-fireos-sdk}


Das neue Fehlerberichtssystem ist obligatorisch, da der Implementor explizit dem `IAccessEnablerDelegate` Schnittstellenprotokoll entsprechen muss. Dieser neue Ansatz ermöglicht es Programmierern, erweiterte Fehlerberichte zu erhalten.

#### Implementierung {#access-enab-fireos-sdk-}

Ein Implementor muss die neue Methode `status`von der -Schnittstelle`IAccessEnablerDelegate` verarbeiten. Die **`status`** erhält ein einzelnes **`AdvancedStatus`** mit dem folgenden Modell:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Beispiel**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

## Referenz zu erweiterten Fehler-Codes {#advanced-error-codes-reference}

In der folgenden Tabelle sind die Fehler-Codes der neueren Fehler-API aufgeführt und beschrieben, zusammen mit empfohlenen Maßnahmen zu ihrer Korrektur:

| ID | Ebene | Beschreibung | Entwickleraktionen | Benutzeraktionen | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL UND AAPL_ERROR | Fehler | Generischer Apple-SSO-Fehler | Der Fehler enthält ein Detailfeld mit dem ursprünglichen VSA-Fehler. | Nicht zutreffend | Nicht zutreffend | Ja | Nicht zutreffend |
| VSA203 | INFO | Der Benutzer hat beschlossen, sich von der Anwendung abzumelden, während die Authentifizierung als Ergebnis einer Anmeldung über die SSO-Plattform erfolgte. | Weisen Sie den Benutzer an, sich explizit unter „Einstellungen“ > „Konten“ > „TV-Anbieter“ unter „tvOS“ abzumelden. <br><br> Weisen Sie den Benutzer an, sich explizit von „Einstellungen“ > „TV-Anbieter“ unter iOS/iPadOS abzumelden. | Melden Sie sich unter „Einstellungen“ > „Konten“ > „TV-Anbieter“ unter „tvOS“ explizit ab. <br> <br> Melden Sie sich unter „Einstellungen“ > „TV-Anbieter“ unter iOS/iPadOS explizit ab. | Nicht zutreffend | Ja | Nicht zutreffend |
| VSA404 | INFO | Die Berechtigung für das Abonnentenkonto des Anwendungsvideos wurde nicht bestimmt. | Indem Sie die Vorteile des Single-Sign-On (SSO)-Benutzererlebnisses erläutern, schaffen Sie Anreize für Benutzer, die sich weigern, Berechtigungen für den Zugriff auf Abonnementinformationen zu erteilen. | Der Benutzer kann seine Entscheidung ändern, indem er zu den Anwendungseinstellungen (Zugriff auf TV Provider) oder zum Abschnitt Einstellungen -> TV Provider auf iOS/iPadOS oder Einstellungen -> Konten -> TV Provider auf tvOS geht. | Nicht zutreffend | Ja | Nicht zutreffend |
| VSA503 | INFO | Anfrage für Metadaten des Abonnentenkontos des Anwendungsvideos fehlgeschlagen. | Der MVPD-Endpunkt reagiert nicht. Die Anwendung kann auf einen regulären Authentifizierungsfluss zurückgreifen. | Nicht zutreffend | Nicht zutreffend | Ja | Nicht zutreffend |
| 500 | Fehler | Interner Fehler | Verwenden Sie AccessEnablerDebug und überprüfen Sie die Debug-Protokolle (Ausgabe von console.log), um festzustellen, was schiefgelaufen ist. | Nicht zutreffend | Ja | Ja | Nicht zutreffend |
| SEK403 | Fehler | Domain-Sicherheitsfehler. Der Antragsteller verwendet eine ungültige Domain. Alle Domains, die von einer bestimmten Anforderer-ID verwendet werden, müssen per Adobe auf die Whitelist gesetzt werden. | - Laden Sie AccessEnabler nur aus der Liste der zulässigen Domains <br> <br> - Adobe kontaktieren, um die Domain-Zulassungsliste für die <br> verwendete Anforderer-ID zu verwalten <br> - iOS: Vergewissern Sie sich, dass Sie das richtige Zertifikat verwenden und dass die Signatur ordnungsgemäß erstellt wurde | Nicht zutreffend | Nicht zutreffend | Ja | Nicht zutreffend |
| SEK412 | Warnung | [In Version 2.5 verfügbar] Geräte-ID stimmt nicht überein. Dies kann vorkommen, wenn die zugrunde liegende Plattform ihre Geräte-ID ändert. In diesem Fall werden die vorhandenen Token gelöscht und der Benutzer wird nicht mehr authentifiziert. Beachten Sie, dass dies rechtmäßig geschieht, wenn der Benutzer die JS-SDK verwendet und Roaming durchführt (bei JS ist die Client-IP Teil der Geräte-ID). Andernfalls könnte dies ein Hinweis auf einen Betrugsversuch sein - einen Versuch, Token von einem anderen Gerät zu kopieren. | - Überwachen Sie die Anzahl der Warnungen. Wenn sie aus keinem ersichtlichen Grund (keine aktuellen Browser-Updates, neue Betriebssysteme) eine Spitze bilden, kann dies ein Hinweis auf Betrugsversuche sein.  <br> <br>- Optional informieren Sie den Benutzer, dass er sich erneut anmelden muss. | Melden Sie sich erneut an. | Ja | Ja | Ja ab 3.2 |
| SEK420 | Fehler | HTTP-Sicherheitsfehler bei der Kommunikation mit Adobe Pass-Authentifizierungsservern. Dieser Fehler tritt normalerweise auf, wenn Spoofing/Proxys vorhanden sind. | - Laden Sie `[https://]{SP_FQDN\}` im Browser und akzeptieren Sie die SSL-Zertifikate manuell, z. B. **https://api.auth.adobe.com** oder **https://api.auth-staging.adobe.com** <br> <br>- Markieren Sie die Proxy-Zertifikate als vertrauenswürdig | Wenn dies bei einem normalen Benutzer auftritt, ist es ein Hinweis auf einen möglichen Man-in-the-Middle-Angriff! | Ja | Ja | Ja ab 3.2 |
| CFG100 | Warnung | Datum/Uhrzeit/Zeitzone des Client-Computers wurde nicht korrekt festgelegt. Dies führt wahrscheinlich zu Authentifizierungs-/Autorisierungsfehlern. | - Informieren Sie den Benutzer, um die richtige Zeit einzustellen. <br> <br> ergreifen Maßnahmen, um Flüsse von Ansprüchen zu verhindern, da diese wahrscheinlich fehlschlagen werden. | Stellen Sie das richtige Datum / die richtige Uhrzeit ein. | Ja | Ja | Ja ab 3.2 |
| CFG400 | Fehler | Es wurde eine ungültige Anforderer-ID angegeben. | Der Entwickler MUSS eine gültige Anforderer-ID angeben. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| CFG404 | Fehler | Die Adobe Pass-Authentifizierungsserver wurden nicht gefunden. Dies kann in drei Instanzen passieren: <br><br> - Der Entwickler verfügt über ein ungültiges Spoofing. <br><br> : Der/die Benutzende hat Netzwerkprobleme und kann die Adobe Pass-Authentifizierungs-Domains nicht erreichen. <br><br> -Adobe Pass-Authentifizierungsserver sind falsch konfiguriert. <br><br>  **Hinweis:** In Firefox wird CFG400 anstelle von CFG404 angezeigt (Browser-Einschränkung) | - Spoofing überprüfen. <br><br> - Prüfen Sie die Netzwerk-/DNS-Einstellungen. <br><br> -Inform Adobe. | Prüfen Sie die Netzwerk-/DNS-Einstellungen. | Ja | Ja | Ja ab 3.2 |
| CFG410 | Fehler | AccessEnabler ist zu alt. | Informieren Sie den Benutzer zum Löschen von Caches. | Löschen Sie den Browser-Cache. | Ja | Nicht zutreffend | Ja ab 3.2 |
| CFG5xx | Fehler | Auf den Adobe Pass-Authentifizierungsservern treten interne Fehler auf. xx kann eine beliebige Zahl sein. | - Informieren Sie den Benutzer darüber, dass die Adobe Pass-Authentifizierung nicht verfügbar ist. <br><br> - Adobe Pass-Authentifizierung umgehen. <br> <br> - Adobe informieren. | Später versuchen. | Ja | Ja | Ja ab 3.2 |
| N000 | INFO | Der Benutzer ist nicht authentifiziert. | Nicht zutreffend | Anmelden. | Ja | Ja | Ja ab 3.2 |
| N001 | INFO | Im Hintergrund wurde ein passiver Authentifizierungsversuch gestartet. Dies geschieht bei MVPDs, die mit „Authentication Per Requestor“ konfiguriert sind. Während der Benutzer hoffentlich automatisch authentifiziert wird, verursacht dies bei der Initialisierung eine Leistungsbeeinträchtigung. | Optional können Sie den Benutzer oder die vorliegende Benutzeroberfläche, die den Benutzer benachrichtigt, darüber informieren, dass „Arbeiten in Bearbeitung sind“. | Warte. | Ja | Ja | Ja ab 3.2 |
| N003 | INFO | Der Benutzer wählt die Option „Andere TV-Anbieter“ in der Apple MVPD-Auswahl aus. | Der *displayProviderDialog*-Rückruf wird aufgerufen und die Anwendung könnte auf den regulären Authentifizierungsfluss zurückgreifen. | Wählen Sie Reguläre MVPD aus und fahren Sie mit dem Anmeldebildschirm fort. | Nicht zutreffend | Ja | Nicht zutreffend |
| N004 | INFO | Der Benutzer wählt einen TV-Anbieter aus, der vom aktuellen Anforderer nicht unterstützt wird. | Der *displayProviderDialog*-Rückruf wird aufgerufen und die Anwendung könnte auf den regulären Authentifizierungsfluss zurückgreifen. | Wählen Sie Reguläre MVPD aus und fahren Sie mit dem Anmeldebildschirm fort. | Nicht zutreffend | Ja | Nicht zutreffend |
| N005 | INFO | Die MVPD-Auswahl wurde abgebrochen. | Nicht zutreffend | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| N010 | Warnung | Die Benutzerin bzw. der Benutzer wurde authentifiziert, während die Authentifizierungsregel für alle Beeinträchtigungen für die ausgewählte MVPD angewendet wurde. | Optional kann der Benutzer darüber informiert werden, dass er aufgrund von MVPD-Schwierigkeiten einen „kostenlosen“ freien Zugang erhält. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| N011 | INFO | Der Benutzer wurde mit TempPass authentifiziert. | - Informieren Sie den Benutzer. <br> <br> : Optional eine Liste der regulären MVPDs. | Optional können Sie sich mit Ihrer regulären MVPD anmelden. | Ja | Ja | Ja ab 3.2 |
| N111 | Warnung | Abgelaufene temporäre Kennzahl. | - Benutzer informieren. <br> <br> - Präsentieren Sie eine Liste der regulären MVPDs. <br> <br> - Blendet die Option TempPass aus. | Melden Sie sich mit Ihrer regulären MVPD an. | Ja | Ja | Ja ab 3.2 |
| N130 | Fehler | **Authentifizierungstoken nicht in Sitzung gefunden.** Dies kann eine der folgenden Ursachen haben: <br> <br> 1. Im Browser sind Cookies von (Drittanbietern) deaktiviert (gilt nicht für AccessEnabler JavaScript SDK Version 4.x) <br> <br> 2. Browser hat Prevent cross-site tracking aktiviert (Safari 11+) <br> <br> 3. Sitzung ist <br> abgelaufen <br> 4. Programmierer ruft Authentifizierungs-APIs in falscher <br> auf <br> HINWEIS: Dieser Fehler-Code ist nicht für Flüsse der vollständigen Umleitungs-Authentifizierung verfügbar. | 1. Benutzer auffordern, Cookies von (Drittanbietern) zu aktivieren <br> <br> 2. Benutzer auffordern, Site-übergreifende Tracking-<br> zu deaktivieren <br> 3. Benutzer auffordern, sich erneut zu authentifizieren <br> <br> 4. Aufrufen von APIs in der richtigen Reihenfolge | 1. Aktivieren von Cookies (von Drittanbietern) <br> <br> 2. Site-übergreifendes Tracking <br> deaktivieren <br> 3. <br> erneut authentifizieren <br> 4. Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| N500 | Fehler | Interner Fehler. <br> <br> Hinweis: Dies ist der „Generische Authentifizierungsfehler“ und „Interner Authentifizierungsfehler“ des ursprünglichen Fehlersystems. Dieser Fehler wird schließlich schrittweise behoben. | Verwenden Sie AccessEnablerDebug und überprüfen Sie die Debug-Protokolle (Ausgabe von console.log), um festzustellen, was schiefgelaufen ist. | Nicht zutreffend | Ja | Ja | Nicht zutreffend |
| R401 | Fehler | Beim Abrufen eines Zugriffs-Tokens ist ein Fehler aufgetreten. <br> <br> Hinweis: Dies ist ein nicht behebbarer Fehler. Informieren Sie den Benutzer, dass die Anwendung nicht verfügbar ist. | - iOS: Überprüfen Sie die Softwareübersicht und die benutzerdefinierten Schemata in Ihrer Anwendung. <br> <br> - JavaScript: Überprüfen Sie die Software-Anweisung in Ihrer Website-Anwendung. <br> <br> Öffnen Sie ein Ticket mit Zendesk und informieren Sie den Benutzer, dass das System vorübergehend nicht verfügbar ist | Nicht zutreffend | Ja, ab Version 4.0 | Ja ab v3.0 | Ja ab 3.2 |
| R400 | Fehler | Die Anwendung ist nicht registriert. Die Software-Anweisung ist ungültig oder wurde widerrufen. <br> <br> Hinweis: Dies ist ein nicht behebbarer Fehler. Informieren Sie den Benutzer, dass die Anwendung nicht verfügbar ist. | - iOS: Überprüfen Sie die Softwareübersicht und die benutzerdefinierten Schemata in Ihrer Anwendung. <br> <br> - JavaScript: Überprüfen Sie die Software-Anweisung in Ihrer Website-Anwendung. <br> <br> Öffnen Sie ein Ticket mit Zendesk und informieren Sie den Benutzer, dass das System vorübergehend nicht verfügbar ist | Nicht zutreffend | Ja, ab Version 4.0 | Ja ab v3.0 | Ja ab 3.2 |
| REG500 | Fehler | Registrierungs-Code konnte nicht vom Server abgerufen werden. <br> <br> Hinweis: Dies ist ein nicht behebbarer Fehler. Informieren Sie den Benutzer, dass die Anwendung nicht verfügbar ist. | Öffnen Sie ein Ticket mit Zendesk und informieren Sie den Benutzer, dass das System vorübergehend nicht verfügbar ist. | Nicht zutreffend | Ja, ab Version 4.0 | Ja ab v3.0 | Ja ab 3.2 |
| NEU CODIEREN | Erfolg | Die Anwendung hat die setSelectedProvider-API auf der tvOS-Plattform aufgerufen. | Weisen Sie den Benutzer an, ein zweites Gerät (Bildschirm) zu verwenden, um sich mit dem bereitgestellten Registrierungs-Code anzumelden. | Verwenden Sie den Regcode auf einem zweiten Gerät (Bildschirm), um die Authentifizierung zu starten. | Nicht zutreffend | Ja Nur für tvOS | Nicht zutreffend |
| Z010 | Warnung | Die Benutzerin bzw. der Benutzer wurde autorisiert, während die Degradationsregel „Authenticate-all“ oder „authorize-all“ für die ausgewählte MVPD vorhanden war. | Optional kann der Benutzer darüber informiert werden, dass er aufgrund von MVPD-Schwierigkeiten einen „kostenlosen“ freien Zugang erhält. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z011 | INFO | Benutzer wurde mit TempPass autorisiert | Optional den Benutzer informieren | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z100 | Fehler | Die Autorisierung ist fehlgeschlagen, weil der Benutzer kein Abonnement für die angeforderte Ressource hat oder aus anderen Gründen, die von der MVPD stammen, z. B. weil das Video nicht mit den Kindersicherungseinstellungen für das Benutzerkonto übereinstimmt | - Wiedergabe nicht zulassen. <br> <br> - Informieren Sie den Benutzer. <br> <br> - Der Schlüssel &#39;message&#39; in der Fehlermeldung kann eine detailliertere Meldung enthalten, die von der MVPD bereitgestellt wird. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z110 | Fehler | Autorisierung aufgrund von wiederholten MVPD-Ablehnungen verweigert. Mögliche Betrugsversuche oder DOS. | - Wiedergabe nicht zulassen. <br> <br> - Informieren Sie den Benutzer. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z120 | Fehler | Die Genehmigung für die Kommunikation mit der MVPD wurde aus technischen Gründen verweigert. Mögliche Netzwerkfehler. | - Wiedergabe nicht zulassen. <br> <br> : Informieren Sie den Benutzer, dass beim MVPD Probleme aufgetreten sind, und versuchen Sie es später erneut. | Später versuchen. | Ja | Ja | Ja ab 3.2 |
| Z130 | Fehler | Autorisierung verweigert, da eine ungültige/falsch formatierte Ressource verwendet wurde. | Überprüfen Sie die Ressourcenzeichenfolge und korrigieren Sie sie. Im Allgemeinen ist dieser Fehler auf eine fehlerhafte MRSS oder die Verwendung einer einfachen Zeichenfolge anstelle von MRSS zurückzuführen. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z169 | Fehler | Autorisierung verweigert, da die Degradationsregel authNone für die angegebene Ressource angewendet wurde. | Informieren des Benutzers | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z500 | Fehler | Interner Fehler. <br> <br> Hinweis: Dies ist der alte „Generischer Authentifizierungsfehler“ und „Interner Authentifizierungsfehler“. Dieser Fehler wird schließlich schrittweise behoben. | Verwenden Sie AccessEnablerDebug und überprüfen Sie die Debug-Protokolle (Ausgabe von console.log), um festzustellen, was schiefgelaufen ist. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| P100 | Fehler | Vorautorisierung fehlgeschlagen. Dies ist wahrscheinlich auf die Anforderung einer Autorisierung für zu viele Ressourcen zurückzuführen. | - Verwenden Sie NICHT mehr als die maximal zulässige Anzahl von Ressourcen. <br> <br> - Wenden Sie sich an den Adobe Pass-Authentifizierungs-Support, um die maximal zulässige Anzahl von Ressourcen zu finden bzw. einzurichten. | Nicht zutreffend | Ja ab v3.0 | Ja | Ja ab 3.2 |
| IS2XX | Fehler | Diese Fehler-Codes werden zurückgegeben, wenn die Antwortdaten des Individualisierungs-Server-Endpunkts ein ungültiges Format aufweisen oder die erforderlichen Individualisierungsinformationen fehlen. | Öffnen Sie ein Ticket mit Zendesk und informieren Sie den Benutzer, dass das System vorübergehend nicht verfügbar ist | Nicht zutreffend | Ja ab v3.0 | Nicht zutreffend | Nicht zutreffend |
| IS4XX | Fehler | Diese Fehler-Codes werden im Fall eines Individualisierungs-Server-Endpunktfehlers 4XX zurückgegeben - ist der HTTP-Status-Code der Antwort. | Öffnen Sie ein Ticket mit Zendesk und informieren Sie den Benutzer, dass das System vorübergehend nicht verfügbar ist | Nicht zutreffend | Ja ab v3.0 | Nicht zutreffend | Nicht zutreffend |
| IS5XX | Fehler | Diese Fehler-Codes werden im Fall eines Individualisierungs-Server-Endpunktfehlers 5XX zurückgegeben - ist der HTTP-Status-Code der Antwort. | Öffnen Sie ein Ticket mit Zendesk und informieren Sie den Benutzer, dass das System vorübergehend nicht verfügbar ist | Nicht zutreffend | Ja ab v3.0 | Nicht zutreffend | Nicht zutreffend |
| IS0 | Fehler | Dieser Code wird zurückgegeben, wenn der Endpunkt des Individualisierungs-Servers überhaupt nicht reagiert hat. Daher ist die Verbindung abgelaufen | Öffnen Sie ein Ticket mit Zendesk und informieren Sie den Benutzer, dass das System vorübergehend nicht verfügbar ist | Nicht zutreffend | Ja ab v3.0 | Nicht zutreffend | Nicht zutreffend |
| LS011 | Warnung | AccessEnabler verwendet einen flüchtigen Speicher, da LSO/LocalStorage-Probleme und WebStorage-Probleme (oder Nichtverfügbarkeit) auftreten. <br> <br> Authentifizierung/Autorisierung bleibt nicht über die aktuelle Seite hinaus bestehen! Bei jedem Laden der Seite muss sich der Benutzer authentifizieren. Konfigurierte TTLs werden nicht über Seitenneuladungen hinweg erzwungen. | - Informieren Sie den Benutzer über Einschränkungen. <br> <br> : Informieren Sie den Benutzer darüber, wie der verfügbare Speicherplatz erhöht werden kann. <br> <br> - Alternativ können Sie sich abmelden, um den Speicher zu löschen. | - Erhöhen Sie die Speicherkapazität. <br> <br> - Melden Sie sich ab, um den Speicher zu leeren. | Ja | Nicht zutreffend | Nicht zutreffend |

<br>

## Ursprüngliche Fehlerberichterstattung {#original-error-reporting}

In diesem Abschnitt werden das ursprüngliche Fehlermeldesystem sowie die ursprünglichen Fehlercodes beschrieben. Im ursprünglichen Fehlermeldesystem übergibt AccessEnabler Fehler an diese beiden Callback-Funktionen: `setAuthenticationStatus()` nach einem Aufruf an `checkAuthentication()`; `tokenRequestFailed()` nach dem Ausfall eines Aufrufs an `checkAuthorization()` oder `getAuthorization()`.

Die ursprünglichen Fehler-Reporting- und Status-APIs funktionieren weiterhin genau wie zuvor. Künftig werden die ursprünglichen Fehler-Reporting-APIs jedoch nicht mehr aktualisiert. Alle neuen Fehlermeldungen und Aktualisierungen zu den alten Fehlern werden NUR im neuen [Advanced Error Reporting System“ &#x200B;](#advanced-error-reporting).


Beispiele für die Verwendung des ursprünglichen Fehlerberichtssystems finden Sie in den Funktionen [JavaScript-API-Referenz](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) und [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#token-request-failed-error-msg), [iOS/tvOS-API-Referenz](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setAuthNStatus)and [tokentRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#tokenReqFailed), [Android-API-Referenz](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus) und [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### Ursprüngliche Callback-Fehlercodes {#original-callback-error-codes}

| **Allgemeine Fehler** | |
|---|---|
| Interner Fehler | Bei der Verarbeitung der Anfrage ist ein Systemfehler aufgetreten. |
| Fehler „Anbieter nicht ausgewählt“ | Tritt auf, wenn der Kunde im Dialogfeld für die Anbieterauswahl abbricht. |
| Fehler „Anbieter nicht verfügbar“ | Tritt auf, wenn keine Anbieter verfügbar sind. |
|  |  |
| **Authentifizierungsfehler** | |
| Allgemeiner Authentifizierungsfehler | Wird zurückgegeben, wenn der Grund unbekannt ist oder nicht veröffentlicht werden kann. |
| Interner Authentifizierungsfehler | Beim Authentifizierungsversuch ist ein Systemfehler aufgetreten. |
| Fehler „Benutzer nicht authentifiziert“ | Benutzer ist nicht authentifiziert. |
|  |  |
| **Autorisierungsfehler** |  |
| Allgemeiner Autorisierungsfehler | Wird zurückgegeben, wenn der Grund unbekannt ist oder nicht veröffentlicht werden kann. |
| Interner Autorisierungsfehler | Beim Autorisierungsversuch ist ein Systemfehler aufgetreten. |
| Fehler „Benutzer nicht autorisiert“ | Der Kunde ist nicht berechtigt, die angeforderten Inhalte anzuzeigen. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
