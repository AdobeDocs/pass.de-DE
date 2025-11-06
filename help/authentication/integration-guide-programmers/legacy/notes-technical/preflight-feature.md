---
title: PreFlight-Funktion, Aktivieren, Fehlerbehebung oder Bestimmen des Problems
description: PreFlight-Funktion, Aktivieren, Fehlerbehebung oder Bestimmen des Problems
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 0%

---

# (Legacy) Preflight-Funktion: So aktivieren, beheben oder bestimmen Sie das Problem {#preflight-feature}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Die Art und Weise, wie die Adobe Pass-Authentifizierung PreAuthorizeResources berechnet, wurde geändert. Die Vorabautorisierungs-API verfügt über eine neue Implementierung. Diese Implementierung ersetzt die alte Lösung, die darin besteht, nur mehrere Autorisierungsaufrufe durchzuführen.
Die externe Schnittstelle für die PreAuthorization-API bleibt unverändert. In der Anwendung des Programmierers sind keine Aktualisierungen erforderlich.

Es gibt drei Möglichkeiten, die Preflight-Ressourcen zu berechnen:

* **Verzweigung und Verknüpfungsmethode mit MVPD**: Dazu gehört, dass Adobe mehrere Autorisierungsaufrufe an die MVPD sendet (der Client muss jedoch weiterhin einen Preflight-Aufruf ausführen).
* **Kanalaufstellung**: Die MVPD legt die Kanalaufstellung für den angemeldeten Benutzer in der SAML-Authentifizierungsantwort offen und Adobe gibt die autorisierten Ressourcen auf dieser Grundlage zurück. Die SAML-Authentifizierungsantwort im SAML-Tracer sollte diese Liste verfügbar machen.
* **Multi-Channel-Autorisierung**: Die Client- und Adobe-Authentifizierung führen beide einen einzigen Aufruf an die MVPD für eine Reihe von Ressourcen durch.

Unabhängig von der MVPD führt die Client-Anwendung einen einzigen Aufruf an den Preflight-Endpunkt (checkPreauthorizedResources-API) durch und übergibt dabei einen Satz von resourceIDs. Basierend auf einer der oben genannten, von MVPD unterstützten Methoden gibt Adobe dann die vorab autorisierten Ressourcen-IDs zurück.

Wenn Preflight auf der Verzweigungs- und Join-Methode basiert, prüft das Adobe Pass-Authentifizierungs-Backend einen Wert, der für die „Max. Preauthorization-Aufrufe“ in seiner Konfiguration festgelegt ist. Dies wird von Adobe konfiguriert.

Der Standardwert für die Konfiguration „max. PreAuthorization Calls“ ist „5“, was bedeutet, dass im Preflight nur 5 Ressourcen für die MVPDs „Verzweigung &amp; Verknüpfung“ gesendet werden können. Die Übergabe von mehr als 5 Ressourcen führt zu einer Ausnahme und eine Nullliste wird zurückgegeben. Dies ist das erwartete Verhalten. Wir können dies für jeden Wert konfigurieren, wenn die MVPD keine Kanalaufstellung oder Mehrkanal-Autorisierung unterstützt, aber nur nach Rücksprache mit ihnen, da mehrere Aufrufe zur Verzweigung und Teilnahme an Autorisierung ihre Ladezeiten erhöhen.

Daher sollten Sie bei der Aktivierung/Fehlerbehebung von Preflight für eine MVPD auf diese Dinge achten:

* Die von MVPD unterstützte Methode (Verzweigung und Verknüpfung, Kanalaufstellung oder Mehrkanal).
* Wenn nur Verzweigung und Verknüpfung unterstützt wird, muss der Programmierer gefragt werden, wie viele Ressourcen-IDs er im Preflight-Aufruf senden wird.
* Der MVPD muss konsultiert werden und die Auswirkungen kennen, die eine „n“-Anzahl von Aufrufen zur Verzweigung und Teilnahme an Autorisierungen hat. Danach muss der Wert in der Konfiguration konfiguriert werden, wenn er größer als 5 ist.

**Einschränkung**

Bitte beachten Sie, dass wir keine resourceID aus dem Preflight-Aufruf für einige MVPDs wie AT&amp;T &amp; TWC erhalten, wenn eine der resourceIDs eine falsche ID oder eine nicht erkannte ID in der Liste der resourceIDs ist, die sie im Preflight-Aufruf senden, obwohl wir auch gültige und autorisierte Ressourcen in dieser Liste haben.
