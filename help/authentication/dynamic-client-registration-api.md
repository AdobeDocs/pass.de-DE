---
title: Dynamische Client-Registrierungs-API
description: Dynamische Client-Registrierungs-API
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 0%

---

# Dynamische Client-Registrierungs-API {#dynamic-client-registration-api}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Derzeit gibt es zwei Möglichkeiten, wie Adobe Pass Authentication Anwendungen identifiziert und registriert:

* Browser-basierte Clients werden über die zulässige [Domain-Auflistung](/help/authentication/programmer-overview.md) registriert.
* native Anwendungs-Clients wie iOS- und Android-Anwendungen werden über den signierten Anforderungsmechanismus registriert.

Adobe Pass Authentication schlägt einen neuen Mechanismus für die Registrierung von Anwendungen vor. Dieser Mechanismus wird in den folgenden Absätzen beschrieben.

## Der Mechanismus zur Registrierung von Anträgen {#appRegistrationMechanism}

### Technische Gründe {#reasons}

Der Authentifizierungsmechanismus in der Adobe Pass-Authentifizierung stützte sich auf Sitzungs-Cookies. Aufgrund der [benutzerdefinierten Android Chrome-Registerkarten](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} und des [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank} kann dieses Ziel jedoch nicht mehr erreicht werden.

Aufgrund dieser Einschränkungen führt Adobe einen neuen Registrierungsmechanismus für alle seine Kunden ein. Er basiert auf der OAuth 2.0 RFC und besteht aus
der folgenden Schritte:

1. Abrufen der Softwareanweisung von TVE
Dashboard
1. Abrufen von Client-Anmeldeinformationen
1. Zugriffstoken abrufen

### Abrufen von Softwareaussagen vom TVE-Dashboard {#softwareStatement}

Für jede Anwendung, die Sie veröffentlichen, müssen Sie eine Softwareanweisung erhalten. Sobald Anwendungen erstellt wurden, werden alle Softwareanweisungen über das TVE-Dashboard bereitgestellt. Die Softwareanweisung sollte zusammen mit der Anwendung auf dem Gerät des Benutzers bereitgestellt werden.

>[!IMPORTANT]
>
>Bei Verwendung einer Softwareanweisung wird der signierte Anfrage-ID-Mechanismus nicht mehr benötigt.

Weitere Informationen zum Erstellen von Softwareanweisungen finden Sie unter [Kundenregistrierung im TVE-Dashboard](/help/authentication/dynamic-client-registration.md).

### Abrufen von Client-Anmeldeinformationen {#clientCredentials}

Nachdem Sie eine Softwareanweisung vom TVE-Dashboard abgerufen haben, müssen Sie Ihre Anwendung beim Adobe Pass-Autorisierungsserver registrieren. Führen Sie dazu einen /register-Aufruf durch und rufen Sie Ihre eindeutige Client-Kennung ab.

**Anfrage**

| HTTP-Aufruf |                    |
|-----------|--------------------|
| path | /o/client/register |
| method | POST |

| fields |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | Die im TVE Dashboard erstellte Softwareanweisung. | mandatory |
| redirect_uri | Der URI, den die Anwendung zum Abschließen des Authentifizierungsflusses verwendet. | optional |

| Anfragekopfzeilen |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Content-Type | application/json | mandatory |
| X-Device-Info | Die Geräteinformationen, wie unter Übergeben von Geräte- und Verbindungsinformationen definiert | mandatory |
| User-Agent | Der Benutzeragent | mandatory |

**Antwort**

| Antwortheader |                  |           |
|------------------|------------------|-----------|
| Content-Type | application/json | mandatory |

| Antwortfelder |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | Zeichenfolge | mandatory |
| client_secret | Zeichenfolge | mandatory |
| client_id_displayed_at | long | mandatory |
| redirect_uris | Liste von Zeichenfolgen | mandatory |
| grant_types | Liste der Zeichenfolgen<br/> **accept value**<br/> `client_credentials`: Wird von unsicheren Clients wie dem Android SDK verwendet. | mandatory |
| error | **akzeptierte Werte**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unauthorised_software_statement</li></ul> | In einem Fehlerfluss obligatorisch |


#### Fehlerantwort {#error-response}

Im Falle eines Fehlers antwortet der Registrierungsserver mit dem Statuscode HTTP 400 (Bad Request) und enthält die folgenden Parameter in der Antwort:

| Statuscode | Antwort-Body | description |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Der Anfrage fehlt ein erforderlicher Parameter, enthält einen nicht unterstützten Parameterwert, wiederholt einen Parameter oder ist anderweitig fehlerhaft. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_redirect_uri&quot;} | Der redirect_uri ist für diesen Client aufgrund seiner registrierten Anwendung nicht zulässig. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | Die Software-Anweisung ist ungültig. |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorised_software_statement&quot;} | Die software_id wird in der Konfiguration nicht gefunden. |

#### Beispiel für Client-Anmeldedaten {#client-credentials-example}

**Anfrage:**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
 {
        "software_statement": "eyJhbGciOiJSUzI1NiJ9.
    eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
    bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
    aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
    GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
    zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
    5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
    fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
    U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
    IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
  "redirect_uri": "adobepass://com.programmer"  }
```

**Antwort:**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**Fehlerantwort:**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### Zugriffstoken abrufen {#accessToken}

Nachdem Sie die eindeutige Client-Kennung (Client-ID und Client-Geheimnis) für Ihre Anwendung abgerufen haben, müssen Sie ein Zugriffstoken abrufen. Das Zugriffstoken ist ein obligatorisches OAuth 2.0-Token, das zum Aufrufen der Adobe Pass-Authentifizierungs-APIs verwendet wird.

>[!NOTE]
>
>Derzeit haben Zugriffstoken eine Live-Lebensdauer von 24 Stunden.

**Anfrage**


| **HTTP-Aufruf** | |
| --- | --- |
| path | `/o/client/token` |
| method | POST |

| **Anforderungsparameter** | |
| --- | --- |
| `grant_type` | Wird im Client-Registrierungsprozess empfangen.<br/> **Akzeptierter Wert**<br/>`client_credentials`: Wird für unsichere Clients wie das Android SDK verwendet. |
| `client_id` | Client-Kennung, die im Client-Registrierungsprozess abgerufen wurde. |
| `client_secret` | Client-Kennung, die im Client-Registrierungsprozess abgerufen wurde. |

**Antwort**

| Antwortfelder | | |
| --- | --- | --- |
| `access_token` | Der Zugriffstoken-Wert, den Sie zum Aufrufen der Adobe Pass-APIs verwenden sollten | mandatory |
| `expires_in` | Die Zeit in Sekunden, bis das access_token abläuft | mandatory |
| `token_type` | Der Typ des Tokens **bearer** | mandatory |
| `created_at` | Die Problemzeit des Tokens | mandatory |
| **Antwortheader** | | |
| `Content-Type` | application/json | mandatory |

**Fehlerantwort**

Im Fall eines Fehlers antwortet der Autorisierungsserver mit dem Statuscode HTTP 400 (Bad Request) und enthält die folgenden Parameter in der Antwort:

| Statuscode | Antwort-Body | description |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Der Anfrage fehlt ein erforderlicher Parameter, enthält einen nicht unterstützten Parameterwert (außer dem Grant-Typ), wiederholt einen Parameter, enthält mehrere Anmeldeinformationen, verwendet mehr als einen Mechanismus zur Authentifizierung des Clients oder ist anderweitig fehlerhaft. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | Die Client-Authentifizierung schlug fehl, da der Client unbekannt war. Das SDK MUSS sich erneut beim Autorisierungsserver registrieren. |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorized_client&quot;} | Der authentifizierte Client ist nicht berechtigt, diesen Autorisierungstyp zu verwenden. |

#### Beispiel für Zugriffstoken: {#obt-access-token}

**Anfrage:**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**Antwort:**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**Fehlerantwort:**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## Ausführen von Authentifizierungsanfragen {#autheticationRequests}

Verwenden Sie das Zugriffstoken, um Adobe Pass [API-Aufrufe für die Authentifizierung](/help/authentication/initiate-authentication.md) durchzuführen. Dazu muss das Zugriffstoken der API-Anfrage auf eine der folgenden Arten hinzugefügt werden:

* durch Hinzufügen eines neuen Abfrageparameters zur Anforderung. Dieser neue Parameter heißt **access_token**.

* durch Hinzufügen eines neuen HTTP-Headers zur Anfrage: Authorization: Bearer. Es wird empfohlen, den HTTP-Header zu verwenden, da Abfragezeichenfolgen in der Regel in Serverprotokollen sichtbar sind.

Im Falle eines Fehlers können die folgenden Fehlerantworten zurückgegeben werden:

| Fehlerantworten |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | Die Anfrage ist fehlerhaft. |
| invalid_client | 403 | Die Client-ID darf keine Anfragen mehr ausführen. Es sollten neue Client-Anmeldeinformationen generiert werden. |
| access_denied | 401 | Das access_token ist ungültig (abgelaufen oder ungültig). Der Client MUSS ein neues access_token anfordern. |

### Beispiele für Authentifizierungsanfragen:

**Senden des Zugriffs-Tokens als Anforderungsparameter:**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**Senden des Zugriffstokens als HTTP-Header:**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**Fehlerantworten als Antworttext:**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**Fehlerantworten als URL-Parameter:**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
