---
title: Amazon FireOS-Integrations-Cookbook
description: Amazon FireOS-Integrations-Cookbook
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# Amazon FireOS-Integrations-Cookbook {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>


## Einführung {#intro}

In diesem Dokument werden die Berechtigungs-Workflows beschrieben, die die Anwendung eines Programmierers auf oberster Ebene über die APIs implementieren kann, die von Amazon FireOS bereitgestellt werden `AccessEnabler` -Bibliothek.

Die Berechtigungslösung für die Adobe Pass-Authentifizierung für Amazon FireOS ist letztendlich in zwei Domänen unterteilt:

- Die UI-Domäne - dies ist die Anwendungsebene der obersten Ebene, die die Benutzeroberfläche implementiert und die von der `AccessEnabler` -Bibliothek, um Zugriff auf eingeschränkte Inhalte bereitzustellen.
- Die `AccessEnabler` -Domäne - hier werden die Berechtigungs-Workflows in folgender Form implementiert:
   - Netzwerkaufrufe an Adobe-Backend-Server
   - Geschäftslogikregeln für die Authentifizierungs- und Autorisierungs-Workflows
   - Verwaltung verschiedener Ressourcen und Verarbeitung des Workflow-Status (z. B. Token-Cache)

Das Ziel der `AccessEnabler` Die Domäne besteht darin, alle Komplexität der Berechtigungs-Workflows auszublenden und die Anwendung auf der obersten Ebene bereitzustellen (über die `AccessEnabler` -Bibliothek) eine Reihe einfacher Berechtigungsprimitive. Mithilfe dieses Prozesses können Sie die Berechtigungs-Workflows implementieren:

1. Legen Sie die Anfragenkennung fest.
1. Überprüfen Sie die Authentifizierung für einen bestimmten Identitäts-Provider und rufen Sie ihn ab.
1. Überprüfen Sie die Autorisierung für eine bestimmte Ressource und erhalten Sie sie.
1. Abmelden.

Die `AccessEnabler`Die Netzwerkaktivität von findet in einem anderen Thread statt, sodass der UI-Thread nie blockiert wird. Daher muss der bidirektionale Kommunikationskanal zwischen den beiden Anwendungsdomänen einem vollständig asynchronen Muster folgen:

- Die UI-Anwendungsebene sendet Nachrichten an `AccessEnabler` -Domäne über die API-Aufrufe, die von der `AccessEnabler` -Bibliothek.
- Die `AccessEnabler` antwortet über die Callback-Methoden, die in der `AccessEnabler` Protokoll, das die UI-Ebene beim `AccessEnabler` -Bibliothek.

## Berechtigungsflüsse {#entitlement}

1. [Voraussetzungen](#prereqs)
1. [Startup-Fluss](#startup_flow)
1. [Authentifizierungsfluss](#authn_flow)
1. [Autorisierungsfluss](#authz_flow)
1. [Medienfluss anzeigen](#media_flow)
1. [Abmeldefluss](#logout_flow)

### A. Voraussetzungen {#prereqs}

1. Erstellen Sie Ihre Callback-Funktionen:
   - [`setRequestorComplete()`](#$setRequestorComplete)

      - Ausgelöst von `setRequestor()`, gibt Erfolg oder Fehler zurück.     &quot;Erfolg&quot;bedeutet, dass Sie mit Berechtigungsaufrufen fortfahren können.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Ausgelöst von `getAuthentication()` nur dann, wenn der Benutzer keinen Anbieter (MVPD) ausgewählt hat und noch nicht authentifiziert ist. Die `mvpds` -Parameter ist ein Array von Anbietern, die dem Benutzer zur Verfügung stehen.

   - [`setAuthenticationStatus(status, reason)`](#$setAuthNStatus)

      - Ausgelöst von `checkAuthentication()` jedes Mal. Ausgelöst von `getAuthentication()` nur dann, wenn der Benutzer bereits authentifiziert ist und einen Provider ausgewählt hat.

      - Der zurückgegebene Status ist authentifiziert oder nicht authentifiziert. Der Grund beschreibt einen Authentifizierungsfehler oder eine Abmeldeaktion.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Diese Methode wird im AmazonFireOS-SDK ignoriert und wird auf Android-Plattformen verwendet, bei denen durch `getAuthentication()` nachdem der Benutzer einen MVPD ausgewählt hat.  Die `url` liefert den Speicherort der Anmeldeseite des MVPD.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - Ausgelöst von `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
Die `event` Parameter gibt an, welches Berechtigungsereignis aufgetreten ist; die `data` -Parameter ist eine Liste von Werten, die sich auf das Ereignis beziehen.

   - [`setToken(token, resource)`](#$setToken)

      - Ausgelöst von `checkAuthorization()` und `getAuthorization()` nach erfolgreicher Autorisierung zum Anzeigen einer Ressource.
      - Die `token` -Parameter ist das kurzlebige Medien-Token; die `resource` -Parameter ist der Inhalt, den der Benutzer anzeigen darf.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

      - Ausgelöst von `checkAuthorization()` und `getAuthorization()` nach einer nicht erfolgreichen Autorisierung.
      - Die `resource` -Parameter ist der Inhalt, den der Benutzer anzuzeigen versucht hat; die `code` -Parameter ist der Fehlercode, der angibt, welcher Fehlertyp aufgetreten ist; der `description` -Parameter beschreibt den Fehler, der dem Fehlercode zugeordnet ist.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

      - Ausgelöst von `getSelectedProvider()`.
      - Die `mvpd` liefert Informationen zum vom Benutzer ausgewählten Provider.

   - [`setMetadataStatus(metadata, key, arguments)`](#$setMetadataStatus)

      - Ausgelöst von `getMetadata().`
      - Die `metadata` liefert die spezifischen Daten, die Sie angefordert haben; die `key` -Parameter ist der Schlüssel, der in der Variablen `getMetadata()` und die `arguments` ist dasselbe Wörterbuch, das an `getMetadata()`.

   - [`preauthorizedResources(resources)`](#$preauthResources)

      - Ausgelöst von `checkPreauthorizedResources()`.
      - Die `authorizedResources` -Parameter zeigt die Ressourcen an, die der Benutzer anzeigen darf.


![](assets/android-entitlement-flows.png)


### B. Startup Flow {#startup_flow}

1. Starten Sie die Anwendung der obersten Ebene.
1. Initiieren der Adobe Pass-Authentifizierung.

   1. Aufruf [`getInstance`](#$getInstance) , um eine Instanz der Adobe Pass-Authentifizierung zu erstellen `AccessEnabler`.

      - **Abhängigkeit:** Adobe Pass Authentication Native Amazon FireOS Library (`AccessEnabler`)

   1. Aufruf` setRequestor()` Festlegung der Identität des Programmierers; Weiterleitung der Programmierung `requestorID` und (optional) ein Array von Adobe Pass-Authentifizierungsendpunkten.

      - **Abhängigkeit:** Gültige Adobe Pass Authentication RequestID (wenden Sie sich an Ihren Adobe Pass Authentication Account Manager, um dies anzuordnen).

      - **Trigger:** setRequestorComplete()-Rückruf

   Berechtigungsanfragen können erst abgeschlossen werden, wenn die Identität des Anfragenden vollständig ermittelt wurde. Dies bedeutet effektiv, dass setRequestor() zwar noch ausgeführt wird, jedoch alle nachfolgenden Berechtigungsanfragen (z. B.`checkAuthentication()`) blockiert werden.

   Sie haben zwei Implementierungsoptionen: Sobald die Identifizierungsinformationen des Anfragenden an den Backend-Server gesendet wurden, kann die UI-Anwendungsschicht einen der beiden folgenden Ansätze wählen:</p>

   1. Warten Sie auf die Auslösung der `setRequestorComplete()` callback (Teil der `AccessEnabler` delegate).  Diese Option bietet die größte Sicherheit, dass `setRequestor()` abgeschlossen ist, daher wird dies für die meisten Implementierungen empfohlen.
   1. Fahren Sie fort, ohne auf die Aktivierung der `setRequestorComplete()` zurücksetzen und mit der Ausgabe von Berechtigungsanfragen beginnen. Diese Aufrufe (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) werden vom `AccessEnabler` -Bibliothek, die die tatsächlichen Netzwerkaufrufe nach der `setRequestor()`. Diese Option kann gelegentlich unterbrochen werden, wenn beispielsweise die Netzwerkverbindung instabil ist.

1. Aufruf [checkAuthentication()](#$checkAuthN) um nach einer vorhandenen Authentifizierung zu suchen, ohne den vollständigen Authentifizierungsfluss zu starten.  Wenn dieser Aufruf erfolgreich ist, können Sie direkt zum Autorisierungsfluss übergehen.  Ist dies nicht der Fall, fahren Sie mit dem Authentifizierungsfluss fort.

- **Abhängigkeit:** Ein erfolgreicher Aufruf an `setRequestor()` (Diese Abhängigkeit gilt auch für alle nachfolgenden Aufrufe).

- **Trigger:** setAuthenticationStatus()-Rückruf

### C. Authentifizierungsfluss {#authn_flow}

1. Aufruf [`getAuthentication()`](#$getAuthN) , um den Authentifizierungsfluss zu initiieren oder um zu bestätigen, dass der Benutzer bereits authentifiziert ist.

   **Trigger:**

   - Der Rückruf setAuthenticationStatus() , wenn der Benutzer bereits authentifiziert ist.  Gehen Sie in diesem Fall direkt zum [Autorisierungsfluss](#authz_flow).
   - Der Rückruf displayProviderDialog() , wenn der Benutzer noch nicht authentifiziert ist.

1. Präsentieren Sie den Benutzer mit der Liste der Anbieter, die an gesendet werden `displayProviderDialog()`.
1. Nachdem der Benutzer einen Anbieter ausgewählt hat, öffnet eine WebView die Anbieterseite, auf der sich der Benutzer anmelden kann

   >[!NOTE]
   >
   >An dieser Stelle hat der Benutzer die Möglichkeit, den Authentifizierungsfluss abzubrechen. Wenn dies der Fall ist, wird die `AccessEnabler` bereinigt den internen Status und setzt den Authentifizierungsfluss zurück.

1. Nach erfolgreicher Anmeldung durch den Benutzer wird die WebView geschlossen.
1. call `getAuthenticationToken(),` , der die `AccessEnabler` , um das Authentifizierungstoken vom Backend-Server abzurufen.
1. [Optional] Aufruf [`checkPreauthorizedResources(resources)`](#$checkPreauth) , um zu überprüfen, welche Ressourcen der Benutzer anzeigen darf. Die `resources` parameter ist ein Array geschützter Ressourcen, die mit dem Authentifizierungstoken des Benutzers verknüpft sind.

   **Trigger:** `preAuthorizedResources()` callback\
   **Ausführungspunkt:** Nach Abschluss des Authentifizierungsablaufs

1. Wenn die Authentifizierung erfolgreich war, fahren Sie mit dem Autorisierungsfluss fort.


### D. Genehmigungsprozess {#authz_flow}

1. Aufruf [`getAuthorization()`](#$getAuthZ) , um den Genehmigungsprozess einzuleiten.

   Abhängigkeit: Gültige ResourceID(s), die mit den MVPD(s) vereinbart wurde.

   **Hinweis:** ResourceIDs sollten mit denen auf anderen Geräten oder Plattformen übereinstimmen und sind über MVPDs hinweg identisch.

1. Validieren Sie Authentifizierung und Autorisierung.

   - Wenn die Variable `getAuthorization()` Aufruf erfolgreich: Der Benutzer verfügt über gültige AuthN- und AuthZ-Token (der Benutzer ist authentifiziert und berechtigt, die angeforderten Medien zu sehen).
   - Wenn `getAuthorization()` schlägt fehl: Untersuchen Sie die ausgelöste Ausnahme, um ihren Typ zu bestimmen (AuthN, AuthZ oder etwas Anderes):
      - Wenn es sich um einen Authentifizierungsfehler (AuthN) handelte, starten Sie den Authentifizierungsfluss neu.
      - Wenn es sich um einen Autorisierungsfehler (AuthZ) handelt, ist der Benutzer nicht berechtigt, das angeforderte Medium zu sehen und dem Benutzer sollte eine Fehlermeldung angezeigt werden.
      - Wenn ein anderer Fehlertyp aufgetreten ist (Verbindungsfehler, Netzwerkfehler usw.) zeigen Sie dem Benutzer eine entsprechende Fehlermeldung an.

1. Validieren Sie das Token für kurze Medien.

   Verwenden Sie die Adobe Pass Authentication Media Token Verifier-Bibliothek, um das von der `getAuthorization()` Aufruf oben:

   - Wenn die Validierung erfolgreich ist: Wiedergabe des angeforderten Mediums für den Benutzer.
   - Wenn die Validierung fehlschlägt: Das AuthZ-Token war ungültig, die Medienanforderung sollte abgelehnt werden und dem Benutzer sollte eine Fehlermeldung angezeigt werden.

1. Kehren Sie zu Ihrem normalen Anwendungsfluss zurück.

### E. Medienfluss anzeigen {#media_flow}

1. Der Benutzer wählt das Medium aus, das angezeigt werden soll.
1. Sind die Medien geschützt?  Ihre Anwendung prüft, ob die ausgewählten Medien geschützt sind:
   - Wenn das ausgewählte Medium geschützt ist, startet Ihre Anwendung die [Autorisierungsfluss](#authz_flow) höher.
   - Wenn das ausgewählte Medium nicht geschützt ist, geben Sie das Medium für den Benutzer wieder.

### F. Abmeldefluss {#logout_flow}

1. Aufruf [`logout()`](#$logout) , um den Benutzer abzumelden. Die `AccessEnabler` löscht alle zwischengespeicherten Werte und Token, die der Benutzer für das aktuelle MVPD erhalten hat, auf allen Anfragenden, die die Anmeldung über Single Sign On nutzen. Nach dem Löschen des Cache wird die `AccessEnabler` führt einen Server-Aufruf aus, um die Server-seitigen Sitzungen zu bereinigen.  Da der Server-Aufruf zu einer SAML-Umleitung zum IdP führen kann (dies ermöglicht die Sitzungsbereinigung auf der IdP-Seite), muss dieser Aufruf allen Umleitungen folgen. Aus diesem Grund wird dieser Aufruf innerhalb eines WebView-Steuerelements verarbeitet, das für den Benutzer unsichtbar ist.

   **Hinweis:** Der Abmeldefluss unterscheidet sich vom Authentifizierungsfluss insofern, als der Benutzer in keiner Weise mit der WebView interagieren muss. Daher ist es möglich (und empfohlen), das WebView-Steuerelement während des Abmeldevorgangs unsichtbar (d. h. ausgeblendet) zu machen.
