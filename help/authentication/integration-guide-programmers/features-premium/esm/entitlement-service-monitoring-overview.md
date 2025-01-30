---
title: Übersicht über die Überwachung des Berechtigungsdienstes
description: Übersicht über die Überwachung des Berechtigungsdienstes
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 0%

---

# Übersicht über die Überwachung des Berechtigungsdienstes {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Einführung {#introduction}

TVE-Sites und -Apps müssen rund um die Uhr verfügbar sein, sodass Kunden Echtzeiteinblicke in Berechtigungsereignisse benötigen, um Probleme so schnell wie möglich zu erkennen und zu beheben. Sie müssen außerdem monatliche Daten analysieren, um festzustellen, welche Plattformen den Großteil des Traffics bereitstellen und welche Plattformen möglicherweise eine schlechte Implementierung und schlechte Konversionsraten aufweisen.

Die Überwachung des Berechtigungs-Services (Entitlement Service Monitoring, ESM) bietet Programmierern und MVPDs einen Daten-Feed, der Echtzeit-Einblick in ihre Authentifizierungs- und Autorisierungsereignisse bietet. Die Daten werden aus den Adobe Pass-Authentifizierungssystemen erfasst und über eine RESTful-API bereitgestellt.  Kunden können die Daten direkt oder aus ihren eigenen benutzerdefinierten operativen Dashboards nutzen.

Die Kernelemente des ESM-Systems sind seine Metriken und Dimensionen. ESM generiert Berichte, die aggregierte Metriken entsprechend der Dimensionsauswahl enthalten. Da Adobe Pass-Ereignisse in der PST-Zeitzone protokolliert werden, sind die ESM-Berichte auch in der PST-Zeitzone verfügbar.

Die ESM-API ist nicht allgemein verfügbar.  Bei Fragen zur Verfügbarkeit wenden Sie sich an Ihren Adobe-Support.

## ESM für Programmierer {#esm-for-programmers}

### Programmierer können die folgenden Metriken überwachen: {#programmers-monitor-metrics}


| *Metrikname* | *Beschreibung* |
|-------------------------|--------------------------|
| auth-attempts | Anzahl der initiierten Authentifizierungsflüsse |
| Auth-erfolgreich | Anzahl der von Clients erfolgreich abgerufenen Authentifizierungs-Token |
| Auth-ausstehend | Anzahl der erfolgreich generierten Authentifizierungs-Token (unabhängig davon, ob der Client sie tatsächlich erhalten hat oder nicht) |
| Auth-fehlgeschlagen | Anzahl fehlgeschlagener Authentifizierungen, die über ein externes System durchgeführt wurden. |
| clientless-token | Anzahl der erfolgreich ausgegebenen Client-losen Token |
| clientless-failure | Anzahl fehlgeschlagener Versuche, Token von der Clientless-API zu erhalten |
| auth-attempts | Anzahl der Genehmigungsversuche |
| auth-successful | Anzahl erfolgreicher Autorisierungen |
| Autorisierung fehlgeschlagen | Anzahl der verweigerten Genehmigungen durch MVPDs auf Anwendungsebene |
| Auth-Rejected | Anzahl der Autorisierungsversuche, die vom Adobe-Dienstleister als bösartig eingestuft und infolge der Verhinderung von DoS-Angriffen zurückgewiesen wurden |
| Auth-Latenz | Gesamtzahl der am Endpunkt von MVPD verbrachten Millisekunden |
| media-token | Anzahl der generierten Short-Media-Token (die der Anzahl der Wiedergabeanfragen entsprechen) |
| unique-accounts | Anzahl der eindeutigen Benutzer, die im ausgewählten Zeitintervall Berechtigungsaktionen (AuthN/AuthZ) ausgeführt haben. (Diese Metrik wird nur angezeigt, wenn Tageswerte angefordert werden.) </br> Wird für jedes einzelne Rechenzentrum berechnet. Wenn die Dimension „dc“ nicht angefordert wird, wird diese Metrik nicht angezeigt. |
| unique-sessions | Anzahl der eindeutigen Sitzungen, die innerhalb des ausgewählten Zeitintervalls Authentifizierungsflussaufrufe an den Adobe Pass-Authentifizierungsdienst durchgeführt haben. (Diese Metrik wird nur angezeigt, wenn Tageswerte angefordert werden.) </br> Wird für jedes einzelne Rechenzentrum berechnet. Wenn die Dimension „dc“ nicht angefordert wird, wird diese Metrik nicht angezeigt. |
| Zählung | Ein einfacher Zähler, der in den ereignisorientierten Berichten verwendet wird |

</br>

### Programmierer können die oben aufgeführten Metriken nach den folgenden Dimensionen filtern: {#progr-filter-metrics}


| *Name der Dimension* | *Beschreibung* |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Jahr | Das 4-stellige Jahr |
| Monat | Der Monat des Jahres (1-12) |
| Tag | Der Tag des Monats (1-31) |
| Stunde | Die Stunde des Tages |
| Minute | Die Minute der Stunde |
| Medienunternehmen | Das Medienunternehmen, das Eigentümer der Website ist und das den Berechtigungsprozess für den Benutzer initiiert hat |
| DC | (Rechenzentrum) Die Heimatregion, in der die Anfrage empfangen wurde. |
| Vollmacht | Der Proxy-MVPD (der bei direkten Integrationen „direkt“ ist) |
| mvpd | Die MVPD, die für die Gewährung der Berechtigung an den Benutzer verantwortlich ist |
| Requestor-id | Die Anforderer-ID, die zum Ausführen der Berechtigungsanfrage verwendet wird |
| Kanal | Die Kanal-Website, extrahiert aus dem Ressourcenfeld (extrahiert aus der MRSS-Payload als Kanal/Titel, falls angegeben, oder dem Ressourcenwert zugeordnet, falls er nicht im RSS-Format vorliegt). |
| resource-id | Der tatsächlich an der Autorisierungsanfrage beteiligte Ressourcentitel (extrahiert aus der MRSS-Payload als Element/Titel, falls angegeben) |
| Gerät | Die Geräteplattform (PC, Mobilgerät, Konsole usw.) |
| EAP | Der externe Authentifizierungsanbieter, wenn der Authentifizierungsfluss über ein externes System ausgeführt wird. </br> Die Werte können sein: </br> - Nicht zutreffend - Die Authentifizierung wurde von Adobe Pass Authentication </br> bereitgestellt - Apple - Das externe System, das die Authentifizierung bereitgestellt hat, ist Apple |
| Betriebssystemfamilie | Auf dem Gerät ausgeführtes Betriebssystem |
| Browserfamilie | Benutzeragent, der für den Zugriff auf die Adobe Pass-Authentifizierung verwendet wird |
| CDT | Die Geräteplattform (alternativ), die derzeit für Clientless verwendet wird. </br> Die Werte können sein: </br> - Nicht zutreffend - Das Ereignis stammt nicht von einem Client-losen SDK-</br> - Nicht bekannt - Da der deviceType-Parameter von einer Client-losen API optional ist, gibt es Aufrufe, die keinen Wert enthalten. </br> : jeder andere Wert, der über die Clientless-API gesendet wurde, z. B. xbox, appleTV, roku usw. </br> |
| platform-version | Die Version der Client-losen SDK |
| Betriebssystemtyp | Betriebssystem, das auf dem Gerät ausgeführt wird, alternativ (derzeit nicht verwendet) |
| browser-version | Version des Benutzeragenten |
| NSDK | Der verwendete Client-SDK (Android, FireTV, js, iOS, tvOS, Nicht-SDK) |
| nsdk-version | Die Version des Adobe Pass-Authentifizierungs-Clients SDK |
| Ereignis | Der Ereignisname für die Adobe Pass-Authentifizierung |
| Grund | Der Grund für die Fehler, wie von der Adobe Pass-Authentifizierung gemeldet |
| SSO-Typ | Der zugrunde liegende SSO-Mechanismus: platform/passive/adobe. Gibt an, dass das Autorisierungs-Token ausgegeben wurde, indem die AuthN in einer anderen Anwendung wiederverwendet wurde |
| Plattform | Das Gerät hat die Plattform identifiziert. Mögliche Werte: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc |
| application-name | Der im TVE-Dashboard konfigurierte Anwendungsname für die zum Verwenden konfigurierte DCR-registrierte Anwendung. |
| application-version | Die im TVE-Dashboard konfigurierte Anwendungsversion für die zum Verwenden konfigurierte DCR-registrierte Anwendung. |
| customer-app | Die benutzerdefinierte Anwendungs-ID, die über [Geräteinformationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md) übergeben wird. |
| content-category | Die Kategorie des Inhalts, der von Ihrer Anwendung angefordert wird. |

## ESM für MVPD {#esm-for-mvpds}

### MVPDs können die folgenden Metriken überwachen:

| *Metrikname* | *Beschreibung* |
|---|---|
| auth-attempts | Anzahl der initiierten Authentifizierungsflüsse |
| Auth-erfolgreich | Anzahl der von Clients erfolgreich abgerufenen Authentifizierungs-Token |
| Auth-ausstehend | Anzahl der erfolgreich generierten Authentifizierungs-Token (unabhängig davon, ob der Client sie tatsächlich erhalten hat oder nicht) |
| Auth-fehlgeschlagen | Anzahl fehlgeschlagener Authentifizierungen, die über ein externes System durchgeführt wurden. |
| auth-attempts | Anzahl der Genehmigungsversuche |
| auth-successful | Anzahl erfolgreicher Autorisierungen |
| Autorisierung fehlgeschlagen | Anzahl der verweigerten Genehmigungen durch MVPDs auf Anwendungsebene |
| Auth-Rejected | Anzahl der Autorisierungsversuche, die vom Adobe-Dienstleister als bösartig eingestuft und infolge der Verhinderung von DoS-Angriffen zurückgewiesen wurden |
| Auth-Latenz | Gesamtzahl der am Endpunkt von MVPD verbrachten Millisekunden |

### MVPDs können die oben aufgeführten Metriken nach den folgenden Dimensionen filtern:

| *Name der Dimension* | *Beschreibung* |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Jahr | Das 4-stellige Jahr |
| Monat | Der Monat des Jahres (1-12) |
| Tag | Der Tag des Monats (1-31) |
| Stunde | Die Stunde des Tages |
| Minute | Die Minute der Stunde |
| mvpd | Die mvpd-ID, die zum Ausführen der Berechtigungsanfrage verwendet wird |
| Requestor-id | Die Anforderer-ID, die zum Ausführen der Berechtigungsanfrage verwendet wird |
| EAP | Der externe Authentifizierungsanbieter, wenn der Authentifizierungsfluss über ein externes System ausgeführt wird. </br> Die Werte können sein: </br> - Nicht zutreffend - Die Authentifizierung wurde von Adobe Pass Authentication </br> bereitgestellt - Apple - Das externe System, das die Authentifizierung bereitgestellt hat, ist Apple |
| CDT | Die Geräteplattform (alternativ), die derzeit für Clientless verwendet wird. </br> Die Werte können sein: </br> - Nicht zutreffend - Das Ereignis stammt nicht von einem Client-losen SDK-</br> - Nicht bekannt - Da der deviceType-Parameter von einer Client-losen API optional ist, gibt es Aufrufe, die keinen Wert enthalten. </br> : jeder andere Wert, der über die Clientless-API gesendet wurde, z. B. xbox, appleTV, roku usw. </br> |
| sdk-type | Die verwendete Client-SDK (Flash, HTML5, nativ in Android, iOS, clientless usw.) |
| Plattform | Das Gerät hat die Plattform identifiziert. Mögliche Werte: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc |
| NSDK | Der verwendete Client-SDK (Android, FireTV, js, iOS, tvOS, Nicht-SDK) |
| nsdk-version | Die Version des Adobe Pass-Authentifizierungs-Clients SDK |

## Anwendungsfälle {#use-cases}

Sie können die ESM-Daten für die folgenden Anwendungsfälle verwenden:

- **Monitoring** - Ops- oder Überwachungs-Teams können ein Dashboard oder Diagramm erstellen, das die API jede Minute aufruft. Anhand der angezeigten Informationen können sie ein Problem (bei der Adobe Pass-Authentifizierung oder bei einem MVPD) in dem Moment erkennen, in dem es angezeigt wird.

- **Debugging/Qualitätstests** Da die Daten auch nach Plattform, Gerät, Browser und Betriebssystem aufgeschlüsselt sind, können bei der Analyse von Nutzungsmustern Probleme auf bestimmte Kombinationen (z. B. Safari unter OSX) festgestellt werden.

- **Analytics** - Die bereitgestellten Daten können verwendet werden, um die Client-seitigen Daten, die über Adobe Analytics oder ein anderes Analytics-Tool erfasst werden, zu ergänzen/zu überprüfen.
