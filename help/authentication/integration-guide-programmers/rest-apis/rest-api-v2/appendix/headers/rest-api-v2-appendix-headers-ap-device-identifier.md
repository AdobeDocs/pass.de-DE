---
title: Header - AP-Device-Identifier
description: REST API v2 - Kopfzeile - AP-Device-Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# Header - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Überblick {#overview}

Der <b>AP-Device-Identifier</b>-Anforderungsheader enthält die Streaming-Gerätekennung, wie sie von der Client-Anwendung erstellt wurde.

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;type&gt; &lt;identifier&gt;</td>
   </tr>
   <tr>
      <td>Kopfzeilentyp</td>
      <td>Anfrage-Header</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Nein</td>
   </tr>
</table>

## Anweisungen {#directives}

<b>&lt;type></b>

Der Typ der Gerätekennung.

Es gibt nur einen unterstützten Typ, wie unten dargestellt.

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Typ</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>Fingerabdruck</td>
      <td>
            Die Gerätekennung besteht aus einer stabilen und eindeutigen Kennung, die von der Client-Anwendung für jedes Gerät erstellt und verwaltet wird.
            <br/>
            Die Client-Anwendung sollte die Geräte-ID im persistenten Speicher zwischenspeichern, da ein Verlust oder eine Änderung die Authentifizierung ungültig macht. Die Client-Anwendung sollte Wertänderungen verhindern, die durch Benutzeraktionen wie Deinstallation, Neuinstallation oder Upgrades der Anwendung verursacht werden.
      </td>
   </tr>
</table>


<b>&lt;identifier></b>

Der `Base64-encoded` der Gerätekennung.

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
> Die Dokumentationsressourcen sind nicht vollständig und erfordern möglicherweise zusätzliche Änderungen, um in Ihrem Projekt zu funktionieren.
> 
> Unabhängig von Ihrer tatsächlichen Implementierung muss die `AP-Device-Identifier`-Kopfzeile einen Wert enthalten, der wie im Abschnitt „Anweisungen[ beschrieben ](#directives).

### Browser {#browsers}

Um den `AP-Device-Identifier` für Geräte zu erstellen, die in einem Browser ausgeführt werden, muss Ihre Client-Anwendung eine stabile und eindeutige Kennung basierend auf verfügbaren Daten wie Browser-, Geräte- oder benutzerspezifischen Daten berechnen.

_(*) Es wird empfohlen, eine Bibliothek oder einen Service zu integrieren, der einen Browser- oder Geräte-Fingerabdruckmechanismus bereitstellt._

### Mobilgeräte {#mobile-devices}

#### iOS und iPadOS {#ios-ipados}

Informationen zum Erstellen des `AP-Device-Identifier` für Geräte, auf denen [iOS oder iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes) ausgeführt wird, finden Sie in den folgenden Dokumenten:

* Apple-Entwicklerdokumentation für [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Es wird empfohlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem angegebenen Wert anzuwenden._

#### Android {#android}

Informationen zum Erstellen des `AP-Device-Identifier`-Headers für Geräte, auf denen [Android](https://developer.android.com/about/versions) ausgeführt wird, finden Sie in den folgenden Dokumenten:

* Entwicklerdokumentation für Android für [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Es wird empfohlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem angegebenen Wert anzuwenden._

### TV-Geräte angeschlossen {#tv-connected-devices}

#### tvOS {#tvos}

Informationen zum Erstellen des `AP-Device-Identifier`-Headers für Geräte, auf [ „tvOS](https://developer.apple.com/documentation/tvos-release-notes) ausgeführt wird, finden Sie in den folgenden Dokumenten:

* Apple-Entwicklerdokumentation für [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Es wird empfohlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem angegebenen Wert anzuwenden._

#### Betriebssystem auslösen {#fireos}

Informationen zum Erstellen des `AP-Device-Identifier`-Headers für Geräte, auf [ „Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) ausgeführt wird, finden Sie in den folgenden Dokumenten:

* Entwicklerdokumentation für Android für [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Es wird empfohlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem angegebenen Wert anzuwenden._

#### Roku OS {#rokuos}

Informationen zum Erstellen des `AP-Device-Identifier`-Headers für Geräte, auf [ (Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) ausgeführt wird, finden Sie in den folgenden Dokumenten:

* Roku-Entwicklerdokumentation für [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string).

_(*) Es wird empfohlen, eine SHA-256-Hash-Funktion auf den vom Betriebssystem angegebenen Wert anzuwenden._

### Sonstige {#others}

Für Geräteplattformen, die nicht in der Dokumentation behandelt werden, sollte die Gerätekennung mit jeder verfügbaren Hardwarekennung verknüpft werden, die normalerweise im Hardware-Handbuch des Geräts angegeben ist.

Wenn keine Hardware-IDs verfügbar sind, sollte eine eindeutig generierte ID basierend auf Client-Anwendungsattributen verwendet und im persistenten Speicher zwischengespeichert werden.
