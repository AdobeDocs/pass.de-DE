---
title: MVPD Content Metadata Exchange
description: MVPD Content Metadata Exchange
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# MVPD Content Metadata Exchange

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#content-metadat-exchange-overview}

Auf dieser Seite werden zwei Standardimplementierungen beschrieben, mit denen die Adobe Pass-Authentifizierung strukturierte Daten bei der Autorisierungsanfrage an MVPDs sendet.  Die strukturierten Daten stellen die Ressource (den Programmierer) dar, die die Anforderung ausführt, und gegebenenfalls zusätzliche Daten wie die Inhaltsbewertung.

Auf der Programmierseite unterstützt die Adobe Pass-Authentifizierung strukturierte MRSS-Datenressourcen wie folgt:

1. Der Programmierer sendet die Ressource als MRSS-Zeichenfolge. Die Adobe Pass-Authentifizierung kodiert sie nicht clientseitig für Web- oder native Geräte. Das MRSS wird als reguläre Zeichenfolge an den Adobe Pass-Authentifizierungsserver gesendet.
1. Auf der Serverseite wird das MRSS anhand des vordefinierten Schemas (http://search.yahoo.com/mrss/) validiert.  Wenn die Validierung erfolgreich ist, extrahiert die Adobe Pass-Authentifizierung die Informationen aus den MRSS-Feldern, einschließlich:
   * Kanaltitel
   * Artikeltitel
   * Ressourcen-ID
   * Bewertungswert und -typ
1. Die aus dem MRSS extrahierten Werte werden verwendet, um die an den MVPD übergebene Genehmigungsanfrage zu erstellen.

Die Adobe Pass-Authentifizierung unterstützt zwei Ansätze zur Übersetzung der MRSS in Formate, die von MVPDs unterstützt werden:

* **XACML**.  Der erste Ansatz entspricht dem OLCA-Standard.  Es verwendet XACML, in dem die MRSS-Werte extrahiert werden, um eine XACMLResource mit Attributen zu erstellen, die den MRSS-Elementen zugeordnet sind.  Dies wird dann an den MVPD übergeben.
* **REST**.  Der zweite Ansatz ist REST-basiert.  Das MRSS ist base64-kodiert und wird als URL-Parameter beim REST-Aufruf übergeben.

In beiden Ansätzen verarbeitet der MVPD die Genehmigungsanfrage, indem die extrahierten Werte in den eigenen logischen Fluss eingefügt werden und eine Autorisierungsantwort zurückgegeben wird.

## Integrationsdetails {#integration-details}

* OLCA-basierte XACML-strukturierte Ressource
* REST-basierte strukturierte Ressource

### OLCA-basierte XACML-strukturierte Ressource {#olca-based-xacml-struc-resource}

Die meisten kabelorientierten MVPDs verwenden den XACML-basierten Ansatz, unterstützen jedoch noch nicht den vollständig strukturierten Datenansatz.  Andere MVPDs, die XACML unterstützen, nehmen den Kanaltitel und akzeptieren dies für das ResourceID-Attribut. Das folgende Beispiel zeigt den vollständig strukturierten XACML-basierten Ansatz. Das Adobe Pass-Authentifizierungsteam empfiehlt, dass für MVPDs, die XACML verwenden, Funktionen wie elterliche Steuerelemente jedoch noch nicht unterstützen, ihre XACML-Integration an das folgende Beispiel angepasst wird:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11=">
    <soap11:Header/>
    <soap11:Body>
        <xacml-samlp:XACMLAuthzDecisionQuery
                xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
                Destination="
                ID="_f1dd34469c5aeac016760e51dbba007d" IssueInstant="2012-06-26T16:30:24.879Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
                https://saml.sp.auth.adobe.com/
            </saml:Issuer>
            <ds:Signature xmlns:ds=">.......</ds:Signature>
            <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                [....info skipped for brevity....]
                <xacml-context:Resource>
 
        // The MRSS item GUID is passed as the XACML Resource resource-id
                    <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
                        <xacml-context:AttributeValue>DISNEY_GUID_12345</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
        // The MRSS channel title is passed as the XACML Resource tv-network
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:tv-network">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // Adobe doesn't yet support an explicit namespace for the GUID, so we reuse the channel title as the GUID.  
        // We expect to add an explicit namespace later next year pulling it from the GUID scheme attribute.
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:id:namespace">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS item title is passed as the XACML Resource content title
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:title">
                        <xacml-context:AttributeValue>Disney Program X</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS media rating is passed as the XACML Resource content rating 
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:rating:vchip">
                        <xacml-context:AttributeValue>TV-Y</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
                </xacml-context:Resource>
 
                <xacml-context:Action>
                    <xacml-context:Attribute>
                        <xacml-context:AttributeValue>VIEW</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
                </xacml-context:Action>
 
                [.....info skipped for brevity....]
            </xacml-context:Request>
        </xacml-samlp:XACMLAuthzDecisionQuery>
    </soap11:Body>
</soap11:Envelope>
 
//formatted for readability
```

### REST-basierte strukturierte Ressource {#rest-based-struct-resource}

Einige MVPDs basieren auf dem folgenden REST-basierten Protokoll zur Autorisierung. Dieser Ansatz ist so umfassend wie der XACML-Ansatz, bietet jedoch eine Implementierung mit &quot;geringerer Gewichtung&quot;.

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
