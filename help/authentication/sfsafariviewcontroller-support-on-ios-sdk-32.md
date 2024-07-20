---
title: SFSafariViewController-Unterstützung für iOS SDK 3.2+
description: SFSafariViewController-Unterstützung für iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# SFSafariViewController-Unterstützung für iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>


**Aus Sicherheitsgründen MÜSSEN einige Anmeldeseiten von MVPD in einem SFSafariViewController anstatt in Webansichten angezeigt werden.**

Einige MVPDs erfordern, dass ihre Anmeldeseiten in einem sicheren Browser-Steuerelement wie SFSafariViewController angezeigt werden. Sie blockieren aktiv Webansichten. Um sich bei ihnen authentifizieren zu können, müssen wir SVC verwenden.

## Kompatibilität {#compatiblity}

Ab iOS SDK-Version 3.1 zeigt das AccessEnabler SDK basierend auf der Serverkonfiguration automatisch die Anmeldeseite für bestimmte MVPDs in einem SFSafariViewController an.

Version 3.1 des SDK zeigt automatisch den SFSafariViewController vom Stamm-View-Controller der Anwendung an. Dies vereinfacht zwar die Verwaltung der Anmeldeseiten für Implementoren, es gibt jedoch Fälle, in denen die Präsentation des SFSafariViewController vom Root-View-Controller aufgrund der speziellen Implementierung der App nicht möglich ist (wie ein Modal-Controller, der bereits sichtbar ist).

In solchen Fällen bietet die Version 3.2 dem Programmierer die Möglichkeit, das SVC manuell zu verwalten.

## Manuelles SVC-Management {#manual-svc-management}

Um SVC manuell zu verwalten, muss der Implementor die folgenden Schritte ausführen:


1. Rufen Sie **setOptions([&quot;handleSVC&quot;:true])** nach der Initialisierung von AccessEnabler auf (stellen Sie sicher, dass dieser Aufruf vor Beginn der Authentifizierung durchgeführt wird). Dies ermöglicht die &quot;manuelle&quot;SVC-Verwaltung. Das SDK zeigt die SVC nicht automatisch an, sondern bei Bedarf     Aufruf **navigate(toUrl:*{url}* useSVC:true)**.

1. implementieren Sie den optionalen Callback **`navigateToUrl:useSVC:`** in der Implementierung. Sie müssen eine svc-Instanz mithilfe der SFSafariViewController -Instanz mithilfe der bereitgestellten URL erstellen und auf dem Bildschirm darstellen:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Notizen:***

   - *Sie können den SFSafariViewController beliebig anpassen. Beispielsweise können Sie in iOS 11 und höher die Bezeichnung &quot;Fertig&quot;in &quot;Abbrechen&quot;ändern.*
   - *Um die svc schließen zu können, benötigen Sie einen Verweis darauf. Erstellen Sie ihn nicht im Bereich von **navigateToUrl:useSVC***
   - *Verwenden Sie Ihren eigenen Ansichtscontroller für &quot;myController&quot;*


1. Öffnen Sie in der Delegate-Implementierung Ihrer Anwendung von **application(\_app: UIApplication, url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool** den Code zum Schließen des SVC. Dort sollte bereits Code vorhanden sein, der **accessEnabler.handleExternalURL()** aufruft. Fügen Sie Folgendes hinzu:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Auch hier ist svc ein Verweis auf den SFSafariViewController, den Sie in Schritt 2 erstellt haben.


1. Implementieren Sie **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** von **SFSafariViewControllerDelegate** , um zu erfassen, wann der Benutzer die SVC über die Schaltfläche &quot;Fertig&quot;abgebrochen hat. In dieser Funktion müssen Sie Folgendes aufrufen, um das SDK darüber zu informieren, dass die Authentifizierung abgebrochen wurde:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
