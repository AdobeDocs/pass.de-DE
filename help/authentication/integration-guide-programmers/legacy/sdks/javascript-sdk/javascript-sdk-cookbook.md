---
title: JavaScript SDK-Cookbook
description: JavaScript SDK-Cookbook
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 0%

---

# (Legacy) JavaScript SDK-Cookbook {#javascript-sdk-cookbook}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Einführung {#intro}

In diesem Dokument werden die Berechtigungs-Workflows beschrieben, die die Anwendung eines Programmierers auf höherer Ebene für eine JavaScript-Integration mit dem Adobe Pass-Authentifizierungs-Service implementiert. Links zur JavaScript-API-Referenz sind überall zu finden.

Beachten Sie außerdem, dass [ Abschnitt „Verwandte ](#related)&quot; einen
Link zu einem Satz von JavaScript-Code-Beispielen.

## Berechtigungsflüsse {#entitlement}

1. [Voraussetzungen](#prereq)
2. [Anlauffluss](#startup)
3. [Authentifizierungsfluss](#authn)
4. [Autorisierungsfluss](#authz)
5. [Medienfluss anzeigen](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## Voraussetzungen {#prereq}

**dependencies:**

- Adobe Pass Authentication Library (AccessEnabler). Wenden Sie sich an Ihren Adobe Pass Authentication Account Manager, um dies zu arrangieren.
- Valid Adobe Pass Authentication RequestorId, wenden Sie sich an Ihren Adobe Pass Authentication Account Manager, um dies zu arrangieren.

Erstellen Sie Ihre Callback-Funktionen:

- `entitlementLoaded`
</br>

**Trigger:** AccessEnabler wurde geladen und die Initialisierung ist abgeschlossen.

- `displayProviderDialog(mvpds)`

  **Trigger:** `getAuthentication(),` nur, wenn der/die Benutzende keinen Anbieter (eine MVPD) ausgewählt hat und noch nicht authentifiziert ist
Der mvpds-Parameter ist ein Array von Anbietern, die dem Benutzer zur Verfügung stehen.

- `setAuthenticationStatus(status, errorcode)`

  **Trigger:**
   - `checkAuthentication()`jedes Mal.
   - Nur `getAuthentication()`, wenn der Benutzer bereits authentifiziert ist und einen Anbieter ausgewählt hat.

  Zurückgegebener Status ist Erfolg oder Fehler. Der Fehler-Code beschreibt den Typ des Fehlers.

- `createIFrame(width, height)`

  **Trigger:** `setSelectedProvider(providerID)`, nur wenn der ausgewählte Anbieter so konfiguriert ist, dass er in einem IFrame angezeigt wird.

  >[!NOTE]
  >
  >Ein Anbieter ist so konfiguriert, dass sein Authentifizierungsbildschirm entweder als Umleitung oder in einem iFrame gerendert wird, und der Programmierer muss beide berücksichtigen.

- `sendTrackingData(event, data)`

  **Trigger:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  Der `event` Parameter gibt an, welches Berechtigungsereignis aufgetreten ist. Der `data` Parameter ist eine Liste von Werten, die sich auf das Ereignis beziehen.
- `setToken(token, resource)`
  **Trigger:** `checkAuthorization()`und `getAuthorization()` nach einer erfolgreichen Autorisierung zum Anzeigen einer Ressource.   Der `token` ist das kurzlebige Medien-Token. Der `resource` ist der Inhalt, den die Benutzerin oder der Benutzer anzeigen darf.

- `tokenRequestFailed(resource, code, description)`
  **Trigger:**`checkAuthorization()` and`getAuthorization()` nach einer fehlgeschlagenen Autorisierung.\
  Der `resource` ist der Inhalt, den die Benutzerin oder der Benutzer versucht hat anzuzeigen. Der `code` ist der Fehlercode, der angibt, welche Art von Fehler aufgetreten ist. Der `description` Parameter beschreibt den Fehler, der mit dem Fehlercode verbunden ist.

- `selectedProvider(mvpd)`

  **Trigger:** [`getSelectedProvider()`](#$getSelProv Der Parameter &quot;`mvpd`&quot; enthält Informationen über den von ausgewählten Anbieter.
Der Benutzer.

- `setMetadataStatus(metadata, key, arguments)`

  **Trigger:** `getMetadata().`\
  Der `metadata` Parameter liefert die angeforderten spezifischen Daten, der Schlüsselparameter ist der in der `getMetadata()`Anforderung verwendete Schlüssel und der `arguments` Parameter ist dasselbe Wörterbuch, das an `getMetadata()` übergeben wurde.


## 2. Anlauffluss

**I. Laden Sie die AccessEnabler-JavaScript:**

**Für Staging-Profil**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

oder…

**Für das Produktionsprofil**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Trigger:** Nach Abschluss der Initialisierung Adobe Pass
Die Authentifizierung ruft Ihre `entitlementLoaded()`-Rückruffunktion auf. Dies ist der Einstiegspunkt für die Kommunikation Ihres Programms mit dem AccessEnabler.


**II.**-Aufruf `setRequestor()`zur Einrichtung der
Identität des Programmierers; übergeben Sie die `requestorID` des Programmierers und
(Optional) Ein Array von Adobe Pass-Authentifizierungsendpunkten.

**Trigger:** Keine, ermöglicht jedoch bei Bedarf den Aufruf von `displayProviderDialog()`.


**III.** Aufruf `checkAuthentication()`, um nach einer vorhandenen Authentifizierung zu suchen, ohne den vollständigen [Authentifizierungsfluss) ].  Wenn dieser Aufruf erfolgreich ist, können Sie direkt zur `authorization flow` wechseln.  Andernfalls fahren Sie mit dem `authentication flow` fort.

**Abhängigkeit:** Erfolgreicher Aufruf an `setRequestor()` (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).

**Trigger:** `setAuthenticationStatus()` Callback

</br>

## 3. Authentifizierungsfluss</span>


**Abhängigkeit:** Erfolgreicher Aufruf an `setRequestor()` (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).


Rufen Sie `getAuthentication()` auf, um den Authentifizierungsstatus ODER den Trigger für den Authentifizierungsfluss des Anbieters abzurufen.

**Triggers:**

- `displayProviderDialog()`Wenn der Benutzer noch nicht authentifiziert wurde
- `setAuthenticationStatus()`, ob die Authentifizierung bereits stattgefunden hat

Der Abschluss des Authentifizierungsflusses wird erreicht, wenn AccessEnabler (mit `isAuthenticated == 1``setAuthenticationStatus()` aufruft.

## 4. Autorisierungsfluss {#authz}

**dependencies:**

- Ein erfolgreicher Aufruf von `setRequestor()` (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).
- Gültige ResourceID(s), die mit den MVPD vereinbart wurden. Beachten Sie, dass die ResourceIDs mit den auf anderen Geräten oder Plattformen verwendeten übereinstimmen müssen und für alle MVPDs gleich sein müssen.

Rufen Sie `getAuthorization()` auf und übergeben Sie die ResourceID für die angeforderten Medien. Bei einem erfolgreichen Aufruf wird ein Kurz-Medien-Token zurückgegeben, das bestätigt, dass der Benutzer berechtigt ist, die angeforderten Medien anzuzeigen.

- Wenn der Aufruf erfolgreich ist: Der Benutzer verfügt über ein gültiges Authentifizierungs-Token und der Benutzer ist berechtigt, die angeforderten Medien anzusehen.
- Wenn der Aufruf fehlschlägt: Untersuchen Sie die ausgelöste Ausnahme, um ihren Typ (AuthN, AuthZ oder etwas Anderes) zu bestimmen:
- Wenn der Aufruf ein AuthN-Fehler war, starten Sie den AuthN-Fluss erneut.
- Wenn der Aufruf ein AuthZ-Fehler war, ist der Benutzer nicht berechtigt, die angeforderten Medien anzusehen, und dem Benutzer sollte eine Fehlermeldung angezeigt werden.
- Wenn ein anderer Fehler aufgetreten ist (Verbindungsfehler, Netzwerkfehler usw.), zeigen Sie dem Benutzer eine entsprechende Fehlermeldung an.

Verwenden Sie den Media Token Verifier, um das von einem erfolgreichen `getAuthorization()`-Aufruf zurückgegebene shortMediaToken zu validieren.


**Abhängigkeit:** Der Short Media Token Verifier (im Lieferumfang der
AccessEnabler-Bibliothek)

- Wenn die Validierung erfolgreich war: Anzeigen/Wiedergabe der angeforderten Medien für den Benutzer.
- Wenn er fehlschlägt: Das AuthZ-Token war ungültig, die Medienanfrage sollte abgelehnt werden und dem Benutzer sollte eine Fehlermeldung angezeigt werden.

## 5. Anzeigen des Medienflusses {#logout}

- Der Benutzer wählt die anzuzeigenden Medien aus.
   - Sind Medien geschützt?
      - Ihre App prüft, ob die Medien geschützt sind:
         - Wenn das Medium geschützt ist, startet Ihre App den oben genannten Autorisierungsfluss (AuthZ).
         - Wenn das Medium nicht geschützt ist, fahren Sie mit dem Fluss Medien anzeigen fort.
         - Medienwiedergabe

## Konfigurieren der Besucher-ID {#visitorID}

Die Konfiguration eines [Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html)-Werts ist aus analytischer Sicht sehr wichtig. Sobald ein EC VisitorID-Wert festgelegt ist, sendet die SDK diese Informationen zusammen mit jedem Netzwerkaufruf, und der Adobe Pass-Authentifizierungsdienst erfasst diese Informationen. Auf diese Weise können Sie die Analysedaten aus dem Adobe Pass-Authentifizierungs-Service mit allen anderen Analyseberichten korrelieren, die Sie möglicherweise von anderen Programmen oder Websites haben. Informationen zum Einrichten von EC visitorID finden Sie [hier](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Beachten Sie, dass diese Funktionsunterstützung ab JS SDK Version 3.1.0 verfügbar ist.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
