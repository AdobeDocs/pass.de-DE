---
title: MVPD Preflight-Autorisierung
description: MVPD Preflight-Autorisierung
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# MVPD Preflight-Autorisierung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#mvpd-preflight-authz-intro}

&quot;Preflight authorization&quot; ist eine einfache Autorisierungsüberprüfung für mehrere Ressourcen. Programmierer verwenden sie hauptsächlich zum Dekorieren ihrer Benutzeroberflächen (z. B. zur Angabe des Zugriffstatus mit Schloss- und Entsperrungssymbolen).

Die Adobe Pass-Authentifizierung kann die Preflight-Autorisierung derzeit auf zwei Arten für MVPDs unterstützen, entweder über AuthN-Antwortattribute oder über eine mehrkanalige AuthZ-Anfrage.  In den folgenden Szenarien werden die Kosten/Nutzen der verschiedenen Methoden beschrieben, mit denen Sie die Vorabgenehmigung implementieren können:

* **Best-Case-Szenario** - Der MVPD stellt die Liste der bereits autorisierten Ressourcen während der Autorisierungsphase (Multi-Channel AuthZ) zur Verfügung.
* **Szenario mit dem schlimmsten Fall** - Wenn ein MVPD keine Mehrfachressourcenautorisierung unterstützt, führt der Adobe Pass-Authentifizierungsserver einen Autorisierungsaufruf an den MVPD für jede Ressource in der Ressourcenliste durch. Dieses Szenario wirkt sich (proportional zur Anzahl der Ressourcen) auf die Reaktionszeit für die Vorabgenehmigung aus. Dies kann die Auslastung auf beiden Adobe- und MVPD-Servern erhöhen und zu Leistungsproblemen führen. Außerdem werden Autorisierungsanfragen/Antwortereignisse generiert, ohne dass eine Wiedergabe tatsächlich erforderlich ist.
* **Veraltet** - Der MVPD stellt während der Authentifizierungsphase die Liste der bereits autorisierten Ressourcen bereit, sodass keine Netzwerkaufrufe erforderlich sind, nicht einmal die Preflight-Anfrage, da die Liste auf dem Client zwischengespeichert wird.

Während MVPDs keine Preflight-Autorisierung unterstützen müssen, werden in den folgenden Abschnitten einige Vorabgenehmigungsverfahren beschrieben, die von der Adobe Pass-Authentifizierung unterstützt werden können, bevor auf das oben genannte Worst-Case-Szenario zurückgegriffen wird.

## Preflight in AuthN {#preflight-authn}

Dieses Preflight-Szenario ist OLCA-kompatibel (Cableabs). In Abschnitt 7.5.2 der Spezifikation für die Authentifizierungs- und Autorisierungsschnittstelle 1.0 mit dem Titel &quot;Attribute Statement Within Authentication Assertion&quot;wird beschrieben, wie eine SAML-Authentifizierungsantwort eine Liste vorab autorisierter Ressourcen enthalten kann. Wenn ein IdP dies unterstützt, kann der Adobe Pass-Authentifizierungsserver zum Zeitpunkt der Authentifizierung die vorausgefüllte Ressourcenliste generieren und sie zusammen mit dem Authentifizierungstoken auf dem Client zwischenspeichern. Diese Methode erzielt auch das beste Fallszenario. Wenn der Programmierer checkPreauthorizedResources() aufruft, werden keine Netzwerkaufrufe durchgeführt, da sich alles bereits auf dem Client befindet.

### Benutzerdefinierte Ressourcenliste in SAML-Attributanweisung {#custom-res-saml-attr}

Die SAML-Authentifizierungsantwort des IdP muss eine AttributeStatement mit Ressourcennamen enthalten, die von AdobePass autorisiert werden sollen.  Einige MVPDs bieten dies im folgenden Format:

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

Das obige Beispiel enthält eine Liste mit zwei vorab autorisierten Ressourcen: &quot;MMOD&quot;und &quot;Olympic2012&quot;.

Dadurch wird das beste Fallszenario erreicht, und es werden keine Netzwerkaufrufe durchgeführt, wenn der Programmierer checkPreauthorizedResources() aufruft, da alles bereits auf dem Client ist.

## Mehrkanal-Preflight in AuthZ {#preflight-multich-authz}

Diese Preflight-Implementierung ist auch OLCA-kompatibel (Cablelabs).  In der Spezifikation für die Authentifizierungs- und Autorisierungsschnittstelle 1.0 (Abschnitte 7.5.3 und 7.5.4) werden Methoden beschrieben, mit denen Autorisierungsinformationen von einem MVPD entweder über SAML-Zusicherungen oder XACML angefordert werden. Dies ist die empfohlene Methode zur Abfrage des Autorisierungsstatus für MVPDs, die dies im Rahmen des Authentifizierungsflusses nicht unterstützen. Adobe Pass Authentication sendet einen einzigen Netzwerkaufruf an den MVPD, um die Liste der autorisierten Ressourcen abzurufen.


Die Adobe Pass-Authentifizierung erhält die Liste der Ressourcen von der Anwendung des Programmierers. Die MVPD-Integration der Adobe Pass-Authentifizierung kann dann einen AuthZ-Aufruf ausführen, der alle diese Ressourcen enthält, dann die Antwort analysieren und die verschiedenen Entscheidungen über Genehmigungen/Ablehnungen extrahieren.  Der Fluss für das Preflight-Szenario mit einem mehrkanaligen AuthZ-Szenario funktioniert wie folgt:

1. Die App des Programmierers sendet eine kommagetrennte Liste von Ressourcen über die Preflight-Client-API, z. B. &quot;TestChannel1,TestChannel2,TestChannel3&quot;.
1. Der MVPD Preflight AuthZ-Anfrageaufruf enthält die verschiedenen Ressourcen und weist die folgende Struktur auf:

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## Benutzerdefinierte Autorisierung für mehrere Ressourcen {#custom-authz}

Einige MVPDs verfügen über Autorisierungsendpunkte, die die Autorisierung mehrerer Ressourcen in einer Anfrage unterstützen, aber nicht unter das in Multi-Channel-AuthZ beschriebene Szenario fallen. Diese spezifischen MVPDs erfordern benutzerdefinierte Arbeit.

Adobe kann auch die Autorisierung mehrerer Kanäle ohne Änderungen an der bestehenden Implementierung unterstützen.  Dieser Ansatz muss zwischen der Adobe und dem technischen MVPD-Team überprüft werden, um sicherzustellen, dass er erwartungsgemäß funktioniert.

## MVPDs, die die Preflight-Autorisierung unterstützen {#mvpds-supp-preflight-authz}

In der folgenden Tabelle sind die MVPDs aufgeführt, die die Preflight-Autorisierung unterstützen, zusammen mit dem von ihnen unterstützten Preflight-Typ und bekannten Einschränkungen:

| Preflight-Ansatz | MVPD | Hinweise |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| Multi-Channel-AuthZ | Comcast AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| Kanalverbindung in Benutzermetadaten | Sudenlink HTC | Alle direkten Synacor-Integrationen können diesen Ansatz ebenfalls unterstützen. |
| Verzweigung und Beitritt | Alle anderen, oben nicht aufgeführten | Standardmäßig ist die maximale Anzahl der aktivierten Ressourcen = 5. |

<!--
![RelatedInformation]
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Authorization](/help/authentication/authz-usecase.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
>* [MVPD User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Preflight Authorization - Programmer Integration Guide](/help/authentication/preflight-authz.md)
>* [AuthN and AuthZ Interface 1.0 Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank} 
-->
