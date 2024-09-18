---
title: Berichte
description: Erfahren Sie, wie die Daten in TVE Dashboard-Berichten aggregiert werden.
exl-id: d8ba48de-d743-4dc2-866c-7d6e3ff94773
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Berichte {#Reports}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle -Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Der Abschnitt **Berichte** im TVE-Dashboard bietet Zugriff auf aggregierte Daten für AuthN TTL-, AuthZ TTL- und SSO-Berichte. Diese Berichte umfassen Ihre Kanalintegrationen mit verschiedenen MVPDs für alle [Plattformen](#platforms).

Mit Berichten können Sie Daten filtern und Erkenntnisse über [bestimmte Kanäle oder MVPDs](#selecting-specific-channels-mvpds) sammeln. Sie können Berichte auch in eine CSV-Datei exportieren, um sie weiter zu analysieren.

## Berichte anzeigen {#view-reports}

Führen Sie diese Schritte aus, um einen bestimmten Bericht anzuzeigen.

1. Wählen Sie im linken Bereich die Registerkarte **Berichte** aus.
1. Wählen Sie eine der folgenden Registerkarten aus, um aggregierte Daten der enthaltenen Kanäle und MVPDs anzuzeigen und zu exportieren:
   * [AuthN TTL-Berichte](#authn-ttl-reports)
   * [AuthZ TTL-Berichte](#authz-ttl-reports)
   * [SSO-Berichte](#sso-reports)

   ![Berichtstyp](../../assets/tve-dashboard/new-tve-dashboard/reports/reports-tabs-view.png)

   *Berichtstyp*

### AuthN TTL-Berichte {#authn-ttl-reports}

Die AuthN-TTL-Berichte, auch &quot;Authentication Time-To-Live (TTL)&quot;genannt, zeigen die Dauer an, für die Authentifizierungstoken für Ihre Kanalintegrationen mit verschiedenen MVPDs auf allen [Plattformen](#platforms) konfiguriert sind. Mit diesen Berichten können Sie überprüfen, wie lange ein Benutzer für einen bestimmten MVPD und eine bestimmte Plattform authentifiziert ist. Die Dauer-Werte werden in benutzerfreundlichen Formaten wie **Tage**, **Stunden**, **Minuten** und **Sekunden** dargestellt. Die Tabelle AuthN TTL Reports bietet horizontale und vertikale Scrollfunktionen, um verschiedene Bildschirmgrößen aufzunehmen.

Sie können auch Daten für [bestimmte Kanäle oder MVPDs](#selecting-specific-channels-mvpds) anzeigen und herunterladen.

![AuthN TTL-Berichte exportieren](../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*AuthN TTL-Berichte exportieren*

>[!IMPORTANT]
>
> Der Platzhalter **Von MVPD gesetzt** wird verwendet, wenn der MVPD den AuthN-TTL-Wert anstelle der Adobe Pass-Authentifizierungskonfiguration erzwingt.

Wählen Sie **Berichte exportieren** aus, um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

### AuthZ TTL-Berichte {#authz-ttl-reports}

Die AuthZ-TTL-Berichte, auch &quot;Authorization Time-To-Live&quot;(TTL) genannt, zeigen die Dauer des Autorisierungstokens an, das für Ihre Kanalintegrationen mit verschiedenen MVPDs auf allen [Plattformen](#platforms) konfiguriert wurde. Mit diesen Berichten können Sie überprüfen, wie viel Zeit ein Benutzer für die Wiedergabe von Inhalten auf einem bestimmten MVPD und einer bestimmten Plattform benötigt. Die Dauer-Werte werden in benutzerfreundlichen Formaten wie **Tage**, **Stunden**, **Minuten** und **Sekunden** dargestellt. Die Tabelle &quot;AuthZ TTL Reports&quot;verfügt über ein horizontales und vertikales Scrollen, um verschiedene Bildschirmgrößen aufzunehmen.

Sie können die Daten auch für [bestimmte Kanäle oder MVPDs](#selecting-specific-channels-mvpds) anzeigen und herunterladen.

![AuthZ TTL-Berichte exportieren](../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*AuthZ TTL-Berichte exportieren*

>[!IMPORTANT]
>
> Der Platzhalter **Von MVPD gesetzt** wird verwendet, wenn der MVPD den AuthZ-TTL-Wert anstelle der Adobe Pass-Authentifizierungskonfiguration erzwingt.

Wählen Sie **Berichte exportieren** aus, um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

### SSO-Berichte {#sso-reports}

Die SSO-Berichte, auch Single Sign-on genannt, zeigen den Single Sign-On-Status an, der für Ihre Kanal-Integrationen mit verschiedenen MVPDs auf allen [Plattformen](#platforms) konfiguriert ist. Mit diesen Berichten können Sie das erwartete SSO-Erlebnis für die Benutzerauthentifizierung auf einen bestimmten MVPD und eine bestimmte Plattform überprüfen. Die Werte werden in benutzerfreundlichen Formaten wie &quot;**SSO deaktiviert**&quot;, &quot;**SSO aktiviert**&quot;und &quot;**SSO nicht sicher**&quot;dargestellt. Die Tabelle &quot;SSO-Berichte&quot;verfügt über ein horizontales und vertikales Scrollen, um verschiedene Bildschirmgrößen aufzunehmen.

Sie können auch Daten für [bestimmte Kanäle oder MVPDs](#selecting-specific-channels-mvpds) anzeigen und herunterladen.

![SSO-Berichte exportieren](../../assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)

*SSO-Berichte exportieren*

>[!IMPORTANT]
>
> Der Platzhalter **SSO Unsicher** gibt an, dass Single Sign-On (SSO) aktiviert ist und möglicherweise funktionsfähig ist. Die unten aufgeführten Einstellungen können jedoch die SSO-Authentifizierung verhindern, wie in den folgenden Beispielen erläutert:
>
> * Benutzerplattformeinstellungen: Die Option zum Blockieren von Drittanbieter-Cookies.
> * Benutzerentscheidungen: Die Benutzer verweigern den Plattformzugriff auf ihr Abonnement des TV-Anbieters.
> * MVPD-Einstellungen: MVPD fordert Authentifizierung für jeden Kanal an.

Wählen Sie **Berichte exportieren** aus, um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

## Plattformen {#platforms}

Die [AuthN TTL-Berichte](#authn-ttl-reports), [AuthZ TTL-Berichte](#authz-ttl-reports) und [SSO-Berichte](#sso-reports) enthalten Daten über verschiedene Plattformen, z. B.:

* **Desktop**: Zeigt Werte an, die auf die Programmimplementierungen über das Adobe Pass Authentication JavaScript SDK angewendet wurden.

* **Mobil**

  **iOS**: Zeigt Werte an, die mit dem Adobe Pass Authentication iOS SDK angewendet wurden.

  **Android**: Zeigt Werte an, die über das Adobe Pass Authentication Android SDK angewendet werden.

  **Sonstige**: Zeigt Werte an, die mit der Adobe Pass Authentication REST API angewendet wurden, die für Mobilgeräte entwickelt wurde.

* **TVCD**

  **Roku**: Zeigt Werte an, die über die Adobe Pass Authentication REST API angewendet werden und Roku als Gerätetyp identifizieren.

  **FireTV**: Zeigt Werte an, die über das Adobe Pass Authentication FireTV SDK angewendet werden.

  **AppleTV**: Zeigt Werte an, die über das Adobe Pass Authentication tvOS SDK angewendet wurden.

  **Sonstige**: Zeigt Werte an, die mit der Adobe Pass Authentication REST API für TV-verbundene Geräte angewendet wurden.

* **Plattform nicht identifiziert**: Zeigt Werte an, die auf Programmimplementierungen angewendet werden, wenn die Adobe Pass-Authentifizierungsdienste einen unbekannten Gerätetyp erkennen.

Um mehr darüber zu erfahren, wie Sie den gewünschten Gerätetyp, z. B. **Roku** mit Adobe Pass Authentication REST APIs oder SDKs teilen können, sehen Sie sich den Mechanismus zur Weitergabe von Client-Informationen an [ an.](/help/authentication/passing-client-information-device-connection-and-application.md)

>[!IMPORTANT]
>
> Die aggregierten Daten basieren auf der spezifischen Konfiguration jeder Adobe Pass-Authentifizierungsumgebung. Beim Wechsel zwischen verschiedenen TVE-Dashboard-Umgebungen sollten Sie Abweichungen in den Daten zwischen den Berichten erwarten. Weitere Informationen finden Sie in den [Adobe Pass-Authentifizierungsumgebungen](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-environments.md) .

## Auswählen bestimmter Kanäle und MVPDs {#selecting-specific-channels-mvpds}

Die [AuthN TTL Reports](#authn-ttl-reports), [AuthZ TTL Reports](#authz-ttl-reports) und [SSO Reports](#sso-reports) enthalten standardmäßig Daten für die Integration von **Alle Kanäle** mit **Alle MVPDs**.

>[!NOTE]
>
> Wenn Sie die Auswahl von **Alle Kanäle** oder **Alle MVPDs** in den entsprechenden Dropdown-Menüs aufheben, wird eine Meldung angezeigt, die eine Auswahl trifft, um aussagekräftige Berichte anzuzeigen.

So generieren Sie einen Bericht für bestimmte Kanäle:

1. Wählen Sie oben im ausgewählten Bericht das Dropdown-Menü **Einbezogene Kanäle** aus.

   ![Dropdown-Menü &quot;Einbezogene Kanäle&quot;](../../assets/tve-dashboard/new-tve-dashboard/reports/reports-included-channels-menu.png)

   *Dropdown-Menü &quot;Einbezogene Kanäle&quot;*

1. Heben Sie die Auswahl von **Alle Kanäle** auf.

1. Wählen Sie die erforderlichen Kanäle aus dem Dropdown-Menü **Einbezogene Kanäle** aus, für die Sie Daten generieren möchten.

>[!NOTE]
>
> Um Optionen im Dropdown-Menü **Eingeschlossene MVPDs** verfügbar zu haben, müssen Sie mindestens einen Kanal im Dropdown-Menü **Einbezogene Kanäle** auswählen.

So generieren Sie einen Bericht für bestimmte MVPDs:

1. Wählen Sie das Dropdown-Menü **Eingeschlossene MVPDs** oben im ausgewählten Bericht aus.

   ![ Dropdown-Menü &quot;Eingeschlossene MVPDs&quot;](../../assets/tve-dashboard/new-tve-dashboard/reports/reports-included-mvpds-menu.png)

   *Dropdown-Menü &quot;Eingeschlossene MVPDs&quot;*

1. Deaktivieren Sie **Alle MVPDs**.

1. Wählen Sie die erforderlichen MVPDs aus dem Dropdown-Menü **Eingeschlossene MVPDs** aus, für die Sie Daten generieren möchten.
