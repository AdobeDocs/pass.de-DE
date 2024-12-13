---
title: Amazon FireOS-Integrations-Cookbook
description: Amazon FireOS-Integrations-Cookbook
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1447'
ht-degree: 0%

---

# (Legacy) Amazon FireOS-Integrations-Cookbook {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>


## Einführung {#intro}

In diesem Dokument werden die Berechtigungs-Workflows beschrieben, die ein Programm eines Programmierers auf höherer Ebene über die APIs implementieren kann, die von der Amazon FireOS-`AccessEnabler` bereitgestellt werden.

Die Lösung für die Berechtigung zur Adobe Pass-Authentifizierung für Amazon FireOS ist letztendlich in zwei Domains unterteilt:

- Die Domain der Benutzeroberfläche : Dies ist die Anwendungsebene der oberen Ebene, die die Benutzeroberfläche implementiert und die von der `AccessEnabler`-Bibliothek bereitgestellten Services verwendet, um Zugriff auf eingeschränkte Inhalte zu gewähren.
- Die `AccessEnabler` Domain - hier werden die Berechtigungs-Workflows in der Form von implementiert:
   - Netzwerkaufrufe an Adobe-Backend-Server
   - Geschäftslogikregeln in Bezug auf die Authentifizierungs- und Autorisierungs-Workflows
   - Verwaltung verschiedener Ressourcen und Verarbeitung des Workflow-Status (z. B. des Token-Cache)

Das Ziel der `AccessEnabler`-Domain besteht darin, alle Komplexitäten der Berechtigungs-Workflows zu verbergen und der Anwendung der oberen Ebene (über die `AccessEnabler`-Bibliothek) eine Reihe einfacher Berechtigungsprimitive bereitzustellen. Dieser Prozess ermöglicht die Implementierung der Berechtigungs-Workflows:

1. Legen Sie die Anfordereridentität fest.
1. Authentifizierung mit einem bestimmten Identitätsanbieter überprüfen und abrufen.
1. Überprüfen Sie und rufen Sie die Autorisierung für eine bestimmte Ressource ab.
1. Abmelden.

Die Netzwerkaktivität des `AccessEnabler` erfolgt in einem anderen Thread, sodass der UI-Thread nie blockiert wird. Daher muss der bidirektionale Kommunikationskanal zwischen den beiden Anwendungsdomänen einem vollständig asynchronen Muster folgen:

- Die Anwendungsebene der Benutzeroberfläche sendet Nachrichten über die API-Aufrufe, die von der `AccessEnabler`-Bibliothek bereitgestellt werden, an die `AccessEnabler`-Domain.
- Der `AccessEnabler` antwortet auf die Benutzeroberflächenebene über die im `AccessEnabler`-Protokoll enthaltenen Rückrufmethoden, die die Benutzeroberflächenebene bei der `AccessEnabler`-Bibliothek registriert.

## Berechtigungsflüsse {#entitlement}

1. [Voraussetzungen](#prereqs)
1. [Anlauffluss](#startup_flow)
1. [Authentifizierungsfluss](#authn_flow)
1. [Autorisierungsfluss](#authz_flow)
1. [Medienfluss anzeigen](#media_flow)
1. [Abmeldefluss](#logout_flow)

### A. Voraussetzungen {#prereqs}

1. Erstellen Sie Ihre Callback-Funktionen:
   - [`setRequestorComplete()`](#$setRequestorComplete)

      - Wird durch `setRequestor()` ausgelöst und gibt „Erfolg“ oder „Fehler“ zurück.     Der Erfolg zeigt an, dass Sie mit Berechtigungsaufrufen fortfahren können.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Wird nur von `getAuthentication()` ausgelöst, wenn der Benutzer keinen Provider (MVPD) ausgewählt hat und noch nicht authentifiziert ist. Der `mvpds` Parameter ist ein Array von Anbietern, die den Benutzenden zur Verfügung stehen.

   - [`setAuthenticationStatus(status, reason)`](#$setAuthNStatus)

      - Wird jedes Mal von `checkAuthentication()` ausgelöst. Wird nur von `getAuthentication()` ausgelöst, wenn der Benutzer bereits authentifiziert ist und einen Anbieter ausgewählt hat.

      - Der zurückgegebene Status ist authentifiziert oder nicht authentifiziert. Der Grund dafür ist ein Authentifizierungsfehler oder eine Abmeldeaktion.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Wird in AmazonFireOS SDK ignoriert, wird die Methode auf Android-Plattformen verwendet, wo sie durch `getAuthentication()` ausgelöst wird, nachdem der Benutzer eine MVPD ausgewählt hat.  Der `url` gibt den Speicherort der Anmeldeseite von MVPD an.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - Ausgelöst durch `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
Der `event` Parameter gibt an, welches Berechtigungsereignis aufgetreten ist. Der `data` Parameter ist eine Liste von Werten, die sich auf das Ereignis beziehen.

   - [`setToken(token, resource)`](#$setToken)

      - Wird durch `checkAuthorization()` und `getAuthorization()` nach erfolgreicher Autorisierung zum Anzeigen einer Ressource ausgelöst.
      - Der `token` ist das kurzlebige Medien-Token. Der `resource` ist der Inhalt, den der Benutzer anzeigen darf.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

      - Wird durch `checkAuthorization()` und `getAuthorization()` nach erfolgloser Autorisierung ausgelöst.
      - Der `resource` ist der Inhalt, den die Benutzerin oder der Benutzer versucht hat anzuzeigen. Der `code` ist der Fehlercode, der angibt, welche Art von Fehler aufgetreten ist. Der `description` Parameter beschreibt den Fehler, der mit dem Fehlercode verbunden ist.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

      - Ausgelöst durch `getSelectedProvider()`.
      - Der Parameter `mvpd` enthält Informationen zum vom Benutzer ausgewählten Anbieter.

   - [`setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

      - Ausgelöst durch `getMetadata().`
      - Der `metadata`-Parameter liefert die angeforderten spezifischen Daten, der `key`-Parameter ist der in der `getMetadata()`-Anfrage verwendete Schlüssel und der `arguments` ist dasselbe Wörterbuch, das an `getMetadata()` übergeben wurde.

   - [`preauthorizedResources(resources)`](#$preauthResources)

      - Ausgelöst durch `checkPreauthorizedResources()`.
      - Der `authorizedResources` Parameter stellt die Ressourcen dar, die der Benutzer anzeigen darf.


![](../../../../assets/android-entitlement-flows.png)


### B. Anlauffluss {#startup_flow}

1. Starten Sie die Anwendung auf höherer Ebene.
1. Adobe Pass-Authentifizierung starten.

   1. Rufen Sie [`getInstance`](#$getInstance) auf, um eine einzelne Instanz des Adobe Pass-Authentifizierungs-`AccessEnabler` zu erstellen.

      - **Abhängigkeit:** Adobe Pass Authentication Native Amazon FireOS Library (`AccessEnabler`)

   1. Aufruf` setRequestor()`, um die Kennung des Programmierers zu ermitteln; übergeben Sie die `requestorID` des Programmierers und (optional) ein Array von Adobe Pass-Authentifizierungsendpunkten.

      - **Abhängigkeit:** Valid Adobe Pass Authentication RequestorID (Wenden Sie sich an Ihren Adobe Pass Authentication Account Manager, um dies zu arrangieren.)

      - **Trigger:** setRequestorComplete()-Rückruf

   Berechtigungsanfragen können erst abgeschlossen werden, wenn die Identität des Antragstellers vollständig ermittelt wurde. Dies bedeutet effektiv, dass während der Ausführung von setRequestor() alle nachfolgenden Berechtigungsanfragen (z. B. `checkAuthentication()`) blockiert werden.

   Sie haben zwei Implementierungsoptionen: Sobald die Anfordereridentifizierungsinformationen an den Backend-Server gesendet wurden, kann die Benutzeroberflächen-Anwendungsebene einen der beiden folgenden Ansätze wählen:</p>

   1. Warten Sie, bis der `setRequestorComplete()`-Rückruf ausgelöst wird (Teil des `AccessEnabler`-Delegaten).  Diese Option bietet die größte Sicherheit, dass abgeschlossen `setRequestor()`. Daher wird sie für die meisten Implementierungen empfohlen.
   1. Fahren Sie fort, ohne auf das Auslösen des `setRequestorComplete()`-Callbacks zu warten, und beginnen Sie mit der Ausgabe von Berechtigungsanfragen. Diese Aufrufe (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) werden von der `AccessEnabler`-Bibliothek in die Warteschlange gestellt, die die eigentlichen Netzwerkaufrufe nach dem `setRequestor()` ausführt. Diese Option kann gelegentlich unterbrochen werden, wenn beispielsweise die Netzwerkverbindung instabil ist.

1. Rufen Sie [checkAuthentication()](#$checkAuthN) auf, um auf eine vorhandene Authentifizierung zu prüfen, ohne den vollständigen Authentifizierungsfluss zu initiieren.  Wenn dieser Aufruf erfolgreich ist, können Sie direkt mit dem Autorisierungsfluss fortfahren.  Andernfalls fahren Sie mit dem Authentifizierungsfluss fort.

- **Abhängigkeit:** Erfolgreicher Aufruf an `setRequestor()` (diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).

- **Trigger:** setAuthenticationStatus()-Rückruf

### C. Authentifizierungsfluss {#authn_flow}

1. Rufen Sie [`getAuthentication()`](#$getAuthN) auf, um den Authentifizierungsfluss zu initiieren oder um eine Bestätigung zu erhalten, dass der Benutzer bereits authentifiziert ist.

   **Trigger:**

   - Der Rückruf setAuthenticationStatus() , wenn der Benutzer bereits authentifiziert ist.  Fahren Sie in diesem Fall direkt mit dem [Autorisierungsfluss“ ](#authz_flow).
   - Der Rückruf displayProviderDialog() , wenn der Benutzer noch nicht authentifiziert ist.

1. Zeigen Sie dem Benutzer die Liste der an `displayProviderDialog()` gesendeten Anbieter.
1. Nachdem der Benutzer einen Anbieter ausgewählt hat, öffnet WebView die Anbieterseite, auf der sich der Benutzer anmelden kann

   >[!NOTE]
   >
   >An dieser Stelle hat der Benutzer die Möglichkeit, den Authentifizierungsfluss abzubrechen. In diesem Fall bereinigt der `AccessEnabler` seinen internen Status und setzt den Authentifizierungsfluss zurück.

1. Nach erfolgreicher Anmeldung durch den Benutzer wird die WebView geschlossen.
1. Rufen Sie `getAuthenticationToken(),` auf, um den `AccessEnabler` anzuweisen, das Authentifizierungstoken vom Backend-Server abzurufen.
1. [Optional] Rufen Sie [`checkPreauthorizedResources(resources)`](#$checkPreauth) auf, um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Der `resources` ist ein Array geschützter Ressourcen, das mit dem Authentifizierungstoken des Benutzers verknüpft ist.

   **Trigger:** `preAuthorizedResources()` Callback\
   **Ausführungspunkt:** Nach Abschluss des Authentifizierungsflusses

1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.


### D. Autorisierungsfluss {#authz_flow}

1. Rufen Sie [`getAuthorization()`](#$getAuthZ) auf, um den Autorisierungsfluss zu initiieren.

   Abhängigkeit: Gültige ResourceID(s), die mit den MVPD vereinbart wurden.

   **Hinweis** Die Ressourcen-IDs müssen mit den auf anderen Geräten oder Plattformen verwendeten IDs übereinstimmen und für alle MVPDs identisch sein.

1. Authentifizierung und Autorisierung überprüfen.

   - Wenn der `getAuthorization()`-Aufruf erfolgreich ist: Der Benutzer verfügt über gültige AuthN- und AuthZ-Token (der Benutzer ist authentifiziert und berechtigt, die angeforderten Medien zu sehen).
   - Wenn `getAuthorization()` fehlschlägt: Untersuchen Sie die ausgelöste Ausnahme, um ihren Typ (AuthN, AuthZ oder etwas Anderes) zu bestimmen:
      - Wenn es sich um einen Authentifizierungsfehler (AuthN) handelte, starten Sie den Authentifizierungsfluss erneut.
      - Wenn es sich um einen Autorisierungsfehler (AuthZ) handelte, ist der Benutzer nicht berechtigt, die angeforderten Medien anzusehen, und dem Benutzer sollte eine Fehlermeldung angezeigt werden.
      - Wenn ein anderer Fehlertyp aufgetreten ist (Verbindungsfehler, Netzwerkfehler usw.), zeigen Sie dem Benutzer eine entsprechende Fehlermeldung an.

1. Validieren des Short Media Token.

   Verwenden Sie die Media Token Verifier-Bibliothek der Adobe Pass-Authentifizierung, um das vom obigen `getAuthorization()`-Aufruf zurückgegebene kurzlebige Medien-Token zu überprüfen:

   - Wenn die Validierung erfolgreich ist: Geben Sie die angeforderten Medien für den Benutzer wieder.
   - Wenn die Validierung fehlschlägt: Das AuthZ-Token war ungültig, sollte die Medienanfrage abgelehnt werden und dem Benutzer sollte eine Fehlermeldung angezeigt werden.

1. Kehren Sie zu Ihrem normalen Programmablauf zurück.

### E. Anzeigen des Medienflusses {#media_flow}

1. Der Benutzer wählt die anzuzeigenden Medien aus.
1. Sind die Medien geschützt?  Ihre Anwendung prüft, ob das ausgewählte Medium geschützt ist:
   - Wenn das ausgewählte Medium geschützt ist, startet Ihre Anwendung den [Autorisierungsfluss](#authz_flow) oben.
   - Wenn das ausgewählte Medium nicht geschützt ist, geben Sie das Medium für den Benutzer wieder.

### F. Abmeldefluss {#logout_flow}

1. Rufen Sie [`logout()`](#$logout) auf, um den Benutzer abzumelden. Die `AccessEnabler` löscht alle zwischengespeicherten Werte und Token, die Benutzende für die aktuelle MVPD erhalten haben, aus allen Anforderern, die die Anmeldung über Single Sign-On teilen. Nachdem der Cache gelöscht wurde, führt der `AccessEnabler` einen Server-Aufruf durch, um die Server-seitigen Sitzungen zu bereinigen.  Da der Server-Aufruf zu einer SAML-Umleitung an den IdP führen kann (dies ermöglicht die Sitzungsbereinigung auf der IdP-Seite), muss dieser Aufruf allen Umleitungen folgen. Aus diesem Grund wird dieser Aufruf innerhalb eines WebView-Steuerelements verarbeitet, das für den Benutzer unsichtbar ist.

   **Hinweis:** Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss insofern, als der Benutzer in keiner Weise mit der WebView interagieren muss. Daher ist es möglich (und empfohlen), das WebView-Steuerelement während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.
