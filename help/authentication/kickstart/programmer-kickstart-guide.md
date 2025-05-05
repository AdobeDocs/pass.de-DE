---
title: Anleitung für Programmierer
description: Anleitung für Programmierer
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 37858fa83aecbdf443a4a6058c78e4f9246eee42
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Anleitung für Programmierer {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Dieses Schnellstartanleitung richtet sich an Inhaltsanbieter (Programmierer), die beabsichtigen, die Adobe® Pass-Authentifizierung in ihre Websites oder Programme zu integrieren.

In diesem Dokument werden die wichtigsten ersten Schritte für einen reibungslosen und effizienten Start des Integrationsprozesses beschrieben. Sie soll die Erwartungen verdeutlichen und Anleitungen dazu geben, wie wir mit Partnern zusammenarbeiten werden, um erfolgreiche Integrationen zu erreichen.

Adobe bietet eine Reihe von Ressourcen, mit denen Sie die Adobe Pass-Authentifizierung in Ihre Website oder Ihr Programm integrieren können. Bitte beachten Sie die **„You will provide“** und **&quot;Adobe will provide“** Erwähnungen aus jedem folgenden Abschnitt.

## Einrichtungsprozess {#setup-process}

Der Einrichtungsprozess umfasst unter anderem die folgenden Schritte:

![Adobe® Authentifizierungs-Integrationsprozess erfolgreich](../assets/progr-flow-int-lifecycle.png)

*Adobe® Authentifizierungs-Integrationsprozess erfolgreich*

**Sie geben** in der Kickoff-Phase Folgendes an:

* **Dienstleister (Anfordererkennung)**

  Dies ist eine Zeichenfolge, die die Marke der Website oder der Anwendung, die Anfragen an die Adobe Pass-Authentifizierung sendet, eindeutig identifiziert. Die Zeichenkette selbst ist willkürlich, muss aber zwischen Adobe und dem Programmierer vereinbart werden

* **Kanalinformationen**

  Hierbei handelt es sich um einen Satz von Zeichenfolgen, mit denen die vom Dienstleister angeforderten Inhaltskanäle identifiziert werden. In vielen Fällen sind Kanal und Dienstleister identisch. Eine einzelne Kennung kann jedoch mehrere Inhaltskanäle darstellen. Diese Kanalnamenszeichenfolgen sollten an die entsprechenden Kabel-TV-Kanäle angepasst sein. Beachten Sie, dass einige MVPDs diesen Wert während des Authentifizierungs- und/oder Autorisierungsprozesses überprüfen können.

* **Domain-Namen**

  Diese Liste enthält die tatsächlichen Domain-Namen, die als Adobe für den Dienstleister aufgeführt sind. Dadurch wird sichergestellt, dass nur Ihre autorisierten Domains über Ihre Metadaten auf die Adobe Pass-Authentifizierung zugreifen können. Stellen Sie sicher, dass Sie Domain-Namen sowohl für Produktions- als auch für Staging-(Test-)Umgebungen bereitstellen und klar identifizieren, da diese unterschiedlich sein können.

**Sie stellen Folgendes** MVPD bereit:

* **Berechtigungssätze**

  Hierbei handelt es sich um Anmeldeinformationen, die zur Authentifizierung und Autorisierung oder nur zur Authentifizierung des Benutzers bei der MVPD verwendet werden. Normalerweise bestehen diese Anmeldeinformationen aus einem Benutzernamen und einem Kennwort, die Ihnen von MVPD für beide Profile (Staging und Produktion) bereitgestellt werden.

* **Ressourcenkennungen**

  Dies sind eindeutige Kennungen für die Inhaltskanäle, Sendungen, Folgen oder Assets, die der Dienstleister schützen möchte. Diese Kennungen werden verwendet, um Autorisierungsentscheidungen anzufordern, und müssen mit der MVPD vereinbart werden.

>[!IMPORTANT]
>
> Der Programmierer ist dafür verantwortlich, sich mit der MVPD abzustimmen, um alle notwendigen Geschäftsvereinbarungen abzuschließen. In der Zwischenzeit arbeitet die Adobe Pass-Authentifizierung mit der MVPD zusammen, um sicherzustellen, dass die technische Integration ordnungsgemäß eingerichtet ist:
>
> * **New MVPD**
>
>     Wenn der MVPD nicht mit Adobe integriert ist, muss benutzerdefinierter Code basierend auf den MVPD-spezifischen Anforderungen entwickelt werden. Bis zum Abschluss dieser Entwicklung ist die MVPD nicht verfügbar und die Produkttests mit dieser MVPD können nicht fortgesetzt werden.
>
> * **Vorhandene MVPD**
>
>     Wenn die MVPD bereits mit Adobe integriert ist, ist der Konnektivitätsprozess erheblich optimiert. In den meisten Fällen kann die Konnektivität schnell durch Konfigurationsanpassungen statt durch umfangreiche Entwicklungen hergestellt werden.
>
> Alle Integrationen erfordern gemeinsame Bemühungen zur Qualitätssicherung (QA), einschließlich Tests durch den MVPD, da der Endbenutzer letztendlich Kunde von MVPD ist. Die Koordination von Testzyklen hängt oft von der Ressourcenverfügbarkeit von MVPD ab, was zu potenziellen Verzögerungen führen kann.

## Zugriff auf den Support {#access-customer-support}

**Adobe bietet** Zugriff auf unser Kundensupportsystem über [Zendesk](https://tve.zendesk.com/home). Um auf Zendesk zugreifen zu können, müssen Sie sich registrieren und ein Konto unter https://tve.zendesk.com/home erstellen. Die Anzahl der Benutzer, die Sie registrieren können, ist unbegrenzt. Nach der Registrierung können Sie Kommentare zu jedem gesendeten Ticket anzeigen und freigeben.

Das Adobe Pass-Authentifizierungsteam steht Ihnen bei allen Fragen oder technischen Problemen zur Verfügung, die während des Integrationsprozesses auftreten können. Bitte kontaktieren Sie uns unter [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Zugriff auf die Dokumentation {#access-documentation}

**Adobe bietet** Zugriff auf unsere öffentliche Dokumentation über [Adobe Experience League](https://experienceleague.adobe.com/de/docs/pass/authentication/home).

Das Adobe Pass-Authentifizierungs-Team bietet eine umfassende Dokumentation zu den verfügbaren Funktionen und APIs im Abschnitt [Integrationshandbuch für Programmierer](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md). Links zu detaillierten Informationen zu den einzelnen Themen finden Sie im Inhaltsverzeichnis unter diesem Abschnitt .

## Zugriff auf das Test-Tool {#access-testing-tool}

**Adobe bietet** Zugriff auf unser API-Explorations-Tool über die [Adobe Developer](https://developer.adobe.com/adobe-pass/)-Website.

## Zugriff auf das Konfigurationsverwaltungs-Tool {#access-configuration-management-tool}

**Adobe bietet** Zugriff auf ein Self-Service-Tool zur Verwaltung Ihrer Konfiguration und Daten über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/pass/authentication).

Das Adobe Pass-Authentifizierungsteam bietet eine umfassende Dokumentation zur Verwendung des TVE-Dashboards im Abschnitt [Benutzerhandbuch für TVE-Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md). Links zu detaillierten Informationen zu den einzelnen Themen finden Sie im Inhaltsverzeichnis unter diesem Abschnitt .
