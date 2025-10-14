---
title: MVPD-Authentifizierung
description: MVPD-Authentifizierung
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# MVPD-Authentifizierung {#mvpd-authn}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#mvpd-authn-overview}

Die eigentliche Dienstanbieterrolle (SP) wird von einem Programmierer verwaltet, aber die Adobe Pass-Authentifizierung dient als SP-Proxy für diesen Programmierer. Durch die Verwendung der Adobe Pass-Authentifizierung als Vermittler können sowohl MVPDs als auch Programmierer vermeiden, ihre Berechtigungsprozesse von Fall zu Fall anpassen zu müssen.

Die folgenden Schritte zeigen die Ereignissequenz unter Verwendung der Adobe Pass-Authentifizierung, wenn ein Programmierer die Authentifizierung von einer MVPD anfordert, die SAML unterstützt. Beachten Sie, dass die Adobe Pass Authentication Access Enabler-Komponente auf dem Client des Benutzers/Abonnenten aktiv ist. Von dort aus erleichtert der Access Enabler alle Schritte des Authentifizierungsflusses.

1. Wenn der Benutzer Zugriff auf geschützte Inhalte anfordert, initiiert Access Enabler die Authentifizierung (AuthN) im Namen des Programmierers (SP).
1. Die App des Serviceprozessors stellt dem Benutzer eine &quot;MVPD-Auswahl“ zur Verfügung, um seinen Pay-TV-Anbieter (MVPD) zu erhalten. Der SP leitet den Browser des Benutzers dann zum ausgewählten Identity Provider-Dienst (IdP) von MVPD weiter.  Dies ist &quot;**Programmer-Initiated Login**.  Der MVPD sendet die Antwort des IdP an den Adobe SAML Assertion Consumer Service, wo er verarbeitet wird.
1. Schließlich leitet der Access Enabler den Browser zurück zur SP-Site, um den SP über den Status (Erfolg/Fehler) der Authentifizierungsanfrage zu informieren.

## Die Authentifizierungsanfrage {#authn-req}

Wie in den obigen Schritten erläutert, muss eine MVPD während des AuthN-Flusses sowohl eine SAML-basierte AuthN-Anfrage akzeptieren als auch eine SAML-AuthN-Antwort senden.

Die [Online Content Access (OLCA) Authentication and Authorization Interface Specification](https://www.cablelabs.com/specifications/search?query=&category=&subcat=&doctype=&content=false&archives=false){target=_blanck} enthält eine standardmäßige AuthN-Anfrage und -Antwort. Während bei der Adobe Pass-Authentifizierung keine MVPDs ihre Berechtigungsnachrichten auf diesem Standard basieren müssen, kann die Überprüfung der Spezifikation Einblicke in die wichtigsten Attribute bieten, die für eine Authentifizierungstransaktion erforderlich sind.

>[!NOTE]
>
>Die AuthN-Anfrage, die eine MVPD mit Adobe Pass-Authentifizierung erhält, enthält eine digitale Signatur. Im folgenden Beispiel wird jedoch aus Gründen der Kürze keine Signatur angezeigt. Ein Beispiel für eine digitale Signatur finden Sie im Beispiel in [Authentifizierungsantwort](#authn-response) in den folgenden Abschnitten.

Beispiel einer SAML-Authentifizierungsanfrage:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

In der folgenden Tabelle werden die Attribute und Tags erläutert, die in einer Authentifizierungsanfrage mit den erwarteten Standardwerten enthalten sein müssen.

**Details zur SAML-Authentifizierungsanfrage**

| sample:AuthRequest | &lt;AuthRequest> , ausgestellt vom Dienstleister für den Identitätsanbieter. |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | Dies ist der Adobe-Endpunkt, der in nachfolgenden Antworten verwendet werden soll. Standardwert: **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| Ziel | Eine URI-Referenz, die die Adresse angibt, an die diese Anfrage gesendet wurde. Dies ist nützlich, um die böswillige Weiterleitung von Anfragen an unbeabsichtigte Empfängerinnen und Empfänger zu verhindern, ein Schutz, der für einige Protokollbindungen erforderlich ist. Wenn sie vorhanden ist, MUSS der tatsächliche Empfänger überprüfen, ob die URI-Referenz den Ort identifiziert, an dem die Nachricht empfangen wurde. Ist dies nicht der Fall, MUSS die Anfrage verworfen werden. Einige Protokollbindungen erfordern möglicherweise die Verwendung dieses Attributs. |
| Auth erzwingen | Das ForceAuth-Attribut, falls mit dem Wert „true“ vorhanden, verpflichtet den Identitätsanbieter, diese Identität neu einzurichten, anstatt sich auf eine vorhandene Sitzung zu verlassen, die er möglicherweise mit dem Prinzipal hat. |
| ID | Eine Kennung für die Anfrage. Siehe [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Abschnitt 1.3.4 für Details. |
| IsPassive | Ein boolescher Wert. Wenn „true“, DÜRFEN der Identitätsanbieter und der Benutzeragent selbst DIE BENUTZEROBERFLÄCHE NICHT SICHTBAR VOM ANFRAGENDEN ÜBERNEHMEN UND MIT DEM PRÄSENTATOR MERKLICH INTERAGIEREN. Wenn kein Wert angegeben wird, ist der Standardwert „false“. |
| Problem-Instant | Der Zeitpunkt der Ausgabe der Antwort. Der Zeitwert wird in UTC codiert, wie in [SAML Core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Abschnitt 1.3.3 beschrieben. |
| protocolBinding | Eine URI-Referenz, die eine SAML-Protokollbindung identifiziert, die beim Zurückgeben der Nachricht &lt;Response> verwendet werden soll. Weitere Informationen [ Protokollbindungen und URI]Verweise, die für sie definiert wurden, finden Sie unter „SAMLBind“. Standardwert : urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST |
| Version | Die Version dieser Anfrage. |
| saml:Issuer | Identifiziert die Entität, die die Antwortnachricht generiert hat. (Weitere Informationen zu diesem Element finden Sie unter SAML core 2.0-os Abschnitt 2.2.5) |
| ds:signature | Eine XML-Signatur, die die Integrität von schützt und den Aussteller der Assertion authentifiziert, wie in Abschnitt 5 in SAML core 2.0-os beschrieben |
| sample:NameIDPolicy | Gibt Einschränkungen für die Namenskennung an, die zur Darstellung des angeforderten Subjekts verwendet werden soll. |
| allowCreate | Ein boolescher Wert, der angibt, ob der Identitätsanbieter bei der Ausführung der Anfrage einen neuen Bezeichner für den Prinzipal erstellen darf. Standard: true |
| Format | Gibt den URI-Verweis an, der einem Namenskennungsformat entspricht. Standard: urn:oasis:names:tc:SAML:2.0:nameid-format:transient Adobe empfiehlt: urn:oasis:names:tc:SAML:2.0:nameid-format:persistent |
| SPNameQualifier | Gibt optional an, dass die Kennung des Assertionssubjekts im Namespace eines anderen Dienstleisters als dem Anforderer zurückgegeben (oder erstellt) werden soll. Standard : http://saml.sp.adobe.adobe.com |

## Die Authentifizierungsantwort {#authn-response}

Nachdem MVPD die Authentifizierungsanfrage empfangen und verarbeitet hat, muss es jetzt eine Authentifizierungsantwort senden.

**Beispiel einer SAML-Authentifizierungsantwort**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


Im obigen Beispiel erwartet der Adobe-SP, die Benutzer-ID aus dem Betreff/der Name-ID abzurufen. Der Adobe-SP kann so konfiguriert werden, dass die Benutzer-ID aus einem benutzerdefinierten Attribut abgerufen wird. Die Antwort sollte ein Element wie das folgende enthalten:

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**Details zur SAML-Authentifizierungsantwort**

| sample:Response | Antwort von Adobe Pass-Authentifizierung empfangen. |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ziel | Eine URI-Referenz, die die Adresse angibt, an die diese Anfrage gesendet wurde. Dies ist nützlich, um die böswillige Weiterleitung von Anfragen an unbeabsichtigte Empfängerinnen und Empfänger zu verhindern, ein Schutz, der für einige Protokollbindungen erforderlich ist. Wenn sie vorhanden ist, MUSS der tatsächliche Empfänger überprüfen, ob die URI-Referenz den Ort identifiziert, an dem die Nachricht empfangen wurde. Ist dies nicht der Fall, MUSS die Anfrage verworfen werden. Einige Protokollbindungen erfordern möglicherweise die Verwendung dieses Attributs. |
| ID | Eine Kennung für die Anfrage. Sie weist den Typ xs:ID auf und MUSS die in Abschnitt 1.3.4 von [SAML core 2.0-os) angegebenen &#x200B;](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} erfüllen, um die Eindeutigkeit der Kennung sicherzustellen. Die Werte des ID-Attributs in einer Anfrage und des InResponseTo-Attributs in der entsprechenden Antwort MÜSSEN übereinstimmen. |
| Antwort an | Die ID einer SAML-Protokollmeldung, als Antwort auf die eine bescheinigende Entität die Bestätigung präsentieren kann. Der Wert muss mit dem Wert im ID-Attribut übereinstimmen, das in der Authentifizierungsanfrage gesendet wurde. Siehe SAML Core 2.0-os |
| Problem-Instant | Der Zeitpunkt der Anfrage. |
| Version | Die Version der Anfrage. |
| saml:Issuer | Identifiziert die Entität, die die Anforderungsnachricht generiert hat. (Weitere Informationen zu diesem Element siehe Abschnitt 2.2.5. SAML Core 2.0-os ) |
| sample:Status | Ein Code, der den Status der entsprechenden Anfrage darstellt. |
| sample:StatusCode | Ein Code, der den Status der als Reaktion auf die entsprechende Anfrage ausgeführten Aktivität darstellt. |
| saml:Assertion | Dieser Typ gibt die grundlegenden Informationen an, die für alle Assertionen gelten. |
| ID | Die Kennung für diese Bestätigung. |
| Version | Die Version dieser Bestätigung. |
| Problem-Instant | Der Zeitpunkt der Anfrage. |
| ds:signature | Eine XML-Signatur, die die Integrität von schützt und den Aussteller der Assertion authentifiziert, wie in Abschnitt 5 in SAML core 2.0-os beschrieben |
| ds:SignedInfo | Die Struktur von SignedInfo umfasst den Kanonisierungsalgorithmus, einen Signaturalgorithmus und einen oder mehrere Verweise. Das SignedInfo-Element kann ein optionales ID-Attribut enthalten, mit dem es von anderen Signaturen und Objekten referenziert werden kann. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:CanonicalizationMethod | CanonicalizationMethod ist ein erforderliches Element, das den Kanonisierungsalgorithmus angibt, der auf das SignedInfo-Element vor der Durchführung von Signaturberechnungen angewendet wird. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:SignatureMethod | SignatureMethod ist ein erforderliches Element, das den für die Signaturgenerierung und -validierung verwendeten Algorithmus angibt. Dieser Algorithmus identifiziert alle kryptografischen Funktionen, die am Signaturvorgang beteiligt sind (z. B. Hashing, Algorithmen für öffentliche Schlüssel, MACs, Padding usw.) Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:reference | Referenz ist ein Element, das ein- oder mehrmals auftreten kann. Er gibt einen Digest-Algorithmus und einen Digest-Wert an, optional eine Kennung des signierten Objekts, den Typ des Objekts und/oder eine Liste der Transformationen, die vor dem Digest angewendet werden sollen. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:transforms | Das optionale Transformationselement enthält eine geordnete Liste von Transformationselementen. Diese beschreiben, wie der Unterzeichner das verwertete Datenobjekt abgerufen hat. Die Ausgabe jeder Transformation dient als Eingabe für die nächste Transformation. Die Eingabe für die erste Transform ist das Ergebnis der Dereferenzierung des URI-Attributs des Referenzelements. Die Ausgabe der letzten Transformation ist die Eingabe für den DigestMethod-Algorithmus. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:DigestMethod | DigestMethod ist ein erforderliches Element, das den Digest-Algorithmus identifiziert, der auf das signierte Objekt angewendet werden soll. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:DigestValue | DigestValue ist ein Element, das den kodierten Wert des Digest enthält. Der Auszug wird immer mit base64 kodiert. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:signatureValue | Das SignatureValue-Element enthält den tatsächlichen Wert der digitalen Signatur. Es wird immer mit base64 codiert. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:KeyInfo | KeyInfo ist ein optionales Element, mit dem der/die Empfänger den Schlüssel erhalten kann/können, der zum Überprüfen der Signatur erforderlich ist. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds:x509data | Ein X509Datenelement in KeyInfo enthält einen oder mehrere Kennungen von Schlüsseln oder X509-Zertifikaten. Siehe Syntax und Verarbeitung von XML-Signaturen |
| ds: X509certificate | Das X509Certificate-Element, das ein base64-kodiertes [X509v3]-Zertifikat enthält |
| saml:Subject | Der Betreff der Anweisung(en) in der Assertion. |
| saml:NameID | Das Element &lt;NameID> ist vom Typ NameIDType (siehe Abschnitt 2.2.2 in SAML core 2.0-os) und wird in verschiedenen SAML-Assertionskonstrukten wie den Elementen &lt;Subject> und &lt;SubjectConfirmation> sowie in verschiedenen Protokollmeldungen verwendet. |
| Format | Eine URI-Referenz, die die Klassifizierung von zeichenfolgenbasierten Kennungsinformationen darstellt. |
| SPNameQualifier | Qualifiziert einen Namen mit dem Namen eines Dienstleisters oder einer Zugehörigkeit von Dienstleistern weiter. Dieses Attribut bietet eine zusätzliche Möglichkeit, Namen auf der Grundlage der vertrauenden Seite oder der vertrauenden Parteien zu verbinden. |
| saml:SubjectConfirmation | Informationen, die eine Bestätigung des Subjekts ermöglichen. Wird mehr als eine Bestätigung des Prüfungsteilnehmers gegeben, so genügt die Erfüllung einer der Bestätigungen, um den Prüfungsteilnehmer zum Zwecke der Anwendung der Bestätigung zu bestätigen. |
| saml:SubjectConfirmationData | Zusätzliche Bestätigungsinformationen, die von einer bestimmten Bestätigungsmethode zu verwenden sind. Der typische Inhalt dieses Elements kann beispielsweise ein <!--<ds:KeyInfo>--> sein, wie in der XML-Signatursyntax und der Verarbeitungsspezifikation definiert |
| NOT ON ODER AFTER | Ein Zeitpunkt, zu dem das Subjekt nicht mehr bestätigt werden kann. |
| Empfängerin oder Empfänger | Ein URI, der die Entität oder den Speicherort angibt, an die bzw. dem eine bescheinigende Entität die Assertion präsentieren kann. Beispielsweise könnte dieses Attribut angeben, dass die Bestätigung an einen bestimmten Netzwerkendpunkt gesendet werden muss, um zu verhindern, dass ein Intermediär sie an einen anderen Ort umleitet. |
| saml:conditions | Das Element &lt;condition> dient als Erweiterungspunkt für neue Bedingungen. |
| Nicht vor | Das NotBefore-Attribut gibt den Zeitpunkt an, zu dem das Gültigkeitsintervall beginnt. |
| saml:AudienceRestriction | Das Element &lt;AudienceRestriction> gibt an, dass die Assertion an eine oder mehrere bestimmte Zielgruppen gerichtet ist, die durch &lt;Audience>-Elemente identifiziert werden. |
| saml:Audience | Eine URI-Referenz, die eine vorgesehene Zielgruppe identifiziert. |
| saml:AuthAnweisung | Eine Authentifizierungsanweisung. |
| AuthInstant | Gibt den Zeitpunkt an, zu dem die Authentifizierung stattgefunden hat. |
| SessionIndex | Gibt den Index einer bestimmten Sitzung zwischen dem vom Subjekt identifizierten Prinzipal und der authentifizierenden Behörde an. |
| saml:AuthContext | Der Kontext, der von der authentifizierenden Behörde bis einschließlich des Authentifizierungsereignisses verwendet wird, das zu dieser Anweisung geführt hat. |
| saml:AuthContextClassRef | Ein URI-Verweis, der eine Authentifizierungskontextklasse identifiziert, die die folgende Authentifizierungskontextdeklaration beschreibt. |
