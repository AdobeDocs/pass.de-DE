---
title: Amazon FireOS SSO mit clientless API-Cookbook
description: Amazon FireOS SSO mit clientless API-Cookbook
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Amazon FireOS SSO mit clientless API-Cookbook {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Einführung {#Introduction}

Dieses Dokument enthält Anweisungen zur Implementierung der SSO-Version des Adobe Pass-Authentifizierungsflusses von Amazon mithilfe der clientlosen API. Der erste Teil dieses Dokuments konzentriert sich auf die Besonderheiten der Amazon-Version der Architektur, da viele Partner bereits mit der Implementierung vertraut sind und über Erfahrung mit ihr verfügen.

Der zweite Teil des Dokuments umfasst die wichtigsten Schritte zur Implementierung der clientless-API für die Adobe Pass-Authentifizierung.

Einen umfassenden technischen Überblick über die Funktionsweise der Clientless-Lösung finden Sie im Abschnitt [REST API - Übersicht](/help/authentication/rest-api-overview.md). Adobe ist der bevorzugte Ansprechpartner für Support zur Gesamtarchitektur und zu den ersten Implementierungen.

## Amazon Clientless SSO {#AMZ-Clientless-SSO}

### Hochrangige Architektur {#High-Level-Arch}

Die clientlose SSO-Implementierung von Amazon ist einfach und größtenteils identisch mit den normalen Adobe Primetime Authentication Client-less-APIs.

Sie müssen das Amazon SDK verwenden, um eine personalisierte Payload abzurufen und sie beim Aufrufen von Adobe clientless-APIs zu verwenden.

Wenn die Payload erkannt wird und einer authentifizierten Sitzung entspricht, geben die clientlosen APIs sofort mit dem Token Ihrer Sitzung zurück.

### Erstellen der Anwendung zur Verwendung des Amazon SDK {#Build-entries}

* Neueste Version herunterladen und kopieren [Amazon Stub SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) in einen Ordner /SSOEnabler parallel zum App-Ordner
* Aktualisieren Sie die Manifest-/Gradle-Dateien, um die Bibliothek zu verwenden:

  **Fügen Sie Ihrer Manifestdatei die folgende Zeile hinzu:**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Gradle-Dateieinträge:**

  Unter Repositorys:

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  Fügen Sie unter Abhängigkeiten Folgendes hinzu:

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* Umgang mit dem Fehlen der Amazon-Companion-App:

  Sollte der Companion nicht auf dem Amazon-Gerät vorhanden sein, auf dem Ihre Anwendung ausgeführt wird, sollten Sie während der Laufzeit in der folgenden Klasse auf eine ClassNotFoundException stoßen: `com.amazon.ottssotokenlib.SSOEnabler`.

  In diesem Fall müssen Sie nur den Payload-Schritt überspringen und auf den normalen PrimeTime-Fluss zurückgreifen. SSO wird nicht aktiviert, aber der normale Autorisierungsfluss erfolgt normal.

</br>

### Abrufen der Amazon SSO-Payload mithilfe des Amazon SDK {#UseAmazonSSO}

Rufen Sie während der Initialisierung Ihrer Anwendung eine Instanz von SSOEnabler ab. Basierend auf Ihrer Anwendungsarchitektur sollten Sie zwischen einer synchronen und einer asynchronen Implementierung entscheiden.

Wenn die API-Aufrufe aus irgendeinem Grund keine Payload zurückgeben, verwenden Sie bitte den regulären Nicht-SSO-Fluss und kontaktieren Sie Ihre Amazon- und Adobe-Partner, um eine Untersuchung durchzuführen.

**Asynchrone API**

* SSO Enabler-Instanz abrufen:

  ```Java
  ssoEnabler = SSOEnabler.getInstance(context);
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```


* Callback festlegen

  ```java
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

   * Das Erfolgsantwort-Bundle enthält:
      * SSO-Token als Zeichenfolge mit dem Schlüssel &quot;SSOToken&quot;
   * Das Fehlerantwort-Bundle enthält:
      * Fehlercode als int mit Schlüssel &quot;ErrorCode&quot;
      * Fehlerbeschreibung als Zeichenfolge mit dem Schlüssel &quot;ErrorDescription&quot;


* SSO-Token abrufen

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* Diese API stellt die Antwort über den während des Init festgelegten Callback bereit.

  **Ex**. Aufruf mithilfe der Singleton-Instanz, die während der init erstellt wurde:

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**Synchrone APIs**

* Rufen Sie die SSO Enabler-Instanz ab und legen Sie den Callback fest.

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* SSO-Token abrufen

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * Diese API blockiert den aufrufenden Thread und reagiert mit dem Ergebnis-Bundle. Da es sich um einen synchronen Aufruf handelt, sollten Sie ihn nicht im Hauptthread verwenden.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * Wert in Millisekunden. Wenn festgelegt, überschreiben Sie den Standardwert für die Zeitüberschreitung von 1 Minute für die Synchronisierungs-API.


### Adobe Pass Clientlose API-Update zur Verwendung der dynamischen Client-Registrierung {#clientlessdcr}

Wenn dies Ihre erste Implementierung ist, lesen Sie die Informationen unter **Clientlose technische Übersicht** und kontaktieren Sie Adobe, falls Sie Support benötigen.

Für die Adobe Client-lose-API müssen Anwendungen die Dynamic Client-Registrierung verwenden, um Adobe-Server aufzurufen.

* Um die Dynamic Client-Registrierung in Ihrer Anwendung zu verwenden, befolgen Sie die Anweisungen unter [Dynamisches Client-Registrierungs-Management zur Registrierung der Anwendung](/help/authentication/dynamic-client-registration-management.md).

* Um die Dynamic Client Registration API zu implementieren, um Authentifizierungs- und Autorisierungsanfragen an Adobe Pass-Server durchzuführen, folgen Sie den Anweisungen unter [Dynamische Client-Registrierungs-API](/help/authentication/dynamic-client-registration-api.md) .

### Adobe Pass Clientlose API-Update zur Verwendung von Amazon SSO {#clientlesssso}

Die vom Amazon SDK abgerufene Amazon SSO-Payload muss bei Anfragen vorhanden sein, die an Adobe Pass-Authentifizierungsendpunkte gesendet werden:

```
      /adobe-services/*
      /reggie/*
      /api/*
```


Alle Adobe Pass-Authentifizierungsendpunkte unterstützen die folgenden Methoden zum Empfangen der ID im Gerätebereich oder der Platform- Scoped-Kennung (in Amazon SSO-Payload vorhanden):

* Als Kopfzeile: &quot;Adobe-Subject-Token&quot;
* Als Abfrageparameter : &quot;ast&quot;
* Als Post-Parameter : &quot;ast&quot;


>[!NOTE]
>
>Wenn die ID im Gerätebereich oder im Plattformbereich als Abfrage-/Post-Parameter gesendet wird, muss sie beim Generieren der Anforderungssignatur einbezogen werden.

>[!NOTE]
>
>Mithilfe des Abfrageparameters &quot;ast&quot;kann die gesamte URL sehr lang werden und abgelehnt werden. Beim /authenticate -Aufruf kann dieser Parameter übersprungen werden, da er beim /regcode -Aufruf bereitgestellt wurde

**Beispiele:**

**Als benutzerdefinierte Kopfzeile senden**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Senden als Abfrageparameter**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**Senden als Post-Parameter**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>Wenn die Amazon SSO nicht vorhanden oder ungültig ist, ignoriert die Adobe Pass-Authentifizierung das -Attribut und die Aufrufe werden so ausgeführt, als wäre keine SSO vorhanden.
