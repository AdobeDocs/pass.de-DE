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



### Updates für Chrome 80 {#Chrome}

Ab Chrome-Version 80 (außer Version 82) werden Cookies, die kein *SameSite* -Attribut angeben, so behandelt, als wären sie *SameSite=Lax*. Daher müssen Cookies, die in einem Site-übergreifenden Kontext bereitgestellt werden müssen, explizit den Wert *SameSite=None* angeben, außerdem mit dem Attribut *Secure* markiert und über *HTTPS* bereitgestellt werden. Weitere Informationen zu diesen Aktualisierungen finden Sie auf der offiziellen Chrom-Seite: <https://www.chromium.org/updates/same-site> und auch von <https://web.dev/samesite-cookies-explained/>.


### Adobe Pass-Authentifizierungsaktualisierungen {#Pass-Updates}

Der Adobe Pass-Authentifizierungsdienst setzt derzeit auf einige Cookies, die aus Browsersicht als Drittanbieter-Cookies betrachtet werden, einschließlich Chrome, um in Kombination mit einigen Plattformen und Versionen von Adobe Pass Authentication SDKs funktionieren zu können. Um die bevorstehenden Änderungen einzuhalten und diese Cookies von diesen älteren SDKs in einem Site-übergreifenden Kontext bereitzustellen, implementiert der Adobe Pass-Authentifizierungsdienst daher die erforderlichen Änderungen in der Version *adobe-pass-2.55.1* .

Diese Änderungen an der Version *adobe-pass-2.5.1* umfassen das Hinzufügen der Attribute *Secure* und *SameSite=None* für alle Cookies, die an alle Adobe Pass Authentication SDKs zurückgegeben werden, wenn Chrome-Browser verwendet werden, die mit Version 80 und höher beginnen (ausgenommen Version 82).

Im nächsten Abschnitt werden einige potenzielle Probleme für eine Liste von Plattformen und Adobe Pass Authentication SDKs-Versionen beschrieben, falls ein Benutzer den Chrome-Browser 80 und höher verwendet (mit Ausnahme von Version 82).

## Fehlerbehebung {#Troubleshooting}

Beachten Sie beim Durchsuchen dieses Abschnitts, dass für alle Adobe Pass-Authentifizierungsdienst-Cookies das Attribut *Secure* in der Version *adobe-pass-2.55.1* festgelegt sein muss, während das Attribut *SameSite=None* nur für Chrome-Browser ab Version 80 festgelegt werden darf (ausgenommen Version 82).


### Allgemeine Fehlerbehebung {#General}

1. Beachten Sie, dass einige Benutzeragenten bekanntermaßen nicht mit dem Attribut *SameSite=None* kompatibel sind.

   - Versionen von Chrome von Chrome 51 bis Chrome 66 (jeweils inbegriffen). Diese Chrome-Versionen lehnen ein Cookie mit *SameSite=None* ab. Dies betrifft auch ältere Versionen von Chromium-abgeleiteten Browsern sowie Android WebView. Dieses Verhalten war entsprechend der damals verwendeten Version der Cookie-Spezifikation korrekt. Mit dem Zusatz &quot;Keine&quot;zur Spezifikation wurde dieses Verhalten jedoch in Chrome 67 und höher aktualisiert. (Vor Chrome 51 wurde das Attribut SameSite vollständig ignoriert und alle Cookies wurden so behandelt, als wären sie *SameSite=None*.)
   - Versionen des UC-Browsers auf Android vor Version 12.13.2. In älteren Versionen wird ein Cookie mit *SameSite=None* abgelehnt. Dieses Verhalten war gemäß der damals verwendeten Version der Cookie-Spezifikation korrekt. Mit dem neuen Wert &quot;Keine&quot;zur Spezifikation wurde dieses Verhalten jedoch in neueren Versionen des UC-Browsers aktualisiert.
   - Versionen von Safari und eingebetteten Browsern in MacOS 10.14 und allen Browsern in iOS 12. Diese Versionen behandeln fälschlicherweise Cookies, die mit *SameSite=None* markiert sind, so, als ob sie mit *SameSite=Strict* markiert wären. Dieser Fehler wurde in neueren Versionen von iOS und MacOS behoben.


1. Beachten Sie, dass Cookies mit dem Attribut *Secure* über *HTTPS* gesendet werden müssen. Andernfalls erreicht das Cookie nicht den Adobe Pass-Authentifizierungsdienst.

   - AccessEnabler JavaScript SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* *HTTPS* für die Versionen *2.35* und *3.5.0* verwendet, bevor eine dynamische Client-Registrierung eingeführt wird.
   - AccessEnabler iOS/tvOS SDK:
      - Die Kommunikation mit *sp.auth.adobe.com* muss *HTTPS* für Versionen vor *3.0.0* verwenden, bevor eine dynamische Client-Registrierung eingeführt wird.
   - AccessEnabler Android SDK:
      - Die Kommunikation mit *sp.auth.adobe.com* muss *HTTPS* für Versionen vor *3.0.0* verwenden, bevor eine dynamische Client-Registrierung eingeführt wird.
   - AccessEnabler FireOS-SDK:
      - Obligatorisch, dass die Kommunikation mit *sp.auth.adobe.com* *HTTPS* für Version *2.0.4* verwendet.

</br>

### AccessEnabler JavaScript SDK Version 2.35 Fehlerbehebung {#235-Troubleshooting}

Der Authentifizierungsfluss des Benutzers kann in Chrome 80 und höher beeinträchtigt sein (mit Ausnahme von Version 82). Um sicherzustellen, dass der Benutzer aufgrund der oben genannten Updates keine Probleme bei der Authentifizierung hat, können Sie Folgendes tun:

- Überprüfen Sie, ob das Cookie *JSESSIONID* im Browser gesetzt ist und die Attribute *SameSite=None* und *Secure* festgelegt sind.
- Überprüfen Sie, ob das Cookie *JSESSIONID* aus der Netzwerkanforderung *https://sp.auth.adobe.com/authenticate/saml* mit dem Cookie *JSESSIONID* aus der Netzwerkanforderung *https://sp.auth.adobe.com/session* übereinstimmt.


### AccessEnabler JavaScript SDK Version 3.5.0 - Fehlerbehebung {#350-Troubleshooting}

Der Authentifizierungsfluss des Benutzers kann in Chrome 80 und höher beeinträchtigt sein (mit Ausnahme von Version 82). Um sicherzustellen, dass der Benutzer aufgrund der oben genannten Updates keine Probleme bei der Authentifizierung hat, können Sie Folgendes tun:

- Überprüfen Sie, ob das Cookie *JSESSIONID* im Browser gesetzt ist und die Attribute *SameSite=None* und *Secure* festgelegt sind.
- Überprüfen Sie, ob das Cookie *JSESSIONID* aus der Netzwerkanforderung *https://sp.auth.adobe.com/authenticate/saml* mit dem Cookie *JSESSIONID* aus der Netzwerkanforderung *https://sp.auth.adobe.com/session* übereinstimmt.
- Überprüfen Sie, ob das Cookie *pass\_sfp* im Browser gesetzt ist und die Attribute *SameSite=None* und *Secure* festgelegt sind.
- Überprüfen Sie, ob das Cookie *pass\_sfp* in der Netzwerkanforderung *https://sp.auth.adobe.com/session* gesetzt ist.


Der Autorisierungsfluss des Benutzers kann in Chrome 80 und höher beeinflusst werden (mit Ausnahme von Version 82). Um sicherzustellen, dass der Benutzer keine Probleme hat, eine geschützte Ressource zu sehen, nachdem er sich erfolgreich authentifiziert hat, kann aufgrund der oben genannten Aktualisierungen Folgendes ausgeführt werden:

- Überprüfen Sie, ob das Cookie *pass\_sfp* im Browser gesetzt ist und die Attribute *SameSite=None* und *Secure* festgelegt sind.
- Überprüfen Sie, ob das Cookie *pass\_sfp* in der Netzwerkanforderung *https://sp.auth.adobe.com/adobe-services/authorize* gesetzt ist.
- Überprüfen Sie, ob das Cookie *pass\_sfp* in der Netzwerkanforderung *https://sp.auth.adobe.com/adobe-services/shortAuthorize* gesetzt ist.
