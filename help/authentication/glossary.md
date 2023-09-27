---
title: Glossar
description: Glossar
exl-id: e64a94f6-7460-4aa8-8d6b-e0553ba1e4ec
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Glossar {#glossary}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## AccessEnabler {#accessEnabler}

Die Client-Komponente der Adobe Pass-Authentifizierung. Adobe Pass Authentication bietet eine AccessEnabler-Bibliothek für jede unterstützte Plattform.

## AuthN {#authn}

Wird als Kurzbezeichnung für &quot;Authentifizierung&quot;verwendet, wie in &quot;AuthN Token&quot;oder &quot;AuthN Flow&quot;.


## AuthN-Token{#authn-token}

Authentifizierungstoken, das von der Adobe Pass-Authentifizierung generiert wird, nachdem ein Benutzer erfolgreich bei einem MVPD authentifiziert wurde. Je nach Integrationsplattform des Programmierers wird das Token auf dem Gerät des Benutzers oder auf den Adobe Pass-Authentifizierungsservern gespeichert.

## AuthZ {#authz}

Wird als Kurzbezeichnung für &quot;Autorisierung&quot;verwendet, wie in &quot;AuthZ Token&quot;oder &quot;AuthZ Flow&quot;.

## AuthZ-Token {#authz-token}

Autorisierungstoken, das von der Adobe Pass-Authentifizierung generiert wird, nachdem ein Benutzer zur Anzeige geschützter Inhalte autorisiert wurde. Das AuthZ-Token wird auf Adobe Pass-Authentifizierungsservern gespeichert und zum Generieren einer [Kurzlebige Medien-Token](#short-lived-token).

## Kanal-ID (nicht mehr unterstützt) {#channel_id}

Ehemaliger Begriff für Ressourcen-ID.

## Clientlose API {#clientless-api}

Eine Adobe Pass-Authentifizierungsintegrationslösung, die Webdienste anstelle der AccessEnabler-Client-Komponente verwendet.

## Geräte-ID {#device-id}

Identifiziert ein Gerät (z. B. ein Telefon, ein Tablet usw.) in der Adobe Pass-Authentifizierung. Diese ID wird von der Anwendung des Programmierers abgerufen/bereitgestellt.


## Berechtigungsfluss{#entitlement_flow}

Der in der Adobe Pass-Authentifizierungsdokumentation verwendete Begriff bezieht sich auf den gesamten Prozess der Registrierung eines Geräts/Benutzers mit Adobe Pass-Authentifizierung, der Authentifizierung eines Benutzers mit einem MVPD, der Autorisierung einer Ressource für einen Benutzer und der Abmeldung eines Benutzers.


## GUID {#guid}

Siehe [Benutzer-ID](#user-id).

## IdP {#idp}

Identifizieren Sie den Provider; Synonym mit MVPD im Kontext der Rolle eines MVPD in einer Adobe Pass-Authentifizierungsintegration. (Die Kunden müssen ihre Identifizierung über die Anmeldeseite ihres Pay TV-Anbieters überprüfen.)

## Media Token Verifier {#media-token-verifier}

Eine von Adobe bereitgestellte Bibliothek, die von Programmierern verwendet wird, um das von der Adobe Pass-Authentifizierung nach erfolgreichem Abschluss eines Berechtigungsflusses generierte kurzlebige Medien-Token zu überprüfen.

## MVPD {#mvpd}

Multichannel Video Programming Distributor; Synonym für &quot;Pay TV Provider&quot;.

## MVPD ID {#mvpd-id}

Siehe [Benutzer-ID](#user-id).

## Partner-ID {#partner-id}

Eine Kennung, die von Adobe an MVPDs übergeben wird, die sie zur Identifizierung verwenden, in deren Namen die Adobe Pass-Authentifizierung eine Authentifizierung anfordert. Manchmal wird es zum Konfigurieren ihrer Benutzeroberflächen für bestimmte Programmierer verwendet, manchmal ist es für alle Programmierer gleich, es hängt von den Anforderungen des MVPD ab.

## Pay TV Provider {#pay-tv-provider}

Synonym mit [MVPD](#mvpd).

## Programmierer {#programmer}

Synonym mit &quot;Inhaltsanbieter&quot;, &quot;Konto&quot;, &quot;Kanal&quot;, &quot;Dienstleister&quot;, &quot;Marke&quot;usw.

## Proxy-MVPD {#proxy-mvpd}

Ein MVPD, der Identitätsdienste für andere MVPDs bereitstellt, die direkt mit der Adobe Pass-Authentifizierung integriert sind.

## Proximiertes MVPD {#proxied-mvpd}

Ein MVPD, das keine direkte Integration mit dem Adobe SP hat, aber über einen Proxy-MVPD integriert ist.

## Anforderer-ID {#requestor-id}

Identifiziert eindeutig eine [Programmierer](#programmer) (ein Konto, eine Marke, einen Kanal oder eine Eigenschaft) innerhalb der Adobe Pass-Authentifizierung. Diese ID wird während der Ersteinrichtung des Kontos zwischen dem Programmierer und der Adobe bestimmt. Im Internet ist die Anforderer-ID mit einer Reihe von auf der Whitelist befindlichen Domänen verknüpft. Aufrufe, die eine Kennung von einer externen Domäne verwenden, werden abgelehnt. Programmierer verwenden auch die Anforderer-ID für Analysen. Normalerweise gibt es nur eine Anforderer-ID pro Programmierer. Eine zusätzliche Funktion im Zusammenhang mit der Anforderer-ID besteht darin, dass der Programmierer Adobe ein öffentliches Zertifikat bereitstellen muss, da der setRequestor-API-Aufruf erwartet, dass verschlüsselte Daten gesendet werden, die zur Authentifizierung des Programmierers im Adobe Pass-Authentifizierungssystem verwendet werden.

## Ressourcen-ID {#resource-id}

Eine Zeichenfolge oder mRSS-Ressource, die eine [Programmierer](#programmer) in MVPDs. Es wird zwischen dem Programmierer und den MVPDs vereinbart. Die Adobe Pass-Authentifizierung übergibt die Ressourcen-ID über &quot;unberührt&quot;, daher muss sie für alle MVPDs gleich sein. Ein Programmierer kann mehrere Ressourcen-IDs verwenden, solange die MVPDs wissen, was jede ID darstellt.

## SessionGUID {#sessionGUID}

Siehe [Benutzer-ID](#user-id).

## Kurzlebige Medien-Token {#short-lived-token}

Dieses Token wird von der Adobe Pass-Authentifizierung nach erfolgreichem Abschluss des Berechtigungsprozesses für einen bestimmten Benutzer generiert. Das Token wird an den Programmierer übergeben, der den Verifikator für Adobe Pass-Authentifizierungstoken auf dem Token für kurzlebige Medien verwendet, um die Sicherheit des Berechtigungsprozesses zu überprüfen.

## Smart Device {#smart-device}

Ein Begriff, der in der gesamten Adobe Pass-Authentifizierungsdokumentation verwendet wird, um auf Set-Top-Boxen, Spielekonsolen und Smart-TVs zu verweisen. Hierbei handelt es sich um Geräte mit Netzwerkfunktionen, die jedoch nicht in der Lage sind, Web-Seiten zu rendern.

## SP{#sp}

Dienstleister; dies bezieht sich normalerweise auf die *Rolle* von SP, gespielt von Adobe Pass Authentication, im Namen eines Programmierers in einer Integration mit einer [MVPD](#mvpd).

## Temporärer Pass {#temp-pass}

Eine Funktion, die es Programmierern ermöglicht, temporären freien Zugang zu bezahlten Inhalten bereitzustellen. Der Zugriff erfolgt pro Anforderer für einen vom Programmierer festgelegten Zeitraum.

## TTL {#ttl}

Time to Live. Dies ist die angegebene Zeitdauer, für die ein Token gültig ist.

## TVE {#tve}

Überall Fernsehen.

## Benutzer-ID {#user-id}

Identifiziert eindeutig den Benutzer der App eines Programmierers, stammt jedoch aus dem MVPD. In verschiedenen Formularen für verschiedene Anwendungsfälle verfügbar. Siehe [Grundlegendes zu Benutzer-IDs in der Programmierer-Übersicht](/help/authentication/programmer-overview.md#user-ids).

## Zulassungsliste {#whitelist}

Eine Liste der Domänen, die für die Kommunikation mit der Adobe Pass-Authentifizierung als rechtmäßig bezeichnet wurden.

## XSTS-Token {#xsts-token}

Ein von Microsoft ausgegebenes Sicherheits-Token für die Entwicklung von Xbox Console-Apps, das in der Integration von Xbox/Adobe Pass-Authentifizierung verwendet wird.
