---
title: Umfang des Dienstleisters
description: Umfang des Dienstleisters
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# Umfang des Dienstleisters {#service-provoider-scoping}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Die Standardimplementierung einer Adobe Pass-Authentifizierungsintegration mit einem MVPD basiert auf der **OLCA-Spezifikation**. Im Abschnitt Authentifizierungsanforderungen der OLCA-Spezifikation (6.5, Subject Identifier) wird angegeben, dass der Umfang des Service Providers (SP) für die Subject Identifier angegeben werden kann. (Die Betreffkennung ist die verschleierte Benutzer-ID, die der MVPD an die SP zurückgibt.)  In einer Adobe Pass-Authentifizierungsintegration ist es erforderlich, dass MVPDs das Scoping der SP-Authentifizierungsanforderungen aktivieren.

Da die Adobe Pass-Authentifizierung die Rolle von SP für den Programmierer übernimmt, ist es erforderlich, eine Anpassung zu implementieren, die das SP-Scoping der Authentifizierungsanforderung ermöglicht.  Dies muss geschehen, damit der MVPD die Netzwerkmarke identifizieren kann, die in der SAML-Assertion an den Identitäts-Provider (IdP) des MVPD übergeben wurde.  Scoping kann auf eine der beiden im nächsten Abschnitt beschriebenen Arten implementiert werden.

## Umfang des Dienstleisters {#service-provider-scoping}

Die Adobe Pass-Authentifizierung unterstützt die folgenden beiden Möglichkeiten, das SP-Scoping von Authentifizierungsanforderungen zu aktivieren:

* **Der SAML-Ausstelleransatz.**  Bei diesem Ansatz wird die &quot;Anforderer-ID&quot;in der SAML-Authentifizierungsanforderung an die Zeichenfolge SAML-Aussteller angehängt.

* **Der Ansatz der benutzerdefinierten Scoping-Eigenschaft.**  Bei diesem Ansatz ist die &quot;Anforderer-ID&quot;explizit als benutzerdefinierte Eigenschaft &quot;Scoping&quot;in der SAML-Authentifizierungsanforderung enthalten.

>[!NOTE]
>
>Die &quot;Anforderer-ID&quot;bezieht sich auf die Netzwerkmarke des Programmierers (z. B.: &quot;CNN&quot;ist eine der Marken des Turner-Netzwerks).

### SAML-Ausstellungsansatz {#saml-issuer-approach}

Bei diesem Ansatz wird SAML verwendet `<Issuer>` -Element in der SAML-Authentifizierungsanforderung ein, wie in diesem Snippet gezeigt:

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Eigene Scoping-Eigenschaftsansatz {#custom-scoping-property-approach}

Dieser Ansatz verwendet eine benutzerdefinierte Eigenschaft mit dem Namen &quot;Scoping&quot;, wie in diesem Ausschnitt einer SAML-Authentifizierungsanforderung dargestellt:

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
