---
title: Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD
description: REST API V2 - Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD
exl-id: e8889395-4434-4bec-a212-a8341bb9c310
source-git-commit: 110e8519d6c042cc38de3fbefcd34297b6edcfad
workflow-type: tm+mt
source-wordcount: '1006'
ht-degree: 0%

---

# Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD {#retrieve-authorization-decisions-using-specific-mvpd}

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
      <td>/api/v2/{serviceProvider}/decisions/authorize/{mvpd}</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>Die interne eindeutige Kennung, die dem Identitätsanbieter während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Hauptteilparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Ressourcen</td>
      <td>Die Liste der Ressourcen, für die eine MVPD-Entscheidung erforderlich ist, bevor sie wiedergegeben werden können.</td>
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
         Es muss application/json;charset=utf-8 sein.
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
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>Die Erstellung der Payload der eindeutigen Benutzerkennung wird in der Header-Dokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a> beschrieben.</td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Visitor-Identifier</td>
      <td>
        Die Erstellung der Payload der Besucherkennung wird in der Kopfzeilendokumentation <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md">AP-Visitor-Identifier</a> beschrieben.
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Akzeptieren</td>
      <td>
         Der von der Client-Anwendung akzeptierte Medientyp.
         <br/><br/>
         Wenn angegeben, muss es application/json;charset=utf-8 sein.
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
        Der Antworttext enthält eine Liste von Entscheidungen mit zusätzlichen Informationen.
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
      <td style="background-color: #DEEBFF;">Entscheidungen</td>
      <td>
         JSON mit einer Liste von Elementen, wobei jedes Element die folgenden Attribute aufweist:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">Ressource</td>
                <td>Die Ressourcenkennung, für die die Autorisierungsentscheidung zurückgegeben wird.</td>
                <td><i>required</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">serviceProvider</td>
                <td>Die interne eindeutige Kennung, die dem Dienstleister während des Onboarding-Prozesses zugeordnet ist.</td>
                <td><i>required</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpd</td>
                <td>Die interne eindeutige Kennung, die dem Identitätsanbieter während des Onboarding-Prozesses zugeordnet ist.</td>
                <td><i>required</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">bevollmächtigt</td>
                <td>Der Entscheidungsstatus für die Ressource, der entweder „true“ oder „false“ sein kann.</td>
                <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Quelle</td>
               <td>
                  Informationen zur Entscheidungsquelle.
                  <br/><br/>
                  Die möglichen Werte sind:
                  <ul>
                    <li><b>mvpd</b><br/>decision wird vom MVPD-Autorisierungsendpunkt ausgegeben.</li>
                    <li><b>Abbau</b><br/>Entscheidung wird aufgrund eines eingeschränkten Zugriffs erlassen.</li>
                    <li><b>temppass</b><br/>Decision wird als Ergebnis des temporären Zugriffs ausgegeben.</li>
                    <li><b>dummy</b><br/>Decision“ wird als Ergebnis der Dummy-Autorisierungsfunktion ausgegeben.</li>
                  </ul>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Token</td>
               <td>
                  Informationen zum Medien-Token.
                  <br/><br/>
                  JSON-Objekt mit den folgenden Attributen:
                  <ul>
                    <li><b>notBefore</b><br/>Der Zeitstempel in Millisekunden, vor dem das Medien-Token nicht gültig ist.</li>
                    <li><b>notAfter</b><br/>Der Zeitstempel in Millisekunden, nach denen das Medien-Token ungültig ist.</li>
                    <li><b>serializedToken</b><br/>Das Base64-kodierte Medien-Token.</li>
                  </ul>
               <td>fakultativ</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Der Zeitstempel in Millisekunden, vor dem die Entscheidung nicht gültig ist.</td>
               <td>fakultativ</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Der Zeitstempel in Millisekunden, nach denen die Entscheidung ungültig ist.</td>
               <td>fakultativ</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Fehler</td>
               <td>Der Fehler liefert zusätzliche Informationen zur Entscheidung „Ablehnen“, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Erweiterte Fehlercodes</a> entspricht.</td>
               <td>fakultativ</td>
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
      <td>
            Der Antworttext kann zusätzliche Fehlerinformationen bereitstellen, die der Dokumentation <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Erweiterte Fehlercodes</a> entsprechen.
            <br/><br/>
            Die Client-Anwendung muss einen Mechanismus zur Fehlerbehandlung implementieren, der die Fehlercodes, die am häufigsten von dieser API zurückgegeben werden, ordnungsgemäß verarbeitet:
            <ul>
                <li>authorized_profile_missing</li>
                <li>authorized_profile_expi</li>
                <li>authorization_denied_by_mvpd</li>
                <li>network_received_error</li>
                <li>usw.</li>
            </ul>
            Die obige Liste erhebt keinen Anspruch auf Vollständigkeit. Die Client-Anwendung muss in der Lage sein, alle in der <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">öffentlichen Dokumentation“ definierten erweiterten Fehler-Codes zu </a>.
      </td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples}

### &#x200B;1. Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD, während die Entscheidung zulässig ist

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/decisions/authorize/Cablevision HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "Cablevision",
            "source": "mvpd",
            "authorized": true,
            "token": {
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            },
            "notBefore": 1697094207324,
            "notAfter": 1697098802367
        }
    ]
}
```

>[!ENDTABS]

### &#x200B;2. Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD, während die Entscheidung abgelehnt wird

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/decisions/authorize/Cablevision HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "Cablevision",
            "source": "mvpd",
            "authorized": false,
            "error": {
                "action": "none",
                "status": 403,
                "code": "authorization_denied_by_mvpd",
                "message": "The MVPD has returned a "Deny" decision when requesting authorization for the specified resource",
                "details": "Your subscription package does not include the "Live" channel",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
            },
            "notBefore": 1697094207324,
            "notAfter": 1697098802367
        }
    ]
}
```

>[!ENDTABS]

### &#x200B;3. Abrufen von Autorisierungsentscheidungen mithilfe spezifischer MVPD während der Degradierung

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/REF30/decisions/authorize/${degradedMvpd} HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB Antwort - AuthNAll-Beeinträchtigung]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            },
            "notBefore": 1697543318183,
            "notAfter": 1697549918183
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            },
            "notBefore": 1697543318183,
            "notAfter": 1697549918183
        }
    ]
}
```

>[!TAB Antwort - AuthZAll-Beeinträchtigung]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

>[!TAB Antwort - AuthZnOne-Abbau]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]

### &#x200B;4. Abrufen von Autorisierungsentscheidungen mit einfachem TempPass

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/apasstest1/decisions/authorize/TempPass_TEST40 HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Antwort - verfügbar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            },
            "notBefore": 1697094207324,
            "notAfter": 1697594802367
        }
    ]
}
```

>[!TAB Antwort - Dauer überschritten]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_duration_limit_exceeded",
                "message": "The temporary access duration limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB Antwort - Ungültige Konfiguration]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 500,
                "code": "invalid_configuration_temporary_access",
                "message": "The temporary access configuration is invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "action": "configuration"
            }
        }
    ]
}
```

>[!ENDTABS]

### &#x200B;5. Abrufen von Autorisierungsentscheidungen mithilfe von Werbe-TempPass

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/v2/apasstest1/decisions/authorize/flexibleTempPass HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Antwort - verfügbar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            },
            "notBefore": 1697543318183,
            "notAfter": 1697843918183
        }
    ]
}
```

>[!TAB Antwort - Dauer überschritten]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_duration_limit_exceeded",
                "message": "The temporary access duration limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB Antwort - Ressourcenlimit überschritten]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_resources_limit_exceeded",
                "message": "The temporary access resources limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB Antwort - Ungültige Konfiguration]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 500,
                "code": "invalid_configuration_temporary_access",
                "message": "The temporary access configuration is invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "action": "configuration"
            }
        }
    ]
}
```

>[!TAB Antwort - Ungültige Identität]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 400,
                "code": "invalid_header_identity_for_temporary_access",
                "message": "The identity for temporary access header value is missing or invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
