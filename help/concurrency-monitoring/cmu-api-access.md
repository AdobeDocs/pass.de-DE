---
title: CMU-API-Zugriff
description: CMU-API-Zugriff
exl-id: 8d216703-aabc-489e-93fe-d4d105616b1d
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 0%

---

# API-Zugriff zur Überwachung der gleichzeitigen Nutzung {#cmu-api-usage-access}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig. Bei Fragen zur Verfügbarkeit wenden Sie sich an Ihren Adobe-Support.

## Übersicht über das Zugriffsverfahren {#api-access-procedure-overview}

Wir haben den Zugriff auf CMU-Berichte aktualisiert, damit er mit dem dynamischen Client-Registrierungsprotokoll von OAuth 2.0 kompatibel ist. Ein benutzerdefinierter OAuth 2.0-Autorisierungs-Server wird bereitgestellt, um die Anforderungen der Anwendung zur Parallelitätsüberwachung zu erfüllen. \
Damit die Client-Anwendungen die OAuth 2.0-Autorisierung nutzen können, muss sich der Server dynamisch registrieren, um bestimmte Informationen (Client-Anmeldeinformationen) zu erhalten, damit er damit interagieren kann. Im Rahmen des Registrierungsprozesses muss der Client dem Client-Registrierungsendpunkt einen Satz integrierter Metadaten vorlegen.
Diese Metadaten werden als Software-Anweisung übermittelt, die eine „software_id“ enthält, damit unser Autorisierungs-Server verschiedene Instanzen einer Anwendung mit derselben Software-Anweisung korrelieren kann.
Eine Software-Anweisung ist ein JSON Web Token (JWT), das Metadatenwerte über die Client-Software als Bundle angibt. Wenn die Anweisung im Rahmen einer Client-Registrierungsanfrage an den Autorisierungs-Server gesendet wird, muss sie mit JSON Web Signature (JWS) digital signiert oder mit einem Mac versehen werden. \
Eine detailliertere Erläuterung der Software-Aussagen und ihrer Funktionsweise finden Sie in der offiziellen Dokumentation <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a>.
Gehen Sie wie in den folgenden Abschnitten beschrieben vor, um Zugriff zu erhalten.

## Schritte des Zugriffsprozesses {#access-procedure-steps}

1. Eine registrierte Anwendung muss sich auf dem Adobe Pass DCR-Server befinden. Wenden Sie sich für diesen Schritt an unser [Support-Team](mailto:tve-support@adobe.com).

2. Abrufen der Software-Anweisung
   1. Zum [Adobe Pass TVE-Dashboard](https://experience.adobe.com/#/pass/authentication)
   2. Programmierer auswählen
   3. Wechseln Sie zur *Registered Applications* Registerkarte
   4. Anwendung auswählen
   5. Klicken Sie in der registrierten Anwendungszeile, für die Sie eine Software-Anweisung abrufen möchten, auf Download und speichern Sie sie als Datei auf Ihrem lokalen Computer
      <figure>
          <img src="assets/programmer-download-software-statement-button.png"
               alt="SOFTWAREANWEISUNG HERUNTERLADEN">
      </figure>

      <figure>
          <img src="assets/software_statement_2.png"
               alt="Software-Anweisungsbeispiel">
      </figure>

3. Zugriffstoken abrufen
   1. Rufen Sie die Anmeldeinformationen des Clients mithilfe der oben abgerufenen Software-Anweisung ab und führen Sie den folgenden Aufruf aus. Auf diese Weise wird ein client_id - client_secret-Paar abgerufen, das zum Abrufen des Zugriffstokens verwendet werden kann.
      *Dieser Schritt sollte nicht jedes Mal ausgeführt werden. Dies sollte nur dann erneut erfolgen, wenn die Anmeldeinformationen ablaufen.*
      <figure>
          <img src="assets/dcr_request_1_get_client_credentials.png"
               alt="Client-Anmeldedaten abrufen">
       </figure>

   2. Rufen Sie das Zugriffs-Token mithilfe des folgenden Aufrufs ab. Verwenden Sie dieses Zugriffstoken, um eine beliebige CMU-API aufzurufen, bis das Token abläuft.
      *Dieser Schritt sollte nur ausgeführt werden, wenn das zuletzt generierte Token abgelaufen ist.*
      <figure>
          <img src="assets/dcr_get_access_token_call.png"
               alt="Zugriffstoken abrufen">
       </figure>

4. Aufrufen der CMU-API - siehe zugehörige Informationen unten.
   <figure>
          <img src="assets/call_cmu_reports_sample.png"
               alt="CMU-API aufrufen">
       </figure>

## Ergänzende Informationen {#related-information}

* [Übersicht über die Kapitalmarktunion](/help/concurrency-monitoring/cm-usage-reports.md)
* [CMU-API](/help/concurrency-monitoring/cmu-api.md)
