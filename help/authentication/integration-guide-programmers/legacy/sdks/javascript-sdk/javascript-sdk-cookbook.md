---
title: JavaScript SDK-Cookbook
description: JavaScript SDK-Cookbook
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# JavaScript SDK-Cookbook {#javascript-sdk-cookbook}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#intro}

In diesem Dokument werden die Berechtigungs-Workflows beschrieben, die die Anwendung eines Programmierers auf oberster Ebene für eine JavaScript-Integration mit dem Adobe Pass-Authentifizierungsdienst implementiert. Links zur JavaScript-API-Referenz sind überall enthalten.

Beachten Sie außerdem, dass der Abschnitt [Verwandte Informationen](#related) eine
einen Link zu einer Reihe von JavaScript-Codebeispielen.

## Berechtigungsflüsse {#entitlement}

1. [Voraussetzungen](#prereq)
2. [Startup-Fluss](#startup)
3. [Authentifizierungsfluss](#authn)
4. [Autorisierungsfluss](#authz)
5. [Medienfluss anzeigen](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## Voraussetzungen {#prereq}

**Abhängigkeiten:**

- Arbeiten Sie mit der Adobe Pass Authentication Library (AccessEnabler) zusammen mit Ihrem Adobe Pass Authentication Account Manager, um dies zu arrangieren.
- Wenden Sie sich an Ihren Adobe Pass Authentication Account Manager, um dies zu arrangieren.

Erstellen Sie Ihre Callback-Funktionen:

- `entitlementLoaded`
</br>

**Trigger:** Der AccessEnabler hat die Initialisierung geladen und abgeschlossen.

- `displayProviderDialog(mvpds)`

  **Trigger:** `getAuthentication(),` nur dann, wenn der Benutzer keinen Anbieter (einen MVPD) ausgewählt hat und noch nicht authentifiziert ist
Der Parameter mvpds ist eine Reihe von Anbietern, die für den Benutzer verfügbar sind.

- `setAuthenticationStatus(status, errorcode)`

  **Trigger:**
   - `checkAuthentication()`jedes Mal.
   - `getAuthentication()` nur dann, wenn der Benutzer bereits authentifiziert ist und einen Anbieter ausgewählt hat.

  Der zurückgegebene Status ist erfolgreich oder fehlgeschlagen. Der Fehler-Code beschreibt den Typ des Fehlers.

- `createIFrame(width, height)`

  **Trigger:** `setSelectedProvider(providerID)`, nur wenn der ausgewählte Provider für die Anzeige in einem IFrame konfiguriert ist.

  >[!NOTE]
  >
  >Ein Provider ist so konfiguriert, dass sein Authentifizierungsbildschirm entweder als Umleitung oder in einem iFrame gerendert wird. Der Programmierer muss beides berücksichtigen.

- `sendTrackingData(event, data)`

  **Trigger:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  Der Parameter `event` gibt an, welches Berechtigungsereignis aufgetreten ist. Der Parameter `data` ist eine Liste von Werten, die sich auf das Ereignis beziehen.
- `setToken(token, resource)`
  **Trigger:** `checkAuthorization()`und `getAuthorization()` nach erfolgreicher Autorisierung zum Anzeigen einer Ressource.   Der Parameter `token` ist das kurzlebige Medien-Token. Der Parameter `resource` ist der Inhalt, den der Benutzer anzeigen darf.

- `tokenRequestFailed(resource, code, description)`
  **Trigger:**`checkAuthorization()` und`getAuthorization()` nach einer nicht erfolgreichen Autorisierung.\
  Der Parameter `resource` ist der Inhalt, den der Benutzer anzeigen wollte. Der Parameter `code` ist der Fehlercode, der angibt, welcher Fehlertyp aufgetreten ist. Der Parameter `description` beschreibt den Fehler, der mit dem Fehlercode verknüpft ist.

- `selectedProvider(mvpd)`

  **Trigger:** [`getSelectedProvider()`](#$getSelProv Der Parameter `mvpd` enthält Informationen zum von ausgewählten Provider
den Benutzer.

- `setMetadataStatus(metadata, key, arguments)`

  **Trigger:** `getMetadata().`\
  Der Parameter `metadata` stellt die spezifischen Daten bereit, die Sie angefordert haben. Der Schlüsselparameter ist der Schlüssel, der in der `getMetadata()`Anfrage verwendet wird, und der Parameter `arguments` ist dasselbe Wörterbuch, das an `getMetadata()` übergeben wurde.


## 2. Startup-Fluss

**I. Laden Sie den AccessEnabler JavaScript:**

**Für Staging-Profil**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

oder ...

**Für das Produktionsprofil**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Trigger:** Wenn die Initialisierung abgeschlossen ist, Adobe Pass
Authentifizierung ruft Ihre `entitlementLoaded()` -Rückruffunktion auf. Dies ist der Einstiegspunkt zur Kommunikation Ihrer Anwendung mit dem AccessEnabler.


**II.** Rufen Sie `setRequestor()` auf, um die
Identität des Programmierers; geben Sie die `requestorID` des Programmierers ein und
(Optional) ein Array von Adobe Pass-Authentifizierungsendpunkten.

**Trigger:** Keine, ermöglicht jedoch bei Bedarf den Aufruf von `displayProviderDialog()`.


**III.** Rufen Sie `checkAuthentication()` auf, um nach einer vorhandenen Authentifizierung zu suchen, ohne den vollständigen [Authentifizierungsfluss] zu starten.  Wenn dieser Aufruf erfolgreich ist, können Sie direkt zum `authorization flow` wechseln.  Wenn nicht, fahren Sie mit dem `authentication flow` fort.

**Abhängigkeit:** Ein erfolgreicher Aufruf von `setRequestor()` (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).

**Trigger:** `setAuthenticationStatus()` callback

</br>

## 3. Authentifizierungsfluss</span>


**Abhängigkeit:** Ein erfolgreicher Aufruf von `setRequestor()` (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).


Rufen Sie `getAuthentication()` auf, um den Authentifizierungsstatus abzurufen ODER um den Authentifizierungsfluss des Anbieters Trigger.

**Auslöser:**

- `displayProviderDialog()`wenn der Benutzer noch nicht authentifiziert wurde
- `setAuthenticationStatus()` , wenn die Authentifizierung bereits stattgefunden hat

Der Authentifizierungsfluss wird abgeschlossen, wenn der AccessEnabler `setAuthenticationStatus()`mit `isAuthenticated == 1` aufruft.

## 4. Genehmigungsprozess {#authz}

**Abhängigkeiten:**

- Ein erfolgreicher Aufruf von `setRequestor()` (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).
- Gültige ResourceID(s), die mit den MVPD(s) vereinbart wurde. Beachten Sie, dass ResourceIDs mit denen auf anderen Geräten oder Plattformen übereinstimmen und über MVPDs hinweg identisch sein sollten.

Rufen Sie `getAuthorization()` auf und übergeben Sie die ResourceID für das angeforderte Medium. Bei einem erfolgreichen Aufruf wird ein Short Media Token zurückgegeben, das bestätigt, dass der Benutzer berechtigt ist, die angeforderten Medien anzuzeigen.

- Wenn der Aufruf erfolgreich ist: Der Benutzer verfügt über ein gültiges AuthN-Token und der Benutzer ist berechtigt, die angeforderten Medien anzuzeigen.
- Wenn der Aufruf fehlschlägt: Überprüfen Sie die ausgelöste Ausnahme, um ihren Typ zu bestimmen (AuthN, AuthZ oder etwas Anderes):
- Wenn der Aufruf ein AuthN-Fehler war, starten Sie den AuthN-Fluss erneut.
- Wenn es sich bei dem Aufruf um einen AuthZ-Fehler handelt, ist der Benutzer nicht berechtigt, das angeforderte Medium zu sehen, und dem Benutzer sollte eine Fehlermeldung angezeigt werden.
- Wenn ein anderer Fehler aufgetreten ist (Verbindungsfehler, Netzwerkfehler usw.), zeigen Sie dem Benutzer eine entsprechende Fehlermeldung an.

Verwenden Sie den Media Token Verifier, um das shortMediaToken zu überprüfen, das von einem erfolgreichen `getAuthorization()` -Aufruf zurückgegeben wurde.


**Abhängigkeit:** Der Verifikator für Kurzmedia-Token (im Lieferumfang von
AccessEnabler-Bibliothek)

- Wenn die Validierung erfolgreich ist: Anzeigen/Wiedergeben des angeforderten Mediums für den Benutzer.
- Wenn es fehlschlägt: Das AuthZ-Token war ungültig, die Medienanforderung sollte abgelehnt werden und dem Benutzer sollte eine Fehlermeldung angezeigt werden.

## 5. Medienfluss anzeigen {#logout}

- Der Benutzer wählt das Medium aus, das angezeigt werden soll.
   - Sind Medien geschützt?
      - Ihre App prüft, ob die Medien geschützt sind:
         - Wenn das Medium geschützt ist, startet Ihre App den obigen Autorisierungsfluss (AuthZ).
         - Wenn das Medium nicht geschützt ist, fahren Sie mit dem Fluss Medien anzeigen fort.
         - Wiedergabemedien

## Konfigurieren der Besucher-ID {#visitorID}

Die Konfiguration eines [Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html) -Werts ist aus Analysesicht sehr wichtig. Sobald ein EC visitorID -Wert festgelegt ist, sendet das SDK diese Informationen zusammen mit jedem Netzwerkaufruf und der Adobe Pass-Authentifizierungsdienst erfasst diese Informationen. Auf diese Weise können Sie die Analysedaten aus dem Adobe Pass-Authentifizierungsdienst mit allen anderen Analyseberichten korrelieren, die Sie möglicherweise aus anderen Anwendungen oder Websites haben. Informationen zum Einrichten der EC-Besucher-ID finden Sie [hier](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Beachten Sie, dass diese Funktion ab JS SDK-Version 3.1.0 unterstützt wird.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
