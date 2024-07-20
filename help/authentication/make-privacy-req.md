---
title: Datenschutzanfrage stellen
description: Datenschutzanfrage stellen
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# Datenschutzanfrage stellen {#howto-make-privacy-request}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Kennungen und Namespaces {#identifier-namespace}

Beim Senden einer Zugriffs- oder Löschanfrage muss die Kundenanwendung die folgenden Kennungen enthalten:

* **mvpdID** - Eindeutige Kennung des MVPD.
* **userID** - Identifiziert eindeutig den Benutzer der App eines Programmierers, stammt jedoch aus dem MVPD. Weitere Informationen finden Sie unter Grundlegendes zu Benutzer-IDs in der Programmübersicht.
* **IMSOrgID** - die Organisations-ID des Adobe Experience Cloud Identity Management-Dienstes, die den Kunden in der Adobe Experience Cloud eindeutig identifiziert


Sehen Sie sich das folgende Muster an:

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Benutzer müssen authentifiziert werden, damit sie Datenschutzanfragen für die Adobe Pass-Authentifizierung generieren können. Andernfalls müssen Programmierer andere Möglichkeiten finden, die MVPD-Benutzer-ID zu extrahieren.

## Anforderungstypen {#req-type}

Die Adobe Pass-Authentifizierung unterstützt Zugriffs- und Löschanfragen.

### Zugriff {#access-req}

Für eine Zugriffsanfrage:

stellen wir eine JSON-Datei bereit, die eine Zusammenfassung der Gesamtanzahl der Authentifizierungs- und Autorisierungsanfragen enthält, die für diese betroffene Person erstellt wurden.
alle diese Ereignisse werden nach Kunde gefiltert.


**Anforderungsbeispiel**

Sie müssen eine JSON-Datei mit den Adobe Pass-Authentifizierungskennungen hochladen, für die Sie die Datenzugriffsanfrage senden. In diesem Beispiel erfahren Sie, wie eine gut geformte JSON aussieht:

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

**Antwort-Beispiel**

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

Sie müssen eine JSON-Datei mit den Adobe Pass-Authentifizierungskennungen hochladen, für die Sie die Datenlöschanfrage senden. In diesem Beispiel erfahren Sie, wie eine gut geformte JSON aussieht:

**Anforderungsbeispiel**

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

**Antwort-Beispiel**

Für eine Löschanfrage:

* Wir teilen nur eine Quittung, dass die Daten entfernt wurden, und keine aggregierte Datei mit allen entfernten Daten.
* Der in der Antwort enthaltene Empfang enthält eine Zusammenfassung der Gesamtzahl der Authentifizierungs- und Autorisierungstoken, die für diese betroffene Person gefunden wurden.

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

## Trigger einer Anforderung {#trigger-req}

Kunden haben zwei Möglichkeiten, Datenschutzanfragen an Adobe zu senden:

* **manuell** - durch Verwendung der [Privacy Service-Benutzeroberfläche](#privacy-service-ui)
* **automatisch** - durch Verwendung der [Privacy Service-API](#privacy-service-api)

### Über die Privacy Service-Benutzeroberfläche {#privacy-service-ui}

Ein [vollständiges Tutorial](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) zum Zugriff auf und zur Verwendung der Privacy Service-Benutzeroberfläche ist online über Adobe I/O-Services verfügbar. Darüber hinaus können Kunden über diesen Link auf die Bibliothek mit Videos und Artikeln zu Datenschutzbestimmungen zugreifen. Klicken Sie auf das Menü Adobe Experience Cloud und DSGVO . Dadurch werden eine Reihe von Videos geöffnet. In &quot;Anleitung zur DSGVO-Benutzeroberfläche&quot;wird erläutert, wie sie verwendet werden kann.

In der Benutzeroberfläche müssen Kunden ihre eigene IMSOrgID und eine JSON mit Details zu DSGVO-Anfragen für jedes Produkt laden.

### Durch Verwendung der Privacy Service-API {#privacy-service-api}

Adobe Experience Platform Privacy Service bietet eine gemeinsame, zentralisierte Erleichterung von Zugriffs-/Löschanfragen und Opt-out-Anfragen für personenbezogene Daten.

Die **Privacy Service API-Dokumentation** beschreibt ausführlich, wie ein Adobe-Kunde mit der Adobe-API integriert werden kann.

**Visualisieren von API-Aufrufen mit Postman (einer kostenlosen Drittanbietersoftware):**

* [Privacy Service-API Postman-Sammlung auf GitHub](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Videoleitfaden zum Erstellen der Postman-Umgebung](https://video.tv.adobe.com/v/28832)
* [Schritte zum Importieren von Umgebungen und Sammlungen in Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API-Pfade:**

* PLATFORM Gateway URL: `https://platform.adobe.io/`
* Basispfad für diese API: `/data/core/privacy/jobs`
* Beispiel eines vollständigen Pfads: `https://platform.adobe.io/data/core/privacy/jobs/ping`


**Erforderliche Kopfzeilen:**

* Alle Aufrufe erfordern die Header `Authorization`, `x-gw-ims-org-id` und `x-api-key`. Weitere Informationen zum Abrufen dieser Werte finden Sie im **Authentifizierungs-Tutorial**.
* Alle Anfragen mit einer Payload im Anfragetext (wie POST-, PUT- und PATCH-Aufrufe) müssen die Kopfzeile `Content-Type` mit dem Wert `application/json` enthalten.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
