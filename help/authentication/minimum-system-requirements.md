---
title: Mindestsystemanforderungen
description: Mindestsystemanforderungen
exl-id: 57b21e2a-abd7-4b4b-85f1-25584a850e40
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Mindestsystemanforderungen {#minimum-system-requirements}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Übersicht {#overview}

In diesem Dokument werden die aktuellen Software- und Hardwareanforderungen für die Implementierung von Adobe Pass-Authentifizierungsintegrationen auf unterstützten Plattformen beschrieben. Alle unten aufgeführten unterstützten Web-/Mobilbrowser und Betriebssysteme werden vom Adobe Pass-Authentifizierungsteam vollständig unterstützt, das an die vereinbarten SLAs gebunden ist.

Während wir als Adobe Pass-Authentifizierungsteam die Verwendung der neuesten stabilen Versionen der Browser und Betriebssysteme fördern, erkennen wir auch an, dass inkompatible/ältere Plattformen und Browser vorhanden sind, die derzeit verwendet werden. Diese veralteten Geräte funktionieren möglicherweise weiterhin problemlos, sind jedoch fehleranfälliger.

Der anfängliche Ansatz, Probleme zu beheben, die auf diesen veralteten Plattformen auftreten, sollte ein Upgrade auf die neuesten Versionen sein. Dabei kann es sich um die Betriebssystemversion, die Browserversion oder die Version der installierten Anwendung handeln.

Alle Probleme, die auf diesen Plattformen auftreten, werden vom Adobe Pass-Authentifizierungsteam nach besten Kräften behoben.

Adobe Pass ermutigt unsere Kunden und Partner, ein Upgrade auf die neuesten Versionen in Erwägung zu ziehen, um neben Leistungs-, Effizienz- und Sicherheitsverbesserungen bei potenziellen Problemen auch die volle Unterstützung von Adobe nutzen zu können.


## Browser- und Betriebssystemanforderungen {#browser-OS-system-requirements}


| Web-/Mobilbrowser (†) | Unterstützte Versionen |
|---|---|
| Google Chrome | **70** oder höher |
| Mozilla Firefox | **57** oder höher |
| Apple Safari | **14** oder höher |
| Microsoft Edge | **100** oder höher |

(†) Adobe rät von der Verwendung des privaten oder Inkognito-Modus ab.

| Betriebssystem | Unterstützte Versionen |
|---|---|
| *Android* | **7.0** (Nougat) oder höher |
| *iOS* | **14** oder höher |
| *iPadOS* | **14** oder höher |
| *tvOS* | **14** oder höher |
| *Fire OS* | **5 (Android 5.1)** oder höher |
| *Mac OS* | **10.13** oder höher |
| *Microsoft Windows* | **10** oder höher |




>[!NOTE]
>
>Drittanbieter-Cookies - Berechtigungsflüsse zur Adobe Pass-Authentifizierung schlagen möglicherweise fehl, wenn Drittanbieter-Cookies deaktiviert sind.  Dieses Problem tritt nur bei Änderung der Browsereinstellungen auf. Bei allen unterstützten Browsern sollte die Adobe Pass-Authentifizierung mit den Standardeinstellungen funktionieren.


## Geräteanforderungen für clientlose (REST)-Implementierungen {#general_clientless_reqs}


Jedes Gerät, das Adobe Pass-Authentifizierungsdienste über clientlose Implementierungen **nutzt, muss in der Lage sein,**:

* Stellen Sie eine eindeutige Hash-Geräte-ID bereit. Wenn das Gerät keine eindeutige Hash-Geräte-ID bereitstellt, muss es in der Lage sein, eine eindeutige ID beizubehalten, die von der Adobe Pass-Authentifizierung bereitgestellt wird. Das Gerät sollte in der Lage sein, die eindeutige ID dauerhaft im lokalen Speicher zu speichern und die eindeutige ID als Geräte-ID anzugeben, wenn die Adobe Pass-Authentifizierungs-APIs aufgerufen werden.
* Generieren digitaler Signaturen mit dem HMAC-SHA1-Algorithmus
* Festlegen beliebiger HTTP-Header
* RESTful-Webdienste nutzen
* XML- und JSON-Datenformate analysieren
* Traffic mithilfe von HTTPS senden
* Umgang mit HTTP-Fehlercodes
