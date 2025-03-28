---
title: Kopfzeile - x-device-info
description: REST API v2 - Kopfzeile - x-device-info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 42df16e34783807e1b5eb1a12ca9db92f4e4c161
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 2%

---

# Kopfzeile - x-device-info {#header-x-device-info}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#overview}

Der <b>X-Device-Info</b>-Anfrage-Header enthält die Client-Informationen (Gerät, Verbindung und Anwendung), die sich auf das eigentliche Streaming-Gerät beziehen, und wird verwendet, um plattformspezifische Regeln zu bestimmen, die von MVPDs erzwungen werden können.

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;device_information&gt;</td>
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

<b>&lt;device_information></b>

Der `Base64-encoded` des JSON-Elements, das mindestens die Attribute enthält, die in der folgenden Tabelle als erforderlich markiert sind.

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Präsenz</th>
        <th style="background-color: #EFF2F7; width: 15%;">Schlüssel</th>
        <th style="background-color: #EFF2F7;">Beschreibung</th>    
        <th style="background-color: #EFF2F7; width: 15%;">eingeschränkt</th>
        <th style="background-color: #EFF2F7;">Mögliche Werte</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwareType</td>
        <td>Der primäre Hardwaretyp des Geräts.</td>
        <td>&amp;check;</td>
        <td>
            Die Werte sind eingeschränkt:
            <ul>
                <li>Kamera</li>
                <li>DataCollectionTerminal</li>
                <li>Desktop</li>
                <li>EmbeddedNetworkModule</li>
                <li>Lesegerät</li>
                <li>Spielekonsole</li>
                <li>GeolocationTracker</li>
                <li>Brille</li>
                <li>MediaPlayer</li>
                <li>Mobiltelefon</li>
                <li>Zahlungsterminal</li>
                <li>Plug-inModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tablet</li>
                <li>Wireless-Hotspot</li>
                <li>Armbanduhr</li>
                <li>Unbekannt</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>required</i></td>
        <td>Modell</td>
        <td>Der Modellname des Geräts.</td>
        <td></td>
        <td>z. B. iPhone, SM-G930V, AppleTV usw.</td>
    </tr>
    <tr>
        <td><i>required</i></td>
        <td>Version</td>
        <td>Die Version des Geräts.</td>
        <td></td>
        <td>z. B. 2.0.1 usw.</td>
    </tr>
    <tr>
        <td></td>
        <td>Hersteller</td>
        <td>Die Produktionsfirma/-organisation des Geräts.</td>
        <td></td>
        <td>z. B. Samsung, LG, ZTE, Huawei, Motorola, Apple usw.</td>
    </tr>
    <tr>
        <td></td>
        <td>Lieferant</td>
        <td>Die verkaufende Firma/Organisation des Geräts.</td>
        <td></td>
        <td>z. B. Apple, Samsung, LG, Google usw.</td>
    </tr>
    <tr>
        <td><i>required</i></td>
        <td>osName</td>
        <td>Der Name des Betriebssystems (OS) des Geräts.</td>
        <td>&amp;check;</td>
        <td>
            Die Werte sind eingeschränkt:
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
        <td></td>
        <td>Betriebssystemfamilie</td>
        <td>Der Gruppenname des Betriebssystems (OS) des Geräts.</td>
        <td>&amp;check;</td>
        <td>
            Die Werte sind eingeschränkt:
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
        <td></td>
        <td>Betriebssystemanbieter</td>
        <td>Der Anbieter des Betriebssystems (OS) des Geräts.</td>
        <td>&amp;check;</td>
        <td>
            Die Werte sind eingeschränkt:
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
        <td><i>required</i></td>
        <td>osVersion</td>
        <td>Die Betriebssystemversion des Geräts.</td>
        <td></td>
        <td>z. B. 10.2, 9.0.1 usw.</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>Der Name des Browsers.</td>
        <td>&amp;check;</td>
        <td>
            Die Werte sind eingeschränkt:
            <ul>
                <li>Android-Browser</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Oper</li>
                <li>Safari</li>
                <li>Salinenkrebs</li>
                <li>Symbian Browser</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>Die Firma/Organisation, die den Browser erstellt.</td>
        <td>&amp;check;</td>
        <td>
            Die Werte sind eingeschränkt:
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
        <td></td>
        <td>browserVersion</td>
        <td>Die Browser-Version des Geräts.</td>
        <td></td>
        <td>z. B. 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>Der Benutzeragent des Geräts.</td>
        <td></td>
        <td>z. B. Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, wie Gecko) Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>Die physische Bildschirmbreite des Geräts.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>Die physische Bildschirmhöhe des Geräts.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>Die physische Pixeldichte des Bildschirms des Geräts.</td>
        <td></td>
        <td>Beispiel: 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>Die Bildschirmdiagonale des Geräts in Zoll.</td>
        <td></td>
        <td>z. B. 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIP</td>
        <td>Die IP des Geräts, die zum Senden von HTTP-Anfragen verwendet wird.</td>
        <td></td>
        <td>z. B. 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>Der Port des Geräts, der zum Senden von HTTP-Anfragen verwendet wird.</td>
        <td></td>
        <td>z. B. 53124</td>
    </tr>
    <tr>
        <td><i>required</i></td>
        <td>connectionType</td>
        <td>Der Netzwerkverbindungstyp.</td>
        <td></td>
        <td>z. B. WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>Der Sicherheitsstatus der Netzwerkverbindung.</td>
        <td>&amp;check;</td>
        <td>
            Die Werte sind eingeschränkt:
            <ul>
                <li>true - bei einem sicheren Netzwerk</li>
                <li>false : Im Fall eines öffentlichen Hotspots</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>Die eindeutige Kennung der Anwendung.</td>
        <td></td>
        <td>z. B. REF30</td>
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

## Cookbooks {#cookbooks}

>[!IMPORTANT]
> 
> Die Code-Snippets und Dokumentationsressourcen werden zu Referenzierungszwecken bereitgestellt.
> 
> Die Codeausschnitte sind nicht vollständig und erfordern möglicherweise zusätzliche Änderungen, um in Ihrem Projekt zu funktionieren.
>
> Unabhängig von Ihrer tatsächlichen Implementierung muss die `X-Device-Info`-Kopfzeile einen Wert enthalten, der wie im Abschnitt [Anweisungen](#directives) beschrieben formatiert ist.

### Browser {#browsers}

Bei Client-Anwendungen, die in einem Browser ausgeführt werden, kann die `X-Device-Info`-Kopfzeile weggelassen werden, da der Browser automatisch einen minimalen Satz der erforderlichen Informationen in der `User-Agent`-Kopfzeile sendet.

Sie können den `X-Device-Info`-Header weiterhin verwenden, um zusätzliche Informationen über das Gerät, die Verbindung und das Programm bereitzustellen, falls Ihre Client-Anwendung eine Bibliothek oder einen Service integriert, die bzw. der einen Mechanismus zur Geräteerkennung bereitstellt.

### Mobilgeräte {#mobile-devices}

#### iOS und iPadOS {#ios-ipados}

Um den `X-Device-Info`-Header für Geräte zu erstellen, auf denen [iOS oder iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes) ausgeführt wird, können Sie die folgenden Dokumente und das folgende Codefragment lesen:

* Apple-Entwicklerdokumentation für [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Apple-Entwicklerdokumentation für [Erreichbarkeit](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Linux-Dokumentation für [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

Die Geräteinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|------------------------|-----------------|
| Modell | uname.machine | iPhone |
| Lieferant | hartcodiert | Apple |
| Hersteller | hartcodiert | Apple |
| Version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Erreichbarkeit currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


Die Anwendungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | hartcodiert | REF30 |

#### Android {#android}

Um den `X-Device-Info`-Header für Geräte zu erstellen, auf denen [Android](https://developer.android.com/about/versions) ausgeführt wird, können Sie die folgenden Dokumente und das folgende Codefragment lesen:

* Android-Entwicklerdokumentation für [Build](https://developer.android.com/reference/android/os/Build.html)-Klasse.

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

Die Geräteinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------------------------|-----------------|
| Modell | Build.MODEL | GT-I9505 |
| Lieferant | Build.BRAND | Samsung |
| Hersteller | Build.MANUFACTURER | Samsung |
| Version | Build.DEVICE | Kehllappen |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | hartcodiert | Android |
| osVersion | Build.VERSION.RELEASE | 5,0,1 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

Die Anwendungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | hartcodiert | REF30 |

### TV-Geräte angeschlossen {#tv-connected-devices}

#### tvOS {#tvos}

Um den `X-Device-Info`-Header für Geräte zu erstellen, auf denen [tvOS](https://developer.apple.com/documentation/tvos-release-notes) ausgeführt wird, lesen Sie die folgenden Dokumente und das folgende Codefragment:

* Apple-Entwicklerdokumentation für [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Apple-Entwicklerdokumentation für [Erreichbarkeit](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Linux-Dokumentation für [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

Die Geräteinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|------------------------|-----------------|
| Modell | uname.machine | AppleTV |
| Lieferant | hartcodiert | Apple |
| Hersteller | hartcodiert | Apple |
| Version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Erreichbarkeit currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

Die Anwendungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | hartcodiert | REF30 |

#### Betriebssystem auslösen {#fireos}

Informationen zum Erstellen des `X-Device-Info`-Headers für Geräte, auf [ „Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) ausgeführt wird, finden Sie in den folgenden Dokumenten:

* Android-Entwicklerdokumentation für [Build](https://developer.android.com/reference/android/os/Build.html)-Klasse.
* Amazon-Entwicklerdokumentation für [Identifizieren von Fire-TV-](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html))

Die Geräteinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------------------------|-----------------|
| Modell | Build.MODEL | AFTM |
| Lieferant | Build.BRAND | Amazon |
| Hersteller | Build.MANUFACTURER | Amazon |
| Version | Build.DEVICE | Montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | hartcodiert | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

Die Anwendungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | hartcodiert | REF30 |

#### Roku OS {#rokuos}

Informationen zum Erstellen des `X-Device-Info`-Headers für Geräte, auf [ (Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) ausgeführt wird, finden Sie in den folgenden Dokumenten:

* Roku-Entwicklerdokumentation für [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md).

Die Geräteinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|--------------------------------------------|-----------------|
| Modell | hartcodiert | „Roku“ |
| Lieferant | ifDeviceInfo.GetModelDetails().VendorName | „Scharf“, „Roku“ |
| Hersteller | ifDeviceInfo.GetModelDetails().VendorName | „Scharf“, „Roku“ |
| Version | ifDeviceInfo.GetModelDetails().ModelNumber | „5303X“ |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | hartcodiert | „Roku“ |
| osVersion | ifDeviceInfo.getVersion() |                 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | „WifiConnection“, „WiredConnection“ |
| connectionSecure | hartcodiert | Wahr, wenn die Verbindung verkabelt ist |

Die Anwendungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | hartcodiert | REF30 |

### Sonstige {#others}

Für Geräteplattformen, die nicht in der Dokumentation behandelt werden, sollten die Client-Informationen (Gerät, Verbindung und Anwendung) mit allen verfügbaren Hardware- und Betriebssystemattributen verknüpft werden, die normalerweise in den Hardware- und Betriebssystemhandbüchern des Geräts angegeben sind.
