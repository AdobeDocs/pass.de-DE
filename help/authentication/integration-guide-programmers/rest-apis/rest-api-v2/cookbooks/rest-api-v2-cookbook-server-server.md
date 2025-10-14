---
title: REST API V2-Cookbook (Server-zu-Server)
description: REST API V2-Cookbook (Server-zu-Server)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2510'
ht-degree: 0%

---

# REST API V2-Cookbook (Server-zu-Server) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

Das Dokument richtet sich an Entwicklerinnen und Entwickler, die die [Adobe Pass Authentication REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) in ihre Streaming-Anwendungen mit einer Server-zu-Server-Architektur (S2S) integrieren.

## Voraussetzungen {#prerequisites}

Definitionen hierzu finden Sie im Glossar zur [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md).

Informationen zu obligatorischen Anforderungen und empfohlenen Vorgehensweisen finden Sie in der Dokumentation [REST API V2 Checklist](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) .

Häufig gestellte Fragen finden Sie in der Dokumentation [Häufig gestellte Fragen zur REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) .

### Komponenten {#components}

Bevor Sie beginnen, sollten Sie über Kenntnisse der folgenden Komponenten und Begriffe verfügen, die im Workflow verwendet werden:

| Typ | Komponente | Beschreibung |
|---------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe-Infrastruktur | Adobe Pass-Service | lässt sich mit dem MVPD-IDp- und -AuthZ-Service integrieren, um Authentifizierungs- und Autorisierungsentscheidungen bereitzustellen. |
| Programmiererinfrastruktur | Programmierdienst | Verbindet das Streaming-Gerät mit dem Adobe Pass-Service, um authentifizierte Profile und Autorisierungsentscheidungen abzurufen. |
| MVPD-Infrastruktur | MVPD IDp-Dienst | MVPD-Endpunkt, der für die berechtigungsbasierte Authentifizierung verantwortlich ist und die Identität des Benutzers validiert. |
|                           | MVPD AuthZ-Service | MVPD-Endpunkt, der Autorisierungsentscheidungen basierend auf Benutzerabonnements, Kindersicherung und anderen Berechtigungsregeln bestimmt. |
| Streaming-Gerät | Streaming-App | Die Anwendung des Programmierers, die auf dem Streaming-Gerät des Benutzers ausgeführt wird und authentifizierte Videoinhalte abspielt. |
|                           | (Optional) Authentifizierungsmodul | Wenn das Streaming-Gerät über einen user-agent verfügt (z. B. einen Browser), übernimmt das AuthN-Modul die Benutzerauthentifizierung für die MVPD-ID. |
| (Optional) Authentifizierungs-Gerät | Authentifizierungs-App | Wenn dem Streaming-Gerät ein user-agent (z. B. ein Browser) fehlt, ist die AuthN-Anwendung eine Programmierer-Web-Anwendung, auf die von einem separaten Gerät über einen Webbrowser zugegriffen wird. |

### Anforderungen {#requirements}

Bei Server-zu-Server (S2S)-Implementierungen müssen die Streaming-App und der Programmiererdienst ein Protokoll einrichten, das dem Programmiererdienst Folgendes ermöglicht:

* Kommunizieren Sie mit dem Adobe Pass-Service im Namen der Streaming-App.

* Erfassen und übergeben Sie eine eindeutige Gerätekennung für das Streaming-Gerät, wie vom [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)-Header benötigt.

* Erfassen und übergeben Sie genaue Streaming-Geräteinformationen, einschließlich des Quell-Ports und gerätespezifischer Details, wie vom [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)-Header benötigt.

* Erfassen und übergeben Sie die IP-Adresse des Streaming-Geräts gemäß der Kopfzeile „X-Forwarded-For“.

* Speichern Sie Parameter wie Geräte-ID, Client-ID und Client-Geheimnis sicher in der Streaming-App oder im Programmierdienst.

* Formatieren und senden Sie Daten in Übereinstimmung mit MVPDs und integrierten Apps, einschließlich Geräte-IP, Quell-Port, gerätespezifischen Informationen, MRSS und optionalen Kennungen wie ECID.

* Verwalten und verwalten Sie Zertifikate, die mit Adobe für die verschlüsselte Übertragung von Benutzermetadaten freigegeben sind.

* Achten Sie beim Caching auf die Gültigkeit des Authentifizierungsprofils und der Autorisierungsentscheidung, um sicherzustellen, dass der Authentifizierungs- und Autorisierungsstatus bei der Benachrichtigung ungültig gemacht wird.

* Rückgabe von Autorisierungsentscheidungen und relevanten Anweisungen an die Streaming-App.

### Umgebungen {#environments}

Bevor Sie mit dem Workflow beginnen, stellen Sie sicher, dass Sie mindestens zwei Umgebungen pflegen: Produktion und Staging.

**Produktion**

Die Produktionsumgebung muss hoch verfügbar sein und entsprechend skaliert werden, um große oder unerwartete Traffic-Spitzen zu bewältigen, z. B. durch Live-Sportereignisse oder aktuelle Nachrichten.

* Der Adobe Pass-Service wird in mehreren geografisch verteilten Rechenzentren in den USA eingesetzt, um die Leistung zu optimieren und die Latenz zu minimieren.

   * Der Programmierer-Service sollte eine ähnliche Infrastrukturstrategie anwenden, um Antwortzeiten mit geringer Latenz von Adobe Pass sicherzustellen.

* Der Programmierer muss den öffentlichen IP-Bereich seiner Produktionsumgebung bereitstellen.

   * Diese IPs werden einer Zulassungsliste in der Adobe Pass-Infrastruktur hinzugefügt.

* Der Programmierdienst muss die DNS-Zwischenspeicherung auf maximal 30 Sekunden beschränken, um ein dynamisches Rerouting zu ermöglichen, falls Adobe Traffic umleiten muss, weil ein Rechenzentrum nicht verfügbar ist.

**Staging**

Die Staging-Umgebung kann minimal sein, sollte jedoch die Produktion widerspiegeln, indem alle kritischen Systemkomponenten und die Geschäftslogik einbezogen werden.

* Sie muss vor der Bereitstellung in der Produktion das Testen von Versionen ermöglichen.

* Sie sollte operativ ähnlich wie die Produktion bleiben und realistische Tests ermöglichen.

* Idealerweise sollte die Staging-Umgebung mit Adobe Pass-Testumgebungen verbunden sein, um:

   * Ermöglichen Sie Programmierern, mit der Adobe-Infrastruktur zu testen.

   * Aktivieren Sie Adobe, um bei Bedarf Tests und Fehlerbehebung zu unterstützen.

## Workflow {#workflow}

Führen Sie die folgenden Schritte aus, wie im folgenden Diagramm dargestellt.

![REST API V2-Cookbook (Server-zu-Server)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

*REST API V2-Cookbook (Server-zu-Server)*

## A. Phase der Registrierung {#registration-phase}

In der Registrierungsphase wird die Streaming-Anwendung über den DCR-Prozess (Dynamic Client Registration) für die Adobe Pass-Authentifizierung registriert.

Für den Prozess der dynamischen Client-Registrierung (DCR) muss die Streaming-Anwendung ein Paar von Client-Anmeldeinformationen abrufen und ein Zugriffstoken als Endziel der Registrierungsphase abrufen.

Die Registrierungsphase ist obligatorisch, aber die Streaming-Anwendung kann diese Phase überspringen, wenn sie ein zwischengespeichertes Paar von Client-Anmeldeinformationen und ein Zugriffstoken hat, die noch gültig sind.

+++Verwandte Artikel

APIs:

* [Abrufen von Client-Anmeldeinformationen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Zugriffstoken abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

Flüsse:

* [Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Häufig gestellte Fragen:

* [Häufig gestellte Fragen zur Registrierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Schritt 1: Registrieren des Programms {#step-1-register-your-application}

* Client-Anmeldeinformationen abrufen: Der Programmierer-Service ruft Client-Anmeldeinformationen ab, indem er den Endpunkt [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) aufruft.

   * Der Programmierdienst oder die Programmieranwendung müssen die Client-Anmeldeinformationen speichern und sie unbegrenzt verwenden, wenn ein Zugriffstoken abgerufen werden muss.


* Zugriffs-Token abrufen: Der Programmierer-Service ruft das Zugriffs-Token ab, indem er den Endpunkt [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) aufruft.

   * Der Programmierdienst oder die Programmieranwendung müssen das Zugriffstoken speichern und verwenden, bis es abläuft. Dann müssen Sie es verwerfen und ein neues abrufen.

## B. Authentifizierungsphase {#authentication-phase}

Der Zweck der Authentifizierungsphase besteht darin, der Streaming-Anwendung die Möglichkeit zu geben, die Identität des Benutzers zu überprüfen und Benutzermetadaten-Informationen abzurufen.

Die Authentifizierungsphase dient als erforderlicher Schritt für die Vorautorisierungsphase oder Autorisierungsphase, wenn die Streaming-Anwendung Inhalte wiedergeben muss.

+++Verwandte Artikel

APIs

* [Authentifizierungssitzung erstellen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Authentifizierungssitzung fortsetzen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Authentifizierungssitzung abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Authentifizierung im Benutzeragenten durchführen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Profile abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Abrufen eines Profils für ein bestimmtes MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Abrufen eines Profils für einen bestimmten Code](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Flows

* [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundlegender Authentifizierungsfluss innerhalb der sekundären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Häufig gestellte Fragen

* [Häufig gestellte Fragen zur Authentifizierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Schritt 2: Überprüfen auf vorhandene authentifizierte Profile {#step-2-check-for-existing-authenticated-profiles}

* **Profile abrufen:** Der Programmierdienst überprüft im Auftrag der Streaming-App vorhandene Profile, indem er den Endpunkt [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) aufruft.


* **Szenario 1:** Profile vorhanden sind, kann der Programmierer-Service mit der Phase [Vorabautorisierung](#preauthorization-phase) oder [Autorisierung) &#x200B;](#authorization-phase).


* **Szenario 2:** Es sind keine vorhandenen Profile vorhanden. Der Programmierer-Service kann mit dem nächsten Schritt fortfahren, um den [&#x200B; zu authentifizieren](#step-3-authenticate-the-user).


* **Szenario 3:** Es sind keine vorhandenen Profile vorhanden. Der Programmierer-Service kann dem Benutzer über die Funktion [TempPass“ temporären Zugriff &#x200B;](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md).

   * Dieses Szenario würde den Rahmen dieses Dokuments sprengen. Weitere Informationen finden Sie in der Dokumentation [Temporärer &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)&quot;.

### Schritt 3: Benutzer authentifizieren {#step-3-authenticate-the-user}

* **Konfiguration abrufen:** Der Programmierdienst ruft die Liste der verfügbaren MVPDs ab, indem er den Endpunkt [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) aufruft.

   * Der Programmierer-Service kann einen benutzerdefinierten Filtermechanismus implementieren, um die Liste der MVPDs aus der Konfigurationsantwort so zu verfeinern, dass die Streaming-App nur die vorgesehenen Anbieter anzeigt, während andere ausgeblendet werden (z. B. MVPDs in Entwicklung, MVPDs testen, TempPass). Dadurch wird sichergestellt, dass Benutzenden bei der Auswahl ihres TV-Anbieters eine kuratierte Auswahl angezeigt wird.


* **Authentifizierungssitzung erstellen:** Der Programmierdienst initiiert eine Authentifizierungssitzung, indem er den Endpunkt [**/api/v2/{serviceProvider}/sessions aufruft**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

   * Der Programmierdienst muss die `code` und die `url` an die Streaming-App zurückgeben.


* **Szenario 1:** Die Streaming-App kann einen Browser oder eine Webansicht öffnen, daher muss die `url` geladen werden.

   * Der Benutzer gibt seinen Benutzernamen und sein Kennwort auf der Anmeldeseite von MVPD ein. Nach erfolgreicher Authentifizierung zeigt die endgültige Umleitung eine Erfolgsseite an.


* **Szenario 2:** Die Streaming-App kann keinen Browser öffnen, daher muss die `code` angezeigt werden. Eine separate Web-Anwendung ist erforderlich, um den Benutzer zur Eingabe der `code` aufzufordern, die `url` zu erstellen und zu öffnen: [**/api/v2/Authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * Der Benutzer gibt seinen Benutzernamen und sein Kennwort auf der Anmeldeseite von MVPD ein. Nach erfolgreicher Authentifizierung zeigt die endgültige Umleitung eine Erfolgsseite an.

### Schritt 4: Prüfen auf authentifizierte Profile {#step-4-check-for-authenticated-profiles}

* **Profil für bestimmten Code abrufen:** Der Programmierer-Service muss einen Abrufmechanismus mithilfe des `code` implementieren, um zu überprüfen, ob das Profil erfolgreich generiert und gespeichert wurde, indem der [**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)-Endpunkt aufgerufen wird.

   * Der Programmierdienst muss **Abrufmechanismus** folgenden Bedingungen starten:

      * **Authentifizierung in der primären (Bildschirm-)Anwendung:** Der Programmierdienst sollte die Abfrage starten, wenn der Benutzer die endgültige Zielseite erreicht, nachdem die Browser-Komponente die für den `redirectUrl` Parameter in der Endpunktanforderung [Sitzungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) angegebene URL geladen hat.

      * **Authentifizierung in einer sekundären (Bildschirm-)Anwendung:** Die Programmierer-Service-Anwendung sollte den Abruf starten, sobald der Benutzer den Authentifizierungsprozess einleitet - direkt nach Erhalt der [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)-Endpunktantwort und Anzeige des Authentifizierungs-Codes für den Benutzer.

   * Der Programmierdienst muss **Abfrage-** unter folgenden Bedingungen stoppen:

      * **Erfolgreiche Authentifizierung:** Die Profilinformationen des Benutzers wurden erfolgreich abgerufen und bestätigen seinen Authentifizierungsstatus. Zu diesem Zeitpunkt ist keine Abfrage mehr erforderlich.

      * **Authentifizierungssitzung und Code-Ablauf:** Die Authentifizierungssitzung und der Code laufen ab, wie durch den `notAfter` Zeitstempel (z. B. 30 Minuten) in der Endpunktantwort [Sitzungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) angegeben. In diesem Fall muss der Benutzer den Authentifizierungsprozess neu starten, und das Abrufen mit dem vorherigen Authentifizierungs-Code sollte sofort gestoppt werden.

      * **Generierter neuer Authentifizierungs-Code:** Wenn der Benutzer einen neuen Authentifizierungs-Code auf dem Primärgerät (Bildschirm) anfordert, ist die vorhandene Sitzung nicht mehr gültig und das Abrufen mit dem vorherigen Authentifizierungs-Code sollte sofort gestoppt werden.

   * Der Programmierdienst muss **Abrufintervall konfigurieren** unter folgenden Bedingungen:

      * **Authentifizierung innerhalb der primären (Bildschirm-)Anwendung:** Der Programmierer-Service sollte alle 3-5 Sekunden oder länger abfragen.

      * **Authentifizierung in einer sekundären (Bildschirm-)Anwendung:** Der Programmierer-Service sollte alle 3-5 Sekunden oder länger abfragen.

   * Der Programmierdienst sollte Teile der Profilinformationen des Benutzers in einem beständigen Speicher zwischenspeichern, um unnötige Anfragen zu vermeiden und das Benutzererlebnis zu verbessern.

## C. (fakultativ) Phase der Vorabgenehmigung {#preauthorization-phase}

Der Zweck der Vorautorisierungsphase besteht darin, der Streaming-Anwendung die Möglichkeit zu geben, eine Untergruppe von Ressourcen aus ihrem Katalog darzustellen, auf die der Benutzer zugreifen könnte.

Die Vorautorisierungsphase kann das Benutzererlebnis verbessern, wenn der Benutzer die Streaming-Anwendung zum ersten Mal öffnet oder zu einem neuen Abschnitt navigiert.

Die Phase der Vorabautorisierung ist nicht obligatorisch. Die Streaming-Anwendung kann diese Phase überspringen, wenn sie einen Katalog von Ressourcen präsentieren möchte, ohne sie zuerst auf der Grundlage der Benutzerberechtigung zu filtern.

+++Verwandte Artikel

APIs

* [Abrufen von Entscheidungen vor der Autorisierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Flows

* [Grundlegender Vorautorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

Häufig gestellte Fragen

* [Häufig gestellte Fragen zur Vorautorisierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Schritt 5: Prüfen auf vorautorisierte Ressourcen {#step-5-check-for-preauthorized-resources}

* **Vorabautorisierungsentscheidungen abrufen:** Der Programmierer-Service ruft Vorabautorisierungsentscheidungen für eine Liste von Ressourcen ab, indem er den Endpunkt [**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) aufruft.

   * Der Programmierer-Service muss der Streaming-App die Liste der Genehmigungsentscheidungen übergeben und Vorabautorisierungsentscheidungen ablehnen.

   * Der Programmierdienst ist nicht erforderlich, um Entscheidungen vor Autorisierung in einem persistenten Speicher zu speichern. Es wird jedoch empfohlen, Zulassungsentscheidungen im Speicher zwischenzuspeichern, um das Benutzererlebnis zu verbessern. Dadurch werden unnötige Aufrufe von Ressourcen vermieden, die bereits vorab autorisiert wurden, die Latenz verringert und die Leistung verbessert.

   * Der Programmierer-Service kann den Grund für eine abgelehnte Vorautorisierungsentscheidung ermitteln, indem er den [Fehlercode und die Nachricht) überprüft](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) die in der Antwort vom Endpunkt Decisions Preauthorize enthalten sind. Diese Details bieten Einblicke in den spezifischen Grund, aus dem die Vorabautorisierungsanfrage abgelehnt wurde, und helfen, das Benutzererlebnis oder den Trigger über die erforderliche Handhabung in der Anwendung zu informieren. Stellen Sie sicher, dass ein Wiederholungsmechanismus, der zum Abrufen von Entscheidungen vor der Autorisierung implementiert ist, nicht zu einer Endlosschleife führt, wenn die Entscheidung vor der Autorisierung abgelehnt wird. Erwägen Sie, weitere Zustellversuche auf eine angemessene Anzahl zu beschränken und Ablehnungen elegant zu handhaben, indem Sie dem Benutzer klares Feedback senden.

   * Der Programmierer-Service kann in einer einzigen API-Anfrage eine Vorabautorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen erhalten, in der Regel bis zu 5, aufgrund von Bedingungen, die von MVPDs auferlegt werden. Diese maximale Anzahl von Ressourcen kann angezeigt und geändert werden, nachdem eine Vereinbarung mit MVPDs über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter getroffen wurde.

## D. Genehmigungsphase {#authorization-phase}

In der Autorisierungsphase soll der Streaming-Anwendung die Möglichkeit gegeben werden, die von den Benutzenden angeforderten Ressourcen abzuspielen, nachdem ihre Rechte bei der MVPD validiert wurden.

Die Autorisierungsphase ist obligatorisch. Die Streaming-Anwendung kann diese Phase nicht überspringen, wenn sie die von der Benutzerin oder dem Benutzer angeforderten Ressourcen wiedergeben möchte, da sie bzw. er bei der MVPD überprüfen muss, ob die Benutzerin oder der Benutzer berechtigt ist, bevor der Stream freigegeben wird.

+++Verwandte Artikel

APIs

* [Abrufen von Autorisierungsentscheidungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Flows

* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Häufig gestellte Fragen

* [Häufig gestellte Fragen zur Genehmigungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Schritt 6: Prüfen auf autorisierte Ressourcen {#step-6-check-for-authorized-resources}

* **Autorisierungsentscheidung abrufen:** Der Programmierer-Service ruft die Autorisierungsentscheidung für eine bestimmte Ressource ab, die von der Streaming-App übergeben wird, indem er den Endpunkt [**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) aufruft.

   * Der Programmierdienst ist nicht erforderlich, um Autorisierungsentscheidungen in einem persistenten Speicher zu speichern.

   * Der Programmierer-Service kann den Grund für eine Entscheidung bezüglich der verweigerten Autorisierung ermitteln, indem er den [Fehlercode und die Meldung) prüft](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) die in der Antwort vom Endpunkt Decisions Authorize enthalten sind. Diese Details geben insight den spezifischen Grund an, aus dem die Autorisierungsanfrage abgelehnt wurde. So kann das Benutzererlebnis oder der Trigger über die erforderliche Verarbeitung in der Streaming-App informiert werden. Stellen Sie sicher, dass ein zum Abrufen von Autorisierungsentscheidungen implementierter Wiederholungsmechanismus nicht zu einer Endlosschleife führt, wenn die Autorisierungsentscheidung abgelehnt wird. Erwägen Sie, weitere Zustellversuche auf eine angemessene Anzahl zu beschränken und Ablehnungen elegant zu handhaben, indem Sie dem Benutzer klares Feedback senden.

   * Der Programmierer-Service kann andere Geschäftsregeln bewerten und eine geeignete Autorisierungsentscheidung an die Streaming-App zurückgeben.

   * Der Programmierer-Service ist nicht erforderlich, um ein abgelaufenes Medien-Token zu aktualisieren, während der Stream aktiv wiedergegeben wird. Wenn das Medien-Token während der Wiedergabe abläuft, sollte der Stream ununterbrochen fortgesetzt werden können. Der Client muss jedoch beim nächsten Versuch, eine Ressource abzuspielen, eine neue Autorisierungsentscheidung anfordern und ein neues Medien-Token abrufen.

   * Der Programmierer-Service kann in einer einzelnen API-Anfrage, in der Regel bis zu 1, eine Autorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen erhalten, da die MVPDs Bedingungen festlegen.

## E. Abmeldephase {#logout-phase}

In der Abmeldephase soll der Streaming-Anwendung die Möglichkeit gegeben werden, das authentifizierte Benutzerprofil innerhalb der Adobe Pass-Authentifizierung auf Benutzeranfrage zu beenden.

Die Abmeldephase ist obligatorisch. Die Streaming-Anwendung muss dem Benutzer die Möglichkeit zum Abmelden bieten.

+++Verwandte Artikel

APIs

* [Initiieren des Abmeldens für bestimmte MVPDs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

Flows

* [Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

Häufig gestellte Fragen

* [Häufig gestellte Fragen zur Abmeldephase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Schritt 7: Abmelden {#step-7-logout}

* Adobe Pass-Abmeldung initiieren: Der Programmierer-Service initiiert den Abmeldefluss wie von der Streaming-App angefordert, indem er den Endpunkt [/api/v2/{serviceProvider}/logout/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) aufruft.

   * Der Programmierdienst kann alle gespeicherten Informationen über den authentifizierten Benutzer bereinigen.

   * Der Programmierdienst muss den Anweisungen in den `actionName`- und `actionType`-Attributen der Antwort des Abmeldeendpunkts folgen, um sicherzustellen, dass der Abmeldevorgang korrekt abgeschlossen wird.

      * Wenn das `actionType` Attribut in der Antwort auf „interaktiv“ festgelegt ist, muss der Programmierer-Service den Wert des `url` Attributs an die Streaming-App zurückgeben.

         * **Szenario 1:** Die Streaming-App kann einen Browser oder eine Webansicht öffnen, daher muss die Abmelde-`url` geladen werden.

         * **Szenario 2:** Die Streaming-App kann keinen Browser öffnen. Daher kann der Abmeldevorgang angehalten werden, da die MVPD-Sitzung nicht in einem Browser-Cache für Streaming-Geräte gespeichert wurde.
