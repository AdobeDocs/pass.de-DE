---
title: Grundlagen zu Adobe-Umgebungen
description: Grundlagen zu Adobe-Umgebungen
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Grundlagen zu Adobe-Umgebungen {#understanding-the-adobe-environments}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Die offizielle Dokumentation zu den Adobe-Umgebungen ist verfügbar. [Einrichten der Umgebung und Testen in der Pre-qual-Phase](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md):

Die Adobe-Umgebungen, zusammengefasst in wenigen Worten:

Adobe verfügt über zwei Umgebungen: **Vorqualifikation** und **Version**.

* In der Umgebung &quot;Vorqualifizierung&quot;bereiten wir den neuen Build vor, der veröffentlicht werden soll.

* Der aktuelle Release-Build befindet sich in der Veröffentlichungsumgebung.

Jede Umgebung hat zwei Profile: **staging** und **production**.

* Das Testprofil stellt eine Verbindung zum MVPDs-Staging-Server her
* Das Produktionsprofil stellt eine Verbindung zum MVPDs-Produktionsprofil her.

Der Grund für die Verwendung der beiden Profile besteht darin, dass wir im Testprofil neue Partner für die Live-Schaltung vorbereiten und das System entweder mit dem bevorstehenden Build (Vorqualifikation) oder mit der Version 1 (stabiler) testen möchten.

Wenn ein Partner die neue Version testen möchte, müssen einige weitere Schritte durchgeführt werden. Siehe [Einrichten der Umgebung und Testen in der Pre-qual-Phase](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

Wenn Sie die obigen Schritte ausführen, ist sichergestellt, dass die bevorstehende Version in der Vorqualifikationsumgebung getestet wird.
