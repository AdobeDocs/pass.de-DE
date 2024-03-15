---
title: Drosselmechanismus
description: Erfahren Sie mehr über den in der Adobe Pass-Authentifizierung verwendeten Drosselmechanismus. Einen Überblick über diesen Mechanismus finden Sie auf dieser Seite.
source-git-commit: 4f81f39427d87e4274c27d8f1b4bd1eb366d9abb
workflow-type: tm+mt
source-wordcount: '987'
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

Weitere Informationen dazu, wie Sie den Header &quot;X-Forwarded-For&quot;übergeben, finden Sie [here](rest-api-cookbook-servertoserver.md).

### Tatsächliche Einschränkungen und Endpunkte

Derzeit erlaubt das Standardlimit maximal 1 Anfrage pro Sekunde, mit einem anfänglichen Berst von 3 Anforderungen (einmalige Berücksichtigung der ersten Interaktion des identifizierten Clients, wodurch die Initialisierung erfolgreich abgeschlossen werden kann). Dies sollte sich nicht auf reguläre Geschäftsfälle für alle unsere Kunden auswirken.

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
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDK-Implementierungskennung

Da Clients, die die SDK mit Adobe Pass-Authentifizierung verwenden, nicht explizit mit Endpunkten interagieren, werden in diesem Abschnitt die bekannten Funktionen, ihr Verhalten beim Auftreten einer Drosselantwort und die zu ergreifenden Maßnahmen vorgestellt.

#### setRequest

Bei Erreichen der Drosselgrenze durch `setRequestor` -Funktion vom SDK aus verwenden, gibt das SDK einen CFG429-Fehlercode über zurück. `errorHandler` Callback.

#### getAuthorization

Bei Erreichen der Drosselgrenze durch `getAuthorization` -Funktion vom SDK aus verwenden, gibt das SDK einen Z100-Fehlercode zurück durch `errorHandler` Callback.

#### checkPreauthorizedResources

Bei Erreichen der Drosselgrenze durch `checkPreauthorizedResources` -Funktion vom SDK aus verwenden, gibt das SDK einen P100-Fehlercode zurück durch `errorHandler` Callback.

#### getMetadata

Bei Erreichen der Drosselgrenze durch `getMetadata` -Funktion aus dem SDK zurückgibt, gibt das SDK eine leere Antwort über `setMetadataStatus` Callback.

Weiterführende Informationen zu einzelnen Implementierungsdetails finden Sie in der jeweiligen SDK-Dokumentation.

- [JavaScript-SDK-API-Referenz](javascript-sdk-api-reference.md)
- [Android-SDK-API-Referenz](android-sdk-api-reference.md)
- [iOS/tvOS-API-Referenz](iostvos-sdk-api-reference.md)

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

Siehe [here](rest-api-cookbook-servertoserver.md) für weitere Details.

### Reagieren auf neuen Antwort-Code

Kunden, die eine benutzerdefinierte Implementierung (einschließlich Server-zu-Server-API) verwenden, um mit der API für die Pass-Authentifizierung zu interagieren, sollten sicherstellen, dass alle nachfolgenden Aufrufe, die nach Erhalt einer 429-zu-viele-Anfrage durchgeführt werden, eine Wartezeit von mindestens 1 Sekunde enthalten. Diese Wartezeit bietet die Möglichkeit, diesen Mechanismus zu ändern und eine gültige geschäftliche Antwort zu erhalten.

## Szenario für Einschränkungen

| Zeit seit der ersten Anfrage | Erhalt der Antwort | Erklärung |
|--------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| Second 0 | Aufruf erhält Erfolgsstatus-Code | 1 vom Limit aus verbrauchte Aufrufe |
| Second 0.3 | Aufruf erhält Erfolgsstatus-Code | 1 vom Limit aus verbrauchte Anrufe und 1 als &quot;Berst&quot;markierte Anrufe |
| Second 0.6 | Aufruf erhält Erfolgsstatus-Code | 1 vom Limit verbrauchte Aufrufe und 2 als &quot;Berst&quot;markierte Aufrufe |
| Zweiter 0,9 | Aufruf erhält Erfolgsstatus-Code | 1 vom Limit aus verbrauchte Anrufe und 3 als &quot;Berst&quot;markierte Anrufe |
| Zweiter 1.2 | Aufruf erhält Erfolgsstatus-Code | 2 vom Limit aus verbrauchte Anrufe und 3 als &quot;Berst&quot;markierte Anrufe |
| Zweiter 1.4 | Aufruf erhält 429 Status-Code | 2 aus der Begrenzung verbrauchte Anrufe und 3 als platziert markierte Anrufe und 1 Anruf erhält &quot;429 Zu viele Anfragen&quot; |
| Zweiter 1.6 | Aufruf erhält 429 Status-Code | 2 aus der Begrenzung verbrauchte Anrufe und 3 als platziert markierte Anrufe und 2 Anrufe erhalten &quot;429 Zu viele Anfragen&quot; |
| Zweiter 1.8 | Aufruf erhält 429 Status-Code | 2 aus der Begrenzung verbrauchte Anrufe und 3 als platziert markierte Anrufe und 3 Anrufe erhalten &quot;429 Zu viele Anfragen&quot;. |
| 2.1 | Aufruf erhält Erfolgsstatus-Code | 3 von der Begrenzung verbrauchte Anrufe und 3 als platziert markierte Anrufe und 3 Anrufe erhalten &quot;429 Zu viele Anfragen&quot; |
