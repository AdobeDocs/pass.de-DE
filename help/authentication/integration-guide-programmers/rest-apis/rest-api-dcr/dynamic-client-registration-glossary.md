---
title: Glossar zur dynamischen Client-Registrierung (DCR)
description: Glossar zur dynamischen Client-Registrierung (DCR)
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Glossar zur dynamischen Client-Registrierung (DCR) {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

In diesem Dokument finden Sie Definitionen für Begriffe, die bei der Integration der Dynamic Client Registration (DCR) für die Adobe Pass-Authentifizierung verwendet werden.

>[!MORELIKETHIS]
> 
> * [Glossar zur REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## Glossarbegriffe {#glossary-terms}

### Ein {#a}

#### Zugriffstoken {#access-token}

Das Zugriffstoken ist ein Token, das von der Adobe Pass-Authentifizierung als Ergebnis des [Dynamische Client-Registrierung (DCR)) generiert ](#dcr), um den Zugriff auf geschützte APIs sicherzustellen.

### C {#c}

#### Client-Anmeldedaten {#client-credentials}

Die Client-Anmeldeinformationen sind ein Satz eindeutiger Werte, die während des [Dynamischen Client-Registrierungsprozesses (DCR)](#dcr) generiert werden und zum Abrufen eines [Zugriffstokens“ verwendet ](#access-token).

#### Benutzerdefiniertes Schema {#custom-scheme}

Das benutzerdefinierte Schema ist ein eindeutiger Wert, der auf eine [Programmierer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)-Anwendung verweist, die über das Adobe Pass-[TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) generiert und heruntergeladen werden kann und als endgültige Umleitung in Anwendungen dienen soll, die auf iOS-Geräten ausgeführt werden.

### D {#d}

#### DCR {#dcr}

Die dynamische Client-Registrierung (DCR) ist ein Autorisierungsmechanismus, der durch [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) definiert wird und auf dem OAuth 2.0-Autorisierungs-Framework basiert, das durch [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) beschrieben wird.

Der DCR wird an einen [Programmierer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) als Adobe Pass-Authentifizierungs-Service bereitgestellt, der den Zugriff auf geschützte APIs weiter ermöglichen kann.

Weitere Informationen finden Sie in der Dokumentation [Übersicht über die Dynamic Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

### R {#r}

#### Registrierte Anwendung {#registered-application}

Die registrierte Anwendung ist ein Adobe Pass-Authentifizierungskonzept, das Informationen über die [Programmierer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)-Anwendung speichert, die zum Fortfahren mit dem [Dynamic Client Registration (DCR)-](#dcr) erforderlich sind.

### S {#s}

#### Software-Anweisung {#software-statement}

Die Software-Anweisung ist ein JSON Web Token (JWT), das vom Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) heruntergeladen werden kann und als Teil des Prozesses [Dynamische Client-Registrierung (DCR)](#dcr) verwendet werden soll.
