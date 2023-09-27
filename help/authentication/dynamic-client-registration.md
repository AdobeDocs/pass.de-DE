---
title: Dynamische Kundenregistrierung
description: Dynamische Kundenregistrierung
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Dynamische Kundenregistrierung {#dynamic-client-registration}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Kontext {#context}

Zur Anpassung an moderne Sicherheitspraktiken, verbesserte UX- und Plattformeigentümeranforderungen, das Adobe Pass Authentication Android SDK und das iOS SDK gehen in Richtung der Übernahme [Benutzerdefinierte Registerkarten für Android Chrome](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari view controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}.

Die aktuelle AdobePass-Implementierung verwendet plattformspezifische Webansichten, um die Webumgebung für die Anzeige der MVPD-Anmeldeseite bereitzustellen. Diese Webansichten verwenden keine gemeinsame Berechtigungsverwaltung für Plattformbrowser. Daher kann der Benutzer bei Verwendung einer Adobe Pass-Authentifizierungsanwendung kein im Browser gespeichertes Kennwort verwenden. Aus Sicherheitsgründen werden die WebView-Controller bei einigen Plattformen für Authentifizierungsaufgaben nicht mehr unterstützt. Sowohl Google als auch Apple bieten alternative Optionen wie &quot;Benutzerdefinierte Chrome-Registerkarten&quot;und &quot;Safari View Controller&quot;. Dabei handelt es sich im Wesentlichen um einzelne Benutzerregisterkarten ihrer jeweiligen Browser. Die Adobe Pass-Authentifizierung wird diese neuen Komponenten 2018 übernehmen.

## Details {#details}

Derzeit gibt es zwei Möglichkeiten, wie Adobe Pass Authentication Anwendungen identifiziert und registriert:

* Browser-basierte Clients werden über die Zulassungsdomänenliste registriert
* native Anwendungsclients wie iOS und Android-Anwendungen werden über den signierten Anforderungsmechanismus registriert.

Um die neuen Flüsse für den Chrome Custom Tabs &amp; Safari View Controller zu beheben, schlägt Adobe Pass einen neuen Client-Registrierungsmechanismus für die Registrierung neuer Anwendungen vor. Dieser Mechanismus ermöglicht eine sicherere und granularere Steuerung Ihrer Anwendungen und kann zur Registrierung von Anwendungen auf allen Plattformen verwendet werden.

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
