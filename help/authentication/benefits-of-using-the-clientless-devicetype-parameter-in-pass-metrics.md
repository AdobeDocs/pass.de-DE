---
title: Vorteile der Verwendung des Parameters Clientless deviceType in Adobe Pass-Authentifizierungsmetriken
description: Vorteile der Verwendung des Parameters Clientless deviceType in Adobe Pass-Authentifizierungsmetriken
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# Vorteile der Verwendung des Parameters Clientless deviceType in Adobe Pass-Authentifizierungsmetriken {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

## Kontext

Obwohl optional, wird der Parameter `deviceType` aus der ClientLess-API wird, sofern vorhanden, in Adobe Pass-Authentifizierungsmetriken verwendet, die über verfügbar gemacht werden. [Überwachung des Berechtigungsdienstes](/help/authentication/entitlement-service-monitoring-overview.md).

Die Verbindung zwischen `deviceType` Parameter und **Vorteile** bei Adobe Pass-Authentifizierungsmetriken zunächst nicht angegeben wurde, besteht der Zweck dieser technischen Anmerkung darin, weitere Informationen zu diesen Metriken hinzuzufügen.

## Erklärung

Die `deviceType` -Parameter war seit der ersten Version in der ClientLess-API vorhanden, aber die Auswirkungen auf die Adobe Pass-Authentifizierungsmetriken wurden in einer neueren Version hinzugefügt.



>[!IMPORTANT]
>
>Wenn der Parameter `deviceType` korrekt eingestellt ist, weist es Folgendes auf **nutzen** in der Überwachung des Entitätsdienstes: Es bietet Metriken, die [aufgeschlüsselt nach Gerätetyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) bei Verwendung von ClientLess, sodass verschiedene Arten der Analyse für z. B. Roku, AppleTV, Xbox usw. durchgeführt werden können.


Weiterführende Informationen zur Berechtigungsdienst-Monitoring-API finden Sie im Abschnitt [Drilldown-Struktur,](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) , das die [Dimensionen](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) (Ressourcen) verfügbar in ESM 2.0.

>[!NOTE]
>
>Der Inhalt dieser Technote wurde auch zum [Clientlose API](#clientless_device_type).




## Implementierung

Um die Adobe Pass-Authentifizierungsmetriken in vollem Umfang nutzen zu können, gibt es zwei Arten von [Clientlose APIs](#web_srvs_summary) die derzeit verwendet werden und die `deviceType` set:

1. APIs mit `regcode` als erforderlichen Parameter verwenden und die `deviceType` -Parameter, der beim Erstellen der `regcode`mit dem folgenden API-Aufruf:
   - [\&lt;reggie _fqdn=&quot;&quot;>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. APIs mit der `deviceType` als optionalen Parameter:
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/checkauthin](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/authorize](#init_authz)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/media](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/mediatoken](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/logout](#init_logout)

Wir empfehlen, die `deviceType` und übergeben Sie den richtigen Client-losen Gerätetyp für alle APIs.
