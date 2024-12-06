---
title: Ermitteln geschützter Ressourcen
description: Ermitteln geschützter Ressourcen
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Ermitteln geschützter Ressourcen {#identifying-protected-resources}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Jede Autorisierungsanfrage (oder Anforderung der Autorisierung) muss eine eindeutige Kennung für die geschützte Ressource enthalten, für die der Benutzer Zugriff anfordert. Eine geschützte Ressource kann eine beliebige Ebene autorisierter Inhalte sein, wie zwischen einem MVPD und teilnehmenden Programmierern vereinbart. Potenzielle geschützte Ressourcen müssen in diese Baumstruktur mit zunehmend spezifischer Granularität passen:

- Netzwerk
   - Kanal
      - Anzeigen
         - Folge
            - Asset

</br>

## Medien-RSS-Format {#media_rss}

Ressourcen können durch eine einfache Zeichenfolge (eine eindeutige Kennung für einen Kanal) identifiziert oder im Media RSS-Format (MRSS) dargestellt werden, wie zwischen der Adobe (oder einem für die Adobe Pass-Authentifizierung autorisierten Partner) und den teilnehmenden MVPDs und Programmierern vereinbart. Die als Ressourcenspezifikator verwendete RSS-Zeichenfolge kann zusätzliche Informationen wie Bewertungen und Metadaten zur elterlichen Kontrolle enthalten.


Wenn Sie eine einfache Ressourcenkennung wie &quot;TNT&quot;verwenden, wird angenommen, dass diese einen Kanal darstellt und in diesen RSS-Ressourcenbezeichner übersetzt wird:

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

Ressourcenspezifikatoren sind für die Adobe Pass-Authentifizierung opak; sie werden einfach an den MVPD weitergeleitet. Wenn der MVPD Ihren Ressourcenbezeichner nicht erkennt oder nicht analysieren kann, wird ein Fehler an die Adobe Pass-Authentifizierung zurückgegeben, wodurch der Fehler an Ihren `tokenRequestFailed()` -Rückruf zurückgegeben wird.

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
