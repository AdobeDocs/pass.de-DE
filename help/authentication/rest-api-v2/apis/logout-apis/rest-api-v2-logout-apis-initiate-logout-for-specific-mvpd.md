---
title: Initiieren der Abmeldung für bestimmte mvpd
description: REST API V2 - Initiierung der Abmeldung für bestimmte mvpd
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 0%

---


# Initiieren der Abmeldung für bestimmte mvpd {#initiate-logout-for-specific-mvpd}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

## Anfrage {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/api/v2/{serviceProvider}/logout/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Pfadparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Die interne eindeutige Kennung, die dem Service Provider während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>Die interne eindeutige Kennung, die dem Identitäts-Provider während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Abfrageparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        Die endgültige Umleitungs-URL, zu der der Benutzeragent navigiert, wenn der Abmeldefluss für den MVPD abgeschlossen ist.
        <br/><br/>
        Der Wert muss URL-kodiert sein.
      </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Erstellung der Payload des Trägertokens wird in der Kopfzeilendokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Autorisierung</a> beschrieben.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>Die Generierung der Payload der Gerätekennung wird in der Kopfzeilendokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> beschrieben.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Die Erstellung der Payload der Geräteinformationen wird in der Kopfzeilendokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> beschrieben.
         <br/><br/>
         Es wird dringend empfohlen, sie immer zu verwenden, wenn die Geräteplattform der Anwendung die explizite Bereitstellung gültiger Werte zulässt.
         <br/><br/>
         Wenn dies bereitgestellt wird, führt das Adobe Pass-Authentifizierungs-Backend explizit Werte mit extrahierten Werten zusammen (standardmäßig).
         <br/><br/>
         Wenn kein Wert angegeben wird, verwendet das Backend für die Adobe Pass-Authentifizierung implizit extrahierte Werte (standardmäßig).
      </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Die IP-Adresse des Streaming-Geräts.
         <br/><br/>
         Es wird dringend empfohlen, ihn immer für Server-zu-Server-Implementierungen zu verwenden, insbesondere wenn der Aufruf vom Programmierer-Dienst und nicht vom Streaming-Gerät erfolgt.
         <br/><br/>
         Bei Client-zu-Server-Implementierungen wird die IP-Adresse des Streaming-Geräts implizit gesendet.
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token</td>
      <td>
        Die Generierung der Single Sign-On-Payload für die Platform Identity-Methode wird in der Kopfzeilendokumentation für <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu für Single Sign-on aktivierten Flüssen mit einer Plattformidentität finden Sie in der Dokumentation zu <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Single Sign-on mit Platform-Identitätsflüssen</a> .
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        Die Generierung der Single Sign-On-Payload für die Service Token-Methode wird in der Kopfzeilendokumentation für <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu für die einmalige Anmeldung aktivierten Flüssen mit einem Dienst-Token finden Sie in der Dokumentation zum <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">Single Sign-on mit Service-Token-Flüssen</a> .
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         Der Medientyp, der von der Clientanwendung akzeptiert wird.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Der Benutzeragent der Clientanwendung.</td>
      <td>optional</td>
   </tr>
</table>

## Reaktion {#response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Der Antworttext enthält eine Liste von Aktionen, die der Client ausführen muss, um den Abmeldefluss für jedes gelöschte Profil abzuschließen.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ungültige Anfrage</td>
      <td>
        Die Anfrage ist ungültig, der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Unerlaubt</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation zur <a href="../../../dcr-api/dynamic-client-registration-overview.md">Übersicht über die dynamische Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode nicht zulässig</td>
      <td>
        Die HTTP-Methode ist ungültig. Der Client muss eine HTTP-Methode verwenden, die für die angeforderte Ressource zulässig ist, und es erneut versuchen. Weitere Informationen finden Sie im Abschnitt <a href="#request">Anfrage</a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interner Server-Fehler</td>
      <td>
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.
      </td>
   </tr>
</table>

### Erfolg {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">body</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">logouts</td>
      <td>
         JSON mit einer Zuordnung von Schlüssel-/Wert-Paaren.
         <br/><br/>
         Das Schlüsselelement wird durch den folgenden Wert definiert:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Wert</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Die interne eindeutige Kennung, die dem Identitäts-Provider während des Onboarding-Prozesses zugeordnet ist.</td>
               <td><i>erforderlich</i></td>
         </table>
         Das value-Element wird durch die folgenden Attribute definiert:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Die Aktion, die das Streaming-Gerät ausführen muss, um den Abmeldefluss abzuschließen.
                  <br/><br/>
                  Mögliche Werte sind:
                  <ul>
                    <li><b>logout</b><br/>Das Streaming-Gerät muss die bereitgestellte URL in einem Benutzeragenten öffnen.<br/>Diese Aktion gilt für die folgenden Szenarien: Abmelden von MVPD mit einem Abmelde-Endpunkt.</li>
                    <li><b>complete</b><br/>Das Streaming-Gerät muss keine nachfolgenden Aktionen durchführen.<br/>Diese Aktion gilt für die folgenden Szenarien: Abmelden von MVPD ohne Abmelde-Endpunkt (Platzhalterabmeldefunktion), Abmelden beim eingeschränkten Zugriff, Abmelden beim temporären Zugriff.</li>
                    <li><b>invalid</b><br/>Das Streaming-Gerät muss keine nachfolgenden Aktionen durchführen.<br/>Diese Aktion gilt für die folgenden Szenarien: Melden Sie sich von MVPD ab, wenn kein gültiges Profil gefunden wird.</li>
                  </ul>  
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Die Art der Interaktion, die das Streaming-Gerät ausführen muss, damit der Fluss mit der durch das Attribut "actionName"angegebenen Aktion fortgesetzt werden kann.
                  <br/><br/>
                  Mögliche Werte sind:
                  <ul>
                    <li><b>interaktiv</b><br/>Dieser Typ gilt für die folgenden Werte des Attributs "actionName": <b>logout</b>.</li>
                    <li><b>none</b><br/>Dieser Typ gilt für die folgenden Werte des Attributs "actionName": <b>complete</b>, <b>invalid</b>.</li>
                  </ul>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Die interne eindeutige Kennung, die dem Identitäts-Provider während des Onboarding-Prozesses zugeordnet ist.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>
                  Die URL, die zum Ausführen des Abmeldeflusses mit dem MVPD-Endpunkt verwendet wird.
                  <br/><br/>
                  Dies ist für die folgenden Werte des Attributs "actionName"nicht vorhanden:
                  <ul>
                    <li><b>complete</b></li>
                    <li><b>ungültig</b></li>
                  </ul>
               </td>
               <td>optional</td>
            </tr>
         </table>
      </td>
      <td><i>erforderlich</i></td>
</table>

### Fehler {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">body</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">error</td>
      <td>Der Fehler enthält zusätzliche Informationen, die der Dokumentation <a href="../../../enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.</td>
      <td><i>erforderlich</i></td>
   </tr>
</table>

## Stichproben {#samples}

### 1. Initiierung der grundlegenden Abmeldung für mvpd, die den Abmeldefluss unterstützt

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
GET /api/v2/logout/REF30/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)  
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "Cablevision" : {
            "actionName" : "logout",
            "actionType" : "interactive",
            "mvpd" : "Cablevision",
            "url": "https://sp.auth.adobe.com/adobe-services/logout?noflash=true&mso_id=Cablevision&requestor_id=REF30&redirect_url=http%3A%2F%2Fadobe.com"
        }
    }
}
```

>[!ENDTABS]

### 2. Initiierung der grundlegenden Abmeldung für mvpd, die den Abmeldefluss nicht unterstützt

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
GET /api/v2/logout/REF30/Dish?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "Dish" : {
            "actionName" : "complete",
            "actionType" : "none",
            "mvpd" : "Dish"
       }
    }
}
```

>[!ENDTABS]

### 3. Initiieren Sie die Abmeldung einschließlich authentifizierter Profile, die über die Single Sign-on mit der Platform Identity-Methode abgerufen wurden.

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
GET /api/v2/logout/REF30/Comcast_SSO?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJObUZtWmpjek5XVXROVFJoWWkwME5ERmlMV0V6Wm .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
   "logouts": {
      "Comcast_SSO": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Comcast_SSO",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Comcast_SSO&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### 4. Initiieren Sie die Abmeldung einschließlich authentifizierter Profile, die über die Single Sign-On mit der Service Token-Methode abgerufen wurden.

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
GET /api/v2/logout/REF30/Spectrum?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJZemsxTXpNMk4yWXRZMk0wTWkwME1X .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
   "logouts": {
      "Spectrum": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Spectrum",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Spectrum&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### 5. Initiierung der Abmeldung für (Werbe-)temporären Pass

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
GET /api/v2/logout/REF30/TempPass_5mins?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "TempPass_5mins" : {
            "actionName" : "invalid",
            "actionType" : "none",
            "mvpd" : "TempPass_5mins"
        }
    }
}
```

>[!ENDTABS]

### 6. Initiierung der Abmeldung für beeinträchtigte MVPD

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
GET /api/v2/logout/REF30/ATTOTT?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "ATTOTT" : {
            "actionName" : "invalid",
            "actionType" : "none",
            "mvpd" : "ATTOTT",
        }
    }
}
```

>[!ENDTABS]
