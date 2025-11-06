---
title: Aktivieren von Adobe Pass-Berechtigungsdiensten für einen Programmierer auf Xbox 360 und XboxOne Clientless
description: Aktivieren von Adobe Pass-Berechtigungsdiensten für einen Programmierer auf Xbox 360 und XboxOne Clientless
exl-id: ff7254de-9ea4-4c27-a186-d1c2eea12222
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# (Legacy) Aktivieren von Adobe Pass-Berechtigungsdiensten für einen Programmierer auf Xbox 360 und XboxOne ClientLess {#enabling-primetime-entitlement-services-for-a-programer-on-xbox-360-and-xboxone-clientless}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.


1. Der Programmierer erstellt ein Zendesk-Ticket, um die Xbox 360/One für die Client-lose Lösung zur Adobe Pass-Authentifizierung zu aktivieren, indem er die folgenden Informationen bereitstellt:

   1. Plattform: z. B. Xbox 360, Xbox One

   1. Antragsteller-ID: z. B. netgeo, CNN usw.

1. Adobe erstellt X509-Zertifikate und konfiguriert den privaten Schlüssel und das Kennwort am Ende.

1. Adobe stellt das öffentliche Zertifikat (des X509-Zertifikats) dem Programmierer im Ticket oder per E-Mail zur Verfügung.

1. Der Programmierer müsste dann dieses öffentliche Zertifikat auf dem BSP-Portal für die App installieren, die bei Microsoft registriert ist.

1. Der Programmierer fordert dann das JWT (Java Web Token)- oder STS Token für XboxOne bzw. 360 vom Microsoft Xbox Live-Service an, der mit dem in Schritt 3 bereitgestellten öffentlichen X509-Zertifikat verschlüsselt wird.

1. Dies sind die Token, die die eindeutige deviceId für Xbox-Geräte enthalten. Fügen Sie das Token (JWT oder STS) mithilfe eines „x“-Parameters wie folgt in die Autorisierungskopfzeile ein:

   1. Für Xbox 360 muss das XSTS-Token Base64-kodiert sein, bevor es an die Pay-TV-Authentifizierung von Adobe Pass gesendet wird.
   1. Für Xbox One ist das JWT bereits ordnungsgemäß codiert, sodass keine zusätzliche Codierung erfolgen sollte.

1. Alle API-Aufrufe vom Xbox-Gerät sollten die Autorisierungs-Kopfzeile mit dem oben genannten Token in einem -Parameter enthalten.



>[!NOTE]
>
>Insbesondere die Xbox hat einige einzigartige Anforderungen im Zusammenhang mit digitalem Signieren. Die Geräte-ID der XBox-Konsole ist im XSTS-Token enthalten.  Bei Xbox 360 ist dies eine verschlüsselte SAML-Bestätigung; bei Xbox One ist dies ein verschlüsseltes JWT. Die XBox-Konsolen-App sendet das gesamte XSTS-Token an die Adobe Pass-Pay-TV-Authentifizierung. Die Adobe Pass-Pay-TV-Authentifizierung entschlüsselt das Token mithilfe seines öffentlichen Schlüssels, analysiert das Token und extrahiert die deviceId daraus.

>[!NOTE]
>
>Aufgrund der großen Länge des XSTS-Tokens weist die XBox-Konsole eine technische Einschränkung auf: Das Token kann nicht als HTTP-GET-Parameter an die Adobe Pass-Authentifizierungs-APIs für Pay-TV gesendet werden. Um diesem Problem zu begegnen, ermöglicht die Pay-TV-Authentifizierung von Adobe Pass das Senden des XSTS-Tokens als Teil des HTTP-Headers „Authorization“ beim Aufruf der APIs. Das XSTS-Token muss mit dem öffentlichen Schlüssel aus dem X.509-Zertifikat verschlüsselt werden, das dem Programmierer von der Adobe Pass-Pay-TV-Authentifizierung ausgestellt wurde. Die Adobe Pass-Pay-TV-Authentifizierung speichert den zugehörigen privaten Schlüssel und verwendet ihn, um das XSTS-Token zu entschlüsseln und die deviceId daraus zu extrahieren.
