---
title: REST API V2-Cookbook (Client-zu-Server)
description: REST API V2-Cookbook (Client-zu-Server)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 0%

---

# REST API V2-Cookbook (Client-zu-Server) {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST-API-V2-Implementierung ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

Das Dokument richtet sich an Entwicklerinnen und Entwickler, die die [Adobe Pass Authentication REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) in ihre Streaming-Anwendungen mit einer Client-zu-Server-Architektur (C2S) integrieren.

## Voraussetzungen {#prerequisites}

Definitionen hierzu finden Sie im Glossar zur [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md).

Informationen zu obligatorischen Anforderungen und empfohlenen Vorgehensweisen finden Sie in der Dokumentation [REST API V2 Checklist](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) .

Häufig gestellte Fragen finden Sie in der Dokumentation [Häufig gestellte Fragen zur REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) .

## A. Phase der Registrierung {#registration-phase}

In der Registrierungsphase wird die Streaming-Anwendung über den DCR-Prozess (Dynamic Client Registration) für die Adobe Pass-Authentifizierung registriert.

Für den Prozess der dynamischen Client-Registrierung (DCR) muss die Streaming-Anwendung ein Paar von Client-Anmeldeinformationen abrufen und ein Zugriffstoken als Endziel der Registrierungsphase abrufen.

Die Registrierungsphase ist obligatorisch, aber die Streaming-Anwendung kann diese Phase überspringen, wenn sie ein zwischengespeichertes Paar von Client-Anmeldeinformationen und ein Zugriffstoken hat, die noch gültig sind.

+++Verwandte Artikel

**APIs:**

* [Abrufen von Client-Anmeldeinformationen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Zugriffstoken abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flows:**

* [Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQs:**

* [Häufig gestellte Fragen zur Registrierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Schritt 1: Registrieren des Programms {#step-1-register-your-application}

* **Client-Anmeldeinformationen abrufen:** Die Streaming-Anwendung ruft Client-Anmeldeinformationen durch Aufruf des Endpunkts [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) ab.

   * Die Streaming-Anwendung muss die Client-Anmeldeinformationen speichern und sie unbegrenzt verwenden, wenn ein Zugriffs-Token abgerufen werden muss.


* **Zugriffs-Token abrufen:** Die Streaming-Anwendung ruft das Zugriffs-Token durch Aufruf des Endpunkts [**/o/client/**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) ab.

   * Die Streaming-Anwendung muss das Zugriffstoken speichern und verwenden, bis es abläuft. Dann muss es verworfen und ein neues abgerufen werden.

## B. Authentifizierungsphase {#authentication-phase}

Der Zweck der Authentifizierungsphase besteht darin, der Streaming-Anwendung die Möglichkeit zu geben, die Identität des Benutzers zu überprüfen und Benutzermetadaten-Informationen abzurufen.

Die Authentifizierungsphase dient als erforderlicher Schritt für die Vorautorisierungsphase oder Autorisierungsphase, wenn die Streaming-Anwendung Inhalte wiedergeben muss.

+++Verwandte Artikel

**APIs**

* [Authentifizierungssitzung erstellen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Authentifizierungssitzung fortsetzen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Authentifizierungssitzung abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Authentifizierung im Benutzeragenten durchführen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Profile abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Abrufen eines Profils für ein bestimmtes MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Abrufen eines Profils für einen bestimmten Code](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**Flows**

* [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundlegender Authentifizierungsfluss innerhalb der sekundären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**FAQs**

* [Häufig gestellte Fragen zur Authentifizierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Schritt 2: Überprüfen auf vorhandene authentifizierte Profile {#step-2-check-for-existing-authenticated-profiles}

* **Profile abrufen:** Streaming-Anwendung sucht nach vorhandenen Profilen, indem der Endpunkt [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) aufgerufen wird.


* **Szenario 1:** Profile vorhanden sind, kann die Streaming-Anwendung in die Phase [Vorabautorisierung](#preauthorization-phase) oder [Autorisierung](#authorization-phase).


* **Szenario 2:** Es sind keine vorhandenen Profile vorhanden. Die Streaming-Anwendung kann mit dem nächsten Schritt fortfahren, um den [ zu authentifizieren](#step-3-authenticate-the-user).


* **Szenario 3:** Es sind keine vorhandenen Profile vorhanden. Die Streaming-Anwendung kann fortfahren, dem Benutzer über die Funktion [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) temporären Zugriff bereitzustellen.

   * Dieses Szenario würde den Rahmen dieses Dokuments sprengen. Weitere Informationen finden Sie in der Dokumentation [Temporärer ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)&quot;.

### Schritt 3: Benutzer authentifizieren {#step-3-authenticate-the-user}

* **Konfiguration abrufen:** Die Streaming-Anwendung ruft die Liste der verfügbaren MVPDs ab, indem sie den Endpunkt [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) aufruft.

   * Die Streaming-Anwendung kann einen benutzerdefinierten Filtermechanismus implementieren, um die Liste der MVPDs aus der Konfigurationsantwort zu verfeinern und nur die vorgesehenen Anbieter anzuzeigen, während andere ausgeblendet werden (z. B. MVPDs in Entwicklung, MVPDs testen, TempPass). Dadurch wird sichergestellt, dass Benutzenden bei der Auswahl ihres TV-Anbieters eine kuratierte Auswahl angezeigt wird.


* **Authentifizierungssitzung erstellen:** Die Streaming-Anwendung initiiert eine Authentifizierungssitzung, indem sie den Endpunkt [**/api/v2/{serviceProvider}/sessions aufruft**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).


* **Szenario 1:** Die Streaming-Anwendung kann einen Browser oder eine Webansicht öffnen, daher muss sie die `url` laden.

   * Der Benutzer gibt seinen Benutzernamen und sein Kennwort auf der Anmeldeseite von MVPD ein. Nach erfolgreicher Authentifizierung zeigt die endgültige Umleitung eine Erfolgsseite an.


* **Szenario 2:** Die Streaming-Anwendung kann keinen Browser öffnen, daher muss sie die `code` anzeigen. Eine separate Web-Anwendung ist erforderlich, um den Benutzer zur Eingabe der `code` aufzufordern, die `url` zu erstellen und zu öffnen: [**/api/v2/Authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * Der Benutzer gibt seinen Benutzernamen und sein Kennwort auf der Anmeldeseite von MVPD ein. Nach erfolgreicher Authentifizierung zeigt die endgültige Umleitung eine Erfolgsseite an.

### Schritt 4: Prüfen auf authentifizierte Profile {#step-4-check-for-authenticated-profiles}

* **Profil für bestimmten Code abrufen:** Die Streaming-Anwendung muss einen Abrufmechanismus mithilfe des `code` implementieren, um zu überprüfen, ob das Profil erfolgreich generiert und gespeichert wurde, indem der Endpunkt [**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) aufgerufen wird.

   * Die Streaming-Anwendung muss **Abrufmechanismus** den folgenden Bedingungen starten:

      * **Authentifizierung innerhalb der primären (Bildschirm-)Anwendung durchgeführt:** Die primäre (Streaming-)Anwendung sollte beginnen, den Abruf durchzuführen, wenn die Benutzerin oder der Benutzer die endgültige Zielseite erreicht, nachdem die Browser-Komponente die URL geladen hat, die für den `redirectUrl`-Parameter in der Endpunktanforderung [Sitzungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) angegeben ist.

      * **Authentifizierung in einer sekundären (Bildschirm-)Anwendung:** Die primäre (Streaming-)Anwendung sollte beginnen, den Abruf durchzuführen, sobald der Benutzer den Authentifizierungsprozess einleitet - direkt nach Erhalt der [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)-Endpunktantwort und Anzeige des Authentifizierungs-Codes für den Benutzer.

   * Die Streaming-Anwendung muss **Abrufmechanismus** folgenden Bedingungen stoppen:

      * **Erfolgreiche Authentifizierung:** Die Profilinformationen des Benutzers wurden erfolgreich abgerufen und bestätigen seinen Authentifizierungsstatus. Zu diesem Zeitpunkt ist keine Abfrage mehr erforderlich.

      * **Authentifizierungssitzung und Code-Ablauf:** Die Authentifizierungssitzung und der Code laufen ab, wie durch den `notAfter` Zeitstempel (z. B. 30 Minuten) in der Endpunktantwort [Sitzungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) angegeben. In diesem Fall muss der Benutzer den Authentifizierungsprozess neu starten, und das Abrufen mit dem vorherigen Authentifizierungs-Code sollte sofort gestoppt werden.

      * **Generierter neuer Authentifizierungs-Code:** Wenn der Benutzer einen neuen Authentifizierungs-Code auf dem Primärgerät (Bildschirm) anfordert, ist die vorhandene Sitzung nicht mehr gültig und das Abrufen mit dem vorherigen Authentifizierungs-Code sollte sofort gestoppt werden.

   * Die Streaming-Anwendung muss **Abrufintervall konfigurieren** unter den folgenden Bedingungen:

      * **Authentifizierung wird innerhalb der primären (Bildschirm-)Anwendung durchgeführt:** Die primäre (Streaming-)Anwendung sollte alle 3-5 Sekunden oder länger eine Abfrage durchführen.

      * **Authentifizierung in einer sekundären (Bildschirm-)Anwendung:** Die primäre (Streaming-)Anwendung sollte alle 3-5 Sekunden oder länger abfragen.

   * Die Streaming-Anwendung sollte Teile der Profilinformationen des Benutzers in einem beständigen Speicher zwischenspeichern, um unnötige Anfragen zu vermeiden und das Benutzererlebnis zu verbessern.

## C. (fakultativ) Phase der Vorabgenehmigung {#preauthorization-phase}

Der Zweck der Vorautorisierungsphase besteht darin, der Streaming-Anwendung die Möglichkeit zu geben, eine Untergruppe von Ressourcen aus ihrem Katalog darzustellen, auf die der Benutzer zugreifen könnte.

Die Vorautorisierungsphase kann das Benutzererlebnis verbessern, wenn der Benutzer die Streaming-Anwendung zum ersten Mal öffnet oder zu einem neuen Abschnitt navigiert.

Die Phase der Vorabautorisierung ist nicht obligatorisch. Die Streaming-Anwendung kann diese Phase überspringen, wenn sie einen Katalog von Ressourcen präsentieren möchte, ohne sie zuerst auf der Grundlage der Benutzerberechtigung zu filtern.

+++Verwandte Artikel

**APIs**

* [Abrufen von Entscheidungen vor der Autorisierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flows**

* [Grundlegender Vorautorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**FAQs**

* [Häufig gestellte Fragen zur Vorautorisierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Schritt 5: Prüfen auf vorautorisierte Ressourcen {#step-5-check-for-preauthorized-resources}

* **Vorabautorisierungsentscheidungen abrufen:** Die Streaming-Anwendung ruft Vorabautorisierungsentscheidungen für eine Liste von Ressourcen ab, indem der Endpunkt [**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) aufgerufen wird.

   * Die Streaming-Anwendung ist nicht erforderlich, um Entscheidungen vor der Autorisierung in einem persistenten Speicher zu speichern. Es wird jedoch empfohlen, Zulassungsentscheidungen im Speicher zwischenzuspeichern, um das Benutzererlebnis zu verbessern. Dadurch werden unnötige Aufrufe von Ressourcen vermieden, die bereits vorab autorisiert wurden, die Latenz verringert und die Leistung verbessert.

   * Die Streaming-Anwendung kann den Grund für eine abgelehnte Vorautorisierungsentscheidung ermitteln, indem sie den [Fehlercode und die Nachricht) überprüft](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) die in der Antwort vom Vorautorisierungs-Endpunkt von Decisions enthalten sind. Diese Details geben insight den spezifischen Grund an, aus dem die Vorabautorisierungsanfrage abgelehnt wurde. So kann das Benutzererlebnis oder der Trigger über die erforderliche Verarbeitung in der Anwendung informiert werden. Stellen Sie sicher, dass ein Wiederholungsmechanismus, der zum Abrufen von Entscheidungen vor der Autorisierung implementiert ist, nicht zu einer Endlosschleife führt, wenn die Entscheidung vor der Autorisierung abgelehnt wird. Erwägen Sie, weitere Zustellversuche auf eine angemessene Anzahl zu beschränken und Ablehnungen elegant zu handhaben, indem Sie dem Benutzer klares Feedback senden.

   * Aufgrund von Bedingungen, die von MVPDs auferlegt werden, kann die Streaming-Anwendung in einer einzigen API-Anfrage eine Vorabautorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen erhalten, in der Regel bis zu 5. Diese maximale Anzahl von Ressourcen kann angezeigt und geändert werden, nachdem eine Vereinbarung mit MVPDs über das Adobe Pass [TVE-Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) von einem Ihrer Organisationsadministratoren oder einem in Ihrem Namen handelnden Adobe Pass-Authentifizierungsmitarbeiter getroffen wurde.


## D. Genehmigungsphase {#authorization-phase}

In der Autorisierungsphase soll der Streaming-Anwendung die Möglichkeit gegeben werden, die von den Benutzenden angeforderten Ressourcen abzuspielen, nachdem ihre Rechte bei der MVPD validiert wurden.

Die Autorisierungsphase ist obligatorisch. Die Streaming-Anwendung kann diese Phase nicht überspringen, wenn sie die von der Benutzerin oder dem Benutzer angeforderten Ressourcen wiedergeben möchte, da sie bzw. er bei der MVPD überprüfen muss, ob die Benutzerin oder der Benutzer berechtigt ist, bevor der Stream freigegeben wird.

+++Verwandte Artikel

**APIs**

* [Abrufen von Autorisierungsentscheidungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flows**

* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**FAQs**

* [Häufig gestellte Fragen zur Genehmigungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Schritt 6: Prüfen auf autorisierte Ressourcen {#step-6-check-for-authorized-resources}

* **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung ruft die Autorisierungsentscheidung für eine bestimmte Ressource ab, indem sie den Endpunkt [**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) aufruft.

   * Die Streaming-Anwendung ist nicht erforderlich, um Autorisierungsentscheidungen in einem persistenten Speicher zu speichern.

   * Die Streaming-Anwendung kann den Grund für eine Entscheidung über die verweigerte Autorisierung ermitteln, indem sie den [Fehlercode und die Nachricht](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) überprüft, die in der Antwort des Endpunkts Decisions Authorize enthalten sind. Diese Details geben insight den spezifischen Grund an, aus dem die Autorisierungsanfrage abgelehnt wurde. So kann das Benutzererlebnis oder der Trigger über die erforderliche Verarbeitung in der Anwendung informiert werden. Stellen Sie sicher, dass ein zum Abrufen von Autorisierungsentscheidungen implementierter Wiederholungsmechanismus nicht zu einer Endlosschleife führt, wenn die Autorisierungsentscheidung abgelehnt wird. Erwägen Sie, weitere Zustellversuche auf eine angemessene Anzahl zu beschränken und Ablehnungen elegant zu handhaben, indem Sie dem Benutzer klares Feedback senden.

   * Die Streaming-Anwendung ist nicht erforderlich, um ein abgelaufenes Medien-Token zu aktualisieren, während der Stream aktiv wiedergegeben wird. Wenn das Medien-Token während der Wiedergabe abläuft, sollte der Stream ununterbrochen fortgesetzt werden können. Der Client muss jedoch beim nächsten Versuch, eine Ressource abzuspielen, eine neue Autorisierungsentscheidung anfordern und ein neues Medien-Token abrufen.

   * Aufgrund von Bedingungen, die von MVPDs auferlegt werden, kann die Streaming-Anwendung in einer einzelnen API-Anfrage, normalerweise bis zu 1, eine Autorisierungsentscheidung für eine begrenzte Anzahl von Ressourcen erhalten.

## E. Abmeldephase {#logout-phase}

In der Abmeldephase soll der Streaming-Anwendung die Möglichkeit gegeben werden, das authentifizierte Benutzerprofil innerhalb der Adobe Pass-Authentifizierung auf Benutzeranfrage zu beenden.

Die Abmeldephase ist obligatorisch. Die Streaming-Anwendung muss dem Benutzer die Möglichkeit zum Abmelden bieten.

+++Verwandte Artikel

**APIs**

* [Initiieren des Abmeldens für bestimmte MVPDs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flows**

* [Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**FAQs**

* [Häufig gestellte Fragen zur Abmeldephase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Schritt 7: Abmelden {#step-7-logout}

* **Adobe Pass-Abmeldung initiieren:** Die Streaming-Anwendung initiiert den Abmeldefluss, indem sie den Endpunkt [**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) aufruft.

   * Die Streaming-Anwendung muss den Anweisungen in den `actionName` und `actionType` Attributen der Antwort des Abmeldeendpunkts folgen, um sicherzustellen, dass der Abmeldevorgang korrekt abgeschlossen wird.

      * Wenn das `actionType` Attribut in der Antwort auf „interaktiv“ festgelegt ist:

         * **Szenario 1:** Die Streaming-Anwendung kann einen Browser oder eine Webansicht öffnen, daher muss sie die Abmelde-`url` laden.

         * **Szenario 2:** Die Streaming-Anwendung kann keinen Browser öffnen. Daher kann der Abmeldevorgang angehalten werden, da die MVPD-Sitzung nicht in einem Browser-Cache für Streaming-Geräte beibehalten wurde.
