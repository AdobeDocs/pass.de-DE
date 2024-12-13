---
title: iOS/tvOS-Speicherintegritätsprüfung
description: iOS/tvOS-Integritätsprüfungsmechanismus
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# (Legacy) Integritätsprüfungsmechanismus von iOS/tvOS {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Einführung {#Intro}

Ab Version 3.8.3 der iOS/tvOS AccessEnabler-SDK ist die Option zur Durchführung von Speicherintegritätsprüfungen bei der Initialisierung von AccessEnabler verfügbar.

Um diesen Mechanismus verwenden zu können, wurde die API um eine zusätzliche Initialisierungsmethode für die AccessEnabler-Klasse erweitert.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Integritätsprüfungen {#Checks}

Die Speicherintegritätsprüfungen sind nützlich, wenn eine Beschädigung des AccessEnabler-Speichers vermutet wird (z. B. wenn während eines Lese-/Schreibspeichervorgangs eine Racebedingung auftritt).

Die folgenden Prüfungen können bei der Initialisierung von AccessEnabler durchgeführt werden:
- Speicherfunktionsfähigkeit: Überprüft, ob Lese- und Schreibvorgänge erfolgreich sind
- Integrität der gespeicherten Werte: Überprüft, ob alle Werte gültig und im erwarteten Format sind

>[!IMPORTANT]
> 
>Wenn eine der Prüfungen fehlschlägt, werden alle Werte im Speicher gelöscht und der Benutzer wird abgemeldet, was zu einem schlechten Benutzererlebnis führen kann. Verwenden Sie Integritätsprüfungen nur, wenn dies für erforderlich erachtet wird.


## Standardverhalten {#Default}

Die Speicherintegritätsprüfungen sind bei der Initialisierung von AccessEnabler mit der Standardinitialisierungsmethode standardmäßig deaktiviert:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

Um explizit anzugeben, welche Speicherintegritätsprüfungen bei der AccessEnabler-Initialisierung durchgeführt werden sollen, verwenden Sie die folgende Initialisierungsmethode:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

Die Enumeration „IntegrityCheckType“ wird für die Client-Anwendung verfügbar gemacht und hat die folgenden Werte:

| Wert | Durchgeführte Prüfungen | Speicher gelöscht | Beschreibung | Empfohlener Anwendungsfall |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Keine | Nie | Bei der Speicherinitialisierung werden keine Integritätsprüfungen durchgeführt. | Wenn die SDK-Flüsse erwartungsgemäß funktionieren |
| INTEGRITY_CHECK_ALL | Speicherfunktionalität <br/> Gültigkeit gespeicherter Werte | Bei fehlgeschlagener Prüfung | Alle verfügbaren Integritätsprüfungen werden bei der Speicherinitialisierung durchgeführt. | Bei Verdacht auf Beschädigung des SDK-Speichers <br/> Wenn eine der Integritätsprüfungen fehlschlägt, wird der Benutzer abgemeldet |
| INTEGRITY_CHECK_CLEAR | Keine | Immer | Der Speicher wird bei der Speicherinitialisierung gelöscht | Wenn die SDK-Flüsse nicht wie erwartet abgeschlossen werden können |
