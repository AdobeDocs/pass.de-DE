---
title: Fragen und Antworten zu Zertifikaten
description: Fragen und Antworten zu Zertifikaten
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# Fragen und Antworten zu Zertifikaten {#certificates-q}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>

**Q1:** Ist es möglich, Zertifikate in iOS und Android zu registrieren?

**A:** Das Zertifikat für iOS und Android ist in der aktuellen Konfiguration identisch. Das native Zertifikat wird für beide Plattformen verwendet.

</br>

**Q2:** Können dieselben iOS-Zertifikate als Primär- und Backup-Zertifikate in den Produktions- und Staging-Umgebungen verwendet werden? Wenn es nicht empfohlen wird, können Sie eine Erklärung anbieten?

**A:** Es hat keinen Zweck, dasselbe Zertifikat wie das Primäre und das Backup-Zertifikat zu konfigurieren. Wir haben das Konzept von Primären und Backup-Zertifikaten, sodass wir mehrere Zertifikate für einen Programmierer konfigurieren können, falls das Primäre Zertifikat abläuft oder widerrufen wird. Durch ein Backup-Zertifikat erhalten Programmierer Zeit, das Primäre zu ändern, ohne dass sich dies auf die Veröffentlichungsumgebung auswirkt. Sie können jedoch denselben Satz von Primären und Backup-Zertifikaten sowohl für die Produktions- als auch für die Staging-Profile verwenden.

</br>

**Q3:** Wird ein neues Zertifikat für Webseiten benötigt, die den neuen flexiblen TempPass verwenden?

**A:** Das Zertifikat (und jedes Zertifikat tatsächlich) wird auf Ebene von Media Company &amp; Programmer konfiguriert. FlexibleTempPass ist ein MVPD, Sie müssen dafür kein Zertifikat konfigurieren. Wenn Sie also einen vorhandenen Programmierer mit flexiblem TempPass integrieren, wird das bereits auf Programmier-/Media-Firmenebene konfigurierte Zertifikat verwendet.
