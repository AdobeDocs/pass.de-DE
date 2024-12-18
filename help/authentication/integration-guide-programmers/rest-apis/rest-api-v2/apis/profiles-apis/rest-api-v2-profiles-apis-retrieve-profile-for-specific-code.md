---
title: Profil für bestimmten Code abrufen
description: REST API V2 - Profil für bestimmten Code abrufen
exl-id: d6ead7d5-de5f-4033-8115-980953a370c0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 1%

---

# Profil für bestimmten Code abrufen {#retrieve-profile-for-specific-code}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) begrenzt.

## Anfrage {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/api/v2/{serviceProvider}/profiles/code/{code}</td>
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
      <td style="background-color: #DEEBFF;">code</td>
      <td>Der Authentifizierungscode, der nach der Erstellung der Authentifizierungssitzung auf dem Streaming-Gerät abgerufen wurde.</td>
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
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>Die Erstellung der Payload der eindeutigen Kennung des Benutzers wird in der Kopfzeilendokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a> beschrieben.</td>
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
        Der Antworttext enthält eine Zuordnung gültiger Profile, die leer sein können.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ungültige Anfrage</td>
      <td>
        Die Anfrage ist ungültig, der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Unerlaubt</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation zur <a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">Übersicht über die dynamische Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Verboten</td>
      <td>
        Die temporäre Zugriffszeit (TTL) ist abgelaufen oder die maximale Anzahl von Ressourcen wird überschritten. Der Client muss den Benutzer anweisen, einen einfachen Authentifizierungsfluss mit einem regulären MVPD zu initiieren. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.
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
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.
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
      <td style="background-color: #DEEBFF;">profiles</td>
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
               <td>Der Zeitstempel, vor dem das Profil nicht gültig ist.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Der Zeitstempel, nach dem das Profil nicht gültig ist.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Emittent</td>
               <td>
                  Die Entität, der das Profil gehört.
                  <br/><br/>
                  Mögliche Werte sind:
                  <ul>
                    <li><b>mvpd (z. B. Spektrum, Kabel usw.)</b><br/>Das Profil wurde durch folgende einfache Authentifizierung erstellt:</li>
                    <li><b>Adobe</b><br/>Das Profil wurde erstellt durch: eingeschränkten Zugriff, temporären Zugriff.</li>
                  </ul>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">type</td>
               <td>
                  Der Profiltyp.
                  <br/><br/>
                  Mögliche Werte sind:
                  <ul>
                    <li><b>normal</b><br/>Das Profil wurde als Ergebnis der einfachen Authentifizierung erstellt.</li>
                    <li><b>degraded</b><br/>Das Profil wurde durch folgenden eingeschränkten Zugriff erstellt:</li>
                    <li><b>temporär</b><br/>Das Profil wurde durch den temporären Zugriff erstellt.</li>
                  </ul>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">attributes</td>
               <td>
                    Die Liste der Benutzermetadatenattribute.
                    <br/><br/>
                    Diese Attribute können:
                    <ul>
                        <li>Obligatorisch, z. B. "userId"</li>
                        <li>Nicht obligatorisch, z. B. "zip", "budgetId", "maxRating" usw.</li>
                    </ul>
                    Die Werte für die Attribute können:
                    <ul>
                        <li>einfach</li>
                        <li>Liste</li>
                        <li>map</li>
                    </ul>
               </td>
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
      <td>400, 401, 403, 405, 500</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>Der Antworttext kann zusätzliche Fehlerinformationen bereitstellen, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.</td>
      <td><i>erforderlich</i></td>
   </tr>
</table>

## Stichproben {#samples}

### 1. Profil für spezifischen Code abrufen, der durch einfache Authentifizierung abgerufen wurde

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/v2/REF30/profiles/code/XTC98W HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
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
                "userId": {
                    "value": "BASE64_value_userId",
                    "state": "plain"
                },
                "householdId" : {
                    "value": "BASE64_value_householdId",
                    "state": "plain"
                },
                "zip" : {
                    "value": "BASE64_value_zip",
                    "state": "enc"
                },
                "parental-controls" : {
                    "value": BASE64_value_parental-controls,
                    "state": "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### 2. Profil für bestimmten Code abrufen, während der grundlegende TempPass ausgewählt ist

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/v2/REF30/profiles/code/XTC98W HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Antwort - verfügbar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "TempPass_TEST40": {
            "notBefore": 1697718650206,
            "notAfter": 1697718710206,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "expiration_date": {
                    "value": 1697718710206,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB Antwort - Maximale Dauer ]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_duration_limit_exceeded",
    "message": "The temporary access duration limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB Antwort - ungültige Konfiguration]

```HTTPS
HTTP/1.1 500 Internal Server Error

Content-Type: application/json;charset=UTF-8

{
    "status": 500,
    "code": "invalid_configuration_temporary_access",
    "message": "The temporary access configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "configuration"
}
```

>[!ENDTABS]

### 3. Profil für bestimmten Code abrufen, während die Promotion &quot;TempPass&quot;ausgewählt ist

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/v2/REF30/profiles/code/XTC98W HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Antwort - verfügbar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "flexibleTempPass": {
            "notBefore": 1697720528524,
            "notAfter": 1697720588524,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "remaining_resources": {
                    "value": 1,
                    "state": "plain"
                },
                "used_assets": {
                    "value": [
                        "res04",
                        "res02",
                        "res03",
                        "res01"
                    ],
                    "state": "plain"
                },
                "expiration_date": {
                    "value": 1697720528524,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB Antwort - Maximale Dauer ]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_duration_limit_exceeded",
    "message": "The temporary access duration limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB Antwort - Ressourcenlimit überschritten]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_resources_limit_exceeded",
    "message": "The temporary access resources limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB Antwort - ungültige Konfiguration]

```HTTPS
HTTP/1.1 500 Internal Server Error

Content-Type: application/json;charset=UTF-8

{
    "status": 500,
    "code": "invalid_configuration_temporary_access",
    "message": "The temporary access configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "configuration"
}
```

>[!TAB Antwort - ungültige Identität]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{
    "status": 400,
    "code": "invalid_header_identity_for_temporary_access",
    "message": "The identity for temporary access header value is missing or invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 4. Profil für spezifischen Code abrufen, während die Verschlechterung angewendet wird

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/v2/REF30/profiles/code/XTC98W HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Antwort - AuthNAll-Abbau]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "${degradedMvpd}": {
            "notBefore": 1697719042666,
            "notAfter": 1697719102666,
            "issuer": "Adobe",
            "type": "degraded",
            "attributes":
                "userID": {
                    "value": "95cf93bcd183214a0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!IMPORTANT]
>
> `95cf93bcd183214a` ist ein abbaubarkeitsspezifisches Präfix.

>[!ENDTABS]
