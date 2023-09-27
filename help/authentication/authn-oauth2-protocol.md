---
title: Authentifizierung mit dem OAuth 2.0-Protokoll
description: Authentifizierung mit dem OAuth 2.0-Protokoll
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 0%

---

# Authentifizierung mit dem OAuth 2.0-Protokoll

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Obwohl SAML immer noch das Hauptprotokoll ist, das für die Authentifizierung von US-MVPDs und Unternehmen im Allgemeinen verwendet wird, gibt es einen klaren Trend zum Übergang zu OAuth 2.0 als Primärauthentifizierungsprotokoll. Das OAuth 2.0-Protokoll (https://tools.ietf.org/html/rfc6749) wurde hauptsächlich für Konsumentensites entwickelt und schnell von Internetgiganten wie Facebook, Google und Twitter übernommen.

OAuth 2.0 ist äußerst erfolgreich, was Unternehmen dazu veranlasst hat, ihre Infrastruktur langsam zu modernisieren, um sie zu unterstützen.



## Vorteile des Übergangs zu OAuth 2.0 {#adv-oauth2}

Auf hoher Ebene bietet das OAuth 2.0-Protokoll die gleiche Funktionalität wie das SAML-Protokoll, aber es gibt einige wichtige Unterschiede.

Einer davon ist, dass der Fluss des Aktualisierungs-Tokens als Möglichkeit verwendet werden kann, die Authentifizierung hinter den Kulissen zu aktualisieren. Dadurch kann der IdP (in diesem Fall die MVPDs) die Kontrolle behalten und gleichzeitig eine gute Benutzererfahrung ermöglichen, da der Benutzer sich aus Sicherheitsgründen nicht mehr oft anmelden muss.

Das Protokoll bietet außerdem mehr Flexibilität in Bezug auf die Daten, die verfügbar gemacht werden, da ein Dienstleister jetzt die Token verwenden kann, um auf andere APIs zuzugreifen und zusätzliche Informationen zu erhalten. Dies führt wiederum zu einem &quot;chattier&quot;-Protokoll für TVE-Anwendungsfälle, ermöglicht jedoch die für komplexe Workflows erforderliche Flexibilität.





## Anforderungen für den Wechsel zu OAuth 2.0 {#oauth-req}

Um die Authentifizierung mit OAuth 2.0 zu unterstützen, muss ein MVPD die folgenden Voraussetzungen erfüllen:

Zuallererst muss der MVPD sicherstellen, dass er die [Autorisierungs-Code-Förderung](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html) Fluss.

Nach Bestätigung, dass der Fluss unterstützt wird, muss uns der MVPD die folgenden Informationen zur Verfügung stellen:

* Authentifizierungsendpunkt
   * Der Endpunkt stellt den Autorisierungscode bereit, der später im Austausch für das Aktualisierungs- und Zugriffstoken verwendet wird.
* der /token-Endpunkt
   * Dadurch werden das Aktualisierungs-Token und das Zugriffstoken bereitgestellt
   * Das Aktualisierungs-Token muss stabil sein (es darf sich nicht bei jeder Anforderung eines neuen Zugriffstokens ändern
   * MVPD muss für jedes Aktualisierungstoken mehrere aktive Zugriffstoken zulassen
   * dieser Endpunkt tauscht auch ein Aktualisierungs-Token gegen ein Zugriffstoken
* benötigen wir eine **Endpunkt für Benutzerprofil**
   * Dieser Endpunkt stellt die userID bereit, die für ein Konto eindeutig sein muss und keine personenbezogenen Informationen enthalten sollte.
* die **/logout** Endpunkt (optional)
   * Die Adobe Pass-Authentifizierung leitet zu diesem Endpunkt weiter, stellt dem MVPD einen Weiterleitungs-URI bereit. An diesem Endpunkt kann das MVPD die Cookies auf dem Clientcomputer löschen oder eine beliebige Logik für die Abmeldung anwenden.
* Es wird dringend empfohlen, die Unterstützung autorisierter Clients zu nutzen (Client-Apps, die keine Benutzerautorisierungsseite Trigger haben)
* Außerdem benötigen wir:
   * **clientID** und **Client-Geheimnis** für die Integrationskonfigurationen
   * **Live-Zeit** (TTL)-Werte für das Aktualisierungs-Token und Zugriffstoken
   * Wir können dem MVPD einen Autorisierungs-Callback- und Abmelde-Callback-URI bereitstellen. Bei Bedarf können wir MVPDs auch eine Liste von IPs zur Verfügung stellen, die in Ihren Firewall-Einstellungen auf die Whitelist gesetzt werden sollen.


## Authentifizierungsfluss {#authn-flow}

Im Authentifizierungsfluss kommuniziert die Adobe Pass-Authentifizierung mit dem MVPD über das in der Konfiguration ausgewählte Protokoll. Der OAuth 2.0-Fluss ist im folgenden Bild dargestellt:



![Diagramm zur Anzeige des Authentifizierungsflusses in der Adobe-Authentifizierung, die mit dem MVPD über das in der Konfiguration ausgewählte Protokoll kommuniziert.](assets/authn-flow.png)

**Abbildung 1: OAuth 2.0-Authentifizierungsfluss**



## Authentifizierungsanfrage und Antwort {#authn-req-response}

Kurz gesagt, der Authentifizierungsfluss für MVPDs, die das OAuth 2.0-Protokoll unterstützen, folgt diesen Schritten:

1. Der Endbenutzer navigiert zur Site des Programmierers und wählt aus, sich mit seinen MVPD-Anmeldeinformationen anzumelden
1. Der auf der Seite des Programmierers installierte AccessEnabler sendet eine Authentifizierungsanforderung in Form einer HTTP-Anfrage an den Adobe Pass-Authentifizierungsendpunkt, den der Adobe Pass-Authentifizierungsendpunkt an den MVPD-Autorisierungsendpunkt weiterleitet.
1. Der MVPD-Autorisierungsendpunkt sendet einen Autorisierungscode an den Adobe Pass-Authentifizierungsendpunkt
1. Die Adobe Pass-Authentifizierung verwendet den erhaltenen Autorisierungscode, um ein Aktualisierungs-Token und ein Zugriffstoken vom Token-Endpunkt des MVPD anzufordern.
1. Ein Aufruf zum Abrufen von Benutzerinformationen und Metadaten kann an den Endpunkt des Benutzerprofils gesendet werden, falls die Benutzerinformationen nicht im Token enthalten sind
1. Das Authentifizierungstoken wird an den Endbenutzer übergeben, der jetzt erfolgreich auf der Programmierer-Site surfen kann.

   >[!NOTE]
   >
   >Das Aktualisierungs-Token wird verwendet, um ein neues Zugriffstoken zu erhalten, nachdem das aktuelle Zugriffstoken ungültig wird oder abläuft.


>[!IMPORTANT]
>
>Das Aktualisierungs-Token darf sich nicht ändern, wenn es in ein Zugriffstoken getauscht wird.

Diese Einschränkung beruht auf den Client-Flüssen, die es dem Server nicht erlauben, den AuthNToken zu aktualisieren, der für das OAuth 2.0-Protokoll auch das Aktualisierungstoken enthält.

Ein typischer Autorisierungsfluss führt einen Austausch des im AuthNToken gespeicherten Aktualisierungstokens durch, um ein Zugriffstoken zu erhalten, das anschließend zum Ausführen des Autorisierungsaufrufs im Namen des Benutzers verwendet wird, der sich zuvor authentifiziert hat. Wenn der Autorisierungsserver (der MVPD) das Aktualisierungstoken ändern und das alte ungültig machen sollte, können wir das gültige AuthNToken nicht aktualisieren. Aus diesem Grund müssen MVPDs stabile Aktualisierungstoken unterstützen, um OAuth 2.0-Integrationen für sie einrichten zu können.


## Migration von SAML zu OAuth 2.0 {#saml-auth2-migr}

Die Migration von Integrationen von SAML auf OAuth 2.0 wird von Adobe und dem MVPD durchgeführt. Es sind keine technischen Änderungen auf der Seite des Programmierers erforderlich, obwohl der Programmierer das Co-Branding auf der MVPD-Anmeldeseite überprüfen/testen möchte. Aus Sicht des MVPD sind die Endpunkte und andere Informationen erforderlich, die in Oauth 2.0-Anforderungen angefordert werden.

Um **Bewahren von SSO**, werden die Benutzer, die bereits über SAML ein Authentifizierungstoken erhalten haben, weiterhin als authentifiziert betrachtet und ihre Anfragen werden über die alte SAML-Integration weitergeleitet.

Aus technischer Sicht:

1. Adobe ermöglicht eine OAuth 2.0-Integration zwischen dem Programmierer und dem MVPD, OHNE die SAML-Integration zu löschen.
1. Nach der Aktivierung verwenden alle neuen Benutzer die OAuth 2.0-Flüsse.
1. Bereits authentifizierte Benutzer, die bereits über ein lokales AuthN-Token verfügen, das die SAML-Betreff-ID enthält, werden von Adobe automatisch über die SAML-Integration weitergeleitet.
1. Für die Benutzer in Schritt 3 behandelt Adobe sie nach Ablauf des von SAML generierten AuthN-Tokens als neue Benutzer und verhält sich wie die Benutzer in Schritt 2.
1. Adobe überprüft Nutzungsmuster, um festzustellen, wann die SAML-Integration sicher deaktiviert werden kann.
