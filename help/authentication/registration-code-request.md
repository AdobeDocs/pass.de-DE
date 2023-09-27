---
title: Registrierungsseite
description: Registrierungsseite
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 0%

---

# Registrierungsseite {#registration-page}

## REST-API-Endpunkte {#clientless-endpoints}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beschreibung {#create-reg-code-svc}

Gibt zufällig generierten Registrierungscode und Anmeldeseiten-URI zurück.

| Endpunkt | aufgerufen  </br>von | Eingabe   </br>Parameter | HTTP  </br>Methode | Reaktion | HTTP  </br>Reaktion |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestor}/regcode</br>Beispiel:</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Streaming-App</br>oder</br>Programmiererdienst | 1. Antragsteller  </br>    (Pfadkomponente)</br>2.  deviceId (Hash)   </br>    (Obligatorisch)</br>3.  device_info/X-Device-Info (erforderlich)</br>4.  mvpd (optional)</br>5.  ttl (optional)</br>6.  _deviceType_</br> 7.  _deviceUser_ (Veraltet)</br>8.  _appId_ (Veraltet) | POST | XML oder JSON, die einen Registrierungs-Code und Informationen oder Fehlerdetails enthalten, falls diese nicht erfolgreich sind. Siehe Schemata und Beispiele weiter unten. | 201 |

{style="table-layout:auto"}

| Eingabeparameter | Beschreibung |
| --- | --- |
| Anfragender | Die Programmer-Anfrage-ID, für die dieser Vorgang gültig ist. |
| deviceId | Die Geräte-ID-Bytes. |
| device_info/</br>X-Device-Info | Informationen zum Streaming-Gerät.</br>**Hinweis**: Dieser Parameter kann als URL-Parameter an device_info übergeben werden. Aufgrund der potenziellen Größe dieses Parameters und der Längenbeschränkungen einer GET-URL sollte er jedoch als X-Device-Info in der HTTP-Kopfzeile übergeben werden. </br>Weitere Informationen finden Sie unter [Weitergeben von Geräte- und Verbindungsinformationen](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| ttl | Wie lange dieser Regcode in Sekunden leben soll.</br>**Hinweis**: Der maximal zulässige Wert für ttl beträgt 36000 Sekunden (10 Stunden). Höhere Werte führen zu einer 400-HTTP-Antwort (ungültige Anfrage). Wenn `ttl` leer ist, setzt die Adobe Pass-Authentifizierung einen Standardwert von 30 Minuten. |
| _deviceType_ | Der Gerätetyp (z. B. Roku, PC).</br>Wenn dieser Parameter korrekt festgelegt ist, bietet ESM Metriken an, die [aufgeschlüsselt nach Gerätetyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) bei Verwendung von ClientLess, sodass verschiedene Arten der Analyse durchgeführt werden können, z. B. Roku, AppleTV und Xbox.</br>Siehe [Vorteile der Verwendung von Client-losen Gerätetypparametern in Pass-Metriken ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Hinweis**: Der Parameter device_info ersetzt diesen Parameter. |
| _deviceUser_ | Die Benutzer-ID des Geräts. |
| _appId_ | Die Anwendungs-ID/der Name. </br>**Hinweis**: Der Parameter device_info ersetzt diesen. |

{style="table-layout:auto"}


>[!CAUTION]
>
>**IP-Adresse des Streaming-Geräts**
></br>
>Bei Client-zu-Server-Implementierungen wird die IP-Adresse des Streaming-Geräts implizit mit diesem Aufruf gesendet.  Bei Server-zu-Server-Implementierungen, bei denen die Variable **regcode** -Aufruf erfolgt als Programmierer-Dienst und nicht als Streaming-Gerät. Der folgende Header ist erforderlich, um die IP-Adresse des Streaming-Geräts zu übergeben:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>where `<streaming\_device\_ip>` ist die öffentliche IP-Adresse des Streaming-Geräts.
></br></br>
>Beispiel :</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### Antwort-XML-Schema {#xml-schema}


#### Registrierungs-Code XSD {#registration-code-xsd}

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

</br>

| Elementname | Beschreibung |
| --------------- | ------------------------------------------------------------------------------------ |
| id | Vom Registrierungs-Code-Dienst generierte UUID |
| code | Registrierungs-Code, der vom Registrierungs-Code-Dienst generiert wird |
| Anfragender | Anforderer-ID |
| mvpd | Mvpd ID |
| generiert | Zeitstempel der Erstellung des Registrierungs-Codes (in Millisekunden seit dem 1. Januar 1970 GMT) |
| expires | Zeitstempel, wenn der Registrierungs-Code abläuft (in Millisekunden seit dem 1. Januar 1970 GMT) |
| deviceId | Eindeutige Geräte-ID (oder XSTS-Token) |
| deviceType | Gerätetyp |
| deviceUser | Benutzer, der beim Gerät angemeldet ist |
| appId | Anwendungs-ID |
| appVersion | Anwendungsversion |
| registrationURL | URL zur Anmelde-Webanwendung, die dem Endbenutzer angezeigt werden soll |

{style="table-layout:auto"}


</br>

### Fehlermeldung XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### Beispielantwort {#sample-response}

**XML:**

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

**JSON:**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
