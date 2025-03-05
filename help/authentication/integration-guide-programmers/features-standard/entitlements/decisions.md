---
title: Entscheidungen
description: Entscheidungen
exl-id: 1efd70af-8c1d-43c4-87fc-14488d42b23d
source-git-commit: a19f4fd40c9cd851a00f05f82adbabb85edd8422
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 0%

---

# Entscheidungen {#decisions}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Entscheidungen werden durch die Adobe Pass-Authentifizierung [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) auf der Grundlage der MVPD-Autorisierungs- oder Vorabautorisierungsanfragen des Benutzers generiert, die bestimmen, ob der Zugriff auf [geschützte Inhalte](#protected-resources) gewährt oder verweigert wird.

Je nach aufgerufener API werden zwei Arten von Entscheidungen bereitgestellt:

* [Entscheidungen vor der Autorisierung](#preauthorization-decisions) die informative Entscheidungen sind.
* [Autorisierungsentscheidungen](#authorization-decisions) die maßgebliche Entscheidungen sind.

## Entscheidungen vor der Zulassung {#preauthorization-decisions}

Die Entscheidung vor der Autorisierung ist eine informative Entscheidung, mit der die Client-Anwendung darüber informiert werden kann, ob die MVPD den Zugriff des Benutzers auf eine [ Ressource zulassen oder ](#protected-resources).

Der Zweck der Vorabautorisierung (PreFlight-Autorisierung) besteht darin, der Anwendung die Anzeige genauer Informationen über den Inhalt zu ermöglichen, den der Benutzer anzeigen darf. Dies wird durch die Erweiterung der Benutzeroberfläche mit Indikatoren wie gesperrten oder entsperrten Symbolen erreicht, die den Zugriffsstatus widerspiegeln.

>[!IMPORTANT]
>
> Die Entscheidung vor der Genehmigung darf nicht in einer maßgeblichen Weise dazu benutzt werden, Ressourcen zu spielen, da dies der Zweck einer [Genehmigungsentscheidung](#authorization-decisions) ist.

Die Verwendung der Vorabautorisierungs-API ist nicht obligatorisch. Die Client-Anwendung kann dies überspringen, wenn sie einen Katalog von Ressourcen ohne Filterung präsentieren möchte.

Wenn die Client-Anwendung diese Funktion verwenden möchte, ist es wichtig zu beachten, dass Entscheidungen vor der Autorisierung nur für eine begrenzte Anzahl von Ressourcen pro API-Anfrage, in der Regel bis zu 5, abgerufen werden können.

>[!IMPORTANT]
> 
> Die maximale Anzahl von Ressourcen kann erst erhöht werden, nachdem eine Vereinbarung mit den MVPDs und den Adobe Pass-Authentifizierungsbeauftragten getroffen wurde. Nach der Vereinbarung können die Änderungen über das TVE-Dashboard von Adobe Pass durch einen Administrator in Ihrem Unternehmen oder einen in Ihrem Namen handelnden Adobe Pass-Authentifizierungsbeauftragten implementiert werden.
> 
> Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

MVPDs können die Vorautorisierung durch verschiedene Mechanismen unterstützen, die jeweils unterschiedliche Auswirkungen auf die Leistung und die maximale Anzahl von Ressourcen haben, die in einer einzelnen API-Anfrage verarbeitet werden können.

Weitere Informationen zu den bestehenden Mechanismen zur Unterstützung der Vorautorisierung finden Sie in der Dokumentation [MVPD Preflight-](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md).

>[!IMPORTANT]
>
> Bei MVPDs ohne vollständige Unterstützung der Vorflugautorisierung muss die Verwendung der Vorautorisierung vorab mit den MVPDs und den Adobe Pass-Authentifizierungsvertretern vereinbart werden, da dies zu Leistungsproblemen und langsameren Antwortzeiten führen kann.

## Autorisierungsentscheidungen {#authorization-decisions}

Die Autorisierungsentscheidung ist eine autorisierende Entscheidung, die es der Client-Anwendung ermöglicht, mit der MVPD-Entscheidung konform zu sein, den Zugriff des Benutzers auf eine [geschützte Ressource“ zuzulassen oder ](#protected-resources).

Die Autorisierung dient dazu, der Anwendung zu ermöglichen, die vom Benutzer angeforderten Ressourcen abzuspielen, nachdem die Rechte mit der MVPD validiert und ein Medien-Token von der Adobe Pass-Authentifizierung empfangen wurden.

>[!IMPORTANT]
> 
> Die Adobe Pass-Authentifizierung empfiehlt, dass Programmierer die Media Token Verifier-Bibliothek verwenden, um das in einer Autorisierungsentscheidung enthaltene Medien-Token zu validieren, sodass ein sicherer Zugriff gewährleistet ist, bevor der Video-Stream gestartet wird.
> 
> Weitere Informationen finden Sie in der Dokumentation [Medien-Token](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) .

Die Verwendung der Autorisierungs-API ist obligatorisch. Die Client-Anwendung kann diese Phase nicht überspringen, wenn sie die von der Benutzerin oder dem Benutzer angeforderten Ressourcen wiedergeben möchte, da sie bzw. er bei der MVPD überprüfen muss, ob die Benutzerin oder der Benutzer berechtigt ist, bevor der Stream freigegeben wird.

Beachten Sie, dass Autorisierungsentscheidungen nur für eine begrenzte Anzahl von Ressourcen pro API-Anfrage, normalerweise 1, abgerufen werden können.

>[!IMPORTANT]
>
> Die maximale Anzahl von Ressourcen kann erst erhöht werden, nachdem eine Vereinbarung mit den MVPDs und den Adobe Pass-Authentifizierungsbeauftragten getroffen wurde.

## Autorisierungs-TTL-Verwaltung (Time-to-Live) {#authorization-ttl-management}

Die Gültigkeitsdauer (Time-to-Live, TTL) für die Autorisierung definiert, wie lange eine Ressource autorisiert bleibt, bevor eine erneute Autorisierung erforderlich wird. Dieser Zeitraum ist begrenzt und muss mit MVPD-Vertretern vereinbart werden. TTL-Werte können je nach folgenden Kriterien variieren:

* Plattformkategorie (z. B. Desktop-Computer, Mobilgeräte, an das Fernsehgerät angeschlossene Geräte)
* Spezifische Plattform (z. B. iOS, Android, tvOS, Roku, FireTV)

Die Autorisierungs-(Autorisierungs-)TTL kann über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder von einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsbeauftragten angezeigt und geändert werden.

Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard Integrations-Benutzerhandbuch](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

## Geschützte Ressourcen {#protected-resources}

Geschützte Ressourcen beziehen sich auf streambare Inhalte, die durch eindeutige Werte identifiziert werden, die durch Vereinbarungen zwischen MVPDs und teilnehmenden Programmierern definiert werden.

Geschützte Ressourcen folgen einer hierarchischen Baumstruktur, wobei jede Ebene eine höhere Granularität für die Inhaltsautorisierung bietet:

* Netzwerk
   * Kanal
      * Anzeigen
         * Episode
            * Asset

>[!IMPORTANT]
>
> Die Vorabautorisierung (Preflight-Autorisierung) konzentriert sich auf Ressourcen auf Kanalebene mit Kennungen entweder im einfachen String- oder MRSS-Format.
> 
> Es wird nicht empfohlen, Ressourcen mit Kennungen zu verwenden, die im Falle einer Vorautorisierung `CDATA` Abschnitte enthalten, da diese hauptsächlich für Ressourcen auf Asset-Ebene verwendet werden, die durch eine MRSS definiert sind.

### Ressourcenkennung {#resource-identifier}

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

Die eindeutigen Kennungen sind für die Adobe Pass-Authentifizierung in erster Linie undurchsichtig. Transformatoren können jedoch je nach den Funktionen und Anforderungen der MVPD angewendet werden. Wenn MVPD eine Ressourcenkennung nicht erkennen oder analysieren kann, wird ein Fehler an die Adobe Pass-Authentifizierung zurückgegeben, die den Fehler anschließend mithilfe eines [erweiterten Fehlercodes) an ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) Client-Anwendung weiterleitet.

## REST-API V2 {#rest-api-v2}

Die Entscheidungen vor der Autorisierung können mit der folgenden API abgerufen werden:

* [Abrufen von Entscheidungen vor der Autorisierung mithilfe bestimmter MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Die Autorisierungsentscheidungen können mithilfe der folgenden API abgerufen werden:

* [Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Weitere Informationen zur Struktur von Entscheidungen vor **und** finden **in den Abschnitten** Antwort“ und „Beispiele der obigen APIs.

Weitere Informationen zur Integration der oben genannten APIs finden Sie in den folgenden Dokumenten:

* [Grundlegender Vorautorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [Häufig gestellte Fragen zur Vorautorisierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)
> [Häufig gestellte Fragen zur Genehmigungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
