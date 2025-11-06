---
title: Proxy-Webdienst von MVPD
description: Proxy-Webdienst von MVPD
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---


# Proxy-Webdienst von MVPD {#proxy-mvpd-wbservice}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Bevor Sie den Proxy-MVPD-Webdienst verwenden, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:
>
> * Rufen Sie die Client-Anmeldeinformationen ab, wie in der API[Dokumentation zum Abrufen von Client](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)Anmeldeinformationen beschrieben.
> * Rufen Sie das Zugriffstoken ab, wie in der API[Dokumentation zum Abrufen des Zugriffstokens &#x200B;](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Weitere Informationen zum Erstellen einer registrierten [&#x200B; und zum Herunterladen der Software-Anweisung finden &#x200B;](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) in der Dokumentation Übersicht zur Dynamic Client-Registrierung .

## Überblick {#overview-proxy-mvpd-webserv}

Ein „Proxy-MVPD&quot; ist ein MVPD, der nicht nur seine eigene Integration mit der Adobe Pass-Authentifizierung verwaltet, sondern auch den Berechtigungsprozess im Namen einer Gruppe verknüpfter „Proxy-MVPDs“. Diese Anordnung ist für Programmierer transparent.

Um die ProxyMVPD-Funktion zu implementieren, stellt die Adobe Pass-Authentifizierung RESTful-Web-Services bereit, mit denen ProxyMVPDs Listen von ProxyMVPDs senden und abrufen können. Das für diese öffentliche API verwendete Protokoll ist REST-HTTP mit den folgenden Annahmen:

&#x200B;- Die Proxy-MVPD verwendet die HTTP-GET-Methode zum Abrufen der Liste der aktuellen integrierten MVPDs.
&#x200B;- Die Proxy-MVPD verwendet die HTTP-POST-Methode, um die Liste der unterstützten MVPDs zu aktualisieren.

## Proxy-MVPD-Services {#proxy-mvpd-services}

&#x200B;- [Proxys abrufen](#retriev-proxied-mvpds)
&#x200B;- [Proxy-MVPDs senden](#submit-proxied-mvpds)

### Proxy-MVPDs abrufen {#retriev-proxied-mvpds}

Ruft die aktuelle Liste der Proxy-MVPDs ab, die in den identifizierten Proxy-MVPD integriert sind.

| Endpunkt | Aufgerufen von | Anfrageparameter | Anfrage-Header | HTTP-Methode | HTTP-Antwort |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorisierung (obligatorisch) | GET | <ul><li> 200 (OK) - Die Anfrage wurde erfolgreich verarbeitet und die Antwort enthält eine Liste von ProxyMVPDs im XML-Format</li><li>401 (Nicht autorisiert) - Zeigt eine der folgenden Möglichkeiten an:<ul><li>Der Client MUSS ein neues Zugriffstoken anfordern</li><li>Die Anfrage stammt von einer IP-Adresse, die nicht in der Zulassungsliste vorhanden ist</li><li>Das Token ist ungültig</li></ul></li><li>403 (Verboten) - Gibt entweder an, dass der Vorgang für die angegebenen Parameter nicht unterstützt wird oder der Proxy-MVPD nicht als Proxy festgelegt ist oder fehlt</li><li>405 (Methode nicht zulässig) - Es wurde eine andere HTTP-Methode als GET oder POST verwendet. Entweder wird die HTTP-Methode im Allgemeinen nicht unterstützt oder wird für diesen spezifischen Endpunkt nicht unterstützt.</li><li>500 (Interner Server-Fehler) - Server-seitig wurde während des Anfragevorgangs ein Fehler ausgelöst.</li></ul> |

cURL-Beispiel:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


Beispiel einer XML-Antwort:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### Proxys für MVPDs senden {#submit-proxied-mvpds}

Überträgt ein Array von MVPDs, die in den identifizierten Proxy-MVPD integriert sind.

| Endpunkt | Aufgerufen von | Anfrageparameter | Anfrage-Header | HTTP-Methode | HTTP-Antwort |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Authorization (mandatory) proxy-mvpds (mandatory) | POST | <ul><li>201 (Erstellt) - Die Push-Benachrichtigung wurde erfolgreich verarbeitet</li><li>400 (fehlerhafte Anfrage) - Der Server weiß nicht, wie die Anfrage verarbeitet werden soll:<ul><li>Eingehende XML-Daten entsprechen nicht dem in dieser Spezifikation veröffentlichten Schema</li><li>Die MVPDs der Proxys haben keine eindeutigen IDs</li><li>Die gepushten RequestorIds existieren nicht. Anderer Servlet-Container-Grund für den 400-Antwort-Code</li></ul><li>401 (Nicht autorisiert) - Zeigt eine der folgenden Möglichkeiten an:<ul><li>Der Client MUSS ein neues Zugriffstoken anfordern</li><li>Die Anfrage stammt von einer IP-Adresse, die nicht in der Zulassungsliste vorhanden ist</li><li>Das Token ist ungültig</li></ul></li><li>403 (Verboten) - Gibt entweder an, dass der Vorgang für die angegebenen Parameter nicht unterstützt wird oder der Proxy-MVPD nicht als Proxy festgelegt ist oder fehlt</li><li>405 (Methode nicht zulässig) - Es wurde eine andere HTTP-Methode als GET oder POST verwendet. Entweder wird die HTTP-Methode im Allgemeinen nicht unterstützt oder wird für diesen spezifischen Endpunkt nicht unterstützt.</li><li>500 (Interner Server-Fehler) - Server-seitig wurde während des Anfragevorgangs ein Fehler ausgelöst.</li></ul> |

cURL-Beispiel:

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



XML-Beispiel:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### Veröffentlichungshäufigkeit {#posting-frequency}

Die Adobe Pass-Authentifizierung empfiehlt, dass ProxyMVPDs ihre Liste von ProxyMVPDs nur bei einer Änderung gegenüber dem vorherigen Push pushen.

### Proxy-MVPDs löschen {#delete-proxied-freqency}

Wenn der ProxyMVPD einen XML-Eintrag mit einer leeren ProxyMVPD-Liste überträgt, wird diese leere Liste wie jede andere Liste in unserem System gespeichert und die vorherige Liste wird somit effektiv gelöscht.



## XSD Format {#xsd-format}

Adobe hat das folgende akzeptierte Format für das Posten/Abrufen von Proxy-MVPDs von/zu unserem öffentlichen Web-Service definiert:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**Anmerkungen zu Elementen:**

&#x200B;- `id` (obligatorisch) - Die MVPD-ID des Proxys muss eine Zeichenfolge sein, die für den Namen der MVPD relevant ist, wobei eines der folgenden Zeichen verwendet wird (da sie für Programmierer zu Tracking-Zwecken verfügbar gemacht wird):
&#x200B;- Alle alphanumerischen Zeichen, Unterstriche („_„) und Bindestriche (“-„).
&#x200B;- Die ID muss dem folgenden regulären Ausdruck entsprechen:
`(a-zA-Z0-9((-)|_)*)`

    Daher muss sie mindestens ein Zeichen aufweisen, mit einem Buchstaben beginnen und mit einem Buchstaben, einer Ziffer, einem Bindestrich oder einem Unterstrich fortfahren.

&#x200B;- `iframeSize` (optional) - Das iframeSize-Element ist optional und definiert die Größe des iFrames, wenn sich die MVPD-Authentifizierungsseite in einem iFrame befinden soll. Wenn andernfalls das iframeSize-Element nicht vorhanden ist, erfolgt die Authentifizierung auf einer vollständigen Browser-Umleitungsseite.
&#x200B;- `requestorIds` (optional) - Die Werte für die RequestorIds werden von Adobe bereitgestellt. Eine Anforderung besteht darin, dass ein Proxy-MVPD mit mindestens einer RequestorId integriert werden muss. Wenn das Tag „RequestorIds“ nicht auf dem MVPD-Proxyelement vorhanden ist, wird dieser MVPD mit allen verfügbaren Anforderern integriert, die unter dem Proxy-MVPD integriert sind.
&#x200B;- `ProviderID` (optional) - Wenn das ProviderID-Attribut im id-Element vorhanden ist, wird der Wert von ProviderID bei der SAML-Authentifizierungsanfrage an die Proxy-MVPD als Proxy-MVPD-/SubMVPD-ID gesendet (anstelle des ID-Werts). In diesem Fall wird der Wert der ID nur in der MVPD-Auswahl verwendet, die auf der Programmiererseite angezeigt wird, und intern von der Adobe Pass-Authentifizierung. Das ProviderID-Attribut muss zwischen 1 und 128 Zeichen lang sein.

## Sicherheit {#security}

Damit ein Antrag als gültig betrachtet werden kann, muss er die folgenden Regeln einhalten:

&#x200B;- Der Anfrage-Header muss das Sicherheits-OAuth2-Zugriffstoken enthalten, das abgerufen wurde, wie in der API-Dokumentation [Zugriffstoken abrufen](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben.
&#x200B;- Die Anfrage muss von einer bestimmten IP-Adresse stammen, die zulässig ist.
&#x200B;- Die Anfrage muss über das SSL-Protokoll gesendet werden.

Alle im Anfrage-Header vorhandenen Parameter, die oben nicht aufgeführt sind, werden ignoriert.

cURL-Beispiel:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/<proxy-mvpd-identifier>/mvpds"`

## Proxy-Endpunkte für den MVPD-Webservice für die Adobe Pass-Authentifizierungsumgebungen {#proxy-mvpd-wevserv-endpoints}

&#x200B;- **Produktions-URL:** https://mgmt.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **Staging-URL:** https://mgmt.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **PreQual-Production URL:** https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **PreQual-Staging URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
