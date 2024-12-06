---
title: Header - X-Device-Info
description: REST API V2 - Header - X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1085'
ht-degree: 2%

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
        <th style="background-color: #EFF2F7; width: 15%;">Präsenz</th>
        <th style="background-color: #EFF2F7; width: 15%;">Schlüssel</th>
        <th style="background-color: #EFF2F7;">Beschreibung</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Beschränkt</th>
        <th style="background-color: #EFF2F7;">Mögliche Werte</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwareType</td>
        <td>Der primäre Hardwaretyp des Geräts.</td>
        <td>&amp;check;</td>
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
        <td><i>erforderlich</i></td>
        <td>model</td>
        <td>Der Modellname des Geräts.</td>
        <td></td>
        <td>z. B. iPhone, SM-G930V, AppleTV usw.</td>
    </tr>
    <tr>
        <td><i>erforderlich</i></td>
        <td>version</td>
        <td>Die Version des Geräts.</td>
        <td></td>
        <td>z. B. 2.0.1 usw.</td>
    </tr>
    <tr>
        <td></td>
        <td>Hersteller</td>
        <td>Das Herstellungsunternehmen/die Organisation des Geräts.</td>
        <td></td>
        <td>z. B. Samsung, LG, ZTE, Huawei, Motorola, Apple usw.</td>
    </tr>
    <tr>
        <td></td>
        <td>Anbieter</td>
        <td>Die verkaufende Firma/Organisation des Geräts.</td>
        <td></td>
        <td>z. B. Apple, Samsung, LG, Google usw.</td>
    </tr>
    <tr>
        <td><i>erforderlich</i></td>
        <td>osName</td>
        <td>Der Betriebssystemname des Geräts.</td>
        <td>&amp;check;</td>
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
        <td></td>
        <td>osFamily</td>
        <td>Der Gruppenname des Betriebssystems (Betriebssystem) des Geräts.</td>
        <td>&amp;check;</td>
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
        <td></td>
        <td>osVendor</td>
        <td>Der Betriebssystemanbieter des Geräts.</td>
        <td>&amp;check;</td>
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
        <td><i>erforderlich</i></td>
        <td>osVersion</td>
        <td>Betriebssystemversion des Geräts.</td>
        <td></td>
        <td>z. B. 10.2, 9.0.1 usw.</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>Der Name des Browsers.</td>
        <td>&amp;check;</td>
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
        <td></td>
        <td>browserVendor</td>
        <td>Das Bauunternehmen/die Organisation des Browsers.</td>
        <td>&amp;check;</td>
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
        <td></td>
        <td>browserVersion</td>
        <td>Die Browser-Version des Geräts.</td>
        <td></td>
        <td>Beispiel: 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>Der Benutzeragent des Geräts.</td>
        <td></td>
        <td>Beispiel: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, wie Gecko) Version/10.0.3 Safari/602.4.8</td>
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
        <td>Die physische Bildschirmpixeldichte des Geräts.</td>
        <td></td>
        <td>Beispiel: 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>Die physische Bildschirmdiagonale des Geräts in Zoll.</td>
        <td></td>
        <td>Beispiel: 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>Die zum Senden von HTTP-Anfragen verwendete IP des Geräts.</td>
        <td></td>
        <td>Beispiel: 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>Der Anschluss des Geräts, der zum Senden von HTTP-Anfragen verwendet wird.</td>
        <td></td>
        <td>Beispiel: 53124</td>
    </tr>
    <tr>
        <td><i>erforderlich</i></td>
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
            Die Werte sind begrenzt:
            <ul>
                <li>true - im Falle eines sicheren Netzwerks</li>
                <li>false - im Falle eines öffentlichen Hotspots</li>
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
> Die Code-Snippets und Dokumentationsressourcen werden zu Verweiszwecken bereitgestellt.
> 
> Die Codefragmente sind nicht vollständig und erfordern möglicherweise zusätzliche Änderungen, um in Ihrem Projekt zu funktionieren.
>
> Unabhängig von Ihrer eigentlichen Implementierung muss die Kopfzeile `X-Device-Info` einen Wert enthalten, der wie im Abschnitt [Direktiven](#directives) beschrieben formatiert ist.

### Browser {#browsers}

Bei Clientanwendungen, die in einem Browser ausgeführt werden, kann der Header `X-Device-Info` weggelassen werden, da der Browser automatisch einen minimalen Satz erforderlicher Informationen in der Kopfzeile `User-Agent` sendet.

Sie können die Kopfzeile `X-Device-Info` weiterhin verwenden, um zusätzliche Informationen über das Gerät, die Verbindung und die Anwendung bereitzustellen, falls Ihre Client-Anwendung eine Bibliothek oder einen Dienst integriert, die bzw. der einen Geräteidentifizierungsmechanismus bereitstellt.

### Mobilgeräte {#mobile-devices}

#### iOS und iPadOS {#ios-ipados}

Um die Kopfzeile `X-Device-Info` für Geräte zu erstellen, auf denen [iOS oder iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes) ausgeführt wird, sehen Sie sich die folgenden Dokumente und den folgenden Codeausschnitt an:

* Apple-Entwicklerdokumentation für [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Apple-Entwicklerdokumentation für [Erreichbarkeit](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Linux-Handbuch für [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

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

Die Geräteinformationen können wie folgt konstruiert werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|------------------------|-----------------|
| model | uname.machine | iPhone |
| Anbieter | fest codiert | Apple |
| Hersteller | fest codiert | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Die Verbindungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Erreichbarkeit currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


Die Anwendungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | fest codiert | REF 30 |

#### Android {#android}

Um die Kopfzeile `X-Device-Info` für Geräte zu erstellen, auf denen [Android](https://developer.android.com/about/versions) ausgeführt wird, verweisen Sie möglicherweise auf die folgenden Dokumente und das folgende Codefragment:

* Android-Entwicklerdokumentation für die Klasse [Build](https://developer.android.com/reference/android/os/Build.html) .

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

Die Geräteinformationen können wie folgt konstruiert werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------------------------|-----------------|
| model | Build.MODEL | GT-I9505 |
| Anbieter | Build.BRAND | Samsung |
| Hersteller | Build.MANUFACTURER | Samsung |
| version | Build.DEVICE | jflte |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | fest codiert | Android |
| osVersion | Build.VERSION.RELEASE | 5,0,1 |

Die Verbindungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

Die Anwendungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | fest codiert | REF 30 |

### TV-vernetzte Geräte {#tv-connected-devices}

#### tvOS {#tvos}

Um die Kopfzeile `X-Device-Info` für Geräte zu erstellen, auf denen [tvOS](https://developer.apple.com/documentation/tvos-release-notes) ausgeführt wird, verweisen Sie möglicherweise auf die folgenden Dokumente und das folgende Codefragment:

* Apple-Entwicklerdokumentation für [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Apple-Entwicklerdokumentation für [Erreichbarkeit](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Linux-Handbuch für [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

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

Die Geräteinformationen können wie folgt konstruiert werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|------------------------|-----------------|
| model | uname.machine | AppleTV |
| Anbieter | fest codiert | Apple |
| Hersteller | fest codiert | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Die Verbindungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Erreichbarkeit currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

Die Anwendungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | fest codiert | REF 30 |

#### Fire OS {#fireos}

Um die Kopfzeile `X-Device-Info` für Geräte zu erstellen, auf denen [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) ausgeführt wird, lesen Sie die folgenden Dokumente:

* Android-Entwicklerdokumentation für die Klasse [Build](https://developer.android.com/reference/android/os/Build.html) .
* Amazon-Entwicklerdokumentation für [Identifizieren von Fire TV-Geräten](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html).

Die Geräteinformationen können wie folgt konstruiert werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------------------------|-----------------|
| model | Build.MODEL | AFTM |
| Anbieter | Build.BRAND | Amazon |
| Hersteller | Build.MANUFACTURER | Amazon |
| version | Build.DEVICE | Montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | fest codiert | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

Die Verbindungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

Die Anwendungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | fest codiert | REF 30 |

#### Roku OS {#rokuos}

Um die Kopfzeile `X-Device-Info` für Geräte zu erstellen, auf denen [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) ausgeführt wird, lesen Sie die folgenden Dokumente:

* Roku-Entwicklerdokumentation für [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md).

Die Geräteinformationen können wie folgt konstruiert werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|--------------------------------------------|-----------------|
| model | fest codiert | &quot;Roku&quot; |
| Anbieter | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| Hersteller | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | fest codiert | &quot;Roku&quot; |
| osVersion | ifDeviceInfo.getVersion() |                 |

Die Verbindungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | fest codiert | true , wenn die Verbindung verkabelt ist |

Die Anwendungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|---------------|-----------|-----------------|
| applicationId | fest codiert | REF 30 |
