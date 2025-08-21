---
title: Verwenden der Experience Cloud ID bei der Adobe Pass-Authentifizierung
description: Verwenden der Experience Cloud ID bei der Adobe Pass-Authentifizierung
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# Verwenden der Experience Cloud ID bei der Adobe Pass-Authentifizierung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Was ist die Experience Cloud ID und wie kann ich sie erhalten? {#what-exp-cloud-id-obtain}

Die Experience Cloud-ID (kurz ECID) ist eine eindeutige ID, die von Adobe Experience Cloud für jeden einzelnen Benutzer in Ihrer Anwendung/Website generiert wird. ECID wird in allen Experience Cloud-Berichten verwendet, die verwendet werden, um Informationen über einen bestimmten Benutzer über mehrere Programme/Websites hinweg miteinander zu verknüpfen.

Wenn Sie bereits über ein System verfügen, das eine Besucher-ID bereitstellt, sollten Sie für den Umfang dieses Dokuments dieselbe ID verwenden.

Eine Möglichkeit, die ECID abzurufen, besteht in der Verwendung des Experience Cloud ID-Service. Sie können Ihren bevorzugten Implementierungstyp verwenden, entweder auf der Grundlage von TDM, JS-Bibliothek, Server-seitig, direkter Integration oder nativen Bibliotheken für mobile Plattformen. Eine umfassende Übersicht über die verfügbaren Services, Bibliotheken, SDKs und Implementierungshandbücher finden Sie unter: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html?lang=de>

## Was sind die Vorteile der Verwendung der Experience Cloud ID bei der Adobe Pass-Authentifizierung? {#benefit-ex-cloud-id}

Wenn Sie unsere SDKs und die Client-lose REST-API für die Verwendung Ihrer ECID konfigurieren, können Sie später die von der Adobe Pass-Authentifizierung erfassten Daten mit Ihren bestehenden Experience Cloud-Lösungen verknüpfen. Auf diese Weise können Sie das Journey und Erlebnis Ihrer Kunden in allen von Adobe bereitgestellten Lösungen besser verstehen.

## Wie wird die Experience Cloud-ID bei der Adobe Pass-Authentifizierung verwendet? {#how-to-ex-cloud-id-authn}

Nachdem Sie die ECID erhalten haben (siehe Erklärung oben), müssen Sie diese Informationen an unsere SDKs und unsere Client-lose REST-API weitergeben. Diese Informationen werden später bei jedem Netzwerkaufruf, den SDK durchführt, an unsere Server weitergeleitet. Der Konfigurationsprozess unterscheidet sich für jede SDK wie folgt:

### JS SDK {#js-sdk}

Bei JavaScript müssen Sie die ECID in einer Zuordnung als dritten Parameter an den setRequestor-Aufruf übergeben.

**Anwendungsbeispiel:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

Für iOS/tvOS SDK gibt es eine dedizierte Methode namens setOptions.

**Anwendungsbeispiel:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/FireTV SDK {#android-sdk}

Bei Android/FireTV SDK ähnelt der Mechanismus iOS. Nur der Parametername ist anders. Die API ist hier dokumentiert.

**Anwendungsbeispiel:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### Clientless-API {#clientless-api}

Bei Verwendung von Adobe Pass über die REST-API v1 sollte der **ECID**-Wert **für alle APIs)** Parameter namens &quot;**_ap_vi“ gesendet**.

**Anwendungsbeispiel:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### REST-API V2 {#rest-api-v2}

Bei Verwendung von Adobe Pass über die REST-API v2 sollte der **ECID**-Wert **für alle APIs)** Kopfzeile namens **&#39;AP-Visitor-Identifier&#39;** gesendet werden.

**Anwendungsbeispiel:**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
Kopfzeilen:\
`AP-Visitor-Identifier: THE_ECID_VALUE`

