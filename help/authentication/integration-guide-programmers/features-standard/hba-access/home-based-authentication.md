---
title: Home-Based Authentication (HBA)
description: Home-Based Authentication (HBA)
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# Home-Based Authentication (HBA) {#home-based-authentication}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Die Home-Based Authentication (HBA) ist eine Funktion von TV Everywhere, mit der Pay-TV-Abonnenten online auf TV-Inhalte zugreifen können, ohne ihre MVPD-Anmeldeinformationen einzugeben, wenn sie mit ihrem Heimnetzwerk verbunden sind, was das Authentifizierungserlebnis erheblich verbessert.

Laut dem Open Authentication Technology Committee (OATC):

> „Die automatische In-Home-Authentifizierung ist der Prozess, bei dem ein MVPD/OVD Merkmale des Heimnetzwerks (oder Kennungen, auf die automatisch zwischen Geräten im Heimnetzwerk zugegriffen werden kann) verwendet, um zu authentifizieren, welches Teilnehmerkonto mit diesem Heimnetzwerk verknüpft ist, sodass Benutzende beim Initiieren einer TVE-Sitzung für den Zugriff auf geschützte Inhalte nicht manuell Anmeldeinformationen eingeben müssen.“

Weitere Informationen zur Home-Based Authentication (HBA) und zu den entsprechenden Branchenstandards finden Sie in den folgenden Ressourcen:

>[!MORELIKETHIS]
>
> * [Sofortiger Zugriff (HBA) durch CTAM](http://www.ctamtve.com/instantaccess)
> * [Anwendungsfälle und Anforderungen für die Home-Based Authentication von OATC](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf)
> * [Startseite-basierte Authentifizierungs-Infografik von Adobe](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640)
> * [Authentifizierung mit OAuth2-aktivierten MVPDs](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
> * [Authentifizierung mit SAML-fähigen MVPDs](/help/authentication/integration-guide-mvpds/authn-usecase.md)
> * [Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

## HBA-Vorteile {#hba-benefits}

Die Home-Based Authentication (HBA) ist eine wichtige Funktion, die die Anmeldebarriere für Zuschauer zu Hause mit einem aktiven Kabelabonnement beseitigt. Diese Barriere stellt eine große Herausforderung für TV Everywhere dar, da fast die Hälfte aller Anmeldeversuche zu Fehlern führen.

HBA kann die Interaktion der Zuschauer erheblich verbessern und ein nahtloses und überlegenes Benutzererlebnis für den Zugriff auf TV Everywhere-Inhalte bieten.

## HBA-Unterstützung {#hba-support}

>[!IMPORTANT]
>
> Wenn Sie an der Nutzung der HBA-Funktionen interessiert sind, wenden Sie sich an Ihren Adobe Pass-Authentifizierungs-Vertriebsmitarbeiter, um weitere Informationen zu erhalten, da bestimmte HBA-Flüsse im Premium-Workflow-Paket enthalten sind.

HBA wird von einer Reihe von MVPDs unterstützt, die in die Adobe Pass-Authentifizierung integriert sind. Um jedoch von HBA zu profitieren, müssen Sie möglicherweise einige zusätzliche Schritte ausführen.

**SAML-MVPDs**

Bei SAML-MVPDs wird HBA nur auf der MVPD-Seite aktiviert.

**OAuth2 MVPDs**

Bei OAuth2-MVPDs kann der HBA über das [Adobe Pass TVE Dashboard ein- oder ausgeschaltet werden](https://experience.adobe.com/#/pass/authentication) indem die Schritte im Benutzerhandbuch für [TVE Dashboard Integrationen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) befolgt werden.

### MVPD {#hba-support-mvpds}

**SAML-MVPDs**

Die folgende Tabelle bietet einen Überblick über die SAML-fähigen MVPDs, die HBA unterstützen:

| MVPD | Grundlegende Funktionalität verfügbar? | Sendet Markierung bei Authentifizierungsantwort? |
|--------------|--------------------------------|----------------------------------------|
| DirectTV | Ja | Nein |
| Teller | Ja | Nein |
| Spektrum | Ja | Ja |
| Steuermann | Ja | Nein |
| AT&amp;T | Ja | Nein |
| Verizon | Ja | Ja |
| Kabelfernsehen | Ja | Nein |
| Mediacom | Ja | Nein |
| Mittelkontinent | Ja | Nein |
| Massision | Ja | Nein |
| AlticeOne | Ja | Ja |

**OAuth2 MVPDs**

Die folgende Tabelle bietet einen Überblick über die OAuth2-fähigen MVPDs, die HBA unterstützen:

| MVPD | Grundlegende Funktionalität verfügbar? | Sendet Markierung bei Authentifizierungsantwort? |
|---------|--------------------------------|----------------------------------------|
| komcast | Ja | Ja |

### Adobe Pass-Authentifizierung {#hba-support-adobe-pass-authentication}

In diesem Abschnitt werden die HBA-aktivierten Funktionen und die von der Adobe Pass-Authentifizierung bereitgestellte Unterstützung beschrieben. Dabei werden wichtige Funktionen hervorgehoben, wie z. B.:

* **HBA-Identifizierung:** Funktion, um Programmierern anzugeben, ob die Authentifizierung HBA oder nicht-HBA war (erfordert MVPD-Unterstützung).
* **Konfigurierbare Authentifizierungs-TTLs:** Möglichkeit, unterschiedliche TTL-Werte (Authentication Time-To-Live) für HBA- und Nicht-HBA-Authentifizierungen festzulegen (MVPD-Unterstützung erforderlich).

Die folgende Tabelle bietet einen allgemeinen Überblick über das Benutzererlebnis in einem HBA und über den regulären Authentifizierungsfluss (ohne HBA):

| Authentifizierungstyp | Benutzererlebnis |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HBA | Benutzer wählen ihre MVPD aus und werden automatisch authentifiziert, wenn sie mit ihrem Heimnetzwerk verbunden sind. Nach Ablauf der Authentifizierung müssen Benutzende ihre MVPD erneut auswählen und sich anschließend automatisch erneut authentifizieren. |
| Normal (kein HBA) | Benutzer wählen ihre MVPD aus und werden unabhängig von ihrer Verbindung zu einem Heimnetzwerk zur Eingabe ihrer Anmeldeinformationen aufgefordert. Nach Ablauf der Authentifizierung müssen Benutzende ihre MVPD erneut auswählen und ihre Anmeldeinformationen erneut eingeben. |

**SAML-MVPDs**

Die folgende Tabelle bietet einen Überblick über die HBA-Implementierung im Fall von SAML-fähigen MVPDs:

| Benutzeraktionen | Systemaktionen |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Der/die Benutzende versucht, ein Video wiederzugeben.<br/><br/>Die MVPD-Auswahl wird angezeigt.<br/><br/>Der Benutzer wählt seine MVPD aus und meldet sich weiterhin an. | Eine Hintergrundprüfung wird durchgeführt, wenn die MVPD ihre Regeln zur Benutzeridentifizierung anwendet. Dies kann beispielsweise die Zuordnung der IP-Adresse des Benutzers zur MAC-Adresse von Modems, die vom Distributor bereitgestellt werden, oder von Set-Top-Boxen mit Breitbandanschluss beinhalten. |
| Es wird ein Bildschirm angezeigt, der etwa 3 Sekunden lang bestehen bleibt.<br/><br/>Auf einer Zwischenübergangsseite kann der Benutzer darüber informiert werden, dass er automatisch mit seinem MVPD-Konto angemeldet wird. | Die Anwendung öffnet die Authentifizierungs-URL in einem Benutzeragenten und initiiert eine HTTP-Anfrage an den Authentifizierungsendpunkt von Adobe Pass.<br/><br/>Der Adobe Pass-Authentifizierungsendpunkt leitet die Anfrage über eine Benutzeragenten-Umleitung an den Authentifizierungsendpunkt von MVPD weiter.<br/><br/>Es wird erwartet, dass MVPD eine Authentifizierungsentscheidung in Form einer SAML-Antwort sendet, die das HBA-Flag (`hba_status`) mit dem Wert „true“ oder „false“ enthält.<br/><br/>Das Backend für die Adobe Pass-Authentifizierung stellt eine Anfrage an den Endpunkt des MVPD-Benutzerprofils, um das `hba_status`-Flag als Teil der [Benutzermetadaten“ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md). |
| Der Benutzer ist authentifiziert und kann jetzt mit dem Titel „TV Everywhere“-Inhalte durchsuchen. | Das Programm ruft das Benutzerprofil ab und kann mit den Flussentscheidungen fortfahren, um Inhalte abzuspielen. |

**OAuth2 MVPDs**

Die folgende Tabelle bietet einen Überblick über die HBA-Implementierung im Fall von OAuth2-fähigen MVPDs:

| Benutzeraktionen | Systemaktionen |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Der/die Benutzende versucht, ein Video wiederzugeben.<br/><br/>Die MVPD-Auswahl wird angezeigt.<br/><br/>Der Benutzer wählt seine MVPD aus und meldet sich weiterhin an. | Eine Hintergrundprüfung wird durchgeführt, wenn die MVPD ihre Regeln zur Benutzeridentifizierung anwendet. Dies kann beispielsweise die Zuordnung der IP-Adresse des Benutzers zur MAC-Adresse von Modems, die vom Distributor bereitgestellt werden, oder von Set-Top-Boxen mit Breitbandanschluss beinhalten. |
| Es wird ein Bildschirm angezeigt, der etwa 3 Sekunden lang bestehen bleibt.<br/><br/>Auf einer Zwischenübergangsseite kann der Benutzer darüber informiert werden, dass er automatisch mit seinem MVPD-Konto angemeldet wird. | Die Anwendung öffnet die Authentifizierungs-URL in einem Benutzeragenten und initiiert eine HTTP-Anfrage an den Authentifizierungsendpunkt von Adobe Pass.<br/><br/>Der Adobe Pass-Authentifizierungsendpunkt leitet die Anfrage über eine Benutzeragenten-Umleitung an den Authentifizierungsendpunkt von MVPD weiter.<br/><br/>Der MVPD-Authentifizierungsendpunkt sendet einen Autorisierungs-Code an den Adobe Pass-Authentifizierungsendpunkt.<br/><br/>Die Adobe Pass-Authentifizierung verwendet den Autorisierungs-Code, um ein Aktualisierungs-Token und ein Zugriffs-Token vom Token-Endpunkt von MVPD anzufordern.<br/><br/>Es wird erwartet, dass MVPD eine Authentifizierungsentscheidung sendet, die das HBA-Flag (`hba_status`) mit dem Wert „true“ oder „false“ als Teil der `id_token` enthält.<br/><br/>Das Backend für die Adobe Pass-Authentifizierung stellt eine Anfrage an den Endpunkt des MVPD-Benutzerprofils, um das `hba_status`-Flag als Teil der [Benutzermetadaten“ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).<br/><br/>MVPD setzt die TTL für das Aktualisierungstoken auf einen von MVPD vereinbarten Wert und Adobe setzt die Authentifizierungs-TTL auf einen Wert, der kleiner oder gleich dem Wert des Aktualisierungstokens ist. |
| Der Benutzer ist authentifiziert und kann jetzt mit dem Titel „TV Everywhere“-Inhalte durchsuchen. | Das Programm ruft das Benutzerprofil ab und kann mit den Flussentscheidungen fortfahren, um Inhalte abzuspielen. |

>[!IMPORTANT]
>
> Im HBA-Fluss für MVPDs, die das OAuth 2.0-Authentifizierungsprotokoll verwenden, gibt die MVPD ein Aktualisierungs-Token mit einer TTL aus, die durch ihre Geschäftsanforderungen definiert ist, während Adobe ein HBA-Authentifizierungs-Token mit einer TTL ausgibt, die die TTL des Aktualisierungs-Tokens nicht überschreiten darf.

## Häufig gestellte Fragen {#faqs}

1. Warum gibt es einen Unterschied zwischen einer HBA-Implementierung für SAML- und OAuth2-Protokolle?

   Die Trennung zwischen Home-Based Authentication (HBA) für SAML- und OAuth2-Protokolle besteht, da diese Protokolle in Bezug auf Authentifizierungsmechanismen, Konfiguration und Implementierungsflexibilität unterschiedlich funktionieren. Bei SAML-MVPDs ist keine Aktion des Programmierers erforderlich, um HBA zu aktivieren, während bei OAuth2-MVPDs der HBA über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) ein- oder ausgeschaltet werden kann.

1. Müssen Benutzer bei aktiviertem HBA bei der ersten Authentifizierung weiterhin ihren Benutzernamen und ihr Kennwort eingeben?

   Nein, Benutzername und Kennwort sind nicht erforderlich.

1. Wie kann man die elterliche Kontrolle durchsetzen?

   Die Adobe Pass-Authentifizierung kann HBA für Integrationen mit Kanälen deaktivieren, die die Genehmigung der Kindersicherung benötigen. Außerdem arbeiten wir mit OATC an einem UX-Dokument, das empfiehlt, wie das HBA-Erlebnis mit Kindersicherung eingerichtet wird.

1. Verwenden Anbieter, die HBA unterstützen, kürzere TTL-Fenster für HBA als für die reguläre Authentifizierung?

   Die TTL-Einstellung ist konfigurierbar. Es wird empfohlen, für MVPD-Integrationen mit HBA eine kürzere TTL festzulegen, um eine fehlerhafte Handhabung zu verhindern.
