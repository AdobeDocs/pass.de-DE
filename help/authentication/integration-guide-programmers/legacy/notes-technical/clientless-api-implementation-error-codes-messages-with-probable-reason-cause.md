---
title: Clientlose API-Implementierung – Fehler Codes/Nachrichten mit wahrscheinlichem Grund/Ursache
description: Clientlose API-Implementierung – Fehler Codes/Nachrichten mit wahrscheinlichem Grund/Ursache
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (Vermächtnis) Clientlose API-Implementierung – Fehler Codes/Nachrichten mit wahrscheinlichem Grund/Ursache {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Die Inhalte auf dieser Seite werden nur zu Informationszwecken zur Verfügung gestellt. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe Systems erforderlich. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Stellen Sie sicher, dass Sie über die neuesten Adobe Pass Authentication Produktankündigungen und Stilllegungszeitpläne auf dem Laufenden bleiben, die in der [Seite &quot;Produktankündigungen](/help/authentication/product-announcements.md) &quot; zusammengefasst sind.

</br>


## Fehler: Nicht autorisiert

### Bewirkt:

1. Autorisierung Kopfzeile fehlt im POST
1. Problem mit Autorisierung Kopfzeile – prüfen Sie, ob Anfrage Zeit in Millisekunden angegeben ist.

## Fehler: SC 400 während der Authentifizierung

### Bewirkt:

1. Der Server konnte den registrierung Code, der für eine bestimmte Anforderung erstellt wurde, nicht finden, und Umgebung.
1. Möglicherweise treten domänenübergreifende Scripting Probleme auf
1. Korrektes Spoofing sollte in die Datei /etc/hosts eingefügt werden

## Fehler: 400 fehlerhafte Anfrage

### Bewirkt:

1. Falsch formatierte URL für POST/GET
1. SAMLAssertionParserException – Die verschlüsselte SAML-Assertion konnte bei Adobe Systems nicht entschlüsselt werden

## Fehler: 403 Verboten

### Bewirkt:

1. Zu viele schnelle Anfragen - eine Funktion des API-Managements, um DoS-Angriffe zu verhindern.
2. Wenn Sie prequal Umgebung verwenden, fügen Sie Spoofing hinzu, andernfalls stellen Sie sicher, dass Spoofing aus der Datei /etc/hosts entfernt wurde

## Fehler: Anmeldung zu MVPD-Seite nicht möglich

### Bewirkt:

1. Benutzername und Kennwort stimmen nicht überein
2. Anmeldung wurde möglicherweise deaktiviert
3. Überprüfen Sie, ob die Log-in für die Produktion oder das Staging bestimmt ist


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
