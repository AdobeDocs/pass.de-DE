---
title: iOS/tvOS-Mechanismus zur Überprüfung der Speicherintegrität
description: iOS/tvOS-Integritätsprüfmechanismus
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# iOS/tvOS-Integritätsprüfmechanismus {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#Intro}

Ab Version 3.8.3 des iOS/tvOS AccessEnabler SDK ist die Option zur Durchführung von Integritätsprüfungen bei der Initialisierung von AccessEnabler verfügbar.

Um diesen Mechanismus zu verwenden, wurde die API mit einer zusätzlichen Initialisierungsmethode für die AccessEnabler-Klasse erweitert.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Integritätsprüfungen {#Checks}

Die Integritätsprüfungen sind nützlich, wenn ein Beschädigung des AccessEnabler-Speichers vermutet wird (z. B. wenn während eines Lese-/Schreibspeichervorgangs eine Wettlaufbedingung auftritt).

Die folgenden Prüfungen sind für die Initialisierung von AccessEnabler verfügbar:
- Speicheroperabilität: überprüft, ob Lese- und Schreibvorgänge erfolgreich sind
- Integrität gespeicherter Werte: überprüft, ob alle Werte gültig sind und das erwartete Format aufweisen

>[!IMPORTANT]
> 
>Wenn eine der Prüfungen fehlschlägt, werden alle Werte im Speicher gelöscht und der Benutzer wird abgemeldet, was zu einem schlechten Benutzererlebnis führen kann. Verwenden Sie die Integritätsprüfungen nur, wenn dies für erforderlich gehalten wird.


## Standardverhalten {#Default}

Die Prüfung der Speicherintegrität ist bei der Initialisierung von AccessEnabler mit der standardmäßigen Initialisierungsmethode standardmäßig deaktiviert:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

Um explizit anzugeben, welche Integritätsprüfungen bei der Initialisierung von AccessEnabler durchgeführt werden sollen, verwenden Sie die folgende Initialisierungsmethode:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

Der Enum IntegrityCheckType wird der Clientanwendung bereitgestellt und hat die folgenden Werte:

| Wert | Durchgeführte Prüfungen | Datenspeicherung gelöscht | Beschreibung | Empfohlener Anwendungsfall |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Keines | Nie | Bei der Speicherinitialisierung werden keine Integritätsprüfungen durchgeführt | Wann die SDK-Flüsse erwartungsgemäß funktionieren |
| INTEGRITY_CHECK_ALL | Speicheroperabilität <br/> Gültigkeit gespeicherter Werte | Bei Prüffehler | Alle verfügbaren Integritätsprüfungen werden bei der Speicherinitialisierung durchgeführt | Wenn eine Beschädigung des SDK-Speichers vermutet wird. <br/> Wenn eine der Integritätsprüfungen fehlschlägt, wird der Benutzer abgemeldet. |
| INTEGRITY_CHECK_CLEAR | Keines | Immer | Der Speicher wird bei der Speicherinitialisierung gelöscht | Wenn die SDK-Flüsse nicht wie erwartet abgeschlossen werden können |
