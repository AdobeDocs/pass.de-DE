---
title: Profile abrufen
description: REST API V2 - Profile abrufen
exl-id: 72922aa8-95ca-48dc-8523-e335802fc366
source-git-commit: 32c3176fb4633acb60deb1db8fb5397bbf18e2d0
workflow-type: tm+mt
source-wordcount: '868'
ht-degree: 1%

---

# Profile abrufen {#retrieve-profiles}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

>[!MORELIKETHIS]
>
> Stellen Sie sicher, dass Sie auch die häufig gestellten Fragen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) besuchen.

## Anfrage {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/api/v2/{serviceProvider}/profiles</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
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
      <td>Die interne eindeutige Kennung, die dem Dienstleister während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Generierung der Bearer-Token-Payload wird in der Dokumentation zur <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">-Kopfzeile </a>.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ap-device-identifier</td>
      <td>Die Erstellung der Payload der Gerätekennung wird in der Header-Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> beschrieben.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">x-device-info</td>
      <td>
         Die Erzeugung der Payload mit Geräteinformationen wird in der Header-Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> beschrieben.
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
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Die IP-Adresse des Streaming-Geräts.
         <br/><br/>
         Es wird dringend empfohlen, sie immer für Server-zu-Server-Implementierungen zu verwenden, insbesondere wenn der Aufruf vom Programmierdienst und nicht vom Streaming-Gerät erfolgt.
         <br/><br/>
         Bei Client-zu-Server-Implementierungen wird die IP-Adresse des Streaming-Geräts implizit gesendet.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-subject-token<br/>or<br/>x-roku-reserved-roku-connect-token</td>
      <td>
        Die Erstellung der Single-Sign-On-Payload für die Platform-Identitätsmethode wird in der Kopfzeilendokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a> / <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md">X-Roku-Reserved-Roku-Connect-Token</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu Flüssen, für die Single Sign-on unter Verwendung einer Platform-Identität aktiviert ist, finden Sie in der Dokumentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Single Sign-on unter Verwendung von Platform-Identitätsflüssen</a> .
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        Die Erstellung der Single Sign-On-Payload für die Service-Token-Methode wird in der Header-Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu Flüssen, die Single Sign-on mit einem Service-Token aktivieren, finden Sie in der Dokumentation <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">Single Sign-on mit Service-Token-Flüssen</a> .
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ap-partner-framework-status</td>
      <td>
        Die Erstellung der Single Sign-On-Payload für die Partner-Methode wird in der Header-Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu Flüssen, für die Single Sign-on unter Verwendung eines Partners aktiviert ist, finden Sie in der <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Single Sign-on unter Verwendung von Partnerflüssen</a> Dokumentation.</td>
      <td>fakultativ</td>
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
        Der Antworttext enthält eine Zuordnung gültiger Profile, die leer sein können.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Fehlerhafte Anfrage</td>
      <td>
        Die Anfrage ist ungültig. Der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Nicht autorisiert</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode nicht zulässig</td>
      <td>
        Die HTTP-Methode ist ungültig. Der Client muss eine HTTP-Methode verwenden, die für die angeforderte Ressource zulässig ist, und erneut versuchen. Weitere Informationen finden Sie im Abschnitt <a href="#request">Anfrage</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interner Server-Fehler</td>
      <td>
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Erweiterte Fehlercodes</a> entsprechen.
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
      <td style="background-color: #DEEBFF;">Profile</td>
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
               <td>Die interne eindeutige Kennung, die dem Identitätsanbieter während des Onboarding-Prozesses zugeordnet ist.</td>
               <td><i>required</i></td>
            </tr>
         </table>
         Das value-Element wird durch die folgenden Attribute definiert:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Der Zeitstempel, vor dem das Profil ungültig ist.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Der Zeitstempel, nach dem das Profil ungültig ist.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Aussteller</td>
               <td>
                  Die Entität, der das Profil gehört.
                  <br/><br/>
                  Die möglichen Werte sind:
                  <ul>
                    <li><b>mvpd (z. B. Spectrum, Cablevision usw.)</b><br/>Das Profil wurde erstellt als Ergebnis von: Standardauthentifizierung, Single Sign-on mit Platform-Identität oder Single Sign-on mit Service-Token.</li>
                    <li><b>Apple</b><br/>Das Profil wurde erstellt aus: Single Sign-on mit Partner-Apple.</li>
                  </ul>
               </td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Typ</td>
               <td>
                  Der Profiltyp.
                  <br/><br/>
                  Die möglichen Werte sind:
                  <ul>
                    <li><b>normal</b><br/>Das Profil wurde als Ergebnis von: Standardauthentifizierung erstellt.</li>
                    <li><b>appleSSO</b><br/>Das Profil wurde erstellt als Ergebnis von: Single Sign-on mit Partner-Apple.</li>
                    <li><b>platformSSO</b><br/>Das Profil wurde erstellt als Ergebnis von: Single Sign-on mit Platform-Identität.</li>
                    <li><b>serviceTokenSSO</b><br/>Das Profil wurde erstellt als Ergebnis von: Single Sign-on mithilfe des Service-Tokens.</li>
                  </ul>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Attribute</td>
               <td>
                    JSON mit einer Zuordnung von Schlüssel-/Wert-Paaren.
                    <br/><br/>
                    Das Schlüsselelement wird durch Benutzermetadatenattribute definiert und kann sein:
                    <ul>
                        <li>Obligatorisch, wie „userID“</li>
                        <li>Nicht obligatorisch, wie „zip“, „householdID“, „maxRating“ usw.</li>
                    </ul>
                    Die Werte für die Attribute können wie folgt sein:
                    <ul>
                        <li>schlicht</li>
                        <li>auflisten</li>
                        <li>kartieren</li>
                    </ul>
                    Benutzermetadaten werden nach Abschluss des Authentifizierungsflusses verfügbar, aber bestimmte Metadatenattribute können während des Autorisierungsflusses aktualisiert werden, je nach MVPD und dem betreffenden spezifischen Metadatenattribut.
               </td>
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
      <td>400, 401, 405, 500</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>Der Antworttext kann zusätzliche Fehlerinformationen bereitstellen, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Erweiterte Fehlercodes</a> entsprechen.</td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples}

### 1. Abrufen von Profilen, die durch Standardauthentifizierung ermittelt wurden

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/v2/REF30/profiles HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8
 
{
    "profiles": {
        "Cablevision": {
            "notBefore": 1623943955,
            "notAfter": 1623951155,
            "issuer": "Cablevision",
            "type": "regular",
            "attributes": {
                "userID": {
                    "value": "BASE64_value_userId",
                    "state": "plain"
                },
                "householdID": {
                    "value": "BASE64_value_householdId",
                    "state": "plain"
                },
                "zip": {
                    "value": "BASE64_value_zip",
                    "state": "enc"
                }       
            }
        },
        "Spectrum": {
            "notBefore": 1623943955,
            "notAfter": 1623951155,
            "issuer": "Spectrum",
            "type": "regular",
            "attributes": {
                "userID": {
                    "value": "BASE64_value_userId",
                    "state": "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### 2. Abrufen von Profilen, die über Standardauthentifizierung oder Single Sign-on mit der Service-Token-Methode abgerufen wurden

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/v2/REF30/profiles HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJkZDNmYWIyN2NmMjg0ZmU2ZWU0ZDY3ZmExZjY4MzE3YyIsImlzcyI6IkFkb2JlIiw.....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "profiles": {
      "AdobeShibboleth": {
         "notBefore": 1748073636999,
         "notAfter": 1748105173000,
         "issuer": "AdobeShibboleth",
         "type": "serviceTokenSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd1...",
               "state": "plain"
            },
            "userID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd14aTV....",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobeShibboleth",
               "state": "plain"
            }
         }
      },
      "Spectrum": {
         "notBefore": 1623943955,
         "notAfter": 1623951155,
         "issuer": "Spectrum",
         "type": "regular",
         "attributes": {
            "userID": {
               "value": "BASE64_value_userId",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. Abrufen von Profilen, die durch einfache Authentifizierung oder Single Sign-on mit der Platform-Identitätsmethode abgerufen wurden

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/v2/REF30/profiles HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIyMmM4MDU1MjEzMDIwYzhmZGYzOGZkMTI1YWViMzUzYSIsImlzcyI6....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8
 
{
   "profiles": {
      "AdobePass_SMI": {
         "notBefore": 1724337476000,
         "notAfter": 1724345252000,
         "issuer": "AdobePass_SMI",
         "type": "platformSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "userID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobePass_SMI",
               "state": "plain"
            }
         }
      },
      "Cablevision": {
         "notBefore": 1623943955,
         "notAfter": 1623951155,
         "issuer": "Spectrum",
         "type": "regular",
         "attributes": {
            "userID": {
               "value": "BASE64_value_userId",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]
