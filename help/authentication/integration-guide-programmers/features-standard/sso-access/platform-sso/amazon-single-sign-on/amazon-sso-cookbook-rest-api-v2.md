---
title: Amazon SSO-Cookbook (REST API v2)
description: Amazon SSO-Cookbook (REST API v2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Amazon SSO-Cookbook (REST API v2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Die Adobe Pass-Authentifizierungs-REST-API V2 unterstützt Platform Single Sign-On (SSO) für Endbenutzer von Client-Anwendungen, die auf FireOS ausgeführt werden.

Dieses Dokument dient als Erweiterung für die vorhandene [REST API V2 - Übersicht](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md), die eine allgemeine Ansicht bietet, sowie für das Dokument, in dem die Implementierung von [Single Sign-on mithilfe von Platform-Identitätsflüssen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) beschrieben wird.

## Amazon-Single-Sign-on mit Platform-Identitätsflüssen {#cookbook}

Die Adobe Pass-Authentifizierung arbeitet mit Amazon zusammen, um das Benutzererlebnis bei der Anmeldung zu verbessern und die einmalige Anmeldung (Single Sign-On, SSO) für TV Everywhere-Programme für TV-Abonnenten zu erleichtern.

### Voraussetzungen {#prerequisites}

Bevor Sie mit dem Amazon-Single-Sign-on unter Verwendung von Platform-Identitätsflüssen fortfahren, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind.

#### Integrieren von Amazon SSO SDK {#integrate-amazon-sso-sdk}

Die Streaming-Anwendung muss die [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar)-Bibliothek für einmaliges Anmelden (SSO) in ihren Build integrieren.

* Laden Sie die neueste Amazon SSO SDK-Bibliothek herunter und kopieren Sie sie in einen `/SSOEnabler` Ordner parallel zum Verzeichnis der Anwendung.

* Aktualisieren Sie die Manifest- und Gradle-Dateien zur Verwendung der Amazon SSO SDK-Bibliothek.

  **manifest:**

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

#### Verwenden von Amazon SSO SDK {#use-amazon-sso-sdk}

Die Streaming-Anwendung muss die Amazon SSO SDK verwenden, um die Payload des SSO-Tokens (Plattformidentität) abzurufen.

Amazon SSO SDK stellt sowohl synchrone als auch asynchrone APIs zum Abrufen der SSO-Token-Payload (Plattformidentität) bereit.

Die Streaming-Anwendung kann je nach Architektur eine der beiden Optionen auswählen.

##### Asynchrone APIs

* Rufen Sie die `SSOEnabler`-Instanz ab und legen Sie die `SSOEnablerCallback` fest:

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

  Das Bundle SSO Token Success Response enthält:
   * Ein SSO-Token als `string` mit dem Schlüssel „SSOToken“.

  <br/>

  Das Bundle mit der SSO-Token-Fehlerantwort enthält:
   * Fehlercode als `int` mit Schlüssel „ErrorCode“.
   * Eine Fehlerbeschreibung als `string` mit dem Schlüssel „ErrorDescription“.

  <br/>

* Abrufen des SSO-Tokens:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Diese API stellt die Antwort über einen Callback bereit, der während der Initialisierung festgelegt wurde.

##### Synchrone APIs

* `SSOEnabler` abrufen:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* Abrufen des SSO-Tokens:

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Diese API blockiert den Aufrufer-Thread und antwortet mit dem Ergebnispaket. Da es sich um einen synchronen Aufruf handelt, sollten Sie sicherstellen, dass Sie ihn nicht in Ihrem Haupt-Thread verwenden.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Diese API legt den Timeout-Wert für den synchronen Aufruf fest. Der Standardwert für die maximale Wartezeit ist 1 Minute.

#### Fallback für Amazon SSO {#fallback-amazon-sso}

Die Streaming-Anwendung muss Fallback-Szenarien vom Amazon-SSO-Fluss zum regulären Authentifizierungsfluss verarbeiten.

Stellen Sie sicher, dass die Streaming-Anwendung Folgendes verarbeitet:

* Das Fehlen der Amazon Companion-Anwendung, die auf dem Amazon-Gerät ausgeführt werden sollte.
   * Die Streaming-Anwendung kann zur Laufzeit auf eine `ClassNotFoundException` bei der folgenden `com.amazon.ottssotokenlib.SSOEnabler` stoßen.

* Die Abwesenheit der Payload des SSO-Tokens (Plattformidentität), das von den oben genannten APIs zurückgegeben werden sollte.
   * Die Streaming-Anwendung kann sich zur Untersuchung an die Amazon- und Adobe-Support-Mitarbeiter wenden.

### Workflow {#workflow}

Die Payload des Amazon-SSO-Tokens (Plattformidentität) muss bei allen HTTP-Anfragen vorhanden sein, die an REST-API-V2-Endpunkten für die Adobe Pass-Authentifizierung gesendet werden:

```
/api/v2/*
```

Die Adobe Pass-Authentifizierungs-REST-API V2 unterstützt die folgenden Methoden, um die Payload des SSO-Tokens (Plattformidentität) zu empfangen, die eine Kennung im Gerätebereich oder Plattformbereich ist:

* Als Kopfzeile mit dem Namen: `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Weitere Informationen zu `Adobe-Subject-Token` Kopfzeile finden Sie in der Dokumentation [Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

#### Beispiele

**Als Kopfzeile senden**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> Falls der `Adobe-Subject-Token`-Header-Wert fehlt oder ungültig ist, verarbeitet die Adobe Pass-Authentifizierung die Anfragen, ohne Single Sign-On zu berücksichtigen.
