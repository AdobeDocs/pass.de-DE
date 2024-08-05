---
title: Abrufen von Vorab-Autorisierungsentscheidungen mit einer bestimmten mvpd
description: REST API V2 - Abrufen von Entscheidungen zur Vorabautorisierung mithilfe bestimmter mvpd
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 0%

---


# Abrufen von Vorab-Autorisierungsentscheidungen mit einer bestimmten mvpd {#retrieve-preauthorization-decisions-using-specific-mvpd}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Anfrage {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/api/v2/{serviceProvider}/entscheidungen/preauthorize/{mvpd}</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>Die interne eindeutige Kennung, die dem Identitäts-Provider während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Textparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">resources</td>
      <td>Die Liste der Ressourcen, für die eine MVPD-Entscheidung erforderlich ist, bevor sie angezeigt werden können.</td>
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
         Es muss application/json sein.
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
        Die Generierung der Single Sign-On-Payload für die Platform Identity-Methode wird in der Dokumentation zu <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu für Single Sign-on aktivierten Flüssen mit einer Plattformidentität finden Sie in der Dokumentation zu <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Single Sign-on mit Platform-Identitätsflüssen</a> .
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        Die Generierung der Single Sign-On-Payload für die Service Token-Methode wird in der Dokumentation zu <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu für die einmalige Anmeldung aktivierten Flüssen mit einem Dienst-Token finden Sie in der Dokumentation zum <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md">Single Sign-on mit Service-Token-Flüssen</a> .
      </td>
      <td>optional</td>
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
        Der Antworttext enthält eine Liste von Entscheidungen mit zusätzlichen Informationen.
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
      <td style="background-color: #DEEBFF;">Entscheidungen</td>
      <td>
         JSON, das eine Liste von Elementen enthält, wobei jedes Element die folgenden Attribute aufweist:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">resource</td>
                <td>Die Kennung der Ressource, für die die Vorautorisierungsentscheidung zurückgegeben wird.</td>
                <td><i>erforderlich</i></td>
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
                <td style="background-color: #DEEBFF;">autorisiert</td>
                <td>Der Entscheidungsstatus für die Ressource, der entweder "true"oder "false"sein kann.</td>
                <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">source</td>
               <td>
                  Informationen zur Entscheidungsquelle:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Wert</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">mvpd</td>
                        <td>Die Entscheidung wird vom MVPD-Vorabgenehmigungs-Endpunkt erteilt.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Abbau</td>
                        <td>Die Entscheidung wird aufgrund des eingeschränkten Zugangs erteilt.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">temppass</td>
                        <td>Die Entscheidung wird aufgrund des vorübergehenden Zugangs erlassen.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Dummy</td>
                        <td>Die Entscheidung wird aufgrund der Platzhaltervorautorisierungsfunktion erteilt.</td>
                     </tr>
                  </table>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">error</td>
               <td>Der Fehler enthält zusätzliche Informationen zur Entscheidung "Ablehnen", die der Dokumentation <a href="../../../enhanced-error-codes.md">Erweiterte Fehlercodes</a> entspricht.</td>
               <td>optional</td>
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

### 1. Vorabgenehmigungsentscheidungen mithilfe der regulären mvpd abrufen

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
POST /api/v2/REF30/decisions/preauthorize/Cablevision

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:       

{
    "resources": ["resource1", "resource2", "resource3"]
}
```

>[!TAB Antwort - Entscheidungen über Genehmigungen und Ablehnen]

```JSON
HTTP/1.1 200 OK 
Content-Type: application/json  

{
   "decisions": [
      {
         "resource": "resource1 ",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": true
      },
      {
         "resource": "resource2",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": true
      },
      {
         "resource": "resource3",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": false,
         "error": {
            "status": 202,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"
         }
      }
   ]
}
```

>[!ENDTABS]

### 2. Vorabgenehmigungsentscheidungen mithilfe von &quot;mvpd&quot;abrufen

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
POST /api/v2/REF30/decisions/preauthorize/degradedMvpd

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB Antwort - AuthNAll-Abbau]

```JSON
HTTP/1.1 200 OK 
Content-Type: application/json
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
    ]
}
```

**Hinweis:** In diesem Fall hat die AuthZAll-Regel &quot;channel&quot;: &quot;apasstest1&quot;

>[!TAB Antwort - AuthZAll-Abbau]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8 
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
    ]
}
```

**Hinweis:** In diesem Fall hat die AuthZAll-Regel &quot;channel&quot;: &quot;apasstest1&quot;

>[!TAB Antwort - AuthZNone Degradation]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8 
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
 
    ]
}
```

**Hinweis:** In diesem Fall hat die AuthZNone-Regel &quot;channel&quot;: &quot;apasstest1&quot;

>[!TAB Antwort - Regel für abgelaufenen Abbau]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_configuration_change",
                "message": "AuthXAll degradation configuration changed, please try again!",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
