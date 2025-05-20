---
title: Partnerauthentifizierungsanfrage abrufen
description: REST API V2 - Partnerauthentifizierungsanfrage abrufen
exl-id: 52d8a8e9-c176-410f-92bc-e83449278943
source-git-commit: 9f872ad94e0aaa0546c8fcac9f88c50f47e10f6e
workflow-type: tm+mt
source-wordcount: '1236'
ht-degree: 0%

---

# Partnerauthentifizierungsanfrage abrufen {#retrieve-partner-authentication-request}

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
      <td>/api/v2/{serviceProvider}/sessions/sso/{partner}</td>
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
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Die Ursprungs-Domain der Anwendung, die die MVPD-Anmeldung durchführt.
        <br/><br/>
        Wenn die Plattform des Streaming-Geräts Einschränkungen bei der Bereitstellung eines Werts hat, muss eine Anwendung die Authentifizierungssitzung fortsetzen und einen gültigen Wert angeben.
        <br/><br/>
        Dies wird bei Fallback-Szenarien verwendet, in denen die Antwort angibt, dass die Streaming-Anwendung mit dem einfachen Authentifizierungsfluss fortfahren soll.
      </td>
      <td><i>required</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        Die endgültige Umleitungs-URL, zu der der Benutzeragent navigiert, wenn der Authentifizierungsfluss für die MVPD abgeschlossen ist.
        <br/><br/>
        Der Wert muss URL-kodiert sein.
        <br/><br/>
        Wenn die Plattform des Streaming-Geräts Einschränkungen bei der Bereitstellung eines Werts hat, muss eine Anwendung die Authentifizierungssitzung fortsetzen und einen gültigen Wert angeben.
        <br/><br/>
        Dies wird bei Fallback-Szenarien verwendet, in denen die Antwort angibt, dass die Streaming-Anwendung mit dem einfachen Authentifizierungsfluss fortfahren soll.
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
      <td>200</td>
      <td>OK</td>
      <td>
        Der Antworttext enthält Informationen zu den nächsten Aktionen, die zum Ausführen der Authentifizierung erforderlich sind.
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
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Die Aktion, die das Streaming-Gerät ausführen muss, um den Authentifizierungsfluss abzuschließen.
                  <br/><br/>
                  Die möglichen Werte sind:
                  <ul>
                    <li><b>partner_profile</b><br/> Das Streaming-Gerät kann die bereitgestellte Partnerauthentifizierungsanfrage verwenden, um eine Partnerauthentifizierungsantwort abzurufen, die zum Abrufen eines Profils genutzt werden kann.</li>
                    <li><b>Authentifizieren</b><br/> Wenn der Fluss für einmaliges Anmelden des Partners nicht fortgesetzt werden kann, kann das Streaming-Gerät auf den einfachen Authentifizierungsfluss zurückgreifen.<br/>Das Streaming-Gerät oder ein anderes Gerät muss die bereitgestellte URL in einem Benutzeragenten öffnen.</li>
                    <li><b>Wiederaufnehmen</b><br/> Wenn der Fluss für einmaliges Anmelden des Partners nicht fortgesetzt werden kann, kann das Streaming-Gerät auf den einfachen Authentifizierungsfluss zurückgreifen.<br/>Das Streaming-Gerät oder ein anderes Gerät muss die fehlenden Parameter angeben und die Authentifizierungssitzung mithilfe des Codes fortsetzen.</li>
                    <li><b>authorize</b><br/> Das Streaming-Gerät kann direkt mit Entscheidungsflüssen fortfahren.</li>
                  </ul>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Der Interaktionstyp, den das Streaming-Gerät ausführen muss, um den Fluss mit der durch das Attribut „actionName“ angegebenen Aktion fortzusetzen.
                  <br/><br/>
                  Die möglichen Werte sind:
                  <ul>
                    <li><b>Interaktiv</b><br/>Der Fluss wird mit einer Navigation zur angegebenen URL mithilfe eines Benutzeragenten fortgesetzt.</li>
                    <li><b>direct</b><br/>Der Fluss wird mit einem direkten Aufruf an die bereitgestellte URL fortgesetzt, wobei ein für die Client-Implementierung verfügbarer HTTP-Client verwendet wird.</li>
                  </ul>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">reasonType</td>
               <td>
                  Der Typ des Grundes, der den „actionName“ erklärt.
                  <br/><br/>
                  Die möglichen Werte sind:
                  <ul>
                    <li><b>none</b><br/>Die Client-Anwendung ist erforderlich, um weiterhin authentifiziert zu werden.</li>
                    <li><b>authentifiziert</b><br/>Die Client-Anwendung wird bereits über grundlegende Zugriffsflüsse authentifiziert.</li>
                    <li><b>Heruntergestuft</b><br/> Die Client-Anwendung wird bereits über eingeschränkte Zugriffsflüsse authentifiziert.</li>
                    <li><b>AuthenticatedSSO</b><br/>Die Client-Anwendung wird bereits über Single Sign-on-Zugriffsflüsse authentifiziert.</li>
                    <li><b>pfs_fallback</b><br/>Die Client-Anwendung muss aufgrund eines fehlenden oder ungültigen Kopfzeilenwerts <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> zum einfachen Authentifizierungsfluss zurückkehren.</li>
                    <li><b>configuration_fallback</b><br/>Die Client-Anwendung muss aufgrund der Partnerkonfiguration für einmaliges Anmelden im Adobe Pass-Backend auf einen einfachen Authentifizierungsfluss zurückgreifen.</li>
                    <li><b>missing_parameters_fallback</b><br />Die Client-Anwendung muss aufgrund eines fehlenden oder ungültigen Parameters zum Fortsetzungsfluss zurückkehren.</li>
                  </ul>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>
                    Die fehlenden Parameter, die angegeben werden müssen, um den einfachen Authentifizierungsfluss abzuschließen.
                    <br/><br/>
                    Dieses Feld ist vorhanden, wenn der Single-Sign-On-Fluss des Partners nicht fortgesetzt werden kann.
               </td>
               <td>fakultativ</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">URL</td>
               <td>Die URL, unter der die Client-Anwendung navigieren muss.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Code</td>
               <td>
                    Der Authentifizierungs-Code, der auf einer sekundären Anwendung verwendet werden kann, um die Authentifizierungssitzung fortzusetzen.
                    <br/><br/>
                    Dieses Feld ist vorhanden, wenn der Single-Sign-On-Fluss des Partners nicht fortgesetzt werden kann.
               </td>
               <td>fakultativ</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    Die Partnerauthentifizierungsanfrage, die im Authentifizierungsfluss mit dem Partner außerhalb des Adobe Pass-Authentifizierungssystems verwendet werden soll.
                    <br/><br/>
                    Dieses Feld ist vorhanden, wenn der Fluss für einmaliges Anmelden des Partners fortgesetzt werden kann.
                    <br/><br/>
                    JSON-Objekt mit den folgenden Attributen:
                    <ul>
                        <li><b>type</b><br/>Gibt den Protokolltyp an, der von MVPD unterstützt wird (nur SAML).</li>
                        <li><b>request</b><br/>Die SAML-Anfrage.</li>
                        <li><b>attributesNames</b><br/>Die SAML-Anfrageattribute.</li>
                    </ul>
               </td>
               <td>fakultativ</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>Die opake Kennung, die zum Tracking von Benutzeraktivitäten verwendet werden kann.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Die interne eindeutige Kennung, die dem Identitätsanbieter während des Onboarding-Prozesses zugeordnet ist.</td>
               <td>fakultativ</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>Die interne eindeutige Kennung, die dem Dienstleister während des Onboarding-Prozesses zugeordnet ist.</td>
               <td><i>required</i></td>
            </tr>
         </table>
      </td>
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

### 1. Partnerauthentifizierungsanfrage abrufen

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "partner_profile",
    "actionType": "direct",
    "reasonType": "none",
    "url": "/api/v2/REF30/profiles/sso/Apple",
    "sessionId": "83c046be-ea4b-4581-b5f2-13e56e69dee9",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "authenticationRequest": {
        "type": "saml",
        "request": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG....",
        "attributesNames": ["uid", "NameID", "uniqueId"]
   }
}    
```

>[!ENDTABS]

### 2. Partner-Authentifizierungsanfrage abrufen, es kommt jedoch zu einer Beeinträchtigung

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiJHtkZWdyYWRlZE12cGR9IiwKICAgICAgImV4cGlyYXRpb25EYXRlIiA6ICIyMDI1NDMwNjM2MDAwIgogICAgfQp9
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "degraded",
    "url": "/api/v2/REF30/decisions/authorize/${degradedMvpd}",
    "sessionId": "14d4f239-e3b1-4a4a-b8b3-6395b968a260",
    "mvpd": "${degradedMvpd}",
    "serviceProvider": "REF30"
}
```

>[!ENDTABS]

### 3. Abrufen der Partnerauthentifizierungsanfrage, aber Zurückfallen auf den einfachen Authentifizierungsfluss aufgrund eines fehlenden oder ungültigen Kopfzeilenwerts für den AP-Partner-Framework-Status

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImRlbmllZCIKICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHt9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authenticate",
    "actionType": "interactive",
    "reasonType": "pfs_fallback",
    "url": "/api/v2/authenticate/REF30/OKTWW2W",
    "code": "OKTWW2W",
    "sessionId": "748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### 4. Abrufen der Partnerauthentifizierungsanfrage, jedoch aufgrund der Partnerkonfiguration mit Single Sign-on im Adobe Pass-Backend auf den einfachen Authentifizierungsfluss zurückgreifen

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authenticate",
    "actionType": "interactive",
    "reasonType": "configuration_fallback",
    "url": "/api/v2/authenticate/REF30/OKTWW2W",
    "code": "OKTWW2W",
    "sessionId": "748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### 5. Partner-Authentifizierungsanfrage abrufen, aber aufgrund fehlender Parameter auf einfachen Authentifizierungsfluss zurückgreifen

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "resume",
    "actionType": "direct",
    "reasonType": "missing_parameters_fallback",
    "missingParameters": [
          "redirectUrl"
    ],
    "url": "/api/v2/REF30/sessions/SB7ZRIO",
    "code": "SB7ZRIO",
    "sessionId": "1476173f-5088-43b8-b7c3-8cf3a185de0a",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]
