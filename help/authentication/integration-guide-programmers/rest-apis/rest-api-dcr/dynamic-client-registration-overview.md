---
title: Übersicht über die dynamische Client-Registrierung
description: Übersicht über die dynamische Client-Registrierung
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: fab5964aeb832d419702b41a6d3bc5676cb3354f
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 0%

---

# Übersicht über die dynamische Client-Registrierung {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Die dynamische Client-Registrierung stellt einen Autorisierungsmechanismus dar, der von [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) definiert wird und auf dem OAuth 2.0-Autorisierungs-Framework basiert, das von [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) beschrieben wird.

Adobe Pass bietet einen dynamischen Client-Registrierungs-Service, der den Zugriff auf die folgenden geschützten APIs ermöglicht:

* Adobe Pass Authentication Management-APIs:
   * [Temporäre Pass-API zurücksetzen](../../features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
   * [Abbauungs-API](../../features-premium/degraded-access/degradation-feature.md#degradation-api-access)
   * [Proxy MVPD API](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [Berechtigungs-Service-Überwachungs-API](../../features-premium/esm/entitlement-service-monitoring-api.md)
* Adobe Pass Authentication REST-APIs:
   * [REST-API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [(Legacy) REST-API v1](../../legacy/rest-api-v1/rest-api-reference.md)
* Adobe Pass-Authentifizierungs-SDKs:
   * [(Legacy) JavaScript SDK](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [(Legacy) iOS/tvOS SDK](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [(Legacy) Android SDK](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [(Legacy) FireOS SDK](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> Der Autorisierungsmechanismus für die dynamische Client-Registrierung ersetzt ältere Adobe Pass-Authentifizierungslösungen, die eingestellt werden müssen:
>
> * Der Mechanismus für die signierte Anforderer-ID.
> * Der Mechanismus zur Domain-Auflistung.
> * Der API-Schlüsselmechanismus.

Mit der Einführung der dynamischen Client-Registrierung sind die wichtigsten Vorteile:

* Verbesserte Sicherheit.
* Einheitliches Modell über Plattformen hinweg.
* Fein abgestimmte Steuerung des Lebenszyklus Ihrer Anwendung.

Weitere Informationen zur Verwaltung und Verwendung der dynamischen Client-Registrierung finden Sie in den folgenden Abschnitten.

## Verwaltung der dynamischen Client-Registrierung {#dynamic-client-registration-management}

Der dynamische Client-Registrierungs-Verwaltungsprozess ermöglicht es Client-Anwendungen, die auf bestimmten Plattformen ausgeführt werden und Zugriff auf bestimmte Adobe Pass-Authentifizierungs-APIs benötigen, sich über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) zu registrieren.

Das Adobe Pass TVE-Dashboard ist ein Tool für Adobe Pass-Authentifizierungskunden (Programmierer) zum Verwalten ihrer Konfiguration und Daten. Dieses Self-Service-Dashboard ermöglicht eine Reihe von Funktionen, die in der Dokumentation [Benutzerhandbuch für das Adobe Pass TVE-Dashboard](../../../user-guide-tve-dashboard/tve-dashboard-overview.md) beschrieben werden.

Wenn Sie Zugriff auf das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) haben, führen Sie die Schritte in den folgenden Abschnitten aus, um eine registrierte Anwendung zu erstellen und die Software-Anweisung herunterzuladen.

### Registrierte Anwendungen verwalten {#manage-registered-applications}

>[!IMPORTANT]
>
> Falls Sie keinen Zugriff auf das Adobe Pass TVE-Dashboard haben, erstellen Sie ein Ticket über unseren [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren Technical Account Manager (TAM), eine registrierte Anwendung zu erstellen und den Software-Kontoauszug mit Ihnen zu teilen.

Es gibt zwei Möglichkeiten, eine registrierte Anwendung zu erstellen:

* **Programmierebene**

  Der Registrierungsprozess auf Programmiererebene ermöglicht es Ihnen, eine registrierte Anwendung zu erstellen, die mit allen verfügbaren Kanälen oder einer ausgewählten Untergruppe von Kanälen verknüpft ist. Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard-Benutzerhandbuch für Programmierer](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md).


* **Kanalebene**

  Der Registrierungsprozess auf Kanalebene ermöglicht das Erstellen einer registrierten Anwendung, die nur mit dem aktuell ausgewählten Kanal verknüpft ist. Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard User Guide for Channels](../../../user-guide-tve-dashboard/tve-dashboard-channels.md).

>[!IMPORTANT]
>
> Es wird empfohlen, registrierte Anwendungen mit spezifischeren und eingeschränkteren Berechtigungen zu erstellen, um die Sicherheit zu erhöhen und nicht autorisierten Zugriff zu verhindern. Daher sollten Sie beim Erstellen registrierter Anwendungen die Verwendung engerer Optionen für die zugewiesenen `channels`, `platforms` und `scopes` in Betracht ziehen.
>
> Es wird empfohlen, für jedes größere Update Ihrer Client-Anwendung eine neue registrierte Anwendung zu erstellen, um deren Lebenszyklus und Nutzung zu verwalten. Erstellen Sie bei Bedarf ein Ticket über unseren [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren Technical Account Manager (TAM), eine registrierte Anwendung zu widerrufen, um die Funktionalität einer bestimmten Client-Anwendungsversion zu blockieren.

### Softwareanweisungen verwalten {#manage-software-statements}

>[!IMPORTANT]
>
> Falls Sie keinen Zugriff auf das Adobe Pass TVE-Dashboard haben, erstellen Sie ein Ticket über unseren [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren Technical Account Manager (TAM), eine registrierte Anwendung zu erstellen und den Software-Kontoauszug mit Ihnen zu teilen.

Stellen Sie vor dem Herunterladen einer Software-Anweisung sicher, dass Sie eine registrierte Anwendung erstellt haben, wie im Abschnitt [Verwalten registrierter Anwendungen](#manage-registered-applications) beschrieben, die Ihren Anforderungen an Client-Anwendungen entspricht.

Es gibt zwei Möglichkeiten, eine Software-Anweisung herunterzuladen, die auf der Ebene basiert, auf der die registrierte Anwendung erstellt wurde:

* **Programmierebene**

  Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard-Benutzerhandbuch für Programmierer](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md).

* **Kanalebene**

  Weitere Informationen finden Sie in der Dokumentation [TVE Dashboard User Guide for Channels](../../../user-guide-tve-dashboard/tve-dashboard-channels.md).

Die Software-Anweisung ist ein JSON Web Token (`JWT`), das Informationen zu Ihrer Client-Anwendungssoftware als Bundle enthält. Wenn die Software-Anweisung der [API zum Abrufen von Client](apis/dynamic-client-registration-apis-retrieve-client-credentials.md)Anmeldeinformationen präsentiert wird, wird sie mit JSON Web Signature (`JWS`) digital signiert.

Ausführlichere Erläuterungen dazu, was Software-Anweisungen sind und wie sie funktionieren, finden Sie in der Dokumentation [RFC 7591](https://tools.ietf.org/html/rfc7591).

## Dynamischer Client-Registrierungsfluss {#dynamic-client-registration-flow}

Zusammenfassend lässt sich sagen, dass der dynamische Client-Registrierungs-Autorisierungsmechanismus mehrere Schritte umfasst:

**Management**

* Ein Kundenbetreuer muss eine registrierte Anwendung erstellen, wie im Abschnitt [Verwalten registrierter Anwendungen](#manage-registered-applications) beschrieben.
* Ein Kundenbetreuer muss eine Software-Anweisung herunterladen und einbetten, wie im Abschnitt [Software-Anweisungen verwalten](#manage-software-statements) beschrieben.

**Fluss**

* Die Client-Anwendung muss die Client-Anmeldeinformationen abrufen, wie in der API[Dokumentation zum Abrufen von Client](apis/dynamic-client-registration-apis-retrieve-client-credentials.md)Anmeldeinformationen beschrieben.
* Die Client-Anwendung muss das Zugriffstoken abrufen, wie in der API-Dokumentation [Zugriffstoken abrufen](apis/dynamic-client-registration-apis-retrieve-access-token.md) beschrieben.

Weitere Informationen zum [ auf Adobe Pass-geschützte APIs finden ](flows/dynamic-client-registration-flow.md) in der Dokumentation zum Dynamic Client Registration Flow . Außerdem können Sie sich diese Aufzeichnung des [Webinars](https://my.adobeconnect.com/pzkp8ujrigg1/) ansehen, die mehr Kontext bietet und eine Demo enthält.
