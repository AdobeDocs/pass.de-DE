---
title: Abrufen einer Liste vorab autorisierter Ressourcen von der Second Screen Web App
description: Abrufen einer Liste vorab autorisierter Ressourcen von der Second Screen Web App
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---

# Abrufen einer Liste vorab autorisierter Ressourcen von der Second Screen Web App {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch den [Drosselmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) begrenzt

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#description}

Anfrage an die Adobe Pass-Authentifizierung zum Abrufen der Liste der bereits autorisierten Ressourcen.

Es gibt zwei Gruppen von APIs: einen Satz für die Streaming-App oder den Programmierer-Dienst und einen Satz für die Second Screen Web App. Auf dieser Seite wird die API für die AuthN-App beschrieben.


| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize/{Registrierungscode} | AuthN-Modul | 1. Registrierungs-Code </br>    (Pfadkomponente)</br>2.  Antragsteller (erforderlich)</br>3.  Ressourcenliste (erforderlich) | GET | XML oder JSON, die einzelne Entscheidungen vor der Autorisierung oder Fehlerdetails enthalten. Siehe Beispiele unten. | 200 - Erfolg</br></br>400 - Ungültige Anfrage</br></br>401 - Nicht autorisiert</br></br>405 - Methode nicht zulässig </br></br>412 - Vorbedingung fehlgeschlagen</br></br>500 - Interner Server-Fehler |



| Eingabeparameter | Beschreibung |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Registrierungscode | Der vom Benutzer am Anfang des Authentifizierungsflusses angegebene Registrierungscode-Wert. |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| Ressourcenliste | Eine Zeichenfolge, die eine kommagetrennte Liste von resourceIds enthält, die den Inhalt identifiziert, auf den ein Benutzer zugreifen kann, und die von MVPD-Autorisierungsendpunkten erkannt wird. |


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
