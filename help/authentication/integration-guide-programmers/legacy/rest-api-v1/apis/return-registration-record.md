---
title: Registrierungseintrag zurückgeben
description: Registrierungseintrag zurückgeben
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 2%

---

# (Legacy) Registrierungseintrag zurückgeben {#return-registration-record}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselungsmechanismus) ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST-API-Endpunkte {#clientless-endpoints}

`<REGGIE_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## Beschreibung {#description}

Gibt einen Registrierungs-Code-Eintrag mit Registrierungs-Code-UUID, Registrierungs-Code und Hash-Geräte-ID zurück.






| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`;/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>Beispiel:<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestId/regcode/TJJCFK?format=xml | Streaming-App</br></br>oder</br></br>Programmierer-Service | 1. </br>    (Pfadkomponente)</br>2.  Registrierungs-Code </br>    (Pfadkomponente) | GET | XML oder JSON mit Registrierungs-Code und Informationen. Siehe Schema und Beispiel unten. | 200 |

{style="table-layout:auto"}




| Eingabeparameter | Beschreibung |
| --- | --- |
| Antragsteller | Die RequestorId des Programmierers, für den dieser Vorgang gültig ist. |
| Registrierungs-Code | Der Registrierungs-Code-Wert, der auf dem Streaming-Gerät angezeigt wird (der in den Authentifizierungsfluss eingegeben werden soll). |




## XML-Antwortschema {#response-xml-schema}

### Registrierungs-Code XSD

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

| Elementname | Beschreibung |
| --- | --- |
| ID | Vom Registrierungs-Code-Service generierte UUID |
| Code | Vom Registrierungs-Code-Service generierter Registrierungs-Code |
| Antragsteller | Antragsteller-ID |
| mvpd | MVPD ID |
| Erzeugt | Zeitstempel der Erstellung des Registrierungs-Codes (in Millisekunden seit dem 1. Januar 1970 GMT) |
| Expires | Zeitstempel, wann der Registrierungscode abläuft (in Millisekunden seit dem 1. Januar 1970 GMT) |
| deviceId | Eindeutige Geräte-ID (oder XSTS-Token) |
| deviceType | Gerätetyp |
| deviceUser | Benutzer hat sich beim Gerät angemeldet |
| appId | Anwendungs-ID |
| appVersion | Anwendungsversion |
| registrationURL | URL zur Anmelde-Web-App, die dem Endbenutzer angezeigt werden soll |

{style="table-layout:auto"}

### Beispielantwort {#sample-response}

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```
