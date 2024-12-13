---
title: Fragen und Antworten zu Zertifikaten
description: Fragen und Antworten zu Zertifikaten
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Fragen und Antworten zu (älteren) Zertifikaten {#certificates-q}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>

**Q1:** Ist es möglich, Zertifikate in iOS und Android zu registrieren?

**A:** Das Zertifikat für iOS und Android ist in der aktuellen Konfiguration identisch. Das native Zertifikat wird für beide Plattformen verwendet.

</br>

**Q2:** Können dieselben iOS-Zertifikate als Primär- und Sicherungszertifikate in der Produktions- und Staging-Umgebung verwendet werden? Wenn es nicht empfohlen wird, können Sie eine Erklärung geben?

**A:** Es hat keinen Sinn, dasselbe Zertifikat zu konfigurieren, das sowohl Primär- als auch Sicherungszertifikat ist. Wir haben das Konzept der Primär- und Sicherungszertifikate, sodass wir mehrere Zertifikate für einen Programmierer konfigurieren können, falls das Primäre Zertifikat abläuft oder widerrufen wird. Ein Backup-Zertifikat gibt Programmierern Zeit, das Primäre Zertifikat zu ändern, ohne die Release-Umgebung zu beeinträchtigen. Sie können jedoch denselben Satz von Primär- und Sicherungszertifikaten sowohl für das Produktions- als auch für das Staging-Profil verwenden.

</br>

**Q3:** Wird ein neues Zertifikat für Webseiten benötigt, die den neuen flexiblen TempPass verwenden werden?

**A:** Das Zertifikat (und jedes beliebige Zertifikat) wird auf der Ebene der Mediengesellschaft und des Programmierers konfiguriert. FlexibleTempPass ist eine MVPD, für die Sie kein Zertifikat konfigurieren müssen. Wenn Sie also einen vorhandenen Programmierer mit flexiblem TempPass integrieren, wird das Zertifikat verwendet, das bereits auf Programmierer-/Medienfirmenebene konfiguriert wurde.
