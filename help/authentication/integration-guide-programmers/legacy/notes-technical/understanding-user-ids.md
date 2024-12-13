---
title: Grundlegendes zu Benutzer-IDs
description: Grundlegendes zu Benutzer-IDs
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# (veraltet) Kennungen von Benutzern {#understanding-user-ids}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Konzeptionell wird jeder Benutzer, der einen Berechtigungsfluss initiiert, mit einer einzelnen, eindeutigen Benutzer-ID verknüpft. Im Verlauf eines Berechtigungsflusses kann diese eine Benutzer-ID jedoch auf unterschiedliche Weise dargestellt werden, je nachdem, von welcher API Sie die ID erhalten.

Der `sessionGUID` im Short Media Token ist die sichere Form der Benutzer-ID, die über den `sendTrackingData()`-Aufruf verfügbar ist. Bei allen aktuellen Integrationen ist dies eine persistente GUID für den -Benutzer über Zeit und Geräte hinweg. Die Quelle der GUID beginnt mit der Benutzer-ID aus der Authentifizierungsantwort von MVPD. Einige MVPDs könnten jedoch in Zukunft ihre Meinung ändern und eine vorübergehende GUID senden. Wenn ein Programmierer sicherstellen möchte, dass die MVPD-Quell-Benutzer-ID in der AuthN-Antwort persistent ist, sollte er dies in seinen Vereinbarungen mit MVPDs arrangieren.

Im Folgenden werden die verschiedenen Arten der Darstellung der Benutzer-ID in den Adobe Pass-Authentifizierungs-APIs beschrieben:

| Eigenschaft | Zweck | Hash | digital signiert | Beschreibung |
| --- | --- | --- | --- | --- |
| sendTrackingData()-GUID-Eigenschaft | Tracking/Analyse | Ja | Nein | - Die MVPD-Benutzer-ID, gehasht von Adobe. Die Benutzer-ID kann nicht zurück zur Quelle bis zum MVPD verfolgt werden. </br> </br> - Diese Form der ID ist nicht digital signiert, sodass sie für die Betrugsbekämpfung nicht sicher ist. Es ist jedoch gut genug für Analysen.  </br> </br> : Diese Form der Benutzer-ID wird Client-seitig für alle Ereignisse bereitgestellt, die von der Adobe Pass-Authentifizierung im AuthN-/AuthZ-Fluss generiert werden. |
| SessionGUID-Eigenschaft des Short Media Token | Verfolgung von Betrug bei gleichzeitiger Nutzung | Ja | Ja | - Dies ist dasselbe wie die Benutzer-ID über sendTrackingData(), diese ist jedoch digital signiert, um ihre Integrität zu schützen, und ist gut genug, um für das Betrugs-Tracking verwendet zu werden. </br> </br> - Sie sollte nach Verwendung unserer Validator-Bibliothek Server-seitig verarbeitet werden und kann auf Betrugsmuster analysiert werden, bevor der Video-Stream an den Client freigegeben wird.  Die Ausführung einer dieser Aufgaben liegt beim Programmierer. |
| getMetadata() userID-Eigenschaft | Kontoverknüpfung, Betrugsermittlung mit MVPD | Nein | Nein | - Diese Eigenschaft ermöglicht es Adobe, dem Programmierer die tatsächliche Quell-MVPD-Benutzer-ID anzuzeigen. </br> </br> - In der Adobe-Konfiguration kann sie als verschlüsselt oder nicht festgelegt werden (je nach MVPD-Voreinstellung). Wenn er verschlüsselt wird, wird er mit dem öffentlichen Schlüssel aus dem dem Adobe bereitgestellten Programmiererzertifikat verschlüsselt, sodass er für den Client nicht in übersichtlicher Form verfügbar gemacht wird. </br> </br> - Dadurch erhält der Programmierer die tatsächliche Benutzer-ID aus der MVPD, sodass sie für die Kontoverknüpfung oder Betrugsermittlung direkt mit der MVPD verwendet werden kann. |


**Zum Schluss**

* Im Allgemeinen stellt die MVPD eine persistente eindeutige ID bereit <u>und übergibt sie bei erfolgreicher Authentifizierung an Adobe</u>. Sie ist im Allgemeinen über alle Netzwerke hinweg konsistent. Die Ausnahme bildet Comcast MVPD, das für jeden Kanal eine andere Benutzer-ID bereitstellt.

* Die MVPD-Benutzer-ID enthält keine personenbezogenen Daten und ist KEINE Kontonummer. Es muss nicht in verschlüsselter Form verfügbar gemacht werden, da wir mit allen MVPDs validiert haben, dass keine PII gesendet werden.

Wie Sie die Benutzer-ID verwenden, hängt vom Anwendungsfall ab:

* Wenn Sie es für Tracking/Analysen benötigen, ist der praktischste Ort, es von `sendTrackingData()` zu erhalten.
* Wenn Sie sie Server-seitig für Stream-Freigabe, Betrug oder Betriebsdaten benötigen, können Sie sie vom Media Token Validator erhalten.
* Wenn Sie es für Kontoverknüpfung und tieferen Betrug benötigen, fragen Sie Ihren Adobe-Ansprechpartner nach der Verfügbarkeit.
