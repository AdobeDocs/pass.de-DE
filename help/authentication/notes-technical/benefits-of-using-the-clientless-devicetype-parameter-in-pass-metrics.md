---
title: Vorteile der Verwendung des Parameters Clientless deviceType in Adobe Pass-Authentifizierungsmetriken
description: Vorteile der Verwendung des Parameters Clientless deviceType in Adobe Pass-Authentifizierungsmetriken
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Vorteile der Verwendung des Parameters Clientless deviceType in Adobe Pass-Authentifizierungsmetriken {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Kontext

Obwohl optional, wird der Parameter &quot;`deviceType`&quot;aus der Client-losen API bei vorhandenen Adobe Pass-Authentifizierungsmetriken verwendet, die über die [Überwachung des Berechtigungsdienstes](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) verfügbar gemacht werden.

Da die Verbindung zwischen dem Parameter `deviceType` und seinen **Vorteilen** bei den Adobe Pass-Authentifizierungsmetriken zunächst nicht angegeben wurde, besteht der Anwendungsbereich dieser Technote darin, weitere Informationen zu ihnen hinzuzufügen.

## Erklärung

Der Parameter `deviceType` war seit der ersten Version in der ClientLess-API vorhanden, seine Auswirkungen auf die Adobe Pass-Authentifizierungsmetriken wurden jedoch in einer neueren Version hinzugefügt.



>[!IMPORTANT]
>
>Wenn der Parameter `deviceType` richtig eingestellt ist, bietet er den folgenden **Vorteil** bei der Überwachung des Berechtigungsdienstes: Er bietet Metriken, die bei Verwendung von ClientLess nach Gerätetyp aufgeschlüsselt sind [, sodass für Roku, AppleTV, Xbox usw. unterschiedliche Analysetypen durchgeführt werden können.](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)


Weitere Informationen zur Berechtigungsdienst-Monitoring-API finden Sie in der [Drilldown-Struktur, ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree) , die die in ESM 2.0 verfügbaren [Dimensionen](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions) (Ressourcen) veranschaulicht.

>[!NOTE]
>
>Der Inhalt dieser Technote wurde auch der [Client-losen API](#clientless_device_type) hinzugefügt.




## Implementierung

Um die Adobe Pass-Authentifizierungsmetriken vollständig nutzen zu können, werden derzeit zwei Typen von [clientlosen APIs](#web_srvs_summary) verwendet, für die der korrekte `deviceType` festgelegt werden muss:

1. APIs, die `regcode` als erforderlichen Parameter aufweisen und den `deviceType` -Parameter verwenden, der beim Erstellen des `regcode` festgelegt wurde, mit dem folgenden API-Aufruf:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. APIs, die den optionalen Parameter `deviceType` aufweisen:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthin](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

Wir empfehlen, den Parameter `deviceType` zu verwenden und den richtigen Client-losen Gerätetyp für alle APIs zu übergeben.
