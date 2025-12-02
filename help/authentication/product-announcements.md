---
title: Produktankündigungen
description: Produktankündigungen
exl-id: 3c9c66e1-d31d-4af3-8ab2-eb32492f42ca
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 1%

---

# Produktankündigungen {#product-announcements}

## End of Life (EOL) {#eol}

In diesem Abschnitt werden die Daten zum Ende der Unterstützung und zum Ende der Nutzungsdauer für bestimmte Adobe Pass-Authentifizierungsfunktionen und -Produkte, die eingestellt werden sollen, beschrieben.

Achten Sie darauf, über die Stilllegungszeitpläne auf dem Laufenden zu bleiben und die im Folgenden beschriebenen Maßnahmen zu ergreifen.

| Ankündigung | Ankündigungsdatum | Ende der Unterstützung | Ende der Nutzungsdauer |   | Beschreibung |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|---------------------|---------------------------------------------|:--|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TVE Dashboard URLs zur Adobe Pass-Authentifizierung - EOL <ul><li><a href="https://console.auth.adobe.com">console.auth.adobe.com</a></li><li><a href="https://console.auth-staging.adobe.com">console.auth-staging.adobe.com</a></li><li><a href="https://console-prequal.auth.adobe.com">console-prequal.auth.adobe.com</a></li><li><a href="https://console-prequal.auth-staging.adobe.com">console-prequal.auth-staging.adobe.com</a></li></ul> | 10/02/2024 | 01/01/2025 | 03/12/2025 |   | Wir planen das Ende des Lebenszyklus für das TVE-Dashboard zur Adobe Pass-Authentifizierung, gehostet unter [console.auth.adobe.com](https://console.auth.adobe.com), [console.auth-staging.adobe.com](https://console.auth-staging.adobe.com), [console-prequal.auth.adobe.com](https://console-prequal.auth.adobe.com) und [console-prequal.auth-staging.adobe.com](https://console-prequal.auth-staging.adobe.com) am **12. März 2025** .</br></br>Nach diesem Datum ist der Zugriff auf die oben genannten URLs nicht mehr verfügbar und Sie werden zum neuen [TVE-Dashboard“ weitergeleitet](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) um weiterhin auf Ihre Integrationen zuzugreifen.</br></br>Um den kontinuierlichen Service sicherzustellen, müssen Sie auf das neue [TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) zugreifen, das unter [experience.adobe.com/pass/authentication gehostet &#x200B;](https://experience.adobe.com/pass/authentication). |
| Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 EOL | 12/11/2024 | Nicht zutreffend | <span style="color: red;">01/08/2025</span> |   | Wir planen das Ende der Lebensdauer von Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 am **. Januar 2025**. Nach diesem Datum funktionieren die von AccessEnabler JavaScript SDK v3.5 bereitgestellten Funktionen und Services nicht mehr, einschließlich Benutzerauthentifizierung und -autorisierung. |
| Adobe Pass-Authentifizierung - Kein DCR-Sicherheitsmechanismus - EOL | 12/11/2024 | Nicht zutreffend | <span style="color: red;">01/20/2025</span> |   | Wir planen das Ende der Nutzungsdauer für den Nicht-DCR-Sicherheitsmechanismus der Adobe Pass-Authentifizierung am **20. Januar 2025**. Dieser Mechanismus wurde verwendet, um den Zugriff auf die folgenden Adobe Pass-Authentifizierungs-APIs zu sichern:<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md">Reset Temp Pass API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md">Abbauungs-API</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">Proxy-MVPD-API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">Berechtigungs-Service-Überwachungs-API</a></li></ul>Nach diesem Datum funktionieren die Funktionen und Services, die von den oben genannten APIs bereitgestellt werden und auf die über diesen Sicherheitsmechanismus zugegriffen wird, nicht mehr.</br></br>Um einen kontinuierlichen Service zu gewährleisten, müssen Sie alle Ihre Anwendungen migrieren, um den Mechanismus [Dynamische Client-Registrierung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) zu verwenden. |
| Adobe Pass Authentication REST API v1 EOL | 12/11/2024 | 12/31/2025 | Ende 2026 (noch festzulegen) |   | Wir planen, die Unterstützung für die Adobe Pass Authentication REST API v1 am **. Dezember 2025**. Nach diesem Datum werden keine Aktualisierungen oder Fehlerbehebungen mehr bereitgestellt.</br></br>Um eine kontinuierliche Unterstützung zu gewährleisten, müssen Sie alle Ihre Anwendungen auf <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a> migrieren.</br></br>Wir planen das Ende der Nutzungsdauer der Adobe Pass Authentication REST API v1 **Ende 2026**. Nach diesem Datum werden die von der REST-API v1 bereitgestellten Funktionen und Dienste nicht mehr funktionieren, einschließlich Benutzerauthentifizierung und -autorisierung.</br></br>Um einen kontinuierlichen Service zu gewährleisten, müssen Sie alle Ihre Anwendungen auf die <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a> migrieren. |
| Adobe Pass Authentication AccessEnabler SDKs EOL | 12/11/2024 | 05/31/2026 | Ende 2026 (noch festzulegen) |   | Wir planen, die Unterstützung für Adobe Pass Authentication AccessEnabler-SDKs am **. Mai 2026 einzustellen**. Nach diesem Datum werden keine Aktualisierungen oder Fehlerbehebungen mehr bereitgestellt.</br></br>Um eine kontinuierliche Unterstützung zu gewährleisten, müssen Sie alle Ihre Anwendungen auf <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a> migrieren.</br></br>Wir planen das Ende der Lebensdauer für Adobe Pass Authentication AccessEnabler-SDKs **Ende 2026**. Nach diesem Datum werden die von AccessEnabler-SDKs bereitgestellten Funktionen und Services nicht mehr funktionieren, einschließlich Benutzerauthentifizierung und -autorisierung.</br></br>Um einen kontinuierlichen Service zu gewährleisten, müssen Sie alle Ihre Anwendungen auf die <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a> migrieren. |

## Produktversionen {#product-releases}

In diesem Abschnitt werden Verweise auf den Versionsverlauf und die entsprechenden Versionshinweise für die Adobe Pass-Authentifizierung kompiliert.

### 2025 {#product-releases-2025}

| Versionshinweise | Daten |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| Versionshinweise zur [Adobe Pass-Authentifizierung 3.5.0](notes-releases/auth-rn-350.md) | 12/09/2025 - 12/11/2025 |
| Versionshinweise zur [Adobe Pass-Authentifizierung für Android 3.8.0](notes-releases/authn-rn-android-380.md) | 09/18/2025 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 3.4.0](notes-releases/auth-rn-340.md) | 09/16/2025 - 09/18/2025 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 3.3.0](notes-releases/auth-rn-330.md) | 07/22/2025 - 07/24/2025 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 3.2.0](notes-releases/auth-rn-320.md) | 06/10/2025 - 06/12/2025 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 3.1.0](notes-releases/auth-rn-310.md) | 02/25/2025 - 02/27/2025 |
| [Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript SDK 4.7.1](notes-releases/authn-rn-javascript-471.md) | 02/25/2025 - 02/27/2025 |

### 2024 {#product-releases-2024}

| Versionshinweise | Daten |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| Versionshinweise zur [Adobe Pass-Authentifizierung 3.0.3](notes-releases/auth-rn-303.md) | 10/29/2024 - 10/31/2024 |
| Versionshinweise zu [Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md) | 09/10/2024 - 09/12/2024 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.70](notes-releases/auth-rn-270.md) | 04/23/2024 - 04/25/2024 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.69](notes-releases/auth-rn-269.md) | 02/27/2024 - 02/29/2024 |
| [Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript SDK 4.7.0](notes-releases/authn-rn-javascript-470.md) | 02/27/2024 - 02/29/2024 |
| [Versionshinweise zu Adobe Pass Authentication iOS / tvOS SDK 3.9.2](notes-releases/authn-rn-ios-tvos-392.md) | 03/26/2024 |
| [Versionshinweise zu Adobe Pass Authentication iOS / tvOS SDK 3.8.4](notes-releases/authn-rn-ios-tvos-384.md) | 01/26/2024 |

### 2023 {#product-releases-2023}

| Versionshinweise | Daten |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.68](notes-releases/auth-rn-268.md) | 12/05/2023 - 12/07/2023 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.67](notes-releases/auth-rn-267.md) | 09/12/2023 - 09/14/2023 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.66](notes-releases/auth-rn-266.md) | 07/11/2023 - 07/13/2023 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.65.1](notes-releases/auth-rn-2651.md) | 06/20/2023 - 06/22/2023 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.65](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.64.1](notes-releases/auth-rn-2641.md) | 01/31/2023 - 02/02/2023 |
| [Versionshinweise zu Adobe Pass Authentication iOS / tvOS SDK 3.8.3](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Versionshinweise zu Adobe Pass Authentication iOS / tvOS SDK 3.8.2](notes-releases/authn-rn-ios-tvos-382.md) | 02/10/2023 |
| [Versionshinweise zu Adobe Pass Authentication iOS / tvOS SDK 3.8.1](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Versionshinweise zur Adobe Pass-Authentifizierung für Android SDK 3.7.3](notes-releases/authn-rn-android-373.md) | 09/19/2023 |

### 2022 {#product-releases-2022}

| Versionshinweise | Daten |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.64](notes-releases/auth-rn-264.md) | 11/08/2022 - 11/10/2022 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.63](notes-releases/auth-rn-263.md) | 09/20/2022 - 09/22/2022 |
| Versionshinweise zur [Adobe Pass-Authentifizierung 2.62.1](notes-releases/auth-rn-2621.md) | 08/02/2022 - 08/04/2022 |
| [Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript SDK 4.6.0](notes-releases/authn-rn-javascript-460.md) | 09/20/2022 - 09/22/2022 |

### 2021 {#product-releases-2021}

| Versionshinweise | Daten |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Versionshinweise zur Adobe Pass-Authentifizierung für JavaScript SDK 4.4.0](notes-releases/authn-rn-javascript-440.md) | 06/22/2021 |
| [Versionshinweise zu Adobe Pass Authentication iOS / tvOS SDK 3.7.0](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |
