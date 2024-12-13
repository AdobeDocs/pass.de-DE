---
title: Grundlagen zu Adobe-Umgebungen
description: Grundlagen zu Adobe-Umgebungen
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Grundlagen zu Adobe-Umgebungen {#understanding-the-adobe-environments}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Die offizielle Dokumentation, die die Adobe-Umgebungen beschreibt, ist verfügbar [Einrichten der Umgebung und Testen in Pre-qual](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md):

Die Adobe-Umgebungen, zusammengefasst in wenigen Worten:

Adobe hat zwei Umgebungen **(**) und **Version**.

* In der Umgebung für die Vorqualifizierung bereiten wir den neuen Build für die Veröffentlichung vor.

* Der aktuelle Versions-Build befindet sich in der Veröffentlichungsumgebung.

Jede Umgebung hat zwei Profile: **Staging** und **Produktion**.

* Das Staging-Profil stellt eine Verbindung zum MVPD-Staging-Server her
* Das Produktionsprofil stellt eine Verbindung zum MVPDs-Produktionsprofil her.

Der Grund für die beiden Profile ist, dass wir im Staging-Profil neue Partner für die Live-Schaltung vorbereiten, und sie möchten das System entweder mit dem bevorstehenden Build (Vorqualifizierung) oder mit dem Release-Build (stabiler) testen.

Wenn ein Partner die neue Version testen möchte, sind einige zusätzliche Schritte erforderlich. Siehe [Einrichten einer Umgebung und Testen in Pre-qual](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

Durch Befolgen der oben genannten Schritte wird sichergestellt, dass die kommende Version in der Umgebung für die Vorqualifizierung getestet wird.
