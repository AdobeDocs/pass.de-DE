---
title: Geschützte Ressourcen
description: Geschützte Ressourcen
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Geschützte Ressourcen {#protected-resources}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Geschützte Ressourcen stellen Inhalte dar, die Benutzer streamen können, und werden durch eindeutige Werte identifiziert, die durch Vereinbarungen zwischen MVPDs und teilnehmenden Programmierern festgelegt werden.

Geschützte Ressourcen folgen einer hierarchischen Baumstruktur, wobei jede Ebene eine höhere Granularität für die Inhaltsautorisierung bietet:

* Netzwerk
   * Kanal
      * Anzeigen
         * Episode
            * Asset

## Geschützte Ressourcenkennungen {#identifiers}

Die eindeutige Kennung der Ressource kann zwei Formate aufweisen:

* Ein einfaches Zeichenfolgenformat wie eine eindeutige Kennung für einen Kanal (eine Marke).
* Ein RSS-Format für Medien (MRSS), das zusätzliche Informationen wie Titel, Bewertungen und Metadaten zur elterlichen Kontrolle enthält.

Bei einer einfachen Ressourcenkennung, wie z. B. „REF30“ (angenommen, dass sie einen Kanal darstellt), kann sie wie folgt in eine RSS-Ressourcenkennung übersetzt werden:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

Bei einer komplexeren Ressourcenkennung kann die RSS-Ressourcenkennung zusätzliche Bewertungsinformationen wie folgt enthalten:

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

## REST-API V2 {#rest-api-v2}

Für die Adobe Pass-Authentifizierungs-APIs zum Abrufen von Vorautorisierungs- und Autorisierungsentscheidungen muss die Client-Anwendung die Kennungen geschützter Ressourcen als Parameter enthalten.

Die Entscheidungen vor Autorisierung und Autorisierung können mit den folgenden APIs abgerufen werden:

* [Abrufen von Entscheidungen vor der Autorisierung mithilfe bestimmter MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Weitere Informationen zum erforderlichen Format für **Bereitstellung der eindeutigen Kennungen geschützter Ressourcen finden Sie in den Abschnitten** Anfrage **und Beispiele** der oben genannten APIs.

Die eindeutigen Kennungen sind für die Adobe Pass-Authentifizierung undurchsichtig, da sie direkt an die MVPD übergeben werden. Wenn MVPD eine Ressourcenkennung nicht erkennen oder analysieren kann, wird ein Fehler an die Adobe Pass-Authentifizierung zurückgegeben, die den Fehler dann mit einem [erweiterten Fehlercode“ an die Client-Anwendung ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

Weitere Informationen zur Integration der oben genannten APIs finden Sie in den folgenden Dokumenten:

* [Grundlegender Vorautorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
