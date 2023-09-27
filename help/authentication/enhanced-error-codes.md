---
title: Verbesserte Fehlercodes
description: Verbesserte Fehlercodes
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 40aeba47c293f2cc4569f01a6fb1ca4bfbcd0de6
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 2%

---

# Verbesserte Fehlercodes {#enhanced-error-codes}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

In diesem Dokument werden die Liste der API-Fehlercodes und zusätzliche Fehlerinformationen beschrieben, die an die Anwendungen zurückgegeben werden.

Um in der Programmeranwendung Enhanced Error Codes zu verwenden, muss eine Anfrage an das Supportteam gestellt werden, damit es mit einer Konfigurationsänderung aktiviert werden kann.

## Umgang mit Antwortfehlern {#response-error-handling}

In den meisten Fällen enthält die Adobe Pass-Authentifizierungs-API zusätzliche Fehlerinformationen im Antworttext, um **aussagekräftiger Kontext** , um herauszufinden, warum ein bestimmter Fehler aufgetreten ist und/oder mögliche Lösungen zur automatischen Behebung des Problems.  *In bestimmten Fällen, bei denen es sich um Authentifizierungs- oder Abmeldevorgänge handelt, geben die Adobe Pass-Authentifizierungsdienste möglicherweise eine HTML-Antwort oder einen leeren Text zurück. Weitere Informationen finden Sie in der API-Dokumentation .*

Während bestimmte Fehlertypen automatisch verarbeitet werden können (z. B. eine Autorisierungsanfrage im Fall einer Netzwerk-Zeitüberschreitung erneut auszuführen oder eine erneute Authentifizierung des Benutzers zu verlangen, wenn seine Sitzung abgelaufen ist), erfordern andere Typen möglicherweise Konfigurationsänderungen oder die Interaktion des Kundenbetreuungsteams. Es ist wichtig, dass Programmierer in solchen Fällen vollständige Fehlerinformationen erfassen und bereitstellen.

Die Adobe Pass Authentication API gibt HTTP-Status-Codes im Bereich 400-500 zurück, um Fehler oder Fehler anzuzeigen. Jeder HTTP-Status-Code hat bestimmte Auswirkungen:

- Die 4xx-Fehlercodes implizieren, dass der Fehler vom Client generiert wird und der Client zusätzliche Schritte ausführen muss, um ihn zu beheben (z. B. Abrufen eines Zugriffstokens, bevor geschützte APIs aufgerufen werden, oder Bereitstellung erforderlicher Parameter)

- Die 5xx-Fehlercodes implizieren, dass der Fehler vom Server erzeugt wird und dass der Server zusätzliche Arbeit zur Behebung des Problems benötigt.

Die zusätzlichen Fehlerinformationen sind im Feld &quot;Fehler&quot;im Antworttext enthalten.

<table>
<thead>
  <tr>
    <th>Name</th>
    <th>Typ</th>
    <th>Beispiel</th>
    <th>Beschreibung</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>error</td>
    <td><i>Objekt</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://tve.helpdocsonline.com/errors<br>&nbsp;&nbsp;&nbsp;&nbsp;/network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://tve.helpdocsonline.com/errors/network_connection_failure&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>Bezieht sich auf Sammlungs- oder Fehlerobjekte, die beim Versuch erfasst wurden, die Anfrage abzuschließen.</td>
  </tr>
</tbody>
</table>

</br>

Adobe Pass-APIs, die mehrere Elemente verarbeiten (API für die Vorabautorisierung usw.), können mithilfe von Fehlerinformationen auf Elementebene anzeigen, ob die Verarbeitung für ein bestimmtes Element fehlgeschlagen ist und für andere Elemente erfolgreich war. In diesem Fall wird die ***&quot;error&quot;*** -Objekt befindet sich auf der Elementebene und der Antworttext kann mehrere ***&quot;errors&quot;*** Objekte - lesen Sie bitte die API-Dokumentation.

</br>

**Beispiel mit Teilerfolg und Fehler auf Artikelebene**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://tve.helpdocsonline.com/errors/network_connection_failure",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

Jedes Fehlerobjekt verfügt über die folgenden Parameter:

| Name | Typ | Beispiel | Beschränkt | Beschreibung |
|---|---|----|:---:|---|
| *status* | *integer* | *403* | &amp;check; | Der Antwort-HTTP-Statuscode, wie in RFC 7231 dokumentiert (<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400 Ungültige Anfrage</li><li>401 Nicht autorisiert</li><li>403 Verboten</li><li>404 Nicht gefunden</li><li>405 Methode nicht zulässig</li><li>409 Konflikt</li><li>410 Stück</li><li>412 Vorbedingung fehlgeschlagen</li><li>429 Zu viele Anfragen</li><li>500 Interval server error</li><li>503 Dienst nicht verfügbar</li></ul> |
| *code* | *Zeichenfolge* | *network_connection_failure* | &amp;check; | Der standardmäßige Adobe Pass-Authentifizierungsfehlercode. Die vollständige Liste der Fehler-Codes finden Sie unten. |
| *message* | *Zeichenfolge* | *Der TV-Provider kann nicht kontaktiert werden.* | | Vom Menschen lesbare Nachricht, die dem Endbenutzer angezeigt werden kann. |
| *details* | *Zeichenfolge* | *Ihr Abonnement-Paket enthält keinen Live-Kanal* | | In einigen Fällen wird eine detaillierte Meldung von den MVPD-Autorisierungsendpunkten oder vom Programmierer durch Abbauregeln bereitgestellt. <p> Beachten Sie, dass dieses Feld möglicherweise nicht in den Fehlerfeldern vorhanden ist, wenn von den Partnerdiensten keine benutzerdefinierte Nachricht empfangen wurde. |
| *helpUrl* | *url* | &quot;`http://`&quot; | | Eine URL, die Links zu weiteren Informationen über den Grund dieses Fehlers und mögliche Lösungen enthält. <p>Der URI stellt eine absolute URL dar und sollte nicht aus dem Fehlercode abgeleitet werden. Je nach Fehlerkontext kann eine andere URL angegeben werden. Beispielsweise liefert derselbe &quot;bad_request&quot;-Fehlercode verschiedene URLs für Authentifizierungs- und Autorisierungsdienste. |
| *trace* | *Zeichenfolge* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | Eine eindeutige Kennung für diese Antwort, die verwendet werden kann, wenn der Support kontaktiert wird, um bestimmte Probleme in komplexeren Szenarien zu identifizieren. |
| *action* | *Zeichenfolge* | *Wiederholen* | &amp;check; | Empfohlene Maßnahmen zur Behebung der Situation: <ul><li> *Keine* - Leider gibt es keine vordefinierten Maßnahmen, um dieses Problem zu beheben. Dies könnte auf einen falschen Aufruf der öffentlichen API hindeuten</li><li>*Konfiguration* - Eine Konfigurationsänderung ist über das TVE-Dashboard oder durch Kontaktaufnahme mit dem Support erforderlich. </li><li>*application-registration* - Die Anmeldung muss sich selbst registrieren. </li><li>*Authentifizierung* - Der Benutzer muss sich authentifizieren oder erneut authentifizieren. </li><li>*Autorisierung* - Der Benutzer muss die Autorisierung für die jeweilige Ressource einholen. </li><li>*Abbau* - Eine gewisse Abbauweise sollte angewendet werden. </li><li>*Wiederholen* - Ein erneuter Versuch mit der Anfrage kann das Problem lösen.</li><li>*Wiederholen* - Das Problem kann durch Wiederholen der Anfrage nach dem angegebenen Zeitraum behoben werden.</li></ul> |

</br>

**Hinweise:**

- ***Beschränkt*** column *gibt an, ob der entsprechende Feldwert einen endlichen Satz darstellt* (z. B. vorhandene HTTP-Status-Codes für &quot;*status*&quot;). Zukünftige Aktualisierungen dieser Spezifikation könnten Werte zur eingeschränkten Liste hinzufügen, vorhandene Werte werden jedoch nicht entfernt oder geändert. Unbeschränkte Felder können in der Regel beliebige Daten enthalten, es gibt jedoch Einschränkungen, um eine angemessene Größe sicherzustellen.

- Jede Adobe-Antwort enthält eine &quot;Adobe-Request-Id&quot;, die die Client-Anfrage über unsere HTTP-Dienste hinweg identifiziert. Die &quot;**trace**&quot; -Feld ergänzt dies und sollte zusammen gemeldet werden.

## HTTP-Status-Codes und Fehlercodes {#http-status-codes-and-error-codes}

Die Inkonsistenzen zwischen verschiedenen Fehlercodes und den zugehörigen HTTP-Status-Codes sind auf die Abwärtskompatibilitätsanforderungen bei älteren SDK und Anwendungen zurückzuführen (z. B. *unknown\_application* gibt 400 Bad Request zurück, während *unknown\_software\_statement* Erträge 401 Nicht autorisiert). Die Behebung dieser Inkonsistenzen wird in künftigen Ausführungen angestrebt.

## Aktionen und Fehlercodes {#actions-and-error-codes}

Für die meisten Fehler-Codes können mehrere Aktionen als Pfade zur Behebung des vorliegenden Problems geeignet sein, oder sogar mehrere Aktionen können erforderlich sein, um sie automatisch zu beheben. Wir haben uns dafür entschieden, die Person mit der höchsten Wahrscheinlichkeit anzugeben, den Fehler zu beheben. Die **Aktionen** kann in drei Kategorien unterteilt werden:

1. , die versuchen, den Anforderungskontext zu beheben (Wiederholen, Wiederholen)
1. , die versuchen, den Benutzerkontext innerhalb der Anwendung zu beheben (Anwendungsregistrierung, Authentifizierung, Autorisierung)
1. , die versuchen, den Integrationskontext zwischen einer Anwendung und einem Identitäts-Provider zu beheben (Konfiguration, Verschlechterung)

Für die erste Kategorie (Wiederholen und Wiederholen) reicht es möglicherweise aus, dieselbe Anfrage einfach erneut auszuführen, um das Problem zu lösen. Im Fall von APIs, die mehrere Elemente verarbeiten, sollte die Anwendung die Anfrage wiederholen und nur die Elemente mit der Aktion &quot;Wiederholen&quot;oder &quot;Wiederholen nach&quot;einschließen. Für &quot;*Wiederholen*&quot; Aktion, ein &quot;<u>Wiederholen nach</u>&quot;-Kopfzeile gibt an, wie viele Sekunden die Anwendung warten soll, bevor die Anfrage wiederholt wird.

Bei der zweiten und dritten Kategorie hängt die tatsächliche Aktionsimplementierung in hohem Maße von den Anwendungsfunktionen ab. Beispiel: &quot;*Abbau*&quot; kann entweder als &quot;Wechsel zu 15 Minuten temporären Pässe implementiert werden, um Benutzern die Wiedergabe des Inhalts zu ermöglichen&quot; oder als &quot;automatisches Tool zur Anwendung von AUTHN-ALL oder AUTHZ-ALL-Abbau für die Integration mit dem angegebenen MVPD&quot;. Ähnlich wie &quot;*Authentifizierung*&quot;Trigger einer passiven Authentifizierung (Back-Channel-Authentifizierung) auf einem Tablet und eines Vollbildauthentifizierungsflusses auf vernetzten TVs. Deshalb haben wir uns dafür entschieden, vollständige URLs mit Schema und allen Parametern bereitzustellen.

## Fehlercodes {#error-codes}

In der folgenden Tabelle sind die möglichen Fehlercodes, die zugehörigen Nachrichten und möglichen Aktionen aufgeführt.

| Aktion | Fehler-Code | HTTP-Statuscode | Beschreibung |
|---|---|---|---|
| **Keine** | *authorization_denied_by_mvpd* | 403 | Der MVPD hat eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben, wenn er eine Autorisierung für die angegebene Ressource anfordert. |
|  | *authorization_denied_by_parental_Controls* | 403 | Der MVPD hat die Entscheidung &quot;Ablehnen&quot;zurückgegeben, da die elterlichen Kontrolleinstellungen für die angegebene Ressource festgelegt wurden. |
|  | *authorization_denied_by_programmer* | 403 | Die vom Programmierer angewendete Abbauregel erzwingt eine &quot;Ablehnen&quot;-Entscheidung für den aktuellen Benutzer. |
|  | *bad_request* | 400 | Die API-Anfrage ist ungültig oder falsch gebildet. Lesen Sie die API-Dokumentation , um die Anforderungsanforderungen zu ermitteln. |
|  | *individualization_service_unavailable* | 503 | Die Anfrage schlug fehl, da der Individualisierungsdienst nicht verfügbar war. |
|  | *internal_error* | 500 | Die Anfrage schlug aufgrund eines internen Server-Fehlers fehl. |
|  | *invalid_client_time* | 400 | Der Client-Computer Datum/Uhrzeit/Zeitzone ist nicht korrekt eingestellt. Dies führt wahrscheinlich zu Authentifizierungs-/Autorisierungsfehlern. |
|  | *invalid_custom_scheme* | 400 | Das in der Anwendungsregistrierung verwendete benutzerdefinierte Schema wird nicht erkannt. Prüfen Sie die Konfiguration des TVE-Dashboards auf die korrekten benutzerdefinierten Schemawerte. |
|  | *invalid_domain* | 400 | Der Anfragende verwendet eine ungültige Domäne. Alle Domänen, die von einer bestimmten Anforderer-ID verwendet werden, müssen von Adobe auf die Whitelist gesetzt werden. |
|  | *invalid_header* | 400 | Die Anfrage schlug fehl, da sie eine ungültige Kopfzeile enthielt. Lesen Sie die API-Dokumentation, um festzustellen, welche Kopfzeilen für Ihre Anfrage gültig sind und ob Einschränkungen für ihren Wert bestehen. |
|  | *invalid_http_method* | 405 | Die mit der Anfrage verknüpfte HTTP-Methode wird nicht unterstützt. Lesen Sie die API-Dokumentation , um die unterstützten HTTP-Methoden für Ihre Anfrage zu ermitteln. |
|  | *invalid_parameter_value* | 400 | Die Anfrage schlug fehl, da sie einen ungültigen Parameter- oder Parameterwert enthielt. Lesen Sie die API-Dokumentation, um festzustellen, welche Parameter für Ihre Anfrage gültig sind und ob Einschränkungen für deren Wert bestehen. |
|  | *invalid_resource_value* | 400 | Die Anfrage schlug fehl, weil eine ungültige oder fehlerhafte Ressource verwendet wurde. Lesen Sie die API-Dokumentation , um festzustellen, wie komplexe Ressourcen für Ihre Anfrage kodiert werden müssen und ob Einschränkungen hinsichtlich Wert und/oder Größe bestehen. |
|  | *invalid_registration_code* | 404 | Der angegebene Registrierungs-Code ist nicht mehr gültig oder abgelaufen. |
|  | *invalid_service_configuration* | 500 | Die Anfrage schlug aufgrund einer falschen Dienstkonfiguration fehl. |
|  | *missing_authentication_header* | 400 | Die Anfrage schlug fehl, da sie nicht den erforderlichen Authentifizierungs-Header für die jeweilige API enthält. |
|  | *missing_resource_mapping* | 400 | Für die angegebene Ressource gibt es keine entsprechende Zuordnung. Wenden Sie sich an den Support, um die erforderliche Zuordnung zu korrigieren. |
|  | *preauthorization_denied_by_mvpd* | 403 | Der MVPD hat eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben, wenn er eine Vorabautorisierung für die angegebene Ressource beantragt. |
|  | *preauthorization_denied_by_programmer* | 403 | Die vom Programmierer angewendeten Abbauregeln erzwingen eine &quot;Ablehnen&quot;-Entscheidung für den aktuellen Benutzer. |
|  | *registration_code_service_unavailable* | 503 | Die Anfrage schlug fehl, da der Registrierungscode-Dienst nicht verfügbar ist. |
|  | *service_unavailable* | 503 | Die Anfrage schlug fehl, weil der Authentifizierungs- oder Autorisierungsdienst nicht verfügbar ist. |
|  | *access_token_unavailable* | 400 | Die Anfrage schlug aufgrund eines unerwarteten Fehlers beim Abrufen des Zugriffstokens fehl. In der TVE-Dashboard-Konfiguration finden Sie verfügbare Softwareanweisungen und registrierte benutzerdefinierte Schemata. |
|  | *unsupported_client_version* | 400 | Diese Version des Adobe Pass Authentication SDK ist zu alt und wird nicht mehr unterstützt. In der API-Dokumentation finden Sie die Schritte, die für die Aktualisierung auf die neueste Version erforderlich sind. |
| **Konfiguration** | *network_required_ssl* | 403 | Es gibt ein SSL-Verbindungsproblem für den Ziel-Partner-Service. Wenden Sie sich an das Supportteam. |
|  | *too_many_resources* | 403 | Die Autorisierungs- oder Vorabautorisierungsanfrage schlug fehl, da zu viele Ressourcen abgefragt wurden. Wenden Sie sich an das Supportteam, um die Autorisierungs- und Vorautorisierungsbeschränkungen ordnungsgemäß zu konfigurieren. |
|  | *unknown_programmer* | 400 | Der Programmierer oder Dienstleister wird nicht erkannt. Registrieren Sie den angegebenen Programmierer über das TVE-Dashboard. |
|  | *unknown_application* | 400 | Die Anwendung wird nicht erkannt. Registrieren Sie die angegebene Anwendung über das TVE-Dashboard. |
|  | *unknown_integration* | 400 | Die Integration zwischen dem angegebenen Programmierer und Identitätsanbieter existiert nicht. Verwenden Sie das TVE-Dashboard , um die erforderliche Integration zu erstellen. |
|  | *unknown_software_statement* | 401 | Die mit dem Zugriffstoken verknüpfte Softwareanweisung wird nicht erkannt. Wenden Sie sich an das Support-Team, um den Status der Software-Anweisung zu klären. |
| **application-registration** | *access_token_expires* | 401 | Das Zugriffstoken ist abgelaufen. Das Programm sollte das Zugriffstoken aktualisieren, wie in der API-Dokumentation angegeben. |
|  | *invalid_access_token_signature* | 401 | Die Unterschrift des Zugriffstokens ist nicht mehr gültig. Das Programm sollte das Zugriffstoken aktualisieren, wie in der API-Dokumentation angegeben. |
|  | *invalid_client_id* | 401 | Die zugehörige Client-Kennung wird nicht erkannt. Die Anwendung sollte den in der API-Dokumentation angegebenen Registrierungsprozess für Anwendungen befolgen. |
| **Authentifizierung** | *authentication_session_expied* | 410 | Die aktuelle Authentifizierungssitzung ist abgelaufen. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|  | *authentication_session_missing* | 401 | Die mit dieser Anfrage verknüpfte Authentifizierungssitzung konnte nicht abgerufen werden. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|  | *authentication_session_invalidated* | 401 | Die Authentifizierungssitzung wurde vom Identitäts-Provider ungültig gemacht. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|  | *authentication_session_issu_mismatch* | 400 | Die Autorisierungsanfrage schlug fehl, weil der angegebene MVPD für den Autorisierungsfluss sich von dem unterscheidet, der die Authentifizierungssitzung ausgestellt hat. Der Benutzer muss sich erneut mit dem gewünschten MVPD authentifizieren, um fortfahren zu können. |
|  | *authorization_denied_by_hba_policies* | 403 | Der MVPD hat aufgrund von Home-basierten Authentifizierungsrichtlinien eine &quot;Ablehnen&quot;-Entscheidung zurückgegeben. Die aktuelle Authentifizierung wurde mithilfe eines Home-based Authentication Flow (HBA) abgerufen, aber das Gerät ist nicht mehr zu Hause, wenn die Autorisierung für die angegebene Ressource angefordert wird. Der Benutzer muss sich erneut mit einem unterstützten MVPD authentifizieren, um fortfahren zu können. |
|  | *identity_not_acknowledged_by_mvpd* | 403 | Die Autorisierungsanfrage schlug fehl, weil die Benutzeridentität vom MVPD nicht erkannt wurde. |
| **Autorisierung** | *authorization_expires* | 410 | Die vorherige Autorisierung für die angegebene Ressource ist abgelaufen. Der Benutzer muss eine neue Autorisierung einholen, um fortfahren zu können. |
|  | *authorization_not_found* | 404 | Für die angegebene Ressource wurde keine Autorisierung gefunden. Der Benutzer muss eine neue Autorisierung einholen, um fortfahren zu können. |
|  | *device_identifier_mismatch* | 403 | Die angegebene Geräte-ID stimmt nicht mit der Identifizierung des Autorisierungsgeräts überein. Der Benutzer muss eine neue Autorisierung einholen, um fortfahren zu können. |
| **Wiederholen** | *network_connection_failure* | 403 | Es ist ein Verbindungsfehler mit dem zugehörigen Partnerdienst aufgetreten. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |
|  | *network_connection_timeout* | 403 | Es gab einen Verbindungstimeout mit dem zugehörigen Partnerdienst. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |
|  | *network_received_error* | 403 | Beim Abrufen der Antwort vom zugehörigen Partnerdienst trat ein Lesefehler auf. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |
|  | *maximum_execution_time_exceeded* | 403 | Die Anfrage wurde in der maximal zulässigen Zeit nicht abgeschlossen. Ein erneuter Versuch mit der Anfrage kann das Problem lösen. |
| **Wiederholen** | *too_many_requests* | 429 | Es wurden zu viele Anfragen innerhalb eines bestimmten Intervalls gesendet. Die Anwendung kann die Anfrage nach dem vorgeschlagenen Zeitraum erneut versuchen. |
|  | *user_rate_limit_exceeded* | 429 | Es wurden zu viele Anfragen von einem bestimmten Benutzer innerhalb eines bestimmten Zeitraums ausgegeben. Die Anwendung kann die Anfrage nach dem vorgeschlagenen Zeitraum erneut versuchen. |
