---
title: Anfrage zur Partnerauthentifizierung abrufen
description: REST API V2 - Anfrage zur Authentifizierung von Partnern abrufen
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---


# Anfrage zur Partnerauthentifizierung abrufen {#retrieve-partner-authentication-request}

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
      <td>/api/v2/{serviceProvider}/sessions/sso/{partner}</td>
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
      <td style="background-color: #DEEBFF;">Partner</td>
      <td>Der Name des Partners (z. B. Apple), der das Single Sign-on-Framework bereitstellt, das in Adobe Pass-Authentifizierungsflüsse integriert ist.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Textparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Die Ursprungsdomäne der Anwendung, die die MVPD-Anmeldung durchführt.
        <br/><br/>
        Wenn die Bereitstellung eines Werts in der Streaming-Geräteplattform eingeschränkt ist, muss eine Anwendung die Authentifizierungssitzung fortsetzen und einen gültigen Wert angeben.
        <br/><br/>
        Dies wird im Falle von Fallback-Szenarien verwendet, in denen die Antwort anzeigt, dass die Streaming-Anwendung mit dem grundlegenden Authentifizierungsfluss fortfahren soll.
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
        <br/><br/>
        Dies wird im Falle von Fallback-Szenarien verwendet, in denen die Antwort anzeigt, dass die Streaming-Anwendung mit dem grundlegenden Authentifizierungsfluss fortfahren soll.
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
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>Die Erstellung der Payload der Gerätekennung wird in der Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> beschrieben.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Die Erstellung der Payload der Geräteinformationen wird in der Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> beschrieben.
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
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        Die Generierung der Single Sign-On-Payload für die Partner-Methode wird in der Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu für Single Sign-on aktivierten Flüssen mit einem Partner finden Sie in der Dokumentation zu <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-partner-flows.md">Single Sign-on mit Partner-Flüssen</a> .</td>
      <td>optional</td>
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
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
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
                        <td style="background-color: #DEEBFF;">partner_profile</td>
                        <td>Das Streaming-Gerät kann die bereitgestellte Authentifizierungsanforderung des Partners verwenden, um eine Antwort zur Partnerauthentifizierung zu erhalten, die zum Abrufen eines Profils genutzt werden kann.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">authentifizieren</td>
                        <td>
                            Wenn der Single-Sign-On-Fluss des Partners nicht fortgesetzt werden kann, kann das Streaming-Gerät auf den grundlegenden Authentifizierungsfluss zurückgreifen.
                            <br/><br/>
                            Das Streaming-Gerät oder ein anderes Gerät muss die bereitgestellte URL in einem Benutzeragenten öffnen.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">resume</td>
                        <td>
                            Wenn der Single-Sign-On-Fluss des Partners nicht fortgesetzt werden kann, kann das Streaming-Gerät auf den grundlegenden Authentifizierungsfluss zurückgreifen.
                            <br/><br/>
                            Das Streaming-Gerät oder ein anderes Gerät muss die fehlenden Parameter bereitstellen und die Authentifizierungssitzung mithilfe des Codes fortsetzen.
                        </td>
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
               <td>
                    Die fehlenden Parameter, die angegeben werden müssen, um den grundlegenden Authentifizierungsfluss abzuschließen.
                    <br/><br/>
                    Dieses Feld ist vorhanden, wenn der Single-Sign-On-Fluss des Partners nicht fortgesetzt werden kann.
               </td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>Die URL, in der die Clientanwendung navigieren muss.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">code</td>
               <td>
                    Der Authentifizierungscode, der in einer sekundären Anwendung verwendet werden kann, um die Authentifizierungssitzung wiederaufzunehmen.
                    <br/><br/>
                    Dieses Feld ist vorhanden, wenn der Single-Sign-On-Fluss des Partners nicht fortgesetzt werden kann.
               </td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    Die Partnerauthentifizierungsanfrage, die im Authentifizierungsfluss mit dem Partner außerhalb des Adobe Pass-Authentifizierungssystem verwendet werden soll.
                    <br/><br/>
                    Dieses Feld ist vorhanden, wenn der Single Sign-On-Fluss für Partner fortgesetzt werden kann.
                    <br/><br/>
                    JSON-Objekt mit den folgenden Attributen:
                    <table>
                        <tr>
                            <th style="background-color: #EFF2F7; width: 30%;">Attribut</th>
                            <th style="background-color: #EFF2F7;"></th>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">type</td>
                            <td>
                                Gibt den vom MVPD unterstützten Protokolltyp an.
                                <br/><br/>
                                Mögliche Werte sind:
                                <table>
                                    <tr>
                                        <th style="background-color: #EFF2F7; width: 30%;">Wert</th>
                                        <th style="background-color: #EFF2F7;"></th>
                                    </tr>
                                    <tr>
                                        <td style="background-color: #DEEBFF;">saml</td>
                                        <td>Der MVPD unterstützt das SAML-Protokoll.</td>
                                    </tr>
                                </table>
                            </td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">Anfrage</td>
                            <td>Die SAML-Anfrage.</td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">attributes</td>
                            <td>Die SAML-Anforderungsattribute.</td>
                        </tr>
                    </table>
               </td>
               <td>optional</td>
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

### 1. Gültige Partner-SSO aktiviert

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"partner_profile",
   "actionType":"direct",
   "url":"/v2/REF30/profiles/sso/Apple/Cablevision",
   "sessionId":"83c046be-ea4b-4581-b5f2-13e56e69dee9",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30",
   "authenticationRequest":{
      "type":"saml",
      "request":"PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG...."
   }
}    
```

>[!ENDTABS]

### 2. Beschädigter MVPD

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
 
{          
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/api/v2/REF30/decisions",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30",
   "sessionId":"14d4f239-e3b1-4a4a-b8b3-6395b968a260"
}
```

>[!ENDTABS]

### 3. Deaktivierte Integration

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Antwort]

```JSON
HTTP/1.1 403
Content-Type: application/json; charset=utf-8
 
{
    "errors" : [
        {
            "code": "unknown_integration",
            "message": "The integration between the specified programmer and identity provider doesn't exist or it's disabled. Use the TVE Dashboard to register or enable the required integration.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}
```

>[!ENDTABS]

### 4. Partner SSO nicht aktiviert (in der Pass-Konfiguration oder in VSA) - Fallback zur regulären Authentifizierung, alle erforderlichen Parameter vorhanden

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status : ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"authenticate",
   "actionType":"interactive",
   "url":"/v2/authenticate/REF30/OKTWW2W",
   "code":"OKTWW2W",
   "sessionId":"748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}  
```

>[!ENDTABS]

### 5. Partner SSO nicht aktiviert (in der Konfiguration &quot;Pass&quot;oder in VSA) - Fallback zur regulären Authentifizierung, fehlende erforderliche Parameter, die Authentifizierungssitzung muss fortgesetzt werden

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"resume",
   "actionType":"direct",
   "missingParameters":[
      "redirectUrl"
   ],
   "url":"/v2/REF30/sessions/SB7ZRIO",
   "code":"SB7ZRIO",
   "sessionId":"1476173f-5088-43b8-b7c3-8cf3a185de0a",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}
```

>[!ENDTABS]
