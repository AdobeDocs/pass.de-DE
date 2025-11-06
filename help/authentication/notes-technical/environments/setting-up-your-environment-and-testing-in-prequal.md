---
title: Einrichten der Umgebung und Testen in Pre-Qual
description: Einrichten der Umgebung und Testen in Pre-Qual
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# Einrichten der Umgebung und Testen in Pre-Qual{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Diese technische Anmerkung soll unseren Partnern beim Einrichten ihrer Umgebung helfen und mit dem Testen eines neuen Builds beginnen, der in der Adobe-Umgebung vor der Qualifizierung bereitgestellt wird.

Da es zwei Build-Varianten gibt: ***Produktion*** und ***Staging***, konzentrieren wir uns in diesem Dokument auf die Produktionseinrichtung mit dem Hinweis, dass alle Schritte für das Staging gleich sind, nur die URLs sind unterschiedlich.

In den Schritten 1 und 2 wird die Testumgebung auf einem der Testgeräte eingerichtet. In Schritt 3 wird der Grundfluss überprüft, und in den Schritten 4 und 5 werden einige Testrichtlinien vorgestellt.

>[!IMPORTANT]
>
> Es ist sehr wichtig, die Schritte 1 und 2 jedes Mal auszuführen, wenn Sie Ihre Testumgebung ändern möchten (vom Staging zum Produktionsprofil oder umgekehrt)


## SCHRITT 1. Problemlösendes Übergeben der Domain an eine IP {#resolving-pass-domain-to-an-ip}

* Führen Sie den folgenden Befehl aus, um eine IP-Adresse mit Lastenausgleich zu finden, die für das Spoofing verwendet werden kann:

* **Unter Windows**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* **Unter Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>Domains werden von der Antwort ausgeschlossen, da sie nicht relevant sind und sich von Benutzer zu Benutzer unterscheiden können.

>[!IMPORTANT]
>
> Diese IP-Adressen können sich in Zukunft ändern und sind für Benutzende in verschiedenen geografischen Regionen möglicherweise nicht mehr identisch.


## SCHRITT 2.  Spoofing der Umgebung für die Vorqualifizierung zur Produktion {#spoofing-the-prequalification-environment}

* Bearbeiten Sie die Datei *c:\\windows\\System32\\drivers\\etc\\hosts* (unter Windows) oder */etc/hosts* (unter Macintosh/Linux/Android) und fügen Sie Folgendes hinzu:

* Testproduktionsprofil
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**Spoofing auf Android:** Zum Spoofing auf Android müssen Sie einen Android-Emulator verwenden.

* Sobald das Spoofing eingerichtet ist, können Sie einfach die regulären URLs für die Produktions- und Staging-Profile verwenden: (d. h. `http://sp.auth-staging.adobe.com` und `http://entitlement.auth-staging.adobe.com`, und Sie werden tatsächlich die *Präqualifikationsumgebung/-produktion* des neuen Builds erreichen.


## SCHRITT 3.  Stellen Sie sicher, dass Sie auf die richtige Umgebung verweisen {#Verify-you-are-pointing-to-the-right-environment}

**Dies ist ein einfacher Schritt:**

* Laden Sie [Umgebung für Berechtigungen ](https://entitlement-prequal.auth.adobe.com/environment.html) und [Berechtigung](https://entitlement.auth.adobe.com/environment.html). Sie sollten dieselbe Antwort zurückgeben.


## SCHRITT 4.  Durchführen eines einfachen Authentifizierungs-/Autorisierungsflusses über die Website des Programmierers {#peform-a-simple-auth-flow}

* Dieser Schritt erfordert die Website-Adresse des Programmierers und einige gültige MVPD-Anmeldeinformationen (einen Benutzer, der authentifiziert und autorisiert ist).

## SCHRITT 5.  Durchführen von Szenario-Tests mithilfe der Websites des Programmierers {#perform-scenario-testing-using-programmer-website}

* Nachdem Sie die Einrichtung der Umgebung abgeschlossen und sichergestellt haben, dass der grundlegende Authentifizierungs-Autorisierungsfluss funktioniert, können Sie mit dem Testen komplexerer Szenarien fortfahren.


## SCHRITT 6.  Durchführen von Tests mithilfe der API-Test-Site {#perform-testing-using-api-testing-site}

* Wenn Sie sich eingehender mit dem Testen der Adobe Pass-Authentifizierung befassen möchten, empfehlen wir die Verwendung der [API-Test-Site](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Weitere Informationen zur API-Test-Site finden Sie unter [Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Adobe-API-Test-Site](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).
