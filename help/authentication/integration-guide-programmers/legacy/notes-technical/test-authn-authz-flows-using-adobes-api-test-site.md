---
title: Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Test-Site der Adobe-API
description: Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Test-Site der Adobe-API
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# (Veraltet) Testen von Authentifizierungs- und Autorisierungsflüssen mithilfe der Adobe-API-Test-Site {#How-to-test-auth-flows}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Um die AuthN- und AuthZ-Flüsse zu testen, haben wir eine **API-Test-Site** vorbereitet, die Ihnen zur Verfügung steht. Unser Support-Team stellt Ihnen gerne Anmeldeinformationen zur Verfügung. Sie können uns unter **support@tve.zendesk.com** kontaktieren.


## Teil 1 {#part-I}

Zum Testen mit der RELEASE-Umgebung fahren Sie direkt mit Teil II fort.  Informationen zum Testen in der Umgebung für die Vorqualifizierung finden Sie unter [Einrichten der Umgebung und Testen in Pre-Qual](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

## Teil 2

Führen Sie nach Abschluss von Teil I die folgenden Schritte aus:


1. Webseite öffnen: [Staging-API-Test](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Load Access Enabler von der folgenden URL:
   * [Access Enabler-JavaScript für Staging](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * ODER
   * [Zugriff auf JavaScript für die Produktion](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * Klicken Sie anschließend auf die Schaltfläche **Load Access Enabler**.
1. Setzen Sie nun den Wert der Anforderer-ID auf &quot;**RequestorID** und klicken Sie auf die Schaltfläche „setRequestor“.
1. Klicken Sie anschließend auf die Schaltfläche „getAuthentication“ und warten Sie, bis die Anzeigeauswahl angezeigt wird.
1. Wählen Sie &quot;**MVPD**&quot; aus der Auswahl aus.
1. Geben Sie auf der Anmeldeseite von &quot;**MVPD**&quot; Ihre Anmeldedaten ein.
1. Wiederholen Sie nach der Umleitung die Schritte 1 bis 3
1. Nach der Wiederholung von Schritt 3 auf „setAuthenticationStatus“ sollte der Wert „1“ angezeigt werden. Wenn die Authentifizierung nicht funktioniert hat, wird das MVPD-Dialogfeld angezeigt.
1. Geben Sie zum Testen der Autorisierung im Feld Ressourceneingabe &quot;**RequestorID**&quot; ein und klicken Sie auf die Schaltfläche „getAuthorization“.
1. Daher wird im Textfeld „setToken“-\>„resource id“ die Ressource angezeigt und im Textfeld „setToken“-\>„token“ wird das kurze Autorisierungs-Token angezeigt, was bedeutet, dass authZ erfolgreich war.
1. Jetzt können Sie auf die Schaltfläche „Abmelden“ klicken, um die Token zu löschen.
