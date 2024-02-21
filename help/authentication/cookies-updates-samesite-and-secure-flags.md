---
title: Cookie-Updates - SameSite- und Secure-Flags
description: Cookie-Updates - SameSite- und Secure-Flags
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Cookie-Updates - SameSite- und Secure-Flags {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>


## Updates {#Updates}

In diesem Abschnitt werden die Änderungen erläutert, die durch den Chrome-Browser und die Adobe Pass-Authentifizierung für die Verarbeitung von Drittanbieter-Cookies eingeführt wurden.



### Aktualisierungen für Chrome 80 {#Chrome}

Ab Chrome-Version 80 (außer Version 82): Cookies, die keine *SameSite* -Attribut wird so behandelt, als wären sie *SameSite=Lax*. Daher müssen Cookies, die in einem Site-übergreifenden Kontext bereitgestellt werden müssen, explizit die Variable *SameSite=None* und muss auch mit dem *Sicher* Attribut und Bereitstellung *HTTPS*. Weitere Informationen zu diesen Aktualisierungen finden Sie auf der offiziellen Chromium-Seite: <https://www.chromium.org/updates/same-site> und <https://web.dev/samesite-cookies-explained/>.


### Adobe Pass-Authentifizierungsaktualisierungen {#Pass-Updates}

Der Adobe Pass-Authentifizierungsdienst setzt derzeit auf einige Cookies, die aus Browsersicht als Drittanbieter-Cookies betrachtet werden, einschließlich Chrome, um in Kombination mit einigen Plattformen und Versionen von Adobe Pass Authentication SDKs funktionieren zu können. Um die anstehenden Änderungen einzuhalten und diese Cookies von diesen älteren SDKs in einem Site-übergreifenden Kontext bereitzustellen, implementiert der Adobe Pass-Authentifizierungsdienst daher die erforderlichen Änderungen in der *adobe-pass-2.55.1* -Version.

Diese Änderungen werden durch die *adobe-pass-2.55.1* -Version umfasst das Hinzufügen von *Sicher* und *SameSite=None* -Attribute für alle Cookies, die an alle Adobe Pass Authentication SDKs zurückgegeben werden, wenn Chrome-Browser verwendet werden, die mit Version 80 und höher beginnen (ausgenommen Version 82).

Im nächsten Abschnitt werden einige potenzielle Probleme für eine Liste von Plattformen und Adobe Pass Authentication SDKs-Versionen beschrieben, falls ein Benutzer den Chrome-Browser 80 und höher verwendet (mit Ausnahme von Version 82).

## Fehlerbehebung {#Troubleshooting}

Beachten Sie beim Durchsuchen dieses Abschnitts, dass alle Cookies des Adobe Pass-Authentifizierungsdienstes über *Sicher* -Attribut in *adobe-pass-2.55.1* -Version für alle Browser, während die Variable *SameSite=None* -Attribut nur für Chrome-Browser Version 80 und höher festgelegt werden (mit Ausnahme von Version 82).


### Allgemeine Fehlerbehebung {#General}

1. Beachten Sie, dass einige Benutzeragenten bekanntermaßen inkompatibel mit dem *SameSite=None* -Attribut.

   - Versionen von Chrome von Chrome 51 bis Chrome 66 (jeweils inklusive). In diesen Chrome-Versionen wird ein Cookie mit *SameSite=None*. Dies betrifft auch ältere Versionen von Chromium-abgeleiteten Browsern sowie Android WebView. Dieses Verhalten war gemäß der damals verwendeten Version der Cookie-Spezifikation korrekt. Mit dem Zusatz &quot;Keine&quot;zur Spezifikation wurde dieses Verhalten jedoch in Chrome 67 und höher aktualisiert. (Vor Chrome 51 wurde das SameSite-Attribut vollständig ignoriert und alle Cookies wurden so behandelt, als wären sie *SameSite=None*.
   - Versionen des UC-Browsers unter Android vor Version 12.13.2. In älteren Versionen wird ein Cookie mit *SameSite=None*. Dieses Verhalten war gemäß der damals verwendeten Version der Cookie-Spezifikation korrekt. Mit dem neuen Wert &quot;Keine&quot;zur Spezifikation wurde dieses Verhalten jedoch in neueren Versionen des UC-Browsers aktualisiert.
   - Versionen von Safari und eingebetteten Browsern in MacOS 10.14 und allen Browsern in iOS 12. Diese Versionen behandeln fälschlicherweise Cookies, die mit *SameSite=None* als ob sie als *SameSite=Strict*. Dieser Fehler wurde in neueren Versionen von iOS und MacOS behoben.


1. Beachten Sie, dass Cookies mit dem *Sicher* -Attribut muss übergeben werden *HTTPS*, andernfalls erreicht das Cookie nicht den Adobe Pass-Authentifizierungsdienst.

   - AccessEnabler JavaScript-SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* uses *HTTPS* für Versionen *2,35* und *3,5,0*, bevor Sie die Dynamic Client-Registrierung einführen.
   - AccessEnabler iOS/tvOS SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* uses *HTTPS* für Versionen vor *3,0,0*, bevor Sie die Dynamic Client-Registrierung einführen.
   - AccessEnabler Android SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* uses *HTTPS* für Versionen vor *3,0,0*, bevor Sie die Dynamic Client-Registrierung einführen.
   - AccessEnabler FireOS-SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* uses *HTTPS* für Version *2,0,4*.

</br>

### AccessEnabler JavaScript SDK Version 2.35 Fehlerbehebung {#235-Troubleshooting}

Der Authentifizierungsfluss des Benutzers kann in Chrome 80 und höher betroffen sein (mit Ausnahme von Version 82). Um sicherzustellen, dass der Benutzer aufgrund der oben genannten Updates keine Probleme bei der Authentifizierung hat, können Sie Folgendes tun:

- Stellen Sie sicher, dass die *JSESSIONID* -Cookie im Browser gesetzt wird und die Variable *SameSite=None* und *Sicher* -Attribute festgelegt.
- Stellen Sie sicher, dass die *JSESSIONID* -Cookie aus der *https://sp.auth.adobe.com/authenticate/saml* Die Netzwerkanforderung stimmt mit der *JSESSIONID* -Cookie aus der *https://sp.auth.adobe.com/session* Netzwerkanforderung.


### AccessEnabler JavaScript SDK Version 3.5.0 Fehlerbehebung {#350-Troubleshooting}

Der Authentifizierungsfluss des Benutzers kann in Chrome 80 und höher betroffen sein (mit Ausnahme von Version 82). Um sicherzustellen, dass der Benutzer aufgrund der oben genannten Updates keine Probleme bei der Authentifizierung hat, können Sie Folgendes tun:

- Stellen Sie sicher, dass die *JSESSIONID* -Cookie im Browser gesetzt wird und die Variable *SameSite=None* und *Sicher* -Attribute festgelegt.
- Stellen Sie sicher, dass die *JSESSIONID* -Cookie aus der *https://sp.auth.adobe.com/authenticate/saml* Die Netzwerkanforderung stimmt mit der *JSESSIONID* -Cookie aus der *https://sp.auth.adobe.com/session* Netzwerkanforderung.
- Stellen Sie sicher, dass die *pass\_sfp* -Cookie im Browser gesetzt wird und die Variable *SameSite=None* und *Sicher* -Attribute festgelegt.
- Stellen Sie sicher, dass die *pass\_sfp* Cookie wird in gesetzt *https://sp.auth.adobe.com/session* Netzwerkanforderung.


Der Autorisierungsfluss des Benutzers kann in Chrome 80 und höher betroffen sein (mit Ausnahme von Version 82). Um sicherzustellen, dass der Benutzer keine Probleme hat, eine geschützte Ressource zu sehen, nachdem er sich erfolgreich authentifiziert hat, kann aufgrund der oben genannten Aktualisierungen Folgendes ausgeführt werden:

- Stellen Sie sicher, dass die *pass\_sfp* -Cookie im Browser gesetzt wird und die Variable *SameSite=None* und *Sicher* -Attribute festgelegt.
- Stellen Sie sicher, dass die *pass\_sfp* Cookie wird in gesetzt *https://sp.auth.adobe.com/adobe-services/authorize* Netzwerkanforderung.
- Stellen Sie sicher, dass die *pass\_sfp* Cookie wird in gesetzt *https://sp.auth.adobe.com/adobe-services/shortAuthorize* Netzwerkanforderung.
