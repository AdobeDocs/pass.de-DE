---
title: So stellen Sie eine Datenschutzanfrage
description: So stellen Sie eine Datenschutzanfrage
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# So stellen Sie eine Datenschutzanfrage {#howto-make-privacy-request}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Kennungen und Namespaces {#identifier-namespace}

Beim Senden einer Zugriffs- oder Löschanfrage muss die Kundenanwendung die folgenden Kennungen enthalten:

* **mvpdID** - Eindeutige Kennung der MVPD.
* **userID** - Identifiziert den Benutzer der App eines Programmierers eindeutig, stammt jedoch vom MVPD. Siehe Grundlegendes zu Benutzer-IDs in der Übersicht zu Programmierern .
* **IMSOrgID** - die Organisations-ID des Adobe Experience Cloud Identity Management-Service, die den Kunden in der Adobe Experience Cloud eindeutig identifiziert


Bitte das folgende Beispiel überprüfen:

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Benutzerinnen und Benutzer müssen authentifiziert sein, um Datenschutzanfragen für die Adobe Pass-Authentifizierung generieren zu können. Andernfalls müssen Programmierer andere Möglichkeiten finden, die MVPD-Benutzer-ID zu extrahieren.

## Anfragetypen {#req-type}

Die Adobe Pass-Authentifizierung unterstützt Zugriffs- und Löschanfragen.

### Zugriff {#access-req}

Für eine Zugriffsanfrage:

Wir stellen eine JSON-Datei zurück, die eine Zusammenfassung der Gesamtzahl der für diese betroffene Person erstellten Authentifizierungs- und Autorisierungsanfragen enthält.
Alle diese Ereignisse werden nach Kunde gefiltert.


**Beispiel anfordern**

Sie müssen eine JSON mit den Adobe Pass-Authentifizierungs-IDs hochladen, für die Sie die Datenzugriffsanfrage senden. Um zu sehen, wie ein wohlgeformtes JSON aussieht, sehen Sie sich dieses Beispiel an:

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Beispiel für eine Antwort**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### Löschen {#delete-req}

Sie müssen eine JSON mit den Adobe Pass-Authentifizierungs-IDs hochladen, für die Sie die Datenlöschanfrage senden. Um zu sehen, wie ein wohlgeformtes JSON aussieht, sehen Sie sich dieses Beispiel an:

**Beispiel anfordern**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Beispiel für eine Antwort**

Für eine Löschanfrage:

* Wir geben nur eine Quittung weiter, aus der hervorgeht, dass die Daten entfernt wurden, keine aggregierte Datei mit allen entfernten Daten.
* die in der Antwort enthaltene Empfangsbestätigung eine Zusammenfassung der Gesamtzahl der für diese betroffene Person gefundenen Authentifizierungs- und Autorisierungs-Token enthält.

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## Trigger einer Anfrage {#trigger-req}

Es gibt zwei Möglichkeiten für Kundinnen und Kunden, Datenschutzanfragen an Adobe zu senden:

* **manuell** - über die [Privacy Service-Benutzeroberfläche](#privacy-service-ui)
* **automatisch** - mithilfe der [Privacy Service-API](#privacy-service-api)

### Durch Verwendung der Privacy Service-Benutzeroberfläche {#privacy-service-ui}

Ein [vollständiges Tutorial](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=de#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) zum Zugriff auf und zur Verwendung der Privacy Service-Benutzeroberfläche ist online über Adobe I/O-Services verfügbar. Darüber hinaus können Kundinnen und Kunden diesen Link verwenden, um auf eine Bibliothek mit Videos und Artikeln zu Datenschutzbestimmungen zuzugreifen. Klicken Sie auf das Menü Adobe Experience Cloud und DSGVO . Dadurch werden eine Reihe von Videos geöffnet. Im Abschnitt „Anleitung zur DSGVO-Benutzeroberfläche“ wird erläutert, wie Sie sie verwenden.

In der -Benutzeroberfläche müssen Kundinnen und Kunden ihre eigene IMSOrgID und eine JSON mit DSGVO-Anfragen für jedes Produkt laden.

### Durch Verwendung der Privacy Service-API {#privacy-service-api}

Adobe Experience Platform Privacy Service bietet eine gemeinsame, zentralisierte Vereinfachung von Zugriffs-/Löschanfragen und Opt-out-Kaufanfragen für private Daten.

In der **Privacy Service-API** Dokumentation wird ausführlich beschrieben, wie ein Adobe-Kunde eine Integration mit der Adobe-API durchführen kann.

**Visualisieren von API-Aufrufen mit Postman (einer kostenlosen Software von Drittanbietern):**

* [Privacy Service-API-Postman-Sammlung auf GitHub](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Videoanleitung zum Erstellen der Postman-Umgebung](https://video.tv.adobe.com/v/28832)
* [Schritte zum Importieren von Umgebungen und Sammlungen in Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API-Pfade:**

* PLATFORM-Gateway-URL: `https://platform.adobe.io/`
* Basispfad für diese API: `/data/core/privacy/jobs`
* Beispiel für einen vollständigen Pfad: `https://platform.adobe.io/data/core/privacy/jobs/ping`


**Erforderliche Kopfzeilen:**

* Für alle Aufrufe sind die Kopfzeilen `Authorization`, `x-gw-ims-org-id` und `x-api-key` erforderlich. Weitere Informationen zum Abrufen dieser Werte finden Sie im **Authentifizierungs-Tutorial**.
* Alle Anfragen mit einer Payload im Anfragetext (z. B. POST-, PUT- und PATCH-Aufrufe) müssen die `Content-Type` mit dem Wert `application/json` enthalten.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=de#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
