---
title: Authentifizierung mit dem OAuth 2.0-Protokoll
description: Authentifizierung mit dem OAuth 2.0-Protokoll
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 0%

---

# Authentifizierung mit dem OAuth 2.0-Protokoll

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Überblick {#overview}

Obwohl SAML immer noch das Hauptprotokoll für die Authentifizierung von US-amerikanischen MVPDs und Unternehmen im Allgemeinen ist, gibt es einen klaren Trend hin zu OAuth 2.0 als primärem Authentifizierungsprotokoll. Das OAuth 2.0-Protokoll (https://tools.ietf.org/html/rfc6749) wurde hauptsächlich für Verbraucherwebsites entwickelt und von Internetriesen wie Facebook, Google und Twitter schnell übernommen.

OAuth 2.0 ist äußerst erfolgreich. Dies hat Unternehmen dazu veranlasst, ihre Infrastruktur langsam zu aktualisieren, um sie zu unterstützen.



## Vorteile des Wechsels zu OAuth 2.0 {#adv-oauth2}

Auf allgemeiner Ebene bietet das OAuth 2.0-Protokoll dieselben Funktionen wie das SAML-Protokoll, es gibt jedoch einige wichtige Unterschiede.

Eine davon ist die Tatsache, dass der Fluss von Aktualisierungstoken als Möglichkeit verwendet werden kann, die Authentifizierung hinter den Kulissen zu aktualisieren. Dadurch kann der IdP (in diesem Fall die MVPDs) die Kontrolle behalten und gleichzeitig ein gutes Benutzererlebnis bieten, da sich der Benutzer aufgrund von Sicherheitsbedenken nicht mehr oft anmelden muss.

Das Protokoll bietet außerdem mehr Flexibilität in Bezug auf die Daten, die verfügbar gemacht werden, da ein Dienstleister jetzt die Token verwenden kann, um auf andere APIs zuzugreifen und zusätzliche Informationen zu erhalten. Dies wiederum führt zu einem „Chattier“-Protokoll für TVE-Anwendungsfälle, ermöglicht jedoch die Flexibilität, die für komplexe Workflows erforderlich ist.





## Voraussetzungen für den Wechsel zu OAuth 2.0 {#oauth-req}

Um die Authentifizierung mit OAuth 2.0 zu unterstützen, muss eine MVPD die folgenden Voraussetzungen erfüllen:

Zuallererst muss die MVPD sicherstellen, dass sie den [Authorization Code Grant](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html) unterstützt.

Nachdem der MVPD bestätigt hat, dass er den Fluss unterstützt, muss er uns die folgenden Informationen zur Verfügung stellen:

* Der Authentifizierungsendpunkt
   * Der Endpunkt gibt den Autorisierungs-Code an, der später im Austausch für das Aktualisierungs- und Zugriffstoken verwendet wird
* Der Endpunkt /token
   * Dadurch werden das Aktualisierungstoken und das Zugriffstoken bereitgestellt
   * Das Aktualisierungstoken muss stabil sein (es darf sich nicht jedes Mal ändern, wenn wir ein neues Zugriffstoken anfordern)
   * Der MVPD muss für jedes Aktualisierungstoken mehrere aktive Zugriffstoken zulassen
   * Dieser Endpunkt tauscht auch ein Aktualisierungs-Token gegen ein Zugriffs-Token aus
* Wir benötigen einen **Endpunkt für „user-profile“**
   * Dieser Endpunkt gibt die Benutzer-ID an, die für ein Konto eindeutig sein muss und keine persönlich identifizierbaren Informationen enthalten sollte
* Der Endpunkt **/Logout** (optional)
   * Die Adobe Pass-Authentifizierung leitet zu diesem Endpunkt um und stellt dem MVPD einen Redirect-Back-URI bereit. An diesem Endpunkt kann der MVPD die Cookies auf dem Client-Computer löschen oder eine beliebige Logik zum Abmelden anwenden
* Es wird dringend empfohlen, Unterstützung für autorisierte Clients zu haben (Client-Apps, die keine Benutzerautorisierungsseite Trigger haben)
* Wir benötigen außerdem:
   * **clientID** und **client secret** für die Integrationskonfigurationen aus
   * **Time to Live** (TTL)-Werte für das Aktualisierungs-Token und Zugriffs-Token
   * Wir können der MVPD einen Autorisierungs-Callback und einen Abmelde-Callback-URI bereitstellen. Bei Bedarf können wir auch MVPDs mit einer Liste von IPs zur Verfügung stellen, die in Ihren Firewall-Einstellungen auf die Whitelist gesetzt werden.


## Authentifizierungsfluss {#authn-flow}

Im Authentifizierungsfluss kommuniziert die Adobe Pass-Authentifizierung mit der MVPD über das in der Konfiguration ausgewählte Protokoll. Der OAuth 2.0-Fluss wird in der folgenden Abbildung dargestellt:



![Diagramm zum Anzeigen des Authentifizierungsflusses in der Adobe-Authentifizierung, der mit der MVPD über das in der Konfiguration ausgewählte Protokoll kommuniziert.](../assets/authn-flow.png)

**Abbildung 1: OAuth 2.0-Authentifizierungsfluss**



## Authentifizierungsanfrage und Antwort {#authn-req-response}

Kurz gesagt, der Authentifizierungsfluss für MVPDs, die das OAuth 2.0-Protokoll unterstützen, folgt diesen Schritten:

1. Der Endbenutzer navigiert zur Website des Programmierers und meldet sich mit seinen MVPD-Anmeldeinformationen an
1. Der auf der Programmiererseite installierte AccessEnabler sendet eine Authentifizierungsanfrage in Form einer HTTP-Anfrage an den Adobe Pass-Authentifizierungsendpunkt, den der Adobe Pass-Authentifizierungsendpunkt an den MVPD-Autorisierungsendpunkt weiterleitet.
1. Der MVPD-Autorisierungsendpunkt sendet einen Autorisierungscode an den Adobe Pass-Authentifizierungsendpunkt
1. Die Adobe Pass-Authentifizierung verwendet den empfangenen Autorisierungs-Code, um ein Aktualisierungstoken und ein Zugriffstoken vom Token-Endpunkt von MVPD anzufordern
1. Ein Aufruf zum Abrufen von Benutzerinformationen und Metadaten kann an den Benutzerprofil-Endpunkt gesendet werden, falls die Benutzerinformationen nicht im Token enthalten sind
1. Das Authentifizierungs-Token wird an den Endbenutzer übergeben, der jetzt erfolgreich die Programmierer-Site durchsuchen kann

   >[!NOTE]
   >
   >Das Aktualisierungs-Token wird verwendet, um ein neues Zugriffs-Token abzurufen, nachdem das aktuelle Zugriffs-Token ungültig wird oder abläuft.


>[!IMPORTANT]
>
>Das Aktualisierungs-Token darf sich nicht ändern, wenn es gegen ein Zugriffs-Token eingetauscht wird.

Diese Einschränkung resultiert aus Client-Flüssen, die es dem Server nicht erlauben, das AuthNToken zu aktualisieren, das für das OAuth 2.0-Protokoll auch das Aktualisierungstoken enthält.

Ein typischer Autorisierungsfluss führt einen Austausch des im AuthNToken gespeicherten Aktualisierungs-Tokens gegen ein Zugriffs-Token durch, das anschließend verwendet wird, um den Autorisierungsaufruf im Namen des Benutzers auszuführen, der ursprünglich authentifiziert wurde. Wenn der Autorisierungs-Server (die MVPD) das Aktualisierungs-Token ändern und das alte ungültig machen sollte, können wir das gültige AuthNToken nicht aktualisieren. Aus diesem Grund müssen MVPDs stabile Aktualisierungstoken unterstützen, um OAuth 2.0-Integrationen für sie einrichten zu können.


## Migration von SAML zu OAuth 2.0 {#saml-auth2-migr}

Die Migration von Integrationen von SAML zu OAuth 2.0 wird von Adobe und MVPD durchgeführt. Auf der Seite des Programmierers sind keine technischen Änderungen erforderlich, obwohl der Programmierer das Co-Branding auf der Anmeldeseite von MVPD überprüfen/testen sollte. Aus Sicht von MVPD sind die Endpunkte und andere Informationen, die in OAuth 2.0-Anforderungen angefordert werden, erforderlich.

Um **SSO** beizubehalten, werden Benutzer, die bereits über ein über SAML erhaltenes Authentifizierungstoken verfügen, weiterhin als authentifiziert betrachtet und ihre Anfragen werden über die alte SAML-Integration weitergeleitet.

Aus technischer Sicht:

1. Adobe ermöglicht eine OAuth 2.0-Integration zwischen dem Programmierer und MVPD, OHNE die SAML-Integration zu löschen.
1. Nach der Aktivierung verwenden alle neuen Benutzer OAuth 2.0-Flüsse.
1. Bereits authentifizierte Benutzer, die bereits über ein lokales AuthN-Token verfügen, das die SAML-Betreff-ID enthält, werden von Adobe automatisch über die SAML-Integration weitergeleitet.
1. Für Benutzende in Schritt 3 behandelt Adobe sie nach Ablauf ihres mit SAML generierten AuthN-Tokens als neue Benutzende und verhält sich wie die Benutzenden in Schritt 2.
1. Adobe überprüft die Nutzungsmuster, um den Zeitpunkt zu bestimmen, zu dem die SAML-Integration sicher deaktiviert werden kann.
