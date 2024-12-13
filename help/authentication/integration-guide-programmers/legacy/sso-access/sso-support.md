---
title: Unterstützung für Single Sign-on
description: Unterstützung für Single Sign-on
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 0%

---

# Unterstützung für (veraltete) Single Sign-On

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Übersicht {#overview-sso-support}

In diesem Dokument werden die Arten von Single Sign-On beschrieben, die von der Adobe Pass-Authentifizierung auf verschiedenen Plattformen unterstützt und unterstützt werden. Ziel dieses Dokuments ist es, aufzuklären, was unterstützt wird und was nicht, was die MVPD-Abdeckung für jede SSO-Methode ist und was von den Programmierern benötigt wird, um von SSO auf jeder Plattform profitieren zu können.

Nachdem sich ein Benutzer mit seinen MVPD-Anmeldeinformationen angemeldet hat, generiert die Adobe Pass-Authentifizierung ein sicheres Token, das die Authentifizierungssitzung von MVPD darstellt und dieses Token mithilfe einer Geräte-ID an das Gerät des Benutzers bindet. Die Adobe Pass-Authentifizierung speichert das Token/die Geräte-ID entweder auf einem Server oder auf dem Gerät. Dadurch können Benutzer ihre Anmeldeinformationen seltener eingeben und gleichzeitig die Transaktionen sicher halten.

>[!NOTE]
>
>SSO-Workflows sind Teil des Premium-Workflow-Pakets. Wenden Sie sich an Ihren Adobe Pass-Vertriebsmitarbeiter, wenn Sie diese Funktion verwenden möchten.

## Aktueller Status für SSO auf verschiedenen Plattformen {#current-sso-status-platforms}

| Plattform/Gerät | SSO-Unterstützung | SSO-Typ | MVPD-Abdeckung | Notizen |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Web (JavaScript) | Ja | Freigegebenes Authentifizierungstoken (Adobe SSO) | Alle | Kein Browser-übergreifendes SSO Bitte befolgen Sie die Anweisungen im Handbuch zur Programmiererintegration für JavaScript. Nach Befolgen der Anweisungen ist SSO standardmäßig aktiviert.  Durch die Aktivierung der Authentifizierung pro Anforderer wird SSO unterbrochen |
| iOS | Ja | Plattform-SSO - Token-Austausch | Abhängig vom Apple-Support finden Sie die Liste hier | Ab iOS 10 führte Apple &amp; Adobe SSO-Funktionen für teilnehmende Programmierer und MVPDs ein. Durch die Verwendung der neuesten Adobe iOS SDK oder durch die Verwendung der Client-losen Adobe REST-API und die Implementierung der Apple SSO-Funktionalität können Sie von SSO auf iOS-Geräten profitieren. Weitere Informationen zur Implementierung von SDK finden Sie hier, weitere Informationen zur Implementierung von ClientLess hier. Zusätzliche Hinweise: Wenn Sie Apple SSO nicht verwenden möchten, können Sie immer noch ein begrenztes SSO zwischen Apps desselben Anbieters (dieselbe Bundle-ID), der Speicher gemeinsam nutzen kann, und einer ID (IDFV) verwenden. Daher ist SSO nur auf die Apps desselben Anbieters beschränkt. |
| Android | Ja | Freigegebenes Authentifizierungstoken (Adobe SSO) | Alle | Wenn der Benutzer die Berechtigungsanfrage WRITE_EXTERNAL_STORAGE nicht akzeptiert, verwendet die Bibliothek einen lokalen Sandbox-Speicher. In diesem Fall bedeutet dies, dass es bei Verwendung des lokalen Speichers kein SSO zwischen verschiedenen Anwendungen gibt. |
| tvOS - neues Apple TV | Ja | Plattform-SSO - Token-Austausch | Abhängig vom Apple-Support finden Sie die Liste hier | Ab tvOS 10 führte Apple &amp; Adobe SSO-Funktionen für teilnehmende Programmierer und MVPDs ein. Durch die Verwendung der neuesten Adobe tvOS SDK oder durch die Verwendung der Client-losen Adobe REST API und die Implementierung der Apple SSO-Funktionalität können Sie von SSO auf tvOS-Geräten profitieren. Weitere Informationen zu tvOS SDK finden Sie hier und hier sowie weitere Informationen zur Client-losen Implementierung hier. |
| Roku | Ja | Freigegebenes Authentifizierungstoken (Adobe SSO) | Umfassende und vollständige Liste wird in Kürze vorgelegt. | Roku SSO funktioniert standardmäßig mit der Clientless-API für alle Kunden, die die Roku-Richtlinien einhalten, ohne dass eine spezielle Implementierung erforderlich ist. SSO basiert auf Geräteidentifizierungsinformationen, die Roku sicher an Adobe sendet. |
| Amazon FireTV | Ja | Freigegebenes Authentifizierungstoken (Adobe SSO) | Umfassende und vollständige Liste wird in Kürze vorgelegt. | FireTV SDK unterstützt Single Sign-On auf Basis der Android-Funktionen. Das SSO auf dieser Plattform ist nur zwischen Apps möglich, die vorerst Adobe FireTV SDK verwenden. Mehr Infos zum neuen FireTV SDK gibt es hier. FireTV-Apps, die zusätzlich zur Clientless-API implementiert wurden, können ab EOY 2018 von SSO profitieren. |
| Xbox 360 | Nein |                                         |                                                     | Es gibt keine Geräte-ID, die wir nutzen können. Es gibt eine App-ID, sodass sich Benutzer nicht jedes Mal authentifizieren müssen. |
| Xbox One | Nein |                                         |                                                     | Es gibt keine Geräte-ID, die wir nutzen können. Es gibt eine App-ID, sodass sich Benutzer nicht jedes Mal authentifizieren müssen. |
| Windows 8/10 | Nein |                                         |                                                     | Es gibt keine Geräte-ID, die wir nutzen können. Es gibt eine App-ID, sodass sich Benutzer nicht jedes Mal authentifizieren müssen. |
| Samsung TVs | Nein |                                         |                                                     | Es gibt keine Geräte-ID, die wir nutzen können. Es gibt eine App-ID, sodass sich Benutzer nicht jedes Mal authentifizieren müssen. |

### Hinweise zu Xbox 360 und Xbox One {#notes-xbox-360}

* **Xbox 360**- Xbox 360 stellt über den Live Service das Token bereit, in das die deviceID eingebettet ist. Die Live Service-Ebenen im appID-Wert für deviceID, sodass er nur für die App gilt. Für Xbox 360 stellte Microsoft eine Java-Bibliothek zum Adobe bereit, die beim Parsen des Tokens hilfreich war.

* **Xbox One** - Es wird ein JSON-Web-Token ausgegeben, das mit dem Cert/Key des Herausgebers verschlüsselt und von Microsoft signiert ist. Adobe extrahiert die Geräte-ID aus einem Parameter namens DPI (Device Pairwise ID), der sich von der Xbox 360 Parameter-PDID (Partner Device ID) unterscheidet. PDID existiert auch in Xbox One, soll aber durch diesen neuen Parameter „Device Pairwise ID“ (DPI) ersetzt werden.


### SSO deaktivieren {#disable-sso}

In bestimmten Situationen möchten einige Apps oder Sites SSO deaktivieren, um erweiterte Geschäftsfälle zu erfüllen.

* **Für JS und native SDKs** - Das Adobe Pass-Authentifizierungs-Support-Team kann SSO für ein Anforderer-ID-/MVPD-Paar deaktivieren. Auf Sites oder in nativen Apps ist keine Arbeit erforderlich.  Sobald SSO vom Adobe Pass-Authentifizierungs-Support-Team deaktiviert wurde, werden Authentifizierungen, die mit dem angegebenen RequestorId-/MVPD-Paar durchgeführt wurden, nicht mehr für Sites oder Apps freigegeben, die andere Requestor IDs verwenden. Darüber hinaus sind bestehende Authentifizierungen mit unterschiedlichen Anforderer-IDs nicht für die Kombination Anforderer-ID/MVPD gültig, in der SSO deaktiviert wurde. Aus technischer Sicht wird die SSO-Deaktivierung erreicht, indem das AuthN-Token an die spezifische Anfrager-ID-/MVPD-Kombination gebunden wird.
* **Für Client-lose API** - Sie können SSO im Client-losen Authentifizierungsfluss deaktivieren, indem Sie einen nicht leeren appId-Parameter in den REST-Aufrufen angeben. Sie können eine beliebige Zeichenfolge als Wert verwenden, sofern diese Zeichenfolge für die Anforderer-ID eindeutig ist. Beachten Sie, dass bei der Client-losen API der Programmierer/Implementierer die Site oder die App ändern muss, um diesen anfordererspezifischen Parameter hinzuzufügen.

>[!IMPORTANT]
>
>WICHTIGER HINWEIS FÜR CLIENTLESS-API SSO: Bei einigen MVPDs muss jedes Netzwerk (Anforderer-ID) einen eigenen Authentifizierungsfluss durchführen. Bei SDK-basierten Flüssen (iOS usw.) wird dies automatisch von der SDK verarbeitet. Bei Client-losen APIs muss dies jedoch vom Programmierer gehandhabt werden. Wir empfehlen Programmierern dringend, an dieser Stelle keine SSO-Flüsse für Client-lose APIs zu aktivieren und stattdessen eine Kombination aus Geräte-ID und App-ID für die Geräte-ID zu verwenden. Adobe wird auch daran arbeiten, die Client-losen API-Flüsse zu verbessern, damit ein ordnungsgemäßes SSO eingerichtet werden kann.

### Abmelden {#logout-sso-support}

Programmierer müssen sich bewusst sein, dass die Aktion „Abmelden“ im Kontext von Single Sign-On, wenn sie in einer App/auf einer Site durchgeführt wird, alle Token auf dem Gerät löscht und der Benutzer über Apps/Sites hinweg abgemeldet wird.

Wenn SSO-Bedingungen erfüllt sind (unabhängig davon, ob SSO aktiviert oder deaktiviert ist), wird eine Abmeldung durchgeführt und alle Authentifizierungs- und Autorisierungsinformationen werden gelöscht.
