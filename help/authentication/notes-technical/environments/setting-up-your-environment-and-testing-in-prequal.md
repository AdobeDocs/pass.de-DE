---
title: Einrichten der Umgebung und Testen in Pre-Qual
description: Einrichten der Umgebung und Testen in Pre-Qual
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Einrichten der Umgebung und Testen in der Qualitur{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>Die Inhalte auf dieser Seite werden nur zu Informationszwecken zur Verfügung gestellt. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe Systems erforderlich. Eine unbefugte Nutzung ist nicht gestattet.

Der Zweck dieses technischen Hinweises besteht darin, unseren Partnern bei der Einrichtung ihrer Umgebung zu helfen und Beginn eine neue Build zu testen, die auf dem Adobe Systems Präqualifikations-Umgebung bereitgestellt wird.

Da es zwei Build Varianten gibt: ***Produktion*** und ***Staging***, werden wir in diesem Dokument auf das Produktions-Setup konzentrieren mit der Erwähnung, dass alle Schritte für das Staging gleich sind, nur die URLs sind unterschiedlich.

In den Schritten 1 und 2 wird die Test Umgebung auf einer der Prüfmaschinen eingerichtet, in Schritt 3 wird der grundlegende Ablauf überprüft und in den Schritten 4 und 5 werden einige Prüfrichtlinien vorgestellt.

>[!IMPORTANT]
>
> Es ist sehr wichtig, die Schritte 1 und 2 jedes Mal auszuführen, wenn Sie Ihre Test-Umgebung ändern möchten (Wechsel von Staging- zu Produktions-Profil oder umgekehrt)


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

* **Unter Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>Domänen wurden von der Antwort ausgeschlossen, da sie nicht relevant sind und sich von User zu User unterscheiden können.

>[!IMPORTANT]
>
> Diese IP-Adressen können sich in Zukunft ändern und sind möglicherweise für Benutzer in verschiedenen geografischen Regionen nicht gleich.


## SCHRITT 2.  Fälschung der Präqualifikation Umgebung zur Produktion {#spoofing-the-prequalification-environment}

* Bearbeiten die *Datei c:\\windows\\System32\\drivers\\etc\\hosts* (unter Windows) bzw *. /etc/hosts* (unter Macintosh/Linux/Android) und fügen Sie Folgendes hinzu:

* Spoof-Produktion Profil
   * 52.13.71.11 entitlement.auth.adobe.com sp.auth.adobe.com api.auth.adobe.com

**Spoofing auf Android:** Zum Spoofing auf Android müssen Sie einen Android-Emulator verwenden.

* Sobald das Spoofing eingerichtet ist, können Sie einfach die regulären URLs für die Produktions- und Staging-Profile verwenden: (d. h. `http://sp.auth-staging.adobe.com` und `http://entitlement.auth-staging.adobe.com`, und Sie werden tatsächlich die *Präqualifikationsumgebung/-produktion* des neuen Builds erreichen.


## SCHRITT 3.  Stellen Sie sicher, dass Sie auf die richtige Umgebung verweisen {#Verify-you-are-pointing-to-the-right-environment}

**Dies ist ein einfacher Schritt:**

* Ladeberechtigung [entspricht Umgebung](https://entitlement-prequal.auth.adobe.com/environment.html) und [Berechtigung](https://entitlement.auth.adobe.com/environment.html). Sie sollten dieselbe Antwort zurückgeben.


## SCHRITT 4.  Durchführen einer einfachen Authentifizierung/eines Autorisierung-Flows über die Website des Programmierers {#peform-a-simple-auth-flow}

* Für diesen Schritt sind die Website-Adresse des Programmierers und einige gültige MVPD-Anmeldeinformationen erforderlich (eine User, dass es authentifiziert und autorisiert ist).

## SCHRITT 5.  Szenariotests auf den Websites des Programmierers durchführen {#perform-scenario-testing-using-programmer-website}

* Nachdem Sie die Einrichtung des Umgebung abgeschlossen und sichergestellt haben, dass der Ablauf für die grundlegende Authentifizierung Autorisierung funktioniert, können Sie mit dem Testen komplexerer Szenarien fortfahren.


## SCHRITT 6.  Durchführen von Tests mithilfe der API-Test-Site {#perform-testing-using-api-testing-site}

* Wenn Sie sich eingehender mit dem Testen der Adobe Pass-Authentifizierung befassen möchten, empfehlen wir die Verwendung der [API-Test-Site](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Weitere Informationen zur API-Test-Site finden Sie unter [Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Adobe-API-Test-Site](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).
