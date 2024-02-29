---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass-Authentifizierung
user-guide-description: Die Adobe Pass-Authentifizierung ist eine Berechtigungslösung für TV Everywhere. Sie bietet ein modulares Framework, mit dem festgestellt werden kann, ob eine Person, die Zugriff auf eine Ressource anfordert, dazu berechtigt ist.
source-git-commit: 579ce868b6ee94e1854bbc51145fc7840268db26
workflow-type: tm+mt
source-wordcount: '919'
ht-degree: 3%

---


# Hilfe zur Adobe Pass-Authentifizierung {#authentication}

+ [Übersicht über die Adobe Pass-Authentifizierung](home.md)
+ Adobe Pass-Authentifizierungskonzepte {#authentication-concepts}
   + [Fachpapier](technical-paper.md)
   + [Übersicht für Programmierer](programmer-overview.md)
   + [MVPD-Übersicht](mvpd-overview.md)
+ Schnellstartanleitungen {#kickstart-guides}
   + [Handbuch zum Schnellstart für Programmierer](programmer-kickstart-guide.md)
   + [MVPD-Schnellstartanleitung](mvpd-kickstart-guide.md)
+ Handbuch zur Programmierintegration {#programmer-integration-guide}
   + [Leitfaden zur Programmierintegration - Übersicht](programmer-integration-guide-overview.md)
   + [Berechtigungsfluss des Programmierers](entitlement-flow.md)
   + [Anwendungsfälle für Programmierer](programmer-use-cases.md)
   + [Weitergeben von Client-Informationen (Gerät, Verbindung und Anwendung)](passing-client-information-device-connection-and-application.md)
   + REST-API {#restapi}
      + [REST API - Übersicht](rest-api-overview.md)
      + [REST API-Cookbook (Server-zu-Server)](rest-api-cookbook-servertoserver.md)
      + [REST API-Cookbook (Client-to-Server)](rest-api-cookbook-clienttoserver.md)
      + REST API Reference {#rest-api-reference}
         + [REST-API-Referenz](rest-api-reference.md)
         + [Registrierungs-Code-Anfrage](registration-code-request.md)
         + [Registrierungsdatensatz](return-registration-record.md)
         + [Registrierungsdatensatz löschen](delete-registration-record.md)
         + [MVPD-Liste bereitstellen](provide-mvpd-list.md)
         + [Authentifizierung initiieren](initiate-authentication.md)
         + [Authentifizierungstoken überprüfen](check-authentication-token.md)
         + [Authentifizierungstoken abrufen](retrieve-authentication-token.md)
         + [Autorisierung initiieren](initiate-authorization.md)
         + [Autorisierungstoken abrufen](retrieve-authorization-token.md)
         + [Short Media Token abrufen](obtain-short-media-token.md)
         + [Überprüfen des Authentifizierungsflusses nach Second Screen Web App](check-authentication-flow-by-second-screen-web-app.md)
         + [Abrufen einer Liste vorab autorisierter Ressourcen](retrieve-list-of-preauthorized-resources.md)
         + [Abrufen einer Liste vorab autorisierter Ressourcen von der Second Screen Web App](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [Abmeldung starten](initiate-logout.md)
         + [Benutzermetadaten](user-metadata.md)
         + [Profil-Anfrage abrufen](retrieve-profilerequest.md)
         + [Token Exchange](token-exchange.md)
         + [Kostenlose Vorschau für den temporären Pass und den temporären Weiterleitungs-Pass](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript-SDK {#javascriptsdk}
         + [Übersicht über das JavaScript-SDK](javascript-sdk-overview.md)
         + [JavaScript SDK-Cookbook](javascript-sdk-cookbook.md)
         + [JavaScript-SDK-API-Referenz](javascript-sdk-api-reference.md)
         + Richtlinien {#js-sdk-guidelines}
            + [Anmeldung ohne Aktualisierung und Abmeldung](refreshless-login-and-logout.md)
         + JavaScript-API {#js-api}
            + [Vorautorisieren](js-preauthorize.md)
      + iOS/tvOS-SDK {#ios-sdk}
         + [Übersicht über das iOS/tvOS-SDK](iostvos-sdk-overview.md)
         + [iOS/tvOS-SDK-Cookbook](iostvos-sdk-cookbook.md)
         + [iOS/tvOS SDK-API-Referenz](iostvos-sdk-api-reference.md)
         + Richtlinien {#ios-tvos-sdk-guidelines}
            + [Registrierung von iOS/tvOS-Anwendungen](iostvos-application-registration.md)
            + Migrationsrichtlinien {#migration-guidelines}
               + [Migrationshandbuch für iOS/tvOS v3.x](iostvos-v3x-migration-guide.md)
            + [iOS/tvOS-Speicherintegritätsprüfungen](iostvos-sdk-storage-integrity-checks.md)
         + iOS/tvOS-API {#ios-tvos-api}
            + [Vorautorisieren](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Android SDK - Übersicht](android-sdk-overview.md)
         + [Android SDK Cookbook](android-sdk-cookbook.md)
         + [Android-SDK-API-Referenz](android-sdk-api-reference.md)
         + Richtlinien {#androidguidelines}
            + [Registrierung von Android-Anwendungen](android-application-registration.md)
            + [Android-SDK mit dynamischer Client-Registrierung](android-sdk-with-dynamic-client-registration.md)
         + Android-API{#androidapi}
            + [Vorautorisieren](preauthorize-android.md)
      + Amazon FireOS-SDK {#fireossdk}
         + [Amazon FireOS SSO - Handbuch zum Programmstart](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Amazon FireOS SSO mit clientless API-Cookbook](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Technische Übersicht über Amazon FireOS](amazon-fireos-technical-overview.md)
         + [Amazon FireOS-Integrations-Cookbook](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS-API-Referenz](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS-Anwendungsregistrierung](amazon-fireos-application-registration.md)
         + [FireOS-SDK mit dynamischer Client-Registrierung](fireos-sdk-with-dynamic-client-registration.md)
   + Platform SSO {#platform-sso}
      + APPLE SSO {#apple-sso}
         + [Überblick über Apple SSO](apple-sso-overview.md)
         + [Apple SSO-Cookbook (REST-API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO-Cookbook (iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + Inhaltsmetadaten {#content-metadata}
      + [Identifizieren geschützter Ressourcen](identify-protected-resources.md)
   + Integration von Inhaltsservern {#content-serv-int}
      + [Integration des Media Token Verifier](media-token-verifier-int.md)
   + Anhänge {#appendices}
      + [Tipps zum Debuggen](appendix-b-debugging-tips.md)
+ Handbuch zur MVPD-Integration {#mvpd-int-guide}
   + [Integrationsfunktionen](mvpd-integr-features.md)
   + [Authentifizierung](authn-usecase.md)
   + [Authentifizierung mit dem OAuth 2.0-Protokoll](authn-oauth2-protocol.md)
   + [Autorisierung](authz-usecase.md)
   + [Vorabgenehmigung](mvpd-preflight-authz.md)
   + [MVPD-Abmeldung](usecase-mvpd-logout.md)
   + [Austausch von Inhaltsmetadaten](mvpd-content-metadata-exchange.md)
   + [Austausch von Benutzermetadaten](mvpd-user-metadata-exchng.md)
   + [Proxy-MVPD-Webdienst](proxy-mvpd-webserv.md)
   + [Proxy-MVPD-SAML-Integration](proxy-mvpd-saml-int.md)
   + [Scoping für Dienstleister](serv-provider-scoping.md)
   + [MVPD-Zulassungsadressen](mvpd-listing-ip-addres.md)
+ Adobe Pass-Authentifizierungsfunktionen {#auth-features}
   + Adobe Analytics-Integration {#analytics-int}
      + [Integrieren von serverseitigen Adobe Pass-Authentifizierungsdaten in Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [Verwenden der Experience Cloud-ID in der Adobe Pass-Authentifizierung](exp-cloud-id-authn.md)
   + Überwachung des Berechtigungsdienstes {#entitlement-service-monitoring}
      + [Übersicht über die Überwachung des Berechtigungsdienstes](entitlement-service-monitoring-overview.md)
      + [API zur Überwachung des Entitätsdienstes](entitlement-service-monitoring-api.md)
      + [Serverseitige Metriken](understanding-serverside-metrics.md)
   + Temporärer Übergang {#temp-pass}
      + [Temporärer Übergang](temp-pass.md)
      + [vorübergehender Promotionsübergang](promotional-temp-pass.md)
      + [Zurücksetzen des Vorübergangs](reset-temp-pass.md)
   + Single Sign-On {#sso}
      + [Unterstützung für Single Sign-On](sso-support.md)
      + [SSO über passive Authentifizierung](sso-passive-authn.md)
   + Eigene Authentifizierung {#home-based-auth}
      + [Hausbasierte Authentifizierung für TV überall](home-based-authn-tve.md)
      + [HBA-Status für MVPDs](hba-status-mvpds.md)
   + Benutzermetadaten {#user-metadat}
      + [Benutzermetadaten](user-metadata-feature.md)
   + [Vorabgenehmigung](preflight-authz.md)
   + Fehlerberichte {#error-reportn}
      + [Fehlerberichte](error-reporting.md)
      + [Erweiterte Fehlercodes](enhanced-error-codes.md)
   + Kundenregistrierung {#client-regn}
      + [Dynamische Kundenregistrierung](dynamic-client-registration.md)
      + [Dynamische Client-Registrierungs-API](dynamic-client-registration-api.md)
      + [Dynamisches Client-Registrierungs-Management](dynamic-client-registration-management.md)
   + Abbaudienst {#degrn-service}
      + [Übersicht über die Abbau-API](degradation-api-overview.md)
   + Datenschutzbereitschaft {#privacy-readiness}
      + [Übersicht über den Datenschutzsupport](privacy-supp-overview.md)
      + [Datenschutzanfrage stellen](make-privacy-req.md)
+ Tipps und Fehlerbehebung {#tips-troubleshoot}
   + [MVPDs im Auswahldialogfeld zulassen](allow-mvpd-selectn-dialog.md)
   + [Verhindern, dass MVPDs im Auswahldialogfeld angezeigt werden](prevent-mvpd-selectn-dialog.md)
+ Support {#support}
   + [Eskalationsverfahren](escalation-procedures.md)
   + [Überwachen von Adobe Pass Adobe PayTV-Pass](monitoring-adobe-pay-tv-pass.md)
   + [Mindestsystemanforderungen](minimum-system-requirements.md)
+ Versionshinweise {#release-notes}
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.69](auth-rn-269.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.68](auth-rn-268.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.67](auth-rn-267.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.66](auth-rn-266.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.65.1](auth-rn-2651.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.65](auth-rn-265.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.64.1](auth-rn-2641.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.64](auth-rn-264.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.63](auth-rn-263.md)
   + [Versionshinweise zur Adobe Pass-Authentifizierung 2.62.1](auth-rn-2621.md)
   + Versionshinweise zum JavaScript-SDK  {#release-notes-javascript}
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.7.0](authn-rn-javascript-470.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.6.0](authn-rn-javascript-460.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.4.0](authn-rn-javascript-440.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.2.0](authn-rn-javascript-420.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.1.1](authn-rn-javascript-411.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.1.0](authn-rn-javascript-410.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 4.0.0](authn-rn-javascript-400.md)
      + [Versionshinweise zur Adobe Pass-Authentifizierung JavaScript 3.5.0](authn-rn-javascript-350.md)
   + Versionshinweise zum iOS/tvOS-SDK  {#release-notes-ios}
      + [Versionshinweise zu Adobe Pass Authentication iOS/tvOS 3.8.4](authn-rn-ios-tvos-384.md)
      + [Versionshinweise zu Adobe Pass Authentication iOS/tvOS 3.8.3](authn-rn-ios-tvos-383.md)
      + [Versionshinweise zu Adobe Pass Authentication iOS/tvOS 3.8.2](authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS/tvOS 3.8.1 - Versionshinweise](authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication iOS/tvOS 3.7.0 - Versionshinweise](authn-rn-ios-tvos-370.md)
   + Android SDK - Versionshinweise {#release-notes-android}
      + [Versionshinweise zur Adobe Pass-Authentifizierung Android 3.7.3](authn-rn-android-373.md)
+ Technische Hinweise {#tech-notes}
   + Adobe Pass Authentication SDKs {#primetime-authentication-sdks}
      + [Fragen und Antworten zu Zertifikaten](certificates-qa.md)
      + JavaScript-SDK {#javascript}
         + [Tracking Prevention Assessment - Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [Tracking Prevention Assessment - Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Cookie-Updates - SameSite- und Secure-Flags](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Zugriff auf Enabler Android SDK Single Sign-On (SSO) in Android 10-Apps](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass-Authentifizierung und das neue Android 6-Berechtigungsmodell &quot;Marshmallow&quot;](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS-SDK {#iostvos}
         + [WKWebView-Unterstützung für iOS SDK 3.1+](wkwebview-support-on-ios-sdk-31.md)
         + [SFSafariViewController-Unterstützung für iOS SDK 3.2+](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO in iOS bei Verwendung des Adobe Pass Authentication Access Enabler](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Zurücksetzen des Temp-Übergangs auf iOS](reset-temp-pass-on-ios.md)
         + [Debugging des AccessEnabler iOS/tvOS-SDK mithilfe von Konsolen-App-Protokollen](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0 Aktualisierungspfad](accessenabler-iostvos-370-upgrade-path.md)
   + Übergeben von Authentifizierungsumgebungen {#primetime-authentication-environments}
      + [Grundlagen zu Adobe-Umgebungen](understanding-the-adobe-environments.md)
      + [Einrichten der Umgebung und Testen in der Pre-Qual-Phase](setting-up-your-environment-and-testing-in-prequal.md)
      + [Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Adobe API-Test-Site](test-authn-authz-flows-using-adobes-api-test-site.md)
   + Clientlose API {#clientless-api}
      + [Clientlose API-Implementierung - Fehlercodes/Meldungen mit möglichen Gründen/Ursache](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Clientloser API-Ablauf bei Fehlen der Geräte-ID](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Clientlos: Vermeiden Sie die Verwendung von &quot;&amp;&quot;reg_code in /authenticate request](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Aktivieren von Adobe Pass Entitlement Services für ein Programm in Xbox 360 und XboxOne Clientless](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Clientloser Gerätetyp und Metriken](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + Anwendererlebnis {#user-exp}
      + [Migrieren der MVPD-Anmeldeseite von iFrame zum Popup](migr-mvpd-login-iframe-popup.md)
      + [Preflight-Funktion: Anleitung zum Aktivieren, Beheben oder Ermitteln des Problems](preflight-feature.md)
   + Tools und Hilfsprogramme {#tools-and-utilities}
      + [Charles Proxy verwenden](using-charles-proxy.md)
   + Konzepte {#concepts}
      + [Grundlegendes zu Benutzer-IDs](understanding-user-ids.md)
+ [Tv Dashboard-Benutzerhandbuch](tve-dashboard-user-guide.md)
+ [Glossar](glossary.md)