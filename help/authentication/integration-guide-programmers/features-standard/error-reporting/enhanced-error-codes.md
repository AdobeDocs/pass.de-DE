---
title: Verbesserte Fehler-Codes
description: Verbesserte Fehler-Codes
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 27aaa0d3351577e60970a4035b02d814f0a17e2f
workflow-type: tm+mt
source-wordcount: '2649'
ht-degree: 3%

---

# Verbesserte Fehler-Codes {#enhanced-error-codes}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Erweiterte Fehler-Codes stellen eine Adobe Pass-Authentifizierungsfunktion dar, die zusätzliche Fehlerinformationen für in integrierte Client-Anwendungen bereitstellt:

* Adobe Pass Authentication REST-APIs:
   * [REST API v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [(Legacy) REST-API v1](../../legacy/rest-api-v1/rest-api-overview.md)
* Adobe Pass-Authentifizierungs-SDKs autorisieren die API vorab:
   * [(Legacy) JavaScript SDK (API vorautorisieren)](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [(Legacy) iOS/tvOS SDK (API vorautorisieren)](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [(Legacy) Android SDK (API vorautorisieren)](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _(*) Preauthorize API ist die einzige SDK-API zur Adobe Pass-Authentifizierung, die Unterstützung für erweiterte Fehler-Codes bietet._

>[!IMPORTANT]
>
> Anwendungen, die die Adobe Pass-Authentifizierungs-REST-API v2 integrieren, profitieren standardmäßig von erweiterten Fehler-Codes, ohne dass eine zusätzliche Konfiguration erforderlich ist.
>
> <br/>
>
> Anwendungen, die die Adobe Pass Authentication REST API v1 oder SDKs integrieren, die die API vorautorisieren, können nur dann von erweiterten Fehler-Codes profitieren, wenn die Funktion explizit aktiviert ist.
>
> <br/>
>
> Um diese Funktion explizit zu aktivieren, erstellen Sie ein Ticket über unseren [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren Technical Account Manager (TAM) um Hilfe.

## Darstellung {#enhanced-error-codes-representation}

Erweiterte Fehler-Codes können je nach integrierter Adobe Pass-Authentifizierungs-API und dem verwendeten Kopfzeilenwert „Accept“ (d. h. `application/json` oder `application/xml`) im `JSON`- oder `XML`-Format dargestellt werden:

| Adobe Pass-Authentifizierungs-API | JSON | XML |
|-------------------------------|---------|---------|
| REST API v2 | &check; |         |
| REST-API v1 | &check; | &check; |
| SDKs autorisieren die API vorab | &check; |         |

>[!IMPORTANT]
>
> Die Adobe Pass-Authentifizierung kann Clientanwendungen erweiterte Fehler-Codes auf zwei Arten übermitteln:
>
> <br/>
>
> * **Fehlerinformationen der obersten Ebene**: In diesem Fall befindet sich das ***&quot;***&quot; auf der obersten Ebene. Daher kann der Antworttext nur das Objekt &quot;***&quot;*** enthalten.
> * **Fehlerinformationen auf Elementebene**: In diesem Fall befindet sich das ***error“***-Objekt auf Elementebene, daher kann der Antworttext ein ***„error“***-Objekt für alle Elemente enthalten, bei denen während der Wartung ein Fehler aufgetreten ist.
>
> <br/>
>
> Lesen Sie die öffentliche Dokumentation für jede integrierte Adobe Pass-Authentifizierungs-API, um die Besonderheiten der erweiterten Fehlercode-Darstellung zu ermitteln.

**REST API v2**

Siehe die folgenden HTTP-Antworten mit Beispielen für erweiterte Fehler-Codes, die `JSON` für die REST-API v2 gelten.

>[!BEGINTABS]

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
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
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
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!ENDTABS]

**REST-API v1**

Siehe die folgenden HTTP-Antworten mit Beispielen für erweiterte Fehler-Codes, die als `JSON` oder `XML` für die REST-API v1 dargestellt werden.

>[!BEGINTABS]

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
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v1 - Fehlerinformationen der obersten Ebene (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
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
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!ENDTABS]

### Struktur {#enhanced-error-codes-representation-structure}

Erweiterte Fehler-Codes enthalten die folgenden `JSON` Felder oder `XML` Attribute mit Beispielen:

| -Name | Typ | Beispiel | eingeschränkt | Beschreibung |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *Aktion* | *Zeichenfolge* | *Keine* | &check; | Die von der Adobe Pass-Authentifizierung empfohlene Aktion, die die in diesem Dokument definierte Situation beheben kann. <br/><br/> Weitere Informationen finden Sie im Abschnitt [Aktion](#enhanced-error-codes-action) . |
| *status* | *Integer* | *403* | &check; | Der HTTP-Antwort-Status-Code wie im Dokument [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6) definiert. <br/><br/> Weitere Informationen finden Sie im Abschnitt [Status](#enhanced-error-codes-status) . |
| *code* | *Zeichenfolge* | *authorization_denied_by_mvpd* | &check; | Der eindeutige Kennungs-Code der Adobe Pass-Authentifizierung, der mit dem Fehler verknüpft ist, wie in diesem Dokument definiert. <br/><br/> Weitere Informationen finden Sie im Abschnitt [Code](#enhanced-error-codes-code) . |
| *message* | *Zeichenfolge* | *Die MVPD hat bei der Anforderung einer Autorisierung für die angegebene Ressource eine Entscheidung „Ablehnen“ zurückgegeben* |            | Die für Menschen lesbare Nachricht, die dem Endbenutzer in einigen Fällen angezeigt werden kann. <br/><br/> Weitere Informationen finden Sie im Abschnitt [Antwortverarbeitung](#enhanced-error-codes-response-handling) . |
| *Details* | *Zeichenfolge* | *Ihr Abonnementpaket enthält nicht den Kanal „Live“* |            | Die detaillierte Nachricht, die in einigen Fällen von einem Service-Partner bereitgestellt werden kann. <br/><br/> Dieses Feld ist möglicherweise nicht vorhanden, wenn der Service-Partner keine benutzerdefinierte Nachricht bereitstellt. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de* |            | Die öffentliche Dokumentations-URL zur Adobe Pass-Authentifizierung, die auf weitere Informationen über die Gründe für diesen Fehler und mögliche Lösungen verweist. <br/><br/> Dieses Feld enthält eine absolute URL und sollte nicht aus dem Fehlercode abgeleitet werden. Abhängig vom Fehlerkontext kann eine andere URL angegeben werden. |
| *trace* | *Zeichenfolge* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | Die eindeutige Kennung für die Antwort, die bei der Kontaktaufnahme mit dem Adobe Pass-Authentifizierungsdienst zur Behebung bestimmter Probleme verwendet werden kann. |

>[!IMPORTANT]
>
> Die Spalte **Eingeschränkt** gibt an, ob das entsprechende Feld einen Wert aus einem endlichen Satz enthält, während nicht eingeschränkte Felder beliebige Daten enthalten können.
>
> <br/>
>
> Zukünftige Aktualisierungen dieses Dokuments könnten Werte zu den endlichen Mengen hinzufügen, werden jedoch vorhandene nicht entfernen oder ändern.

### Aktion {#enhanced-error-codes-representation-action}

Erweiterte Fehler-Codes enthalten ein Feld „Aktion“, das eine empfohlene Aktion bereitstellt, mit der die Situation behoben werden kann.

Zu den möglichen Werten für das Feld „Aktion“ gehören:

| Aktion | Beschreibung | Kategorie |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| Keine | Es gibt keine vordefinierte Aktion, um dieses Problem zu beheben, aber in einigen Fällen kann dies auf einen falschen Aufruf der API hinweisen. | Korrigieren Sie den Anfragekontext. |
| Konfiguration | Die Client-Anwendung erfordert eine Konfigurationsänderung, die meistens über das TVE-Dashboard von Adobe Pass durchgeführt wird. | Korrigieren Sie den Integrationskonfigurationskontext. |
| application-registration | Die Client-Anwendung muss sich erneut registrieren. | Korrigieren Sie den Client-Anwendungskontext. |
| Authentifizierung | Die Client-Anwendung muss den Benutzer authentifizieren oder erneut authentifizieren. | Korrigieren Sie den Client-Anwendungskontext. |
| Autorisierung | Die Client-Anwendung benötigt eine Autorisierung für die angegebene Ressource. | Korrigieren Sie den Client-Anwendungskontext. |
| Erneut versuchen | Die Client-Anwendung benötigt, um die Anfrage erneut auszuführen. | Korrigieren Sie den Anfragekontext. |

_(*) Bei einigen Fehlern können mehrere Aktionen mögliche Lösungen sein, aber das Feld „Aktion“ zeigt die Aktion mit der höchsten Wahrscheinlichkeit an, den Fehler zu beheben._

### Status {#enhanced-error-codes-representation-status}

Erweiterte Fehler-Codes enthalten ein Feld „Status“, das den mit dem Fehler verknüpften HTTP-Status-Code angibt.

Zu den möglichen Werten für das Feld „Status“ gehören:

| Code | Begründungsphrase |
|------|-----------------------|
| 400 | Fehlerhafte Anfrage |
| 401 | Nicht autorisiert |
| 403 | Verboten |
| 404 | Nicht gefunden |
| 405 | Methode nicht zulässig |
| 410 | Abgelaufen |
| 412 | Voraussetzung fehlgeschlagen |
| 500 | Interner Server-Fehler |

Erweiterte Fehler-Codes mit einem 4xx „Status“ erscheinen in der Regel, wenn der Fehler vom Client erzeugt wird und in den meisten Fällen bedeutet dies, dass der Client zusätzliche Arbeit benötigt, um ihn zu beheben.

Erweiterte Fehler-Codes mit einem 5xx „Status“ erscheinen normalerweise, wenn der Fehler vom Server erzeugt wird, und in den meisten Fällen bedeutet dies, dass der Server zusätzliche Arbeit benötigt, um ihn zu beheben.

>[!IMPORTANT]
>
> Es gibt Fälle, in denen sich der HTTP-Antwortstatus-Code vom Feld „Status“ des erweiterten Fehlercodes unterscheidet, insbesondere bei der Interaktion mit einer Adobe Pass-Authentifizierungs-API, die erweiterte Fehlercodes als Fehlerinformationen auf Elementebene übermittelt.

### Code {#enhanced-error-codes-representation-code}

Erweiterte Fehler-Codes enthalten ein Feld „Code“, das eine eindeutige Kennung für die Adobe Pass-Authentifizierung bereitstellt, die mit dem Fehler verknüpft ist.

Die möglichen Werte für das Feld „Code“ werden ([ unten) ](#enhanced-error-codes-list) zwei Listen aggregiert, die auf der integrierten Adobe Pass-Authentifizierungs-API basieren.

## Listen {#enhanced-error-codes-lists}

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

In der folgenden Tabelle sind mögliche erweiterte Fehler-Codes aufgeführt, auf die eine Client-Anwendung stoßen kann, wenn sie in die Adobe Pass Authentication REST API v2 integriert ist.

| Aktion | Code | Status | Nachricht |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Keine** | *invalid_parameter_service_provider* | 400 | Der Parameterwert des Dienstleisters fehlt oder ist ungültig. |
|                              | *invalid_parameter_mvpd* | 400 | Der mvpd-Parameterwert fehlt oder ist ungültig. |
|                              | *invalid_parameter_code* | 400 | Der Code-Parameterwert fehlt oder ist ungültig. |
|                              | *invalid_parameter_resources* | 400 | Der Ressourcenparameterwert fehlt oder ist ungültig. |
|                              | *invalid_parameter_redirect_url* | 400 | Der Parameterwert der Umleitungs-URL fehlt oder ist ungültig. |
|                              | *invalid_parameter_partner* | 400 | Der Parameterwert des Partners fehlt oder ist ungültig. |
|                              | *invalid_parameter_saml_response* | 400 | Der SAML-Antwortparameterwert fehlt oder ist ungültig. |
|                              | *invalid_header_device_info* | 400 | Der Kopfzeilenwert für die Geräteinformationen fehlt oder ist ungültig. |
|                              | *invalid_header_device_identifier* | 400 | Der Kopfzeilenwert der Gerätekennung fehlt oder ist ungültig. |
|                              | *invalid_header_identity_for_temporary_access* | 400 | Die Identität für den Header für den temporären Zugriff fehlt oder ist ungültig. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | Der Wert für den Zugriffsberechtigungsstatus in der Statuskopfzeile des Partner-Frameworks ist nicht vorhanden. |
|                              | *invalid_header_pfs_permission_access_not_determine* | 400 | Der Wert für den Zugriffsberechtigungsstatus in der Statuskopfzeile des Partner-Frameworks wird nicht bestimmt. |
|                              | *invalid_header_pfs_permission_access_not_granted* | 400 | Der Zugriffsstatuswert für die Berechtigung aus der Statuskopfzeile des Partner-Frameworks wird nicht gewährt. |
|                              | *invalid_header_pfs_provider_id_not_determine* | 400 | Der Provider-ID-Wert aus der Status-Kopfzeile des Partner-Frameworks ist keinem bekannten MVPD zugeordnet. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | Der Provider-ID-Wert aus der Status-Kopfzeile des Partner-Frameworks stimmt nicht mit dem als Parameter gesendeten MVPD überein. |
|                              | *invalid_header_pfs_provider_info_expi* | 400 | Die Anbieterinformationen aus der Statuskopfzeile des Partner-Frameworks sind abgelaufen. |
|                              | *invalid_integration* | 400 | Die Integration zwischen dem angegebenen Dienstleister und mvpd existiert nicht oder ist deaktiviert. |
|                              | *invalid_authentication_session* | 400 | Die dieser Anfrage zugeordnete Authentifizierungssitzung fehlt oder ist ungültig. |
|                              | *preauthorization_denied_by_mvpd* | 403 | Die MVPD hat eine Entscheidung „Ablehnen“ zurückgegeben, wenn sie eine Vorabautorisierung für die angegebene Ressource anfordert. |
|                              | *authorization_denied_by_mvpd* | 403 | Die MVPD hat bei der Anforderung einer Autorisierung für die angegebene Ressource eine Entscheidung „Ablehnen“ zurückgegeben. |
|                              | *authorization_denied_by_parent_Controls* | 403 | Die MVPD hat eine Entscheidung „Ablehnen“ aufgrund der Einstellungen der Kindersicherung für die angegebene Ressource zurückgegeben. |
|                              | *authorization_denied_by_egradation_rule* | 403 | Bei der Integration zwischen dem angegebenen Dienstleister und mvpd wird eine Degradationsregel angewendet, die die Autorisierung für die angeforderten Ressourcen verweigert. |
|                              | *internal_server_error* | 500 | Die Anfrage ist aufgrund eines internen Server-Fehlers fehlgeschlagen. |
| **Konfiguration** | *too_many_resources* | 403 | Die Autorisierungs- oder Vorabautorisierungsanfrage ist fehlgeschlagen, da zu viele Ressourcen abgefragt wurden. Bitte wenden Sie sich an das Support-Team , um die Autorisierungs- und Vorautorisierungsbeschränkungen ordnungsgemäß zu konfigurieren. |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | Die Konfiguration des Benutzermetadaten-Zertifikats fehlt oder ist ungültig. |
|                              | *invalid_configuration_temporary_access* | 500 | Die Konfiguration für den temporären Zugriff ist ungültig. |
|                              | *invalid_configuration_platform* | 500 | Die Plattform-Konfiguration fehlt oder ist für die Integration ungültig. |
|                              | *invalid_configuration_platform_id* | 500 | Die Plattform-ID-Konfiguration fehlt oder ist ungültig. |
|                              | *invalid_configuration_platform_trait* | 500 | Die Konfiguration der Plattformeigenschaften fehlt oder ist ungültig. |
|                              | *invalid_configuration_platform_category_trait* | 500 | Die Konfiguration der Plattformkategorie-Eigenschaft fehlt oder ist ungültig. |
|                              | *invalid_configuration_platform_services* | 500 | Die Konfiguration der Platform-Services fehlt oder ist für die Integration ungültig. |
|                              | *invalid_configuration_mvpd_platform* | 500 | Die Konfiguration der mvpd-Plattform fehlt oder ist für mvpd und Plattform ungültig. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | Die Konfiguration des Status des MVPD-Plattform-Boarding fehlt oder ist für MVPD und Plattform ungültig. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | Die Konfiguration für den Austausch von MVPD-Plattformprofilen fehlt oder ist für MVPD und Plattform ungültig. |
| **application-registration** | *invalid_access_token_service_provider* | 401 | Das Zugriffstoken ist aufgrund eines ungültigen Dienstleisters ungültig. |
|                              | *invalid_access_token_client_application* | 401 | Das Zugriffstoken ist aufgrund einer ungültigen Client-Anwendung ungültig. |
| **Authentifizierung** | *AUTHENTICATED_PROFILE_MISS* | 403 | Das dieser Anfrage zugeordnete authentifizierte Profil fehlt. |
|                              | *AUTHENTICATED_PROFILE_EXPIRED* | 403 | Das mit dieser Anfrage verknüpfte authentifizierte Profil ist abgelaufen. |
|                              | *AUTHENTICATED_PROFILE_INVALIDED* | 403 | Das mit dieser Anfrage verknüpfte authentifizierte Profil wurde ungültig gemacht. |
|                              | *TEMPORARY_ACCESS_DURATION_LIMIT_EXCEEDED* | 403 | Die maximale Dauer des temporären Zugriffs wurde überschritten. |
|                              | *TEMPORARY_ACCESS_RESOURCES_LIMIT_EXCEEDED* | 403 | Das temporäre Ressourcenlimit für den Zugriff wurde überschritten. |
|                              | *authorization_denied_by_hba_policies* | 403 | Die MVPD hat eine Entscheidung „Ablehnen“ aufgrund von einheimischen Authentifizierungsrichtlinien zurückgegeben. Die aktuelle Authentifizierung wurde über einen Startseiten-basierten Authentifizierungsfluss abgerufen, und das Gerät ist nicht mehr zu Hause, wenn eine Autorisierung für die angegebene Ressource angefordert wird. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                              | *authorization_denied_by_session_invalidated* | 403 | Die Authentifizierungssitzung wurde vom Identitätsanbieter ungültig gemacht. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                              | *identity_not_Recognized_by_mvpd* | 403 | Die Autorisierungsanfrage schlug fehl, da die Benutzeridentität von MVPD nicht erkannt wurde. |
| **Erneut versuchen** | *NETWORK_RECEIVED_ERROR* | 403 | Beim Abrufen der Antwort vom zugehörigen Partner-Service ist ein Lesefehler aufgetreten. Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden. |
|                              | *network_connection_timeout* | 403 | Es gab ein Verbindungs-Timeout mit dem zugehörigen Partner-Service. Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden. |
|                              | *maximum_execution_time_exceeded* | 403 | Die Anfrage wurde nicht in der maximal zulässigen Zeit abgeschlossen. Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden. |

### REST-API v1 {#enhanced-error-codes-lists-rest-api-v1}

In der folgenden Tabelle sind mögliche erweiterte Fehler-Codes aufgeführt, auf die eine Client-Anwendung stoßen kann, wenn sie in die Adobe Pass Authentication REST API v1 integriert ist.

| Aktion | Code | Status | Nachricht |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Keine** | *invalid_Requestor* | 400 | Der Anforderungsparameter fehlt oder ist ungültig. |
|                    | *invalid_device_info* | 400 | Die Geräteinformationen fehlen oder sind ungültig. |
|                    | *invalid_device_id* | 400 | Die Gerätekennung fehlt oder ist ungültig. |
|                    | *missing_resource* | 400 412 | Der Ressourcenparameter fehlt. |
|                    | *malformed_authz_request* | 400 412 | Autorisierungsanfrage ist null oder ungültig. |
|                    | *preauthorization_denied_by_mvpd* | 403 | Die MVPD hat eine Entscheidung „Ablehnen“ zurückgegeben, wenn sie eine Vorabautorisierung für die angegebene Ressource anfordert. |
|                    | *authorization_denied_by_mvpd* | 403 | Die MVPD hat bei der Anforderung einer Autorisierung für die angegebene Ressource eine Entscheidung „Ablehnen“ zurückgegeben. |
|                    | *authorization_denied_by_parent_Controls* | 403 | Die MVPD hat eine Entscheidung „Ablehnen“ aufgrund der Einstellungen der Kindersicherung für die angegebene Ressource zurückgegeben. |
|                    | *internal_error* | 400, 405, 500 | Die Anfrage ist aufgrund eines internen Server-Fehlers fehlgeschlagen. |
| **Konfiguration** | *UNKNOWN_INTEGRATION* | 400 412 | Die Integration zwischen dem angegebenen Programmierer und dem Identitätsanbieter existiert nicht. Verwenden Sie das TVE-Dashboard, um die erforderliche Integration zu erstellen. |
|                    | *too_many_resources* | 403 | Die Autorisierungs- oder Vorabautorisierungsanfrage ist fehlgeschlagen, da zu viele Ressourcen abgefragt wurden. Bitte wenden Sie sich an das Support-Team , um die Autorisierungs- und Vorautorisierungsbeschränkungen ordnungsgemäß zu konfigurieren. |
| **Authentifizierung** | *AUTHENTICATION_SESSION_ISEMITTER_MISMATCH* | 400 | Die Autorisierungsanfrage schlug fehl, da sich der angegebene MVPD für den Autorisierungsfluss von dem unterscheidet, der die Authentifizierungssitzung ausgestellt hat. Die Benutzerin bzw. der Benutzer muss sich erneut bei der gewünschten MVPD authentifizieren, um fortfahren zu können. |
|                    | *authorization_denied_by_hba_policies* | 403 | Die MVPD hat eine Entscheidung „Ablehnen“ aufgrund von einheimischen Authentifizierungsrichtlinien zurückgegeben. Die aktuelle Authentifizierung wurde mithilfe eines Home-Based Authentication Flow (HBA) abgerufen, aber das Gerät ist nicht mehr zu Hause, wenn es eine Autorisierung für die angegebene Ressource anfordert. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *authorization_denied_by_session_invalidated* | 403 | Die Authentifizierungssitzung wurde vom Identitätsanbieter ungültig gemacht. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *identity_not_Recognized_by_mvpd* | 403 | Die Autorisierungsanfrage schlug fehl, da die Benutzeridentität von MVPD nicht erkannt wurde. |
|                    | *AUTHENTICATION_SESSION_INVALIDATED* | 403 | Die Authentifizierungssitzung wurde vom Identitätsanbieter ungültig gemacht. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *AUTHENTICATION_SESSION_MISS* | 403 412 | Die mit dieser Anfrage verknüpfte Authentifizierungssitzung konnte nicht abgerufen werden. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *AUTHENTICATION_SESSION_EXPIRED* | 403 412 | Die aktuelle Authentifizierungssitzung ist abgelaufen. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *preauthorization_authentication_session_missing* | 412 | Die mit dieser Anfrage verknüpfte Authentifizierungssitzung konnte nicht abgerufen werden. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|                    | *PREAUTHORIZATION_AUTHENTICATION_SESSION_EXPIRED* | 412 | Die aktuelle Authentifizierungssitzung ist abgelaufen. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
| **Autorisierung** | *authorization_not_found* | 403 404 | Für die angegebene Ressource wurde keine Autorisierung gefunden. Der Benutzer muss eine neue Autorisierung erhalten, um fortfahren zu können. |
|                    | *authorization_expi* | 410 | Die vorherige Authentifizierung für die angegebene Ressource ist abgelaufen. Der Benutzer muss eine neue Autorisierung erhalten, um fortfahren zu können. |
| **Erneut versuchen** | *NETWORK_RECEIVED_ERROR* | 403 | Beim Abrufen der Antwort vom zugehörigen Partner-Service ist ein Lesefehler aufgetreten. Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden. |
|                    | *network_connection_timeout* | 403 | Es gab ein Verbindungs-Timeout mit dem zugehörigen Partner-Service. Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden. |
|                    | *maximum_execution_time_exceeded* | 403 | Die Anfrage wurde nicht in der maximal zulässigen Zeit abgeschlossen. Das Problem kann möglicherweise durch Wiederholen der Anfrage behoben werden. |

### SDKs autorisieren die API vorab {#enhanced-error-codes-lists-sdks-preauthorize-api}

Im vorherigen [ finden Sie ](#enhanced-error-codes-list-rest-api-v1) möglichen erweiterten Fehler-Codes, auf die eine Client-Anwendung stoßen könnte, wenn sie mit Adobe Pass Authentication SDKs integriert wird, die die API vorautorisieren.

## Umgang mit Antworten {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Es gibt erweiterte Fehler-Codes, die automatisch im Client-Anwendungs-Code verarbeitet werden können, z. B. das Wiederholen einer Autorisierungsanfrage im Falle einer Netzwerk-Zeitüberschreitung oder die Anforderung, dass sich der Benutzer nach Ablauf der Sitzung erneut authentifiziert, aber andere Typen erfordern möglicherweise Konfigurationsänderungen oder die Interaktion des Kundenunterstützungs-Teams für die Adobe Pass-Authentifizierung.
>
> <br/>
>
> Daher ist es wichtig, bei der Erstellung eines Tickets über unseren [Zendesk](https://adobeprimetime.zendesk.com) vollständige Fehlerinformationen zu erfassen und bereitzustellen, um sicherzustellen, dass die erforderlichen Änderungen vorgenommen werden, bevor die neue Anwendung oder neue Funktion gestartet wird.

Zusammenfassend sollten Sie beim Umgang mit Antworten mit erweiterten Fehler-Codes Folgendes beachten:

1. **Beide Statuswerte überprüfen**: Überprüfen Sie immer sowohl den HTTP-Antwort-Status-Code als auch das Feld „Status“ des erweiterten Fehler-Codes. Sie können unterschiedlich sein und beide liefern wertvolle Informationen.

1. **Unabhängig von Fehlerinformationen auf oberster Ebene und auf Elementebene**: Verarbeiten Sie Fehlerinformationen auf oberster und Elementebene unabhängig von der Art und Weise, wie sie kommuniziert werden, und stellen Sie sicher, dass Sie beide Formen der Übertragung von erweiterten Fehler-Codes verarbeiten können.

1. **Wiederholungslogik**: Stellen Sie bei Fehlern, für die ein erneuter Versuch erforderlich ist, sicher, dass weitere Versuche mit exponentiellem Backoff erfolgen, um zu vermeiden, dass der Server überlastet wird. Außerdem sollten Sie bei Adobe Pass-Authentifizierungs-APIs, die mehrere Elemente gleichzeitig verarbeiten (z. B. die Vorabautorisierungs-API), in die wiederholte Anfrage nur die Elemente einbeziehen, die mit „Wiederholen“ gekennzeichnet sind, und nicht die gesamte Liste.

1. **Konfigurationsänderungen**: Stellen Sie bei Fehlern, die Konfigurationsänderungen erfordern, sicher, dass die erforderlichen Änderungen vorgenommen werden, bevor Sie das neue Programm oder die neue Funktion starten.

1. **Authentifizierung und Autorisierung**: Bei Fehlern im Zusammenhang mit der Authentifizierung und Autorisierung müssen Sie den Benutzer auffordern, sich erneut zu authentifizieren oder bei Bedarf eine neue Autorisierung zu erhalten.

1. **Benutzer-Feedback**: Optional können Sie die Felder „Nachricht“ und (potenzielle) „Details“ in menschenlesbarer Form verwenden, um den Benutzer über das Problem zu informieren. Die Textnachricht „Details“ wird möglicherweise von den Vorautorisierungs- oder Autorisierungsendpunkten von MVPD oder vom Programmierer übergeben, wenn Abbauregeln angewendet werden.
