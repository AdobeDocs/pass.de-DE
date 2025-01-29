---
title: Home-basierte Authentifizierung für TV Everywhere
description: Home-basierte Authentifizierung für TV Everywhere
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 0%

---

# Home-basierte Authentifizierung für TV Everywhere

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Was ist die Authentifizierung zu Hause? {#whatis-home-based-authn}

Die Home Based Authentication (HBA) ist eine Funktion von TV Everywhere, mit der Pay-TV-Abonnentinnen und -Abonnenten TV-Inhalte online ansehen können, ohne MVPD-Anmeldeinformationen einzugeben, wenn sie zu Hause sind. Dies verbessert das Benutzererlebnis des Authentifizierungsflusses erheblich.

Heimbasierte Authentifizierung durch das Open Authentication Technology Committee (OATC): „Die automatische Authentifizierung im Haus ist der Prozess, bei dem ein MVPD/OVD Merkmale des Heimnetzwerks (oder Kennungen, die automatisch zwischen Geräten im Heimnetzwerk zugänglich sind) verwendet, um zu authentifizieren, welches Teilnehmerkonto mit diesem Heimnetzwerk verknüpft ist, damit Benutzende beim Einrichten einer TVE-Sitzung für den Zugriff auf TVE-geschützte Inhalte nicht manuell Anmeldeinformationen eingeben müssen.“



Weitere Informationen zu HBA und den Branchenstandards finden Sie in der Dokumentation [OATC-Anwendungsfälle und -Anforderungen](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} und in den **OATC-Richtlinien für Benutzererlebnisse für HBA**.

>[!NOTE]
>
>Einige HBA-Flüsse sind Teil des Premium-Workflow-Pakets. Wenden Sie sich an Ihren Adobe Pass-Vertriebsmitarbeiter, wenn Sie diese Funktion verwenden möchten.

## Warum HBA für Sie wichtig ist {#why-hba}

HBA ist wichtig, da es die Anmeldebarriere für Ihre Zuschauer, die zu Hause sind und bereits über ein Kabelabonnement verfügen, praktisch beseitigt. Außerdem kann die Home-Based-Authentifizierung die Interaktion Ihrer Zuschauer erheblich steigern und ein besseres Benutzererlebnis für Ihre TV Everywhere-Inhalte bieten.

Derzeit ist fast die Hälfte der Anmeldeversuche nicht erfolgreich.

Nachdem HBA von einem der fünf wichtigsten MVPDs aktiviert wurde, stieg die Konversionsrate der Authentifizierung **um 40 %** (von 45 % auf 63 %)

![](../../../assets/authn-conv-pre-post.png)

Außerdem sehen Sie unten die Anmeldekonversionsrate für einen Kanal, der mit verschiedenen MVPDs integriert ist: mit solchen, die HBA für diesen Kanal aktiviert haben, und mit solchen, die keinen HBA haben. Die Konversionsrate ist bei Personen mit HBA deutlich höher als bei Personen ohne HBA.

![](../../../assets/hba-vs-non-hba.png)

Sechs Monate nach der Aktivierung von HBA für die meisten mit dieser MVPD integrierten Kanäle ist ein Anstieg der Unique Users um 82 % zu verzeichnen (die Anzahl der User, die über diese MVPD auf TV Everywhere-Kanäle zugreifen, hat sich fast verdoppelt).

2w3Im Gegensatz dazu zeigten andere MVPDs, bei denen HBA nicht aktiviert war, in den letzten 6 Monaten lediglich einen Anstieg um 26 % bei den Unique Users.

![](../../../assets/unique-visitors-incr.png)

Unsere Daten, die 6 Monate vor und 6 Monate nach der Aktivierung von HBA erfasst wurden, haben gezeigt, dass die Interaktion der Zuschauer mit den Kanälen, für die HBA aktiviert wurde, deutlich zugenommen hat. Praktisch Benutzer von MVPDs, die HBA aktiviert haben, sehen im Durchschnitt 30 % mehr Inhalte als Benutzer von MVPDs, die HBA nicht aktiviert haben.

![](../../../assets/user-engagement-increase.png)

## Adobe Pass-Authentifizierung - HBA-Unterstützung {#auth-hba-support}

Dieser Abschnitt beschreibt die HBA-Unterstützung durch die Adobe Pass-Authentifizierung, das Verhalten von Adobe Pass-Authentifizierungsplattformen in HBA-Flüssen und bietet außerdem technische Details, die für die Implementierung von HBA nützlich sind.

Adobe Pass-Authentifizierungsfunktionen, die HBA unterstützen

* Möglichkeit, unterschiedliche Authentifizierungs-TTLs für HBA- und Nicht-HBA-Authentifizierungen festzulegen (erfordert auch MVPD-Unterstützung)
* Möglichkeit zur automatischen Auswahl einer MVPD (MVPD-Auswahl überspringen), wenn die Authentifizierung abgelaufen ist. Dies ist besonders dann nützlich, wenn HBA-TTLs klein sind.
* Möglichkeit, den Programmierern anzuzeigen, ob die Authentifizierung HBA war oder nicht (erfordert auch MVPD-Unterstützung)

### HBA-Benutzererlebnis auf Adobe Pass-Authentifizierungsplattformen {#hba-user-exp}

Die folgenden Tabellen enthalten Informationen zum Benutzererlebnis für die unterstützten Plattformen, wenn HBA aktiviert und nicht aktiviert ist:

| Benutzerfluss - Platform-Typ | SWF, iOS, Android |
|---|---|
| Mit aktiviertem HBA | Wenn Benutzende zu Hause sind, werden sie automatisch authentifiziert. Nach Ablauf des HBA-Authentifizierungs-Tokens werden Benutzer automatisch erneut authentifiziert. |
| Ohne HBA | Benutzer werden aufgefordert, ihre MVPD auszuwählen und ihre Anmeldeinformationen einzugeben, selbst wenn sie zu Hause sind. Nach Ablauf des AuthN-Tokens müssen Benutzer ihre Anmeldeinformationen erneut eingeben. |

| Benutzerfluss - Platform-Typ | js, Windows (nativ) |
|---|---|
| Mit aktiviertem HBA | Wenn Benutzende zu Hause sind, werden sie automatisch authentifiziert. Nach Ablauf des HBA-AuthN-Tokens müssen Benutzerinnen und Benutzer ihre MVPD in der Auswahl erneut auswählen und werden automatisch authentifiziert. |
| Ohne HBA | Benutzende werden aufgefordert, ihre MVPD auszuwählen und ihre Anmeldeinformationen einzugeben, auch wenn sie zu Hause sind. Nach Ablauf des Authentifizierungs-Tokens müssen Benutzer ihre Anmeldeinformationen erneut eingeben. |

| Benutzerfluss - Platform-Typ | Client-lose REST-API (Authentifizierung auf dem zweiten Bildschirm) |
|---|---|
| Mit aktiviertem HBA | Wenn Benutzer zu Hause sind und eine Client-lose REST-API-App verwenden, werden sie nach Eingabe des Registrierungs-Codes und Auswahl ihrer MVPD automatisch auf dem zweiten Bildschirmgerät authentifiziert. Nach Ablauf des HBA-Authentifizierungs-Tokens werden Benutzer automatisch erneut authentifiziert (auf dem zweiten Bildschirmgerät). |
| Ohne HBA | Benutzende werden aufgefordert, ihre MVPD auszuwählen und ihre Anmeldeinformationen einzugeben, auch wenn sie zu Hause sind. Nach Ablauf des Authentifizierungs-Tokens müssen Benutzer ihre Anmeldeinformationen erneut eingeben. |

### Technische Details zur Implementierung von HBA {#tech-details-hba}

#### OAuth 2.0-Protokoll {#oauth-2-protocol}

Im HBA-Fluss für MVPDs, die mit dem OAuth 2.0-Authentifizierungsprotokoll integriert sind, gibt der MVPD ein Aktualisierungstoken aus und das Adobe gibt ein HBA-Authentifizierungstoken aus:

* Das Aktualisierungs-Token verfügt über eine TTL, die durch die Geschäftsanforderungen des MVPD bestimmt wird.
* Die HBA-Authentifizierungstoken-TTL **muss kleiner oder gleich)** die TTL für das Aktualisierungstoken sein.


*Beschreibung des HBA-Authentifizierungsflusses für das OAuth 2.0-Protokoll*


| Benutzeraktionen | Systemaktionen |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Der/die Benutzende navigiert zur Website des Programmierers. Beim Versuch, ein Video wiederzugeben, wird die MVPD-Auswahl angezeigt. Der/die Benutzende wählt seine/ihre MVPD aus und klickt auf „Anmelden“. | Es wird eine Hintergrundprüfung durchgeführt. Die MVPD wendet ihre Regeln zur Benutzererkennung an (ordnen Sie beispielsweise die IP-Adresse des Benutzers der MAC-Adresse von Modems zu, die vom Distributor bereitgestellt werden, oder von mit Breitband verbundenen Set-Top-Boxen). |
| Es wird ein Bildschirm angezeigt, der etwa 3 Sekunden lang bestehen bleibt. Eine zwischengeschaltete Seite kann angezeigt werden, die den Benutzer darüber informiert, dass er automatisch mit seinem MVPD-Konto angemeldet wird. | <ol><li>AccessEnabler wird vom Programmierer installiert und sendet eine Authentifizierungsanfrage (als HTTP-Anfrage) an den Authentifizierungsendpunkt von Adobe Pass.</li><li>Der Adobe Pass-Authentifizierungsendpunkt leitet die Anfrage an den MVPD-Authentifizierungsendpunkt weiter. <br />**Hinweis:** Die Anfrage enthält den `hba_flag` (Versuch HBA = true), der angibt, dass die MVPD die HBA-Authentifizierung versuchen soll.</li><li>Der MVPD-Authentifizierungsendpunkt sendet einen Autorisierungs-Code an den Adobe Pass-Authentifizierungsendpunkt.</li><li>Die Adobe Pass-Authentifizierung verwendet den Autorisierungs-Code, um ein Aktualisierungs-Token und ein Zugriffs-Token vom Token-Endpunkt von MVPD anzufordern.</li><li>Der MVPD sendet eine Authentifizierungsentscheidung und den Parameter `hba_status` (true/false) im `id_token`.</li><li>Ein Aufruf an den Endpunkt des MVPD-Benutzerprofils wird gesendet, um den [hba_status-Schlüssel in Benutzermetadaten“ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).</li><li>MVPD setzt die TTL des Aktualisierungstokens auf einen von MVPD vereinbarten Wert und Adobe setzt die TTL des AuthN-Tokens auf einen Wert, der kleiner oder gleich dem Wert des Aktualisierungstokens ist.</li></ol> |
| Der Benutzer ist authentifiziert und kann jetzt mit dem Titel „TV Everywhere“-Inhalte durchsuchen. | Das Authentifizierungs-Token wird an den Benutzer übergeben, der jetzt erfolgreich die Site des Programmierers durchsuchen kann. |

#### SAML-Protokoll {#saml-protocol}

Beschreibung des HBA-Authentifizierungsflusses für das SAML-Authentifizierungsprotokoll

| Benutzeraktionen | Systemaktionen |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Der/die Benutzende navigiert zur Website des Programmierers. Beim Versuch, ein Video wiederzugeben, wird die MVPD-Auswahl angezeigt. Der/die Benutzende wählt seine/ihre MVPD aus und klickt auf „Anmelden“. | Es wird eine Hintergrundprüfung durchgeführt. Die MVPD wendet ihre Regeln zur Benutzererkennung an (ordnen Sie beispielsweise die IP-Adresse des Benutzers der MAC-Adresse von Modems zu, die vom Distributor bereitgestellt werden, oder von mit Breitband verbundenen Set-Top-Boxen). |
| Es wird ein Bildschirm angezeigt, der etwa 3 Sekunden lang bestehen bleibt. Eine zwischengeschaltete Seite kann angezeigt werden, die den Benutzer darüber informiert, dass er automatisch mit seinem MVPD-Konto angemeldet wird. | <ol><li>AccessEnabler wird vom Programmierer installiert und sendet eine Authentifizierungsanfrage (als HTTP-Anfrage) an den Authentifizierungsendpunkt von Adobe Pass.</li><li>Der Adobe Pass-Authentifizierungsendpunkt leitet die Anfrage an den MVPD-Authentifizierungsendpunkt weiter.</li><li>Der MVPD sollte eine Authentifizierungsentscheidung in Form einer SAML-Antwort senden, die das HBA-Flag enthalten sollte: hba_status (true/false).</li><li>Ein Aufruf an den Endpunkt des MVPD-Benutzerprofils wird gesendet, um den [hba_status-Schlüssel in Benutzermetadaten“ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).</li></ol> |
| Der Benutzer ist authentifiziert und kann jetzt mit dem Titel „TV Everywhere“-Inhalte durchsuchen. | Das Authentifizierungs-Token wird an den Benutzer übergeben, der jetzt erfolgreich die Site des Programmierers durchsuchen kann. |


## Aktivieren von HBA {#how-to-activate-hba}

* **OAuth-Protokoll:**
   * Informationen zur Aktivierung von HBA finden Sie im [Benutzerhandbuch zum Adobe Pass TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* **SAML-Protokoll:** Die Home-Based Authentication wird auf der MVPD-Seite aktiviert. Der Programmierer oder die Adobe benötigt keine Aktion.
Weitere Informationen zu den MVPDs, die die Home-Based Authentication unterstützen, finden Sie unter [HBA-Status für MVPDs](/help/authentication/integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md).

## FAQs {#faqs}


**Frage:** Warum die Trennung zwischen Home-Based Authentication mit SAML- und OAuth2-Protokollen?

**Antwort:** Der HBA-Fluss ist für die beiden Protokolle unterschiedlich. Aus Programmierersicht besteht kein Handlungsbedarf, um sicherzustellen, dass HBA für SAML-MVPDs aktiviert ist, während für OAuth2-MVPDs der HBA im Adobe Pass TVE-Dashboard ein- oder ausgeschaltet werden kann.



**Frage:** Müssen Benutzer bei der ersten Authentifizierung, wenn HBA aktiviert ist, einen Benutzernamen und ein Kennwort eingeben?

**Antwort:** Nein, Benutzername und Kennwort sind nicht erforderlich.



**Frage:** Wie setzt man die elterliche Kontrolle durch?

**Antwort 1:** Adobe kann HBA für Integrationen mit Kanälen deaktivieren, die die Genehmigung der Kindersicherung benötigen.

**Antwort 2:** Adobe arbeitet mit OATC an einem UX-Dokument, das die Einrichtung des HBA-Erlebnisses mit Kindersicherung empfiehlt.



**Frage:** verfügen die Anbieter, die HBA unterstützen, über kürzere TTL-Fenster für HBA als für die reguläre Authentifizierung?

**Antwort:** Die TTL-Einstellung ist konfigurierbar. Es wird empfohlen, eine kürzere TTL für HBA-Authentifizierungs-Token festzulegen, um eine fehlerhafte Verarbeitung zu verhindern.


## Nützliche Informationen {#useful-info}

* [Instant Access (HBA) Recommendations](http://www.ctamtve.com/instantaccess){target=_blank} - von CTAM
* [Beispielimplementierung von HBA in der Programmierer-App](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/HBA_Flow_Sample.pdf?dc=201604222139-1346){target=_blank} - per Adobe
  <!--* [Home Based Authentication User Experience Guidelines for TV Everywhere](http://oatc.us/Standards/DownloadRecommendedPractices.aspx){target=_blank} - by OATC-->
* [Anwendungsfälle und Anforderungen für die Home-basierte Authentifizierung](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} von OATC
* [Home Based Authentication Infografik](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640){target=_blank} - von Adobe
* [Authentifizierung mit dem OAuth 2.0-Protokoll](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
* [Authentifizierung mit SAML-MVPDs](/help/authentication/integration-guide-mvpds/authn-usecase.md)
* [Benutzerhandbuch zum Adobe Pass TVE-Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* [hba_status Benutzermetadaten](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)
