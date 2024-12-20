---
title: Benutzermetadaten-Funktion
description: Benutzermetadaten-Funktion
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# Benutzermetadaten {#user-metadata}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

</br>
</br>

## Einführung {#intro}

Die Funktion „Benutzermetadaten“ ermöglicht es Programmierern, auf verschiedene Arten von benutzerspezifischen Daten zuzugreifen, die von MVPDs verwaltet werden.  Zu den Benutzer-Metadatentypen gehören Postleitzahlen, Bewertungen der Eltern, Benutzer-IDs und mehr.  *Benutzer* Metadaten sind eine Erweiterung der zuvor verfügbaren *statischen* Metadaten (Authentifizierungstoken-TTL, Autorisierungstoken-TTL und Geräte-ID).


Wichtige Punkte zu Benutzermetadaten:

- Wird während der Authentifizierungs- und Autorisierungsflüsse an das Programm des Programmierers übergeben
- Werte werden im Token gespeichert
- Werte können normalisiert werden, wenn verschiedene MVPDs Daten in verschiedenen Formaten bereitstellen
- Einige Parameter können mit dem Schlüssel des Programmierers verschlüsselt werden (z. B. Postleitzahl). Siehe [Benutzermetadatenzertifikat für die Verschlüsselung](user-metadata-certificate.md) für die Erstellung von Verschlüsselungszertifikaten
- Bestimmte Werte werden per Adobe über eine Konfigurationsänderung zur Verfügung gestellt

## Abrufen von Benutzermetadaten {#obtaining}

Benutzermetadaten stehen Programmierern über die AccessEnabler-`getMetadata()` und den `/usermetadata`-Endpunkt in der Clientless-API zur Verfügung.  Weitere Informationen zur Verwendung von `getMetadata()` und der entsprechenden Callback-`setMetadataStatus()` (oder für die in der Client-losen API verwendeten Endpunkte und Parameter) finden Sie in der Dokumentation zur Platform-API .

Programmierer erhalten Metadaten, indem sie einen Schlüssel für den Typ der Metadaten angeben, die sie abrufen möchten: `getMetadata(key)`.

Die Metadaten werden wie folgt zurückgegeben: `setMetadataStatus(key, encrypted, data)`:

| Parameter | Typ | Beschreibung |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | Zeichenfolge | Gibt den Typ der angeforderten Metadaten an |
| `encrypted` | Boolesch | Ein boolesches Flag, das angibt, ob der „Wert“ verschlüsselt ist oder nicht. Wenn dies „true“ ist, ist „value“ tatsächlich eine JSON-Web-verschlüsselte Darstellung des tatsächlichen Werts. |
| `data` | Objekt | Ein JSON-Objekt, das die Darstellung der Metadaten enthält |



Die Struktur des Datenparameters, Werte können je nach Typ variieren:

| Schlüssel | Werttyp | Beispiel | Beschreibung |
| --- | --- | --- | --- |
| `zip` | JSON-Array | \[„77754“, „12345“\] | Postleitzahl |
| `householdID` | JSON-String | „1O7241P“ | Haushaltskennung. Wenn MVPD keine Unterkonten unterstützt, ist dies identisch mit `userID` |
| `maxRating` | JSON-Objekt | { MPAA: „NR“, <br> VCHIP: „X“, <br> URL: &quot;http://manage.my/parental&quot; } | Maximale Kindersicherungsstufe für den Benutzer |
| `userID` | JSON-String | „1O7241P“ | Die Benutzerkennung. Falls eine MVPD Unterkonten unterstützt und der Benutzer nicht das Hauptkonto ist, unterscheidet sich `userID` von `householdID`. |
| `channelID` | JSON-Array | \[„channel-1“, „channel-2“ \] | Eine Liste der Kanäle, die ein Benutzer anzeigen darf |
| `is_hoh` | JSON-String | „1“ | Eine Markierung, die angibt, ob ein Benutzer Haushaltsleiter ist |
| `encryptedZip` | JSON-String | &quot;&quot; | Comcast stellt die Postleitzahl verschlüsselt zur Verfügung |
| `typeID` | JSON-String | Primär | Eine Markierung, die angibt, ob das Benutzerkonto ein primäres/sekundäres Konto ist |
| `primaryOID` | JSON-String | „UUIDd1e19ec9-012c-124f-b520-acaf118d16a0“ | Haushaltskennung. Wenn `typeID` Primär ist, enthält den Wert der `userID` |
| hba_status | Boolesch | „true“ „false“ | Eine boolesche Markierung, die angibt, ob die Home-Based-Authentifizierung für eine bestimmte Integration aktiviert ist |
| allowMirroring | Boolesch | „true“ „false“ | Gibt an, ob die Bildschirmspiegelung für dieses Gerät zulässig ist oder nicht |

>[!NOTE]
>
> **Hinweis:** Wenn der Datenparameter verschlüsselt ist, wie dies normalerweise für den **Zip-Schlüssel** der Fall ist, ist die Darstellung des Metadatenschlüssels eine JSON-Zeichenfolge anstelle eines Arrays oder eines Objekts.


**Wichtig:** Welche Benutzermetadaten einem Programmierer tatsächlich zur Verfügung stehen, hängt davon ab, was MVPD zur Verfügung stellt.  Rechtliche Vereinbarungen müssen mit MVPDs unterzeichnet werden, bevor sensible Benutzermetadaten (wie Zip-Code) in der Produktionsumgebung verfügbar gemacht werden.

</br>


| -Name | Details | Verschlüsselung erforderlich | Kommentare |
| --- | --- | --- | --- |
| Benutzer-ID | Wie vom MVPD bereitgestellt | Nein | Dies ist der Wert, der dann vom Adobe gehasht und im Medien-Token und im sendTrackingData()-Rückruf verfügbar gemacht wird.<br><br>Der Hash-Vorgang wurde in diesem Fall aus historischen Gründen durchgeführt<br><br>Diese ID kann eine Haushalts-ID oder eine Unterkonto-ID sein. Dies wird in der Regel nicht angegeben, sondern es ist nur die ID, die mit der Anmeldung verknüpft ist, die zu diesem Zeitpunkt verwendet wurde (bei der es sich um ein primäres oder ein Unterkonto handeln kann) |
| Upstream-Benutzer-ID | Von MVPD bereitgestellt, nur zur Verwendung für Flüsse zur Überwachung von gleichzeitigen Aufrufen | Nein | Dieser Wert wird verwendet, wenn Parallelitätsbeschränkungen für Sites und Apps von MVPD und Programmierern erzwungen werden. <br><br>Die ID kann auch Richtlinien zur Überwachung von <br><br> enthalten. Für die meisten MVPDs entspricht dieser Wert der Benutzer-ID. |
| Haushaltsbenutzer-ID | Wird vom MVPD bereitgestellt und dient hauptsächlich zur elterlichen Kontrolle | Nein | ID, die es Programmierern ermöglicht, die Nutzung von Haushalts- und Unterkonten zu verstehen.<br><br>Es wird manchmal als Ersatz für die Kindersicherung verwendet, wenn keine echten Bewertungen verfügbar sind (wenn der Benutzer mit dem Haushaltskonto angemeldet war, das er ansehen kann, andernfalls werden bewertete Inhalte nicht angezeigt)<br><br>Es gibt viele Unterschiede zwischen den verschiedenen VPDs für die Darstellung - Haushaltsbenutzer-ID, Haushaltsleiter-ID, Haushaltsflagge usw. |
| Haushaltsvorstand | Zeigt an, ob das Girokonto Haushaltsvorstand ist oder nicht | Nein | Siehe oben |
| Typ-ID/Primäre ID | Kennungen des Haushaltskontos | Nein | AT&amp;T-spezifische Indikatoren für Haushaltsvorstände.<br><br>Typ-ID = Flag, das angibt, ob das Benutzerkonto ein primäres/sekundäres Konto ist<br><br>Primäre OID = Haushaltskennung. Wenn TypeID Primär ist, enthält den Wert der userID |
| Max. Bewertung | Die maximal zulässige Bewertung für das aktuelle Konto | Nein | Ermöglicht es Programmierern, Inhalte herauszufiltern, die für ein Konto nicht geeignet sind.<br><br>Hat MPAA- oder VCHIP-Bewertungen |
| Kanalanordnung | Liste der im Package des Benutzers verfügbaren Kanäle | Nein | Wird verwendet, um schnell verschiedene Kanäle von Portalen zuzulassen oder daraus zu entfernen, die mehrere Netzwerke aggregieren</br></br> *Bitte beachten Sie, dass die Preflight-Autorisierung im Allgemeinen mehr Flexibilität für diesen Anwendungsfall ermöglicht und stattdessen verwendet werden sollte* <br><br>Die OLCA-Spezifikation ermöglicht dies als AttributeStatement in der AuthN-Antwort |
| HBA-Status | Gibt an, ob die Authentifizierung über HBA stattgefunden hat | Nein |     |
| Postleitzahl | Postleitzahl für die Rechnungsstellung des Benutzers | Ja | Wird für die Übertragung oder Sportveranstaltungen verwendet<br><br>Kann auch mit der AuthZ-Antwort für schnelle Aktualisierungen/<br><br> Daten bereitgestellt werden, erfordert MVPD-Rechtsvereinbarungen |
| Verschlüsselte Postleitzahl | Postleitzahl für die Rechnungsstellung des Benutzers (Comcast) | Ja | Wie oben, jedoch mit Comcast verschlüsselt |
| Sprache | User language settings | Nein | Wird verwendet, um Nachrichten entsprechend den Benutzereinstellungen anzuzeigen |
| Spiegelung zulassen | Gibt an, ob die Bildschirmspiegelung für dieses Gerät zulässig ist oder nicht | Nein |     |



## Verfügbare Metadaten {#available_metadata}

In der folgenden Tabelle wird der aktuelle Status von Benutzermetadaten im Adobe Pass-Authentifizierungs-Ökosystem dargestellt:


|     | **Legal **<br><br>**Agreement **<br><br>**Signed (nur Zip)** | **Benutzer-ID **<br><br>**bei AuthN** | **Postleitzahl **<br><br>**bei AuthN/Z** | **Rating **<br><br>**on AuthN/Z** | **Haushalt **<br><br>**ID auf AuthN/Z** | **Kanal-ID bei AuthN** | **Haushaltsvorstand auf AuthN** | **Typ-ID in AuthN** | **Primäre OID bei AuthN** | Sprache | Upstream-Benutzer-ID **bei AuthN** | HBA-Status | OnNet | inHome | Spiegelung bei AuthZ zulassen | **Hinweise** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Formaler Name** | Nicht zutreffend | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | Sprache | upstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. Für AuthN - Der OiosamlMetadataParser muss geändert werden, sodass für alle Parser dieses neue Attribut aktiviert ist <br>2.  Für AuthZ gibt es keine generische Methode, da die Autorisierung MVPD-spezifisch ist |
| **Verschlüsselung erforderlich** | Nicht zutreffend | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| **sensitiv** | Nicht zutreffend | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |     |     |
| **Adobe-IDp** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Ja** | **Ja** | **Ja** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Keine rechtliche Vereinbarung erforderlich, kann aktiviert werden. |
| **Synacor** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Rechtliche Vereinbarung deckt nicht alle MVPDs ab.**   <br>Dies ist allgemeine Unterstützung für Synacor - möglicherweise nicht für alle ihre MVPDs bereitgestellt. |
| Teller | **Nein** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Gibt die gleiche Liste wie alle Synacor-MVPDs plus upstreamUserID weiter. |
| komcast | **Nein** | **Ja** | **Nein** | **Ja (nur AuthZ)** | **Ja (nur AuthZ)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** |     |
| **AT&amp;T** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Rechtsvereinbarung unterzeichnet. |
| **Cablevision** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Rechtsvereinbarung unterzeichnet. |
| **HTC** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| **Proxy-Massilion** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Rechtsvereinbarung unterzeichnet. |
| **Proxy ClearLeap** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthZ)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Rechtsvereinbarung unterzeichnet. |
| Rogers | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| RCN | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Chartervertrag | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Verizon | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** |     |
| Ost-Link | **Nein** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Proxy-GLDS | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| DTV | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| COX | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Cogeco | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Videotron | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja*** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Zeigt die householdID mit dem gleichen Wert wie userID an |
| Spektrum | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Ja** |     |
| **Alle anderen **<br><br>**MVPDs** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Noch keine rechtliche Vereinbarung, vertrauliche Metadaten nicht für die Produktion verfügbar.** <br>Für alle MVPDs ist die Benutzer-ID ohne zusätzlichen Aufwand verfügbar. |


Die Liste der Benutzer-Metadatentypen wird erweitert, sobald neue Typen verfügbar gemacht und zum Adobe Pass-Authentifizierungssystem hinzugefügt werden.

## Code-Beispiele {#code_samples}

- [Code-Beispiel 1](#code_sample1)
- [Code-Beispiel 2 (Pseudo-getMetadata)](#code_sample2)


### Code-Beispiel 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### Code-Beispiel 2 (Pseudo-getMetadata) {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
