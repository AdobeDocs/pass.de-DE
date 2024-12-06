---
title: Registrierungsdatensatz
description: Registrierungsdatensatz
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 2%

---

# Registrierungsdatensatz {#return-registration-record}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch den [Drosselmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) begrenzt

## REST-API-Endpunkte {#clientless-endpoints}

`<REGGIE_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## Beschreibung {#description}

Gibt den Registrierungs-Code-Datensatz mit der Registrierungs-Code-UUID, dem Registrierungs-Code und der Hash-Geräte-ID zurück.






| Endpunkt | </br>von aufgerufen | Eingabe   </br>Parameter | HTTP </br>Methode | Reaktion | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`;/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>Beispiel:<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestId/regcode/TJJCFK?format=xml | Streaming-App</br></br>oder</br></br>Programmierer-Dienst | 1. Anforderer </br>    (Pfadkomponente)</br>2.  Registrierungs-Code </br>    (Pfadkomponente) | GET | XML oder JSON, die einen Registrierungs-Code und Informationen enthalten. Siehe Schema und Beispiel unten. | 200 |

{style="table-layout:auto"}




| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| Registrierungscode | Der Registrierungscode-Wert, der auf dem Streaming-Gerät angezeigt wird (in den Authentifizierungsfluss einzugeben). |




## Antwort-XML-Schema {#response-xml-schema}

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
| id | Vom Registrierungs-Code-Dienst generierte UUID |
| code | Registrierungs-Code, der vom Registrierungs-Code-Dienst generiert wird |
| Anfragender | Anforderer-ID |
| mvpd | MVPD ID |
| generiert | Zeitstempel der Erstellung des Registrierungs-Codes (in Millisekunden seit dem 1. Januar 1970 GMT) |
| expires | Zeitstempel, wenn der Registrierungscode abläuft (in Millisekunden seit dem 1. Januar 1970 GMT) |
| deviceId | Eindeutige Geräte-ID (oder XSTS-Token) |
| deviceType | Gerätetyp |
| deviceUser | Benutzer, der beim Gerät angemeldet ist |
| appId | Anwendungs-ID |
| appVersion | Anwendungsversion |
| registrationURL | URL zur Anmelde-Webanwendung, die dem Endbenutzer angezeigt werden soll |

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
