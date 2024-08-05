---
title: Header - X-Device-Info
description: REST API V2 - Header - X-Device-Info
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Header - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Der Anforderungsheader <b>X-Device-Info</b> enthält die Client-Informationen (Gerät, Verbindung und Anwendung), die sich auf das eigentliche Streaming-Gerät beziehen.

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;device_information&gt;</td>
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

<b>&lt;device_information></b>

Der `Base64-encoded` -Wert des JSON-Elements, das mindestens die Attribute enthält, die für die folgende Tabelle als erforderlich markiert sind.

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Schlüssel</th>
        <th style="background-color: #EFF2F7;">Beschreibung</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Präsenz</th>
        <th style="background-color: #EFF2F7;">Mögliche Werte</th>
    </tr>
    <tr>
        <td>primaryHardwareType</td>
        <td>Der primäre Hardwaretyp des Geräts.</td>
        <td></td>
        <td>
            Die Werte sind begrenzt:
            <ul>
                <li>Kamera</li>
                <li>DataCollectionTerminal</li>
                <li>Desktop</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>Brille</li>
                <li>MediaPlayer</li>
                <li>MobilePhone</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tablette</li>
                <li>WirelessHotspot</li>
                <li>Wristwatch</li>
                <li>unbekannt</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>model</td>
        <td>Der Modellname des Geräts.</td>
        <td><i>erforderlich</i></td>
        <td>z. B. iPhone, SM-G930V, AppleTV usw.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>Die Version des Geräts.</td>
        <td></td>
        <td>z. B. 2.0.1 usw.</td>
    </tr>
    <tr>
        <td>Hersteller</td>
        <td>Das Herstellungsunternehmen/die Organisation des Geräts.</td>
        <td></td>
        <td>z. B. Samsung, LG, ZTE, Huawei, Motorola, Apple usw.</td>
    </tr>
    <tr>
        <td>Anbieter</td>
        <td>Die verkaufende Firma/Organisation des Geräts.</td>
        <td></td>
        <td>z. B. Apple, Samsung, LG, Google usw.</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>Der Betriebssystemname des Geräts.</td>
        <td><i>erforderlich</i></td>
        <td>
            Die Werte sind begrenzt:
            <ul>
                <li>Android</li>
                <li>CHROME OS</li>
                <li>Linux</li>
                <li>MAC OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osFamily</td>
        <td>Der Gruppenname des Betriebssystems (Betriebssystem) des Geräts.</td>
        <td></td>
        <td>
            Die Werte sind begrenzt:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation OS</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVendor</td>
        <td>Der Betriebssystemanbieter des Geräts.</td>
        <td></td>
        <td>
            Die Werte sind begrenzt:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Tizen-Projekt</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVersion</td>
        <td>Betriebssystemversion des Geräts.</td>
        <td></td>
        <td>z. B. 10.2, 9.0.1 usw.</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>Der Name des Browsers.</td>
        <td></td>
        <td>
            Die Werte sind begrenzt:
            <ul>
                <li>Android-Browser</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Symbian-Browser</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVendor</td>
        <td>Das Bauunternehmen/die Organisation des Browsers.</td>
        <td></td>
        <td>
            Die Werte sind begrenzt:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVersion</td>
        <td>Die Browser-Version des Geräts.</td>
        <td></td>
        <td>Beispiel: 60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>Der Benutzeragent des Geräts.</td>
        <td></td>
        <td>Beispiel: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, wie Gecko) Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>Die physische Bildschirmbreite des Geräts.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>Die physische Bildschirmhöhe des Geräts.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>Die physische Bildschirmpixeldichte des Geräts.</td>
        <td></td>
        <td>Beispiel: 294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>Die physische Bildschirmdiagonale des Geräts in Zoll.</td>
        <td></td>
        <td>Beispiel: 5.5, 10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>Die zum Senden von HTTP-Anfragen verwendete IP des Geräts.</td>
        <td></td>
        <td>Beispiel: 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>Der Anschluss des Geräts, der zum Senden von HTTP-Anfragen verwendet wird.</td>
        <td></td>
        <td>Beispiel: 53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>Der Netzwerkverbindungstyp.</td>
        <td></td>
        <td>z. B. WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>Der Sicherheitsstatus der Netzwerkverbindung.</td>
        <td></td>
        <td>
            Die Werte sind begrenzt:
            <ul>
                <li>true - im Falle eines sicheren Netzwerks</li>
                <li>false - im Falle eines öffentlichen Hotspots</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>Die eindeutige Kennung der Anwendung.</td>
        <td></td>
        <td>z. B. CNN</td>
    </tr>
</table>


## Beispiele {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```
