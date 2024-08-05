---
title: Temporärer Zugriffsfluss
description: REST API V2 - Fluss temporärer Zugriffsberechtigungen
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '3210'
ht-degree: 0%

---


# Temporäre Zugriffsflüsse {#temporary-access-flows}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

>[!IMPORTANT]
>
> Die REST API V2-Implementierung wird durch die Dokumentation zum [Drosselungsmechanismus](/help/authentication/throttling-mechanism.md) begrenzt.

TempPass ermöglicht es Programmierern, temporären Zugriff auf ihren geschützten Inhalt zu gewähren, ohne Benutzer zu bitten, sich mit einem gültigen MVPD-Konto zu authentifizieren.

Weitere Informationen zur Funktion &quot;TempPass&quot;finden Sie in der Dokumentation zu [TempPass](../../../temp-pass.md) .

Mit temporären Zugriffsflüssen können Sie nach folgenden Szenarien abfragen:

* [Abrufen von Autorisierungsentscheidungen mit einfachen TempPass](#retrieve-authorization-decisions-using-basic-temppass)
* [Abrufen von Autorisierungsentscheidungen mit der Promotion TempPass](#retrieve-authorization-decisions-using-promotional-temppass)
* [Maximale Ressourcenanzahl mithilfe der Promotion TempPass nutzen](#consume-maximum-number-of-resources-using-promotional-temppass)
* [Abrufen von Autorisierungsentscheidungen bei Ablauf von einfachen oder Werbe-TempPass](#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires)
* [Profil für einfachen TempPass abrufen](#retrieve-profile-for-basic-temppass)
* [Profil für die Promotion TempPass abrufen](#retrieve-profile-for-promotional-temppass)

## Abrufen von Autorisierungsentscheidungen mit einfachen TempPass {#retrieve-authorization-decisions-using-basic-temppass}

### Voraussetzungen {#prerequisites-retrieve-authorization-decisions-using-basic-temppass}

Bevor Sie Autorisierungsentscheidungen mit einfachen TempPass abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung möchte temporären Zugriff auf Wiedergabeinhalte bereitstellen, ohne den Benutzer zur Authentifizierung aufzufordern.
* Die Streaming-Anwendung muss eine Autorisierungsentscheidung abrufen, bevor eine vom Benutzer ausgewählte Ressource wiedergegeben wird.

>[!IMPORTANT]
>
> Annahmen
> 
> <br/>
> 
> * Es muss eine gültige Konfigurationseinstellung des einfachen TempPass geben, der auf die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` angewendet wird.
> * Die für den einfachen TempPass konfigurierte TTL (Time-To-Live) ist nicht abgelaufen.

### Workflow {#workflow-retrieve-authorization-decisions-using-basic-temppass}

Führen Sie die angegebenen Schritte aus, um den Autorisierungsfluss mithilfe des einfachen TempPass zu implementieren, wie im folgenden Diagramm dargestellt.

![Abrufen von Autorisierungsentscheidungen mit einfachen TempPass](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-using-basic-temppass-flow.png)

*Abrufen von Autorisierungsentscheidungen mit einfachen TempPass*

1. **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Endpunkt Entscheidungsautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API:
   > 
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Validieren des einfachen TempPass-Dienstes:** Der Adobe Pass-Server überprüft, ob eine gültige Konfigurationseinstellung des einfachen TempPass vorhanden ist, der auf die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` angewendet wird.

1. **Rückgabe `Permit` -Entscheidung mit Medien-Token:** Die Antwort des Endpunkts Entscheidungsautorisierung enthält eine `Permit` -Entscheidung und ein Medien-Token.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API .
   >
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung verwendet die Anfragedaten, um zu überprüfen, ob temporäre Zugriffsbedingungen erfüllt sind:
   >
   > * Die für den einfachen TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.
   >
   > <br/>
   > 
   > Wenn die Validierung des temporären Zugriffs fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Stream mit Medien-Token starten:** Die Streaming-Anwendung verwendet das Medien-Token zur Wiedergabe des Inhalts.

## Abrufen von Autorisierungsentscheidungen mit der Promotion TempPass {#retrieve-authorization-decisions-using-promotional-temppass}

### Voraussetzungen {#prerequisites-retrieve-authorization-decisions-using-promotional-temppass}

Bevor Sie Autorisierungsentscheidungen mit dem Promo-TempPass abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung möchte temporären Zugriff gewähren, um eine maximale Anzahl von Ressourcen abzuspielen, ohne den Benutzer zur Authentifizierung aufzufordern.
* Die Streaming-Anwendung muss eindeutige Informationen über die Identität des Benutzers enthalten, wenn eine Autorisierungsentscheidung abgerufen wird.
* Die Streaming-Anwendung muss eine Autorisierungsentscheidung abrufen, bevor eine vom Benutzer ausgewählte Ressource wiedergegeben wird.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Es muss eine gültige Konfigurationseinstellung für die Promotion TempPass geben, die auf die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` angewendet wird.
> * Die für den Promo-TempPass konfigurierte TTL (Time-To-Live) ist nicht abgelaufen.
> * Die maximale Anzahl von Ressourcen, die für die Promotion TempPass konfiguriert wurden, wurde nicht verbraucht.

### Workflow {#workflow-retrieve-authorization-decisions-using-promotional-temppass}

Führen Sie die angegebenen Schritte aus, um den Autorisierungsfluss mithilfe von TempPass zu implementieren, wie in der folgenden Abbildung dargestellt.

![Abrufen von Autorisierungsentscheidungen mit der Promotion TempPass](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-using-promotional-temppass-flow.png)

*Abrufen von Autorisierungsentscheidungen mit der Promotion TempPass*

1. **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Endpunkt Entscheidungsautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API:
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   >
   > Der Entscheidungsendpunkt Autorisierung erfordert bei Verwendung des Promotionsprinzips TempPass das Vorhandensein von `AP-TempPass-Identity` -Kopfzeilen. Die Kopfzeile enthält eindeutige Informationen zur Identität des Benutzers, der auf den Inhalt zugreift.
   > 
   > <br/>
   > 
   > Weitere Informationen zum Header `AP-TempPass-Identity` finden Sie in der Dokumentation [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) .

1. **Validieren Sie die Werbeaktion TempPass:** Der Adobe Pass-Server überprüft, ob eine gültige Konfigurationseinstellung für die Werbeaktion TempPass vorhanden ist, die auf die Integration zwischen dem bereitgestellten `serviceProvider` und dem bereitgestellten `mvpd` angewendet wird.

1. **Rückgabe `Permit` -Entscheidung mit Medien-Token:** Die Antwort des Endpunkts Entscheidungsautorisierung enthält eine `Permit` -Entscheidung und ein Medien-Token.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API .
   > 
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung verwendet die Anfragedaten, um zu überprüfen, ob temporäre Zugriffsbedingungen erfüllt sind:
   >
   > * Die für den Promotion-TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.
   > * Die maximale Anzahl von Ressourcen, die für die Promotion TempPass konfiguriert sind, darf nicht verbraucht werden.
   >
   > <br/>
   > 
   > Wenn die Validierung des temporären Zugriffs fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Stream mit Medien-Token starten:** Die Streaming-Anwendung verwendet das Medien-Token zur Wiedergabe des Inhalts.

## Maximale Ressourcenanzahl mithilfe der Promotion TempPass nutzen {#consume-maximum-number-of-resources-using-promotional-temppass}

### Voraussetzungen {#prerequisites-consume-maximum-number-of-resources-using-promotional-temppass}

Bevor Sie eine maximale Anzahl von Ressourcen mit der Promotion TempPass verbrauchen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung möchte temporären Zugriff gewähren, um eine maximale Anzahl von Ressourcen abzuspielen, ohne den Benutzer zur Authentifizierung aufzufordern.
* Die Streaming-Anwendung muss eindeutige Informationen über die Identität des Benutzers enthalten, wenn eine Autorisierungsentscheidung abgerufen wird.
* Die Streaming-Anwendung muss eine Autorisierungsentscheidung abrufen, bevor eine vom Benutzer ausgewählte Ressource wiedergegeben wird.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Es muss eine gültige Konfigurationseinstellung für die Promotion TempPass geben, die auf die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` angewendet wird.
> * Die für den Promo-TempPass konfigurierte TTL (Time-To-Live) ist nicht abgelaufen.
> * Die maximale Anzahl von Ressourcen, die für die Promotion &quot;TempPass&quot;konfiguriert sind, beträgt 1.

### Workflow {#workflow-consume-maximum-number-of-resources-using-promotional-temppass}

Führen Sie die angegebenen Schritte aus, um den Autorisierungsfluss zu implementieren, wenn Sie eine maximale Anzahl von Ressourcen mit der Promotion TempPass verbrauchen, wie im folgenden Diagramm dargestellt.

![Maximale Anzahl an Ressourcen mit der Promotion TempPass](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-consume-maximum-number-of-resources-using-promotional-temppass-flow.png) verbrauchen

*Maximale Anzahl an Ressourcen mit der Promotion TempPass* verbrauchen

1. **Profil für den Promotion-TempPass abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen für den Promo-TempPass abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen des Profils für bestimmte mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) :
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `mvpd`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   > 
   > Die Abfrage des Endpunkts Profile ist optional und kann verwendet werden, um zu bestimmen, wie viele Ressourcen noch mit dem Promo-TempPass abgespielt werden können.

1. **Validieren Sie die Werbeaktion TempPass:** Der Adobe Pass-Server überprüft, ob eine gültige Konfigurationseinstellung für die Werbeaktion TempPass vorhanden ist, die auf die Integration zwischen dem bereitgestellten `serviceProvider` und dem bereitgestellten `mvpd` angewendet wird.

1. **Rückgabe von Informationen zum temporären Profil:** Die Antwort des Profilendpunkts enthält Informationen zum temporären Profil, einschließlich des Attributs `type`, das auf &quot;temporär&quot;gesetzt ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Profilantwort finden Sie in der Dokumentation zur API [Profil für bestimmte mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) -API.
   > 
   > <br/>
   > 
   > Der Endpunkt Profile validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   > 
   > <br/>
   >
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   > 
   > Der Endpunkt Profile verwendet die Anfragedaten, um zu überprüfen, ob temporäre Zugriffsbedingungen erfüllt sind:
   >
   > * Die für den Promotion-TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.
   > * Die maximale Anzahl von Ressourcen, die für die Promotion TempPass konfiguriert sind, darf nicht verbraucht werden.
   >
   > <br/>
   > 
   > Wenn die Validierung des temporären Zugriffs fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Mit Entscheidungsflüssen fortfahren:** Wenn die Profil-Endpunktantwort ein Profil enthält, verwendet die Streaming-Anwendung die temporären Profilinformationen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.

1. **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Endpunkt Entscheidungsautorisierung aufruft.

   >[!IMPORTANT]
   > 
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API:
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   > 
   > Der Entscheidungsendpunkt Autorisierung erfordert bei Verwendung des Promotionsprinzips TempPass das Vorhandensein von `AP-TempPass-Identity` -Kopfzeilen. Die Kopfzeile enthält eindeutige Informationen zur Identität des Benutzers, der auf den Inhalt zugreift.
   > 
   > <br/>
   > 
   > Weitere Informationen zum Header `AP-TempPass-Identity` finden Sie in der Dokumentation [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) .

1. **Validieren Sie die Werbeaktion TempPass:** Der Adobe Pass-Server überprüft, ob eine gültige Konfigurationseinstellung für die Werbeaktion TempPass vorhanden ist, die auf die Integration zwischen dem bereitgestellten `serviceProvider` und dem bereitgestellten `mvpd` angewendet wird.

1. **Rückgabe `Permit` -Entscheidung mit Medien-Token:** Die Antwort des Endpunkts Entscheidungsautorisierung enthält eine `Permit` -Entscheidung und ein Medien-Token.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API .
   > 
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   > 
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung verwendet die Anfragedaten, um zu überprüfen, ob temporäre Zugriffsbedingungen erfüllt sind:
   >
   > * Die für den Promotion-TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.
   > * Die maximale Anzahl von Ressourcen, die für die Promotion TempPass konfiguriert sind, darf nicht verbraucht werden.
   >
   > <br/>
   > 
   > Wenn die Validierung des temporären Zugriffs fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Endpunkt Entscheidungsautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API:
   >
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   > 
   > Der Entscheidungsendpunkt Autorisierung erfordert bei Verwendung des Promotionsprinzips TempPass das Vorhandensein von `AP-TempPass-Identity` -Kopfzeilen. Die Kopfzeile enthält eindeutige Informationen zur Identität des Benutzers, der auf den Inhalt zugreift.
   >
   > <br/>
   > 
   > Weitere Informationen zum Header `AP-TempPass-Identity` finden Sie in der Dokumentation [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) .

1. **Validieren Sie die Werbeaktion TempPass:** Der Adobe Pass-Server überprüft, ob eine gültige Konfigurationseinstellung für die Werbeaktion TempPass vorhanden ist, die auf die Integration zwischen dem bereitgestellten `serviceProvider` und dem bereitgestellten `mvpd` angewendet wird.

1. **Rückgabe `Deny` -Entscheidung mit Details:** Die Antwort des Endpunkts Entscheidungsautorisierung enthält eine `Deny` -Entscheidung und eine Fehler-Payload, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API .
   > 
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung verwendet die Anfragedaten, um zu überprüfen, ob temporäre Zugriffsbedingungen erfüllt sind:
   >
   > * Die für den Promotion-TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.
   > * Die maximale Anzahl von Ressourcen, die für die Promotion TempPass konfiguriert sind, darf nicht verbraucht werden.
   >
   > <br/>
   > 
   > Wenn die Validierung des temporären Zugriffs fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Entscheidungsdetails verarbeiten `Deny`:** Die Streaming-Anwendung verarbeitet die Fehlerinformationen aus der Antwort und kann sie verwenden, um optional eine bestimmte Meldung auf der Benutzeroberfläche anzuzeigen.

   >[!NOTE]
   >
   > Empfehlung: Die Streaming-Anwendung kann Benutzer darüber informieren, dass die maximale Ressourcenanzahl überschritten wurde, und ihnen empfehlen, einen einfachen Authentifizierungsfluss mit einem regulären MVPD zu initiieren, um die Wiedergabe fortzusetzen.

## Abrufen von Autorisierungsentscheidungen bei Ablauf von einfachen oder Werbe-TempPass {#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

### Voraussetzungen {#prerequisites-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

Bevor Autorisierungsentscheidungen abgerufen werden, wenn grundlegende oder Werbe-TempPass abläuft, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* [Voraussetzungen vor dem Abrufen von Autorisierungsentscheidungen mit dem einfachen TempPass](#prerequisites-retrieve-authorization-decisions-using-basic-temppass).
* [Voraussetzungen vor dem Abrufen von Autorisierungsentscheidungen mithilfe des Promotionprogramms TempPass](#prerequisites-retrieve-authorization-decisions-using-promotional-temppass).

>[!IMPORTANT]
>
> Annahmen
> 
> <br/>
> 
> * Es muss eine gültige Konfigurationseinstellung für einfachen oder Werbe-TempPass geben, der auf die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` angewendet wird.
> * Die für den einfachen oder Werbe-TempPass konfigurierte TTL (Time-To-Live) ist abgelaufen.

### Workflow {#workflow-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

Führen Sie die angegebenen Schritte aus, um den Autorisierungsfluss zu implementieren, wenn der grundlegende oder Werbe-TempPass abläuft, wie im folgenden Diagramm dargestellt.

![Abrufen von Autorisierungsentscheidungen, wenn der grundlegende oder Werbe-TempPass abläuft](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires-flow.png)

*Abrufen von Autorisierungsentscheidungen, wenn der grundlegende oder Werbe-TempPass abläuft*

1. **Autorisierungsentscheidung abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um eine Autorisierungsentscheidung für eine bestimmte Ressource zu erhalten, indem sie den Endpunkt Entscheidungsautorisierung aufruft.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API:
   > 
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider`, `mvpd` und `resources`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_
   >
   > <br/>
   > 
   > Der Entscheidungsendpunkt Autorisierung erfordert bei Verwendung des Promotionsprinzips TempPass das Vorhandensein von `AP-TempPass-Identity` -Kopfzeilen. Die Kopfzeile enthält eindeutige Informationen zur Identität des Benutzers, der auf den Inhalt zugreift.
   > 
   > <br/>
   > 
   > Weitere Informationen zum Header `AP-TempPass-Identity` finden Sie in der Dokumentation [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) .

1. **Validieren Sie den einfachen oder Werbe-TempPass:** Der Adobe Pass-Server überprüft, ob eine gültige Konfigurationseinstellung für den einfachen oder Werbe-TempPass vorhanden ist, der auf die Integration zwischen dem bereitgestellten `serviceProvider` und dem bereitgestellten `mvpd` angewendet wird.

1. **Rückgabe `Deny` -Entscheidung mit Details:** Die Antwort des Endpunkts Entscheidungsautorisierung enthält eine `Deny` -Entscheidung und eine Fehler-Payload, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entspricht.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den in einer Entscheidungsantwort bereitgestellten Informationen finden Sie in der Dokumentation zur [Abrufen von Autorisierungsentscheidungen mithilfe einer bestimmten mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -API .
   > 
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   > 
   > Der Endpunkt Entscheidungsautorisierung verwendet die Anfragedaten, um zu überprüfen, ob temporäre Zugriffsbedingungen erfüllt sind:
   >
   > * Die für den einfachen oder Werbe-TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.
   > * Die maximale Anzahl von Ressourcen, die für die Promotion TempPass konfiguriert sind, darf nicht verbraucht werden.
   >
   > <br/>
   > 
   > Wenn die Validierung des temporären Zugriffs fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Entscheidungsdetails verarbeiten `Deny`:** Die Streaming-Anwendung verarbeitet die Fehlerinformationen aus der Antwort und kann sie verwenden, um optional eine bestimmte Meldung auf der Benutzeroberfläche anzuzeigen.

   >[!NOTE]
   >
   > Empfehlung: Die Streaming-Anwendung kann Benutzer darüber informieren, dass der temporäre Zugriff abgelaufen ist, und ihnen empfehlen, einen einfachen Authentifizierungsfluss mit einem regulären MVPD zu initiieren, um die Wiedergabe fortzusetzen.

## Profil für einfachen TempPass abrufen {#retrieve-profile-for-basic-temppass}

>[!IMPORTANT]
>
> Die Abfrage des Endpunkts Profile ist für den einfachen TempPass optional.

### Voraussetzungen {#prerequisites-retrieve-profile-for-basic-temppass}

Bevor Sie das Profil für den einfachen TempPass abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung möchte das temporäre Profil abrufen, um sicherzustellen, dass der temporäre Zugriff nicht abgelaufen ist.

>[!IMPORTANT]
>
> Annahmen
> 
> <br/>
> 
> * Es muss eine gültige Konfigurationseinstellung des einfachen TempPass geben, der auf die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` angewendet wird.
> * Die für den einfachen TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.

### Workflow {#workflow-retrieve-profile-information-for-basic-temppass}

Führen Sie die angegebenen Schritte aus, um den Profilabruf-Fluss für den einfachen TempPass zu implementieren, wie im folgenden Diagramm dargestellt.

![Profil für grundlegenden TempPass abrufen](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-profile-for-basic-temppass-flow.png)

*Profil für grundlegenden TempPass abrufen*

1. **Profil für einfachen TempPass abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen für einfachen TempPass abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen des Profils für bestimmte mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) :
   > 
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `mvpd`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Validieren des einfachen TempPass-Dienstes:** Der Adobe Pass-Server überprüft, ob eine gültige Konfigurationseinstellung des einfachen TempPass vorhanden ist, der auf die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` angewendet wird.

1. **Rückgabe von Informationen zum temporären Profil:** Die Antwort des Profilendpunkts enthält Informationen zum temporären Profil, einschließlich des Attributs `type`, das auf &quot;temporär&quot;gesetzt ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Profilantwort finden Sie in der Dokumentation zur API [Profil für bestimmte mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) -API.
   > 
   > <br/>
   > 
   > Der Endpunkt Profile validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   > 
   > Der Endpunkt Profile verwendet die Anfragedaten, um zu überprüfen, ob temporäre Zugriffsbedingungen erfüllt sind:
   >
   > * Die für den einfachen TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.
   >
   > <br/>
   > 
   > Wenn die Validierung des temporären Zugriffs fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Mit Entscheidungsflüssen fortfahren:** Wenn die Profil-Endpunktantwort ein Profil enthält, verwendet die Streaming-Anwendung die temporären Profilinformationen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.

## Profil für die Promotion TempPass abrufen {#retrieve-profile-for-promotional-temppass}

>[!IMPORTANT]
>
> Die Abfrage des Endpunkts Profile ist für die Promotion TempPass optional.

### Voraussetzungen {#prerequisites-retrieve-profile-for-promotional-temppass}

Bevor Sie das Profil für die Promotion &quot;TempPass&quot;abrufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Streaming-Anwendung möchte das temporäre Profil abrufen, um sicherzustellen, dass der temporäre Zugriff nicht abgelaufen ist, oder um zu bestimmen, wie viele Ressourcen noch wiedergegeben werden können.

>[!IMPORTANT]
>
> Annahmen
>
> <br/>
> 
> * Es muss eine gültige Konfigurationseinstellung für die Promotion TempPass geben, die auf die Integration zwischen den bereitgestellten `serviceProvider` und `mvpd` angewendet wird.
> * Die für den Promo-TempPass konfigurierte TTL (Time-To-Live) ist nicht abgelaufen.
> * Die maximale Anzahl von Ressourcen, die für die Promotion TempPass konfiguriert wurden, wurde nicht verbraucht.

### Workflow {#workflow-retrieve-profile-information-for-promotional-temppass}

Führen Sie die angegebenen Schritte aus, um den Profilabruf-Fluss für die Promotion TempPass zu implementieren, wie im folgenden Diagramm dargestellt.

![Profil für die Promotion TempPass abrufen](../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-profile-for-promotional-temppass-flow.png)

*Profil für die Promotion TempPass abrufen*

1. **Profil für den Promotion-TempPass abrufen:** Die Streaming-Anwendung erfasst alle erforderlichen Daten, um Profilinformationen für den Promo-TempPass abzurufen, indem eine Anfrage an den Endpunkt Profile gesendet wird.

   >[!IMPORTANT]
   >
   > Weitere Informationen finden Sie in der Dokumentation zur API [Abrufen des Profils für bestimmte mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) :
   > 
   > * Alle _erforderlichen_ Parameter, wie `serviceProvider` und `mvpd`
   > * Alle _erforderlichen_ Kopfzeilen, wie `Authorization` und `AP-Device-Identifier`
   > * Alle Parameter und Kopfzeilen von _optional_

1. **Validieren Sie die Werbeaktion TempPass:** Der Adobe Pass-Server überprüft, ob eine gültige Konfigurationseinstellung für die Werbeaktion TempPass vorhanden ist, die auf die Integration zwischen dem bereitgestellten `serviceProvider` und dem bereitgestellten `mvpd` angewendet wird.

1. **Rückgabe von Informationen zum temporären Profil:** Die Antwort des Profilendpunkts enthält Informationen zum temporären Profil, einschließlich des Attributs `type`, das auf &quot;temporär&quot;gesetzt ist.

   >[!IMPORTANT]
   >
   > Weitere Informationen zu den Informationen in einer Profilantwort finden Sie in der Dokumentation zur API [Profil für bestimmte mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) -API.
   > 
   > <br/>
   > 
   > Der Endpunkt Profile validiert die Anfragedaten, um sicherzustellen, dass die grundlegenden Bedingungen erfüllt sind:
   >
   > * Die Parameter und Header _required_ müssen gültig sein.
   > * Die Integration zwischen dem bereitgestellten `serviceProvider` und `mvpd` muss aktiv sein.
   >
   > <br/>
   > 
   > Wenn die grundlegende Validierung fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.
   >
   > <br/>
   > 
   > Der Endpunkt Profile verwendet die Anfragedaten, um zu überprüfen, ob temporäre Zugriffsbedingungen erfüllt sind:
   >
   > * Die für den Promotion-TempPass konfigurierte TTL (Time-To-Live) darf nicht abgelaufen sein.
   > * Die maximale Anzahl von Ressourcen, die für die Promotion TempPass konfiguriert sind, darf nicht verbraucht werden.
   >
   > <br/>
   > 
   > Wenn die Validierung des temporären Zugriffs fehlschlägt, wird eine Fehlerantwort generiert, die zusätzliche Informationen bereitstellt, die der Dokumentation [Verbesserte Fehlercodes](../../../enhanced-error-codes.md) entsprechen.

1. **Mit Entscheidungsflüssen fortfahren:** Wenn die Profil-Endpunktantwort ein Profil enthält, verwendet die Streaming-Anwendung die temporären Profilinformationen, um mit nachfolgenden Entscheidungsflüssen fortzufahren.
