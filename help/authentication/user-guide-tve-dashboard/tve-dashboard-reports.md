---
title: Berichte
description: Erfahren Sie, wie die Daten in TVE-Dashboard-Berichten aggregiert werden.
exl-id: d8ba48de-d743-4dc2-866c-7d6e3ff94773
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Berichte {#Reports}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Der Abschnitt **Berichte** im TVE-Dashboard bietet Zugriff auf aggregierte Daten für AuthN-TTL-, AuthZ-TTL- und SSO-Berichte. Diese Berichte enthalten Ihre Kanalintegrationen mit verschiedenen MVPDs in allen [Plattformen](#platforms).

Mit Berichten können Sie Daten filtern und Einblicke über ([&#x200B; Kanäle oder MVPDs) &#x200B;](#selecting-specific-channels-mvpds). Sie können Berichte auch zur weiteren Analyse in eine CSV-Datei exportieren.

## Anzeigen von Berichten {#view-reports}

Führen Sie diese Schritte aus, um einen bestimmten Bericht anzuzeigen.

1. Wählen Sie die **Berichte** im linken Bedienfeld aus.
1. Wählen Sie eine der folgenden Registerkarten aus, um aggregierte Daten der eingeschlossenen Kanäle und MVPDs anzuzeigen und zu exportieren:
   * [AuthNr. TTL-Berichte](#authn-ttl-reports)
   * [AuthZ-TTL-Berichte](#authz-ttl-reports)
   * [SSO-Berichte](#sso-reports)

   ![Berichtstyp](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-tabs-view.png)

   *Berichtstyp*

### AuthNr TTL-Berichte {#authn-ttl-reports}

Die AuthN-TTL-Berichte, auch als TTL (Authentication Time-To-Live) bezeichnet, zeigen die Dauer an, für die Authentifizierungs-Token für Ihre Kanalintegrationen mit verschiedenen MVPDs auf allen [Plattformen) konfiguriert &#x200B;](#platforms). Mit diesen Berichten können Sie überprüfen, wie lange ein Benutzer für eine bestimmte MVPD und Plattform authentifiziert bleibt. Die Werte für die Dauer werden in benutzerfreundlichen Formaten angezeigt, wie **Tage**, **Stunden**, **Minuten** und **Sekunden**. Die Tabelle AuthN TTL-Berichte bietet horizontalen und vertikalen Bildlauf, um verschiedene Bildschirmgrößen zu ermöglichen.

Sie können auch Daten für (bestimmte Kanäle [&#x200B; MVPDs) anzeigen und &#x200B;](#selecting-specific-channels-mvpds).

![Exportieren von AuthN-TTL-Berichten](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*Exportieren von AuthN-TTL-Berichten*

>[!IMPORTANT]
>
> Der **Von MVPD festgelegt** wird verwendet, wenn die MVPD den AuthN-TTL-Wert erzwingt, anstatt die Adobe Pass-Authentifizierungskonfiguration zu verwenden.

Wählen Sie **Berichte exportieren**, um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

### AuthZ-TTL-Berichte {#authz-ttl-reports}

Die AuthZ-TTL-Berichte, auch als Autorisierungs-Time-To-Live (TTL) bezeichnet, zeigen die Dauer des Autorisierungs-Tokens an, das für Ihre Kanalintegrationen mit verschiedenen MVPDs in allen [Plattformen) konfiguriert &#x200B;](#platforms). Mit diesen Berichten können Sie überprüfen, wie lange ein Benutzer berechtigt bleibt, Inhalte für eine bestimmte MVPD und Plattform zu beobachten. Die Werte für die Dauer werden in benutzerfreundlichen Formaten angezeigt, wie **Tage**, **Stunden**, **Minuten** und **Sekunden**. Die Tabelle AuthZ TTL-Berichte bietet horizontalen und vertikalen Bildlauf, um verschiedene Bildschirmgrößen zu ermöglichen.

Sie können die Daten auch für ([&#x200B; Kanäle oder MVPDs) anzeigen und &#x200B;](#selecting-specific-channels-mvpds).

![Exportieren von AuthZ-TTL-Berichten](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Exportieren von AuthZ-TTL-Berichten*

>[!IMPORTANT]
>
> Der **Von MVPD festgelegt** wird verwendet, wenn der MVPD den AuthZ-TTL-Wert erzwingt, anstatt die Adobe Pass-Authentifizierungskonfiguration zu verwenden.

Wählen Sie **Berichte exportieren**, um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

### SSO-Berichte {#sso-reports}

Die SSO-Berichte, auch als Single Sign-On bezeichnet, zeigen den Single Sign-On-Status an, der für Ihre Kanalintegrationen mit verschiedenen MVPDs über alle [Plattformen hinweg konfiguriert &#x200B;](#platforms). Diese Berichte ermöglichen es Ihnen, das erwartete SSO-Erlebnis für die Benutzerauthentifizierung für eine bestimmte MVPD und Plattform zu überprüfen. Die Werte werden in benutzerfreundlichen Formaten angezeigt, wie **SSO deaktiviert**, **SSO aktiviert** und **SSO unsicher**. Die Tabelle SSO-Berichte bietet horizontalen und vertikalen Bildlauf für verschiedene Bildschirmgrößen.

Sie können auch Daten für (bestimmte Kanäle [&#x200B; MVPDs) anzeigen und &#x200B;](#selecting-specific-channels-mvpds).

![Exportieren von SSO-Berichten](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)

*Exportieren von SSO-Berichten*

>[!IMPORTANT]
>
> Der Platzhalter **SSO Uncertain** gibt an, dass Single Sign-On (SSO) aktiviert ist und potenziell funktioniert. Die unten aufgeführten Einstellungen können jedoch die SSO-Authentifizierung behindern, wie in den folgenden Beispielen erläutert:
>
> * Benutzerplattformeinstellungen: Die Option zum Blockieren von Drittanbieter-Cookies.
> * Benutzerentscheidungen: Die Benutzer verweigern der Plattform den Zugriff auf ihr TV-Anbieter-Abonnement.
> * MVPD-Einstellungen: MVPD fordert die Authentifizierung für jeden Kanal an.

Wählen Sie **Berichte exportieren**, um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

## Plattformen {#platforms}

Die [AuthN-TTL](#authn-ttl-reports)Berichte, [AuthZ-TTL-Berichte](#authz-ttl-reports) und [SSO-Berichte &#x200B;](#sso-reports) Daten über verschiedene Plattformen hinweg dar, z. B.:

* **Desktop**: Zeigt Werte an, die über die Adobe Pass-Authentifizierung JavaScript SDK auf die Programmierimplementierungen angewendet werden.

* **Mobil**

  **iOS**: Zeigt Werte an, die mithilfe der Adobe Pass-Authentifizierung iOS SDK angewendet wurden.

  **Android**: Zeigt Werte an, die über die Adobe Pass-Authentifizierung Android SDK angewendet wurden.

  **Sonstige**: Zeigt Werte an, die mithilfe der für Mobilgeräte entwickelten Adobe Pass-Authentifizierungs-REST-API angewendet werden.

* **TVCD**

  **Roku**: Zeigt Werte an, die über die Adobe Pass-Authentifizierungs-REST-API angewendet werden, und identifiziert Roku als Gerätetyp.

  **FireTV**: Zeigt Werte an, die über die Adobe Pass-Authentifizierungs-FireTV-SDK angewendet werden.

  **AppleTV**: Zeigt Werte an, die über die Adobe Pass-Authentifizierungs-tvOS-SDK angewendet werden.

  **Sonstige**: Zeigt Werte an, die mithilfe der Adobe Pass-Authentifizierungs-REST-API für TV-Geräte angewendet werden.

* **Platform nicht identifiziert**: Zeigt Werte an, die auf Programmierimplementierungen angewendet werden, wenn die Adobe Pass-Authentifizierungsdienste einen unbekannten Gerätetyp erkennen.

Weitere Informationen zum Freigeben des gewünschten Gerätetyps, z. B. **Roku** mit Adobe Pass-Authentifizierungs-REST-APIs oder SDKs, finden Sie im Abschnitt zum [&#x200B; von Client-Informationen](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> Die aggregierten Daten basieren auf der spezifischen Konfiguration jeder Adobe Pass-Authentifizierungsumgebung. Wenn Sie zwischen verschiedenen TVE-Dashboard-Umgebungen wechseln, sollten Sie Variationen in den Daten der Berichte erwarten. Weitere Informationen finden Sie unter [Adobe Pass](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)Authentifizierungsumgebungen.

## Auswählen bestimmter Kanäle und MVPDs {#selecting-specific-channels-mvpds}

Die Berichte [AuthN TTL](#authn-ttl-reports), [AuthZ TTL](#authz-ttl-reports) und [SSO &#x200B;](#sso-reports) standardmäßig Daten für **Alle Kanäle**-Integrationen mit **Alle MVPDs**.

>[!NOTE]
>
> Wenn Sie die Auswahl **Alle Kanäle** oder **Alle MVPDs** in den entsprechenden Dropdown-Menüs aufheben, wird eine Meldung angezeigt, mit der Sie eine Auswahl treffen können, um aussagekräftige Berichte anzuzeigen.

So generieren Sie einen Bericht für bestimmte Kanäle:

1. Wählen Sie **Dropdown** Menü „Enthaltene Kanäle“ oben im ausgewählten Bericht.

   ![Dropdown-Menü „Enthaltene Kanäle“](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-included-channels-menu.png)

   *Dropdown-Menü „Enthaltene Kanäle“*

1. Deaktivieren Sie **Alle Kanäle**.

1. Wählen Sie die erforderlichen Kanäle aus dem Dropdown **Menü** Enthaltene Kanäle“ aus, für die Sie Daten generieren möchten.

>[!NOTE]
>
> Um Optionen im Dropdown-Menü **Enthaltene MVPDs** verfügbar zu haben, müssen Sie mindestens einen Kanal im Dropdown-Menü **Enthaltene Kanäle** auswählen.

So erstellen Sie einen Bericht für bestimmte MVPDs:

1. Wählen Sie **Dropdown** Menü „Enthaltene MVPDs“ oben im ausgewählten Bericht aus.

   ![Enthaltenes MVPDs-Dropdown-Menü](/help/authentication/assets/tve-dashboard/new-tve-dashboard/reports/reports-included-mvpds-menu.png)

   *Enthaltenes MVPDs-Dropdown-Menü*

1. Deaktivieren Sie **Alle MVPDs**.

1. Wählen Sie die erforderlichen MVPDs aus dem Dropdown **Menü „Enthaltene MVPDs**, für die Sie Daten generieren möchten.
