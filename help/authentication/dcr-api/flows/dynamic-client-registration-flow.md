---
title: Dynamischer Ablauf zur Kundenregistrierung
description: Dynamischer Ablauf zur Kundenregistrierung
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---


# Dynamischer Ablauf zur Kundenregistrierung {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die Implementierung der Dynamic Client Registration API wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

## Zugriff auf durch Adobe Pass geschützte APIs {#access-adobe-pass-protected-apis}

### Voraussetzungen {#prerequisites-access-adobe-pass-protected-apis}

Stellen Sie vor Zugriff auf durch Adobe Pass geschützte APIs sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Ein Client-Support-Mitarbeiter muss eine registrierte Anwendung erstellen, wie im Abschnitt [Registrierte Anwendungen verwalten](../dynamic-client-registration-overview.md#manage-registered-applications) beschrieben.
* Ein Client-Support-Mitarbeiter muss eine Softwareanweisung herunterladen und einbetten, wie im Abschnitt [Verwalten von Softwareanweisungen](../dynamic-client-registration-overview.md#manage-software-statements) beschrieben.

>[!IMPORTANT]
>
> Adobe Pass Authentication SDKs sind für das Abrufen und Aktualisieren der Client-Anmeldeinformationen und des Zugriffstokens im Namen der Clientanwendung verantwortlich.
> 
> Bei allen anderen Adobe Pass-geschützten APIs muss die Client-Anwendung den unten stehenden Workflow befolgen.

### Workflow {#workflow-access-adobe-pass-protected-apis}

Führen Sie die angegebenen Schritte aus, um auf Adobe Pass-geschützte APIs zuzugreifen, wie im folgenden Diagramm dargestellt.

![Zugriff auf durch Adobe Pass geschützte APIs](../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Zugriff auf durch Adobe Pass geschützte APIs*

1. **Abrufen von Client-Anmeldeinformationen:** Die Client-Anwendung erfasst alle erforderlichen Daten, um Client-Anmeldeinformationen abzurufen, indem sie den Client Register-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) .
   >
   > * Alle _erforderlichen_ Parameter, z. B. `software_statement`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Content-Type`, `X-Device-Info`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Zurückgeben von Client-Anmeldeinformationen:** Die Antwort des Client Register-Endpunkts enthält Informationen zu den Client-Anmeldeinformationen, die mit den empfangenen Parametern und Kopfzeilen verknüpft sind.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in der Antwort auf Client-Anmeldedaten finden Sie in der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) .
   >
   > <br/>
   >
   > Das Kundenregister validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert und zusätzliche Informationen bereitgestellt, die der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) entsprechen.

   >[!TIP]
   >
   > Empfehlung: Die Client-Anmeldeinformationen müssen zwischengespeichert werden und können unbegrenzt verwendet werden.

1. **Zugriffstoken abrufen:** Die Client-Anwendung erfasst alle erforderlichen Daten, um das Zugriffstoken abzurufen, indem sie den Client-Token-Endpunkt aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der API-Dokumentation [Zugriffstoken abrufen](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request) .
   >
   > * Alle _erforderlichen_ Parameter, wie `client_id`, `client_secret` und `grant_type`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Content-Type`, `X-Device-Info`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Zugriffstoken zurückgeben:** Die Client-Token-Endpunktantwort enthält Informationen zum Zugriffstoken, das mit den empfangenen Parametern und Kopfzeilen verknüpft ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen, die in einer Zugriffstoken-Antwort bereitgestellt werden, finden Sie in der API-Dokumentation zum [Abrufen des Zugriffstokens](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) .
   >
   > <br/>
   >
   > Das Client-Token validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert und zusätzliche Informationen bereitgestellt, die der API-Dokumentation [Zugriffstoken abrufen](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error) entsprechen.

   >[!TIP]
   >
   > Empfehlung: Das Zugriffstoken darf nur innerhalb der angegebenen Dauer zwischengespeichert und verwendet werden (z. B. 24 Stunden Live-Zeit). Nach dem Ablauf muss die Client-Anwendung ein neues Zugriffstoken anfordern.

1. **Fahren Sie mit dem Zugriff auf geschützte APIs fort:** Die Clientanwendung verwendet das Zugriffstoken, um auf andere durch Adobe Pass geschützte APIs zuzugreifen. Die Clientanwendung muss das Zugriffstoken im Anfrageheader `Authorization` mit dem `Bearer` Authentifizierungsschema (d. h. `Authorization: Bearer <access_token>`) enthalten.

   >[!IMPORTANT]
   >
   > Die durch Adobe Pass geschützten APIs überprüfen das Zugriffstoken, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Der _access_token_ muss gültig sein.
   > * Der _access_token_ muss mit einer gültigen _client_id_ und _client_secret_ verknüpft sein.
   > * Der _access_token_ muss mit einer gültigen _software_statement_ verknüpft sein.
   >
   > <br/>
   >
   > Wenn die Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../enhanced-error-codes.md) entsprechen.
