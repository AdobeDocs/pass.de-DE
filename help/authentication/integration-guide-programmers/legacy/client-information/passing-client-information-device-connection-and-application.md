---
title: Übergeben von Client-Informationen (Gerät, Verbindung und Anwendung)
description: Übergeben von Client-Informationen (Gerät, Verbindung und Anwendung)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 1%

---

# (Legacy) Übergeben von Client-Informationen (Gerät, Verbindung und Anwendung) {#pass-client-info}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

## Umfang {#pass-client-info-scope}

In diesem Dokument werden Details und Arbeitsmappen zum Übergeben von Client-Informationen (Gerät, Verbindung und Anwendung) von einer Programmieranwendung an REST-APIs oder SDKs für die Adobe Pass-Authentifizierung zusammengefasst.

Die Bereitstellung von Kundeninformationen bietet folgende Vorteile:

* Die Möglichkeit, die Home Base Authentication (HBA) im Falle einiger Gerätetypen und MVPDs, die HBA unterstützen können, ordnungsgemäß zu aktivieren.
* Die Möglichkeit, bei einigen Gerätetypen TTLs ordnungsgemäß anzuwenden (konfigurieren Sie beispielsweise längere TTLs für Authentifizierungssitzungen auf mit Fernsehgeräten verbundenen Geräten).
* Die Möglichkeit, Geschäftsmetriken mithilfe der Berechtigungs-Service-Überwachung (ESM) in aufgeschlüsselten Berichten über verschiedene Gerätetypen hinweg ordnungsgemäß zu aggregieren.
* Entsperrt die Möglichkeit, verschiedene Geschäftsregeln ordnungsgemäß anzuwenden (z. B. Beeinträchtigung) bei bestimmten Gerätetypen.

## Überblick {#pass-client-info-overview}

Die Client-Informationen bestehen aus:

* **Gerät** Informationen über die Hardware- und Softwareattribute des Geräts, von dem aus der Benutzer versucht, den Programmiererinhalt zu nutzen.
* **Verbindung** Informationen zu den Verbindungsattributen des Geräts, von dem aus der Benutzer eine Verbindung zu Adobe Pass-Authentifizierungs- und/oder Programmierdiensten herstellt (z. B. Server-zu-Server-Implementierungen).
* **Application** Informationen über die registrierte Anwendung, von der aus der Benutzer versucht, den Programmiererinhalt zu nutzen.

Die Client-Informationen sind ein JSON-Objekt, das mit den in der folgenden Tabelle aufgeführten Schlüsseln erstellt wird.

>[!NOTE]
>
>Die folgenden **Schlüssel** sind **obligatorisch** im JSON-Objekt für Client-Informationen zu senden: **model**, **osName**.
>
>Die folgenden Schlüssel haben **eingeschränkte** Werte: `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Schlüssel | eingeschränkt | Beschreibung | Mögliche Werte |
|---|---|---|---|---|
|            | primaryHardwareType | # Ja | Der primäre Hardwaretyp des Geräts. | # Die Werte sind eingeschränkt:                                                                     Kamera                                                      DataCollectionTerminal                                                      Desktop                                                      EmbeddedNetworkModule                                                      Lesegerät                                                      Spielekonsole                                                      GeolocationTracker                                                      Brille                                                      MediaPlayer                                                      Mobiltelefon                                                      Zahlungsterminal                                                      Plug-inModem                                                      SetTopBox                                                      TV                                                      Tablet                                                      Wireless-Hotspot                                                      Armbanduhr                                                      Unbekannt |
| #mandatory | Modell | Nein | Der Modellname des Geräts. | z. B. iPhone, SM-G930V, AppleTV usw. |
|            | Version | Nein | Die Version des Geräts. | z. B. 2.0.1 usw. |
|            | Hersteller | Nein | Die Produktionsfirma/-organisation des Geräts. | z. B. Samsung, LG, ZTE, Huawei, Motorola, Apple usw. |
|            | Lieferant | Nein | Die Firma/Organisation, die das Gerät verkauft. | z. B. Apple, Samsung, LG, Google usw. |
| #mandatory | osName | # Ja | Der Name des Betriebssystems (OS) des Geräts. | # Die Werte sind eingeschränkt:                                                   Android                   CHROME OS                   Linux                   MAC OS                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | Betriebssystemfamilie | Ja | Der Gruppenname des Betriebssystems (OS) des Geräts. | # Die Werte sind eingeschränkt:                                                   Android                   BSD                   Linux                   PlayStation OS                   Roku OS                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | Betriebssystemanbieter | Nein | Der Lieferant des Betriebssystems (OS) des Geräts. | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   Nintendo                   Nokia                   Roku                   Samsung                   Sony                   Tizen-Projekt |
|            | osVersion | Nein | Die Betriebssystemversion des Geräts. | z. B. 10.2, 9.0.1 usw. |
|            | browserName | # Ja | Der Name des Browsers. | # Die Werte sind eingeschränkt:                                                   Android-Browser                   Chrome                   Edge                   Firefox                   Internet Explorer                   Oper                   Safari                   Salinenkrebs                   Symbian Browser |
|            | browserVendor | # Ja | Die Firma/Organisation, die den Browser erstellt. | # Die Werte sind eingeschränkt:                                                   Amazon                   Apple                   Google                   Microsoft                   Motorola                   Mozilla                   Netscape                   Nintendo                   Nokia                   Samsung                   Sony Ericsson |
|            | browserVersion | Nein | Die Browser-Version des Geräts. | z. B. 60.0.3112 |
|            | userAgent | Nein | Der Benutzeragent des Geräts. | z. B. Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, wie Gecko) Version/10.0.3 Safari/602.4.8 |
|            | displayWidth | Nein | Die physische Bildschirmbreite des Geräts. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | Nein | Die physische Bildschirmhöhe des Geräts. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | Nein | Die physische Pixeldichte auf dem Bildschirm des Geräts. | Beispiel: 294 |
|            | diagonalScreenSize | Nein | Die Bildschirmdiagonale des Geräts in Zoll. | z. B. 5.5, 10.1 |
|            | connectionIP | Nein | Die IP des Geräts, die zum Senden von HTTP-Anfragen verwendet wird. | z. B. 8.8.4.4 |
|            | connectionPort | Nein | Der Port des Geräts, der zum Senden von HTTP-Anfragen verwendet wird. | z. B. 53124 |
|            | connectionType | Nein | Der Netzwerkverbindungstyp. | z. B. WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Ja | Der Sicherheitsstatus der Netzwerkverbindung. | # Die Werte sind eingeschränkt:                                                   true - bei einem sicheren Netzwerk                   false : Im Fall eines öffentlichen Hotspots |
|            | applicationId | Nein | Die eindeutige Kennung der Anwendung. | Beispiel: CNN |

## API-Referenzen {#api-ref}

In diesem Abschnitt wird die API vorgestellt, die für die Verarbeitung von Client-Informationen bei der Verwendung von Adobe Pass Authentication REST-APIs oder SDKs verantwortlich ist.

### REST-API {#rest-api}

Adobe Pass-Authentifizierungs-Services unterstützen den Empfang der Client-Informationen wie folgt:

* as a **header: „X-Device-Info“**
* Als **Abfrageparameter: „device_info“**
* Als **post-Parameter: „device_info“**

>[!IMPORTANT]
>
>In allen drei Szenarien muss die Payload der Kopfzeile oder des Parameters (Base64 **-codiert und URL-codiert)**.

**SDK**

#### JavaScript SDK {#js-sdk}

AccessEnabler JavaScript SDK erstellt standardmäßig ein JSON-Objekt mit Client-Informationen, das an die Adobe Pass-Authentifizierungsdienste übergeben wird, sofern es nicht überschrieben wird.

AccessEnabler JavaScript SDK unterstützt **nur Überschreiben** des Schlüssels „applicationId“ aus dem JSON-Objekt für Client-Informationen über den [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options))-Optionsparameter *applicationId*.

>[!CAUTION]
>
>Der `applicationId` Parameterwert muss ein Nur-Text-Zeichenfolgenwert sein.
>Falls sich die Programmieranwendung für die Übergabe der applicationId entscheidet, werden die übrigen Client-Informationsschlüssel weiterhin vom AccessEnabler JavaScript SDK berechnet.

#### iOS/tvOS SDK {#ios-tvos-sdk}

Die AccessEnabler iOS/tvOS SDK erstellt standardmäßig ein Client-JSON-Objekt mit Informationen, das an die Adobe Pass-Authentifizierungsdienste übergeben wird, sofern es nicht überschrieben wird.

Die AccessEnabler iOS/tvOS SDK unterstützt **Überschreiben des gesamten** Client-JSON-Objekts über den Parameter [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions) „device_info“.

>[!CAUTION]
>
>Der *device_info* Parameterwert muss ein **Base64-kodierter** *NSString* Wert sein.
>
>Falls die Programmieranwendung beschließt, die *device_info* zu übergeben, werden alle von AccessEnabler iOS/tvOS SDK berechneten Client-Informationsschlüssel überschrieben. Daher ist es sehr wichtig, die Werte für so viele Schlüssel wie möglich zu berechnen und zu übergeben. Weitere Informationen zur Implementierung finden Sie in der Tabelle [Übersicht](#pass-client-info-overview) und im [iOS/tvOS-Cookbook](#ios-tvos).

#### Android/FireOS SDK {#and-fire-os-sdk}

Die `AccessEnabler` Android/FireOS-SDK erstellt standardmäßig ein JSON-Objekt mit Client-Informationen, das an die Adobe Pass-Authentifizierungsdienste übergeben wird, sofern es nicht überschrieben wird.

Die `AccessEnabler` Android/FireOS-SDK unterstützt **Überschreiben des gesamten** Client-Informations-JSON-Objekts durch den [-Parameter ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions)setOptions[s/](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption)setOptions`device_info`.

>[!NOTE]
>
>Der `device_info` Parameterwert muss ein **Base64-kodierter)** sein.

>[!IMPORTANT]
>
>Falls die Programmieranwendung beschließt, die `device_info` zu übergeben, werden alle von der `AccessEnabler` Android/FireOS-SDK berechneten Client-Informationsschlüssel überschrieben. Daher ist es sehr wichtig, die Werte für so viele Schlüssel wie möglich zu berechnen und zu übergeben. Weitere Informationen zur Implementierung finden Sie in der Tabelle [Übersicht](#pass-client-info-overview) und im Cookbook [Android](#android) und [FireOS](#fire-tv).

## Cookbooks {#cookbooks}

In diesem Abschnitt wird ein Cookbook zum Erstellen des JSON-Objekts mit Client-Informationen für verschiedene Gerätetypen vorgestellt.

>[!IMPORTANT]
>
>Die mit **markierten Tasten!** müssen gesendet werden.

### Android {#android}

Die Geräteinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------------------------|---------------|
| ! | Modell | Build.MODEL | GT-I9505 |
|   | Lieferant | Build.BRAND | Samsung |
|   | Hersteller | Build.MANUFACTURER | Samsung |
| ! | Version | Build.DEVICE | Kehllappen |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | hartcodiert | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5,0,1 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

Die Anwendungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------|--------------|
|   | applicationId | hartcodiert | CNN |

>[!IMPORTANT]
>
>Die Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt (Base64 **-kodiert)**. Bei REST-APIs für die Adobe Pass-Authentifizierung muss der Wert außerdem **URL-codiert** sein.

**Beispielcode**

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
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
>**Ressourcen:**
>* public class [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} in der Dokumentation von Java-Entwicklern.

### FireTV {#fire-tv}

Die Geräteinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (z. B. ) |
|---|---------------|-----------------------------|--------------|
| ! | Modell | Build.MODEL | AFTM |
|   | Lieferant | Build.BRAND | Amazon |
|   | Hersteller | Build.MANUFACTURER | Amazon |
| ! | Version | Build.DEVICE | Montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | hartcodiert | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

Die Anwendungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------|--------------|
|   | applicationId | hartcodiert | CNN |

>[!IMPORTANT]
>
>Die Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt (Base64 **-kodiert)**. Bei REST-APIs für die Adobe Pass-Authentifizierung muss der Wert außerdem **URL-codiert** sein.

>[!NOTE]
>
>**Ressourcen:**
>* public class [Build](https://developer.android.com/reference/android/os/Build.html){target=_blank} in der Dokumentation von Android Developers.
>* [Identifizieren von FireTV-Geräten](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

Die Geräteinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|------------------------|--------------|
| ! | Modell | uname.machine | iPhone |
|   | Lieferant | hartcodiert | Apple |
|   | Hersteller | hartcodiert | Apple |
| ! | Version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10,2 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Erreichbarkeit currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


Die Anwendungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------|--------------|
|   | applicationId | hartcodiert | CNN |

>[!IMPORTANT]
>
>Die Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt mit Base64 codiert sein. Außerdem muss bei REST-APIs für die Adobe Pass-Authentifizierung der Wert URL-kodiert sein.

**Beispielcode**

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
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
>**Ressourcen:**
>* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
>* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
>* [Über die Erreichbarkeit](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

Die Geräteinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | Modell | hartcodiert | „Roku“ |
|     | Lieferant | ifDeviceInfo.GetModelDetails().VendorName | „Scharf“, „Roku“ |
|     | Hersteller | ifDeviceInfo.GetModelDetails().VendorName | „Scharf“, „Roku“ |
| ! | Version | ifDeviceInfo.GetModelDetails().ModelNumber | „5303X“ |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | hartcodiert | „Roku“ |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | „WifiConnection“, „WiredConnection“ |
|   | connectionSecure | hartcodiert | Wahr, wenn die Verbindung verkabelt ist |

Die Anwendungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------|--------------|
|   | applicationId | hartcodiert | CNN |

>[!IMPORTANT]
>
>Die Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt (Base64 **-kodiert)**. Außerdem muss bei REST-APIs für die Adobe Pass-Authentifizierung der Wert URL-kodiert sein.

>[!NOTE]
>
>Weitere Informationen finden Sie unter [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

Die Geräteinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---|---|---|
| ! | Modell | EasClientDeviceInformation.SystemProductName |                 |
|   | Lieferant | hartcodiert | Microsoft |
|   | Hersteller | hartcodiert | Microsoft |
| ! | Version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

Die Verbindungsinformationen können wie folgt aufgebaut werden:

|   | Schlüssel | Source | Beispiel |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | „None“, „WPA“ usw |

Die Anwendungsinformationen können wie folgt aufgebaut werden:

| Schlüssel | Source | Wert (Beispiel) |
|---|---|---|
| applicationId | hartcodiert | CNN |

>[!IMPORTANT]
>
>Die Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt (Base64 **-kodiert)**. Bei REST-APIs für die Adobe Pass-Authentifizierung muss der Wert außerdem **URL-codiert** sein.

**Ressourcen**

* [EasClientDeviceInformation-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [DisplayInformation-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
