---
title: Identifizieren geschützter Ressourcen
description: Identifizieren geschützter Ressourcen
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Identifizieren geschützter Ressourcen {#identifying-protected-resources}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#overview}

Jede Autorisierungsanfrage (oder Anfrage zur Überprüfung der Autorisierung) muss eine eindeutige Kennung für die geschützte Ressource enthalten, für die der Benutzer den Zugriff anfordert. Eine geschützte Ressource kann eine beliebige Ebene autorisierter Inhalte sein, wie zwischen einem MVPD und teilnehmenden Programmierern vereinbart. Potenzielle geschützte Ressourcen müssen in diese Baumstruktur mit zunehmend spezifischer Granularität passen:

- Netzwerk
   - Kanal
      - Anzeigen
         - Episode
            - Asset

</br>

## RSS-Format für Medien {#media_rss}

Ressourcen können durch eine einfache Zeichenfolge (eine eindeutige Kennung für einen Kanal) identifiziert oder im Media RSS-Format (MRSS) dargestellt werden, wie es zwischen Adobe (oder einem autorisierten Adobe Pass-Authentifizierungspartner) und den beteiligten MVPDs und Programmierern vereinbart wurde. Die als Ressourcenbezeichner verwendete RSS-Zeichenfolge kann zusätzliche Informationen enthalten, z. B. Bewertungen und Metadaten der elterlichen Kontrolle.


Wenn Sie eine einfache Ressourcenkennung verwenden, z. B. „TNT“, wird davon ausgegangen, dass es sich um einen Kanal handelt, und er wird in diesen RSS-Ressourcenbezeichner übersetzt:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


Ein komplexerer Bezeichner kann beispielsweise zusätzliche Bewertungsinformationen enthalten. Sie können die gesamte RSS-Zeichenfolge an Access Enabler-Funktionen übergeben, für die eine Ressourcen-ID erforderlich ist, z. B. [`getAuthorization()`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md):

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

Ressourcenbezeichner sind für die Adobe Pass-Authentifizierung undurchsichtig; sie werden einfach an die MVPD weitergeleitet. Wenn MVPD Ihren Ressourcenbezeichner nicht erkennt oder nicht analysieren kann, wird ein Fehler an die Adobe Pass-Authentifizierung zurückgegeben, wodurch der Fehler an Ihren `tokenRequestFailed()`-Callback zurückgegeben wird.

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
