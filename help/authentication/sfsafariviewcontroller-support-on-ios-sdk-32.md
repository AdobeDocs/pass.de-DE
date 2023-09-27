---
title: SFSafariViewController-Unterstützung für iOS SDK 3.2+
description: SFSafariViewController-Unterstützung für iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# SFSafariViewController-Unterstützung für iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>


**Aufgrund von Sicherheitsanforderungen MÜSSEN einige Anmeldeseiten von MVPD in einem SFSafariViewController anstatt in Webansichten angezeigt werden.**

Einige MVPDs erfordern, dass ihre Anmeldeseiten in einem sicheren Browser-Steuerelement wie SFSafariViewController angezeigt werden. Sie blockieren aktiv Webansichten. Um sich bei ihnen authentifizieren zu können, müssen wir SVC verwenden.

## Kompatibilität {#compatiblity}

Ab iOS SDK-Version 3.1 zeigt das AccessEnabler SDK basierend auf der Serverkonfiguration automatisch die Anmeldeseite für bestimmte MVPDs in einem SFSafariViewController an.

Version 3.1 des SDK zeigt automatisch den SFSafariViewController vom Stamm-View-Controller der Anwendung an. Dies vereinfacht zwar die Verwaltung der Anmeldeseiten für Implementoren, es gibt jedoch Fälle, in denen die Präsentation des SFSafariViewController vom Root-View-Controller aufgrund der speziellen Implementierung der App nicht möglich ist (wie ein Modal-Controller, der bereits sichtbar ist).

In solchen Fällen bietet die Version 3.2 dem Programmierer die Möglichkeit, das SVC manuell zu verwalten.

## Manuelles SVC-Management {#manual-svc-management}

Um SVC manuell zu verwalten, muss der Implementor die folgenden Schritte ausführen:


1. call **setOptions([&quot;handleSVC&quot;:true])** nach der Initialisierung von AccessEnabler (stellen Sie sicher, dass dieser Aufruf durchgeführt wird, bevor die Authentifizierung beginnt). Dies ermöglicht die &quot;manuelle&quot;SVC-Verwaltung. Das SDK stellt die SVC nicht automatisch bereit, sondern ruft bei Bedarf stattdessen **navigate(toUrl:*{url}* useSVC:true)**.

1. den optionalen Rückruf implementieren **navigateToUrl:useSVC:** Innerhalb der Implementierung müssen Sie mithilfe der SFSafariViewController -Instanz mithilfe der angegebenen URL eine svc-Instanz erstellen und sie auf dem Bildschirm darstellen:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Hinweise:***

   - *Sie können den SFSafariViewController beliebig anpassen. Beispielsweise können Sie in iOS 11 und höher die Bezeichnung &quot;Fertig&quot;in &quot;Abbrechen&quot;ändern.*
   - *Um die svc schließen zu können, benötigen Sie einen Verweis darauf. Erstellen Sie ihn bitte nicht im Rahmen von **navigateToUrl:useSVC***
   - *Verwenden Sie Ihren eigenen Ansichtscontroller für &quot;myController&quot;.*


1. In der Delegate-Implementierung Ihrer Anwendung von **application(\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**, fügen Sie Code hinzu, um den svc zu schließen. Sie sollten dort bereits Code haben, der Aufrufe **accessEnabler.handleExternalURL()**. Fügen Sie Folgendes hinzu:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Auch hier ist svc ein Verweis auf den SFSafariViewController, den Sie in Schritt 2 erstellt haben.


1. Implementierung **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** von **SFSafariViewControllerDelegate** um zu erfassen, wann der Benutzer die svc mithilfe der Schaltfläche &quot;Fertig&quot;abgebrochen hat. In dieser Funktion müssen Sie Folgendes aufrufen, um das SDK darüber zu informieren, dass die Authentifizierung abgebrochen wurde:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
