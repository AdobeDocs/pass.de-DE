---
title: Überwachen der Adobe Pass-Authentifizierung
description: Überwachen der Adobe Pass-Authentifizierung
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# (Legacy) Überwachen der Adobe Pass-Authentifizierung {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung {#intro}

Kunden können [Nagios](http://www.nagios.org) oder andere Tools verwenden, um zu überprüfen, ob die Adobe Pass-Authentifizierung aktiv oder inaktiv ist.

## Überwachen von Endpunkten {#monitoring-endpoints}

### Endpunkte, die Sie überwachen können {#endpoints-to-monitor}

* Der Konfigurationsendpunkt für alle Plattformen: `https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]`- Er ist entweder über HTTP oder HTTPS verfügbar (je nach der Entscheidung des Entwicklers des Inhaltsanbieters). Wenn dieser Endpunkt fehlt, bedeutet dies, dass Ihre Inhalte nicht auf allen Plattformen und MVPDs verfügbar sein werden. Für die Client-lose REST-API haben wir außerdem den folgenden Endpunkt: `https://api.auth.adobe.com/adobe-services/config your-config-ID]`.

* Die folgenden Endpunkte sind Teil der Web-SDK für die Adobe Pass-Authentifizierung.  Wenn er fehlt, bedeutet dies, dass Pay-TVpass für alle Programmierer und alle Web-Eigenschaften heruntergefahren wird:

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### Endpunkte, die Sie nicht überwachen sollten {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  Es wird immer die Fehlermeldung 503 angezeigt, da für diesen Endpunkt eine MVPD SAML-Antwort erforderlich ist.

* Andere Berechtigungsendpunkte - `adobe-services/1.0/authenticate/`, `adobe-services/1.0/deviceShortAuthorize`, `adobe-services/1.0/authorize`

Sie können diese Endpunkte nicht überwachen, da sie eine Payload für eine relevante Antwort benötigen.
