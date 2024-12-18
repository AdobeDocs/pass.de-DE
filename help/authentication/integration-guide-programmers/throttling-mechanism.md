---
title: Drosselmechanismus
description: Erfahren Sie mehr über den in der Adobe Pass-Authentifizierung verwendeten Drosselmechanismus. Einen Überblick über diesen Mechanismus finden Sie auf dieser Seite.
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 0%

---

# Drosselmechanismus {#throttling-mechanism}

Alle Kunden von Pass Authentication müssen gemäß Anweisungen und Geschäftsfall auf die Pass Authentication API für jeden ihrer Benutzer zugreifen können.

Mit der Pass-Authentifizierung wird ein Drosselungsmechanismus eingeführt, um eine gerechte Verteilung der Ressourcen unter den Benutzern unserer Kunden sicherzustellen.

Dieser Mechanismus ist aus einigen Gründen wichtig:

- Eine der goldenen Regeln der Architektur mit mehreren Mandanten besteht darin, dass das Verhalten eines Benutzers sich nicht auf einen anderen auswirkt.
- Die Ratenbegrenzung ist für APIs wichtig, da bei der Integration in eine API leicht Fehler auftreten können. Da APIs programmgesteuert sind, ist es relativ einfach, versehentlich mehr Anfragen als beabsichtigt zu senden.

## Mechanik - Übersicht {#mechanism-overview}

### Client-Implementierungen

Die Pass Authentication bietet Richtlinien und SDK für die Interaktion mit der API, steuert jedoch nicht, wie der Kunde sie verwendet. Einige Implementierungen verfügen möglicherweise über eine rudimentäre Implementierung und könnten den Dienst mit unnötigen API-Anfragen überschwemmen, sei es versehentlich oder nicht, was dazu führen könnte, dass andere Benutzer Verlangsamungen oder Kapazitätsprobleme erleben.

Der Dienst selbst sollte in der Lage sein, jede angemessene Kapazität zu bewältigen. Unabhängig davon, wie skalierbar oder leistungsfähig ein Dienst ist, gibt es immer Einschränkungen. Daher muss der Dienst über Beschränkungen für die Anzahl der akzeptierten Aufrufe innerhalb eines bestimmten Zeitintervalls verfügen.

### Einführung in die Einschränkungen

Die Pass-Authentifizierung basiert auf der Benutzeridentifizierung und einem Token-Bucket-Ratenbegrenzungsalgorithmus mit vordefinierten Werten, um den Gerätezugriff jedes Benutzers auf unsere API zu steuern.

### Geräteidentifizierungsmechanismus

Der vorgeschlagene Drosselmechanismus verwendet die identifizierten Geräte einzeln mithilfe des Headers &quot;X-Forwarded-For&quot;. Die Beschränkungen werden für jedes Gerät auf die gleiche Weise angewendet.

### Erforderliche Updates

Bei Server-zu-Server-Implementierungen müssen die IP-Adressen ihrer Kunden mithilfe des Header-Mechanismus &quot;X-Forwarded-For&quot;weitergeleitet werden.

Weitere Informationen zum Übergeben der X-Forwarded-For-Kopfzeile [finden Sie hier](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md).

### Tatsächliche Einschränkungen und Endpunkte

Derzeit erlaubt das Standardlimit maximal 1 Anfrage pro Sekunde mit einem anfänglichen Burst von 10 Anfragen (einmalige Berücksichtigung der ersten Interaktion des identifizierten Clients, was eine erfolgreiche Initialisierung ermöglichen sollte). Dies sollte sich nicht auf reguläre Geschäftsfälle für alle unsere Kunden auswirken.

Der Drosselmechanismus wird für die folgenden Endpunkte aktiviert:

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
- /api/v1/checkauthin
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /adobe-services/config/
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDK-Implementierungskennung

Da Clients, die die SDK mit Adobe Pass-Authentifizierung verwenden, nicht explizit mit Endpunkten interagieren, werden in diesem Abschnitt die bekannten Funktionen, ihr Verhalten beim Auftreten einer Drosselantwort und die zu ergreifenden Maßnahmen vorgestellt.

#### setRequest

Wenn das SDK die Drosselgrenze mithilfe der Funktion `setRequestor` aus dem SDK erreicht, gibt es einen CFG429-Fehlercode über den Rückruf `errorHandler` zurück.

#### getAuthorization

Wenn das SDK die Drosselgrenze mithilfe der Funktion `getAuthorization` aus dem SDK erreicht, gibt es einen Z100-Fehlercode über den Rückruf `errorHandler` zurück.

#### checkPreauthorizedResources

Wenn das SDK die Drosselgrenze mithilfe der Funktion `checkPreauthorizedResources` aus dem SDK erreicht, gibt es einen P100-Fehlercode über den Rückruf `errorHandler` zurück.

#### getMetadata

Wenn das SDK die Drosselgrenze mithilfe der Funktion `getMetadata` aus dem SDK erreicht, gibt es eine leere Antwort über den Rückruf `setMetadataStatus` zurück.

Weiterführende Informationen zu einzelnen Implementierungsdetails finden Sie in der jeweiligen SDK-Dokumentation.

- [JavaScript SDK-API-Referenz](legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
- [Android SDK-API-Referenz](legacy/sdks/android-sdk/android-sdk-api-reference.md)
- [iOS/tvOS-API-Referenz](legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

### API-Antwortänderungen und -Antwort

Wenn wir feststellen, dass die Grenze überschritten wurde, markieren wir diese Anfrage mit einem bestimmten Antwortstatus (HTTP 429 Zu viele Anforderungen) und weisen Sie an, dass Sie alle dem Benutzergerät (IP-Adresse) für das Zeitintervall zugewiesenen Token verbraucht haben.

Die Drosselung läuft nach einer Sekunde der ersten 429 Antworten ab. Jede Anwendung, die eine 429-Antwort erhält, muss mindestens 1 Sekunde warten, bevor eine neue Anfrage generiert wird.

Alle Kundenanwendungen sollten die Antwort &quot;429 Zu viele Anforderungen&quot;entsprechend handhaben.

Im Folgenden finden Sie eine Beispielantwort mit 429 :

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

### Übergeben der X-Forwarded-For-Kopfzeile

Kunden, die eine benutzerdefinierte Implementierung (einschließlich Server-zu-Server-API) zur Interaktion mit der API für die Pass-Authentifizierung verwenden, sollten sicherstellen, dass sie ihre Benutzer-IP-Adresse erfassen und korrekt weiterleiten können. Dazu wird der Header X-Forwarded-For verwendet, der weiter zur API für die Pass-Authentifizierung dient.

Weitere Informationen finden Sie unter [hier](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md) .

### Reagieren auf neuen Antwort-Code

Kunden, die eine benutzerdefinierte Implementierung (einschließlich Server-zu-Server-API) verwenden, um mit der API für die Pass-Authentifizierung zu interagieren, sollten sicherstellen, dass alle nachfolgenden Aufrufe, die nach Erhalt einer 429-zu-viele-Anfrage durchgeführt werden, eine Wartezeit von mindestens 1 Sekunde enthalten. Diese Wartezeit bietet die Möglichkeit, diesen Mechanismus zu ändern und eine gültige geschäftliche Antwort zu erhalten.

## Szenario für Einschränkungen

| Zeit seit der ersten Anfrage | Erhalt der Antwort | Erklärung |
|--------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| Second 0 | Aufruf erhält Erfolgsstatus-Code | 1 vom Limit aus verbrauchte Aufrufe |
| Second 0.3 | Aufruf erhält Erfolgsstatus-Code | 1 vom Limit aus verbrauchte Anrufe und 1 als &quot;Berst&quot;markierte Anrufe |
| Second 0.6 | Aufruf erhält Erfolgsstatus-Code | 1 vom Limit verbrauchte Aufrufe und 2 als &quot;Berst&quot;markierte Aufrufe |
| Zweiter 0,9 | Aufruf erhält Erfolgsstatus-Code | 1 vom Limit aus verbrauchte Anrufe und 3 als &quot;Berst&quot;markierte Anrufe |
| Zweiter 1.2 | Aufruf erhält Erfolgsstatus-Code | 2 vom Limit aus verbrauchte Anrufe und 3 als &quot;Berst&quot;markierte Anrufe |
| Zweites 1.3 | Aufruf erhält Erfolgsstatus-Code | 2 vom Limit aus verbrauchte Anrufe und 4 als &quot;Berst&quot;markierte Anrufe |
| Zweiter 1.4 | Aufruf erhält Erfolgsstatus-Code | 2 vom Limit aus verbrauchte Anrufe und 5 als platziert markierte Anrufe |
| Zweiter 1.5 | Aufruf erhält Erfolgsstatus-Code | 2 vom Limit aus verbrauchte Anrufe und 6 als &quot;Berst&quot;markierte Anrufe |
| Zweiter 1.6 | Aufruf erhält Erfolgsstatus-Code | 2 vom Limit aus verbrauchte Anrufe und 7 als &quot;Berst&quot;markierte Anrufe |
| Zweiter 1.7 | Aufruf erhält Erfolgsstatus-Code | 2 vom Limit aus verbrauchte Anrufe und 8 als &quot;Berst&quot;markierte Anrufe |
| Zweiter 1.8 | Aufruf erhält Erfolgsstatus-Code | 2 vom Limit aus verbrauchte Anrufe und 9 als &quot;Berst&quot;markierte Anrufe |
| 2.1 | Aufruf erhält Erfolgsstatus-Code | 3 vom Limit verbrauchte Anrufe und 9 als &quot;Berst&quot;markierte Anrufe |
| 2.2 | Aufruf erhält Erfolgsstatus-Code | 3 vom Limit verbrauchte Aufrufe und 10 als &quot;Berst&quot;markierte Aufrufe |
| 2.4 | Aufruf erhält 429 Status-Code | 3 von der Begrenzung verbrauchte Anrufe und 10 als abgestürzt gekennzeichnete Anrufe und 1 Anruf erhält &quot;429 Zu viele Anfragen&quot; |
| 2.6 | Aufruf erhält 429 Status-Code | 3 von der Begrenzung verbrauchte Anrufe und 10 als platziert markierte Anrufe und 2 Anrufe erhalten &quot;429 Zu viele Anfragen&quot; |
| 2.8 | Aufruf erhält 429 Status-Code | 3 von der Begrenzung verbrauchte Anrufe und 10 als platziert markierte Anrufe und 3 Anrufe erhalten &quot;429 Zu viele Anfragen&quot; |
| Zweites 3.1 | Aufruf erhält Erfolgsstatus-Code | 4 Anrufe, die von der Begrenzung aus verbraucht werden, 10 Anrufe, die als &quot;Berst&quot;gekennzeichnet sind, und 3 Anrufe erhalten &quot;429 Zu viele Anfragen&quot; |
