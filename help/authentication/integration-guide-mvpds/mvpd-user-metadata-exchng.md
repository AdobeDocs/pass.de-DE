---
title: Austausch von Benutzermetadaten in MVPD
description: Austausch von Benutzermetadaten in MVPD
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# Austausch von Benutzermetadaten in MVPD

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Einführung {#intro-user-metadata-exchange}

MVPDs verwalten benutzerspezifische Metadaten über ihre Kunden, die in einigen Fällen für Programmierer freigegeben sind. Ziel der Adobe Pass-Authentifizierung ist es, einen Austausch dieser „Benutzermetadaten“ zu vermitteln, jedoch keine Regeln in Bezug auf den Austausch durchzusetzen. Die Austauschregeln sind für MVPDs, um mit ihren Programmierern zu arbeiten.

Die für Exchange verfügbaren Benutzer-Metadatentypen umfassen derzeit Folgendes:

* Postleitzahl
* Maximale Bewertung (VChip oder MPAA)
* Benutzer-ID
* Haushalts-ID
* Kanal-ID

Mit dieser Funktion können MVPDs und Programmierer spezielle Anwendungsfälle wie die elterliche Kontrolle implementieren. Beispielsweise kann ein MVPD die Daten der elterlichen Bewertung an einen Programmierer übergeben, der diese Daten dann verwendet, um die verfügbaren Anzeigeoptionen für einen Benutzer zu filtern.

Wichtige Punkte zu Benutzermetadaten:

* MVPD übergibt während der Authentifizierungs- und Autorisierungsflüsse Benutzermetadaten an das Programm des Programmierers
* Die Adobe Pass-Authentifizierung speichert die Metadatenwerte in den AuthN- und AuthZ-Token
* Die Adobe Pass-Authentifizierung kann Werte für MVPDs normalisieren, die Benutzermetadaten in verschiedenen Formaten bereitstellen
* Einige Parameter können mit dem Programmierschlüssel verschlüsselt werden
* Bestimmte Werte werden von Adobe über eine Konfigurationsänderung bereitgestellt

>[!NOTE]
>
>Benutzermetadaten sind eine Erweiterung der statischen Metadaten (Authentifizierungstoken-TTL, Autorisierungstoken-TTL und Geräte-ID), die zuvor in der Adobe Pass-Authentifizierung verfügbar waren.

## Beispiele {#example-mvpd-user-metadata-exch}

### elterliche Gewalt {#example-parental-control}

Dieses Beispiel zeigt den Austausch von Folgendem:

* [Programmierer für MVPD-Metadatenaustausch](#progr-mvpd-metadata-exch)

* [Austausch von MVPD-zu-Programmierer-Metadaten](#mvpd-progr-exchange-flow)

### Programmierer für MVPD-Metadatenaustausch {#progr-mvpd-metadata-exch}

Derzeit unterstützen die Programmierer-API, die Adobe Pass-Authentifizierung und die MVPD-Autorisierer nur die Autorisierung auf Kanalebene. Der Kanal wird im Aufruf der getAuthorization()-API des Programmierers als Nur-Text-Zeichenfolge angegeben. Diese Zeichenfolge wird bis zum autorisierenden Backend von MVPD übertragen:

Von der App oder Site des Programmierers wählt der Benutzer eine XACML-fähige MVPD (in diesem Beispiel „TNT„). Informationen zu XACML finden Sie unter [eXtensible Access Control Markup Language](https://en.wikipedia.org/wiki/XACML){target=_blank}.
Die App des Programmierers bildet eine AuthZ-Anfrage, die die Ressource und ihre Metadaten enthält.  Dieses Beispiel enthält eine MPAA-Bewertung von „pg“ im Medienattribut des Kanalelements:

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

Die Adobe Pass-Authentifizierung unterstützt tatsächlich eine detailliertere Autorisierung bis hin zur Asset-Ebene, wenn sie sowohl von MVPD als auch vom Programmierer unterstützt wird. Die Ressource und ihre Metadaten sind für Adobe undurchsichtig. Ziel ist es, ein Standardformat für die normalisierte Angabe der Ressourcen-ID und der Metadaten zu erstellen, um Ressourcen-IDs an verschiedene MVPDs zu senden.

>[!NOTE]
>
>Wenn der/die Benutzende eine Nur-Kanal-MVPD auswählt, extrahiert die Adobe Pass-Authentifizierung NUR den Kanalitel („TNT“ im obigen Beispiel) und übergibt nur den Titel an die MVPD.

### Austausch von MVPD-zu-Programmierer-Metadaten {#mvpd-progr-exchange-flow}

Bei der Adobe Pass-Authentifizierung wird von folgenden Annahmen ausgegangen:

* Die MVPD sendet die maximale Bewertung als Teil der SAML-Antwort
* Diese Informationen werden als Teil des Authentifizierungstokens gespeichert
* Die Adobe Pass-Authentifizierung stellt eine API bereit, mit der Programmierer diese Informationen abrufen können
* Programmierer implementieren diese Funktion auf ihrer Site oder in ihrem Programm (um beispielsweise Videos auszublenden, die über der maximalen Bewertung für den Benutzer liegen)

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### Notizen {#notes-mvpd-progr-metadata-exch-flow}

**Ressourcennormalisierung und -validierung.** Ressourcen-IDs können als Nur-String oder MRSS-String übergeben werden. Ein Programmierer kann entweder das Nur-String-Format oder das MRSS verwenden, benötigt jedoch eine vorherige Vereinbarung mit der MVPD, damit die MVPD weiß, wie diese Ressource zu behandeln ist.

**Ressourcen-ID und Metadatenspezifikation.** Adobe Pass-Authentifizierung verwendet den RSS-Standard mit der RSS-Erweiterung für Medien, um eine Ressource und ihre Metadaten anzugeben. In Verbindung mit der Media RSS-Erweiterung unterstützt die Adobe Pass-Authentifizierung eine Vielzahl von Metadaten, z. B. Kindersicherung (über `<media:rating>`) oder Geolokalisierung (`<media:location>`).

Die Adobe Pass-Authentifizierung kann auch eine transparente Konvertierung der alten Kanalzeichenfolge in die entsprechende RSS-Ressource für MVPDs unterstützen, für die RSS erforderlich ist. Umgekehrt unterstützt die Adobe Pass-Authentifizierung die Konvertierung von RSS+MRSS in einen Nur-Kanal-Titel für Nur-Kanal-MVPDs.

Die **Adobe Pass-Authentifizierung stellt die vollständige Abwärtskompatibilität mit bestehenden Integrationen sicher.** das heißt, für Programmierer, die Authentifizierung auf Kanalebene verwenden, wird bei der Adobe Pass-Authentifizierung darauf geachtet, die Kanal-ID im erforderlichen Format zu verpacken, bevor sie an eine MVPD gesendet wird, die dieses Format versteht. Umgekehrt gilt auch Folgendes: Wenn ein Programmierer alle seine Ressourcen in einem neuen Format angibt, übersetzt die Adobe Pass-Authentifizierung das neue Format in eine einfache Kanalzeichenfolge, wenn die Autorisierung für eine MVPD erfolgt, die nur eine Autorisierung auf Kanalebene ausführt.

## Anwendungsfälle für Benutzermetadaten {#user-metadata-use-cases}

Anwendungsfälle ändern sich ständig und werden immer größer, da mehr MVPDs rechtliche Vorkehrungen treffen und Funktionen hinzufügen. Im Folgenden finden Sie Beispiele dafür, wofür Benutzermetadaten verwendet werden können.

* [MVPD-Benutzer-ID](#mvpd-user-id)
* [Haushalts-ID](#household-user-id)
* [Postleitzahl](#zip-code)
* [Max. Bewertung (Kindersicherung)](#max-rating-parental-control)
* [Kanalanordnung](#channel-line-up)

### MVPD-Benutzer-ID {#mvpd-user-id}

* Wie vom MVPD bereitgestellt
* Nicht die eigentlichen Anmeldeinformationen des Benutzers, da sie von der MVPD gehasht werden
* Kann verwendet werden, um Probleme mit oder für bestimmte Benutzende anzuzeigen
* verschlüsselt
* MVPD-Unterstützung: Alle MVPDs

### Haushaltsbenutzer-ID {#household-user-id}

* Ermöglicht gute Metrikinformationen
* verschlüsselt
* MVPD-Unterstützung: Einige MVPDs

### Postleitzahl {#zip-code}

* Die Postleitzahl der Rechnungsstellung des Benutzers
* Wird hauptsächlich verwendet, um Regeln für das Einfrieren von Sportereignissen zu erzwingen
* Kann mit der AuthZ-Antwort für schnelle Aktualisierungen bereitgestellt werden
* MVPD-Unterstützung: Einige MVPDs

### Max. Bewertung (Kindersicherung) {#max-rating-parental-control}

* AuthN anfänglich plus AuthZ-Aktualisierung
* Filtern von Inhalten aus der Benutzeroberfläche
* MPAA- oder VChip-Bewertungen
* MVPD-Unterstützung: Einige MVPDs

### Kanalanordnung {#channel-line-up}

* MVPDs können eine Liste von Kanälen bereitstellen, die der Benutzer anzeigen darf
* Ermöglicht schnelles Zeichnen der Benutzeroberfläche
* Die OLCA-Spezifikation ermöglicht dies als AttributeStatement in der Authentifizierungsantwort
* Unterstützende MVPDs: Einige MVPDs

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
