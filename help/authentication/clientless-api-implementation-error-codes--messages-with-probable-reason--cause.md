---
title: Clientlose API-Implementierung – Fehler Codes/Nachrichten mit wahrscheinlichem Grund/Ursache
description: Clientlose API-Implementierung – Fehler Codes/Nachrichten mit wahrscheinlichem Grund/Ursache
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Clientlose API-Implementierung – Fehler Codes/Nachrichten mit wahrscheinlichem Grund/Ursache {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Die Inhalte auf dieser Seite werden nur zu Informationszwecken zur Verfügung gestellt. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe Systems erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>


## Fehler: Nicht autorisiert

### Ursachen:

1. Autorisierung Kopfzeile fehlt im POST
1. Problem mit Autorisierung Kopfzeile – prüfen Sie, ob Anfrage Zeit in Millisekunden angegeben ist.

## Fehler: SC 400 während der Authentifizierung

### Ursachen:

1. Der Server konnte den registrierung Code, der für eine bestimmte Anforderung erstellt wurde, nicht finden, und Umgebung.
1. Möglicherweise treten domänenübergreifende Scripting Probleme auf
1. Korrektes Spoofing sollte in die Datei /etc/hosts eingefügt werden

## Fehler: 400 Ungültige Anfrage

### Ursachen:

1. Falsch formatierte URL für POST/GET
1. SAMLAssertionParserException – Die verschlüsselte SAML-Assertion konnte bei Adobe Systems nicht entschlüsselt werden

## Fehler: 403 Verboten

### Ursachen:

1. Zu viele schnelle Anfragen - eine Funktion des API-Managements, um DoS-Angriffe zu verhindern.
2. Wenn Sie prequal Umgebung verwenden, fügen Sie Spoofing hinzu, andernfalls stellen Sie sicher, dass Spoofing aus der Datei /etc/hosts entfernt wurde

## Fehler: Anmeldung zu MVPD-Seite nicht möglich

### Ursachen:

1. Benutzername und Kennwort stimmen nicht überein
2. Anmeldung wurde möglicherweise deaktiviert
3. Überprüfen Sie, ob die Anmeldung für Produktion oder Staging verwendet wird.


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
