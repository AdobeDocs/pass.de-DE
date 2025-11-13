---
title: Adobe Single Sign-On Service
description: Erfahren Sie mehr über den Adobe Pass SSO-Service, der eine nahtlose Authentifizierung über mehrere Geräte und Anwendungen ermöglicht.
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 2%

---


# Adobe Single Sign-On Service {#sso-service}

In diesem Dokument werden Anwendungsfälle, Endpunkte und APIs für den Adobe Single Sign-On Service beschrieben.

**Aktuelle Version - 1.0.0**

## Umfang {#scope}

Der Adobe Pass SSO Service ermöglicht eine nahtlose Authentifizierung über mehrere Geräte und Anwendungen hinweg und bietet ein einheitliches Benutzererlebnis bei gleichzeitiger Einhaltung von Sicherheits- und Compliance-Standards. Dieser Service deckt den wachsenden Bedarf an plattformübergreifender Authentifizierung in der heutigen Streaming-Landschaft mit mehreren Geräten.

## Überblick {#overview}

### Was es ist {#what-it-is}

Eine umfassende Single-Sign-On-Lösung, mit der sich Benutzer einmal authentifizieren und die Übertragung der Authentifizierung auf informierte Weise an andere Geräte verwalten können

### Aktuelle Herausforderungen bei der Authentifizierung {#current-challenges}

* Der Benutzer muss sich bei jedem Streaming-Service authentifizieren, für den er ein Abonnement hat
* Der Benutzer muss sich auf jedem Gerät oder jeder Anwendung separat authentifizieren
* Aufgrund der Schwierigkeit, ein Passwort im Authentifizierungsfluss auf einigen Plattformen einzugeben, kann dies zu erhöhten Abbruchraten führen

### Möglichkeit für einen Adobe Pass SSO-Service {#opportunity}

* Steigende Anzahl von Streaming-Services, die eine eigene Authentifizierung erfordern
* Wachsende Nachfrage nach nahtlosen geräteübergreifenden Erlebnissen
* Steigende Anzahl von Streaming-Geräten pro Haushalt
* Notwendigkeit einer einheitlichen Authentifizierung pro Haushalt

### Geschäftliche Vorteile {#business-benefits}

#### Inhalts-Provider {#content-providers}

* **Erhöhte Benutzerinteraktion** - Nahtloses Erlebnis führt zu längeren Sitzungszeiten
* **Geringere Reibungsverluste** - Niedrigere Authentifizierungsbarrieren erhöhen den Content-Verbrauch
* **Verbesserte Kundenbindung** - Besseres Anwendererlebnis reduziert Abwanderung
* **Kostensenkung** - Weniger Support-Tickets im Zusammenhang mit Authentifizierungsproblemen

#### Endbenutzer {#end-users}

* **Nahtloses Erlebnis** - Einmalige Authentifizierung, Zugriff überall
* **Zeitersparnis** - Keine wiederholten Anmeldevorgänge
* **Geräteflexibilität** - unterbrechungsfreies Umschalten zwischen Geräten
* **Consistent Experience** - Einheitliche Authentifizierung über alle Plattformen hinweg

#### IDs (MVPDs, Telekommunikationsgeräte usw.) {#idps}

* MVPDs können mit einem authentifizierten SSO über zusätzliche Geräte informiert werden
* **Verbesserte Benutzerzufriedenheit** - Verbessertes Authentifizierungserlebnis
* **Geringere Supportlast** - Weniger Support-Aufrufe im Zusammenhang mit der Authentifizierung
* **Wettbewerbsvorteil** - Besseres Anwendererlebnis als Mitbewerber

## Anwendungsfälle {#use-cases}

### Identitätszuordnung {#identity-mapping}

Der Service ermöglicht die Verknüpfung eines D2C- und eines TVE-Kontos in derselben App.

Weitere Streaming-Services erstellen Pakete, die an Dritte verkauft werden sollen (MVPD/Virtual MVPD/Telco usw.). Benutzer müssen am Ende möglicherweise mehrere Konten in derselben App verarbeiten. Um eine nahtlose Authentifizierung zu ermöglichen, muss ein Service diese Konten verbinden, um die Anmeldung zu vereinfachen.

Benutzer authentifizieren sich mit ihrem D2C-Konto und authentifizieren sich dann EINMAL mit einem anderen Konto (z. B. MVPD). Mit unserem Service werden diese Konten verknüpft, was bedeutet, dass in Zukunft nachfolgende Authentifizierungen auf verschiedenen Geräten nur mit dem D2C-Konto durchgeführt werden können.

### Geräteübergreifendes SSO {#cross-device-sso}

Die Authentifizierung auf einem mit dem Fernsehgerät verbundenen Gerät kann umständlicher sein als auf einem Mobiltelefon. Ein gutes Benutzererlebnis bestünde darin, sich am Telefon zu authentifizieren und diese Authentifizierung dann an das Smart-TV weiterzugeben.

## Schlüsselkomponenten {#key-components}

* **Service Token API** - Schlüsselkomponente, die den Single-Sign-On-Mechanismus sicher verwaltet
* **List API** - Die Anwendung kann Benutzenden dabei helfen, die Liste der Geräte in ihrem Ökosystem zu verstehen
* **Link-API** - Anwendung, mit der Benutzende zusätzliche Geräte zu ihrem Ökosystem hinzufügen können
* **API trennen** - Anwendung, die es dem Benutzer ermöglicht, Geräte in seinem Ökosystem zu entfernen

## Anwendungsbeispiele - detailliert {#use-cases-detailed}

### D2C-TVE SSO {#d2c-tve-sso}

Dieser Anwendungsfall ermöglicht die Verknüpfung zwischen D2C- und TVE(MVPD)-Anmeldeinformationen auf einem Gerät und die Nutzung dieses verknüpften Profils auf anderen Geräten innerhalb derselben Anwendung.

![D2C-TVE SSO-Fluss](../../../assets/sso_service_d2c_1.png)

![Geräteübergreifender SSO-Fluss](../../../assets/sso_service_d2c_2.png)

### Geräteübergreifendes SSO {#cross-device-sso-detailed}

Dieser Anwendungsfall ermöglicht es Benutzenden, die sich bereits auf einem Gerät authentifiziert haben, das authentifizierte Profil auf derselben auf anderen Geräten installierten D2C- oder TVE-Anwendung wiederzuverwenden (Anwendung, die zur Implementierung von Adobe Pass REST V2 für die Authentifizierung und Autorisierung erforderlich ist).

## Integrieren des Adobe SSO Service in eine D2C-TVE-Anwendung {#integration}

### Schritt 1: Abrufen einer gemeinsamen Kennung {#step-1}

Um den Adobe SSO-Service zu integrieren, sollte eine Anwendungsimplementierung eine eindeutige und persistente ID festlegen, die als gemeinsames SSO-Identifizierungsattribut in der X-SSO-ID verwendet werden kann. Dies kann durch die Authentifizierung eines Benutzers mit einem D2C-Service erreicht werden und ein Attribut zu dieser Authentifizierung beibehalten werden.

### Schritt 2: Abrufen eines Service-Tokens {#step-2}

Durch Verwendung der allgemeinen Kennung in der X-SSO-ID im Endpunkt POST /serviceToken wird ein signiertes JWT mit der folgenden Payload abgerufen:

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

Das Service-Token hat eine Ablaufzeit von „iat“ (ausgestellt am) und von „exp“ (ablaufend), in der das Intervall des Service-Tokens gültig ist. Im Falle eines Ablaufs kann ein neues JWT mithilfe des Endpunkts GET /serviceToken mit dem abgelaufenen Service-Token abgerufen werden.

### Schritt 3: Authentifizierung mit der Adobe Pass REST API V2 mit einer TVE MVPD {#step-3}

Die Authentifizierung mit Adobe Pass sollte mithilfe des Service-Tokens implementiert werden: [REST API V2 - Single Sign-On Service Token Flows](https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### Schritt 4: Verknüpfen eines anderen Geräts {#step-4}

Von der authentifizierten Anwendung kann ein Link erstellt werden, der auf einem anderen Gerät mithilfe der /link-API verwendet werden kann

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

Der „Code“ ist ein kurzlebiger Link-Code in einer Form einer Sequenz von 6 Ziffern, die der Benutzer in eine nicht authentifizierte Anwendung auf einem sekundären Gerät einführt

### Schritt 5: Abrufen der Single-Sign-On-Authentifizierung {#step-5}

Auf einem anderen Gerät kann die Anwendung, sobald der Benutzer den Code eingeführt hat:

* Abrufen von Identitäten aus dem D2C-Service
* Abrufen des MVPD-Profils aus Adobe Pass mit der REST V2-API

Das MVPD-Profil ist über SSO für den Zeitraum gültig, für den die erste Authentifizierung abgerufen wurde.

Falls MVPD das Profil ungültig macht oder der Benutzer sich von MVPD abmeldet, verfügt das Programm, das Adobe Pass REST API V2 verwendet, nicht mehr über einen Profildatensatz und sollte den Benutzer auffordern, sich erneut bei MVPD zu authentifizieren.

### Schritt 6: Single Sign-On auf anderen Geräten verwalten {#step-6}

Eine Anwendung kann mithilfe der /list-API Informationen über alle anderen Geräte abrufen, die mit derselben gemeinsamen Kennung verknüpft sind.

Ein Gerät kann jederzeit aus dem Single Sign-On entfernt werden, indem die Verknüpfung dieses Geräts mit der /unlink-API aufgehoben wird.

## APIs {#apis}

### Service-Token-API {#service-token-api}

#### Beschreibung {#service-token-description}

Die Service-Token-API kann verwendet werden, um Service-Token anzufordern und zu verwalten, die Single Sign-On (SSO)-Funktionen zwischen mehreren Anwendungen oder Geräten ermöglichen. Diese Service-Token identifizieren authentifizierte Profile (SSO-Profile) und sind für die Einrichtung und Wartung von SSO-Verbindungen unerlässlich.

>[!WARNING]
>
>Service-Token enthalten vertrauliche Authentifizierungsinformationen. Anwendungen müssen diese Token sicher verarbeiten und dürfen sie niemals nicht vertrauenswürdigen Parteien zugänglich machen.

Die Service-Token-API stellt zwei primäre Endpunkte bereit:

* **POST /api/{serviceProvider}/serviceToken** - Abrufen eines neu erstellten JWS-Service-Tokens
* **GET /api/{serviceProvider}/serviceToken** - Vorhandenes JWS-Service-Token aktualisieren

Wenn die Anfrage zur Service-Token-API aufgrund eines Fehlers des Adobe Pass-Authentifizierungs-Services nicht verarbeitet werden konnte, werden zusätzliche Fehlerinformationen als Teil der API-Antwort aufgenommen.

#### POST - serviceToken {#post-service-token}

##### Anfrage {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Pfadparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Die Kennung des Dienstleisters, für den das Token angefordert wird.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Generierung der Bearer-Token-Payload wird in der Dokumentation zur <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">-Kopfzeile </a>.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ap-device-identifier</td>
      <td>
         Die Erstellung der Payload der Gerätekennung wird in der Header-Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> beschrieben.
         <br/><br/>
         Diese Kennung wird als Standard-SSO-Kennung verwendet, wenn keine X-SSO-ID angegeben wird.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">x-device-info</td>
      <td>
         Die Geräteinformationen, wie in der Kopfzeilendokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a> angegeben.
         <br/><br/>
         <b>dringend empfohlen</b> zu verwenden, wenn die Geräteplattform der Anwendung die explizite Angabe gültiger Werte erlaubt.
         <br/><br/>
         Das Adobe Pass-Authentifizierungs-Backend führt explizit eingestellte Werte mit implizit extrahierten Werten zusammen. Wenn keine Standardwerte angegeben werden, werden die extrahierten Werte verwendet.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-LINK</td>
      <td>
         Der Link-Code, der diese Anfrage mit einem vorhandenen authentifizierten Profil verknüpft. Wenn angegeben, enthält die Antwort ein Service-Token für SSO mit dem Profil, das den Link-Code generiert hat.
         <br/><br/>
         Dies wird in der Regel verwendet, wenn eine sekundäre Anwendung oder ein sekundäres Gerät über eine primäre Anwendung oder ein Gerät eine Verbindung zu einem authentifizierten Profil herstellen möchte.
      </td>
      <td>Erforderlich, wenn keine x-SSO-ID angegeben ist</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         Die allgemeine Kennung, auf der die Anwendung SSO-basierte Anforderungen stellt.
         <br/><br/>
         Wenn angegeben, wird diese Kennung verwendet, um ein gemeinsames SSO-Profil für alle Geräte und/oder Anwendungen zu erstellen.
      </td>
      <td>Erforderlich, wenn kein x-SSO-Link angegeben wird</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Akzeptieren</td>
      <td>
         Der von der Client-Anwendung akzeptierte Medientyp.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">user-agent</td>
      <td>Der Benutzeragent der Client-Anwendung.</td>
      <td>fakultativ</td>
   </tr>
</table>

##### Antwort {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Erstellt</td>
      <td>
        Das Service-Token wurde erfolgreich generiert und im Antworttext zurückgegeben.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Fehlerhafte Anfrage</td>
      <td>
        Die Anfrage ist ungültig. Der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Nicht autorisiert</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interner Server-Fehler</td>
      <td>
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
</table>

###### Erfolg {#success-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>HTTP-Status (z. B. „CREATED„)</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Eine Base64-kodierte JSON-Web-Signatur (JWS), die das Service-Token enthält. Dieses Token kann in nachfolgenden API-Aufrufen verwendet werden, um das authentifizierte Profil zu identifizieren und SSO-Funktionen zu aktivieren.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoche ms oder 0 bei Fehler</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoche ms oder 0 bei Fehler</td>
      <td><i>required</i></td>
   </tr>
</table>

###### Fehler {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Der Antworttext kann zusätzliche Fehlerinformationen bereitstellen, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.</td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples-post-service-token}

### &#x200B;1. Anfordern eines neuen Service-Tokens (mit SSO-ID)

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### &#x200B;2. Anfordern eines neuen Service-Tokens (mit SSO-Link)

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### Anfrage {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Pfadparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Die Kennung des Dienstleisters, für den das Token angefordert wird.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Generierung der Bearer-Token-Payload wird in der Dokumentation zur <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">-Kopfzeile </a>.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         Ein zuvor abgerufenes Service-Token, das aktualisiert werden muss.
         <br/><br/>
         Dieses Token muss gültig oder kürzlich abgelaufen sein, um aktualisiert werden zu können.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Akzeptieren</td>
      <td>
         Der von der Client-Anwendung akzeptierte Medientyp.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">user-agent</td>
      <td>Der Benutzeragent der Client-Anwendung.</td>
      <td>fakultativ</td>
   </tr>
</table>

##### Antwort {#get-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Das Service-Token wurde erfolgreich aktualisiert und im Antworttext zurückgegeben.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Fehlerhafte Anfrage</td>
      <td>
        Die Anfrage ist ungültig. Der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Nicht autorisiert</td>
      <td>
        Das Zugriffstoken oder Service-Token ist ungültig. Der Client muss ein neues Zugriffstoken oder Service-Token abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interner Server-Fehler</td>
      <td>
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
</table>

###### Erfolg {#success-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>HTTP-Status (z. B. „OK„)</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Eine Base64-kodierte JSON-Web-Signatur (JWS), die das aktualisierte Service-Token enthält. Dieses Token kann in nachfolgenden API-Aufrufen verwendet werden, um das authentifizierte Profil zu identifizieren und SSO-Funktionen zu aktivieren.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoche ms oder 0 bei Fehler</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoche ms oder 0 bei Fehler</td>
      <td><i>required</i></td>
   </tr>
</table>

###### Fehler {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Der Antworttext kann zusätzliche Fehlerinformationen bereitstellen, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.</td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples-get-service-token}

### &#x200B;1. Anfordern der Aktualisierung eines Service-Tokens

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### Link-API {#link-api}

#### Beschreibung {#link-description}

Die Link-API kann verwendet werden, um einen Link-Code (oder QR-Code) anzufordern, der Single Sign-On (SSO) zwischen mehreren Anwendungen oder Geräten aktivieren kann. Dieser Link-Code ermöglicht es Benutzenden, eine neue Anwendung oder ein neues Gerät mit einem vorhandenen authentifizierten Profil (SSO-Profil) zu verbinden und so ein nahtloses SSO-Erlebnis über Anwendungen oder Geräte hinweg bereitzustellen.

Für die Link-API muss im AD-Service-Token-Header ein gültiges Service-Token angegeben werden.

Der generierte Link-Code wird in der Regel Benutzern auf einer primären Anwendung oder einem Gerät angezeigt und auf einer sekundären Anwendung oder einem sekundären Gerät eingegeben, um die SSO-Verbindung herzustellen. Der Link-Code hat eine begrenzte Gültigkeitsdauer (in der Regel 5-30 Minuten) und ist für die einmalige Verwendung vorgesehen.

Falls die Link-API-Anfrage aufgrund eines Adobe Pass-Authentifizierungs-Service-Fehlers nicht verarbeitet werden konnte, werden zusätzliche Fehlerinformationen als Teil des Link-API-Antwortergebnisses aufgenommen.

#### POST - Link {#post-link}

##### Anfrage {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Pfadparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Die Kennung des Dienstleisters, für den das Token angefordert wird.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Generierung der Bearer-Token-Payload wird in der Dokumentation zur <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">-Kopfzeile </a>.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ap-device-identifier</td>
      <td>Die Erstellung der Payload der Gerätekennung wird in der Header-Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> beschrieben.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         Die Erstellung des Service-Tokens wird in der Dokumentation zur Service-Token-API beschrieben.
         <br/><br/>
         Dieses Service-Token identifiziert das authentifizierte Profil, für das der Link-Code generiert wird.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Akzeptieren</td>
      <td>
         Der von der Client-Anwendung akzeptierte Medientyp.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">user-agent</td>
      <td>Der Benutzeragent der Client-Anwendung.</td>
      <td>fakultativ</td>
   </tr>
</table>

##### Antwort {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Erstellt</td>
      <td>
        Der Link-Code wurde erfolgreich generiert und im Antworttext zurückgegeben.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Fehlerhafte Anfrage</td>
      <td>
        Die Anfrage ist ungültig. Der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Nicht autorisiert</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interner Server-Fehler</td>
      <td>
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
</table>

###### Erfolg {#success-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>HTTP-Status (z. B. „CREATED„)</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Relation</td>
      <td>Ein kurzer numerischer oder alphanumerischer Code, der auf einer sekundären Anwendung oder einem sekundären Gerät zum Herstellen einer SSO-Verbindung verwendet werden kann.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Der Zeitstempel (in Millisekunden seit Epoche), wann der Link-Code gültig wird.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Der Zeitstempel (in Millisekunden seit Epoche), wann der Link-Code abläuft.</td>
      <td><i>required</i></td>
   </tr>
</table>

###### Fehler {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Der Antworttext kann zusätzliche Fehlerinformationen bereitstellen, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.</td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples-post-link}

### &#x200B;1. Anfordern eines Link-Codes für ein vorhandenes authentifiziertes Profil

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### Verknüpfung der API aufheben {#unlink-api}

#### Beschreibung {#unlink-description}

Die Unlink-API kann verwendet werden, um das Entfernen eines oder mehrerer Geräte aus einem authentifizierten Profil (SSO-Profil) anzufordern. Mit dieser API können Benutzer Geräte von ihrer SSO-Einrichtung trennen und so steuern, welche Geräte Zugriff auf ihr authentifiziertes Profil haben.

>[!WARNING]
>
>Für die Unlink-API muss im AD-Service-Token-Header ein gültiges Service-Token angegeben werden.

Falls die Unlink API-Anfrage aufgrund eines Fehlers der Adobe Pass-Authentifizierungs-Services nicht verarbeitet werden konnte, werden zusätzliche Fehlerinformationen als Teil des Ergebnisses der Unlink API-Antwort einbezogen.

#### POST - Link aufheben {#post-unlink}

##### Anfrage {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/api/{serviceProvider}/unlink</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Pfadparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Die Kennung des Dienstleisters.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Hauptteilparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Geräte</td>
      <td>
         Array von Geräte-IDs, deren Verknüpfung aufgehoben werden soll.
         <br/><br/>
         Beispiel:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Generierung der Bearer-Token-Payload wird in der Dokumentation zur <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">-Kopfzeile </a>.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>
         Der akzeptierte Medientyp für die gesendeten Ressourcen.
         <br/><br/>
         Es muss application/json sein.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ap-device-identifier</td>
      <td>Die Erstellung der Payload der Gerätekennung wird in der Header-Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> beschrieben.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         Die Erstellung des Service-Tokens wird in der Dokumentation zur Service-Token-API beschrieben.
         <br/><br/>
         Dieses Service-Token identifiziert das authentifizierte Profil, für das die Verknüpfung mit Geräten aufgehoben wird.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Akzeptieren</td>
      <td>
         Der von der Client-Anwendung akzeptierte Medientyp.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">user-agent</td>
      <td>Der Benutzeragent der Client-Anwendung.</td>
      <td>fakultativ</td>
   </tr>
</table>

##### Antwort {#post-unlink-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Die Verknüpfung der angeforderten Geräte mit der SSO-Einrichtung wurde erfolgreich aufgehoben.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Fehlerhafte Anfrage</td>
      <td>
        Die Anfrage ist ungültig. Der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Nicht autorisiert</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode nicht zulässig</td>
      <td>
        Die HTTP-Methode ist ungültig. Der Client muss eine HTTP-Methode verwenden, die für die angeforderte Ressource zulässig ist, und erneut versuchen.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interner Server-Fehler</td>
      <td>
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
</table>

###### Erfolg {#success-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>Informationen zum Operationsergebnis: „OK“</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         Liste der erfolgreich getrennten Geräte.
         <br/><br/>
         Beispiel:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>required</i></td>
   </tr>
</table>

###### Fehler {#error-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Der Antworttext kann zusätzliche Fehlerinformationen bereitstellen, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.</td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples-post-unlink}

### &#x200B;1. Anfrage zum Aufheben der Verknüpfung von Geräten mit einem SSO-Profil

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### &#x200B;2. Anfrage zum Aufheben der Verknüpfung von Geräten mit teilweisem Erfolg

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### Listen-API {#list-api}

#### Beschreibung {#list-description}

Mit der Listen-API kann eine Liste von Geräten abgerufen werden, die derzeit mit einem authentifizierten Profil (SSO-Profil) verbunden sind. Diese API ermöglicht es Benutzenden und Anwendungen, anzuzeigen, welche Geräte Teil ihrer SSO-Einrichtung sind, und bietet Sichtbarkeit und Verwaltungsfunktionen für ihr authentifiziertes Erlebnis auf mehreren Geräten.

>[!WARNING]
>
>Für die Listen-API muss im AD-Service-Token-Header ein gültiges Service-Token angegeben werden.

Die Listen-API gibt Details zu jedem Gerät im authentifizierten Profil (SSO-Profil) zurück, einschließlich Geräte-IDs und Metadaten, die Benutzern bei der Erkennung ihrer Geräte helfen können. Diese Informationen können Benutzenden helfen, fundierte Entscheidungen darüber zu treffen, welche Geräte im SSO-Setup verbleiben sollen.

Falls die Liste-API-Anfrage aufgrund eines Adobe Pass-Authentifizierungs-Service-Fehlers nicht verarbeitet werden konnte, werden zusätzliche Fehlerinformationen als Teil des Listen-API-Antwortergebnisses aufgenommen.

#### GET - Liste {#get-list}

##### Anfrage {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/api/{serviceProvider}/list</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Pfadparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Die Kennung des Dienstleisters.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Generierung der Bearer-Token-Payload wird in der Dokumentation zur <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">-Kopfzeile </a>.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ap-device-identifier</td>
      <td>Die Erstellung der Payload der Gerätekennung wird in der Header-Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> beschrieben.</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         Die Erstellung des Service-Tokens wird in der Dokumentation zur Service-Token-API beschrieben.
         <br/><br/>
         Dieses Service-Token identifiziert das authentifizierte Profil, für das die Geräteliste abgerufen wird.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Akzeptieren</td>
      <td>
         Der von der Client-Anwendung akzeptierte Medientyp.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">user-agent</td>
      <td>Der Benutzeragent der Client-Anwendung.</td>
      <td>fakultativ</td>
   </tr>
</table>

##### Antwort {#get-list-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Die Liste der Geräte im SSO-Setup wurde erfolgreich abgerufen und im Antworttext zurückgegeben.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Fehlerhafte Anfrage</td>
      <td>
        Die Anfrage ist ungültig. Der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Nicht autorisiert</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Übersicht über die Dynamic Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode nicht zulässig</td>
      <td>
        Die HTTP-Methode ist ungültig. Der Client muss eine HTTP-Methode verwenden, die für die angeforderte Ressource zulässig ist, und erneut versuchen.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interner Server-Fehler</td>
      <td>
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.
      </td>
   </tr>
</table>

###### Erfolg {#success-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Geräte</td>
      <td>
         JSON mit einer Zuordnung von Schlüssel-/Wert-Paaren.
         <br/><br/>
         <b>Key:</b> deviceId - Die Payload der Gerätekennung, wie in der Kopfzeilendokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> beschrieben
         <br/><br/>
         <b>Wert:</b>-Attribute - JSON, das eine Zuordnung von Geräte-Metadatenattributen enthält, einschließlich:
         <ul>
            <li>Gerätetyp</li>
            <li>Plattform</li>
            <li>Benutzeragent</li>
            <li>Andere relevante Metadaten, die bei der Identifizierung des Geräts helfen</li>
         </ul>
         Die Werte für die Attribute können einfach sein (Zeichenfolge, Ganzzahl, boolesch usw.)
      </td>
      <td><i>required</i></td>
   </tr>
</table>

###### Fehler {#error-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Der Antworttext kann zusätzliche Fehlerinformationen bereitstellen, die der Dokumentation <a href="https://experienceleague.adobe.com/de/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Erweiterte Fehlercodes</a> entsprechen.</td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples-get-list}

### &#x200B;1. Auflisten von Geräten in einem SSO-Profil anfordern

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. Anfrage zur Auflistung von Geräten ohne verknüpfte Geräte

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Antwort]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## Fehler-Codes {#error-codes}

### Struktur der Fehlerantwort {#error-structure}

Alle Fehlerantworten enthalten diese Felder:

| Feld | Typ | Beschreibung |
|:---|:---|:---|
| Status | Ganzzahl | HTTP-Status-Code (400, 401, 500 usw.) |
| Code | Zeichenfolge | Maschinenlesbarer Fehlercode |
| Nachricht | Zeichenfolge | Lesbare Beschreibung |
| Handlung | Zeichenfolge | Vorgeschlagene Aktion für Client |
| helpUrl | Zeichenfolge | Dokumentations-Link |
| Spur | Zeichenfolge | Eindeutige Anfrage-ID (UUID) für die Korrelation |

**Beispiel für eine Fehlerantwort**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=de",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**Beispiel für eine erfolgreiche Antwort**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>Erfolgsantworten enthalten KEIN Fehlerfeld. Fehlerantworten enthalten KEINE Erfolgsfelder.

### Fehlerkatalog {#error-catalog}

#### Allgemeine Fehler

| Code | Status | Nachricht | Handlung |
|:---|:---|:---|:---|
| token_invalid | 400 | Das angegebene Token ist ungültig | get_new_token |
| token_expi | 401 | Das Token ist abgelaufen | get_new_token |
| header_missing | 400 | Eine erforderliche Kopfzeile fehlt | check_headers |
| nicht befugt | 401 | Nicht autorisierter Zugriff | Keine |
| internal_error | 500 | Ein interner Fehler ist aufgetreten | Keine |

#### Validierungsfehler

| Code | Status | Nachricht | Handlung |
|:---|:---|:---|:---|
| request_null | 400 | Anfrageobjekt darf nicht null sein | Keine |

#### POST-ServiceToken-Fehler

| Code | Status | Nachricht | Handlung |
|:---|:---|:---|:---|
| header_missing | 400 | Für POST-Anfragen ist entweder der Header „x-sso-id“ oder „x-sso-link“ erforderlich | check_headers |
| header_missing | 400 | Für POST-Anfragen ist die Kopfzeile „AP-Device-Identifier“ erforderlich | check_headers |

#### GET ServiceToken-Fehler

| Code | Status | Nachricht | Handlung |
|:---|:---|:---|:---|
| header_missing | 400 | Für GET-Anfragen ist die Kopfzeile „AD-Service-Token“ erforderlich | check_headers |
| header_invalid | 401 | Ungültige JWT-Signatur im AD-Service-Token | get_new_token |
| header_invalid | 401 | Fehler beim Überprüfen der JWT-Signatur | get_new_token |
| header_invalid | 401 | JWT-Betreff (Sub) fehlt oder ist im AD-Service-Token leer | get_new_token |
| header_invalid | 401 | Fehler beim Extrahieren des JWT-Betreffs | get_new_token |

#### Fehler bei der Link-Validierung

| Code | Status | Nachricht | Handlung |
|:---|:---|:---|:---|
| header_missing | 401 | Für Linkanforderungen ist die Kopfzeile „AD-Service-Token“ erforderlich | check_headers |
| header_invalid | 401 | Ungültige JWT-Signatur im AD-Service-Token | get_new_token |
| header_invalid | 401 | Fehler beim Überprüfen der JWT-Signatur | get_new_token |

#### Aufheben der Verknüpfung mit Validierungsfehlern

| Code | Status | Nachricht | Handlung |
|:---|:---|:---|:---|
| header_missing | 401 | Für Anfragen zum Aufheben der Verknüpfung ist die Kopfzeile „AD-Service-Token“ erforderlich. | check_headers |
| header_invalid | 401 | Ungültige JWT-Signatur im AD-Service-Token | get_new_token |
| header_invalid | 401 | Fehler beim Überprüfen der JWT-Signatur | get_new_token |
| header_invalid | 401 | JWT-Betreff (Sub) fehlt oder ist im AD-Service-Token leer | get_new_token |
| request_invalid | 400 | Geräteliste darf nicht null oder leer sein | check_request_body |

#### Fehler bei der Listenvalidierung

| Code | Status | Nachricht | Handlung |
|:---|:---|:---|:---|
| header_missing | 401 | Für Listenanfragen ist die Kopfzeile AD-Service-Token erforderlich | check_headers |
| header_invalid | 401 | Ungültige JWT-Signatur im AD-Service-Token | get_new_token |
| header_invalid | 401 | Fehler beim Überprüfen der JWT-Signatur | get_new_token |
| header_invalid | 401 | JWT-Betreff (Sub) fehlt oder ist im AD-Service-Token leer | get_new_token |
| header_invalid | 401 | Fehler beim Extrahieren des JWT-Betreffs | get_new_token |

