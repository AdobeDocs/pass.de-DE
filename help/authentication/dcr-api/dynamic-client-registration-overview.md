---
title: Überblick über die dynamische Kundenregistrierung
description: Überblick über die dynamische Kundenregistrierung
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Überblick über die dynamische Kundenregistrierung {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Die dynamische Client-Registrierung stellt einen von [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) definierten Autorisierungsmechanismus dar und basiert auf dem OAuth 2.0-Autorisierungs-Framework, das von [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) beschrieben wird.

Adobe Pass bietet einen dynamischen Client-Registrierungsdienst, der den Zugriff auf die folgenden geschützten APIs ermöglicht:

* Adobe Pass Authentication Management-APIs:
   * [Zurücksetzen der API für temporären Pass](../reset-temp-pass.md)
   * [Abbau-API](../degradation-api-overview.md)
   * [Proxy-MVPD-API](../proxy-mvpd-webserv.md)
   * [API zur Überwachung von Entitätsdiensten](../entitlement-service-monitoring-api.md)
* Adobe Pass Authentication REST APIs:
   * [REST API V1](../rest-api-reference.md)
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
* Adobe Pass Authentication SDKs:
   * [JAVASCRIPT SDK](../javascript-sdk-api-reference.md)
   * [iOS/tvOS-SDK](../iostvos-sdk-api-reference.md)
   * [ANDROID SDK](../android-sdk-api-reference.md)
   * [FireOS-SDK](../amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> Der Mechanismus zur Autorisierung der dynamischen Client-Registrierung ersetzt ältere Adobe Pass-Authentifizierungslösungen, die eingestellt werden müssen:
>
> * Der Mechanismus für die ID des signierten Anforderers.
> * Der Mechanismus für die Domänenauflistung.
> * Der API-Schlüsselmechanismus.

Mit der Einführung einer dynamischen Kundenregistrierung sind die wichtigsten Vorteile:

* Verbesserte Sicherheit.
* Einheitliches Modell plattformübergreifend.
* Präzise Kontrolle über den Lebenszyklus Ihrer Anwendung.

Weitere Informationen zur Verwaltung und Verwendung der dynamischen Kundenregistrierung finden Sie in den folgenden Abschnitten.

## Dynamisches Client-Registrierungs-Management {#dynamic-client-registration-management}

Der dynamische Prozess zur Verwaltung der Clientregistrierung ermöglicht es Clientanwendungen, die auf bestimmten Plattformen ausgeführt werden und Zugriff auf bestimmte Adobe Pass-Authentifizierungs-APIs benötigen, um sich über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) zu registrieren.

Das Adobe Pass TVE-Dashboard ist ein Tool für Adobe Pass-Authentifizierungskunden (Programmierer) zur Verwaltung ihrer Konfiguration und Daten. Dieses Self-Service-Dashboard ermöglicht eine Reihe von Funktionen, die in der Dokumentation zum [Adobe Pass TVE Dashboard-Benutzerhandbuch](../tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md) beschrieben sind.

Falls Sie Zugriff auf das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) haben, führen Sie die folgenden Schritte aus, um eine registrierte Anwendung zu erstellen und die Softwareanweisung herunterzuladen.

### Registrierte Anwendungen verwalten {#manage-registered-applications}

>[!IMPORTANT]
>
> Wenn Sie keinen Zugriff auf das Adobe Pass TVE-Dashboard haben, erstellen Sie ein Ticket über unser [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren technischen Kundenbetreuer (TAM), eine registrierte Anwendung zu erstellen und den Softwareauszug für Sie freizugeben.

Es gibt zwei Möglichkeiten, eine registrierte Anwendung zu erstellen:

* **Programmiererebene**

  Der Registrierungsprozess auf Programmierebene ermöglicht die Erstellung einer registrierten Anwendung, die mit allen verfügbaren Kanälen oder einer ausgewählten Untergruppe von Kanälen verknüpft ist. Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard-Benutzerhandbuch für Programmierer](../tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md) .


* **Kanalebene**

  Mit der Registrierung auf Kanalebene können Sie eine registrierte Anwendung erstellen, die nur mit dem aktuell ausgewählten Kanal verknüpft ist. Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard-Benutzerhandbuch für Kanäle](../tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md) .

>[!IMPORTANT]
>
> Es wird empfohlen, registrierte Anwendungen mit spezifischeren und eingeschränkteren Berechtigungen zu erstellen, um die Sicherheit zu erhöhen und den unbefugten Zugriff zu verhindern. Daher sollten Sie beim Erstellen registrierter Anwendungen engere Optionen für die zugewiesenen `channels`, `platforms` und `scopes` verwenden.
>
> Es wird empfohlen, eine neue registrierte Anwendung für jede größere Aktualisierung Ihrer Clientanwendung zu erstellen, um deren Lebenszyklus und Nutzung zu verwalten. Erstellen Sie bei Bedarf ein Ticket über unser [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren technischen Kundenbetreuer (TAM), eine registrierte Anwendung zu widerrufen, um die Funktionalität einer bestimmten Client-Anwendungsversion zu blockieren.

### Verwalten von Softwareanweisungen {#manage-software-statements}

>[!IMPORTANT]
>
> Wenn Sie keinen Zugriff auf das Adobe Pass TVE-Dashboard haben, erstellen Sie ein Ticket über unser [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren technischen Kundenbetreuer (TAM), eine registrierte Anwendung zu erstellen und den Softwareauszug für Sie freizugeben.

Stellen Sie vor dem Herunterladen einer Softwareanweisung sicher, dass Sie eine registrierte Anwendung erstellt haben, wie im Abschnitt [Registrierte Anwendungen verwalten](#manage-registered-applications) beschrieben, die Ihren Anforderungen an die Clientanwendung entspricht.

Es gibt zwei Möglichkeiten, eine Softwareanweisung herunterzuladen, die auf der Ebene basiert, auf der die registrierte Anwendung erstellt wurde:

* **Programmiererebene**

  Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard-Benutzerhandbuch für Programmierer](../tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md) .

* **Kanalebene**

  Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard-Benutzerhandbuch für Kanäle](../tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md) .

Die Softwareanweisung ist ein JSON-Web-Token (`JWT`), das Informationen zu Ihrer Client-Anwendungs-Software als Bundle enthält. Wenn der API [Client-Anmeldeinformationen abrufen](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md) angezeigt wird, wird die Softwareanweisung digital mit JSON Web Signature (`JWS`) signiert.

Weitere Informationen dazu, welche Softwareanweisungen vorhanden sind und wie sie funktionieren, finden Sie in der Dokumentation zu [RFC 7591](https://tools.ietf.org/html/rfc7591) .

## Dynamischer Ablauf zur Kundenregistrierung  {#dynamic-client-registration-flow}

Zusammenfassend umfasst der dynamische Mechanismus zur Autorisierung der Kundenregistrierung mehrere Schritte:

**Management**

* Ein Client-Support-Mitarbeiter muss eine registrierte Anwendung erstellen, wie im Abschnitt [Registrierte Anwendungen verwalten](#manage-registered-applications) beschrieben.
* Ein Client-Support-Mitarbeiter muss eine Softwareanweisung herunterladen und einbetten, wie im Abschnitt [Verwalten von Softwareanweisungen](#manage-software-statements) beschrieben.

**Fluss**

* Die Clientanwendung muss die Client-Anmeldeinformationen abrufen, wie in der API-Dokumentation zum [Abrufen von Client-Anmeldeinformationen](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md) beschrieben.
* Die Clientanwendung muss das Zugriffstoken abrufen, wie in der API-Dokumentation [Zugriffstoken abrufen](./apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben.

Weitere Informationen zum Zugriff auf durch Adobe Pass geschützte APIs finden Sie in der Dokumentation zum [Fluss zur dynamischen Client-Registrierung](./flows/dynamic-client-registration-flow.md) . Außerdem können Sie sich diese [Webinar](https://my.adobeconnect.com/pzkp8ujrigg1/)-Aufzeichnung ansehen, die mehr Kontext bietet und eine Demo enthält.
