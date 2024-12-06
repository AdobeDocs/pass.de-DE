---
title: Charles Proxy verwenden
description: Charles Proxy verwenden
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Charles Proxy verwenden {#using-charles-proxy}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


**Charles:** <http://charlesproxy.com>


## Herunterladen, Installieren und Erste Schritte mit Charles Proxy {#download-install-and-get-stared-with-charles-proxy}

- **Download** - <http://www.charlesproxy.com/download/>
- **Install** - <http://www.charlesproxy.com/documentation/installation/>
- **Erste Schritte** - <http://www.charlesproxy.com/documentation/getting-started/>


## Struktur- und Sequenzregisterkarten {#structure-vs-sequence-tabs}

Es gibt zwei verschiedene Möglichkeiten, den Traffic anzuzeigen:

1. **Struktur** - Anforderungen werden nach Host gruppiert
1. **Sequenz** - Anforderungen werden in der Reihenfolge aufgelistet, in der sie aufgerufen werden


## SSL und Zertifikate {#ssl-and-certificates}

Aktivieren der SSL-Proxying `\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

Aktivieren Sie das Kontrollkästchen &quot;SSL-Proxying aktivieren&quot;und fügen Sie alle HTTPS-Speicherorte hinzu.


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)



- SSL-Proxy - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL-Zertifikate - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- SSL-Proxy von Mobilgeräten - siehe unten.


## Hosts ignorieren/ausschließen {#ignore-/-exclude-hosts}

Wenn Ihre Ausgabe zu übersichtlich wird, können Sie Positionen ignorieren oder ausschließen. Sie können Positionen ignorieren oder ausschließen, indem Sie einen der folgenden Schritte ausführen:

- Klicken Sie mit der rechten Maustaste auf die Anforderungen, die Sie ignorieren möchten, und wählen Sie &quot;Ignorieren&quot;
- Fügen Sie die Orte manuell hinzu, die von `\[ *Proxy -\> Recording Settings... -\> Exclude* \]` ausgeschlossen werden sollen


## DNS-Spoofing {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS-Spoofing ist sehr nützlich, wenn versucht wird, eine Anfrage an eine andere IP-Adresse umzuleiten, insbesondere bei der Arbeit mit Mobilgeräten:

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Remote {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Mit map remote können Sie eine &quot;eingehende&quot;Anfrage an einen anderen Endpunkt umleiten. Der häufigste Anwendungsfall für diese Funktion ist die Zuordnung von `AccessEnabler.swf` zu `AccessEnablerDebug.swf:`

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Reverse Proxy {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Mobilnummer {#mobile}

### Verwenden von Charles auf einem iOS-Gerät (iPhone/iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### SSL-Verbindung von iPhone {#ssl-connection-from-iphone}

Navigieren Sie auf Ihrem iOS-Gerät zu &quot;<http://charlesproxy.com/charles.crt>&quot;.  Dadurch wird das Dialogfeld für die Zertifikatinstallation gestartet:

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)

</br>

Klicken Sie auf `\[ *Install*... *Install*... *Done* \]` , um die Installation des Zertifikats abzuschließen.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Verwenden von Charles auf einem iOS-Gerät {#using-charles-from-an-ios-device}

Wählen Sie auf Ihrem iOS-Gerät `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]` aus. Klicken Sie auf den kleinen blauen Pfeil neben Ihrem Netzwerk, gehen Sie dann zum HTTP-Proxy und wählen Sie &quot;Manuell&quot;:


</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)


</br>
Hier müssen Sie die IP und den Port des Computers angeben, auf dem Sie Charles ausführen. <span style="line-height: 1.6em;">Wenn Sie jetzt Safari auf Ihrem iOS-Gerät öffnen und versuchen, eine Webseite zu öffnen, sollten Sie das folgende Popup auf dem Computer erhalten, auf dem Charles ausgeführt wird:

</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)

</br>
Klicken Sie auf "Zulassen", damit das Gerät Charles zum Proxy aller zugehörigen Elemente verwenden kann
-Anfragen.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Zertifikate vertrauen {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Charles für Android verwenden

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Navigieren Sie von Ihrem Android-Gerät zu [Charles proxy](http://charlesproxy.com/charles.crt).
