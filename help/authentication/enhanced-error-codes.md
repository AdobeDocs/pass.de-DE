---
title: Verbesserte Fehlercodes
description: Verbesserte Fehlercodes
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '2593'
ht-degree: 2%

---

# Verbesserte Fehlercodes {#enhanced-error-codes}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Verbesserte Fehlercodes stellen eine Adobe Pass-Authentifizierungsfunktion dar, die zusätzliche Fehlerinformationen für Client-Anwendungen bereitstellt, die in Folgendes integriert sind:

* Adobe Pass Authentication REST APIs:
   * [REST API v1](./rest-api-overview.md)
   * [REST API v2](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
* Adobe Pass Authentication SDKs Preauthorize API:
   * [JavaScript SDK (Preauthorize API)](./preauthorize-api-javascript-sdk.md)
   * [iOS/tvOS-SDK (Vorabautorisierungs-API)](./preauthorize-api-ios-tvos-sdk.md)
   * [Android SDK (Preauthorize API)](./preauthorize-api-android-sdk.md)

  _(*) Die Vorabautorisierungs-API ist die einzige Adobe Pass Authentication SDK-API, die Unterstützung für erweiterte Fehlercodes bietet._

>[!IMPORTANT]
>
> Anwendungen, die die Adobe Pass Authentication REST API v2 integrieren, profitieren standardmäßig von erweiterten Fehlercodes, ohne dass eine zusätzliche Konfiguration erforderlich ist.
>
> <br/>
>
> Anwendungen, die die Adobe Pass Authentication REST API v1 oder die Vorabautorisierungs-API der SDKs integrieren, können nur dann von den erweiterten Fehlercodes profitieren, wenn die Funktion explizit aktiviert ist.
>
> <br/>
>
> Um diese Funktion explizit zu aktivieren, erstellen Sie ein Ticket über unseren [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren technischen Kundenbetreuer (TAM) um Hilfe.

## Darstellung {#enhanced-error-codes-representation}

Verbesserte Fehlercodes können je nach integrierter Adobe Pass-Authentifizierungs-API und dem verwendeten Header-Wert &quot;Accept&quot;(d. h. `application/json` oder `application/xml`) im Format `JSON` oder `XML` dargestellt werden:

| Adobe Pass Authentication API | JSON | XML |
|-------------------------------|---------|---------|
| REST API v1 | &amp;check; | &amp;check; |
| REST API v2 | &amp;check; |         |
| Vorabautorisierungs-API für SDKs | &amp;check; |         |

>[!IMPORTANT]
>
> Die Adobe Pass-Authentifizierung kann Clientanwendungen in zwei Formularen mit erweiterten Fehlercodes kommunizieren:
>
> <br/>
>
> * **Fehlerinformationen der obersten Ebene**: In diesem Fall befindet sich das Objekt ***&quot;error&quot;*** auf der obersten Ebene, daher kann der Antworttext nur das Objekt ***&quot;error&quot;*** enthalten.
> * **Fehlerinformationen auf Elementebene**: In diesem Fall befindet sich das Objekt ***&quot;error&quot;*** auf Elementebene. Daher kann der Antworttext ein Objekt vom Typ ***&quot;error&quot;*** für alle Elemente enthalten, bei denen während der Wartung ein Fehler aufgetreten ist.
>
> <br/>
>
> Überprüfen Sie die öffentliche Dokumentation für jede integrierte Adobe Pass-Authentifizierungs-API, um die Darstellungsspezifikationen für erweiterte Fehlercodes zu ermitteln.

Sehen Sie sich die folgenden HTTP-Antworten an, die Beispiele für erweiterte Fehlercodes enthalten, die als `JSON` oder `XML` dargestellt werden.

>[!BEGINTABS]

>[!TAB REST API v1 - Fehlerinformationen der obersten Ebene (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB REST API v1 - Fehlerinformationen der obersten Ebene (XML)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!TAB REST API v1 - Fehlerinformationen auf Elementebene (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v2 - Fehlerinformationen der obersten Ebene (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!TAB REST API v2 - Fehlerinformationen auf Elementebene (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!ENDTABS]

Verbesserte Fehlercodes beinhalten die folgenden `JSON` -Felder oder `XML` -Attribute:

| Name | Typ | Beispiel | Beschränkt | Beschreibung |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *action* | *string* | *retry* | &amp;check; | Die Adobe Pass-Authentifizierung empfahl eine Aktion, die die in diesem Dokument beschriebene Situation beheben könnte. <br/><br/> Weitere Informationen finden Sie im Abschnitt [Aktion](#enhanced-error-codes-action) . |
| *status* | *integer* | *403* | &amp;check; | Der HTTP-Antwortstatuscode gemäß der Definition im Dokument [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6). <br/><br/> Weitere Informationen finden Sie im Abschnitt [Status](#enhanced-error-codes-status) . |
| *code* | *string* | *network_connection_failure* | &amp;check; | Der eindeutige Adobe Pass-Authentifizierungscode, der mit dem Fehler verknüpft ist, wie in diesem Dokument definiert. <br/><br/> Weitere Informationen finden Sie im Abschnitt [Code](#enhanced-error-codes-code) . |
| *message* | *string* | *Der Dienst Ihres Fernsehanbieters kann nicht kontaktiert werden* |            | Die für Menschen lesbare Nachricht, die dem Endbenutzer in einigen Fällen angezeigt werden könnte. <br/><br/> Weitere Informationen finden Sie im Abschnitt [Reaktionsverwaltung](#enhanced-error-codes-response-handling) . |
| *details* | *string* | *Ihr Abonnement-Paket enthält nicht den Kanal &quot;Live&quot;* |            | Die detaillierte Meldung, die in einigen Fällen von einem Dienstleistungspartner bereitgestellt werden könnte, <br/><br/> Dieses Feld ist möglicherweise nicht vorhanden, falls der Dienstleistungspartner keine benutzerdefinierte Nachricht bereitstellt. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | Die öffentliche Dokumentations-URL zur Adobe Pass-Authentifizierung , die weitere Informationen darüber enthält, warum dieser Fehler aufgetreten ist, und mögliche Lösungen. <br/><br/> Dieses Feld enthält eine absolute URL und sollte nicht aus dem Fehlercode abgeleitet werden. Je nach Fehlerkontext kann eine andere URL angegeben werden. |
| *trace* | *string* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | Die eindeutige Kennung für die Antwort, die verwendet werden kann, wenn der Adobe Pass Authentication Support kontaktiert wird, um bestimmte Probleme zu beheben. |

>[!IMPORTANT]
>
> Die Spalte **Eingeschränkt** gibt an, ob das entsprechende Feld einen Wert aus einem endlichen Satz enthält, während unbeschränkte Felder beliebige Daten enthalten können.
>
> <br/>
>
> Zukünftige Aktualisierungen dieses Dokuments könnten den endlichen Sätzen Werte hinzufügen, vorhandene jedoch nicht entfernen oder ändern.

### Aktion {#enhanced-error-codes-representation-action}

Verbesserte Fehlercodes enthalten ein Feld &quot;Aktion&quot;, das eine empfohlene Aktion zur Behebung der Situation bereitstellt.

Zu den möglichen Werten für das Feld &quot;Aktion&quot;gehören:

| Aktion | Beschreibung | Kategorie |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| Keine | Es gibt keine vordefinierte Aktion, um dieses Problem zu beheben. In einigen Fällen kann dies jedoch auf einen falschen Aufruf der API hindeuten. | Korrigieren Sie den Anforderungskontext. |
| Konfiguration | Die Clientanwendung erfordert eine Konfigurationsänderung, die meistens über das Adobe Pass TVE Dashboard durchgeführt wird. | Korrigieren Sie den Integrationskonfigurationskontext. |
| application-registration | Die Clientanwendung erfordert eine erneute Registrierung. | Korrigieren Sie den Client-Anwendungskontext. |
| Authentifizierung | Die Clientanwendung erfordert die Authentifizierung oder erneute Authentifizierung des Benutzers. | Korrigieren Sie den Client-Anwendungskontext. |
| Autorisierung | Die Client-Anwendung erfordert eine Autorisierung für die angegebene Ressource. | Korrigieren Sie den Client-Anwendungskontext. |
| Wiederholen | Die Client-Anwendung muss die Anfrage erneut versuchen. | Korrigieren Sie den Anforderungskontext. |

_(*) Bei einigen Fehlern können mehrere Aktionen mögliche Lösungen sein, aber das Feld &quot;Aktion&quot;zeigt die Aktion an, bei der die höchste Wahrscheinlichkeit besteht, den Fehler zu beheben._

### Status {#enhanced-error-codes-representation-status}

Verbesserte Fehlercodes enthalten ein Feld &quot;status&quot;, das den mit dem Fehler verknüpften HTTP-Status-Code angibt.

Zu den möglichen Werten für das Feld &quot;Status&quot;gehören:

| Code | Reason-Wortgruppe |
|------|-----------------------|
| 400 | Ungültige Anfrage |
| 401 | Unerlaubt |
| 403 | Verboten |
| 404 | Nicht gefunden |
| 405 | Methode nicht zulässig |
| 410 | Gone |
| 412 | Vorbedingung fehlgeschlagen |
| 500 | Interner Server-Fehler |

Verbesserte Fehlercodes mit einem 4.x-Status werden normalerweise angezeigt, wenn der Fehler vom Client erzeugt wird, und meistens bedeutet dies, dass der Client zusätzliche Arbeit benötigt, um ihn zu beheben.

Verbesserte Fehlercodes mit einem 5xx-Status werden normalerweise angezeigt, wenn der Fehler vom Server erzeugt wird, und meistens bedeutet dies, dass der Server zusätzliche Arbeit benötigt, um ihn zu beheben.

>[!IMPORTANT]
>
> In einigen Fällen unterscheidet sich der HTTP-Antwortstatus-Code vom &quot;Status&quot;-Feld für den erweiterten Fehlercode, insbesondere bei der Interaktion mit einer Adobe Pass-Authentifizierungs-API, die erweiterte Fehlercodes als Fehlerinformationen auf Elementebene übermittelt.

### Code {#enhanced-error-codes-representation-code}

Verbesserte Fehlercodes enthalten ein Feld &quot;code&quot;, das eine eindeutige Adobe Pass-Authentifizierungskennung bereitstellt, die mit dem Fehler verknüpft ist.

Die möglichen Werte für das Feld &quot;code&quot;werden [unter ](#enhanced-error-codes-list) in zwei Listen auf der Grundlage der integrierten Adobe Pass-Authentifizierungs-API aggregiert.

## Listen {#enhanced-error-codes-lists}

### REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

In der folgenden Tabelle sind mögliche erweiterte Fehlercodes aufgeführt, auf die eine Clientanwendung bei der Integration mit der Adobe Pass Authentication REST API v1 stoßen kann.

| Aktion | Code | Status | Nachricht |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **none** | *invalid_requestor* | 400 | Der Anforderungsparameter fehlt oder ist ungültig. |
|                    | *invalid_device_info* | 400 | Die Geräteinformationen fehlen oder sind ungültig. |
|                    | *invalid_device_id* | 400 | Die Geräte-ID fehlt oder ist ungültig. |
|                    | *missing_resource* | 400.412 | Der Ressourcenparameter fehlt. |
|                    | *malformed_authz_request* | 400.412 | Die Autorisierungsanfrage ist null oder ungültig. |
|                    | *preauthorization_denied_by_mvpd* | 403 | Der MVPD hat eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben, wenn er eine Vorabautorisierung für die angegebene Ressource beantragt. |
|                    | *authorization_denied_by_mvpd* | 403 | Der MVPD hat eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben, wenn er eine Autorisierung für die angegebene Ressource anfordert. |
|                    | *authorization_denied_by_parental_control* | 403 | Der MVPD hat die Entscheidung &quot;Ablehnen&quot;zurückgegeben, da die elterlichen Kontrolleinstellungen für die angegebene Ressource festgelegt wurden. |
|                    | *internal_error* | 400, 405, 500 | Die Anfrage schlug aufgrund eines internen Server-Fehlers fehl. |
| **configuration** | *unknown_integration* | 400.412 | Die Integration zwischen dem angegebenen Programmierer und Identitätsanbieter existiert nicht. Verwenden Sie das TVE-Dashboard , um die erforderliche Integration zu erstellen. |
|                    | *too_many_resources* | 403 | Die Autorisierungs- oder Vorabautorisierungsanfrage schlug fehl, da zu viele Ressourcen abgefragt wurden. Wenden Sie sich an das Supportteam, um die Autorisierungs- und Vorautorisierungsbeschränkungen ordnungsgemäß zu konfigurieren. |
| **authentication** | *authentication_session_emitter_mismatch* | 400 | Die Autorisierungsanfrage schlug fehl, weil der angegebene MVPD für den Autorisierungsfluss sich von dem unterscheidet, der die Authentifizierungssitzung ausgestellt hat. Der Benutzer muss sich erneut mit dem gewünschten MVPD authentifizieren, um fortfahren zu können. |
|                    | *authorization_denied_by_hba_policies* | 403 | Der MVPD hat aufgrund von Home-basierten Authentifizierungsrichtlinien eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben. Die aktuelle Authentifizierung wurde mithilfe eines Home-based Authentication Flow (HBA) abgerufen, aber das Gerät ist nicht mehr zu Hause, wenn die Autorisierung für die angegebene Ressource angefordert wird. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *authorization_denied_by_session_invalidated* | 403 | Die Authentifizierungssitzung wurde vom Identitäts-Provider ungültig gemacht. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *identity_not_acknowledged_by_mvpd* | 403 | Die Autorisierungsanfrage schlug fehl, weil die Benutzeridentität vom MVPD nicht erkannt wurde. |
|                    | *authentication_session_invalidated* | 403 | Die Authentifizierungssitzung wurde vom Identitäts-Provider ungültig gemacht. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *authentication_session_missing* | 403, 412 | Die mit dieser Anfrage verknüpfte Authentifizierungssitzung konnte nicht abgerufen werden. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *authentication_session_expied* | 403, 412 | Die aktuelle Authentifizierungssitzung ist abgelaufen. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *preauthorization_authentication_session_missing* | 412 | Die mit dieser Anfrage verknüpfte Authentifizierungssitzung konnte nicht abgerufen werden. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *preauthorization_authentication_session_expied* | 412 | Die aktuelle Authentifizierungssitzung ist abgelaufen. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
| **Autorisierung** | *authorization_not_found* | 403.404 | Für die angegebene Ressource wurde keine Autorisierung gefunden. Der Benutzer muss eine neue Autorisierung einholen, um fortfahren zu können. |
|                    | *authorization_expi}* | 410 | Die vorherige Autorisierung für die angegebene Ressource ist abgelaufen. Der Benutzer muss eine neue Autorisierung einholen, um fortfahren zu können. |
| **retry** | *network_received_error* | 403 | Beim Abrufen der Antwort vom zugehörigen Partnerdienst trat ein Lesefehler auf. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |
|                    | *network_connection_timeout* | 403 | Es gab einen Verbindungstimeout mit dem zugehörigen Partnerdienst. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |
|                    | *maximum_execution_time_exceeded* | 403 | Die Anfrage wurde in der maximal zulässigen Zeit nicht abgeschlossen. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |

### Vorabautorisierungs-API für SDKs {#enhanced-error-codes-lists-sdks-preauthorize-api}

Im vorherigen Abschnitt [finden Sie mögliche erweiterte Fehlercodes, auf die eine Client-Anwendung stoßen kann, wenn sie in die Adobe Pass Authentication SDKs Preauthorize API integriert ist.](#enhanced-error-codes-list-rest-api-v1)

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

In der folgenden Tabelle sind mögliche erweiterte Fehlercodes aufgeführt, auf die eine Client-Anwendung bei der Integration mit der Adobe Pass Authentication REST API v2 stoßen kann.

| Aktion | Code | Status | Nachricht |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **none** | *invalid_parameter_service_provider* | 400 | Der Parameterwert des Dienstanbieters fehlt oder ist ungültig. |
|                              | *invalid_parameter_mvpd* | 400 | Der Parameterwert mvpd fehlt oder ist ungültig. |
|                              | *invalid_parameter_code* | 400 | Der Code-Parameterwert fehlt oder ist ungültig. |
|                              | *invalid_parameter_resources* | 400 | Der Ressourcenparameterwert fehlt oder ist ungültig. |
|                              | *invalid_parameter_redirect_url* | 400 | Der Umleitungs-URL-Parameterwert fehlt oder ist ungültig. |
|                              | *invalid_parameter_partner* | 400 | Der Partnerparameterwert fehlt oder ist ungültig. |
|                              | *invalid_parameter_saml_response* | 400 | Der SAML-Antwortparameterwert fehlt oder ist ungültig. |
|                              | *invalid_header_device_info* | 400 | Der Header-Wert für Geräteinformationen fehlt oder ist ungültig. |
|                              | *invalid_header_device_identifier* | 400 | Der Kopfzeilenwert der Gerätekennung fehlt oder ist ungültig. |
|                              | *invalid_header_identity_for_temporarily_access* | 400 | Die Identität für den Wert des temporären Kopfzeilen für den Zugriff fehlt oder ist ungültig. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | Der Wert für den Berechtigungszugriffstatus aus der Statuskopfzeile des Partner-Frameworks ist nicht vorhanden. |
|                              | *invalid_header_pfs_permission_access_not_defined* | 400 | Der Wert für den Berechtigungszugriffstatus aus der Statuskopfzeile des Partner-Frameworks wird nicht bestimmt. |
|                              | *invalid_header_pfs_permission_access_not_assigned* | 400 | Der Wert für den Berechtigungszugriffstatus aus der Statuskopfzeile des Partner-Frameworks wird nicht gewährt. |
|                              | *invalid_header_pfs_provider_id_not_defined* | 400 | Der Anbieter-ID-Wert aus der Statuskopfzeile des Partner-Frameworks ist keiner bekannten mvpd zugeordnet. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | Der Anbieter-ID-Wert aus der Statuskopfzeile des Partner-Frameworks stimmt nicht mit der als Parameter gesendeten mvpd überein. |
|                              | *invalid_integration* | 400 | Die Integration zwischen dem angegebenen Dienstleister und mvpd existiert nicht oder ist deaktiviert. |
|                              | *invalid_authentication_session* | 400 | Die mit dieser Anfrage verknüpfte Authentifizierungssitzung fehlt oder ist ungültig. |
|                              | *preauthorization_denied_by_mvpd* | 403 | Der MVPD hat eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben, wenn er eine Vorabautorisierung für die angegebene Ressource beantragt. |
|                              | *authorization_denied_by_mvpd* | 403 | Der MVPD hat eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben, wenn er eine Autorisierung für die angegebene Ressource anfordert. |
|                              | *authorization_denied_by_parental_control* | 403 | Der MVPD hat die Entscheidung &quot;Ablehnen&quot;zurückgegeben, da die elterlichen Kontrolleinstellungen für die angegebene Ressource festgelegt wurden. |
|                              | *authorization_denied_by_degradation_rule* | 403 | Für die Integration zwischen dem angegebenen Dienstleister und mvpd wird eine Abbauregel angewendet, die die Autorisierung für die angeforderten Ressourcen verweigert. |
|                              | *internal_server_error* | 500 | Die Anfrage schlug aufgrund eines internen Server-Fehlers fehl. |
| **configuration** | *too_many_resources* | 403 | Die Autorisierungs- oder Vorabautorisierungsanfrage schlug fehl, da zu viele Ressourcen abgefragt wurden. Wenden Sie sich an das Supportteam, um die Autorisierungs- und Vorautorisierungsbeschränkungen ordnungsgemäß zu konfigurieren. |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | Die Konfiguration des Benutzermetadaten-Zertifikats fehlt oder ist ungültig. |
|                              | *invalid_configuration_temporarily_access* | 500 | Die Konfiguration für den temporären Zugriff ist ungültig. |
|                              | *invalid_configuration_platform* | 500 | Die Plattformkonfiguration fehlt oder ist für die Integration ungültig. |
|                              | *invalid_configuration_platform_id* | 500 | Die Konfiguration der Plattform-ID fehlt oder ist ungültig. |
|                              | *invalid_configuration_platform_trait* | 500 | Die Konfiguration der Plattformeigenschaften fehlt oder ist ungültig. |
|                              | *invalid_configuration_platform_category_trait* | 500 | Die Konfiguration der Eigenschaften der Plattformkategorie fehlt oder ist ungültig. |
|                              | *invalid_configuration_platform_services* | 500 | Die Konfiguration der Plattformdienste fehlt oder ist für die Integration ungültig. |
|                              | *invalid_configuration_mvpd_platform* | 500 | Die mvpd-Plattformkonfiguration fehlt oder ist für mvpd und die Plattform ungültig. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | Die Konfiguration des MVPD-Plattformboarding-Status fehlt oder ist für mvpd und die Plattform ungültig. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | Die Konfiguration des Profilaustauschs für die mvpd-Plattform fehlt oder ist für mvpd und die Plattform ungültig. |
| **application-registration** | *invalid_access_token_service_provider* | 401 | Das Zugriffstoken ist aufgrund eines ungültigen Dienstanbieters ungültig. |
|                              | *invalid_access_token_client_application* | 401 | Das Zugriffstoken ist aufgrund einer ungültigen Clientanwendung ungültig. |
| **authentication** | *authenticated_profile_missing* | 403 | Das mit dieser Anfrage verknüpfte authentifizierte Profil fehlt. |
|                              | *authenticated_profile_expied* | 403 | Das mit dieser Anfrage verknüpfte authentifizierte Profil ist abgelaufen. |
|                              | *authenticated_profile_invalidated* | 403 | Das mit dieser Anfrage verknüpfte authentifizierte Profil wurde ungültig gemacht. |
|                              | *temporärer_Zugriff_Dauer_Limit_überschritten* | 403 | Die zeitweilige Zugriffsdauer wurde überschritten. |
|                              | *temporärer_Zugriff_Ressourcen_Limit_überschritten* | 403 | Die zeitweilige Beschränkung für Zugriffsressourcen wurde überschritten. |
|                              | *authorization_denied_by_hba_policies* | 403 | Der MVPD hat aufgrund von Home-basierten Authentifizierungsrichtlinien eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben. Die aktuelle Authentifizierung wurde über einen Home-basierten Authentifizierungsfluss abgerufen, aber das Gerät ist nicht mehr zu Hause, wenn die Autorisierung für die angegebene Ressource angefordert wird. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                              | *authorization_denied_by_session_invalidated* | 403 | Die Authentifizierungssitzung wurde vom Identitäts-Provider ungültig gemacht. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                              | *identity_not_acknowledged_by_mvpd* | 403 | Die Autorisierungsanfrage schlug fehl, weil die Benutzeridentität vom MVPD nicht erkannt wurde. |
| **retry** | *network_received_error* | 403 | Beim Abrufen der Antwort vom zugehörigen Partnerdienst trat ein Lesefehler auf. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |
|                              | *network_connection_timeout* | 403 | Es gab einen Verbindungstimeout mit dem zugehörigen Partnerdienst. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |
|                              | *maximum_execution_time_exceeded* | 403 | Die Anfrage wurde in der maximal zulässigen Zeit nicht abgeschlossen. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |

## Reaktionsverwaltung {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Es gibt erweiterte Fehlercodes, die automatisch im Clientanwendungs-Code verarbeitet werden können, z. B. einen erneuten Versuch mit einer Autorisierungsanfrage im Fall einer Netzwerk-Zeitüberschreitung oder eine erneute Authentifizierung des Benutzers erfordern, wenn seine Sitzung abgelaufen ist. Für andere Typen sind jedoch möglicherweise Konfigurationsänderungen oder die Interaktion des Adobe Pass Authentication-Kundendienstteams erforderlich.
>
> <br/>
>
> Daher ist es wichtig, beim Erstellen eines Tickets über unsere [Zendesk](https://adobeprimetime.zendesk.com) vollständige Fehlerinformationen zu sammeln und bereitzustellen, um sicherzustellen, dass die erforderlichen Änderungen vorgenommen werden, bevor die neue Anwendung oder neue Funktion gestartet wird.

Zusammenfassend sollten Sie beim Umgang mit Antworten, die erweiterte Fehlercodes enthalten, Folgendes beachten:

1. **Überprüfen Sie beide Statuswerte**: Überprüfen Sie immer den HTTP-Antwortstatus-Code und das Feld &quot;Status&quot;des erweiterten Fehlercodes. Sie unterscheiden sich möglicherweise und bieten wertvolle Informationen.

1. **Fehlerinformationen auf oberster Ebene und Fehlerinformationen auf Elementebene**: Behandeln Sie Fehlerinformationen auf oberster Ebene und auf Elementebene unabhängig von der Art und Weise, wie sie kommuniziert werden, und stellen Sie sicher, dass Sie beide Formen der Übermittlung von erweiterten Fehlercodes handhaben können.

1. **Logik wiederholen**: Stellen Sie bei Fehlern, für die ein erneuter Versuch erforderlich ist, sicher, dass erneute Versuche mit exponentiellem Backoff durchgeführt werden, um zu verhindern, dass der Server überlastet wird. Außerdem sollten Sie bei Adobe Pass-Authentifizierungs-APIs, die mehrere Elemente gleichzeitig verarbeiten (z. B. Vorabautorisierungs-API), in die wiederholte Anfrage nur die Elemente einschließen, die mit &quot;Wiederholen&quot;markiert sind, und nicht die gesamte Liste.

1. **Konfigurationsänderungen**: Stellen Sie bei Fehlern, die Konfigurationsänderungen erfordern, sicher, dass die erforderlichen Änderungen vorgenommen werden, bevor Sie die neue Anwendung oder Funktion starten.

1. **Authentifizierung und Autorisierung**: Bei Fehlern im Zusammenhang mit Authentifizierung und Autorisierung müssen Sie den Benutzer auffordern, sich bei Bedarf erneut zu authentifizieren oder eine neue Autorisierung einzuholen.

1. **Benutzer-Feedback**: Optional können Sie die für Menschen lesbaren Felder &quot;Meldung&quot;und (potenziellen) &quot;Details&quot;verwenden, um den Benutzer über das Problem zu informieren. Die &quot;Details&quot;-Textnachricht kann von den MVPD-Vorabautorisierungs- oder Autorisierungsendpunkten oder vom Programmierer bei Anwendung der Abbauregeln weitergeleitet werden.
