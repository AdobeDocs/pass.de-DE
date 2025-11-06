---
title: Service Provider-Umfang
description: Service Provider-Umfang
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Service Provider-Umfang {#service-provoider-scoping}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Überblick {#overview}

Die Standardimplementierung einer Adobe Pass-Authentifizierungsintegration mit einem MVPD basiert auf der **OLCA-Spezifikation**. Im Abschnitt Authentifizierungsanforderungen der OLCA-Spezifikation (6.5, Subjekt-ID) wird angegeben, dass es möglich ist, den Umfang des Service Providers (SP) für die Subjekt-ID anzugeben. (Die Betreffkennung ist die verschleierte Benutzer-ID, die MVPD an den SP zurückgibt.)  In einer Adobe Pass-Authentifizierungsintegration ist es erforderlich, dass MVPDs den Umfang der SP-Authentifizierungsanfragen aktivieren.

Da die Adobe Pass-Authentifizierung für den Programmierer die Rolle des SP übernimmt, ist es erforderlich, eine Anpassung zu implementieren, die das SP-Scoping der Authentifizierungsanfrage ermöglicht.  Dies muss geschehen, damit der MVPD die Netzwerkmarke identifizieren kann, die in der SAML-Bestätigung an den Identitätsanbieter (IdP) des MVPD übergeben wurde.  Die Berechnung kann auf eine der beiden Arten implementiert werden, die im nächsten Abschnitt beschrieben werden.

## Service Provider-Umfang {#service-provider-scoping}

Die Adobe Pass-Authentifizierung unterstützt die folgenden beiden Möglichkeiten, um die SP-Begrenzung von Authentifizierungsanfragen zu aktivieren:

* **Der SAML-Ausstelleransatz.** Bei diesem Ansatz wird die „Anforderer-ID“ an die SAML-Ausstellerzeichenfolge in der SAML-Authentifizierungsanfrage angehängt.

* **Der Ansatz der benutzerdefinierten Scoping-Eigenschaft.** Bei diesem Ansatz wird die „Anforderer-ID“ explizit als benutzerdefinierte Eigenschaft „Scoping“ in die SAML-Authentifizierungsanfrage aufgenommen.

>[!NOTE]
>
>Die „Anforderer-ID“ bezeichnet die Adobe Pass-Authentifizierung als die Netzwerkmarke des Programmierers (z. B.: „CNN“ ist eine der Marken des Turner-Netzwerks).

### SAML-Ausstelleransatz {#saml-issuer-approach}

Dieser Ansatz verwendet das SAML-`<Issuer>`-Element in der SAML-Authentifizierungsanfrage, wie in diesem Ausschnitt gezeigt:

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Eigenschaftsansatz für benutzerdefinierte Umfangsberechnung {#custom-scoping-property-approach}

Dieser Ansatz verwendet eine benutzerdefinierte Eigenschaft mit dem Namen „Scoping“, wie in diesem Ausschnitt einer SAML-Authentifizierungsanfrage gezeigt:

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
