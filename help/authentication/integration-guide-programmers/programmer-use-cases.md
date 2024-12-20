---
title: Anwendungsfälle für Programmierer
description: Anwendungsfälle für Programmierer
exl-id: 51ca7e4f-b0d8-4e35-8398-2efb4879de2a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 1%

---

# Anwendungsfälle für Programmierer {#programmer-use-cases}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#overview}

In diesem Dokument werden die Anwendungsfälle der Programmierer-Integration zusammengefasst, die von der Adobe Pass-Authentifizierung unterstützt werden. Sie können diese Seite überprüfen, bevor Sie ein Integrationsprojekt starten, um zu sehen, welche Funktionen derzeit unterstützt werden.

## Anwendungsbeispiele {#use-cases}


### Einfache Integration: Federated Authentication and Authorization for a Single Channel Network {#basic-integration}

**Priorität** - Hoch

**Aufschlüsselung** - TVE-App mit einem Programmierer und gehostetem 1-Kanal-Netzwerk im Erlebnis

Dadurch können Programmierer Premium-Inhalte in ihrer eigenen markenspezifischen TVE-App* mit einer Federated-Berechtigungsprüfung für die MVPD anbieten. Die Anforderer-ID sollte so ausgerichtet sein, dass sie mit der Marke des Programms, das den Inhalt bereitstellt, zum Viewer übereinstimmt. In diesem Szenario besteht eine 1:1-Beziehung zwischen der Anforderer-ID der Adobe Pass-Authentifizierung und der Ressourcen-ID, die auf Berechtigung überprüft wird.

>[!NOTE]
>
>Die TVE-App wird in diesem Dokument verwendet, um kollektiv auf die verschiedenen Arten von Anwendungen (Web-Apps, Mobile Apps usw.) zu verweisen, die von der Adobe Pass-Authentifizierung unterstützt werden. Die nachstehende Spalte Plattformen enthält möglicherweise Details zu unterstützten Plattformen für bestimmte Anwendungsfälle.

#### Spezifische Anwendungsfälle (häufig bei den meisten Integrationen) {#sp-use-cases-basic-int}

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|:--------:|:-----------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------:|:-----------------------------------------:|
| Hoch | MVPD Discovery vom Programmierer TVE App | Der Benutzer startet mit der markenspezifischen TVE-App des Programmierers und wird aufgefordert, seinen MVPD-Anbieter auszuwählen. | Web (SWF/JS)                    Mobile (iOS/Android)                   Client-lose API (für den 2. Bildschirm) |                                           |
| Hoch | Federated Authentication From the Programmer TVE App | Der/die Benutzende startet in der gebrandeten TVE-App des/r Programmierenden und nach der Auswahl des/r MVPD-Anbieters/in wird zur MVPD-Anmeldeseite gewechselt, um seine/ihre Anmeldeinformationen einzugeben. | Web (SWF/JS)                    Mobile (iOS/Android) |                                           |
| Hoch | Autorisierung durch Programmierer TVE App | Nachdem der Benutzer authentifiziert wurde, kann die TVE-App des Programmierers Backchannel-Autorisierungsanfragen an den MVPD senden, um die Berechtigung des Benutzers zu überprüfen. Normalerweise wird nur überprüft, ob das Kanalnetzwerk im MVPD-Abonnementpaket für Benutzer enthalten ist.                                  In diesem Fall stimmen die Anforderer-ID und die Ressourcen-ID 1:1 überein. | Alle Plattformen |                                           |
| Medium | Abmelden von der Programmierer-TVE-App | Ermöglicht dem Benutzer das Abmelden und Löschen der Adobe Pass-Authentifizierungs-AuthN/AuthZ-Token. In vielen Fällen wird der Benutzer dadurch auch von der MVPD abgemeldet. MVPDs unterscheiden sich jedoch in der Frage, ob dies unterstützt wird. Die Adobe Pass-Authentifizierungssitzung und die Token werden immer gelöscht. | Alle Plattformen außer XBox Native | Mehrere MVPDs unterstützen dies nicht. |
| Hoch | Single Sign-on für Sites und Apps | Ermöglicht es dem Benutzer, die Anmeldesitzung über Sites und Apps hinweg zu teilen, ohne sich erneut anmelden zu müssen. | Alle Plattformen außer der Client-losen API | Für einige MVPDs ist mindestens SDK 1.7 erforderlich. |

### Einzelne TV-App, die mehrere Kanalnetze hostet {#single-app-multi-channel}

**PRIORITÄT** HOCH

Ermöglicht es dem Programmierer, mehrere Kanalnetze mit Inhalten für seine Betrachter im selben Markenziel zu aggregieren.

#### Spezifische Anwendungsfälle {#sp-use-cases-singl-tve-app}

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hoch | Einzelkanalautorisierung | Der Benutzer kann Inhalte aus mehreren Kanalnetzwerken innerhalb derselben TV-App ansehen. Der Programmierer kann für jedes Kanalnetz spezifische Autorisierungsaufrufe ausführen, um die Berechtigung der Benutzer zu bestätigen. | Alle Plattformen | Alle MVPDs unterstützen dies nun in irgendeiner Form. |
| niedrig | Autorisierungsabfrage vor einem Flug | Dadurch kann der Programmierer in einem einzigen API-Aufruf überprüfen, welche Kanäle der Benutzer im Paket hat. Dies geschieht vor den eigentlichen AuthZ-Aufrufen, um Inhalte aus der Benutzeroberfläche zu filtern, auf die der Benutzer keinen Zugriff hat. |               | Die meisten MVPDs machen diese Daten noch nicht als Benutzerattribute verfügbar, sodass Adobe tatsächlich AuthZ-Aufrufe durchführt, um sie zu erhalten. Außerdem sind die meisten MVPDs auf jeweils 5 beschränkt, da sie nicht mehrere Kanäle in einem einzigen Aufruf unterstützen.                             Es ist sehr wichtig zu überprüfen, wie viele Kanäle der Programmierer benötigt, um Preflight zu überprüfen. Unabhängig von der Nummer müssen wir überprüfen, ob es mit den MVPDs in Ordnung ist. Die meisten MVPDs unterstützen derzeit nicht mehr als 5 Kanäle (3. Quartal 2013). |

### Autorisierung auf Asset-Ebene {#asset-level-authz}

**Priorität** - Niedrig

**Aufschlüsselung** - Übergeben einer Asset-Kennung auf der Autorisierungsanfrage

**Plattformen** - Alle Plattformen

#### Spezifische Anwendungsfälle {#sp-use-cases-asset-lvl-authz}

Ermöglicht MVPD, bei jedem AuthZ-Aufruf eine Analyse auf Asset-Ebene abzurufen. Dies hat den Nachteil, dass der AuthZ-Cache der Adobe Pass-Authentifizierung negiert wird.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|-------------|--------------------------------------|
| niedrig | Übergeben einer Asset-Kennung auf Autorisierungsanfrage | Ermöglicht MVPD, bei jedem AuthZ-Aufruf eine Analyse auf Asset-Ebene abzurufen.  Dies hat den Nachteil, dass der AuthZ-Cache der Adobe Pass-Authentifizierung negiert wird. | Alle Plattformen | Dies wird derzeit nur von einer MVPD unterstützt. |




### Kindersicherung {#parental-controls}

**Priorität** - Niedrig

Ermöglicht die Anwendung von MVPD-Benutzerkontobeschränkungen auf die TVE-App des Programmierers.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|-----------------------------------|
| niedrig | Filtern von Inhalt anhand von Benutzerattributen | Ermöglicht es dem Programmierer, die für einen Benutzer maximal zulässige Bewertung zu überprüfen, bevor er die Liste der verfügbaren Inhalte für den Benutzer rendert. | Web (Flash/JS)                    Mobile (iOS/Android) | Funktioniert derzeit nur mit einer MVPD. |
| niedrig | Übergeben von Inhaltsbewertungen in der AuthZ-Anfrage | Ermöglicht es dem Programmierer, die spezifische Bewertung der Inhalte, die er im Rahmen der AuthZ-Anfrage ansehen möchte, an MVPD zu übergeben                             Mit #3 verbunden, da Ratings normalerweise auf Asset-Ebene erstellt werden. | Alle Plattformen | Funktioniert derzeit nur mit einer MVPD. |

#### Anpassung der MVPD-Integration nach Programmierermarke {#mvpd-int-cust-prog-brand}

**Priorität** - Medium

Aktiviert benutzerdefiniertes Erlebnis während der AuthZ oder für AuthZ-Fehlermeldungen.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------------------------------------|
| Medium | Übergeben Sie die Kennung des Dienstleisters in der Authentifizierungsanfrage. | Aktivieren Sie das spezifische Branding auf der für den Dienstleister spezifischen MVPD-Anmeldeseite. Aktivieren Sie außerdem die automatische Auswahl der Standardeinstellung, die mit der Zielgruppe übereinstimmt, z. B. Spanisch für Univision . | Alle Plattformen | Variiert je nach MVPD. Einige unterstützen dies nicht. |
| Medium | Benutzerdefinierte Fehlermeldungen in der AuthZ-Antwort | Aktiviert Programmierer- oder markenspezifische Fehlermeldungen von MVPD, die eine bestimmte Upsell-Nachricht mit einem Link enthalten können, der das Paket aktualisiert. | Web, Android, iOS | Variiert je nach MVPD. Einige unterstützen dies nicht. |


### Anwendungsfälle für verbundene Geräte {#connected-devices}

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Medium | XBox LiveID SSO in Apps und Konsolen | Ermöglicht es dem Benutzer, die AuthN-Sitzung zwischen Apps und zwischen verschiedenen Spielekonsolen zu teilen - verknüpft mit seinem LiveID-Konto. | Native XBox-SDK | Die meisten MVPDs mögen dies nicht, da das typische Modell darin besteht, das Token an das Gerät und nicht an den Benutzer zu binden.                             Wir empfehlen diesen Ansatz nicht mehr, wenn überhaupt möglich. |
| Hoch | Verbundenes Gerät mit Token, die an die App-ID auf dem Gerät gebunden sind | Ermöglicht es dem Programmierer, die MVPD-Berechtigung im Token an die appID des Geräts zu binden, für das sie ausgegeben wurde. | Clientless-API | Dadurch wird das verbundene Gerät enger an der standardmäßigen Pass-Implementierung für Token ausgerichtet.                             Muss dennoch verbessert werden, um eine geräteweite ID zu sein. |

### Gerätespezifische AuthN-TTL-Länge {#authn-ttl-length}

Aktivieren Sie die TVE-Berechtigung für besondere Ereignisse, bei denen es sich möglicherweise nicht um Ressourcen handelt, die sich wie normale Kanäle in der MVPD-Berechtigungsdatenbank befinden.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------|
| Hoch | Festlegen verschiedener TTL-Werte pro Plattform | Ermöglicht es dem Programmierer, eine andere TTL-Länge für Web-, mobile und verbundene Geräte festzulegen. Derzeit unterstützt die Adobe Pass-Authentifizierung die Möglichkeit von drei separaten TTL-Werten:                                Web (Flash)                    Mobil/HTML5                    Clientlos - verbundene Geräte |           | Einige MVPDs legen die TTL dynamisch fest. Adobe kann diese dynamischen Einstellungen bei Bedarf mithilfe der Konfigurationseinstellungen überschreiben. |

### Spezielle ereignisbasierte Anwendungen {#special-event}

**Priorität** - Niedrig

Aktivieren Sie die TVE-Berechtigung für besondere Ereignisse, bei denen es sich möglicherweise nicht um Ressourcen handelt, die sich wie normale Kanäle in der MVPD-Berechtigungsdatenbank befinden.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|--------|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|------------------------------------------|
| niedrig | Mehrere Kanäle als Proxy für ein Ereignis | Dies geschah bei den Olympischen Spielen, wo die Teilnehmer zwei verschiedene Kanäle in ihrem Paket brauchten, um Zugang zu haben. In diesem Fall hat die Adobe Pass-Authentifizierung eine neue resourceID erstellt und alle MVPDs die Zuordnung zu den spezifischen Kanälen auf ihrer Seite durchführen lassen.  Das funktionierte gut und rechtzeitig. Dies war wichtig, da die meisten MVPDs mehrere Ressourcenaufrufe nicht unterstützen. | Alle Plattformen | Unterstützt von allen MVPDs mit der richtigen Ankündigung. |
| niedrig | Spezielle neue Ereignisanwendung unter Verwendung vorhandener Kanalressourcen | Das war für den März-Wahnsinn. Der Content Provider hat eine neue App mit einer neuen RequestorID erstellt. Alle MVPDs müssen Unterstützung für die neue RequestorID in ihrem System hinzufügen. Die Ressourcen-IDs waren normale Kanäle.  Einige MVPDs mussten auch die Kanäle unter dem neuen Antragsteller als gültig zuordnen, sodass für diese Fälle mehr Zeit benötigt wurde. | Alle Plattformen | Unterstützt von allen MVPDs mit der richtigen Ankündigung. |
| niedrig | Vorhandene RequestorID, resourceID | Dies geschah für das Masters Golf Wochenendturnier. Es war nur ein kleines Ereignis für ein paar Tage, und die Meister hatten ihre eigene mobile App, die in Ordnung war, um den Inhalt anzuzeigen. Der Programmierer plante, für den Adobe Pass-Authentifizierungs-Traffic zu bezahlen und einfach seine standardmäßige RequestorID und resourceID zu verwenden. Der einzige Trick bestand darin, dass der Programmierer ein mobiles Zertifikat für die RequestorID-Signatur mit den Mastern teilt und dieses zu seiner Konfiguration als Backup-Zertifikat für dieses Wochenende hinzufügt. | Alle Plattformen | Keine Auswirkungen auf MVPDs |

### Content Server-Integration {#content-server-integration}

**priority**- Medium

Aktivierung der Medien-Token-Validierung vor Freigabe des Video-Streams für den Client-Player.

| Priorität | Anwendungsfall | Beschreibung | Plattformen | MVPD-Hinweise |
|---------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------|
| Hoch | Programmer Federated Player - mit Autorisierung auf Seitenebene | Adobe Pass-Authentifizierungs-APIs werden in JavaScript auf der Seite ausgeführt und das Token wird an den Player übergeben. Token können auf verschiedene Weise an den Validierungs-Service übergeben werden:                                 Parameter zur Validierungs-Service-URL abrufen                    In der Abfragezeichenfolge der Stream-URL übergebener URL-Parameter                    Externe API-Schnittstelle                    FlashVars |           |            |
| Medium | Programmer Federated Player - mit interner Player-Autorisierung | Adobe Pass-Authentifizierungs-APIs werden im ActionScript auf der Player-SWF ausgeführt, sodass das Token für den Player über den Callback verfügbar ist. |           |            |
| Hoch | Syndicated Player - wird im MVPD Portal gehostet und auf Seitenebene autorisiert. Verwendet wird ein iFrame, um den Player einzuschließen | Ähnlich wie der Player mit Autorisierung auf Seitenebene, aber mit dem Player Page Wrapper iFramed in das MVPD-Portal. Die Authentifizierung muss separat im MVPD-Portal erfolgen. |           |                        |


<!--
>[!RELATEDINFORMATION]
>
>* MVPD Integration Features
>* Entitlement Flow
>* Platform / Device Requirements
-->
