---
title: Zugriffstoken abrufen
description: Dynamic Client Registration API - Zugriffstoken abrufen
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---


# Zugriffstoken abrufen {#retrieve-access-token}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die Implementierung der Dynamic Client Registration API wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

## Anfrage {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            Die Client-Anwendungs-ID-Zeichenfolge.
            <br/><br/>
            Weitere Informationen zum Abrufen der Client-ID-Zeichenfolge finden Sie in der API-Dokumentation zum <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Abrufen von Client-Anmeldeinformationen</a> .
      </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secret</td>
      <td>
            Die geheime Zeichenfolge der Clientanwendung.
            <br/><br/>
            Weitere Informationen zum Abrufen der Client-geheimen Zeichenfolge finden Sie in der API-Dokumentation zum <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Abrufen von Client-Anmeldeinformationen</a> .
      </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            Die Zeichenfolge vom Typ "grant"(z. B. "client_credentials"), die die Client-Anwendung für den Client-Token-Endpunkt verwenden kann.
            <br/><br/>
            Weitere Informationen zum Abrufen der Zeichenfolge vom Typ "grant"finden Sie in der API-Dokumentation zum <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Abrufen von Client-Anmeldeinformationen</a> .
      </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Der akzeptierte Medientyp für die gesendeten Ressourcen.
         <br/><br/>
         Es muss application/x-www-form-urlencoded sein.
      </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Die Erstellung der Payload der Geräteinformationen wird in der Dokumentation <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> beschrieben.
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

### Erfolg {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>
         JSON-Objekt mit den folgenden Attributen:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">id</td>
               <td>Die undurchsichtige Kennung, die zur Verfolgung der Benutzeraktivität verwendet werden kann.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>Der Zugriffstoken-Wert, den die Client-Anwendung für die Autorisierungskopfzeile verwenden muss.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>Der Zeitpunkt, zu dem das Zugriffstoken ausgestellt wurde.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>Die Zeit in Sekunden, bis das Zugriffstoken abläuft.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>Der Tokentyp (z. B. "bearer").</td>
               <td><i>erforderlich</i></td>
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
      <td>400</td>
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
      <td>
        Mögliche Werte sind:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Wert</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    Die Anfrage ist aus einem der folgenden Gründe ungültig:
                    <ul>
                        <li>Bei der Anfrage fehlt ein erforderlicher Parameter.</li>
                        <li>Die Anfrage enthält einen nicht unterstützten Parameterwert (außer Grant-Typ).</li>
                        <li>Die Anfrage wiederholt einen -Parameter.</li>
                        <li>Die Anfrage umfasst mehrere Anmeldeinformationen.</li>
                        <li>Die Anfrage nutzt mehr als einen Mechanismus zur Authentifizierung des Clients.</li>
                        <li>Die Anfrage ist fehlerhaft.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>Die Client-Anmeldeinformationen sind ungültig. Der Client muss neue Client-Anmeldeinformationen abrufen und es erneut versuchen. Weitere Informationen finden Sie in der API-Dokumentation zum <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Abrufen von Client-Anmeldeinformationen</a> .</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>Der verwendete Grant-Typ ist ungültig.</td>
            </tr>
         </table>
      </td>
      <td><i>erforderlich</i></td>
   </tr>
</table>

## Stichproben {#samples}

### Zugriffstoken abrufen {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB Antwort - Erfolg]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1723227212,
  "expires_in": 86400,
  "token_type": "bearer"
}
```

>[!TAB Antwort - Fehler]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
