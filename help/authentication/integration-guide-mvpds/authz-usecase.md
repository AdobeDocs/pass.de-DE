---
title: MVPD-Autorisierung
description: MVPD-Autorisierung
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# MVPD-Autorisierung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Überblick {#mvpd-authz-overview}

Die Autorisierung (AuthZ) erfolgt über eine Back-Channel-Kommunikation (Server-zu-Server) zwischen einem in Adobe gehosteten Backend-Server und dem MVPD AuthZ-Endpunkt.

Bei AuthZ-Anfragen sollte der Autorisierungsendpunkt mindestens die folgenden Parameter verarbeiten können:

* **UID**. Die vom Authentifizierungsschritt empfangene Benutzer-ID .

* **Ressourcen-ID**. Eine Zeichenfolge, die eine bestimmte Inhaltsressource identifiziert. Diese Ressourcen-ID wird vom Programmierer angegeben, und der MVPD muss die Geschäftsregeln für diese Ressourcen verstärken (z. B. durch Überprüfen, ob der Benutzer einen bestimmten Kanal abonniert hat).

Die Antwort muss nicht nur ermitteln, ob der Benutzer berechtigt ist, sondern auch die Time-to-Live (TTL) dieser Autorisierung enthalten, d. h. den Zeitpunkt, zu dem die Autorisierung abläuft. Wenn die TTL nicht festgelegt ist, schlägt die Authentifizierungsanfrage fehl.  Aus diesem Grund **die TTL eine obligatorische Konfigurationseinstellung auf der Seite der Adobe Pass-Authentifizierung**, um den Fall abzudecken, dass eine MVPD die TTL nicht in ihre Anfrage einbezieht.

## Die Autorisierungsanfrage {#authz-req}

Eine AuthZ-Anfrage muss Folgendes enthalten: ein Subjekt, in dessen Namen die Anfrage erfolgt, die Ressource(n), auf die das Subjekt zuzugreifen versucht, die Aktion, die das Subjekt für die Ressource auszuführen versucht, und die Umgebung, in der der Vorgang demnächst stattfinden wird. Im speziellen Fall der Adobe Pass-Authentifizierung entsprechen diese Elemente:

| XACML-Element | Entspricht |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| Subjekt | Der Prinzipal, der von der authentifizierten Sitzung identifiziert wird und durch den „subject-token“-Attributwert der SAML-Bestätigung referenziert wird. |
| Ressource | Ein URI für die geschützte Ressource. |
| Aktion | ANZEIGEN. |
| Umgebung | Enthält die IP-Adresse des anfragenden Clients, wie vom SP gesehen. |



Der SP muss zu diesem Zeitpunkt eine XACML Authorization DecisionQuery vorbereiten und sie (über HTTP POST) für den IdP an den (zuvor vereinbarten) Policy Decision Point (PDP) senden. Nachfolgend finden Sie ein Beispiel für eine einfache XACML-Anfrage (siehe XACML-Kernspezifikation):

```XML
POST https://authz.site.com/XACML_endpoint
<Request  xmlns="urn:oasis:names:tc:xacm:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os
http://docs.oasis-open.org/xacml/access_control-xacml-2.0-context-schema-os.xsd">
<Subject>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-token"
        DataType="http://www.w3.org/2001/XMLSchema#base64Binary">
      <AttributeValue>{Base64 Data}</AttributeValue>
   </Attribute>
</Subject>
<Resource>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
        DataType="http://www.w3.org/2001/XMLSchema#anyURI">
<AttributeValue>urn:tve:tms:1234</AttributeValue>
   </Attribute>
</Resource>
<Action>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
        DataType="http://www.w3.org/2001/XMLSchema#string">
       <AttributeValue>VIEW</AttributeValue>
   </Attribute>
</Action>
<Environment>
   <Attribute
       AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
       DataType="http://www.w3.org/2001/XMLSchema#string">
      <AttributeValue>1.2.3.4</AttributeValue>
   </Attribute>
</Environment>
</Request>
```


Nach Erhalt der AuthZ-Anfrage bewertet der PDP von MVPD die Anfrage und bestimmt, ob dem Subjekt erlaubt werden soll, die angeforderte Aktion für die Ressource durchzuführen. Die MVPD gibt dann eine Antwort mit einer Entscheidung, einem Status-Code und einer Nachricht zurück, wie in der Autorisierungsantwort unten beschrieben.

## Die Autorisierungsantwort {#authz-response}

Die Antwort auf die AuthZ-Anfrage erfolgt, nachdem der MVPD die Anfrage ausgewertet hat und die angeforderten Geschäftsregeln anwendet, um zu bestimmen, ob der Betreffende die angeforderte Aktion für die Ressource ausführen darf. Die zurückgegebene Antwort auf die Adobe Pass-Authentifizierung wird nach der XACML-Kernspezifikation erneut mit einer Entscheidung, einem Status-Code, einer Nachricht und Verpflichtungen ausgedrückt, die der SPA als Richtliniendurchsetzungspunkt (Policy Enforcement Point, PEP) hat. Im Folgenden finden Sie eine Beispielantwort:

```XML
<Response xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os">
  <Result>
  <Decision>Permit</Decision>
  <Status>
     <StatusCode Value="urn:oasis:names:tc:xacml:1.0:status:ok"/>
     <StatusMessage>ok</StatusMessage>
  </Status>
  <xacml:Obligations     
          xmlns:xacml="urn:oasis:names:tc:xacml:2.0:policy:schema:os">
     <xacml:Obligation    
              ObligationId="urn:cablelabs:olca:1.0:obligations:log"
              FulfillOn="Permit" />
  </xacml:Obligations>
 </Result>
</Response>
```

Im Folgenden finden Sie eine Liste der Ablehnungsverpflichtungen, die von der Adobe Pass-Authentifizierung unterstützt werden und es Programmierern ermöglichen, Folgendes zu erfüllen:

* **urn:tve:xacml:2.0:obligations:restrictions-pc** - Der Abonnent hat eine elterliche Kontrollprüfung nicht bestanden und der SP muss geeignete Maßnahmen ergreifen, um den Zugriff auf diesen Inhalt zu beschränken.

* **urn:tve:xacml:2.0:obligations:upgrade** - Der Abonnent hat keine geeignete Abonnementebene.  Abonnement muss aktualisiert werden, um auf Inhalte zugreifen zu können.

Die Adobe Pass-Authentifizierung unterstützt die folgenden **ERLAUBT**-Verpflichtungen und ermöglicht es Programmierern, sie zu erfüllen:

* **urn:cablelabs:olca:1.0:obligations:log** - Adobe Pass protokolliert die Transaktion und kann sie über den vereinbarten Berichtsmechanismus bereitstellen.

* **urn:cablelabs:olca:1.0:obligations:re-authz** - Die Adobe Pass-Authentifizierung aktualisiert die Autorisierung in n Sekunden erneut (angegeben als Argument für die Obligation über eine XACML-Attributzuweisung - siehe XACML-Kernspezifikation , Abschnitt 5.46).

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
