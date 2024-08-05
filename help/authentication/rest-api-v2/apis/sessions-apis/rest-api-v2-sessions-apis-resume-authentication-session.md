---
title: Authentifizierungssitzung fortsetzen
description: REST API V2 - Fortsetzen der Authentifizierungssitzung
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 1%

---


# Authentifizierungssitzung fortsetzen {#resume-authentication-session}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

## Anfrage {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/api/v2/{serviceProvider}/sessions/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Pfadparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Die interne eindeutige Kennung, die dem Service Provider während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>erforderlich</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">code</td>
      <td>Der Authentifizierungscode, der nach der Erstellung der Authentifizierungssitzung auf dem Streaming-Gerät abgerufen wurde.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Textparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>
        Die interne eindeutige Kennung, die dem Identitäts-Provider während des Onboarding-Prozesses zugeordnet ist.
        <br/><br/>
        Wenn die Bereitstellung eines Werts in der Streaming-Geräteplattform eingeschränkt ist, muss eine Anwendung die Authentifizierungssitzung fortsetzen und einen gültigen Wert angeben.
      </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Die Ursprungsdomäne der Anwendung, die die MVPD-Anmeldung durchführt.
        <br/><br/>
        Wenn die Bereitstellung eines Werts in der Streaming-Geräteplattform eingeschränkt ist, muss eine Anwendung die Authentifizierungssitzung fortsetzen und einen gültigen Wert angeben.
      </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        Die endgültige Umleitungs-URL, zu der der Benutzeragent navigiert, wenn der Authentifizierungsfluss für den MVPD abgeschlossen ist.
        <br/><br/>
        Der Wert muss URL-kodiert sein.
        <br/><br/>
        Wenn die Bereitstellung eines Werts in der Streaming-Geräteplattform eingeschränkt ist, muss eine Anwendung die Authentifizierungssitzung fortsetzen und einen gültigen Wert angeben.
        </td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Erstellung der Payload des Trägertokens wird in der Dokumentation <a href="../../../dynamic-client-registration-api.md">Dynamische Client-Registrierung</a> beschrieben.</td>
      <td><i>erforderlich</i></td>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">Code</th>
      <th style="background-color: #EFF2F7; width: 20%;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Der Antworttext enthält Informationen zu den nächsten Aktionen, die zur Durchführung der Authentifizierung erforderlich sind.
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
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation zur <a href="../../../dynamic-client-registration-api.md">dynamischen Client-Registrierung</a> .
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">body</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         JSON-Objekt mit den folgenden Attributen:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Die Aktion, die das Streaming-Gerät ausführen muss, um den Authentifizierungsfluss abzuschließen.
                  <br/><br/>
                  Mögliche Werte sind:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Wert</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">authentifizieren</td>
                        <td>Das Streaming-Gerät oder ein anderes Gerät muss die bereitgestellte URL in einem Benutzeragenten öffnen.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Wiederholen</td>
                        <td>Das Streaming-Gerät oder ein anderes Gerät muss die fehlenden Parameter bereitstellen und die Wiederaufnahme der Authentifizierungssitzung mit dem Code versuchen.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Autorisieren</td>
                        <td>Das Streaming-Gerät kann direkt mit Entscheidungsflüssen fortfahren.</td>
                     </tr>
                  </table>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Die Art der Interaktion, die das Streaming-Gerät ausführen muss, damit der Fluss mit der durch das Attribut "actionName"angegebenen Aktion fortgesetzt werden kann.
                  <br/><br/>
                  Mögliche Werte sind:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Wert</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">direct</td>
                        <td>Der Fluss wird mit einem direkten Aufruf an die angegebene URL fortgesetzt, wobei ein für die Client-Implementierung verfügbarer HTTP-Client verwendet wird.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">interaktiv</td>
                        <td>Der Fluss wird mit der Navigation zur angegebenen URL mithilfe eines Benutzeragenten fortgesetzt.</td>
                     </tr>
                  </table>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>Die fehlenden Parameter, die angegeben werden müssen, um den grundlegenden Authentifizierungsfluss abzuschließen.</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>Die URL, in der die Clientanwendung navigieren muss.</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">code</td>
               <td>Der Authentifizierungscode, der in einer sekundären Anwendung verwendet werden kann, um die Authentifizierungssitzung wiederaufzunehmen.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>Die undurchsichtige Kennung, die zur Verfolgung der Benutzeraktivität verwendet werden kann.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Die interne eindeutige Kennung, die dem Identitäts-Provider während des Onboarding-Prozesses zugeordnet ist.</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>Die interne eindeutige Kennung, die dem Service Provider während des Onboarding-Prozesses zugeordnet ist.</td>
               <td><i>erforderlich</i></td>
            </tr>
         </table>
      </td>
      <td><i>erforderlich</i></td>
</table>

### Fehler {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">body</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">error</td>
      <td>Der Fehler enthält zusätzliche Informationen, die der Dokumentation <a href="../../../enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.</td>
      <td><i>erforderlich</i></td>
   </tr>
</table>

## Stichproben {#samples}

### 1. Die Authentifizierungssitzung fortsetzen, ohne Werte für alle erforderlichen Parameter anzugeben

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
POST /api/v2/REF30/sessions/8BLW4RW
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "retry",
   "actionType" : "interactive",
   "url" : "/v2/REF30/sessions/8BLW4RW",
   "missingParameters" : ["redirectUrl"]
   "code" : "8BLW4RW",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
