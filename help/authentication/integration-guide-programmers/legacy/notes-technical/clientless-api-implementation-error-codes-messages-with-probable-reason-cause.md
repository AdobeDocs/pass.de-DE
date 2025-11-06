---
title: Client-lose API-Implementierung - Fehler-Codes/Meldungen mit wahrscheinlichem Grund/Ursache
description: Client-lose API-Implementierung - Fehler-Codes/Meldungen mit wahrscheinlichem Grund/Ursache
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (Veraltete) Client-lose API-Implementierung - Fehler-Codes/Meldungen mit wahrscheinlicher Ursache/Ursache {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>


## Fehler: Nicht autorisiert

### Ursachen:

1. Fehlende Autorisierungskopfzeile im POST-Test
1. Problem mit dem Autorisierungs-Header - überprüfen, ob die Anfragezeit in Millisekunden ist.

## Fehler: SC 400 während der Authentifizierung

### Ursachen:

1. Der Server konnte den Registrierungs-Code, der für einen bestimmten Anforderer und eine bestimmte Umgebung erstellt wurde, nicht finden.
1. Es könnten Probleme mit Domain-übergreifender Skripterstellung auftreten
1. Der Datei /etc/hosts sollte das entsprechende Spoofing hinzugefügt werden.

## Fehler: 400 fehlerhafte Anfrage

### Ursachen:

1. Fehlerhafte URL für POST/GET
1. SAMLAsertionParserException - Die verschlüsselte SAML-Assertion konnte am Ende von Adobe nicht entschlüsselt werden.

## Fehler: 403 verboten

### Ursachen:

1. Zu viele schnelle Anfragen - eine Funktion der API-Verwaltung, um DoS-Angriffe zu verhindern.
2. Wenn Sie eine Prequal-Umgebung verwenden, fügen Sie Spoofing hinzu. Vergewissern Sie sich andernfalls, dass das Spoofing aus der Datei &quot;/etc/hosts“ entfernt wurde

## Fehler: Anmeldung bei MVPD-Seite nicht möglich

### Ursachen:

1. Benutzername und Kennwort stimmen nicht überein
2. Anmeldung wurde möglicherweise deaktiviert
3. Überprüfen, ob die Anmeldung für Produktion oder Staging vorgesehen ist


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
