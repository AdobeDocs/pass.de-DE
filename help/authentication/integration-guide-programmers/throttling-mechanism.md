---
title: Drosselmechanismus
description: Erfahren Sie mehr über den Drosselungsmechanismus, der bei der Adobe Pass-Authentifizierung verwendet wird. Einen Überblick über diesen Mechanismus finden Sie auf dieser Seite .
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 0%

---

# Drosselmechanismus {#throttling-mechanism}

Kunden mit der Passauthentifizierung müssen für jeden ihrer Benutzer gemäß den Anweisungen und ihrem Business Case auf die Passauthentifizierungs-API zugreifen können.

Die Passauthentifizierung führt einen Drosselungsmechanismus ein, um eine gerechte Verteilung der Ressourcen unter den Benutzern unserer Kunden sicherzustellen.

Dieser Mechanismus ist aus mehreren Gründen wichtig:

- Eine der goldenen Regeln der Multi-Mandanten-Architektur ist, dass das Verhalten eines Benutzers keinen Einfluss auf andere Personen haben sollte.
- Die Ratenbegrenzung ist für APIs wichtig, da bei der Integration mit einer API Fehler leicht gemacht werden können. Da APIs programmgesteuert sind, ist es relativ einfach, versehentlich mehr Anfragen als beabsichtigt zu senden.

## Mechanism - Übersicht {#mechanism-overview}

### Client-Implementierungen

Die Passauthentifizierung stellt Richtlinien und SDK für die Interaktion mit der API bereit, steuert jedoch nicht, wie der Kunde sie verwendet. Einige Implementierungen haben möglicherweise eine rudimentäre Implementierung und könnten den Service mit unnötigen API-Anfragen überfluten, ob versehentlich oder nicht, was dazu führen könnte, dass andere Benutzer Verzögerungen oder Kapazitätsprobleme erleben.

Der Dienst selbst sollte in der Lage sein, jede angemessene Kapazität zu verarbeiten. Doch egal wie skalierbar oder leistungsstark ein Service ist, es gibt immer Grenzen. Daher müssen für den Service Limits für die Anzahl der akzeptierten Aufrufe innerhalb eines bestimmten Zeitintervalls konfiguriert sein.

### Einführung in Einschränkungen

Die Passauthentifizierung basiert auf einer Benutzeridentifizierung und einem Token-Bucket-Limitationsalgorithmus mit vordefinierten Werten, um den Gerätezugriff jedes Benutzers auf unsere API zu steuern.

### Mechanismus zur Geräteerkennung

Der vorgeschlagene Drosselungsmechanismus verwendet die identifizierten Geräte einzeln mithilfe des Headers „X-Forwarded-For“. Die Grenzwerte werden für jedes Gerät auf die gleiche Weise angewendet.

### Erforderliche Aktualisierungen

Server-zu-Server-Implementierungen müssen die IP-Adressen ihrer Clients unter Verwendung des Header-Mechanismus „X-Forwarded-For“ weiterleiten.

Weitere Informationen zum Übergeben der Kopfzeile „X-Forwarded-For“ finden [ hier](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md).

### Tatsächliche Grenzwerte und Endpunkte

Derzeit ermöglicht das standardmäßige Limit maximal 1 Anfrage pro Sekunde mit einem anfänglichen Burst von 10 Anfragen (einmalige Vergütung für die erste Interaktion des identifizierten Clients, die es ermöglichen sollte, dass die Initialisierung erfolgreich abgeschlossen wird). Dies sollte keinen regulären Business Case für alle unsere Kunden betreffen.

Der Drosselungsmechanismus wird für die folgenden Endpunkte aktiviert:

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediaToken
- /api/v1/authenticate/freePreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /adobe-services/config/
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDK-Implementierungsverzweigung

Da Clients, die die bereitgestellten Adobe Pass-Authentifizierungs-SDKs verwenden, nicht explizit mit Endpunkten interagieren, werden in diesem Abschnitt die bekannten Funktionen, ihr Verhalten bei einer Drosselungsantwort und die zu ergreifenden Maßnahmen beschrieben.

#### setRequestor

Beim Erreichen des Drosselungslimits mithilfe `setRequestor` Funktion aus dem SDK gibt der SDK über `errorHandler` Callback einen CFG429-Fehlercode zurück.

#### getAuthorization

Beim Erreichen des Drosselungslimits mithilfe `getAuthorization` Funktion aus dem SDK gibt der SDK über `errorHandler` Callback einen Z100-Fehlercode zurück.

#### checkPreauthorizedResources

Beim Erreichen des Drosselungslimits mithilfe `checkPreauthorizedResources` Funktion aus dem SDK gibt der SDK über `errorHandler` Callback einen P100-Fehlercode zurück.

#### getMetadata

Beim Erreichen des Drosselungslimits mithilfe `getMetadata` Funktion aus der SDK gibt die SDK über `setMetadataStatus` Callback eine leere Antwort zurück.

Informationen zu den einzelnen Implementierungsdetails finden Sie in der entsprechenden SDK-Dokumentation.

- [JavaScript SDK API-Referenz](legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
- [Android SDK API-Referenz](legacy/sdks/android-sdk/android-sdk-api-reference.md)
- [iOS/tvOS-API-Referenz](legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

### API-Antwortänderungen und -antwort

Wenn wir feststellen, dass der Grenzwert überschritten wird, markieren wir diese Anfrage mit einem bestimmten Antwortstatus (HTTP 429: Zu viele Anfragen) und weisen Sie an, dass Sie alle dem Benutzergerät zugewiesenen Token (IP-Adresse) für das Zeitintervall verwendet haben.

Die Drosselung läuft nach einer Sekunde der ersten 429 Antworten ab. Jede Anwendung, die eine 429-Antwort erhält, muss mindestens 1 Sekunde warten, bevor eine neue Anfrage generiert wird.

Alle Kundenanwendungen sollten die Antwort „429 Zu viele Anfragen“ angemessen verarbeiten.

Hier finden Sie eine Beispiel-429-Antwortnachricht:

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## Auswirkungen und erforderliche Änderungen

### Übergeben des Headers „X-Forwarded-For“

Kunden, die eine benutzerdefinierte Implementierung verwenden (einschließlich Server-zu-Server-Implementierungen), um mit der Pass Authentication API zu interagieren, sollten sicherstellen, dass sie ihre Benutzer-IP-Adresse erfassen und korrekt weiterleiten können, indem sie den X-Forwarded-For-Header verwenden, um die Authentifizierungs-API weiter zu übergeben.

Weitere Informationen [ Sie ](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)hier).

### Reaktion auf neuen Antwort-Code

Kunden, die eine benutzerdefinierte Implementierung (einschließlich Server-zu-Server-Implementierungen) zur Interaktion mit der Passauthentifizierungs-API verwenden, sollten sicherstellen, dass jeder nachfolgende Aufruf, der nach Erhalt einer 429-zu-viele-Anfragen erfolgt, eine Wartezeit von mindestens 1 Sekunde beinhaltet. Diese Wartezeit bietet die Möglichkeit, diesen Mechanismus zu ändern und eine gültige Geschäftsantwort zu erhalten.

## Szenario-Beispiel für Einschränkungen

| Zeit seit der ersten Anfrage | Antwort erhalten | Erklärung |
|--------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| Sekunde 0 | Erfolgsstatus-Code des Anrufs | 1 Aufrufe vom Limit verbraucht |
| Sekunde 0,3 | Erfolgsstatus-Code des Anrufs | 1 Aufrufe, die vom Limit verbraucht werden, und 1 Aufrufe, die als Burst markiert sind |
| Sekunde 0,6 | Erfolgsstatus-Code des Anrufs | 1 vom Limit verbrauchte Aufrufe und 2 als Burst markierte Aufrufe |
| Zweite 0,9 | Erfolgsstatus-Code des Anrufs | 1 Aufrufe, die vom Limit verbraucht werden, und 3 Aufrufe, die als Burst markiert sind |
| Sekunde 1.2 | Erfolgsstatus-Code des Anrufs | 2 Aufrufe, die vom Limit verbraucht werden, und 3 Aufrufe, die als Burst markiert sind |
| Sekunde 1.3 | Erfolgsstatus-Code des Anrufs | 2 Aufrufe werden vom Limit verbraucht und 4 Aufrufe werden als Burst markiert |
| Sekunde 1.4 | Erfolgsstatus-Code des Anrufs | 2 Aufrufe, die vom Limit verbraucht werden, und 5 Aufrufe, die als Burst markiert sind |
| Sekunde 1,5 | Erfolgsstatus-Code des Anrufs | 2 Aufrufe werden vom Limit verbraucht und 6 Aufrufe sind als Burst gekennzeichnet |
| Sekunde 1,6 | Erfolgsstatus-Code des Anrufs | 2 Aufrufe werden vom Limit verbraucht und 7 Aufrufe sind als Burst gekennzeichnet |
| Sekunde 1,7 | Erfolgsstatus-Code des Anrufs | 2 Aufrufe werden vom Limit verbraucht und 8 Aufrufe werden als Burst markiert |
| Sekunde 1,8 | Erfolgsstatus-Code des Anrufs | 2 Aufrufe, die vom Limit verbraucht werden, und 9 Aufrufe, die als Burst markiert sind |
| 2.1 | Erfolgsstatus-Code des Anrufs | 3 Aufrufe, die vom Limit verbraucht werden, und 9 Aufrufe, die als Burst markiert sind |
| 2.2. | Erfolgsstatus-Code des Anrufs | 3 Aufrufe werden vom Limit verbraucht und 10 Aufrufe werden als Burst markiert |
| 2,4 Sekunden | Anruf erhält 429-Status-Code | 3 Aufrufe, die vom Limit verbraucht werden, und 10 Aufrufe, die als Burst markiert sind, und 1 Aufruf erhalten „429 Zu viele Anfragen“ |
| 2,6 Sekunden | Anruf erhält 429-Status-Code | 3 Aufrufe, die vom Limit verbraucht werden, 10 Aufrufe, die als Burst markiert sind, und 2 Aufrufe erhalten „429 Zu viele Anfragen“ |
| 2,8 Sekunden | Anruf erhält 429-Status-Code | 3 Aufrufe, die vom Limit verbraucht werden, 10 Aufrufe, die als Burst markiert sind, und 3 Aufrufe erhalten „429 Zu viele Anfragen“ |
| Zweite 3.1 | Erfolgsstatus-Code des Anrufs | 4 Aufrufe, die vom Limit verbraucht werden, 10 Aufrufe, die als Burst markiert sind, und 3 Aufrufe erhalten „429 Zu viele Anfragen“ |
