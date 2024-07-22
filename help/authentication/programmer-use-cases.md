---
title: Anwendungsfälle für Programmierer
description: Anwendungsfälle für Programmierer
exl-id: 51ca7e4f-b0d8-4e35-8398-2efb4879de2a
source-git-commit: 189d0e5c83d1bc171c6ff3057f22dbf4055ea6b1
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# Anwendungsfälle für Programmierer {#programmer-use-cases}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

In diesem Dokument werden die Anwendungsfälle für die Programmintegration zusammengefasst, die von der Adobe Pass-Authentifizierung unterstützt werden. Sie können diese Seite vor dem Beginn eines Integrationsprojekts überprüfen, um zu sehen, welche Funktionen derzeit unterstützt werden.

## Anwendungsbeispiele {#use-cases}


### Grundlegende Integration: Federated Authentication and Authorization für ein einzelnes Kanal-Netzwerk {#basic-integration}

**Priorität** - Hoch

**Aufschlüsselung** - TVE-App mit Einzelprogrammierermarke mit 1 Kanalnetzwerk, das innerhalb des Erlebnisses gehostet wird

Dadurch können Programmierer in ihrer eigenen TVE-App* Premium-Inhalte mit einer Federated-Berechtigungsprüfung an den MVPD anbieten. Die Anforderer-ID sollte an der Marke der Anwendung ausgerichtet werden, die den Inhalt für den Viewer bereitstellt. In diesem Szenario besteht eine 1:1-Beziehung zwischen der Adobe Pass-Authentifizierungsanfrage-ID und der Ressourcen-ID, die für die Berechtigung verifiziert wird.

>[!NOTE]
>
>Die TVE-App wird in diesem Dokument verwendet, um kollektiv auf die verschiedenen Anwendungstypen (Web-Apps, mobile Apps usw.) zu verweisen. unterstützt von der Adobe Pass-Authentifizierung. Die Spalte Plattformen unten kann Details zu unterstützten Plattformen für bestimmte Anwendungsfälle enthalten.

#### Spezifische Anwendungsfälle (häufig bei den meisten Integrationen) {#sp-use-cases-basic-int}

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|:--------:|:-----------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------:|:-----------------------------------------:|
| Hoch | MVPD Discovery von der Programmierer-TVE-App | Der Benutzer startet in der TVE-App des Programmierers und wird aufgefordert, seinen MVPD-Provider auszuwählen. | Web (SWF/JS)                    Mobil (iOS/Android)                   Clientlose API (für 2. Bildschirm) |                                           |
| Hoch | Federated Authentication from the Programmer TVE App | Der Benutzer startet in der TVE-App des Programmierers und nach Auswahl seines MVPD-Anbieters wechselt der Benutzer zur eigenen Anmeldeseite des MVPD, um seine Anmeldeinformationen einzugeben. | Web (SWF/JS)                    Mobil (iOS/Android) |                                           |
| Hoch | Autorisierung von der Programmierer-TVE-App | Nachdem der Benutzer authentifiziert wurde, kann die TVE-App des Programmierers Back-Channel-Autorisierungsanfragen an den MVPD senden, um die Berechtigung des Benutzers zu überprüfen. Normalerweise wird dabei nur geprüft, ob sich das Channel-Netzwerk im Benutzer-MVPD-Abonnement-Paket befindet.                                  In diesem Fall stimmen die Anforderer-ID und die Ressourcen-ID mit 1:1 überein. | Alle Plattformen |                                           |
| Medium | Abmelden von der Programmierungs-TVE-App | Ermöglicht dem Benutzer das Abmelden und Löschen der Adobe Pass-Authentifizierungs-AuthN/AuthZ-Token. In vielen Fällen meldet dies auch den Benutzer aus dem MVPD ab. MVPDs variieren jedoch in der Frage, ob dies unterstützt wird. Es werden immer die Adobe Pass-Authentifizierungssitzung und Token gelöscht. | Alle Plattformen außer XBox Native | Mehrere MVPDs unterstützen dies nicht. |
| Hoch | Single-Sign-On über Sites und Apps hinweg | Ermöglicht dem Benutzer, die Anmeldesitzung über Sites und Apps hinweg freizugeben, ohne sich erneut anmelden zu müssen. | Alle Plattformen außer ClientLess API | Erfordert mindestens SDK 1.7 für einige MVPDs. |

### Einzelne TVE-App, die mehrere Kanalnetzwerke hostet {#single-app-multi-channel}

**Priorität** - Hoch

Ermöglicht dem Programmierer das Aggregieren mehrerer Inhaltskanäle auf demselben Markenziel für die Betrachter.

#### Spezifische Anwendungsfälle {#sp-use-cases-singl-tve-app}

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hoch | Unique Channel-Autorisierung | Der Benutzer kann Inhalte aus mehreren Kanalnetzwerken innerhalb derselben TVE-App ansehen. Der Programmierer kann Autorisierungsaufrufe durchführen, die für jedes Kanalnetzwerk spezifisch sind, um die Berechtigung des Benutzers zu bestätigen. | Alle Plattformen | Alle MVPDs unterstützen dies jetzt in irgendeiner Form. |
| Niedrig | Preflight-Autorisierungsabfrage | Dadurch kann der Programmierer in einem einzelnen API-Aufruf überprüfen, welche Kanäle der Benutzer in seinem Paket hat. Dies geschieht vor tatsächlichen AuthZ-Aufrufen, um Inhalte aus der Benutzeroberfläche zu filtern, auf die der Benutzer keinen Zugriff hat. |               | Die meisten MVPDs stellen diese Daten noch nicht als Benutzerattribute zur Verfügung. Daher führt Adobe AuthZ-Aufrufe durch, um sie zu erhalten. Außerdem sind die meisten MVPDs auf jeweils fünf begrenzt, da sie nicht mehrere Kanäle in einem einzelnen Aufruf unterstützen.                             Es ist sehr wichtig zu überprüfen, wie viele Kanäle der Programmierer benötigt, um den Preflight-Check durchzuführen. Unabhängig von der Nummer müssen wir überprüfen, ob sie mit den MVPDs in Ordnung ist. Die meisten MVPDs unterstützen derzeit nicht mehr als 5 Kanäle (3. Quartal 2013). |

### Autorisierung auf Asset-Ebene {#asset-level-authz}

**Priorität** - niedrig

**Aufschlüsselung** - Weitergeben einer Asset-Kennung bei Genehmigungsanfrage

**Plattformen** - Alle Plattformen

#### Spezifische Anwendungsfälle {#sp-use-cases-asset-lvl-authz}

Ermöglicht dem MVPD, bei jedem AuthZ-Aufruf Analysen auf Asset-Ebene zu erhalten. Dies hat den Nachteil, dass der Adobe Pass Authentication AuthZ-Cache negiert wird.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|-------------|--------------------------------------|
| Niedrig | Weitergeben einer Asset-ID in einer Autorisierungsanforderung | Ermöglicht dem MVPD, bei jedem AuthZ-Aufruf Analysen auf Asset-Ebene zu erhalten.  Hat den Nachteil, dass der Adobe Pass Authentication AuthZ-Cache negiert wird. | Alle Plattformen | Nur ein MVPD unterstützt dies derzeit. |




### Elternkontrollen {#parental-controls}

**Priorität** - niedrig

Aktiviert die Anwendung von MVPD-Benutzerkontobeschränkungen auf die TVE-App des Programmierers.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|-----------------------------------|
| Niedrig | Filtern von Inhalten anhand von Benutzerattributen | Ermöglicht dem Programmierer, die zulässige Maximalbewertung für einen Benutzer zu überprüfen, bevor die Liste der verfügbaren Inhalte für den Benutzer gerendert wird. | Web (Flash/JS)                    Mobil (iOS/Android) | Funktioniert derzeit nur mit einem MVPD. |
| Niedrig | Übergeben von Inhaltsbewertungen in der AuthZ-Anfrage | Ermöglicht es dem Programmierer, die spezifische Bewertung des Inhalts, den der Benutzer im Rahmen der AuthZ-Anfrage ansehen möchte, an den MVPD zu übergeben.                             Bezieht sich auf Nr. 3, da sich Bewertungen normalerweise auf der Asset-Ebene befinden. | Alle Plattformen | Funktioniert derzeit nur mit einem MVPD. |

#### Anpassung der MVPD-Integration pro Programmierer-Marke {#mvpd-int-cust-prog-brand}

**Priorität** - Medium

Aktiviert das benutzerdefinierte Erlebnis während AuthN oder für AuthZ-Fehlermeldungen.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------------------------------------|
| Medium | Übergeben Sie die Service Provider-ID in der AuthN-Anfrage. | Aktivieren Sie spezifisches Branding auf der für den Dienstleister spezifischen MVPD-Anmeldeseite. Aktivieren Sie außerdem die automatische Auswahl der Standardeinstellung, die der Zielgruppe wie Spanisch für Univision entspricht. | Alle Plattformen | Variiert nach MVPD. Einige unterstützen dies nicht. |
| Medium | Benutzerdefinierte Fehlermeldungen bei AuthZ-Antwort | Aktiviert programmgesteuerte oder markenspezifische Fehlermeldungen aus dem MVPD, die eine bestimmte Nachricht für den Upsell mit einem Link enthalten können, der das Paket aktualisiert. | Web, Android, iOS | Variiert nach MVPD. Einige unterstützen dies nicht. |


### Anwendungsfälle für verbundene Geräte {#connected-devices}

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Medium | XBox LiveID SSO über Apps und Konsolen hinweg | Ermöglicht dem Benutzer, die AuthN-Sitzung zwischen Apps und zwischen verschiedenen Spielkonsolen freizugeben, die mit seinem LiveID-Konto verknüpft sind. | Natives XBox-SDK | Die meisten MVPDs mögen dies nicht, da das typische Modell darin besteht, das Token an das Gerät zu binden - nicht an den Benutzer.                             Wir empfehlen diesen Ansatz nicht mehr, wenn möglich. |
| Hoch | Verbundenes Gerät mit Token, die an die appID auf dem Gerät gebunden sind | Ermöglicht dem Programmierer, die MVPD-Berechtigung im Token an die appID auf dem Gerät zu binden, an das sie ausgegeben wurde. | Clientlose API | Dadurch wird das Connected Device enger an der standardmäßigen Implementierung für Token für die Übermittlung angepasst.                             Eine geräteweite ID muss weiterhin verbessert werden. |

### Gerätespezifische AuthN-TTL-Länge {#authn-ttl-length}

Aktivieren Sie die TVE-Berechtigung für Spezialereignisse, bei denen es sich möglicherweise nicht um Ressourcen handelt, die sich in der MVPD-Berechtigungsdatenbank wie normale Kanäle befinden.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------|
| Hoch | Festlegen verschiedener TTL-Werte pro Plattform | Ermöglicht es dem Programmierer, eine andere TTL-Länge für Web-, Mobile- und vernetzte Geräte festzulegen. Derzeit unterstützt die Adobe Pass-Authentifizierung die Möglichkeit, drei separate TTL-Werte zu verwenden:                                Web (Flash)                    Mobil/HTML5                    Clientless - Connected Devices |           | Einige MVPDs legen die TTL dynamisch fest. Adobe kann diese dynamischen Einstellungen bei Bedarf mithilfe der Konfigurationseinstellungen überschreiben. |

### Spezielle ereignisbasierte Anwendungen {#special-event}

**Priorität** - niedrig

Aktivieren Sie die TVE-Berechtigung für Spezialereignisse, bei denen es sich möglicherweise nicht um Ressourcen handelt, die sich in der MVPD-Berechtigungsdatenbank wie normale Kanäle befinden.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|------------------------------------------|
| Niedrig | Mehrere Kanäle als Proxy für ein Ereignis | Dies geschah für die Olympischen Spiele, bei denen der Abonnent zwei verschiedene Kanäle in seinem Paket brauchte, um Zugang zu erhalten. In diesem Fall erstellte die Adobe Pass-Authentifizierung eine neue resourceID und ließ alle MVPDs die Zuordnung zu den spezifischen Kanälen auf ihrem Ende durchführen.  Das funktionierte gut mit genügend fortgeschrittenem Hinweis. Dies war wichtig, da die meisten MVPDs nicht mehrere Ressourcenaufrufe unterstützen. | Alle Plattformen | Unterstützt von allen MVPDs mit entsprechender Benachrichtigung. |
| Niedrig | Spezielle Anwendung für neue Ereignisse unter Verwendung vorhandener Kanalressourcen | Das wurde für den Madness-Märchen gemacht. Der Inhaltsanbieter hat eine neue App mit einer neuen Anforderer-ID erstellt. Alle MVPDs, die benötigt werden, um Unterstützung für die neue requestorID in ihrem System hinzuzufügen. Die resourceIDs waren normale Kanäle.  Einige MVPDs mussten die Kanäle auch unter der neuen Anfrage als gültig zuordnen, sodass für diese Fälle mehr Zeit benötigt wurde. | Alle Plattformen | Unterstützt von allen MVPDs mit entsprechender Benachrichtigung. |
| Niedrig | Vorhandene requestorID, resourceID | Dies wurde für das Masters Golf Wochenende Turnier getan. Es war nur ein kleines Ereignis für ein paar Tage, und die Master hatten ihre eigene mobile App, die den Inhalt anzeigte. Der Programmierer plante, für den Adobe Pass-Authentifizierungs-Traffic zu bezahlen und nutzte einfach seine standardmäßige requestorID und resourceID. Der einzige Trick war, dass der Programmierer ein mobiles Cert für die Signierung von requestorID mit den Masters teilen ließ, und dass dies zu ihrer Konfiguration als Backup-Cert für dieses Wochenende hinzugefügt wurde. | Alle Plattformen | Keine Auswirkungen für MVPDs |

### Integration von Inhaltsservern {#content-server-integration}

**Priorität** - Medium

Aktivieren der Validierung des Medien-Tokens vor dem Freigeben des Video-Streams im Client-Player

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|---------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------|
| Hoch | Federated Player für Programmierer - mit Autorisierung auf Seitenebene | Adobe Pass-Authentifizierungs-APIs werden in JavaScript auf der Seite ausgeführt und das Token wird an den Player übergeben. Token können auf verschiedene Weise an den Validierungsdienst übergeben werden:                                 Parameter für die Validierungsdienst-URL abrufen                    URL-Parameter, der in der Abfragezeichenfolge der Stream-URL übergeben wird                    Externe API                    FlashVars |           |            |
| Medium | Programmierer Federated Player - mit interner Player-Autorisierung | Adobe Pass-Authentifizierungs-APIs werden im ActionScript auf der Player-SWF ausgeführt, sodass das Token vom Callback aus für den Player verfügbar ist. |           |            |
| Hoch | Syndizierter Player - Auf MVPD-Portal gehostet mit Seitenebenenautorisierung mit einem iFrame zum Aufnehmen des Players | Ähnlich wie der Player mit Autorisierung auf Seitenebene, jedoch mit dem Player-Seiten-Wrapper iFramed in das MVPD-Portal. Die Authentifizierung muss separat im MVPD-Portal erfolgen. |           |                        |


<!--
>[!RELATEDINFORMATION]
>
>* MVPD Integration Features
>* Entitlement Flow
>* Platform / Device Requirements
-->
