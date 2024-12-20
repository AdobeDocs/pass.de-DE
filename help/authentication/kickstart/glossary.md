---
title: Glossar
description: Glossar
exl-id: e64a94f6-7460-4aa8-8d6b-e0553ba1e4ec
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Glossar {#glossary}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Access Enabler {#accessEnabler}

Die Client-Komponente der Adobe Pass-Authentifizierung. Die Adobe Pass-Authentifizierung bietet eine AccessEnabler-Bibliothek für jede unterstützte Plattform.

## AuthNr {#authn}

Wird als Kurzschreibweise für „Authentifizierung“ verwendet, wie in „AuthN Token“ oder „AuthN Flow“.


## Authentifizierungs-Token{#authn-token}

Authentifizierungstoken, das von der Adobe Pass-Authentifizierung generiert wird, nachdem ein Benutzer erfolgreich mit einer MVPD authentifiziert wurde. Je nach Integrationsplattform des Programmierers wird das Token auf dem Gerät des Benutzers oder auf den Adobe Pass-Authentifizierungsservern gespeichert.

## AuthZ {#authz}

Wird als Kurzschreibweise für „Autorisierung“ verwendet, wie in „AuthZ Token“ oder „AuthZ Flow“.

## AuthZ-Token {#authz-token}

Autorisierungs-Token, das von der Adobe Pass-Authentifizierung generiert wird, nachdem ein Benutzer berechtigt wurde, geschützte Inhalte anzuzeigen. Das AuthZ-Token wird auf Adobe Pass-Authentifizierungs-Servern gespeichert und zum Generieren eines [kurzlebigen Medien-Tokens](#short-lived-token) verwendet.

## Kanal-ID (veraltet) {#channel_id}

Ehemaliger Begriff für Ressourcen-ID.

## Clientless-API {#clientless-api}

Eine Lösung zur Integration der Adobe Pass-Authentifizierung, die Web-Services anstelle der AccessEnabler-Client-Komponente verwendet.

## Geräte-ID {#device-id}

Identifiziert ein Gerät (Smartphone, Tablet usw.) in der Adobe Pass-Authentifizierung eindeutig. Diese ID wird vom Programm des Programmierers abgerufen/bereitgestellt.


## Berechtigungsfluss{#entitlement_flow}

Der in der Dokumentation zur Adobe Pass-Authentifizierung verwendete Begriff bezieht sich auf den gesamten Prozess der Registrierung eines Geräts/Benutzers bei der Adobe Pass-Authentifizierung, der Authentifizierung eines Benutzers bei einer MVPD, der Autorisierung einer Ressource für einen Benutzer und der Abmeldung eines Benutzers.


## GUID {#guid}

Siehe [Benutzer-ID](#user-id).

## IdP {#idp}

Identifizieren Sie den -Anbieter, der in einer Adobe Pass-Authentifizierungsintegration als Synonym für MVPD im Kontext der Rolle eines MVPDs fungiert. (Kunden müssen ihre Identität über die Anmeldeseite ihres Pay-TV-Anbieters überprüfen.)

## Medien-Token-Verifizierer {#media-token-verifier}

Eine von Adobe bereitgestellte Bibliothek, die von Programmierern verwendet wird, um das kurzlebige Medien-Token zu überprüfen, das von der Adobe Pass-Authentifizierung nach erfolgreichem Abschluss eines Berechtigungsflusses generiert wurde.

## MVPD {#mvpd}

Multi-Channel Video Programming Distributor; Synonym für „Pay TV Provider“.

## MVPD ID {#mvpd-id}

Siehe [Benutzer-ID](#user-id).

## Partner-ID {#partner-id}

Eine Kennung, die Adobe an MVPDs übergibt, die damit identifizieren, für wen die Adobe Pass-Authentifizierung eine Authentifizierung anfordert. Manchmal wird sie zum Konfigurieren der Benutzeroberflächen für bestimmte Programmierer verwendet, manchmal ist sie für alle Programmierer gleich und hängt von den Anforderungen der MVPD ab.

## Pay-TV-Anbieter {#pay-tv-provider}

Synonym für [MVPD](#mvpd).

## Programmierer {#programmer}

Synonym für „Content Provider“, „Account“, „Channel“, „Service Provider“, „Brand“ usw.

## Proxy-MVPD {#proxy-mvpd}

Eine MVPD, die Identitätsdienste für andere MVPDs bereitstellt; direkt in die Adobe Pass-Authentifizierung integriert.

## Proxy-MVPD {#proxied-mvpd}

Eine MVPD, die keine direkte Integration mit dem Adobe-SP aufweist, aber über einen Proxy-MVPD integriert ist.

## Antragsteller-ID {#requestor-id}

Identifiziert einen [Programmierer](#programmer) (ein Konto, eine Marke, einen Kanal oder eine Eigenschaft) innerhalb der Adobe Pass-Authentifizierung. Diese ID wird zwischen dem Programmierer und dem Adobe bei der Ersteinrichtung des Accounts festgelegt. Im Internet ist die Anforderer-ID mit einer Reihe von Domains auf der Whitelist verknüpft. Aufrufe, die eine ID von einer externen Domain verwenden, werden abgelehnt. Programmierer verwenden auch die Anforderer-ID für die Analyse. Pro Programmierer gibt es in der Regel nur eine Anforderer-ID. Eine zusätzliche Funktion im Zusammenhang mit der Anforderer-ID besteht darin, dass der Programmierer Adobe ein öffentliches Zertifikat bereitstellen muss, da der setRequestor-API-Aufruf das Senden verschlüsselter Daten erwartet, die zur Authentifizierung des Programmierers im Adobe Pass-Authentifizierungssystem verwendet werden.

## Ressourcen-ID {#resource-id}

Eine String- oder mRSS-Ressource, die einen [Programmierer](#programmer) für MVPDs identifiziert. Es wird zwischen dem Programmierer und den MVPDs vereinbart; die Adobe Pass-Authentifizierung übergibt die Ressourcen-ID durch unberührt, sodass sie für alle MVPDs gleich sein muss. Ein Programmierer kann mehrere Ressourcen-IDs verwenden, solange die MVPDs wissen, was jede ID darstellt.

## SessionGUID {#sessionGUID}

Siehe [Benutzer-ID](#user-id).

## Kurzlebiges Medien-Token {#short-lived-token}

Dieses Token wird von der Adobe Pass-Authentifizierung nach erfolgreichem Abschluss des Berechtigungsprozesses für einen bestimmten Benutzer generiert. Das Token wird an den Programmierer übergeben, der den Adobe Pass Authentication Token Verifier auf dem kurzlebigen Medien-Token verwendet, um die Sicherheit des Berechtigungsprozesses zu überprüfen.

## Intelligentes Gerät {#smart-device}

Ein Begriff, der in der Dokumentation zur Adobe Pass-Authentifizierung verwendet wird und sich auf Set-Top-Boxen, Spielekonsolen und Smart-TVs bezieht. Hierbei handelt es sich um Geräte mit Netzwerkfunktionen, die jedoch keine Web-Seiten rendern können.

## SP{#sp}

Dienstleister; bezieht sich in der Regel auf die *Rolle* des SP, die von der Adobe Pass-Authentifizierung wahrgenommen wird und im Namen eines Programmierers in einer Integration mit einer [MVPD agiert](#mvpd).

## Temp Pass {#temp-pass}

Eine Funktion, mit der Programmierer temporären kostenlosen Zugriff auf Pay-Inhalte bereitstellen können. Der Zugriff erfolgt pro Anfragendem für einen vom Programmierer festgelegten Zeitraum.

## TTL {#ttl}

Zeit zu leben. Dies ist die angegebene Zeitspanne, für die ein Token gültig ist.

## TVE {#tve}

Überall Fernsehen.

## Benutzer-ID {#user-id}

identifiziert den Benutzer einer Programmierer-App eindeutig, stammt jedoch vom MVPD. In verschiedenen Formularen für verschiedene Anwendungsfälle verfügbar. Siehe [Grundlegendes zu Benutzer-IDs in der Übersicht zu ](/help/authentication/kickstart/programmer-overview.md#user-ids).

## Zulassungsliste {#whitelist}

Eine Liste der Domains, die für die Kommunikation mit der Adobe Pass-Authentifizierung als legitim gekennzeichnet sind.

## XSTS-Token {#xsts-token}

Ein von Microsoft ausgestelltes Sicherheits-Token für die Entwicklung von Xbox Console-Apps, das in der Xbox/Adobe Pass-Authentifizierungsintegration verwendet wird.
