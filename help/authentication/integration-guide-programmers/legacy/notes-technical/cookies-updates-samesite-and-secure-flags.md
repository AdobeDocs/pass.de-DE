---
title: Cookie-Updates - SameSite- und Sicherheits-Flags
description: Cookie-Updates - SameSite- und Sicherheits-Flags
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '956'
ht-degree: 0%

---

# (Veraltete) Cookie-Aktualisierungen - SameSite- und Sicherheits-Flags {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>


## Updates {#Updates}

In diesem Abschnitt werden die Änderungen vorgestellt, die durch den Chrome-Browser und die Adobe Pass-Authentifizierung für die Verarbeitung von Drittanbieter-Cookies eingeführt wurden.



### Chrome 80-Updates {#Chrome}

Ab Chrome Version 80 (außer Version 82) werden Cookies, die kein *SameSite*-Attribut angeben, so behandelt, als wären sie *SameSite=Lax*. Daher müssen Cookies, die in einem Site-übergreifenden Kontext bereitgestellt werden müssen, explizit *SameSite=None* angeben und auch mit dem *Secure*-Attribut gekennzeichnet und über *HTTPS* bereitgestellt werden. Weitere Informationen zu diesen Updates finden Sie auf der offiziellen Chromium-Seite: <https://www.chromium.org/updates/same-site> und auch von <https://web.dev/samesite-cookies-explained/>.


### Adobe Pass-Authentifizierungsaktualisierungen {#Pass-Updates}

Der Adobe Pass-Authentifizierungsdienst verwendet derzeit einige Cookies, die aus Browser-Sicht als Drittanbieter-Cookies betrachtet werden, einschließlich Chrome, um in Kombination mit einigen Plattformen und Versionen von Adobe Pass Authentication SDKs zu funktionieren. Um die bevorstehenden Änderungen einzuhalten und diese Cookies weiterhin in einem Site-übergreifenden Kontext über diese älteren SDKs bereitzustellen, implementiert der Adobe Pass-Authentifizierungs-Service daher die erforderlichen Änderungen in der Version *adobe-pass-2.55.1*.

Diese Änderungen gegenüber der Version *adobe-pass-2.55.1* umfassen das Hinzufügen der Attribute *Secure* und *SameSite=None* für alle Cookies, die bei Verwendung von Chrome-Browsern ab Version 80 und höher (außer Version 82) an alle Adobe Pass-Authentifizierungs-SDKs zurückgegeben werden.

Im nächsten Abschnitt werden einige potenzielle Probleme für eine Liste der Plattformen und Adobe Pass-Authentifizierungs-SDKs vorgestellt, falls ein Benutzer Chrome Browser 80 und höher (außer Version 82) verwendet.

## Fehlerbehebung {#Troubleshooting}

Beachten Sie beim Durchsuchen dieses Abschnitts, dass für alle Cookies des Adobe Pass-Authentifizierungsdienstes das Attribut *Secure* in der Version *adobe-pass-2.55.1* für alle Browser festgelegt sein muss, während das Attribut *SameSite=None* nur für Chrome-Browser der Version 80 und höher (außer Version 82) festgelegt werden muss.


### Allgemeine Fehlerbehebung {#General}

1. Beachten Sie, dass einige Benutzeragenten bekanntermaßen mit dem Attribut *SameSite=None* inkompatibel sind.

   - Chrome-Versionen von Chrome 51 bis Chrome 66 (jeweils einschließlich). Diese Chrome-Versionen lehnen ein Cookie mit *SameSite=None* ab. Dies betrifft auch ältere Versionen von Chromium-abgeleiteten Browsern sowie Android WebView. Dieses Verhalten war gemäß der damaligen Version der Cookie-Spezifikation korrekt, aber mit dem Hinzufügen des neuen Werts „None“ zur Spezifikation wurde dieses Verhalten in Chrome 67 und höher aktualisiert. (Vor Chrome 51 wurde das SameSite-Attribut vollständig ignoriert, und alle Cookies wurden so behandelt, als wären sie *SameSite=None*.)
   - Versionen des UC-Browsers auf Android vor Version 12.13.2. In älteren Versionen wird ein Cookie mit *SameSite=None* abgelehnt. Dieses Verhalten war gemäß der damaligen Version der Cookie-Spezifikation korrekt, aber mit dem Hinzufügen des neuen Werts „None“ zur Spezifikation wurde dieses Verhalten in neueren Versionen des UC-Browsers aktualisiert.
   - Safari- und eingebettete Browser in MacOS 10.14 und alle Browser in iOS 12. Diese Versionen behandeln mit „SameSite=None *markierte Cookies fälschlicherweise so* als wären sie mit &quot;*=Strict* markiert. Dieser Fehler wurde in neueren Versionen von iOS und MacOS behoben.


1. Beachten Sie, dass Cookies mit dem *Secure*-Attribut über *HTTPS* gesendet werden müssen, da das Cookie sonst den Adobe Pass-Authentifizierungsdienst nicht erreicht.

   - AccessEnabler JavaScript SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* *HTTPS* für *2.35* und *3.5.0* verwendet, bevor die dynamische Client-Registrierung eingeführt wird.
   - AccessEnabler iOS/tvOS SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* *HTTPS* für Versionen vor *3.0.0* verwendet, bevor die dynamische Client-Registrierung eingeführt wird.
   - AccessEnabler Android SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* *HTTPS* für Versionen vor *3.0.0* verwendet, bevor die dynamische Client-Registrierung eingeführt wird.
   - AccessEnabler FireOS SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* *HTTPS* für Version *2.0.4)*.

</br>

### Fehlerbehebung bei AccessEnabler JavaScript SDK Version 2.35 {#235-Troubleshooting}

Der Authentifizierungsfluss des Benutzers könnte in Chrome 80 und höher (außer Version 82) beeinträchtigt sein. Um sicherzustellen, dass der Benutzer keine Probleme bei der Authentifizierung aufgrund der oben genannten Aktualisierungen hat, können Sie:

- Vergewissern Sie sich, dass *JSESSIONID*-Cookie im Browser gesetzt ist und die Attribute *SameSite=None* und *Secure* festgelegt sind.
- Vergewissern Sie sich, dass *JSESSIONID*-Cookie der *https://sp.auth.adobe.com/authenticate/saml*-Netzwerkanfrage mit dem *JSESSIONID*-Cookie der *https://sp.auth.adobe.com/session*-Netzwerkanfrage übereinstimmt.


### Fehlerbehebung bei AccessEnabler JavaScript SDK Version 3.5.0 {#350-Troubleshooting}

Der Authentifizierungsfluss des Benutzers könnte in Chrome 80 und höher (außer Version 82) beeinträchtigt sein. Um sicherzustellen, dass der Benutzer keine Probleme bei der Authentifizierung aufgrund der oben genannten Aktualisierungen hat, können Sie:

- Vergewissern Sie sich, dass *JSESSIONID*-Cookie im Browser gesetzt ist und die Attribute *SameSite=None* und *Secure* festgelegt sind.
- Vergewissern Sie sich, dass *JSESSIONID*-Cookie der *https://sp.auth.adobe.com/authenticate/saml*-Netzwerkanfrage mit dem *JSESSIONID*-Cookie der *https://sp.auth.adobe.com/session*-Netzwerkanfrage übereinstimmt.
- Überprüfen Sie, ob *Cookie „pass\* sfp“ im Browser gesetzt ist und die Attribute *SameSite=None* und *Secure* gesetzt sind.
- Überprüfen Sie, ob *Cookie „pass\_*&quot; in der Netzwerkanfrage *https://sp.auth.adobe.com/session* festgelegt ist.


Der Autorisierungsfluss des Benutzers könnte in Chrome 80 und höher (mit Ausnahme von Version 82) beeinträchtigt sein. Um sicherzustellen, dass der Benutzer keine Probleme hat, eine geschützte Ressource zu beobachten, kann man nach der erfolgreichen Authentifizierung aufgrund der oben genannten Updates:

- Überprüfen Sie, ob *Cookie „pass\* sfp“ im Browser gesetzt ist und die Attribute *SameSite=None* und *Secure* gesetzt sind.
- Überprüfen Sie, ob *Cookie „pass\_*&quot; in der Netzwerkanfrage *https://sp.auth.adobe.com/adobe-services/authorize* festgelegt ist.
- Überprüfen Sie, ob *Cookie „pass\_*&quot; in der Netzwerkanfrage *https://sp.auth.adobe.com/adobe-services/shortAuthorize* festgelegt ist.
