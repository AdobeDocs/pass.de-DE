---
title: Verwenden der Experience Cloud-ID in der Adobe Pass-Authentifizierung
description: Verwenden der Experience Cloud-ID in der Adobe Pass-Authentifizierung
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---

# Verwenden der Experience Cloud-ID in der Adobe Pass-Authentifizierung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Was ist Experience Cloud ID und wie kann ich sie abrufen? {#what-exp-cloud-id-obtain}

Die Experience Cloud-ID (kurz ECID) ist eine eindeutige ID, die von Adobe Experience Cloud für jeden einzelnen Benutzer in Ihrer Anwendung/Website generiert wird. ECID wird häufig in allen Experience Cloud-Berichten verwendet, die verwendet werden, um Informationen über einen bestimmten Benutzer über mehrere Anwendungen/Websites hinweg zu verknüpfen.

Wenn Sie bereits über ein System verfügen, das eine Besucher-ID bereitstellt, sollten Sie für den Umfang dieses Dokuments dieselbe ID verwenden.

Eine Möglichkeit zum Abrufen der ECID besteht in der Verwendung des Experience Cloud ID-Diensts. Sie können Ihren bevorzugten Implementierungstyp verwenden, entweder basierend auf TDM, JS-Bibliothek, Server-seitig, direkter Integration oder nativen Bibliotheken für mobile Plattformen. Eine umfassende Übersicht der verfügbaren Dienste, Bibliotheken, SDKs und Implementierungshandbücher finden Sie unter: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>

## Welchen Nutzen hat die Verwendung der Experience Cloud ID in der Adobe Pass-Authentifizierung? {#benefit-ex-cloud-id}

Wenn Sie unsere SDKs und die clientlose REST-API für die Verwendung Ihrer ECID konfigurieren, können Sie später die von der Adobe Pass-Authentifizierung erfassten Daten mit Ihren bestehenden Experience Cloud-Lösungen verknüpfen. Auf diese Weise können Sie die Journey und Erfahrung Ihrer Kunden in allen von Adobe bereitgestellten Lösungen besser verstehen.

## Wie wird die Experience Cloud ID in der Adobe Pass-Authentifizierung verwendet? {#how-to-ex-cloud-id-authn}

Nachdem Sie die ECID (Erläuterung oben) erhalten haben, müssen Sie diese Informationen an unsere SDKs und unsere clientlose REST-API weitergeben. Diese Informationen werden später bei jedem Netzwerkaufruf des SDK an unsere Server übergeben. Der Konfigurationsprozess unterscheidet sich für jedes SDK wie folgt:

### JS-SDK {#js-sdk}

Für JavaScript müssen Sie die ECID in einer Zuordnung als dritten Parameter an den setRequestor-Aufruf übergeben.

**Anwendungsbeispiel:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS-SDK {#ios-sdk}

Für das iOS/tvOS-SDK gibt es eine dedizierte Methode namens setOptions.

**Anwendungsbeispiel:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Für das Android/fireTV-SDK ähnelt der Mechanismus iOS. Nur der Parametername ist anders. Die API wird hier dokumentiert.

**Anwendungsbeispiel:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### Clientlose API {#clientless-api}

Bei Verwendung von Adobe Pass über die REST-API des Unternehmens wird die **ECID** Wert gesendet werden **auf allen APIs** als Parameter mit dem Namen **&quot;ap_vi&quot;**.

**Anwendungsbeispiel:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`
