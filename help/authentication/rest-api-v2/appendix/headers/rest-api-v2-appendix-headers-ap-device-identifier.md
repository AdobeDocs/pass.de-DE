---
title: Header - AP-Device-Identifier
description: REST API V2 - Header - AP-Device Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 8f4fb5d6cc8b45b300010438c56d4af2e8fc0a76
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Header - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Die Anfragekopfzeile <b>AP-Device-Identifier</b> enthält die Streaming-Geräte-ID, wie sie von der Clientanwendung erstellt wurde.

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;type&gt; &lt;identifier&gt;</td>
   </tr>
   <tr>
      <td>Kopfzeilentyp</td>
      <td>Anforderungs-Header</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Nein</td>
   </tr>
</table>

## Richtlinien {#directives}

<b>&lt;type></b>

Der Gerätekennungstyp.

Es gibt nur einen unterstützten Typ, wie unten dargestellt.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Typ</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>Fingerabdruck</td>
      <td>
            Die Gerätekennung besteht aus einer stabilen und eindeutigen Kennung, die von der Clientanwendung erstellt und verwaltet wird.
            <br/>
            Die Clientanwendung muss Wertänderungen verhindern, die durch Benutzeraktionen wie Deinstallation, Neuinstallation oder Aktualisierung der Anwendung verursacht werden.
      </td>
   </tr>
</table>


<b>&lt;identifier></b>

Der `Base64-encoded` -Wert der Geräte-ID.

## Beispiel {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
>
> Die Dokumentationsressourcen werden zu Referenzzwecken bereitgestellt.
>
> Die Dokumentationsressourcen sind nicht vollständig und erfordern möglicherweise zusätzliche Änderungen, damit Sie in Ihrem Projekt arbeiten können.
> 
> Unabhängig von Ihrer eigentlichen Implementierung muss die Kopfzeile `AP-Device-Identifier` einen formatierten Wert enthalten, wie im Abschnitt [Direktiven](#directives) beschrieben.

### Browser {#browsers}

Um die Kopfzeile &quot;`AP-Device-Identifier`&quot;für Geräte zu erstellen, die in einem Browser ausgeführt werden, muss Ihre Clientanwendung eine stabile und eindeutige Kennung basierend auf verfügbaren Daten wie Browser-, Geräte- oder benutzerspezifischen Daten berechnen.

_(*) Es wird empfohlen, eine Bibliothek oder einen Dienst zu integrieren, die bzw. der einen Fingerabdruckmechanismus für Browser oder Geräte bereitstellt._

### Mobilgeräte {#mobile-devices}

#### iOS und iPadOS {#ios-ipados}

Um die Kopfzeile `AP-Device-Identifier` für Geräte zu erstellen, auf denen [iOS oder iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes) ausgeführt wird, lesen Sie die folgenden Dokumente:

* Apple-Entwicklerdokumentation für [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Wir empfehlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem bereitgestellten Wert anzuwenden._

#### Android {#android}

Um die Kopfzeile `AP-Device-Identifier` für Geräte zu erstellen, auf denen [Android](https://developer.android.com/about/versions) ausgeführt wird, lesen Sie die folgenden Dokumente:

* Android-Entwicklerdokumentation für [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Wir empfehlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem bereitgestellten Wert anzuwenden._

### TV-vernetzte Geräte {#tv-connected-devices}

#### tvOS {#tvos}

Um die Kopfzeile `AP-Device-Identifier` für Geräte zu erstellen, auf denen [tvOS](https://developer.apple.com/documentation/tvos-release-notes) ausgeführt wird, lesen Sie die folgenden Dokumente:

* Apple-Entwicklerdokumentation für [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Wir empfehlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem bereitgestellten Wert anzuwenden._

#### Fire OS {#fireos}

Um die Kopfzeile `AP-Device-Identifier` für Geräte zu erstellen, auf denen [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) ausgeführt wird, lesen Sie die folgenden Dokumente:

* Android-Entwicklerdokumentation für [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Wir empfehlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem bereitgestellten Wert anzuwenden._

#### Roku OS {#rokuos}

Um die Kopfzeile `AP-Device-Identifier` für Geräte zu erstellen, auf denen [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) ausgeführt wird, lesen Sie die folgenden Dokumente:

* Roku-Entwicklerdokumentation für [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string).

_(*) Wir empfehlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem bereitgestellten Wert anzuwenden._
