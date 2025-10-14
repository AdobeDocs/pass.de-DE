---
title: Vorteile der Verwendung des Parameters Clientless-DeviceType in Adobe Pass-Authentifizierungsmetriken
description: Vorteile der Verwendung des Parameters Clientless-DeviceType in Adobe Pass-Authentifizierungsmetriken
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# (Veraltete Version) Vorteile der Verwendung des Parameters Clientless-DeviceType in Adobe Pass-Authentifizierungsmetriken {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>

## Kontext

Der von der Clientless-API `deviceType` Parameter wird, sofern vorhanden, in Adobe Pass-Authentifizierungsmetriken verwendet, die über die [Überwachung des Berechtigungsdienstes“ verfügbar gemacht &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md).

Da die Verbindung zwischen dem `deviceType` Parameter und seinen **Vorteilen** in der Adobe Pass-Authentifizierungsmetrik ursprünglich nicht angegeben wurde, umfasst dieser technische Hinweis das Hinzufügen weiterer Informationen dazu.

## Erklärung

Der `deviceType` Parameter war seit der ersten Version in der Clientless-API vorhanden, seine Auswirkungen auf die Adobe Pass-Authentifizierungsmetriken wurden jedoch in einer neueren Version hinzugefügt.



>[!IMPORTANT]
>
>Wenn der Parameter `deviceType` richtig festgelegt ist, hat er in der Überwachung des Berechtigungs-Service **folgenden**: Er bietet Metriken, die bei Verwendung von [&#128279;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) (nach Gerätetyp ) sind, sodass verschiedene Arten der Analyse für z. B. Roku, AppleTV, Xbox usw. durchgeführt werden können.


Weitere Informationen zur API zur Überwachung des Berechtigungs-Service finden Sie in der [Aufschlüsselungsstruktur“, &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree) der die in [&#x200B; 2.0 verfügbaren Dimensionen](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions) (Ressourcen) veranschaulicht.

>[!NOTE]
>
>Der Inhalt dieser technischen Anmerkung wurde auch der &quot;[-API“ &#x200B;](#clientless_device_type).




## Implementierung

Um die Authentifizierungsmetriken von Adobe Pass in vollem Umfang nutzen zu können, gibt es zwei Arten von [Client-losen APIs](#web_srvs_summary) die derzeit verwendet werden und die den richtigen `deviceType` haben müssen:

1. APIs, die als erforderlichen Parameter `regcode` haben und den `deviceType` verwenden, der beim Erstellen des `regcode` festgelegt wurde, mit dem folgenden API-Aufruf:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. APIs mit dem `deviceType` als optionalem Parameter:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=„s1“>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

Wir empfehlen, den `deviceType`-Parameter zu verwenden und den richtigen Clientless-Gerätetyp für alle APIs zu übergeben.
