---
title: Charles-Proxy verwenden
description: Charles-Proxy verwenden
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 175755aa7463257487b29c5f4da989cf34e91bfd
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# (veraltet) Verwenden von Charles Proxy {#using-charles-proxy}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

**Charles:** <http://charlesproxy.com>


## Herunterladen, Installieren und Erste Schritte mit Charles Proxy {#download-install-and-get-stared-with-charles-proxy}

- **Herunterladen** - <http://www.charlesproxy.com/download/>
- **Installieren** - <http://www.charlesproxy.com/documentation/installation/>
- **Erste Schritte** - <http://www.charlesproxy.com/documentation/getting-started/>


## Struktur vs. Sequenz-Registerkarten {#structure-vs-sequence-tabs}

Es gibt zwei verschiedene Möglichkeiten, den Traffic anzuzeigen:

1. **Struktur** - Anfragen werden nach Host gruppiert
1. **Sequenz** - Anfragen werden in der Reihenfolge aufgelistet, in der sie aufgerufen werden


## SSL und Zertifikate {#ssl-and-certificates}

SSL-Proxy-`\[ *Proxy -\> Proxy Settings... -\> SSL* \]` aktivieren

Aktivieren Sie das Kontrollkästchen „SSL-Proxy aktivieren“ und fügen Sie alle HTTPS-Speicherorte hinzu.

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)
-->

- SSL-Proxy - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL-Zertifikate - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- SSL-Proxys von Mobilgeräten - Siehe unten.


## Hosts ignorieren/ausschließen {#ignore-/-exclude-hosts}

Wenn Ihre Ausgabe zu überladen ist, können Sie Speicherorte ignorieren oder ausschließen. Sie können Speicherorte ignorieren oder ausschließen, indem Sie einen der folgenden Schritte ausführen:

- Klicken Sie mit der rechten Maustaste auf die Anfragen, die Sie ignorieren möchten, und wählen Sie dann „Ignorieren“
- Manuelles Hinzufügen der von der `\[ *Proxy -\> Recording Settings... -\> Exclude* \]` auszuschließenden Speicherorte


## DNS-Spoofing {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS-Spoofing ist sehr nützlich, wenn Sie versuchen, eine Anfrage an eine andere IP-Adresse umzuleiten, insbesondere bei der Arbeit mit Mobilgeräten:

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Remote zuordnen {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Mit Remote Map können Sie eine „eingehende“ Anfrage an einen anderen Endpunkt umleiten. Der häufigste Anwendungsfall für diese Funktion ist die „Zuordnung“ von `AccessEnabler.swf` zu `AccessEnablerDebug.swf:`

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Reverse-Proxy {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Mobiltelefon {#mobile}

### Verwenden von Charles auf einem iOS-Gerät (iPhone/iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### SSL-Verbindung von iPhone {#ssl-connection-from-iphone}

Navigieren Sie von Ihrem iOS-Gerät aus zu <http://charlesproxy.com/charles.crt> .  Dadurch wird das Dialogfeld für die Zertifikatinstallation gestartet:

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)
-->

</br>

Klicken Sie auf `\[ *Install*... *Install*... *Done* \]` , um die Installation des Zertifikats abzuschließen.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Verwenden von Charles auf einem iOS-Gerät {#using-charles-from-an-ios-device}

Wählen Sie auf Ihrem iOS-Gerät `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]` aus. Klicken Sie auf den kleinen blauen Pfeil neben Ihrem Netzwerk, gehen Sie dann zu HTTP-Proxy und wählen Sie „Manuell“ aus:


</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)
-->

</br>
Hier müssen Sie die IP-Adresse und den Port des Computers angeben, auf dem Sie Charles ausführen. <span style="line-height: 1.6em;">Wenn Sie jetzt Safari auf Ihrem iOS-Gerät öffnen und versuchen, eine Web-Seite zu öffnen, sollten Sie das folgende Popup auf dem Computer erhalten, auf dem Charles ausgeführt wird:

</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)
-->

</br>
Klicken Sie auf „Zulassen“, damit das Gerät Charles verwenden kann, um alle seine
Anfragen.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Einem Zertifikat vertrauen {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS-Authentifizierungsfehler - adobepass.ios.app kann nicht gefunden werden

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Charles für Android verwenden

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Navigieren Sie von [&#x200B; Android-Gerät &#x200B;](http://charlesproxy.com/charles.crt)Charles proxy“.
