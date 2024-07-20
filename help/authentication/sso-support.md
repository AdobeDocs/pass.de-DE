---
title: Single-Sign-On-Support
description: Single-Sign-On-Support
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%

---

# Single-Sign-On-Support

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview-sso-support}

In diesem Dokument werden die Arten von Single Sign On beschrieben, die von der Adobe Pass-Authentifizierung auf verschiedenen Plattformen unterstützt und unterstützt werden. Das Dokument soll Aufschluss darüber geben, was unterstützt wird und was nicht, welche MVPD-Abdeckung für jede SSO-Methode besteht und was von den Programmierern verlangt wird, von SSO auf jeder Plattform profitieren zu können.

Nachdem sich ein Benutzer mit seinen MVPD-Anmeldeinformationen angemeldet hat, generiert die Adobe Pass-Authentifizierung ein sicheres Token, das die Authentifizierungssitzung des MVPD darstellt und dieses Token mithilfe einer Geräte-ID an das Gerät des Benutzers bindet. Die Adobe Pass-Authentifizierung speichert das Token/die Geräte-ID entweder auf einem Server oder auf dem Gerät. Auf diese Weise können Benutzer ihre Anmeldedaten weniger häufig eingeben und gleichzeitig Transaktionen sicher halten.

>[!NOTE]
>
>SSO-Workflows sind Teil des Premium Workflow-Pakets. Wenden Sie sich an Ihren Adobe Pass-Vertriebsmitarbeiter, wenn Sie diese Funktion nutzen möchten.

## Aktueller Status der einmaligen Anmeldung auf verschiedenen Plattformen {#current-sso-status-platforms}

| Plattform/Gerät | SSO-Unterstützung | SSO-Typ | MVPD-Abdeckung | Hinweise |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Web (JavaScript) | Ja | Freigegebenes Authentifizierungstoken (Adobe SSO) | Alle | Keine browserübergreifende SSO Bitte befolgen Sie die Anweisungen im Programmer-Integrationsleitfaden für JavaScript. Nach Befolgen der Anweisungen ist SSO standardmäßig aktiviert.  Aktivierung der Authentifizierung nach Anforderer bricht SSO ab |
| iOS | Ja | Platform SSO - Token-Austausch | Abhängig vom Apple-Support - die Liste finden Sie hier . | Ab iOS 10 führte Apple &amp; Adobe SSO-Funktionen für teilnehmende Programmierer und MVPDs ein. Durch die Verwendung der neuesten Adobe iOS SDK oder durch Verwendung der Adobe Client less REST API und Implementierung der Apple SSO-Funktion können Sie von SSO auf iOS-Geräten profitieren. Weitere Informationen zur SDK-Implementierung finden Sie hier und weitere Details zur clientless-Implementierung finden Sie hier. Zusätzliche Hinweise: - Wenn Sie Apple SSO nicht verwenden möchten, können Sie weiterhin eine begrenzte SSO zwischen Apps desselben Anbieters (dieselbe Bundle-ID) verwenden, die Speicher und ID (IDFV) gemeinsam nutzen können. Daher ist SSO nur auf die Apps desselben Anbieters beschränkt. |
| Android | Ja | Freigegebenes Authentifizierungstoken (Adobe SSO) | Alle | Wenn der Benutzer die Berechtigungsanfrage WRITE_EXTERNAL_STORAGE nicht akzeptiert, verwendet die Bibliothek einen lokalen Sandbox-Speicher. In diesem Fall wird es keine einmalige Anmeldung zwischen verschiedenen Anwendungen geben, wenn der lokale Speicher verwendet wird. |
| tvOS - neues Apple TV | Ja | Platform SSO - Token-Austausch | Abhängig vom Apple-Support - die Liste finden Sie hier . | Ab tvOS 10 führte Apple &amp; Adobe SSO-Funktionen für teilnehmende Programmierer und MVPDs ein. Durch die Verwendung der neuesten Adobe tvOS SDK oder durch die Implementierung der Apple SSO-Funktion &quot;Adobe Client less REST API&quot;und der SSO-Funktion können Sie von SSO auf tvOS-Geräten profitieren. Weitere Informationen zum tvOS-SDK: hier und hier sowie weitere Details zur clientless-Implementierung finden Sie hier. |
| Roku | Ja | Freigegebenes Authentifizierungstoken (Adobe SSO) | Umfassende Liste erheblicher Abdeckung wird demnächst vorgelegt. | Roku SSO arbeitet standardmäßig mit der ClientLess-API für alle Kunden, die die Roku-Richtlinien einhalten. Es ist keine spezielle Implementierung erforderlich. SSO basiert auf Informationen zur Geräteidentifizierung, die Roku sicher an Adobe sendet. |
| Amazon FireTV | Ja | Freigegebenes Authentifizierungstoken (Adobe SSO) | Umfassende Liste erheblicher Abdeckung wird demnächst vorgelegt. | Das FireTV SDK unterstützt Single Sign-On auf der Basis von Android-Funktionen. Die einmalige Anmeldung auf dieser Plattform ist nur zwischen Apps möglich, die derzeit das Adobe FireTV SDK verwenden. Weitere Informationen zum neuen FireTV-SDK finden Sie hier . FireTV-Apps, die zusätzlich zur ClientLess-API implementiert wurden, können ab EOY 2018 von der einmaligen Anmeldung profitieren. |
| Xbox 360 | Nein |                                         |                                                     | Es gibt keine Geräte-ID, die wir verwenden können. Es gibt eine App-ID, sodass Benutzer nicht jedes Mal authentifiziert werden müssen. |
| Xbox One | Nein |                                         |                                                     | Es gibt keine Geräte-ID, die wir verwenden können. Es gibt eine App-ID, sodass Benutzer nicht jedes Mal authentifiziert werden müssen. |
| Windows 8/10 | Nein |                                         |                                                     | Es gibt keine Geräte-ID, die wir verwenden können. Es gibt eine App-ID, sodass Benutzer nicht jedes Mal authentifiziert werden müssen. |
| Samsung TVs | Nein |                                         |                                                     | Es gibt keine Geräte-ID, die wir verwenden können. Es gibt eine App-ID, sodass Benutzer nicht jedes Mal authentifiziert werden müssen. |

### Hinweise zu Xbox 360 und Xbox One {#notes-xbox-360}

* **Xbox 360**- Xbox 360 verlässt sich bei der Bereitstellung des Tokens, das die deviceID einbettet, auf den Live-Dienst. Der Live-Dienst wird im appID-Wert für deviceID auf die Ebene gesetzt, sodass er nur auf die App übertragen wird. Für Xbox 360 stellte Microsoft eine Java-Bibliothek zur Verfügung, die Adobe beim Parsen des Tokens unterstützt.

* **Xbox One** - Es wird ein JSON-Web-Token ausgegeben, das mit dem Zertifikat/Schlüssel des Herausgebers verschlüsselt und von Microsoft signiert wird. Adobe extrahiert die deviceID aus einem Parameter mit dem Namen DPI (Device Paarweise ID), der sich von der Xbox 360-Parameter-PDID (Partner Device ID) unterscheidet. Die PDID existiert auch in Xbox One, soll jedoch durch diesen neuen Parameter &quot;Device Paarthrough ID&quot;(DPI) ersetzt werden.


### SSO deaktivieren {#disable-sso}

In bestimmten Situationen sollten einige Apps oder Sites SSO deaktivieren, um erweiterte Geschäftsfälle zu erfüllen.

* **Für JS und native SDK** - Das Adobe Pass Authentication Support-Team kann SSO für ein Anforderer-ID-/MVPD-Paar deaktivieren. Auf Sites oder in nativen Apps ist keine Arbeit erforderlich.  Sobald die einmalige Anmeldung vom Adobe Pass Authentication Support-Team deaktiviert wurde, werden Authentifizierungen, die mit dem angegebenen RequestorId/MVPD-Paar durchgeführt werden, nicht mehr für Sites oder Apps mit anderen Anforderer-IDs freigegeben. Darüber hinaus gelten vorhandene Authentifizierungen mit unterschiedlichen Anforderer-IDs nicht für die Anforderer-ID/MVPD-Kombination, in der SSO deaktiviert wurde. Technisch gesehen wird die SSO-Deaktivierung erreicht, indem das AuthN-Token an die jeweilige Anforderer-ID/MVPD-Kombination gebunden wird.
* **Für clientlose API** - Sie können die einmalige Anmeldung im clientless-Authentifizierungsfluss deaktivieren, indem Sie einen nicht leeren Parameter appId in den REST-Aufrufen angeben. Sie können eine beliebige Zeichenfolge als Wert verwenden, solange diese Zeichenfolge für die Anforderer-ID eindeutig ist. Beachten Sie, dass der Programmierer/Implementor für die Client-lose API die Site oder App ändern muss, um diesen Anforderer-spezifischen Parameter hinzuzufügen.

>[!IMPORTANT]
>
>WICHTIGER HINWEIS FÜR CLIENTLESS API SSO: Einige MVPDs erfordern, dass jedes Netzwerk (Anfragende-ID) einen eigenen Authentifizierungsfluss durchführt. Bei SDK-basierten Flüssen (iOS usw.) wird dies automatisch vom SDK verarbeitet. Für die Client-losen APIs muss dies jedoch vom Programmierer verarbeitet werden. Wir empfehlen Programmierern dringend, zu diesem Zeitpunkt keine SSO-Flüsse für Client-lose APIs zu aktivieren und stattdessen eine Geräte-ID- und App-ID-Kombination für Geräte-IDs zu verwenden. Adobe arbeitet auch an der Verbesserung der clientlosen API-Flüsse, sodass eine ordnungsgemäße SSO eingerichtet werden kann.

### Abmelden {#logout-sso-support}

Programmierer müssen wissen, dass die Aktion &quot;Abmelden&quot;im Kontext von Single Sign-On bei Ausführung in einer App/auf einer Site alle Token auf dem Gerät löscht und der Benutzer über Apps/Sites hinweg abgemeldet wird.

Wenn SSO-Bedingungen erfüllt sind (unabhängig davon, ob SSO aktiviert oder deaktiviert ist), wird die Abmeldung durchgeführt und alle Authentifizierungs- und Autorisierungsinformationen werden gelöscht.
