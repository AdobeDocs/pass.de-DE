---
title: Integrationshandbuch für Programmierer
description: Integrationshandbuch für Programmierer
exl-id: 51461caf-08ef-459e-b284-8f317f45e7b1
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2119'
ht-degree: 0%

---

# Integrationshandbuch für Programmierer {#programmer-integration-guide}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Dieses Integrationshandbuch richtet sich an Inhaltsanbieter (Programmierer), die eine Integration mit der Adobe® Pass-Authentifizierung planen.

In der heutigen digitalen Landschaft können Betrachter jederzeit und überall auf das Internet zugreifen und den Zugriff auf Ihre geschützten Inhalte anfordern. Vielleicht wollen sie eine einmalige Veranstaltung sehen oder die Rechte für eine ganze Fernsehserie, die sie gerade ausstrahlen, einholen.

Bevor Sie Zugriff auf geschützte Inhalte gewähren, müssen Sie feststellen, ob der Viewer dazu berechtigt ist. Zu den wichtigsten Fragen gehören:

* **Hat der Betrachter ein aktives Abonnement bei einem Mehrkanal-Videoprogrammierungsverteiler (MVPD)?**
* **Enthält dieses Abonnement Ihre Programmierung?**

## Adobe Pass-Authentifizierung für TV Everywhere {#adobe-pass-authentication-for-tv-everywhere}

Für Programmierer ist die Bestimmung von Berechtigungen nicht immer einfach. MVPDs sind die Verwahrer der Identifizierungsdaten und Zugriffsberechtigungen ihrer Kunden. Erschwerend kommt hinzu, dass Programmierer und Zuschauer eine Vielzahl von MVPDs abonnieren können, von denen jedes mit einzigartigen Systemen arbeitet. Aufgrund dieser Komplexität ist die Überprüfung von Berechtigungen sowohl technisch schwierig als auch ressourcenintensiv.

![Benutzerberechtigungen, die direkt vom Programmierer bestimmt werden](../assets/user-ent-by-progr.png){align="center"}

*Benutzerberechtigungen, die direkt vom Programmierer bestimmt werden*

Die Adobe Pass-Authentifizierung erleichtert auf sichere Weise Berechtigungstransaktionen zwischen Programmierern und MVPDs, wodurch die Bereitstellung geschützter Inhalte für geeignete Betrachter schnell, einfach und sicher ist.

![Benutzerberechtigung vermittelt durch Adobe Pass-Authentifizierung](../assets/user-ent-mediatedby-authn.png){align="center"}

*Benutzerberechtigung vermittelt durch Adobe Pass-Authentifizierung*

Die Adobe Pass-Authentifizierung dient als Proxy und erleichtert den Berechtigungsfluss zwischen Programmierern und MVPDs, indem sie sichere und konsistente Schnittstellen für beide Parteien bietet.

Für Programmierer stellt die Adobe Pass-Authentifizierung APIs als Teil einer **Standard**- oder **Premium**-Ebene bereit:

* Standard-APIs zur Adobe Pass-Authentifizierung:
   * [REST-API-DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST-API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium Adobe Pass-Authentifizierungs-APIs:
   * [Temporäre Pass-API zurücksetzen](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass-Funktion](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [Abbauungs-API](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [Abbaumerkmal](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [Berechtigungs-Service-Überwachungs-API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

### Anwendungsfälle {#use-cases}

In diesem Abschnitt werden die von der Adobe Pass-Authentifizierung unterstützten Anwendungsfälle für die Programmierer-Integration näher erläutert:

* Programmieranwendung (TVE) mit einem Einkanalnetz

  Dadurch kann der Programmierer den Zuschauern innerhalb einer TVE-Anwendung den Zugriff auf die Inhalte über ein Single-Branded-Kanalnetzwerk ermöglichen.

* Programmieranwendung (TVE) mit Mehrkanalnetzen

  Dadurch kann der Programmierer den Zuschauern innerhalb einer TVE-Anwendung Zugriff auf die Inhalte aus mehreren Kanalnetzen gewähren.

* Programmierprogramm (TVE) für spezielle Ereignisse

  Dadurch kann der Programmierer Betrachtern Zugriff auf den Inhalt spezieller Ereignisse gewähren, bei denen es sich möglicherweise nicht um Ressourcen handelt, die sich wie normale Kanäle in der MVPD-Berechtigungsdatenbank befinden.

| **Phase** | **Priorität** | **Anwendungsfall** | **Dokumente** |
|----------------------|--------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Authentifizierung** | **Hoch** | Authentifizierung | Weitere Informationen finden Sie in den Dokumenten, die im Abschnitt [Authentifizierungsphase](#authentication-phase) zusammengefasst sind. |
|                      | **Hoch** | Home-Based Authentication (HBA) | Weitere Informationen finden Sie unter [Home-Based Authentication](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md). |
|                      | **Hoch** | Single Sign-On (SSO) | Weitere Informationen finden Sie in den aggregierten Dokumenten im Abschnitt [Single Sign-On (SSO](#sso) . |
|                      | **Hoch** | MVPD auswählen | Weitere Informationen finden Sie in den Dokumenten, die im Abschnitt [Konfigurationsphase](#configuration-phase) zusammengefasst sind. |
|                      | **Medium** | Anmeldeseite für MVPD mit Markenbezeichnungen | Ermöglicht es MVPDs, Anmeldeseiten mit einem Branding zu versehen, das für den Programmierer oder Dienstleister spezifisch ist, einschließlich Unterstützung für standardmäßige Spracheinstellungen. |
|                      | **Hoch** | Konfigurieren von Time-to-Live (TTL)-Werten pro Plattform | Weitere Informationen finden Sie im Benutzerhandbuch [TVE Dashboard Integrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows). |
| **Vorautorisierung** | **Niedrig** | Vorabgenehmigung (Vorabgenehmigung) | Weitere Informationen finden Sie in den Dokumenten, die im Abschnitt [Vorabautorisierungsphase](#preauthorization-phase) zusammengefasst sind. |
|                      | **Medium** | Verbesserte Fehler-Codes | Weitere Informationen finden Sie unter [Erweiterte Fehler-Codes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
| **Autorisierung** | **Hoch** | Autorisierung | Weitere Informationen finden Sie in den Dokumenten, die im Abschnitt [Autorisierungsphase](#authorization-phase) zusammengefasst sind. |
|                      | **Hoch** | Einzelkanalautorisierung | Ermöglicht Benutzenden den Zugriff auf Inhalte aus mehreren Kanalnetzwerken innerhalb einer TVE-Anwendung. Programmierer können kanalspezifische Autorisierungsaufrufe ausführen, um die Berechtigung zu überprüfen. |
|                      | **Niedrig** | Autorisierung auf Asset-Ebene | Ermöglicht es MVPDs, während der Autorisierung detaillierte Analysen für einzelne Inhalts-Assets zu erfassen. |
|                      | **Medium** | Verbesserte Fehler-Codes | Weitere Informationen finden Sie unter [Erweiterte Fehler-Codes](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
|                      | **Hoch** | Programmer Federated Player - mit Autorisierung auf Seitenebene | Weitere Informationen finden Sie unter [Medien-Token](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Medium** | Programmer Federated Player - mit interner Player-Autorisierung | Weitere Informationen finden Sie unter [Medien-Token](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Hoch** | Syndicated Player - gehostet auf MVPD Portal mit Autorisierung auf Seitenebene | Weitere Informationen finden Sie unter [Medien-Token](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Niedrig** | Kindersicherung - Inhaltsbewertungen in Autorisierungsanfragen | Ermöglicht es dem Programmierer, Inhaltsbewertungen als Teil der Autorisierungsanfrage für die MVPD einzubeziehen, die für die Autorisierung auf Asset-Ebene nützlich sind. |
|                      | **Niedrig** | Kindersicherung - Content-Filter basierend auf Benutzerattributen | Ermöglicht es dem Programmierer, die für einen Benutzer maximal zulässige Inhaltsbewertung zu überprüfen und den verfügbaren Inhalt entsprechend zu filtern. |
| **Abmelden** | **Medium** | Abmelden | Weitere Informationen finden Sie in den Dokumenten, die im Abschnitt [Abmeldephase](#logout-phase) zusammengefasst sind. |

## Berechtigungsfluss {#entitlement-flow}

Der Berechtigungsfluss ist eine Reihe von Schritten, die ein Programmierprogramm (TVE) durchführen muss, um geschützte Inhalte zu streamen. Der Fluss besteht aus den folgenden Phasen:

* [Registrierungsphase](#registration-phase)
* [Konfigurationsphase](#configuration-phase)
* [Authentifizierungsphase](#authentication-phase)
* [(Optional) Phase vor der Autorisierung](#preauthorization-phase)
* [Autorisierungsphase](#authorization-phase)
* [Abmeldephase](#logout-phase)

Beim ersten Besuch eines Benutzers bei einem Programmierer-Programm (TVE) folgt der Berechtigungsfluss der beschriebenen Reihenfolge. Bei nachfolgenden Besuchen kann die Anwendung jedoch bestimmte Schritte umgehen, die vom Status der Registrierung oder Authentifizierung und den geltenden Anzeigerichtlinien abhängen.

Um eine detaillierte Untersuchung des Berechtigungsflusses und seiner Phasen zu erhalten, lesen Sie dieses Dokument weiter und lesen Sie anschließend die zugehörigen Cookbook-Handbücher, um weitere Einblicke zu erhalten:

* [REST API V2-Cookbook (Client-zu-Server)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
* [REST API V2-Cookbook (Server-zu-Server)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)

>[!NOTE]
>
> Programmer-Anwendung (TVE) wird in diesem Dokument verwendet, um kollektiv auf die Anwendungstypen zu verweisen, die auf verschiedenen Plattformen ausgeführt werden (Browser, Mobilgeräte, an Fernsehgeräte angeschlossene Geräte usw.), die von der Adobe Pass-Authentifizierung unterstützt werden.

### Registrierungsphase {#registration-phase}

In der Registrierungsphase wird die Client-Anwendung über den DCR-Prozess (Dynamic Client Registration[&#x200B; für die Adobe Pass-Authentifizierung &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Für den Prozess der dynamischen Client-Registrierung (DCR) muss die Client-Anwendung ein Paar von Client-Anmeldeinformationen abrufen und ein Zugriffstoken als Endziel der Registrierungsphase abrufen.

**APIs**

* [Abrufen von Client-Anmeldeinformationen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Zugriffstoken abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flows**

* [Dynamischer Client-Registrierungsfluss](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQs**

* [Häufig gestellte Fragen zur &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#registration-phase-faqs-general).

### Konfigurationsphase {#configuration-phase}

Die Konfigurationsphase dient dazu, der Client-Anwendung die Liste der MVPDs bereitzustellen, mit denen sie aktiv integriert ist, sowie die Konfigurationsdetails, die von der Adobe Pass-Authentifizierung für jede MVPD gespeichert wurden.

Die Konfigurationsphase dient als erforderlicher Schritt für die Authentifizierungsphase, wenn das Client-Programm den Benutzer bzw. die Benutzerin auffordern muss, seinen/ihren TV-Anbieter auszuwählen.

**APIs**

* [Abrufen der Konfiguration für einen bestimmten Dienstleister](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

**FAQs**

* [Häufig gestellte Fragen zur &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general).

>[!TIP]
>
> Die TV-Anwendung sollte eine MVPD-Auswahlschnittstelle enthalten, über die die Nutzer ihren Fernsehanbieter leicht identifizieren und auswählen können.

### Authentifizierungsphase {#authentication-phase}

In der Authentifizierungsphase soll die Client-Anwendung die Möglichkeit erhalten, die Identität des Benutzers mit der MVPD zu überprüfen und Informationen zu Benutzermetadaten abzurufen.

Die Authentifizierungsphase fungiert als erforderlicher Schritt für die Vorautorisierungsphase oder Autorisierungsphase, wenn die Client-Anwendung Inhalte wiedergeben muss.

Bei erfolgreicher Authentifizierung wird ein Profil generiert, das mit der Anwendung, dem Gerät und dem Dienstleister verknüpft ist und auch Informationen zu Benutzermetadaten enthält.

**Allgemeine Schritte**

In den folgenden Schritten werden die allgemeinen Schritte für den Fall einer SAML-Integration beschrieben:

1. **Laden der Anwendung (Website) des Programmierers**\
   Der/die Benutzende navigiert zur Programmieranwendung (Website), die die Adobe Pass-Authentifizierung ([-API V2) &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Geschützte Inhaltsanfrage**\
   Wenn Benutzende versuchen, auf geschützte Inhalte zuzugreifen, zeigt die Anwendung des Programmierers eine Liste von MVPDs an, aus denen die Benutzenden auswählen können.

1. **Initialisierung der Authentifizierungsanfrage**\
   Nach Auswahl von MVPD wird der Benutzer an einen Adobe Pass-Authentifizierungsserver umgeleitet. Hier wird im Falle einer SAML-Integration eine verschlüsselte SAML-Authentifizierungsanfrage für den ausgewählten MVPD generiert. Diese Anfrage wird im Namen des Programmierers an die MVPD gesendet. Je nach System von MVPD wird der Browser des Benutzers entweder zur Anmeldeseite von MVPD umgeleitet oder ein Anmelde-iFrame ist in die Anwendung des Programmierers eingebettet.

1. **MVPD-Anmeldung**\
   Der MVPD akzeptiert die Anfrage und stellt seine Anmeldeschnittstelle bereit, entweder über Umleitung oder iFrame.

1. **Benutzeranmeldung und -validierung**\
   Der Benutzer meldet sich mit seinen MVPD-Anmeldeinformationen an. MVPD überprüft den Abonnementstatus des Benutzers und richtet eine eigene HTTP-Sitzung ein.

1. **MVPD-Antwort auf Adobe Pass-Authentifizierung**\
   Nach Abschluss der Validierung generiert der MVPD eine SAML-Antwort (verschlüsselt) und sendet sie zurück an die Adobe Pass-Authentifizierung.

1. **Profilgenerierung**\
   Die Adobe Pass-Authentifizierung überprüft die SAML-Antwort, generiert ein Benutzerprofil, das zwischengespeichert wird, und leitet den Benutzer zurück zur Anwendung des Programmierers (Website).

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

>[!TIP]
>
> Die TVE-Anwendung sollte den Authentifizierungsstatus des Benutzers deutlich vermitteln, z. B. indem ihr MVPD-Logo neben „gesperrt“ oder „entsperrt“ angezeigt wird, um die Zugänglichkeit geschützter Inhalte anzuzeigen.

#### Single Sign-On (SSO) {#single-sign-on}

**APIs**

* [Partnerauthentifizierungsanfrage abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Erstellen und Abrufen eines Profils mithilfe der Antwort zur Partnerauthentifizierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**Flows**

* [Single Sign-on mit Partnerflüssen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
* [Single Sign-on mit Platform-Identitätsflüssen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
* [Single Sign-on mit Service-Token-Flüssen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)

### (Optional) Phase vor der Autorisierung {#preauthorization-phase}

Der Zweck der Vorautorisierungsphase besteht darin, der Client-Anwendung die Möglichkeit zu geben, eine Untergruppe von Ressourcen aus ihrem Katalog darzustellen, auf die der Benutzer zugreifen könnte.

Die Vorautorisierungsphase kann das Benutzererlebnis verbessern, wenn der Benutzer die Client-Anwendung zum ersten Mal öffnet oder zu einem neuen Abschnitt navigiert.

**APIs**

* [Abrufen von Entscheidungen vor der Autorisierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flows**

* [Grundlegender Vorautorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**FAQs**

* [Häufig gestellte Fragen zur Vorautorisierungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

>[!TIP]
>
> Die TVE-Anwendung sollte eingeschränkte Inhalte deutlich von autorisierten Inhalten unterscheiden, indem sie visuelle Indikatoren verwendet, z. B. ein „gesperrtes“ Symbol für eingeschränkte Inhalte und ein „entsperrtes“ Symbol für autorisierte Inhalte.

### Autorisierungsphase {#authorization-phase}

In der Autorisierungsphase soll der Client-Anwendung die Möglichkeit gegeben werden, die von den Benutzenden angeforderten Ressourcen abzuspielen, nachdem ihre Rechte bei der MVPD validiert wurden.

Bei erfolgreicher Autorisierung wird eine Entscheidung generiert, die auch ein Medien-Token enthält, das dem Programmierprogramm (TVE) zu Sicherheitszwecken bereitgestellt wird.

**Allgemeine Schritte**

Die folgenden Schritte beschreiben die allgemeinen Schritte:

1. **Umgang mit Ressourcenkennungen**\
   Der geschützte Inhalt wird durch eine [Ressourcenkennung“ identifiziert](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier) die eine einfache Zeichenfolge oder eine komplexere Struktur sein kann. Diese Kennung ist vordefiniert und wird vom Programmierer und der MVPD vereinbart. Die Anwendung des Programmierers sendet die Ressourcenkennung an die Adobe Pass-Authentifizierung [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **MVPD-Autorisierungsprüfung**\
   Der Adobe Pass-Authentifizierungsserver kommuniziert mithilfe standardisierter Protokolle mit dem Autorisierungsendpunkt von MVPD.

1. **MVPD-Antwort auf Adobe Pass-Authentifizierung**\
   Nach Abschluss der Validierung bestätigt der MVPD, dass der Benutzer berechtigt ist (oder nicht), auf den Inhalt zuzugreifen, und sendet eine Antwort zurück an die Adobe Pass-Authentifizierung.

1. **Generieren von Entscheidungs- und Medien-Token**\
   Die Adobe Pass-Authentifizierung überprüft die Antwort, generiert eine [Entscheidung](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) die zwischengespeichert wird und gibt die Entscheidung mit einem Medien-Token zurück an die Anwendung des Programmierers (Website).

1. **Überprüfung des Inhaltszugriffs**\
   Das Programm des Programmierers verwendet den [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) um zu bestätigen, dass der richtige Benutzer auf den richtigen Inhalt zugreift. Nach der Validierung erhält der Benutzer Zugriff, um die geschützten Inhalte anzuzeigen.

**APIs**

* [Abrufen von Autorisierungsentscheidungen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flows**

* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**FAQs**

* [Häufig gestellte Fragen zur Genehmigungsphase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

>[!TIP]
>
> Die TVE-Anwendung sollte eingeschränkte Inhalte deutlich von autorisierten Inhalten unterscheiden, indem sie visuelle Indikatoren verwendet, z. B. ein „gesperrtes“ Symbol für eingeschränkte Inhalte und ein „entsperrtes“ Symbol für autorisierte Inhalte.

### Abmeldephase {#logout-phase}

Die Abmeldephase soll der Client-Anwendung die Möglichkeit geben, das authentifizierte Benutzerprofil innerhalb der Adobe Pass-Authentifizierung auf Benutzeranfrage zu beenden.

**APIs**

* [Initiieren des Abmeldens für bestimmte MVPDs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flows**

* [Grundlegender Abmeldefluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**FAQs**

* [Häufig gestellte Fragen zur Abmeldephase](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

#### Single Logout (SLO) {#single-logout}

**Flows**

* [Fluss für einmaliges Abmelden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)

## Grundlegendes zu Berechtigungen {#understanding-entitlements}

Die Adobe Pass-Authentifizierungslösung umfasst die Erstellung von Berechtigungen - spezifische Datenelemente, die nach erfolgreichem Abschluss der Authentifizierungs- und Autorisierungs-Workflows generiert werden. Diese Berechtigungen gewähren Zugriff auf geschützte Inhalte, haben jedoch eine begrenzte Lebensdauer. Sobald eine Berechtigung abläuft, muss sie erneuert werden, indem die Authentifizierungs- oder Autorisierungsprozesse erneut initiiert werden.

Weitere Informationen zu Berechtigungen finden Sie in den folgenden Dokumenten:

* **Profile**

  Bei erfolgreicher Authentifizierung erstellt die Adobe Pass-Authentifizierung ein authentifiziertes Profil („langlebig„), das mit der ID der anfragenden Anwendung, des Geräts und des Dienstleisters (Anfordererkennung) verknüpft ist.

* **[Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Nach erfolgreicher Authentifizierung (und in einigen Fällen auch nach der Autorisierung) empfängt die Adobe Pass-Authentifizierung Benutzermetadaten von der MVPD, die sie für die anfragende Anwendung verfügbar machen.

* **[Entscheidungen](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung eine Autorisierungsentscheidung („langlebig„), die mit der anfragenden Anwendung, dem Gerät, der Kennung des Dienstleisters (Anfordererkennung) und einer bestimmten geschützten Ressource (Ressourcenkennung) verknüpft ist.

* **[Medien-Token](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Nach erfolgreicher Autorisierung erstellt die Adobe Pass-Authentifizierung ein Medien-Token („kurzlebig„), das mit einer erfolgreichen Wiedergabeanfrage verknüpft ist, und bietet Unterstützung für Best Practices der Branche zur Risikominderung (z. B. Stream-Ripping).

Die TTL-Werte (Time-to-Live) für Profile und Entscheidungen werden auf der Grundlage von Vereinbarungen zwischen Programmierern und Pay-TV-Anbietern festgelegt, die sich auf einen Wert einigen, der allen Beteiligten am besten dient.
