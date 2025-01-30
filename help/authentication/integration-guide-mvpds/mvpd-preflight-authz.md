---
title: MVPD Preflight-Autorisierung
description: MVPD Preflight-Autorisierung
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# MVPD Preflight-Autorisierung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Einführung {#mvpd-preflight-authz-intro}

„PreFlight-Autorisierung“ ist eine einfache Autorisierungsprüfung für mehrere Ressourcen. Programmierer verwenden sie hauptsächlich zum Dekorieren ihrer Benutzeroberflächen (z. B. zum Anzeigen des Zugriffsstatus mit Sperren- und Entsperren-Symbolen).

Die Adobe Pass-Authentifizierung kann die PreFlight-Autorisierung für MVPDs derzeit auf zwei Arten unterstützen, entweder über AuthN-Antwortattribute oder über eine mehrkanalige AuthZ-Anfrage.  In den folgenden Szenarien werden die Kosten/Nutzen der verschiedenen Möglichkeiten beschrieben, wie Sie die Vorabautorisierung implementieren können:

* **Best-Case**-Szenario: MVPD stellt die Liste der vorab autorisierten Ressourcen während der Autorisierungsphase bereit (Multi-Channel-AuthZ).
* **Worst-Case-**: Wenn eine MVPD keine Autorisierung für mehrere Ressourcen unterstützt, führt der Adobe Pass-Authentifizierungsserver für jede Ressource in der Ressourcenliste einen Autorisierungsaufruf an die MVPD durch. Dieses Szenario wirkt sich (proportional zur Anzahl der Ressourcen) auf die Antwortzeit für die Autorisierungsanfrage vor dem Flug aus. Dies kann die Auslastung der Adobe- und MVPD-Server erhöhen und zu Leistungsproblemen führen. Außerdem werden damit Autorisierungsanfragen/Antworten-Ereignisse generiert, ohne dass ein Abspielen tatsächlich erforderlich ist.
* **Veraltet** - MVPD stellt die Liste der vorab autorisierten Ressourcen während der Authentifizierungsphase bereit, sodass keine Netzwerkaufrufe erforderlich sind, auch nicht die Preflight-Anfrage, da die Liste auf dem Client zwischengespeichert wird.

Während MVPDs keine PreFlight-Autorisierung unterstützen müssen, werden in den folgenden Abschnitten einige PreFlight-Autorisierungsmethoden beschrieben, die die Adobe Pass-Authentifizierung unterstützen kann, bevor auf das obige Worst-Case-Szenario zurückgegriffen wird.

## PreFlight in AuthN {#preflight-authn}

Dieses Preflight-Szenario ist OLCA-kompatibel (Kabel). In Abschnitt 7.5.2 der Spezifikation von Authentication and Authorization Interface 1.0 mit dem Titel „Attribute Statement Within Authentication Assertion“ wird beschrieben, wie eine SAML-Authentifizierungsantwort eine Liste vorautorisierter Ressourcen enthalten kann. Wenn ein IdP dies unterstützt, kann der Adobe Pass-Authentifizierungsserver die Liste der im Voraus ausgewählten Ressourcen zur Authentifizierungszeit generieren und sie zusammen mit dem Authentifizierungstoken auf dem Client zwischenspeichern. Mit dieser Methode wird auch das Szenario des besten Falls erzielt, und beim Aufrufen von checkPreauthorizedResources() durch den Programmierer werden keine Netzwerkaufrufe ausgeführt, da sich bereits alles auf dem Client befindet.

### Benutzerdefinierte Ressourcenliste in SAML-Attributanweisung {#custom-res-saml-attr}

Die SAML-Authentifizierungsantwort des IdP muss eine AttributeStatement enthalten, die Ressourcennamen enthält, die AdobePass autorisieren sollte.  Einige MVPDs bieten dies im folgenden Format:

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

Das obige Beispiel enthält eine Liste mit zwei vorab autorisierten Ressourcen: „MMOD“ und „Olympic2012“.

Dadurch wird effektiv das Szenario des besten Falls erreicht, und beim Aufrufen von checkPreauthorizedResources() durch den Programmierer werden keine Netzwerkaufrufe ausgeführt, da sich bereits alles auf dem Client befindet.

## Multi-Channel Preflight in AuthZ {#preflight-multich-authz}

Diese Preflight-Implementierung ist auch mit OLCA (CableLabs) kompatibel.  In der Spezifikation der Authentifizierungs- und Autorisierungsschnittstelle 1.0 (Abschnitte 7.5.3 und 7.5.4) werden Methoden zum Anfordern von Autorisierungsinformationen von einem MVPD mithilfe von SAML-Assertionen oder XACML beschrieben. Dies ist die empfohlene Methode, um den Autorisierungsstatus für MVPDs abzufragen, die dies nicht als Teil des Authentifizierungsflusses unterstützen. Die Adobe Pass-Authentifizierung führt einen einzigen Netzwerkaufruf an die MVPD aus, um die Liste der autorisierten Ressourcen abzurufen.


Die Adobe Pass-Authentifizierung erhält die Liste der Ressourcen vom Programm des Programmierers. Die MVPD-Integration der Adobe Pass-Authentifizierung kann dann einen AuthZ-Aufruf mit allen diesen Ressourcen durchführen, die Antwort analysieren und die mehreren Entscheidungen über die Genehmigung/Ablehnung extrahieren.  Der Fluss für das Preflight-mit-Multi-Channel-AuthZ-Szenario funktioniert wie folgt:

1. Die -App des Programmierers sendet über die Preflight-Client-API eine kommagetrennte Liste von Ressourcen, z. B.: „TestChannel1,TestChannel2,TestChannel3“.
1. Der MVPD Preflight AuthZ-Anforderungsaufruf enthält die mehreren Ressourcen und weist die folgende Struktur auf:

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

Einige MVPDs haben Autorisierungsendpunkte, die die Autorisierung für mehrere Ressourcen in einer Anfrage unterstützen, aber sie fallen nicht unter das in Multi-Channel-AuthZ beschriebene Szenario. Diese spezifischen MVPDs erfordern benutzerdefinierte Arbeit.

Adobe kann auch die Autorisierung über mehrere Kanäle unterstützen, ohne die bestehende Implementierung zu ändern.  Dieser Ansatz muss zwischen Adobe und dem technischen Team von MVPD überprüft werden, um sicherzustellen, dass er wie erwartet funktioniert.

## MVPDs, die die Preflight-Autorisierung unterstützen {#mvpds-supp-preflight-authz}

In der folgenden Tabelle sind die MVPDs aufgeführt, die die Preflight-Autorisierung unterstützen, zusammen mit dem Typ des von ihnen unterstützten Preflight und bekannten Einschränkungen:

| Vorfluganflug | MVPD | Notizen |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| Multi-Channel-Authentifizierung | Comcast AT&amp;T Proxy ClearLeap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| Kanalaufstellung in Benutzermetadaten | SuddenLink HTC | Alle Synacor Direct-Integrationen können diesen Ansatz ebenfalls unterstützen. |
| Verzweigung und Verbindung | Alle anderen nicht oben aufgeführt | Die standardmäßige maximale Anzahl von überprüften Ressourcen = 5. |

