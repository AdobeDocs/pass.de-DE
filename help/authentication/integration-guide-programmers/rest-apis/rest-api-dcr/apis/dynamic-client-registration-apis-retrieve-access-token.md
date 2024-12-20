---
title: Zugriffstoken abrufen
description: Dynamic Client Registration API - Zugriffstoken abrufen
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Zugriffstoken abrufen {#retrieve-access-token}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die Implementierung der dynamischen Client-Registrierungs-API ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

## Anfrage {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Hauptteilparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            Die Zeichenfolge der Client-Anwendungskennung.
            <br/><br/>
            Weitere Informationen zum Abrufen der Client-Kennungszeichenfolge finden Sie in der API<a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Dokumentation zum Abrufen von Client</a>Anmeldeinformationen.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secret</td>
      <td>
            Die geheime Zeichenfolge des Client-Programms.
            <br/><br/>
            Weitere Informationen zum Abrufen der Client-Geheimnis-Zeichenfolge finden Sie in der API<a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Dokumentation zum Abrufen von Client</a>Anmeldeinformationen.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            Die Zeichenfolge vom Gewährungstyp (z. B. „client_credentials„), die die Client-Anwendung für den Client-Token-Endpunkt verwenden kann.
            <br/><br/>
            Weitere Informationen zum Abrufen der Zeichenfolge vom Gewährungstyp finden Sie in der API<a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Dokumentation zum Abrufen von Client</a>Anmeldeinformationen.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>
         Der akzeptierte Medientyp für die gesendeten Ressourcen.
         <br/><br/>
         Es muss application/x-www-form-urlencoded lauten.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">x-device-info</td>
      <td>
         Die Erzeugung der Payload mit Geräteinformationen wird in der Dokumentation <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> beschrieben.
         <br/><br/>
         Es wird dringend empfohlen, sie immer dann zu verwenden, wenn die Geräteplattform der Anwendung die explizite Bereitstellung gültiger Werte zulässt.
         <br/><br/>
         Wenn angegeben, führt das Backend für die Adobe Pass-Authentifizierung explizit eingestellte Werte mit extrahierten Werten implizit zusammen (standardmäßig).
         <br/><br/>
         Wenn keine Angabe gemacht wird, verwendet das Backend für die Adobe Pass-Authentifizierung implizit die extrahierten Werte (standardmäßig).
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Akzeptieren</td>
      <td>
         Der von der Client-Anwendung akzeptierte Medientyp.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">user-agent</td>
      <td>Der Benutzeragent der Client-Anwendung.</td>
      <td>fakultativ</td>
   </tr>
</table>

## Antwort {#response}

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
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
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
               <td style="background-color: #DEEBFF;">ID</td>
               <td>Die opake Kennung, die zum Tracking von Benutzeraktivitäten verwendet werden kann.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>Der Wert des Zugriffstokens, den die Client-Anwendung für die Autorisierungs-Kopfzeile verwenden muss.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>Der Zeitpunkt, zu dem das Zugriffstoken ausgegeben wurde.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>Die Zeit in Sekunden, bis das Zugriffstoken abläuft.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>Der Tokentyp (z. B. „Bearer„).</td>
               <td><i>required</i></td>
            </tr>
         </table>
      </td>
      <td><i>required</i></td>
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
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Fehler</td>
      <td>
        Die möglichen Werte sind:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Wert</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Invalid_request</td>
               <td>
                    Die Anfrage ist aus einem der folgenden Gründe ungültig:
                    <ul>
                        <li>In der Anfrage fehlt ein erforderlicher Parameter.</li>
                        <li>Die Anfrage enthält einen nicht unterstützten Parameterwert (außer Gewährungstyp).</li>
                        <li>Die Anfrage wiederholt einen Parameter.</li>
                        <li>Die Anfrage enthält mehrere Anmeldeinformationen.</li>
                        <li>Die Anfrage verwendet mehr als einen Mechanismus zur Authentifizierung des Clients.</li>
                        <li>Die Anfrage ist fehlerhaft.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>Die Client-Anmeldeinformationen sind ungültig. Der Client muss neue Client-Anmeldeinformationen abrufen und es erneut versuchen. Weitere Informationen finden Sie in der API<a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Dokumentation zum Abrufen von </a>.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>Der verwendete Gewährungstyp ist ungültig.</td>
            </tr>
         </table>
      </td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples}

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
