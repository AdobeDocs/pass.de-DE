---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass-Authentifizierung
user-guide-description: Die Adobe Pass-Authentifizierung ist eine Berechtigungslösung für TV Everywhere. Sie bietet ein modulares Framework, mit dem festgestellt werden kann, ob eine Person, die Zugriff auf eine Ressource anfordert, dazu berechtigt ist.
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 3%

---


# Hilfe zur Adobe Pass-Authentifizierung {#authentication}

+ [Adobe Pass-Authentifizierung](home.md)
+ Schnellstart {#kickstart}
   + [Fachpapier](kickstart/technical-paper.md)
   + [Übersicht über Programmierer](kickstart/programmer-overview.md)
   + [MVPD-Übersicht](kickstart/mvpd-overview.md)
   + [Handbuch zum Schnellstart für Programmierer](kickstart/programmer-kickstart-guide.md)
   + [MVPD-Schnellstartanleitung](kickstart/mvpd-kickstart-guide.md)
   + [Eskalationsverfahren](notes-technical/escalation-procedures.md)
   + [Glossar](kickstart/glossary.md)
+ Integrationsleitfaden für Programmierer {#integration-guide-programmers}
   + REST-APIs {#rest-apis}
      + REST API DCR {#rest-api-dcr}
         + [Übersicht über die dynamische Kundenregistrierung](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + APIs {#rest-api-dcr-apis}
            + [Abrufen von Client-Anmeldeinformationen](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [Zugriffstoken abrufen](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + Flüsse {#rest-api-dcr-flows}
            + [Dynamischer Client-Registrierungsfluss](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + REST API V2 {#rest-api-v2}
         + [REST API V2 - Überblick](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [REST API V2-Glossar](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [Häufig gestellte Fragen zur REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + APIs {#rest-api-v2-apis}
            + [Übersicht über REST API V2-APIs](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + Konfiguration {#rest-api-v2-configuration-apis}
               + [Konfiguration für bestimmte Dienstleister abrufen](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sitzungen {#rest-api-v2-sessions-apis}
               + [Erstellen einer Authentifizierungssitzung](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [Authentifizierungssitzung fortsetzen](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [Abrufen einer Authentifizierungssitzung](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [Authentifizierung im Benutzeragenten durchführen](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + Profile {#rest-api-v2-profiles-apis}
               + [Profile abrufen](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [Profil für bestimmte mvpd abrufen](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [Profil für bestimmten Code abrufen](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + Entscheidungen {#rest-api-v2-decisions-apis}
               + [Abrufen von Autorisierungsentscheidungen mit einer bestimmten mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [Abrufen von Vorab-Autorisierungsentscheidungen mit einer bestimmten mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + Abmelden {#rest-api-v2-logout-apis}
               + [Initiieren der Abmeldung für bestimmte mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + Single Sign-On für Partner {#rest-api-v2-partner-single-sign-on-apis}
               + [Anfrage zur Partnerauthentifizierung abrufen](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [Profil mithilfe der Antwort auf die Partnerauthentifizierung abrufen](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + Flüsse {#rest-api-v2-flows}
            + [REST API V2-Flüsse - Überblick](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + Grundlegende Zugriffsflüsse {#rest-api-v2-basic-access-flows}
               + [Durchfluss von Standardprofilen innerhalb der primären Anwendung](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [Durchgang der einfachen Profile, der in der sekundären Anwendung ausgeführt wird](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [Grundlegender Authentifizierungsfluss innerhalb der primären Anwendung](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [Grundlegender Authentifizierungsfluss, der in der sekundären Anwendung ausgeführt wird](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [Grundlegender Autorisierungsfluss innerhalb der Hauptanwendung](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [Grundlegender Ablauf der Vorautorisierung innerhalb der Hauptanwendung](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [Grundlegender Abmeldefluss innerhalb der primären Anwendung](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + Beschädigte Zugriffsflüsse {#rest-api-v2-degraded-access-flows}
               + [Verringerte Zugriffsflüsse](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + Temporärer Zugriffsfluss {#rest-api-v2-temporary-access-flows}
               + [Temporäre Zugriffsflüsse](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + Einmal-Anmeldungs-Zugriffsfluss {#rest-api-v2-single-sign-on-access-flows}
               + [Single Sign-on mit Partnerflüssen](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [Single Sign-on mit Platform-Identitätsflüssen](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [Single Sign-on mit Service-Token-Flüssen](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [Einmaliger Abmeldefluss](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + Cookbooks {#rest-api-v2-cookbooks}
            + [REST API V2-Cookbook (Client-to-Server)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [REST API V2-Cookbook (Server-zu-Server)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + Anhang {#rest-api-v2-appendix}
            + Kopfzeilen {#rest-api-v2-appendix-headers}
               + [Kopfzeile - Autorisierung](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [Header - AP-Device-Identifier](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [Header - X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [Header - AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [Header - Adobe-Subject-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [Header - AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [Header - AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + Standardfunktionen {#standard-features}
      + Berechtigungen {#entitlements}
         + [Identifizieren geschützter Ressourcen](integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)
         + [Vorabgenehmigung](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [Integration des Media Token Verifier](integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)
         + [Benutzermetadaten](integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)
         + [Benutzer-Metadatenzertifikat für Verschlüsselung](integration-guide-programmers/features-standard/entitlements/user-metadata-certificate.md)
      + Fehlerberichte {#error-reporting}
         + [Erweiterte Fehlercodes](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
         + [Fehlerberichte](integration-guide-programmers/features-standard/error-reporting/error-reporting.md)
      + Single-Sign-On-Zugriff {#sso-access}
         + Single Sign-On für Partner {#partner-sso}
            + Apple Single Sign-On {#apple-sso}
               + [Überblick über Apple SSO](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Apple SSO-Cookbook (REST API V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
               + [Apple SSO-Cookbook (REST API V1)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v1.md)
               + [Apple SSO-Cookbook (iOS/tvOS SDK)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-iostvos-sdk.md)
         + Single-Sign-On für Platform {#platform-sso}
            + Amazon Single Sign-On {#amazon-sso}
               + [Amazon SSO-Cookbook (REST API V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
               + [Amazon SSO-Cookbook (REST API V1)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
            + Roku Single Sign-On {#roku-sso}
               + [Roku SSO-Übersicht](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
         + [Unterstützung für Single Sign-On](integration-guide-programmers/features-standard/sso-access/sso-support.md)
         + [SSO über passive Authentifizierung](integration-guide-programmers/features-standard/sso-access/sso-passive-authn.md)
      + Zugriff auf Home-basierte Authentifizierung {#hba-access}
         + [Hausbasierte Authentifizierung für TV überall](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [HBA-Status für MVPDs](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + Datenschutzunterstützung {#privacy-support}
         + [Übersicht über den Datenschutzsupport](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [Datenschutzanfrage stellen](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Premium-Funktionen {#features-premium}
      + Temporärer Zugriff {#temporary-access}
         + [Temporärer Übergang](integration-guide-programmers/features-premium/temporary-access/temp-pass.md)
         + [vorübergehender Promotionsübergang](integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
         + [Zurücksetzen des Vorübergangs](integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
      + Beschädigter Zugriff {#degraded-access}
         + [Übersicht über die Abbau-API](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + ESM {#esm}
         + [Übersicht über die Überwachung des Berechtigungsdienstes](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [API zur Überwachung des Entitätsdienstes](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [Serverseitige Metriken](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analytics {#analytics}
         + [Integrieren von serverseitigen Adobe Pass-Authentifizierungsdaten in Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Verwenden der Experience Cloud-ID in der Adobe Pass-Authentifizierung](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + Legacy {#legacy}
      + (Legacy) REST API V1 {#rest-api-v1}
         + [REST API V1 - Überblick](integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
         + [REST API V1-Referenz](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + (Veraltet) APIs {#rest-api-v1-apis}
            + [Registrierungs-Code-Anfrage](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [Registrierungsdatensatz](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [Registrierungsdatensatz löschen](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [MVPD-Liste bereitstellen](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [Authentifizierung initiieren](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [Authentifizierungstoken überprüfen](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [Authentifizierungstoken abrufen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [Autorisierung initiieren](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [Autorisierungstoken abrufen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [Short Media Token abrufen](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [Überprüfen des Authentifizierungsflusses nach Second Screen Web App](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [Abrufen einer Liste vorab autorisierter Ressourcen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [Abrufen einer Liste vorab autorisierter Ressourcen von der Second Screen Web App](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [Abmeldung starten](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [Benutzermetadaten](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [Profil-Anfrage abrufen](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [Token Exchange](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [Kostenlose Vorschau für den temporären Pass und den temporären Weiterleitungs-Pass](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + (Legacy) Cookbooks {#rest-api-v1-cookbooks}
            + [REST API V1-Cookbook (Client-to-Server)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [REST API V1-Cookbook (Server-zu-Server)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + (Veraltet) SDKs {#sdks}
         + (Veraltet) JavaScript SDK {#javascript-sdk}
            + [Übersicht über das JavaScript SDK](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [JavaScript SDK-Cookbook](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [JavaScript SDK-API-Referenz](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [Vorabautorisierung der JavaScript SDK-API](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + (Legacy) Richtlinien {#javascript-sdk-guidelines}
               + [Anmeldung ohne Aktualisierung und Abmeldung](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + (Veraltet) iOS/tvOS SDK {#ios-tvos-sdk}
            + [Übersicht über das iOS/tvOS-SDK](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [iOS/tvOS-SDK-Cookbook](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [iOS/tvOS SDK-API-Referenz](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [Vorabautorisierung der iOS/tvOS SDK-API](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + (Legacy) Richtlinien {#ios-tvos-sdk-guidelines}
               + [Registrierung von iOS/tvOS-Anwendungen](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [Migrationshandbuch für iOS/tvOS v3.x](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [iOS/tvOS-Speicherintegritätsprüfungen](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + (Veraltet) Android SDK {#android-sdk}
            + [Übersicht über das Android SDK](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [Android SDK-Cookbook](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [Android SDK-API-Referenz](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [Vorabautorisierung der Android SDK-API](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + (Legacy) Richtlinien {#android-sdk-guidelines}
               + [Android-Anwendungsregistrierung](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [Android SDK mit dynamischer Client-Registrierung](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + (Legacy) FireOS-SDK {#fireos-sdk}
            + [Technische Übersicht über Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [Amazon FireOS-Integrations-Cookbook](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [Amazon FireOS-API-Referenz](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [Amazon FireOS-Anwendungsregistrierung](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [FireOS-SDK mit dynamischer Client-Registrierung](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [Amazon FireOS SSO - Handbuch zum Programmstart](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
   + [Leitfaden zur Programmierintegration - Übersicht](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [Drosselmechanismus](integration-guide-programmers/throttling-mechanism.md)
   + [Mindestsystemanforderungen](integration-guide-programmers/minimum-system-requirements.md)
   + [Berechtigungsfluss des Programmierers](integration-guide-programmers/entitlement-flow.md)
   + [Anwendungsfälle für Programmierer](integration-guide-programmers/programmer-use-cases.md)
   + [Weitergeben von Client-Informationen (Gerät, Verbindung und Anwendung)](integration-guide-programmers/passing-client-information-device-connection-and-application.md)
+ Integrationsleitfaden für MVPDs {#integration-guide-mvpds}
   + [Integrationsfunktionen](integration-guide-mvpds/mvpd-integr-features.md)
   + [Authentifizierung](integration-guide-mvpds/authn-usecase.md)
   + [Authentifizierung mit dem OAuth 2.0-Protokoll](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Autorisierung](integration-guide-mvpds/authz-usecase.md)
   + [Vorabgenehmigung](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [MVPD-Abmeldung](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [Austausch von Inhaltsmetadaten](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [Austausch von Benutzermetadaten](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Proxy-MVPD-Webdienst](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Proxy-MVPD-SAML-Integration](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [Scoping für Dienstleister](integration-guide-mvpds/serv-provider-scoping.md)
   + [MVPD-Zulassungsadressen](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ Benutzerhandbuch für TVE Dashboard {#user-guide-tve-dashboard}
   + [Übersicht über TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [Umgebungen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [Änderungen überprüfen und pushen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [Kanäle](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [Programmierer](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPDs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [Integrationen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [Berichte](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [Änderungsprotokoll](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
   + [Tv Dashboard-Benutzerhandbuch](user-guide-tve-dashboard/tve-dashboard-user-guide.md)
+ Versionshinweise {#release-notes}
   + 2024 {#release-notes-2024}
      + [Versionshinweise zur Adobe Pass-Authentifizierung 3.0.3](notes-releases/auth-rn-303.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 3.0](notes-releases/auth-rn-300.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.70](notes-releases/auth-rn-270.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.69](notes-releases/auth-rn-269.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.7.0](notes-releases/authn-rn-javascript-470.md)
      + [Versionshinweise zu Adobe Pass Authentication iOS/tvOS 3.9.2](notes-releases/authn-rn-ios-tvos-392.md)
      + [Versionshinweise zu Adobe Pass Authentication iOS/tvOS 3.8.4](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 {#release-notes-2023}
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.68](notes-releases/auth-rn-268.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.67](notes-releases/auth-rn-267.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.66](notes-releases/auth-rn-266.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.65.1](notes-releases/auth-rn-2651.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.65](notes-releases/auth-rn-265.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.64.1](notes-releases/auth-rn-2641.md)
      + [Versionshinweise zu Adobe Pass Authentication iOS/tvOS 3.8.3](notes-releases/authn-rn-ios-tvos-383.md)
      + [Versionshinweise zu Adobe Pass Authentication iOS/tvOS 3.8.2](notes-releases/authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS/tvOS 3.8.1 - Versionshinweise](notes-releases/authn-rn-ios-tvos-381.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung Android 3.7.3](notes-releases/authn-rn-android-373.md)
   + 2022 {#release-notes-2022}
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.64](notes-releases/auth-rn-264.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.63](notes-releases/auth-rn-263.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung 2.62.1](notes-releases/auth-rn-2621.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.6.0](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#release-notes-2021}
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.4.0](notes-releases/authn-rn-javascript-440.md)
      + [Adobe Pass Authentication iOS/tvOS 3.7.0 - Versionshinweise](notes-releases/authn-rn-ios-tvos-370.md)
+ Technische Hinweise {#tech-notes}
   + Umgebungen {#tech-notes-environments}
      + [Grundlagen zu Adobe-Umgebungen](notes-technical/understanding-the-adobe-environments.md)
      + [Einrichten der Umgebung und Testen in der Pre-Qual-Phase](notes-technical/setting-up-your-environment-and-testing-in-prequal.md)
      + [Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Adobe API-Test-Site](notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + Benutzererlebnis {#tech-notes-user-experience}
      + [Migrieren der MVPD-Anmeldeseite von iFrame zum Popup](notes-technical/migr-mvpd-login-iframe-popup.md)
      + [Preflight-Funktion: Anleitung zum Aktivieren, Beheben oder Ermitteln des Problems](notes-technical/preflight-feature.md)
      + [MVPDs im Auswahldialogfeld zulassen](notes-technical/allow-mvpd-selectn-dialog.md)
      + [Verhindern, dass MVPDs im Auswahldialogfeld angezeigt werden](notes-technical/prevent-mvpd-selectn-dialog.md)
   + REST API V1 {#tech-notes-rest-api-v1}
      + [Clientlose API-Implementierung - Fehlercodes/Meldungen mit möglichen Gründen/Ursache](notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Clientloser API-Ablauf bei Fehlen der Geräte-ID](notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
      + [Clientlos: Vermeiden Sie die Verwendung von &quot;&amp;&quot;reg_code in /authenticate request](notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Aktivieren von Adobe Pass Entitlement Services für ein Programm in Xbox 360 und XboxOne Clientless](notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Clientloser Gerätetyp und Metriken](notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + SDKs {#tech-notes-sdks}
      + [Fragen und Antworten zu Zertifikaten](notes-technical/certificates-qa.md)
      + [Grundlegendes zu Benutzer-IDs](notes-technical/understanding-user-ids.md)
      + JavaScript SDK {#tech-notes-javascript-sdk}
         + [Tracking Prevention Assessment - Apple Safari](notes-technical/tracking-prevention-assessment-apple-safari.md)
         + [Tracking Prevention Assessment - Google Chrome](notes-technical/tracking-prevention-assessment-google-chrome.md)
         + [Cookie-Updates - SameSite- und Secure-Flags](notes-technical/cookies-updates-samesite-and-secure-flags.md)
         + [Tipps zum Debuggen](notes-technical/appendix-b-debugging-tips.md)
      + Android SDK {#tech-notes-android-sdk}
         + [Zugriff auf Enabler Android SDK Single Sign-On (SSO) in Android 10-Apps](notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass-Authentifizierung und das neue Android 6-Berechtigungsmodell &quot;Marshmallow&quot;](notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS-SDK {#tech-notes-ios-tvos-sdk}
         + [WKWebView-Unterstützung für iOS SDK 3.1+](notes-technical/wkwebview-support-on-ios-sdk-31.md)
         + [SFSafariViewController-Unterstützung für iOS SDK 3.2+](notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO in iOS bei Verwendung des Adobe Pass Authentication Access Enabler](notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden](notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Zurücksetzen des Temp-Übergangs auf iOS](notes-technical/reset-temp-pass-on-ios.md)
         + [Debugging des AccessEnabler iOS/tvOS-SDK mithilfe von Konsolen-App-Protokollen](notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0 Aktualisierungspfad](notes-technical/accessenabler-iostvos-370-upgrade-path.md)
   + Fehlerbehebung {#tech-notes-troubleshooting}
      + [Charles Proxy verwenden](notes-technical/using-charles-proxy.md)
      + [Überwachen von Adobe Pass Adobe PayTV-Pass](notes-technical/monitoring-adobe-pay-tv-pass.md)
