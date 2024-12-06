---
title: Grundlagen zu Adobe-Umgebungen
description: Grundlagen zu Adobe-Umgebungen
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Grundlagen zu Adobe-Umgebungen {#understanding-the-adobe-environments}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Die offizielle Dokumentation, die die Adobe-Umgebungen beschreibt, finden Sie unter [Einrichten der Umgebung und Testen in Pre-qual](/help/authentication/notes-technical/setting-up-your-environment-and-testing-in-prequal.md):

Die Adobe-Umgebungen, zusammengefasst in wenigen Worten:

Adobe verfügt über zwei Umgebungen: **Vorqualifikation** und **Version**.

* In der Umgebung &quot;Vorqualifizierung&quot;bereiten wir den neuen Build vor, der veröffentlicht werden soll.

* Der aktuelle Release-Build befindet sich in der Veröffentlichungsumgebung.

Jede Umgebung hat zwei Profile: **staging** und **production**.

* Das Testprofil stellt eine Verbindung zum MVPDs-Staging-Server her
* Das Produktionsprofil stellt eine Verbindung zum MVPDs-Produktionsprofil her.

Der Grund für die Verwendung der beiden Profile besteht darin, dass wir im Testprofil neue Partner für die Live-Schaltung vorbereiten und das System entweder mit dem bevorstehenden Build (Vorqualifikation) oder mit der Version 1 (stabiler) testen möchten.

Wenn ein Partner die neue Version testen möchte, müssen einige weitere Schritte durchgeführt werden. Siehe [Einrichten Ihrer Umgebung und Testen in Pre-qual](/help/authentication/notes-technical/setting-up-your-environment-and-testing-in-prequal.md).

Wenn Sie die obigen Schritte ausführen, ist sichergestellt, dass die bevorstehende Version in der Vorqualifikationsumgebung getestet wird.
