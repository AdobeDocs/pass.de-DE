---
product: adobe primetime
feature: Concurrency Monitoring
audience: end-user
user-guide-title: Überwachung der Parallelität von Adobe Pass
user-guide-description: Erfahren Sie, wie Sie Begrenzungen für die gleichzeitige Nutzung mehrerer Anwendungen definieren und durchsetzen.
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 5%

---


# Hilfe zur Adobe Pass-Parallelitätsüberwachung {#cm}

- [CM-Einführung](cm-home.md) {#cm-intro}
- Erste Schritte {#getting-started}
   - [Erste Schritte mit der Parallelitätsüberwachung](getting-started/getting-started-overview.md)
   - [Schlüsselkonzepte](getting-started/key-concepts.md)
- Integrationshandbuch {#integration-guide}
   - [Glossar](cm-glossary.md)
   - API-Referenz {#api-reference}
      - [Übersicht](api/api-reference-overview.md)
      - [API-Endpunkte](api/api-endpoints.md)
      - [API-Referenz](api/cm-api-overview.md)
   - [API zur Überwachung der gleichzeitigen Nutzung](reports/cmu-api.md)
   - [API-Zugriff](reports/cmu-api-access.md)
   - [Beispiele für Nutzungsberichte zur gleichzeitigen Überwachung](reports/cm-usage-reports-examples.md)
- Anwendungsfälle und Implementierung {#use-cases-implementation}
   - [Anwendungsfälle - Übersicht](use-cases/cm-use-cases.md)
   - [LIFO vs. FIFO-Strategien](use-cases/lifo-fifo-strategies.md)
   - [Umgang mit Sitzungskonflikten](use-cases/handling-409-errors.md)
   - [Implementierungsmodelle](technical/implementation-models.md)
   - [Einzelmandant-Richtlinie für mehrere Apps](technical/single-tenant-policy-mult-app.md)
   - [Beschränken der gleichzeitigen Nutzung mehrerer Apps](technical/restrict-concurr-usage-mult-apps.md)
- [CM-Nutzungsberichte](reports/cm-usage-reports.md) {#cm-usage-reports}
- Technische Hinweise {#tech-notes}
   - [Standard-Metadatenattribute](technical/standard-metadata-attributes.md)
   - [Benutzerdefinierte Metadaten](technical/custom-metadata.md)
   - [Entscheidungspunkt der Richtlinie](technical/cm-policy-decision-point.md)
   - [Informationspunkt für Richtlinien](technical/policy-info-pt-versionone.md)
   - [Drosselung](technical/throttling-mechanism.md)
   - [Vorgehensweise: Unterscheiden zwischen VOD und Live-Inhalten bei der gleichzeitigen Überwachung](technical/vod-live-dist.md)
   - [Richtlinie zur Datenaufbewahrung](technical/data-retention-policy.md)
- Versionen {#cm-release-notes}
   - [Parallelitätsüberwachung - Versionshinweise zu 3.6.2](releases/rn-cm-services-362.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.6.1](releases/rn-cm-services-361.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.6.0](releases/rn-cm-services-360.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.5.1](releases/rn-cm-services-351.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.5.0](releases/rn-cm-services-350.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.4.4](releases/rn-cm-services-344.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.4.3](releases/rn-cm-services-343.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.4.2](releases/rn-cm-services-342.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.4.0](releases/rn-cm-services-340.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.3.7](releases/rn-cm-services-337.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.3.6](releases/rn-cm-services-336.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.3.2](releases/rn-cm-services-332.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.3.1](releases/rn-cm-services-331.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.3.0](releases/rn-cm-services-330.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.2.3](releases/rn-cm-services-323.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.2.2](releases/rn-cm-services-322.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.2.1](releases/rn-cm-services-321.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.2](releases/rn-cm-services-320.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 3.1](releases/rn-cm-services-31.md)
   - [Parallelitätsüberwachung - Versionshinweise zu Version 3.0](releases/rn-cm-services-30.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 2.9.6](releases/rn-cm-296.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 2.9](releases/rn-cm-29.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 2.8.2](releases/rn-cm-282.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 2.7.2](releases/rn-cm-272.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 2.6.0](releases/rn-cm-260.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 2.5.0](releases/rn-cm-250.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 2.3.2](releases/rn-cm-232.md)
   - [Parallelitätsüberwachung - Versionshinweise zu 2.2.2](releases/rn-cm-222.md)
- [Support-Verfahren](support/cm-escalation-procedures.md) {#support-procedures}
