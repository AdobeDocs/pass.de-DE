---
title: Fehlerberichte
description: Fehlerberichte
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2989'
ht-degree: 0%

---

# Fehlerberichte {#error-reporting}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Übersicht {#overview}

Die Fehlerberichterstellung in der Adobe Pass-Authentifizierung ist derzeit auf zwei verschiedene Arten implementiert:

* **Erweiterte Fehlerberichte** Der Implementor registriert einen Fehler-Callback im Fall von [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk) oder implementiert im Fall von [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk) und dem [AccessEnabler Android SDK](#accessenabler-android-sdk) eine Schnittstellenmethode mit dem Namen &quot;`status`&quot;, um erweiterte Fehlerberichte zu erhalten. Fehler werden in die Typen **Information**, **Warnung** und **Fehler** kategorisiert. Dieses Berichterstellungssystem ist **asynchron**, da **keine Garantie für die Reihenfolge gibt, in der mehrere Fehler ausgelöst werden.**  Weitere Informationen zum erweiterten Fehlermeldesystem finden Sie im Abschnitt [Erweiterte Fehlerberichte](#advanced-error-reporting) .

* **Ursprüngliche Fehlerberichte -** Ein statisches Berichterstattungssystem, in dem Fehlermeldungen an bestimmte Callback-Funktionen weitergegeben werden, wenn bestimmte Anforderungen fehlschlagen. Fehler werden in allgemeine, Authentifizierungs- und Autorisierungstypen gruppiert. Eine Liste der im Originalsystem gemeldeten Fehler finden Sie im Abschnitt [Ursprüngliche Fehlerberichte](#original-error-reporting) .


## Fortschrittliche Fehlerberichte {#advanced-error-reporting}

* [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [AccessEnabler Android SDK](#accessenabler-android-sdk)
* [AccessEnabler FireOS-SDK](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>Die alte API [Ursprüngliche Fehlerberichte](#original-error-reporting) funktioniert weiterhin wie zuvor. Die erweiterten Fehlerberichte beeinträchtigen die Funktionalität nicht, aber die ursprünglichen Fehlerberichte erhalten KEINE Aktualisierungen mehr. Alle neuen Fehler und Aktualisierungen werden im erweiterten Fehlermeldesystem vorgenommen.

### AccessEnabler JavaScript SDK {#accessenabler-javascript-sdk}

Das neue Fehlermeldesystem ist optional. Daher kann der Implementor einen Rückruf des Fehler-Handlers explizit registrieren, um erweiterte Fehlerberichte zu erhalten. Das System bietet die Möglichkeit, mehrere Fehler-Rückrufe dynamisch zu registrieren und zu deaktivieren. Darüber hinaus können Sie alle neuen Fehlerrückrufe registrieren, sobald das AccessEnabler JavaScript SDK geladen wird, ohne eine andere Initialisierung durchführen zu müssen (vor dem Aufruf von `setRequestor()`). Dies bedeutet, dass Sie erweiterte Berichte zu Initialisierungs- und Konfigurationsfehlern erhalten können.


#### Implementierung {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


Ihre Rückruffunktion des Fehler-Handlers erhält ein einzelnes Objekt (eine Zuordnung) mit der folgenden Struktur:

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

Hängt einen Handler für ein Ereignis an.

**`eventType`** - NUR der Wert &quot;`errorEvent`&quot; führt dazu, dass das AccessEnabler JavaScript SDK erweiterte Fehlerberichte auslöst.

**`handlerName`** - eine Zeichenfolge, die den Namen der Fehler-Handler-Funktion angibt.


Beide Bindungsparameter dürfen nur Zeichen des folgenden Satzes verwenden: `[0-9a-zA-Z][-._a-zA-Z0-9]`; d. h. Parameter müssen mit einer Zahl oder einem Buchstaben beginnen und dürfen dann nur Bindestriche, Punkte, Unterstriche und alphanumerische Zeichen enthalten.  Darüber hinaus dürfen Parameter 1024 Zeichen nicht überschreiten.

**Beispiel** der Binding-Fehler-Handler:

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

Aufgrund technischer Einschränkungen können Schließungen oder anonyme Funktionen nicht gebunden werden. Sie müssen den Namen der Methode im zweiten Parameter angeben.


### 2. Bindung aufheben {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

Entfernt einen zuvor angehängten Ereignishandler.

**`eventType`** - NUR der Wert &#39;`errorEvent`&#39; führt dazu, dass das AccessEnabler JavaScript SDK erweiterte Fehlerberichte auslöst.

**`handlerName`** - Eine Zeichenfolge, die den Namen der Fehler-Handler-Funktion angibt. Wenn null ist oder fehlen, werden alle angehängten Handler für das angegebene `eventType` entfernt.

Beide Bindungsparameter dürfen nur Zeichen des folgenden Satzes verwenden: `[0-9a-zA-Z][-._a-zA-Z0-9]`; d. h. Parameter müssen mit einer Zahl oder einem Buchstaben beginnen und dürfen dann nur Bindestriche, Punkte, Unterstriche und alphanumerische Zeichen enthalten.  Darüber hinaus dürfen Parameter 1024 Zeichen nicht überschreiten.

**Beispiel** zum Entfernen eines einzelnen Fehler-Handlers:

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**Beispiel** Entfernen aller Fehler-Handler:

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

Das neue Fehlermeldesystem ist obligatorisch, daher muss der Implementor explizit dem neuen Objective C-&quot;EntitlementStatus&quot;-Protokoll entsprechen. Dieser neue Ansatz ermöglicht es Programmierern, erweiterte Fehlerberichte zu erhalten.

#### Implementierung {#accessenab-ios-tvossdk-imp}

Ein Implementor muss dem folgenden **EntitlementStatus** -Protokoll entsprechen:

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

Ihre **status** -Funktion erhält ein einzelnes Objekt (ein `NSDictionary`) mit der folgenden Struktur:

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

Das neue Fehlermeldesystem ist obligatorisch, da der Implementor explizit dem in der `IAccessEnablerDelegate` -Schnittstelle definierten Protokoll entsprechen muss. Dieser neue Ansatz ermöglicht es Programmierern, erweiterte Fehlerberichte zu erhalten.

#### Implementierung {#access-enablr-androidsdk-imp}

Ein Implementor muss die neue `status` -Methode über die Schnittstelle`IAccessEnablerDelegate` verarbeiten. Die Funktion **`status`** erhält ein einzelnes **`AdvancedStatus`** -Objekt mit dem folgenden Modell:

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

### AccessEnabler FireOS-SDK {#accessenabler-fireos-sdk}


Das neue Fehlermeldesystem ist obligatorisch, da der Implementor explizit dem in der `IAccessEnablerDelegate` -Schnittstelle definierten Protokoll entsprechen muss. Dieser neue Ansatz ermöglicht es Programmierern, erweiterte Fehlerberichte zu erhalten.

#### Implementierung {#access-enab-fireos-sdk-}

Ein Implementor muss die neue `status`Methode über die Schnittstelle`IAccessEnablerDelegate` verarbeiten. Die Funktion **`status`** erhält ein einzelnes **`AdvancedStatus`** -Objekt mit dem folgenden Modell:

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

## Referenz zu erweiterten Fehlercodes {#advanced-error-codes-reference}

In der folgenden Tabelle sind die von der neueren Fehler-API offen gelegten Fehler-Codes sowie die zu ihrer Korrektur empfohlenen Maßnahmen aufgeführt und beschrieben:

| ID | Ebene | Beschreibung | Entwickleraktionen | Benutzeraktionen | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp; AAPL_ERROR | Fehler | Allgemeiner Apple SSO-Fehler | Der Fehler enthält ein Detailfeld mit dem ursprünglichen VSA-Fehler. | Nicht zutreffend | Nicht zutreffend | Ja | Nicht zutreffend |
| VSA203 | Info | Der Benutzer entschied sich, sich von der Anwendung abzumelden, während die Authentifizierung aufgrund einer Anmeldung über die Plattform-SSO erfolgte. | Weisen Sie den Benutzer an/fordern Sie ihn auf, sich explizit unter Einstellungen > Konten > TV-Anbieter unter tvOS abzumelden. <br><br> Weisen Sie den Benutzer an bzw. fordern Sie ihn auf, sich unter &quot;Einstellungen&quot;-> &quot;TV-Anbieter&quot;unter iOS/iPadOS explizit abzumelden. | Melden Sie sich explizit von Einstellungen > Konten > TV-Anbieter unter tvOS ab. <br> <br> Explizites Abmelden von Einstellungen -> TV-Anbieter unter iOS/iPadOS | Nicht zutreffend | Ja | Nicht zutreffend |
| VSA404 | Info | Die Berechtigung für das Konto für Abonnenten von Anwendungsvideos ist unbestimmt. | Ermöglichen Sie Benutzern, die sich weigern, Zugriff auf Abonnementinformationen zu gewähren, indem Sie die Vorteile des Single Sign-On (SSO)-Benutzererlebnisses erläutern. | Der Benutzer kann seine Entscheidung ändern, indem er zu den Anwendungseinstellungen (TV Provider-Zugriff) oder zu Einstellungen > TV-Anbieter unter iOS/iPadOS oder Einstellungen > Konten > TV-Anbieter unter tvOS navigiert. | Nicht zutreffend | Ja | Nicht zutreffend |
| VSA 503 | Info | Metadaten-Anfrage für das App-Video-Abonnentenkonto fehlgeschlagen. | Der MVPD-Endpunkt reagiert nicht. Die Anwendung könnte auf einen normalen Authentifizierungsfluss zurückgreifen. | Nicht zutreffend | Nicht zutreffend | Ja | Nicht zutreffend |
| 500 | Fehler | Interner Fehler | Verwenden Sie AccessEnablerDebug und überprüfen Sie Debug-Protokolle (console.log output), um festzustellen, was schief gelaufen ist. | Nicht zutreffend | Ja | Ja | Nicht zutreffend |
| SEK 403 | Fehler | Domänensicherheitsfehler. Der Anfragende verwendet eine ungültige Domäne. Alle Domänen, die von einer bestimmten Anforderer-ID verwendet werden, müssen von Adobe auf die Whitelist gesetzt werden. | - Laden Sie den AccessEnabler nur aus der Liste der zulässigen Domänen <br> <br> - Kontaktieren Sie Adobe, um die Domain-Whitelist für die Anforderungs-ID zu verwalten, die <br> verwendet wird. <br> - iOS: Überprüfen Sie, ob Sie das richtige Zertifikat verwenden und die Signatur ordnungsgemäß erstellt wurde. | Nicht zutreffend | Nicht zutreffend | Ja | Nicht zutreffend |
| SEK 412 | Warnung | [In Version 2.5 verfügbar] Geräte-ID stimmt nicht überein. Dies kann vorkommen, wenn die zugrunde liegende Plattform ihre Geräte-ID ändert. In diesem Fall werden die vorhandenen Token gelöscht und der Benutzer wird nicht mehr authentifiziert. Beachten Sie, dass dies rechtmäßig geschieht, wenn der Benutzer das JS-SDK verwendet und das Roaming durchführt (auf JS ist die Client-IP Teil der Geräte-ID). Andernfalls könnte dies ein Hinweis auf einen Betrugsversuch sein - ein Versuch, Token von einem anderen Gerät zu kopieren. | - Überwachen Sie die Anzahl der Warnungen. Wenn sie aus keinem sichtbaren Grund (keine aktuellen Browser-Updates, neue Betriebssysteme), die ein Indikator für Betrugsversuche sein könnten.  <br> <br> - Optional können Sie den Benutzer darüber informieren, dass er sich erneut anmelden muss. | Melden Sie sich erneut an. | Ja | Ja | Ja ab 3.2 |
| SEK 420 | Fehler | HTTP-Sicherheitsfehler bei der Kommunikation mit Adobe Pass-Authentifizierungsservern. Dieser Fehler tritt normalerweise auf, wenn Spoofing/Proxys vorhanden sind. | - Laden Sie `[https://]{SP_FQDN\}` in den Browser und akzeptieren Sie die SSL-Zertifikate manuell, z. B. **https://api.auth.adobe.com** oder **https://api.auth-staging.adobe.com** <br> <br> - Markieren Sie die Proxy-Zertifikate als vertrauenswürdig. | Wenn dies für einen normalen Benutzer auftritt, ist dies ein Hinweis auf einen möglichen Man-in-the-Middle-Angriff! | Ja | Ja | Ja ab 3.2 |
| CFG 100 | Warnung | Der Client-Computer Datum/Uhrzeit/Zeitzone ist nicht korrekt eingestellt. Dies führt wahrscheinlich zu Authentifizierungs-/Autorisierungsfehlern. | - Informieren Sie den Benutzer darüber, die richtige Zeit festzulegen. <br> <br> Ergreifen Sie Maßnahmen, um Berechtigungsflüsse zu verhindern, da sie wahrscheinlich fehlschlagen werden. | Legen Sie das richtige Datum/die richtige Uhrzeit fest. | Ja | Ja | Ja ab 3.2 |
| CFG 400 | Fehler | Eine ungültige Anforderungs-ID wurde angegeben. | Der Entwickler MUSS eine gültige Anforderungs-ID angeben. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| CFG 404 | Fehler | Die Adobe Pass-Authentifizierungsserver wurden nicht gefunden. Dies kann in 3 Instanzen passieren: <br><br> - Der Entwickler hat einen ungültigen Spoofing eingerichtet. <br><br> - Der Benutzer hat Netzwerkprobleme und kann die Adobe Pass-Authentifizierungsdomänen nicht erreichen. <br><br> - Adobe Pass-Authentifizierungsserver sind falsch konfiguriert. <br><br>  **Hinweis:** In Firefox wird CFG400 anstelle von CFG404 angezeigt (Browserbeschränkung). | - Überprüfen Sie den Spoofing. <br><br> - Überprüfen Sie die Netzwerk-/DNS-Einstellungen. <br><br> -Inform Adobe. | Überprüfen Sie die Netzwerk-/DNS-Einstellungen. | Ja | Ja | Ja ab 3.2 |
| CFG 410 | Fehler | AccessEnabler ist zu alt. | Informieren den Benutzer darüber, dass er Zwischenspeicher löschen soll. | Löschen Sie den Browser-Cache. | Ja | Nicht zutreffend | Ja ab 3.2 |
| CFG5xx | Fehler | Bei den Adobe Pass-Authentifizierungsservern treten interne Fehler auf. xx kann eine beliebige Zahl sein. | - Informieren Sie den Benutzer über die Nichtverfügbarkeit der Adobe Pass-Authentifizierung. <br><br> - Umgehen Sie die Adobe Pass-Authentifizierung. <br> <br> - Inform Adobe. | Versuchen Sie es später. | Ja | Ja | Ja ab 3.2 |
| N000 | Info | Der Benutzer ist nicht authentifiziert. | Nicht zutreffend | Melden Sie sich an. | Ja | Ja | Ja ab 3.2 |
| N001 | Info | Ein passiver Authentifizierungsversuch wurde im Hintergrund gestartet. Dies geschieht für MVPDs, die mit &quot;Authentifizierung pro Anforderer&quot;konfiguriert wurden. Während der Benutzer hoffentlich automatisch authentifiziert wird, wird dies bei der Initialisierung zu einer Leistungsbeeinträchtigung. | Optional können Sie den Benutzer informieren oder die Benutzeroberfläche anzeigen, die den Benutzer darauf hinweist, dass die Arbeit noch läuft. | Warten Sie! | Ja | Ja | Ja ab 3.2 |
| N003 | Info | Der Benutzer wählt in der Apple-MVPD-Auswahl die Option &quot;Andere TV-Anbieter&quot;aus. | Der Rückruf *displayProviderDialog* wird aufgerufen und die Anwendung könnte auf den normalen Authentifizierungsfluss zurückgreifen. | Wählen Sie reguläres MVPD aus und fahren Sie mit dem Anmeldebildschirm fort. | Nicht zutreffend | Ja | Nicht zutreffend |
| N004 | Info | Der Benutzer wählt einen TV-Anbieter aus, der vom aktuellen Anfragenden nicht unterstützt wird. | Der Rückruf *displayProviderDialog* wird aufgerufen und die Anwendung könnte auf den normalen Authentifizierungsfluss zurückgreifen. | Wählen Sie reguläres MVPD aus und fahren Sie mit dem Anmeldebildschirm fort. | Nicht zutreffend | Ja | Nicht zutreffend |
| N005 | Info | Die MVPD-Auswahl wurde abgebrochen. | Nicht zutreffend | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| N010 | Warnung | Der Benutzer wurde authentifiziert, während die Authentifizierungsregel für den gesamten Abbau für den ausgewählten MVPD vorhanden war. | Sie können den Benutzer darüber informieren, dass er aufgrund von MVPD-Schwierigkeiten &quot;kostenlosen&quot; freien Zugang erhält. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| N011 | Info | Der Benutzer wurde mit TempPass authentifiziert. | - Informieren Sie den Benutzer. <br> <br> - Optional eine Liste mit regulären MVPDs anzeigen. | Optional können Sie sich mit Ihrem normalen MVPD anmelden. | Ja | Ja | Ja ab 3.2 |
| N111 | Warnung | Abgelaufener TempPass. | - Benutzer informieren. <br> <br> - Präsentieren Sie eine Liste der regulären MVPDs. <br> <br> - Blendet die Option &quot;TempPass&quot;aus. | Melden Sie sich mit Ihrem regulären MVPD an. | Ja | Ja | Ja ab 3.2 |
| N130 | Fehler | **Authentifizierungstoken wurde in der Sitzung nicht gefunden.** Dies kann auf einen der folgenden Gründe zurückzuführen sein: <br> <br> 1. Für den Browser sind (Drittanbieter-) Cookies deaktiviert (nicht anwendbar für AccessEnabler JavaScript SDK Version 4.x) <br> <br> 2 Für den Browser ist die Option Prevent cross-site tracking aktiviert (Safari 11+) <br> <br> 3. Sitzung ist abgelaufen <br> <br> 4 Programmierer ruft Authentifizierungs-APIs in falscher Folge auf <br> <br> HINWEIS: Dieser Fehlercode ist nicht für die Vollseitenumleitungs-Authentifizierungsflüsse verfügbar. | 1. Benutzer auffordern, Cookies von Drittanbietern zu aktivieren <br> <br> 2 Benutzer dazu anregen, Site-übergreifendes Tracking zu deaktivieren <br> <br> 3. Benutzer zur erneuten Authentifizierung von <br> auffordern <br> 4 APIs in der richtigen Reihenfolge aufrufen | 1. (Drittanbieter-Cookies) aktivieren <br> <br> 2 Site-übergreifendes Tracking deaktivieren <br> <br> 3. Erneutes Authentifizieren von <br> <br> 4 Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| N500 | Fehler | Interner Fehler. <br> <br> Hinweis: Dies ist der &quot;generische Authentifizierungsfehler&quot;und &quot;Interner Authentifizierungsfehler&quot;des ursprünglichen Fehlersystems. Dieser Fehler wird schließlich auslaufen. | Verwenden Sie AccessEnablerDebug und überprüfen Sie Debug-Protokolle (console.log output), um festzustellen, was schief gelaufen ist. | Nicht zutreffend | Ja | Ja | Nicht zutreffend |
| R401 | Fehler | Beim Versuch, ein Zugriffstoken zu erhalten, trat ein Fehler auf. <br> <br> Hinweis: Dies ist ein nicht behebbarer Fehler. Informieren Sie den Benutzer darüber, dass die Anwendung nicht verfügbar ist. | - iOS: Überprüfen Sie die Softwareanweisung und die benutzerdefinierten Schemas in Ihrer Anwendung. <br> <br> - JavaScript: Überprüfen Sie die Softwareanweisung in Ihrer Website-Anwendung. <br> <br> Öffnen Sie ein Ticket mit Zendesk und teilen Sie dem Benutzer mit, dass das System vorübergehend nicht verfügbar ist. | Nicht zutreffend | Ja ab v4.0 | Ja von v3.0 | Ja ab 3.2 |
| R400 | Fehler | Die Anwendung ist nicht registriert. Die Software-Anweisung ist ungültig oder wurde widerrufen. <br> <br> Hinweis: Dies ist ein nicht behebbarer Fehler. Informieren Sie den Benutzer darüber, dass die Anwendung nicht verfügbar ist. | - iOS: Überprüfen Sie die Softwareanweisung und die benutzerdefinierten Schemas in Ihrer Anwendung. <br> <br> - JavaScript: Überprüfen Sie die Softwareanweisung in Ihrer Website-Anwendung. <br> <br> Öffnen Sie ein Ticket mit Zendesk und teilen Sie dem Benutzer mit, dass das System vorübergehend nicht verfügbar ist. | Nicht zutreffend | Ja ab v4.0 | Ja von v3.0 | Ja ab 3.2 |
| REG 500 | Fehler | Der Registrierungs-Code konnte nicht vom Server abgerufen werden. <br> <br> Hinweis: Dies ist ein nicht behebbarer Fehler. Informieren Sie den Benutzer darüber, dass die Anwendung nicht verfügbar ist. | Öffnen Sie ein Ticket mit Zendesk und teilen Sie dem Benutzer mit, dass das System vorübergehend nicht verfügbar ist. | Nicht zutreffend | Ja ab v4.0 | Ja von v3.0 | Ja ab 3.2 |
| REGCODE | Erfolg | Die Anwendung namens setSelectedProvider-API auf der tvOS-Plattform. | Weisen Sie den Benutzer an, sich mit einem zweiten Gerät (Bildschirm) mit dem angegebenen Registrierungs-Code anzumelden. | Verwenden Sie den Regcode auf einem zweiten Gerät (Bildschirm), um die Authentifizierung zu starten. | Nicht zutreffend | Nur Ja für tvOS | Nicht zutreffend |
| Z010 | Warnung | Der Benutzer wurde autorisiert, während die &quot;Authentifizieren-Alle&quot;- oder &quot;Autorisieren-Alles-Abbauen&quot;-Regel für den ausgewählten MVPD vorhanden war. | Sie können den Benutzer darüber informieren, dass er aufgrund von MVPD-Schwierigkeiten &quot;kostenlosen&quot; freien Zugang erhält. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z011 | Info | Der Benutzer wurde mithilfe von TempPass autorisiert | Benutzer optional informieren | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z100 | Fehler | Die Autorisierung schlug fehl, da der Benutzer keine Anmeldung für die angeforderte Ressource hat oder aus anderen Gründen, die von der MVPD ausgehen, z. B. weil das Video nicht mit den Einstellungen der elterlichen Kontrolle für das Benutzerkonto übereinstimmt | - Wiedergabe nicht zulassen. <br> <br> - Informieren Sie den Benutzer. <br> <br> - Der &#39;message&#39;-Schlüssel in der Fehlermeldung kann eine detailliertere Nachricht vom MVPD enthalten. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z110 | Fehler | Genehmigung aufgrund wiederholter MVPD-Ablehnungen verweigert. Mögliche Betrugsversuche oder DOS. | - Wiedergabe nicht zulassen. <br> <br> - Informieren Sie den Benutzer. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z120 | Fehler | Die Genehmigung wurde aufgrund technischer Gründe für die Kommunikation mit dem MVPD verweigert. Mögliche Netzwerkfehler. | - Wiedergabe nicht zulassen. <br> <br> - Informieren Sie den Benutzer darüber, dass der MVPD Schwierigkeiten hatte und er es später versuchen sollte. | Versuchen Sie es später. | Ja | Ja | Ja ab 3.2 |
| Z130 | Fehler | Die Autorisierung wurde verweigert, weil eine ungültige/fehlerhafte Ressource verwendet wurde. | Überprüfen Sie die Ressourcenzeichenfolge und korrigieren Sie sie. Im Allgemeinen ist dieser Fehler auf ein falsch formatiertes MRSS oder die Verwendung einer einfachen Zeichenfolge anstelle von MRSS zurückzuführen. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z169 | Fehler | Die Autorisierung wurde verweigert, da die Abbauregel authzNone für die angegebene Ressource angewendet wurde. | Benutzer informieren | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| Z500 | Fehler | Interner Fehler. <br> <br> Hinweis: Dies ist der alte &quot;generische Authentifizierungsfehler&quot;und &quot;Interner Authentifizierungsfehler&quot;. Dieser Fehler wird schließlich auslaufen. | Verwenden Sie AccessEnablerDebug und überprüfen Sie Debug-Protokolle (console.log output), um festzustellen, was schief gelaufen ist. | Nicht zutreffend | Ja | Ja | Ja ab 3.2 |
| P100 | Fehler | Vorautorisierung fehlgeschlagen. Wahrscheinlich liegt dies an der Anforderung einer Autorisierung für zu viele Ressourcen. | - Verwenden Sie NICHT mehr als die maximal zulässige Anzahl von Ressourcen. <br> <br> - Wenden Sie sich an den Adobe Pass-Authentifizierungssupport, um die maximal zulässige Anzahl von Ressourcen zu finden/einzurichten. | Nicht zutreffend | Ja von v3.0 | Ja | Ja ab 3.2 |
| IS2XX | Fehler | Diese Fehlercodes werden zurückgegeben, wenn die Antwortdaten des Individualisierungsservers ein ungültiges Format aufweisen oder die erforderlichen Individualisierungsinformationen fehlen. | Öffnen Sie ein Ticket mit Zendesk und teilen Sie dem Benutzer mit, dass das System vorübergehend nicht verfügbar ist. | Nicht zutreffend | Ja von v3.0 | Nicht zutreffend | Nicht zutreffend |
| IS4XX | Fehler | Diese Fehlercodes werden zurückgegeben, wenn der Individualisierungsserver-Endpunkt 4XX fehlschlägt - der HTTP-Statuscode der Antwort ist. | Öffnen Sie ein Ticket mit Zendesk und teilen Sie dem Benutzer mit, dass das System vorübergehend nicht verfügbar ist. | Nicht zutreffend | Ja von v3.0 | Nicht zutreffend | Nicht zutreffend |
| IS5XX | Fehler | Diese Fehlercodes werden zurückgegeben, wenn der Individualisierungsserver-Endpunkt 5XX fehlschlägt - der HTTP-Statuscode der Antwort ist. | Öffnen Sie ein Ticket mit Zendesk und teilen Sie dem Benutzer mit, dass das System vorübergehend nicht verfügbar ist. | Nicht zutreffend | Ja von v3.0 | Nicht zutreffend | Nicht zutreffend |
| IS0 | Fehler | Dieser Code wird zurückgegeben, wenn der Individualisierungsserver-Endpunkt überhaupt nicht reagiert hat. Daher hat die Verbindung eine Zeitüberschreitung verursacht | Öffnen Sie ein Ticket mit Zendesk und teilen Sie dem Benutzer mit, dass das System vorübergehend nicht verfügbar ist. | Nicht zutreffend | Ja von v3.0 | Nicht zutreffend | Nicht zutreffend |
| LS011 | Warnung | AccessEnabler verwendet einen flüchtigen Speicher, da LSO/LocalStorage-Probleme und WebStorage-Probleme (oder Nicht-Verfügbarkeit) auftreten. <br> <br> Authentifizierung/Autorisierung bleibt nicht über die aktuelle Seite hinaus bestehen! Jeder Seitenladevorgang führt dazu, dass sich der Benutzer authentifizieren muss. Konfigurierte TTLs werden nicht über Seitenneuladungen hinweg erzwungen. | - Informieren Sie den Benutzer über Einschränkungen. <br> <br> - Informieren Sie den Benutzer darüber, wie der verfügbare Speicherplatz erhöht werden kann. <br> <br> - Alternativ zur Abmeldung, um den Speicher zu löschen. | - Erhöhen Sie den Speicher. <br> <br> - Melden Sie sich ab, um den Speicher zu löschen. | Ja | Nicht zutreffend | Nicht zutreffend |

<br>

## Ursprüngliche Fehlerberichte {#original-error-reporting}

In diesem Abschnitt wird das ursprüngliche Fehlermeldesystem zusammen mit den ursprünglichen Fehlercodes beschrieben. Im ursprünglichen Fehlermeldesystem übergibt der AccessEnabler Fehler an diese beiden Callback-Funktionen: `setAuthenticationStatus()` nach einem Aufruf an `checkAuthentication()`; `tokenRequestFailed()` nach dem Fehler eines Aufrufs an `checkAuthorization()` oder `getAuthorization()`.

Die ursprünglichen Fehler-Berichte und Status-APIs funktionieren weiterhin genau wie zuvor. Die ursprünglichen Fehler-Reporting-APIs werden jedoch in Zukunft nicht aktualisiert. Alle neuen Fehlerberichte und -aktualisierungen zu den alten Fehlern werden NUR im neuen [erweiterten Fehlermeldesystem](#advanced-error-reporting) angezeigt.


Beispiele für die Verwendung des ursprünglichen Fehlermeldesystems finden Sie in der [JavaScript API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) - und [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#token-request-failed-error-msg) -Funktionen, der [iOS/tvOS API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setAuthNStatus)und [tokentRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#tokenReqFailed) , [Android API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus) und [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### Ursprüngliche Rückruffehlercodes {#original-callback-error-codes}

| **Allgemeine Fehler** | |
|---|---|
| Interner Fehler | Beim Versuch, eine Anfrage zu verarbeiten, ist ein Systemfehler aufgetreten. |
| Fehler bei Provider nicht ausgewählt | Tritt auf, wenn der Kunde im Dialogfeld zur Anbieterauswahl abbricht. |
| Fehler bei Provider nicht verfügbar | Tritt auf, wenn keine Anbieter verfügbar sind. |
|  |  |
| **Authentifizierungsfehler** | |
| Generischer Authentifizierungsfehler | Wird zurückgegeben, wenn der Grund nicht bekannt ist oder nicht veröffentlicht werden kann. |
| Interner Authentifizierungsfehler | Beim Authentifizierungsversuch ist ein Systemfehler aufgetreten. |
| Benutzer nicht authentifiziert Fehler | Der Benutzer ist nicht authentifiziert. |
|  |  |
| **Autorisierungsfehler** |  |
| Allgemeiner Autorisierungsfehler | Wird zurückgegeben, wenn der Grund nicht bekannt ist oder nicht veröffentlicht werden kann. |
| Interner Autorisierungsfehler | Beim Autorisieren ist ein Systemfehler aufgetreten. |
| Benutzer nicht autorisiert Fehler | Der Kunde ist nicht berechtigt, den angeforderten Inhalt anzuzeigen. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
