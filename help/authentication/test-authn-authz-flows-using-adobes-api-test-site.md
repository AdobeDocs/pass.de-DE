---
title: Testen der Authentifizierungs-Autorisierungsflüsse mithilfe der Adobe API-Test-Site
description: Testen der Authentifizierungs-Autorisierungsflüsse mithilfe der Adobe API-Test-Site
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Testen von Authentifizierungs-Autorisierungsflüssen mithilfe der Adobe API Test-Site {#How-to-test-auth-flows}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Um die AuthN- und AuthZ-Flüsse zu testen, haben wir eine **API-Testseite** die Ihnen zur Verfügung steht. Unser Support-Team stellt Ihnen gerne Anmeldedaten zur Verfügung. Sie können uns kontaktieren unter **support@tve.zendesk.com**.


## Teil I {#part-I}

Überspringen Sie zum Testen mit der RELEASE-Umgebung direkt zu Teil II.  Informationen zum Testen in der Vorqualifizierungsumgebung finden Sie unter [Einrichten der Umgebung und Testen in der Pre-Qual-Phase](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## Teil II

Führen Sie nach Abschluss von Teil I die folgenden Schritte aus:


1. Öffnen Sie die Webseite: [Staging-API-Test](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Laden Sie den Enabler von Zugriff über die folgende URL:
   * [Zugriff auf aktiviertes JavaScript für das Staging](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * ODER
   * [Zugriff auf aktiviertes JavaScript für die Produktion](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * Klicken Sie anschließend auf den **Zugriffsaktivierung laden**&quot;.
1. Setzen Sie nun den Anforderer-ID-Wert auf &quot;**requestorID** und klicken Sie auf die Schaltfläche &quot;setRequestor&quot;.
1. Drücken Sie danach die Taste &quot;getAuthentication&quot; und warten Sie, bis die Anzeige der Anzeigeauswahl erscheint.
1. Wählen Sie &quot;**MVPD**&quot; aus der Auswahl.
1. Geben Sie Ihre Anmeldedaten ein unter &quot;**MVPD**&quot;Anmeldeseite.
1. Nachdem Sie zurückgeleitet wurden, wiederholen Sie die Schritte 1 bis 3
1. Nach dem Wiederholen von Schritt 3 für &quot;setAuthenticationStatus&quot;sollte der Wert &quot;1&quot;angezeigt werden. Wenn die Authentifizierung nicht funktioniert hat, wird das MVPD-Dialogfeld angezeigt.
1. Um die Autorisierung zu testen, geben Sie im Eingabefeld der Ressource &quot;**requestorID**&quot; und klicken Sie auf die Schaltfläche &quot;getAuthorization&quot;.
1. Daher wird im Textfeld &quot;setToken&quot;-\>&quot;resource id&quot;die Ressource angezeigt und im Textfeld &quot;setToken&quot;-\>&quot;token&quot;wird das shortAuthorizationToken angezeigt, was bedeutet, dass authZ erfolgreich war.
1. Jetzt können Sie auf die Schaltfläche &quot;Abmelden&quot;klicken, um die Token zu löschen.
