---
title: Abrufen der Liste der vorab autorisierten Ressourcen durch die Web-App im zweiten Bildschirm
description: Abrufen der Liste der vorab autorisierten Ressourcen durch die Web-App im zweiten Bildschirm
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# (Legacy) Liste der vorab autorisierten Ressourcen durch Web-App auf dem zweiten Bildschirm abrufen {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselungsmechanismus) ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Eine Anfrage an die Adobe Pass-Authentifizierung zum Abrufen der Liste der vorab autorisierten Ressourcen.

Es gibt zwei Sätze von APIs: einen Satz für die Streaming-App oder den Programmierer-Service und einen Satz für die Web-App des zweiten Bildschirms. Auf dieser Seite wird die API für die AuthN-App beschrieben.


| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize/{registration code} | AuthN-Modul | 1. Registrierungs-Code </br>    (Pfadkomponente)</br>2.  Antragsteller (obligatorisch)</br>3.  Ressourcenliste (obligatorisch) | GET | XML oder JSON mit einzelnen Entscheidungen vor der Autorisierung oder Fehlerdetails. Siehe Beispiele unten. | 200 - </br></br>400 - Fehlerhafte Anfrage</br></br>401 - Nicht autorisiert</br></br>405 - Methode nicht zulässig </br></br>412 - Voraussetzung fehlgeschlagen</br></br>500 - Interner Server-Fehler |



| Eingabeparameter | Beschreibung |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Registrierungs-Code | Der Wert des Registrierungs-Codes, der vom Benutzer zu Beginn des Authentifizierungsflusses angegeben wurde. |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| Ressourcenliste | Eine Zeichenfolge, die eine kommagetrennte Liste von resourceIds enthält, die den Inhalt identifiziert, auf den ein Benutzer zugreifen kann und der von MVPD-Autorisierungsendpunkten erkannt wird. |


### Beispielantwort {#sample-response}

**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4`
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
    <resource>
        <id>TestStream1</id>
        <authorized>true</authorized>
    </resource>
    <resource>
        <id>TestStream2</id>
        <authorized>false</authorized>  
        <error>
            <status>403</status>
            <code>authorization_denied_by_mvpd</code>
            <message>User not authorized</message>
            <details>Your subscription package does not include the "TestStream3" channel.</details>
            <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
            <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
            <action>none</action>
        </error>
    <resource>
</resources>
```

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
