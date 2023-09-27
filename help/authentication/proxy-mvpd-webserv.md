---
title: Proxy-MVPD-Webdienst
description: Proxy-MVPD-Webdienst
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 0%

---

# Proxy-MVPD-Webdienst {#proxy-mvpd-wbservice}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview-proxy-mvpd-webserv}

Ein &quot;Proxy-MVPD&quot;ist ein MVPD, der neben der Verwaltung seiner eigenen Integration mit der Adobe Pass-Authentifizierung auch den Berechtigungsprozess im Namen einer Gruppe von zugeordneten &quot;Proxied MVPDs&quot;verwaltet. Diese Regelung ist für Programmierer transparent.

Um die ProxyMVPD-Funktion zu implementieren, bietet Adobe Pass Authentication RESTful-Webservices, mit denen ProxyMVPDs Listen von ProxiedMVPDs senden und abrufen können. Das für diese öffentliche API verwendete Protokoll ist REST HTTP mit den folgenden Annahmen:

* Der Proxy-MVPD verwendet die HTTP-GET-Methode, um die Liste der derzeit integrierten MVPDs abzurufen.
* Der Proxy-MVPD verwendet die HTTP-POST-Methode, um die Liste der unterstützten MVPDs zu aktualisieren.

## Proxy-MVPD-Dienste {#proxy-mvpd-services}

* [Proximierte MVPDs abrufen](#retriev-proxied-mvpds)
* [Senden von proximierten MVPDs](#submit-proxied-mvpds)

### Proximierte MVPDs abrufen {#retriev-proxied-mvpds}

Ruft die aktuelle Liste von proxied MVPDs für den ProxyMVPD ab, der durch den apikey -Parameter identifiziert wird.

| Endpunkt | aufgerufen von | Anforderungsheader | HTTP-Methode | HTTP-Antwort |
|---|---|---|---|---|
| &lt;fqdn>/control/v1/proxiedMvpds | ProxyMVPD | apikey (erforderlich) | GET | <ul><li> 200 (ok) - Die Anfrage wurde erfolgreich verarbeitet und die Antwort enthält eine Liste von ProxiedMVPDs im XML-Format</li><li>401 (nicht autorisiert) - Benutzerauthentifizierung erforderlich oder Autorisierung nicht für die angegebenen Anmeldeinformationen erteilt.  Gibt einen der folgenden Werte an:<ul><li>Das apikey-Token ist nicht in der Anfragekopfzeile vorhanden</li><li>Die Anfrage stammt von einer IP-Adresse, die nicht in der Zulassungsliste vorhanden ist</li><li>Das Token ist nicht gültig.</li></ul></li><li>403 (Verboten) - Gibt an, ob der Vorgang für die angegebenen Parameter nicht unterstützt wird oder der Proxy-MVPD nicht als Proxy festgelegt ist oder fehlt</li><li>405 (Methode nicht erlaubt) - Es wurde eine andere HTTP-Methode als GET oder POST verwendet. Entweder wird die HTTP-Methode im Allgemeinen nicht unterstützt oder für diesen spezifischen Endpunkt wird sie nicht unterstützt.</li><li>500 (interner Server-Fehler) - Auf der Serverseite wurde während des Anfrageprozesses ein Fehler ausgelöst.</li></ul> |

Curl-Beispiel:

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`


Beispiel für eine XML-Antwort:

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

### Senden von proximierten MVPDs {#submit-proxied-mvpds}

Pusht ein Array von MVPDs, die in das Proxy-MVPD integriert sind, das durch den apikey-Parameter identifiziert wird.

| Endpunkt | aufgerufen von | Anforderungsheader | HTTP-Methode | HTTP-Antwort |
|:------------------------------:|:---------:|:--------------------------------------------:|:-----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;fqdn>/control/v1/proxiedMvpds | ProxyMVPD | apikey (obligatorisch) proxied-mvpds (erforderlich) | POST | <ul><li>201 (erstellt) - Push-Benachrichtigung wurde erfolgreich verarbeitet</li><li>400 (ungültige Anforderung) - Der Server weiß nicht, wie die Anfrage verarbeitet werden soll:<ul><li>Eingehende XML entspricht nicht dem in dieser Spezifikation veröffentlichten Schema</li><li>Die proximierten mvpds verfügen nicht über eindeutige IDs</li><li>Die gepushten requestorIds sind nicht vorhanden Andere Servlet-Container-Grund für 400-Antwortcode</li></ul><li>401 (nicht autorisiert) - Der apikey ist ungültig oder die aufrufende IP befindet sich nicht auf der Zulassungsliste</li><li>403 (Verboten) - Gibt an, ob der Vorgang für die angegebenen Parameter nicht unterstützt wird oder der Proxy-MVPD nicht als Proxy festgelegt ist oder fehlt</li><li>405 (Methode nicht erlaubt) - Es wurde eine andere HTTP-Methode als GET oder POST verwendet. Entweder wird die HTTP-Methode im Allgemeinen nicht unterstützt oder für diesen spezifischen Endpunkt wird sie nicht unterstützt.</li><li>500 (interner Server-Fehler) - Auf der Serverseite wurde während des Anfrageprozesses ein Fehler ausgelöst.</li></ul> |

Curl-Beispiel:

`curl -X POST -H "apikey: <API_KEY>" "https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



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


### Posting-Häufigkeit {#posting-frequency}

Adobe Pass Authentication empfiehlt, dass ProxyMVPDs ihre Liste von ProxiedMVPDs nur dann per Push übertragen, wenn eine Änderung gegenüber dem vorherigen Push erfolgt.

### Löschen von proximierten MVPDs {#delete-proxied-freqency}

Wenn der ProxyMVPD einen XML-Datensatz mit einer leeren ProxiedMVPDs-Liste pusht, wird diese leere Liste in unserem System wie jede andere Liste gespeichert, wodurch die vorherige Liste effektiv gelöscht wird.



## XSD-Format {#xsd-format}

Adobe hat das folgende akzeptierte Format für das Posten/Abrufen von proximierten MVPDs von/zu unserem öffentlichen Webdienst definiert:

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

**Hinweise zu Elementen:**

* `id` (erforderlich) - Die Proxied MVPD ID muss eine Zeichenfolge sein, die für den Namen des MVPD relevant ist, wobei eines der folgenden Zeichen verwendet wird (da sie Programmierern zu Tracking-Zwecken bereitgestellt wird):
   * Alle alphanumerischen Zeichen, Unterstriche (_) und Bindestriche (&quot;-&quot;).
   * Die idID muss dem folgenden regulären Ausdruck entsprechen:
     `(a-zA-Z0-9((-)|_)*)`

     Daher muss es mindestens ein Zeichen enthalten, mit einem Brief beginnen und mit jedem Buchstaben, jeder Ziffer, einem Bindestrich oder einem Unterstrich fortfahren.

* `iframeSize` (optional) - Das iframeSize-Element ist optional und definiert die Größe des iFrame, wenn sich die MVPD-Authentifizierungsseite in einem iFrame befinden soll. Wenn das iframeSize-Element nicht vorhanden ist, erfolgt die Authentifizierung andernfalls auf einer vollständigen Browser-Umleitungsseite.
* `requestorIds` (optional) - Die Werte der requestorIds werden von Adobe bereitgestellt. Eine Anforderung besteht darin, dass ein proximierter MVPD mit mindestens einer requestorId integriert werden muss. Wenn das &quot;requestorIds&quot;-Tag nicht im proximierten MVPD-Element vorhanden ist, wird dieses proximierte MVPD in alle verfügbaren Anforderer integriert, die unter dem Proxy-MVPD integriert sind.
* `ProviderID` (optional) - Wenn das ProviderID-Attribut im ID-Element vorhanden ist, wird der Wert von ProviderID bei der SAML-Authentifizierungsanfrage als Proxy-MVPD/SubMVPD-ID (anstelle des ID-Werts) an den Proxy-MVPD gesendet. In diesem Fall wird der Wert der ID nur in der auf der Programmier-Seite angezeigten MVPD-Auswahl und intern von der Adobe Pass-Authentifizierung verwendet. Das ProviderID-Attribut muss zwischen 1 und 128 Zeichen lang sein.

## Sicherheit {#security}

Damit ein Antrag als gültig betrachtet werden kann, muss er folgende Regeln beachten:

* Die Anfragekopfzeile muss den Sicherheitspaket-Parameter enthalten. (Dies ist ein Anwendungsschlüssel, der die Aufrufe des Proxy-MVPD eindeutig identifiziert.)
* Die Anfrage muss von einer bestimmten IP-Adresse stammen, die zugelassen wurde.
* Die Anfrage muss über das SSL-Protokoll gesendet werden.

Adobe stellt den (statischen) Wert des Tokens bereit. Dieser Wert wird im Authentifizierungs- und Autorisierungsprozess verwendet.  Alle im Anforderungsheader vorhandenen Parameter, die oben nicht aufgeführt sind, werden ignoriert.

Curl-Beispiel:

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`

## Proxy-MVPD-Webdienst-Endpunkte für die Adobe Pass-Authentifizierungsumgebungen {#proxy-mvpd-wevserv-endpoints}

* **Produktions-URL:** https://mgmt.auth.adobe.com/control/v1/proxiedMvpds
* **Staging-URL:** https://mgmt.auth-staging.adobe.com/control/v1/proxiedMvpds
* **PreQual-Production-URL:** https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds
* **PreQual-Staging-URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
