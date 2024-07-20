---
title: Grundlegendes zu Benutzer-IDs
description: Grundlegendes zu Benutzer-IDs
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 0%

---

# Grundlegendes zu Benutzer-IDs {#understanding-user-ids}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Grundsätzlich ist jeder Benutzer, der einen Berechtigungsfluss initiiert, einer einzelnen eindeutigen Benutzer-ID zugeordnet. Im Zuge eines Berechtigungsablaufs kann jedoch eine Benutzer-ID auf unterschiedliche Weise angezeigt werden, je nachdem, von welcher API Sie die ID erhalten.

Die `sessionGUID` im Token für kurze Medien ist die sichere Form der Benutzer-ID, die über den Aufruf `sendTrackingData()` verfügbar ist. In allen aktuellen Integrationen ist dies eine beständige GUID für den Benutzer über Zeit und Geräte hinweg. Die Quelle der GUID beginnt mit der Benutzer-ID aus der Authentifizierungsantwort, die vom MVPD stammt. Einige MVPDs könnten sich jedoch in Zukunft anders entscheiden und eine vorübergehende GUID senden. Wenn ein Programmierer sicherstellen möchte, dass die MVPD-Quell-Benutzer-ID in der AuthN-Antwort persistent ist, sollte er dies in seinen Vereinbarungen mit MVPDs anordnen.

Im Folgenden finden Sie die verschiedenen Möglichkeiten, wie die Benutzer-ID in den Adobe Pass-Authentifizierungs-APIs dargestellt wird:

| Eigenschaft | Zweck | Hash | Digital signiert | Beschreibung |
| --- | --- | --- | --- | --- |
| sendTrackingData() GUID-Eigenschaft | Tracking/Analytics | Ja | Nein | - Die MVPD-Benutzer-ID, gehasht durch Adobe. Die Benutzer-ID kann nicht zurück zur Quelle zum MVPD verfolgt werden. </br> </br> - Diese Form der ID ist nicht digital signiert, daher ist sie für die Betrugsvorbeugung nicht sicher. Es ist jedoch ausreichend für Analysen.  </br> </br> - Dieses Formular der Benutzer-ID wird clientseitig für alle Ereignisse bereitgestellt, die die Adobe Pass-Authentifizierung im AuthN/AuthZ-Fluss generiert. |
| sessionGUID-Eigenschaft des Short Media Token | Betrugsverfolgung der gleichzeitigen Nutzung | Ja | Ja | - Dies entspricht der Benutzer-ID über sendTrackingData(). Diese ist jedoch digital signiert, um ihre Integrität zu schützen, und eignet sich gut genug für die Verfolgung von Betrug. </br> </br> - Sie soll nach Verwendung unserer Validator-Bibliothek serverseitig verarbeitet und auf Betrugsmuster analysiert werden, bevor der Video-Stream an den Client freigegeben wird.  Die Durchführung dieser Aufgaben obliegt dem Programmierer. |
| getMetadata() userID-Eigenschaft | Kontoverknüpfung, Betrugsuntersuchung mit MVPD | Nein | Nein | - Diese Eigenschaft ermöglicht es Adobe, die eigentliche MVPD-Benutzer-ID für den Programmierer verfügbar zu machen. </br> </br> - In der Adobe-Konfiguration kann sie als verschlüsselt oder nicht festgelegt werden (je nach MVPD-Voreinstellung). Wenn sie verschlüsselt ist, wird sie mit dem öffentlichen Schlüssel aus dem dem Adobe bereitgestellten Programmerzertifikat verschlüsselt, sodass sie dem Client nicht eindeutig angezeigt wird. </br> </br> - Dadurch erhält der Programmierer die tatsächliche Benutzer-ID aus dem MVPD, sodass sie direkt mit dem MVPD für die Kontoverknüpfung oder Betrugsuntersuchung verwendet werden kann. |


**Schlussfolgerung**

* Im Allgemeinen stellt der MVPD eine beständige eindeutige ID <u>bereit und übergibt sie bei erfolgreicher Authentifizierung an Adobe.</u> Sie ist im Allgemeinen in allen Netzwerken konsistent. Die Ausnahme ist Comcast MVPD, das für jeden Kanal eine andere Benutzer-ID bereitstellt.

* Die MVPD-Benutzer-ID enthält keine personenbezogenen Daten und ist KEINE Kontonummer. Es muss nicht in verschlüsselter Form offen gelegt werden, da wir mit allen MVPDs überprüft haben, dass keine PII gesendet werden.

Wie Sie die Benutzer-ID verwenden, hängt vom Anwendungsfall ab:

* Wenn Sie es für Tracking/Analysen benötigen, ist es am praktischsten, es von `sendTrackingData()` zu erhalten.
* Wenn Sie es Server-seitig für Stream-Veröffentlichung, Betrug oder operative Daten benötigen, können Sie es vom Media Token-Validator abrufen.
* Wenn Sie es für Kontoverknüpfung und tieferen Betrug benötigen, wenden Sie sich an Ihren Adobe-Ansprechpartner, um die Verfügbarkeit zu erhalten.
