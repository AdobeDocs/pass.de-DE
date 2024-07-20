---
title: Weitergeben von Client-Informationen (Gerät, Verbindung und Anwendung)
description: Weitergeben von Client-Informationen (Gerät, Verbindung und Anwendung)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1643'
ht-degree: 1%

---

# Weitergeben von Client-Informationen (Gerät, Verbindung und Anwendung) {#pass-client-info}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Anwendungsbereich {#pass-client-info-scope}

In diesem Dokument werden Details und Cookies zur Weitergabe von Client-Informationen (Gerät, Verbindung und Anwendung) von einer Programmierungsanwendung an Adobe Pass Authentication REST APIs oder SDKs zusammengefasst.

Die Bereitstellung von Kundeninformationen bietet folgende Vorteile:

* Die Fähigkeit, die Home Base Authentication (HBA) ordnungsgemäß zu aktivieren, wenn es sich um einige Gerätetypen und MVPDs handelt, die HBA unterstützen können.
* Die Möglichkeit, TTLs bei bestimmten Gerätetypen ordnungsgemäß anzuwenden (z. B. längere TTLs für Authentifizierungssitzungen auf TV-verbundenen Geräten konfigurieren).
* Die Fähigkeit, Geschäftsmetriken mithilfe der Entitlement Service Monitoring (ESM) ordnungsgemäß in aufgeschlüsselten Berichten über Gerätetypen zu aggregieren.
* Hebt die Fähigkeit auf, verschiedene Geschäftsregeln (z. B. Abbau) bei bestimmten Gerätetypen.

## Übersicht {#pass-client-info-overview}

Die Client-Informationen bestehen aus:

* **Gerät** Informationen zu den Hardware- und Softwareattributen des Geräts, von dem aus der Benutzer versucht, den Programminhalt zu verwenden.
* **Verbindung** Informationen über die Verbindungsattribute des Geräts, von dem aus der Benutzer eine Verbindung zu Adobe Pass-Authentifizierungsdiensten und/oder Programmiererdiensten herstellt (z. B. Server-zu-Server-Implementierungen).
* **Anwendung** Informationen zur registrierten Anwendung, von der aus der Benutzer versucht, den Programminhalt zu verwenden.

Die Client-Informationen sind ein JSON-Objekt, das mit Schlüsseln erstellt wurde, die in der folgenden Tabelle dargestellt sind.

>[!NOTE]
>
>Die folgenden **Schlüssel** sind **obligatorisch**, die im JSON-Objekt mit den Client-Informationen gesendet werden: **model**, **osName**.
>
>Die folgenden Schlüssel haben **restricted** -Werte: `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Schlüssel | Beschränkt | Beschreibung | Mögliche Werte |
|---|---|---|---|---|
|            | primaryHardwareType | # Yes | Der primäre Hardwaretyp des Geräts. | # Die Werte sind eingeschränkt:                                                                     Kamera                                                      DataCollectionTerminal                                                      Desktop                                                      EmbeddedNetworkModule                                                      eReader                                                      GamesConsole                                                      GeolocationTracker                                                      Brille                                                      MediaPlayer                                                      MobilePhone                                                      PaymentTerminal                                                      PluginModem                                                      SetTopBox                                                      TV                                                      Tablette                                                      WirelessHotspot                                                      Wristwatch                                                      unbekannt |
| #mandatory | model | Nein | Der Modellname des Geräts. | z. B. iPhone, SM-G930V, AppleTV usw. |
|            | version | Nein | Die Version des Geräts. | z. B. 2.0.1 usw. |
|            | Hersteller | Nein | Das Herstellungsunternehmen/die Organisation des Geräts. | z. B. Samsung, LG, ZTE, Huawei, Motorola, Apple usw. |
|            | Anbieter | Nein | Die verkaufende Firma/Organisation des Geräts. | z. B. Apple, Samsung, LG, Google usw. |
| #mandatory | osName | # Yes | Der Betriebssystemname des Geräts. | # Die Werte sind eingeschränkt:                                                   Android                   CHROME OS                   Linux                   MAC OS                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | Ja | Der Gruppenname des Betriebssystems des Geräts. | # Die Werte sind eingeschränkt:                                                   Android                   BSD                   Linux                   PlayStation OS                   Roku OS                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | Nein | Der Betriebssystemanbieter des Geräts. | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   Nintendo                   Nokia                   Roku                   Samsung                   Sony                   Tizen-Projekt |
|            | osVersion | Nein | Betriebssystemversion des Geräts. | z. B. 10.2, 9.0.1 usw. |
|            | browserName | # Yes | Der Name des Browsers. | # Die Werte sind eingeschränkt:                                                   Android-Browser                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   SeaMonkey                   Symbian-Browser |
|            | browserVendor | # Yes | Das Bauunternehmen/die Organisation des Browsers. | # Die Werte sind eingeschränkt:                                                   Amazon                   Apple                   Google                   Microsoft                   Motorola                   Mozilla                   Netscape                   Nintendo                   Nokia                   Samsung                   Sony Ericsson |
|            | browserVersion | Nein | Die Browser-Version des Geräts. | Beispiel: 60.0.3112 |
|            | userAgent | Nein | Der Benutzeragent des Geräts. | Beispiel: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, wie Gecko) Version/10.0.3 Safari/602.4.8 |
|            | displayWidth | Nein | Die physische Bildschirmbreite des Geräts. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | Nein | Die physische Bildschirmhöhe des Geräts. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | Nein | Die physische Bildschirmpixeldichte des Geräts. | Beispiel: 294 |
|            | diagonalScreenSize | Nein | Die physische Bildschirmdiagonale des Geräts in Zoll. | Beispiel: 5.5, 10.1 |
|            | connectionIp | Nein | Die zum Senden von HTTP-Anfragen verwendete IP des Geräts. | Beispiel: 8.8.4.4 |
|            | connectionPort | Nein | Der Anschluss des Geräts, der zum Senden von HTTP-Anfragen verwendet wird. | Beispiel: 53124 |
|            | connectionType | Nein | Der Netzwerkverbindungstyp. | z. B. WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Yes | Der Sicherheitsstatus der Netzwerkverbindung. | # Die Werte sind eingeschränkt:                                                   true - im Falle eines sicheren Netzwerks                   false - im Falle eines öffentlichen Hotspots |
|            | applicationId | Nein | Die eindeutige Kennung der Anwendung. | z. B. CNN |

## API-Referenzen {#api-ref}

In diesem Abschnitt wird die API vorgestellt, die für die Verarbeitung von Client-Informationen bei der Verwendung von Adobe Pass Authentication REST APIs oder SDKs zuständig ist.

### REST-API {#rest-api}

Adobe Pass-Authentifizierungsdienste unterstützen den Empfang von Client-Informationen wie folgt:

* Als **Header: &quot;X-Device-Info&quot;**
* Als **Abfrageparameter: &quot;device_info&quot;**
* Als **Post-Parameter: &quot;device_info&quot;**

>[!IMPORTANT]
>
>In allen drei Szenarien muss die Payload der Kopfzeile oder des Parameters **Base64-kodiert und URL-kodiert** sein.

**SDK**

#### JAVASCRIPT SDK {#js-sdk}

Das AccessEnabler JavaScript SDK erstellt standardmäßig ein JSON-Objekt mit Client-Informationen, das an Adobe Pass-Authentifizierungsdienste übergeben wird, sofern es nicht überschrieben wird.

Das AccessEnabler JavaScript SDK unterstützt **nur das Überschreiben des Schlüssels &quot;applicationId&quot;aus dem JSON-Objekt für Client-Informationen über den Optionsparameter [setRequestor](/help/authentication/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options))&#39; *applicationId* .**

>[!CAUTION]
>
>Der Parameterwert `applicationId` muss ein Nur-Text-String-Wert sein.
>Falls die Programmeranwendung beschließt, die applicationId zu übergeben, werden die restlichen Client-Informationsschlüssel weiterhin vom AccessEnabler JavaScript SDK berechnet.

#### iOS/tvOS-SDK {#ios-tvos-sdk}

Das AccessEnabler iOS/tvOS SDK erstellt standardmäßig ein JSON-Objekt mit Client-Informationen, das an Adobe Pass-Authentifizierungsdienste übergeben wird, sofern es nicht überschrieben wird.

Das AccessEnabler iOS/tvOS-SDK unterstützt **beim Überschreiben des gesamten JSON-Objekts für Client-Informationen über den Parameter device_info des [setOptions](/help/authentication/iostvos-sdk-api-reference.md#setoptions).**

>[!CAUTION]
>
>Der Parameterwert *device_info* muss ein **Base64-kodierter** *NSString* -Wert sein.
>
>Falls die Programmeranwendung beschließt, die *device_info* zu übergeben, werden alle vom AccessEnabler iOS/tvOS SDK berechneten Client-Informationsschlüssel überschrieben. Daher ist es sehr wichtig, die Werte für so viele Schlüssel wie möglich zu berechnen und weiterzugeben. Weitere Informationen zur Implementierung finden Sie in der Tabelle [Überblick](#pass-client-info-overview) und im Cookbook [iOS/tvOS](#ios-tvos).

#### Android/FireOS-SDK {#and-fire-os-sdk}

Das SDK `AccessEnabler` Android/FireOS erstellt standardmäßig ein JSON-Objekt mit Client-Informationen, das an Adobe Pass-Authentifizierungsdienste übergeben wird, sofern es nicht überschrieben wird.

Das SDK `AccessEnabler` Android/FireOS unterstützt **das Überschreiben des gesamten JSON-Objekts für Client-Informationen über den Parameter [setOptions](/help/authentication/android-sdk-api-reference.md#setOptions)/[setOptions](/help/authentication/amazon-fireos-native-client-api-reference.md#fire_setOption)&#39; `device_info` .**

>[!NOTE]
>
>Der Parameterwert `device_info` muss ein **Base64-kodierter** String -Wert sein.

>[!IMPORTANT]
>
>Falls die Programmiereranwendung beschließt, den `device_info` zu übergeben, werden alle vom `AccessEnabler` Android/FireOS SDK berechneten Client-Informationsschlüssel überschrieben. Daher ist es sehr wichtig, die Werte für so viele Schlüssel wie möglich zu berechnen und weiterzugeben. Weitere Informationen zur Implementierung finden Sie in der Tabelle [Überblick](#pass-client-info-overview) und im Cookbook [Android](#android) und [FireOS](#fire-tv) .

## Cookbooks {#cookbooks}

In diesem Abschnitt wird ein Cookbook zum Erstellen des JSON-Objekts für Client-Informationen bei verschiedenen Gerätetypen vorgestellt.

>[!IMPORTANT]
>
>Die Schlüssel, die mit **markiert sind!** ist erforderlich, um gesendet zu werden.

### Android {#android}

Die Geräteinformationen können wie folgt konstruiert werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------------------------|---------------|
| ! | model | Build.MODEL | GT-I9505 |
|   | Anbieter | Build.BRAND | Samsung |
|   | Hersteller | Build.MANUFACTURER | Samsung |
| ! | version | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | fest codiert | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5,0,1 |

Die Verbindungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

Die Anwendungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------|--------------|
|   | applicationId | fest codiert | CNN |

>[!IMPORTANT]
>
Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt **Base64-kodiert** sein. Außerdem muss der Wert bei Adobe Pass Authentication REST APIs **URL-codiert** sein.

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
**Ressourcen:**
* public class [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} in der Dokumentation der Java-Entwickler.

### FireTV {#fire-tv}

Die Geräteinformationen können wie folgt konstruiert werden:

|   | Schlüssel | Source | Wert (beispielsweise) |
|---|---------------|-----------------------------|--------------|
| ! | model | Build.MODEL | AFTM |
|   | Anbieter | Build.BRAND | Amazon |
|   | Hersteller | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | Montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | fest codiert | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

Die Verbindungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

Die Anwendungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------|--------------|
|   | applicationId | fest codiert | CNN |

>[!IMPORTANT]
>
Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt **Base64-kodiert** sein. Außerdem muss der Wert bei Adobe Pass Authentication REST APIs **URL-codiert** sein.

>[!NOTE]
>
**Ressourcen:**
* öffentliche Klasse [Build](https://developer.android.com/reference/android/os/Build.html){target=_blank} in der Dokumentation der Android-Entwickler.
* [Identifizieren von FireTV-Geräten](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

Die Geräteinformationen können wie folgt konstruiert werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|------------------------|--------------|
| ! | model | uname.machine | iPhone |
|   | Anbieter | fest codiert | Apple |
|   | Hersteller | fest codiert | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10,2 |

Die Verbindungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Erreichbarkeit currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


Die Anwendungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------|--------------|
|   | applicationId | fest codiert | CNN |

>[!IMPORTANT]
>
Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Anschließend muss das resultierende Objekt Base64-kodiert sein. Außerdem muss der Wert bei Adobe Pass Authentication REST APIs URL-codiert sein.

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
**Ressourcen:**
* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
* [Über die Erreichbarkeit](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

Die Geräteinformationen können wie folgt konstruiert werden:

| Schlüssel | Source | Wert (Beispiel) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | model | fest codiert | &quot;Roku&quot; |
|     | Anbieter | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
|     | Hersteller | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| ! | version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | fest codiert | &quot;Roku&quot; |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

Die Verbindungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | fest codiert | true , wenn die Verbindung verkabelt ist |

Die Anwendungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---------------|-----------|--------------|
|   | applicationId | fest codiert | CNN |

>[!IMPORTANT]
>
Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt **Base64-kodiert** sein. Außerdem muss der Wert bei Adobe Pass Authentication REST APIs URL-codiert sein.

>[!NOTE]
>
Weitere Informationen finden Sie unter [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

Die Geräteinformationen können wie folgt konstruiert werden:

|   | Schlüssel | Source | Wert (Beispiel) |
|---|---|---|---|
| ! | model | EasClientDeviceInformation.SystemProductName |                 |
|   | Anbieter | fest codiert | Microsoft |
|   | Hersteller | fest codiert | Microsoft |
| ! | version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

Die Verbindungsinformationen können wie folgt erstellt werden:

|   | Schlüssel | Source | Beispiel |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | &quot;None&quot;, &quot;Wpa&quot;usw. |

Die Anwendungsinformationen können wie folgt erstellt werden:

| Schlüssel | Source | Wert (Beispiel) |
|---|---|---|
| applicationId | fest codiert | CNN |

>[!IMPORTANT]
>
Geräte-, Verbindungs- und Anwendungsinformationen müssen demselben JSON-Objekt hinzugefügt werden. Danach muss das resultierende Objekt **Base64-kodiert** sein. Außerdem muss der Wert bei Adobe Pass Authentication REST APIs **URL-codiert** sein.

**Ressourcen**

* [EasClientDeviceInformation-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [DisplayInformation-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
