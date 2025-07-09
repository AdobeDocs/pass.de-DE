---
title: Erstellen und Abrufen eines Profils mithilfe der Antwort zur Partnerauthentifizierung
description: REST API V2 - Erstellen und Abrufen von Profilen mit der Antwort der Partnerauthentifizierung
exl-id: cae260ff-a229-4df7-bbf9-4cdf300c0f9a
source-git-commit: ebe0a53e3ba54c2effdef45c1143deea0e6e57d3
workflow-type: tm+mt
source-wordcount: '779'
ht-degree: 1%

---

# Erstellen und Abrufen eines Profils mithilfe der Antwort zur Partnerauthentifizierung {#create-and-retrieve-profile-using-partner-authentication-response}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

## Anfrage {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/api/v2/{serviceProvider}/profiles/sso/{partner}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>POST</td>
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
      <td style="background-color: #DEEBFF;">Kamerad</td>
      <td>Der Name des Partners (z. B. Apple), der das in den Adobe Pass-Authentifizierungsflüssen integrierte Single-Sign-On-Framework bereitstellt.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Hauptteilparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">SAMLResponse</td>
      <td>
        Die Antwort zur Partnerauthentifizierung, die die erforderlichen Benutzermetadaten zum Erstellen und Speichern eines Partnerprofils enthält.
        <br/><br/>
        Der Wert muss Base64-kodiert und danach URL-kodiert sein.
      </td>
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
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>
         Der akzeptierte Medientyp für die gesendeten Ressourcen.
         <br/><br/>
         Es muss application/x-www-form-urlencoded lauten.
      </td>
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
      <td style="background-color: #DEEBFF;">ap-partner-framework-status</td>
      <td>
        Die Erstellung der Single Sign-On-Payload für die Partner-Methode wird in der Header-Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> beschrieben.
        <br/><br/>
        Weitere Informationen zu Flüssen, für die Single Sign-on unter Verwendung eines Partners aktiviert ist, finden Sie in der <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Single Sign-on unter Verwendung von Partnerflüssen</a> Dokumentation.</td>
      <td>fakultativ</td>
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
      <td>201</td>
      <td>Erstellt</td>
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
               <td>Der Zeitstempel in Millisekunden, vor dem das Profil ungültig ist.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Der Zeitstempel in Millisekunden, nach denen das Profil ungültig ist.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Aussteller</td>
               <td>
                  Die Entität, der das Profil gehört.
                  <br/><br/>
                  Die möglichen Werte sind:
                  <ul>
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
                    <li><b>appleSSO</b><br/>Das Profil wurde erstellt als Ergebnis von: Single Sign-on mit Partner-Apple.</li>
                  </ul>
               </td>
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

### &#x200B;1. Erstellen und Abrufen eines Profils mithilfe der Antwort der Partnerauthentifizierung

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "Cablevision": {
            "notBefore": 1623943955,
            "notAfter": 1623951155,
            "issuer": "Apple",
            "type": "appleSSO",
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
        }
     }
}  
```

>[!ENDTABS]

### &#x200B;2. Profil mithilfe der Antwort der Partnerauthentifizierung erstellen und abrufen, es kommt jedoch zu einer Beeinträchtigung

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiJHtkZWdyYWRlZE12cGR9IiwKICAgICAgImV4cGlyYXRpb25EYXRlIiA6ICIyMDI1NDMwNjM2MDAwIgogICAgfQp9
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "${degradedMvpd}": {
            "notBefore": 1706636062704,
            "notAfter": 1706696062704,
            "issuer": "Adobe",
            "type": "degraded",
            "attributes": {
                "userID": {
                    "value": "95cf93bcd183214ac9e4433153cb8a9d180a463128c0a5d26f202e8c",
                    "state": "plain"
                }
            }
        }
   }
}
```

>[!ENDTABS]
