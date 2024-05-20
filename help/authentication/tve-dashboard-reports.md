---
title: Berichte
description: Erfahren Sie, wie die Daten in TVE Dashboard-Berichten aggregiert werden.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Berichte {#Reports}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle -Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Die **Berichte** im TVE Dashboard bietet Zugriff auf aggregierte Daten für AuthN TTL-, AuthZ TTL- und SSO-Berichte. Diese Berichte umfassen Ihre Kanalintegrationen mit verschiedenen MVPDs für alle [Plattformen](#platforms).

Mit Berichten können Sie Daten filtern und Erkenntnisse über [bestimmte Kanäle oder MVPDs](#selecting-specific-channels-mvpds). Sie können Berichte auch in eine CSV-Datei exportieren, um sie weiter zu analysieren.

## Berichte anzeigen {#view-reports}

Führen Sie diese Schritte aus, um einen bestimmten Bericht anzuzeigen.

1. Wählen Sie die **Berichte** im linken Bereich.
1. Wählen Sie eine der folgenden Registerkarten aus, um aggregierte Daten der enthaltenen Kanäle und MVPDs anzuzeigen und zu exportieren:
   * [AuthN TTL-Berichte](#authn-ttl-reports)
   * [AuthZ TTL-Berichte](#authz-ttl-reports)
   * [SSO-Berichte](#sso-reports)

   ![Berichtstyp](assets/type-of-reports.png)

   *Berichtstyp*

### AuthN TTL-Berichte {#authn-ttl-reports}

Die AuthN-TTL-Berichte, auch &quot;Authentication Time-To-Live (TTL)&quot;genannt, zeigen die Dauer an, für die Authentifizierungstoken für Ihre Kanalintegrationen mit verschiedenen MVPDs in allen [Plattformen](#platforms). Mit diesen Berichten können Sie überprüfen, wie lange ein Benutzer für einen bestimmten MVPD und eine bestimmte Plattform authentifiziert ist. Die Dauer-Werte werden in benutzerfreundlichen Formaten wie **Tage**, **Stunden**, **Minuten**, und **Sekunden**. Die Tabelle AuthN TTL Reports bietet horizontale und vertikale Scrollfunktionen, um verschiedene Bildschirmgrößen aufzunehmen.

Sie können auch Daten für [bestimmte Kanäle oder MVPDs](#selecting-specific-channels-mvpds).

![AuthN TTL-Berichte exportieren](assets/authn-ttl-reports.png)

*AuthN TTL-Berichte exportieren*

>[!IMPORTANT]
>
> Die **Festgelegt durch MVPD** Platzhalter wird verwendet, wenn der MVPD den AuthN-TTL-Wert anstelle der Adobe Pass-Authentifizierungskonfiguration erzwingt.

Auswählen **Berichte exportieren** , um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

### AuthZ TTL-Berichte {#authz-ttl-reports}

Die AuthZ-TTL-Berichte, auch &quot;Authorization Time-To-Live&quot;(TTL) genannt, zeigen die Dauer des Autorisierungstokens an, das für Ihre Kanalintegrationen mit verschiedenen MVPDs in allen Bereichen konfiguriert wurde [Plattformen](#platforms). Mit diesen Berichten können Sie überprüfen, wie viel Zeit ein Benutzer für die Wiedergabe von Inhalten auf einem bestimmten MVPD und einer bestimmten Plattform benötigt. Die Dauer-Werte werden in benutzerfreundlichen Formaten wie **Tage**, **Stunden**, **Minuten**, und **Sekunden**. Die Tabelle &quot;AuthZ TTL Reports&quot;verfügt über ein horizontales und vertikales Scrollen, um verschiedene Bildschirmgrößen aufzunehmen.

Sie können auch die Daten für [bestimmte Kanäle oder MVPDs](#selecting-specific-channels-mvpds).

![AuthZ TTL-Berichte exportieren](assets/authz-ttl-reports.png)

*AuthZ TTL-Berichte exportieren*

>[!IMPORTANT]
>
> Die **Festgelegt durch MVPD** Platzhalter wird verwendet, wenn der MVPD den AuthZ-TTL-Wert anstelle der Adobe Pass-Authentifizierungskonfiguration erzwingt.

Auswählen **Berichte exportieren** , um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

### SSO-Berichte {#sso-reports}

Die SSO-Berichte, auch Single Sign-on genannt, zeigen den Single Sign-On-Status an, der für Ihre Kanal-Integrationen mit verschiedenen MVPDs in allen [Plattformen](#platforms). Mit diesen Berichten können Sie das erwartete SSO-Erlebnis für die Benutzerauthentifizierung auf einen bestimmten MVPD und eine bestimmte Plattform überprüfen. Die Werte werden in benutzerfreundlichen Formaten wie **SSO deaktiviert**, **SSO aktiviert**, und **SSO Nicht sicher**. Die Tabelle &quot;SSO-Berichte&quot;verfügt über ein horizontales und vertikales Scrollen, um verschiedene Bildschirmgrößen aufzunehmen.

Sie können auch Daten für [bestimmte Kanäle oder MVPDs](#selecting-specific-channels-mvpds).

![Exportieren von SSO-Berichten](assets/sso-reports.png)

*Exportieren von SSO-Berichten*

>[!IMPORTANT]
>
> Die **SSO Nicht sicher** Platzhalter gibt an, dass Single Sign-On (SSO) aktiviert ist und potenziell funktionsfähig ist. Die unten aufgeführten Einstellungen können jedoch die SSO-Authentifizierung verhindern, wie in den folgenden Beispielen erläutert:
>
> * Benutzerplattformeinstellungen: Die Option zum Blockieren von Drittanbieter-Cookies.
> * Benutzerentscheidungen: Die Benutzer verweigern den Plattformzugriff auf ihr Abonnement des TV-Anbieters.
> * MVPD-Einstellungen: MVPD fordert Authentifizierung für jeden Kanal an.

Auswählen **Berichte exportieren** , um die Daten als CSV-Datei auf Ihrem lokalen Computer zu speichern.

## Plattformen {#platforms}

Die [AuthN TTL-Berichte](#authn-ttl-reports), [AuthZ TTL-Berichte](#authz-ttl-reports), und [SSO-Berichte](#sso-reports) Daten über verschiedene Plattformen hinweg darstellen, z. B.:

* **Desktop**: Zeigt Werte an, die auf die Programmimplementierungen über das Adobe Pass Authentication JavaScript SDK angewendet wurden.

* **Mobilnummer**

  **iOS**: Zeigt Werte an, die mit dem Adobe Pass Authentication iOS SDK angewendet wurden.

  **Android**: Zeigt Werte an, die über das Adobe Pass Authentication Android SDK angewendet werden.

  **sonstige**: Zeigt Werte an, die mit der Adobe Pass Authentication REST API angewendet wurden, die für Mobilgeräte entwickelt wurde.

* **TVCD**

  **Roku**: Zeigt Werte an, die über die Adobe Pass Authentication REST API angewendet werden und Roku als Gerätetyp identifizieren.

  **FireTV**: Zeigt Werte an, die über das Adobe Pass Authentication FireTV SDK angewendet werden.

  **AppleTV**: Zeigt Werte an, die über das Adobe Pass Authentication tvOS SDK angewendet wurden.

  **sonstige**: Zeigt Werte an, die mit der Adobe Pass Authentication REST API für TV-verbundene Geräte angewendet wurden.

* **Plattform nicht identifiziert**: Zeigt Werte an, die auf Programmimplementierungen angewendet werden, wenn die Adobe Pass-Authentifizierungsdienste einen unbekannten Gerätetyp erkennen.

Weitere Informationen zum Freigeben des gewünschten Gerätetyps, z. B. **Roku** mit Adobe Pass Authentication REST APIs oder SDKs, den Mechanismus von [Übergeben von Client-Informationen](/help/authentication/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> Die aggregierten Daten basieren auf der spezifischen Konfiguration jeder Adobe Pass-Authentifizierungsumgebung. Beim Wechsel zwischen verschiedenen TVE-Dashboard-Umgebungen sollten Sie Abweichungen in den Daten zwischen den Berichten erwarten. Siehe Abschnitt [Adobe Pass-Authentifizierungsumgebungen](/help/authentication/tve-dashboard-environments.md) um mehr zu erfahren.

## Auswählen bestimmter Kanäle und MVPDs {#selecting-specific-channels-mvpds}

Die [AuthN TTL-Berichte](#authn-ttl-reports), [AuthZ TTL-Berichte](#authz-ttl-reports), und [SSO-Berichte](#sso-reports) Daten für **Alle Kanäle** Integrationen mit **Alle MVPDs** Standardmäßig.

>[!NOTE]
>
> Wenn Sie die Auswahl aufheben **Alle Kanäle** oder **Alle MVPDs** In den entsprechenden Dropdown-Menüs wird eine Meldung angezeigt, die eine Auswahl trifft, um aussagekräftige Berichte anzuzeigen.

So generieren Sie einen Bericht für bestimmte Kanäle:

1. Wählen Sie die **Einbezogene Kanäle** Dropdown-Menü am oberen Rand des ausgewählten Berichts aus.

   ![Dropdown-Menü &quot;Einbezogene Kanäle&quot;](assets/include-channels.png)

   *Dropdown-Menü &quot;Einbezogene Kanäle&quot;*

1. Auswahl deaktivieren **Alle Kanäle**.
1. Wählen Sie die erforderlichen Kanäle aus dem **Einbezogene Kanäle** Dropdown-Menü, für das Sie Daten generieren möchten.

>[!NOTE]
>
> So stellen Sie Optionen in der **Enthaltene MVPDs** Dropdown-Menü müssen Sie mindestens einen Kanal im **Einbezogene Kanäle** Dropdown-Menü.

So generieren Sie einen Bericht für bestimmte MVPDs:

1. Wählen Sie die **Enthaltene MVPDs** Dropdown-Menü am oberen Rand des ausgewählten Berichts aus.

   ![Eingeschlossenes Dropdown-Menü MVPDs](assets/include-mvpds.png)

   *Eingeschlossenes Dropdown-Menü MVPDs*

1. Auswahl deaktivieren **Alle MVPDs**.
1. Wählen Sie die erforderlichen MVPDs aus der **Enthaltene MVPDs** Dropdown-Menü, für das Sie Daten generieren möchten.
