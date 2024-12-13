---
title: Unterstützung von SFSafariViewController auf iOS SDK 3.2+
description: Unterstützung von SFSafariViewController auf iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# (Legacy) Unterstützung von SFSafariViewController auf iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

</br>


**Aufgrund von Sicherheitsanforderungen MÜSSEN einige Anmeldeseiten von MVPD in einem SFSafariViewController anstelle von Web-Ansichten angezeigt werden.**

Einige MVPDs erfordern, dass ihre Anmeldeseiten in einem sicheren Browser-Steuerelement wie SFSafariViewController angezeigt werden. Sie blockieren aktiv Webansichten. Um sich bei ihnen authentifizieren zu können, müssen wir SVC verwenden.

## Kompatibilität {#compatiblity}

Ab iOS SDK Version 3.1 zeigt AccessEnabler SDK automatisch die Anmeldeseite für bestimmte MVPDs in einem SFSafariViewController an, basierend auf der Server-Konfiguration.

In Version 3.1 des SDK wird der SFSafariViewController automatisch vom Root View Controller des Programms angezeigt. Dies vereinfacht zwar die Verwaltung der Anmeldeseite für Implementierungsprogramme, es gibt jedoch Fälle, in denen die Darstellung des SFSafariViewController vom Stamm-Ansichts-Controller aufgrund der spezifischen Implementierung des Programms nicht möglich ist (z. B. ein bereits sichtbarer modaler Controller).

Für solche Fälle bietet die Version 3.2 dem Programmierer die Möglichkeit, SVC manuell zu verwalten.

## Manuelle SVC-Verwaltung {#manual-svc-management}

Um SVC manuell verwalten zu können, muss das Implementierungsprogramm die folgenden Schritte ausführen:


1. Aufruf **setOptions([„handleSVC“:true])** nach der Initialisierung von AccessEnabler (stellen Sie sicher, dass dieser Aufruf ausgeführt wird, bevor die Authentifizierung beginnt). Dies ermöglicht eine „manuelle“ SVC-Verwaltung, die SDK stellt die SVC nicht automatisch dar, sondern bei Bedarf     Aufruf **navigate(toUrl:*{url}* useSVC:true)**.

1. Um die optionale Callback-**`navigateToUrl:useSVC:`** innerhalb der Implementierung zu implementieren, müssen Sie mithilfe der SFSafariViewController-Instanz unter Verwendung der angegebenen URL eine SVC-Instanz erstellen und auf dem Bildschirm darstellen:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Hinweise:***

   - *Sie können den SFSafariViewController beliebig anpassen. Beispielsweise können Sie unter iOS 11+ die Bezeichnung „Fertig“ in „Abbrechen“ ändern.*
   - *Um das svc schließen zu können, benötigen Sie einen Verweis darauf. Erstellen Sie es nicht im Bereich von „navigateToUrl **useSVC***
   - *Verwenden Sie Ihren eigenen Ansichtscontroller für „myController“*


1. In der delegierten Implementierung Ihrer Anwendung von **application(\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> bool** fügen Sie Code hinzu, um den svc zu schließen. Sie sollten dort bereits über Code verfügen, der **accessEnabler.handleExternalURL()**. Fügen Sie direkt unten hinzu:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Auch hier ist svc ein Verweis auf den SFSafariViewController, den Sie in Schritt 2 erstellt haben.


1. Implementieren Sie **safariViewControllerDidFinish(\_ Controller: SFSafariViewController)** aus **SFSafariViewControllerDelegate**, um festzustellen, wann Benutzer svc mithilfe der Schaltfläche „Fertig“ abgebrochen haben. Um in dieser Funktion die SDK darüber zu informieren, dass die Authentifizierung abgebrochen wurde, müssen Sie Folgendes aufrufen:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
