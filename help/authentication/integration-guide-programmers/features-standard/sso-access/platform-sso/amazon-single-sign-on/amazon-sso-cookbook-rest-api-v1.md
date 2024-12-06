---
title: Amazon SSO-Cookbook (REST API V1)
description: Amazon SSO-Cookbook (REST API V1)
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Amazon SSO-Cookbook (REST API V1) {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Die Adobe Pass Authentication REST API V1 unterstützt Platform Single Sign-On (SSO) für Endbenutzer von Client-Anwendungen, die auf FireOS ausgeführt werden.

Dieses Dokument dient als Erweiterung der vorhandenen [REST API V1 Overview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md) , die eine allgemeine Ansicht bietet.

## Amazon-Single Sign-On mit Platform-Identitätsflüssen {#cookbook}

### Voraussetzungen {#prerequisites}

Bevor Sie mit der Amazon-Single-Sign-On mit Platform-Identitätsflüssen fortfahren, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

#### Integrieren des Amazon SSO SDK {#integrate-amazon-sso-sdk}

Die Streaming-Anwendung muss die Bibliothek [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) für Single Sign-On (SSO) in ihren Build integrieren.

* Laden Sie die neueste Amazon SSO SDK-Bibliothek herunter und kopieren Sie sie in einen Ordner &quot;`/SSOEnabler`&quot;, der parallel zum Verzeichnis der Anwendung liegt.

* Aktualisieren Sie die Manifest- und Gradle-Dateien, um die Amazon SSO SDK-Bibliothek zu verwenden.

  **Manifest:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  Unter Repositorys:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  Unter Abhängigkeiten:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Verwenden des Amazon SSO SDK {#use-amazon-sso-sdk}

Die Streaming-Anwendung muss das Amazon SSO SDK verwenden, um die Payload des SSO-Tokens (Platform Identity) zu erhalten.

Das Amazon SSO SDK stellt sowohl synchrone als auch asynchrone APIs bereit, um die Payload des SSO-Tokens (Platform Identity) zu erhalten.

Die Streaming-Anwendung kann je nach Architektur eine der beiden Optionen auswählen.

##### Asynchrone APIs

* Rufen Sie die `SSOEnabler` -Instanz ab und legen Sie die `SSOEnablerCallback` fest:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  Dies kann während der Initialisierung der Streaming-Anwendung erfolgen.

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  Das SSO-Token-Erfolgsantwort-Bundle enthält:
   * Ein SSO-Token als `string` mit dem Schlüssel &quot;SSOToken&quot;.

  <br/>

  Das SSO-Token-Antwort-Bundle für Fehler enthält:
   * Ein Fehlercode als `int` mit dem Schlüssel &quot;ErrorCode&quot;.
   * Eine Fehlerbeschreibung als `string` mit dem Schlüssel &quot;ErrorDescription&quot;.

  <br/>

* SSO-Token abrufen:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Diese API stellt die Antwort über einen während der Initialisierung festgelegten Callback bereit.

##### Synchrone APIs

* Rufen Sie die Instanz `SSOEnabler` ab:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* SSO-Token abrufen:

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Diese API blockiert den aufrufenden Thread und reagiert mit dem Ergebnis-Bundle. Da es sich um einen synchronen Aufruf handelt, sollten Sie ihn nicht in Ihrem Haupt-Thread verwenden.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Diese API legt den Timeout-Wert für den synchronen Aufruf fest. Der Standardwert für die Zeitüberschreitung beträgt 1 Minute.

#### Fallback für Amazon SSO {#fallback-amazon-sso}

Die Streaming-Anwendung muss Fallback-Szenarien vom Amazon SSO-Fluss zum normalen Authentifizierungsfluss handhaben.

Stellen Sie sicher, dass die Streaming-Anwendung Folgendes verarbeitet:

* Das Fehlen der Amazon-Begleitanwendung, die auf dem Amazon-Gerät ausgeführt werden sollte.
   * Die Streaming-Anwendung kann zur Laufzeit auf eine `ClassNotFoundException` in der folgenden Klasse `com.amazon.ottssotokenlib.SSOEnabler` stoßen.

* Das Fehlen der SSO-Token-Payload (Plattform-Identität), die von den oben genannten APIs zurückgegeben werden sollte.
   * Die Streaming-Anwendung kann sich an die Amazon- und Adobe-Vertreter wenden, um Untersuchungen durchzuführen.

### Workflow {#workflow}

Die Payload des Amazon SSO-Tokens (Plattformidentität) muss bei allen HTTP-Anfragen vorhanden sein, die an Adobe Pass-Authentifizierungs-Endpunkten gesendet werden:

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> Die Streaming-Anwendung kann das Senden der Amazon SSO-Token-Payload (Plattformidentität) beim `/authenticate`-Aufruf überspringen, da dies beim `/regcode` -Aufruf angegeben wurde.

Die Adobe Pass-Authentifizierung unterstützt die folgenden Methoden zum Empfangen der SSO-Token-Payload (Plattformidentität), bei der es sich um eine ID mit Gerätebereich oder Plattformbereich handelt:

* Als Kopfzeile mit dem Namen: `Adobe-Subject-Token`
* Als Abfrageparameter mit dem Namen: `ast`
* Als Post-Parameter mit dem Namen: `ast`

>[!IMPORTANT]
>
> Wenn als Abfrageparameter gesendet, kann die gesamte URL sehr lang werden und abgelehnt werden.
>
> Wenn als Abfrage-/Post-Parameter gesendet, muss er beim Generieren der Anforderungssignatur einbezogen werden.

#### Stichproben

**Als Kopfzeile senden**

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Senden als Abfrageparameter**

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**Senden als Post-Parameter**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> Wenn der Parameterwert `Adobe-Subject-Token` oder `ast` fehlt oder ungültig ist, verarbeitet die Adobe Pass-Authentifizierung die Anforderungen, ohne Single Sign-On zu berücksichtigen.
