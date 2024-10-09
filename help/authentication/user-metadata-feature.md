---
title: Funktion für Benutzermetadaten
description: Funktion für Benutzermetadaten
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8c5203aa4a897a5e119a9886afc64a1b1556ee4f
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# Benutzermetadaten {#user-metadata}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>
</br>

## Einführung {#intro}

Mit der Funktion &quot;Benutzermetadaten&quot;können Programmierer auf verschiedene Arten benutzerspezifischer Daten zugreifen, die von MVPDs gepflegt werden.  Zu den Metadatentypen für Benutzer gehören Postleitzahlen, elterliche Bewertungen, Benutzer-IDs und mehr.  *Benutzer* -Metadaten sind eine Erweiterung der zuvor verfügbaren *statischen* -Metadaten (Authentifizierungstoken TTL, Autorisierungstoken TTL und Geräte-ID).


Wichtige Punkte zu Benutzermetadaten:

- Wird während der Authentifizierungs- und Autorisierungsflüsse an die Anwendung des Programmierers übergeben
- Werte werden im Token gespeichert
- Werte können normalisiert werden, wenn verschiedene MVPDs Daten in verschiedenen Formaten bereitstellen
- Einige Parameter können mit dem Schlüssel des Programmierers verschlüsselt werden (z. B. Postleitzahl). Informationen zur Generierung von Verschlüsselungszertifikaten finden Sie unter [Benutzer-Metadatenzertifikat für Verschlüsselung](./user-metadata-certificate.md) .
- Spezifische Werte werden durch Adobe über eine Konfigurationsänderung bereitgestellt

## Abrufen von Benutzermetadaten {#obtaining}

Benutzermetadaten stehen Programmierern über die AccessEnabler `getMetadata()` -Funktion und über den `/usermetadata` -Endpunkt in der ClientLess-API zur Verfügung.  Weitere Informationen zur Verwendung von `getMetadata()` und dem zugehörigen Callback `setMetadataStatus()` (oder zu den Endpunkten und Parametern, die in der Client-losen API verwendet werden) finden Sie in der Dokumentation zur Plattform-API .

Programmierer erhalten Metadaten, indem sie einen Schlüssel für den Metadatentyp bereitstellen, den sie abrufen möchten: `getMetadata(key)`.

Die Metadaten werden wie folgt zurückgegeben: `setMetadataStatus(key, encrypted, data)`:

| Parameter | Typ | Beschreibung |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | Zeichenfolge | Gibt den Typ der angeforderten Metadaten an |
| `encrypted` | Boolesch | Eine boolesche Markierung, die angibt, ob der &quot;Wert&quot;verschlüsselt ist oder nicht. Wenn dies &quot;true&quot;ist, ist &quot;value&quot;tatsächlich eine JSON Web-verschlüsselte Darstellung des tatsächlichen Werts. |
| `data` | Objekt | Ein JSON-Objekt mit der Darstellung der Metadaten |



Die Struktur des Datenparameters und die Werte variieren zwischen den Typen:

| Schlüssel | Werttyp | Beispiel | Beschreibung |
| --- | --- | --- | --- |
| `zip` | JSON-Array | \[&quot;77754&quot;, &quot;12345&quot;\] | Postleitzahl |
| `householdID` | JSON-Zeichenfolge | &quot;1o7241p&quot; | Haushaltskennung. Wenn der MVPD keine Unterkonten unterstützt, ist dies identisch mit `userID` |
| `maxRating` | JSON-Objekt | { MPAA: &quot;NR&quot;, <br> VCHIP: &quot;X&quot;, <br> URL: &quot;http://manage.my/parental&quot; } | Maximale elterliche Bewertung für den Benutzer |
| `userID` | JSON-Zeichenfolge | &quot;1o7241p&quot; | Die Benutzer-ID. Wenn ein MVPD Subkonten unterstützt und der Benutzer nicht das Hauptkonto ist, unterscheidet sich `userID` von `householdID`. |
| `channelID` | JSON-Array | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | Eine Liste der Kanäle, die ein Benutzer anzeigen darf |
| `is_hoh` | JSON-Zeichenfolge | &quot;1&quot; | Eine Markierung, die angibt, ob ein Benutzer Haushaltsleiter ist |
| `encryptedZip` | JSON-Zeichenfolge | &quot;&quot; | Comcast legt die verschlüsselte Postleitzahl offen |
| `typeID` | JSON-Zeichenfolge | Primär | Eine Markierung, die angibt, ob das Benutzerkonto ein primäres/sekundäres Konto ist |
| `primaryOID` | JSON-Zeichenfolge | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Haushaltskennung. Wenn `typeID` Primär ist, enthält den Wert des `userID` |
| hba_status | Boolesch | &quot;true&quot; &quot;false&quot; | Eine boolesche Kennzeichnung, die angibt, ob die Home-basierte Authentifizierung für eine bestimmte Integration aktiviert ist |
| allowMirroring | Boolesch | &quot;true&quot; &quot;false&quot; | Gibt an, ob die Bildschirmspiegelung für dieses Gerät zulässig ist oder nicht |

>[!NOTE]
>
> **Hinweis:** Wenn der Datenparameter verschlüsselt ist, wie dies normalerweise bei der ZIP-Taste **der Fall ist, ist die Darstellung des Metadatenschlüssels eine JSON-Zeichenfolge anstelle eines Arrays oder Objekts.**


**Wichtig:** Die tatsächlichen Benutzermetadaten, die einem Programmierer zur Verfügung stehen, hängen davon ab, was ein MVPD bereitstellt.  Rechtliche Vereinbarungen müssen mit MVPDs unterzeichnet werden, bevor sensible Benutzermetadaten (wie Postleitzahl) in der Produktionsumgebung verfügbar gemacht werden.

</br>


| Name | Details | Erfordert Verschlüsselung | Kommentare |
| --- | --- | --- | --- |
| Benutzer-ID | Wie vom MVPD bereitgestellt | Nein | Dies ist der Wert, der dann von Adobe gehasht und im Medien-Token und im Rückruf sendTrackingData() verfügbar gemacht wird.<br><br>Das Hashing in diesem Fall wurde aus historischen Gründen durchgeführt<br><br>Diese ID kann eine Haushalts-ID oder eine Unter-Konto-ID sein. Dies ist normalerweise nicht angegeben, sondern nur die Kennung, die an die damals verwendete Anmeldung gebunden war (bei der es sich um ein primäres Konto oder ein Unterkonto handeln kann). |
| Upstream-Benutzer-ID | Wird vom MVPD bereitgestellt und darf nur für die Überwachung der Parallelität verwendet werden | Nein | Dieser Wert wird verwendet, wenn Parallelitätsbeschränkungen über MVPD- und Programmierer-Sites und -Apps hinweg erzwungen werden. <br><br>Die ID kann auch Richtlinien zur Überwachung von Parallelität enthalten<br><br>Für die meisten MVPDs entspricht dieser Wert der Benutzer-ID |
| Haushalts-Benutzer-ID | Wird vom MVPD bereitgestellt, um hauptsächlich für die elterlichen Kontrollflüsse verwendet zu werden | Nein | ID, die es Programmierern ermöglicht, die Nutzung von Haushalts- und Unterkonten zu verstehen.<br><br>Manchmal wird es als Ersatz für die elterliche Steuerung verwendet, wenn keine echten Bewertungen verfügbar sind (wenn der Benutzer mit dem Haushaltskonto angemeldet ist, das er ansehen kann, sonst würden keine bewerteten Inhalte angezeigt)<br><br>Es gibt eine Vielzahl von Variationen zwischen MVPDs, wie dies dargestellt wird - Haushalts-Benutzer-ID, Kopf der Haushalts-ID, Leiter der Haushaltskennzeichnung usw. |
| Leiter der Haushaltsabteilung | Kennzeichnungsanzeige, wenn das Leistungskonto Haushaltsleiter ist oder nicht | Nein | siehe oben |
| Typ-ID / Primäre ID | Kennungen von Haushaltskonten | Nein | AT&amp;T-spezifische Indikatoren für die Haushaltsleiter.<br><br>Typ-ID = Flag, das angibt, ob das Benutzerkonto ein primäres/sekundäres Konto ist<br><br>Primäre OID = Haushaltskennung. Wenn TypeID Primär ist, enthält den Wert der userID |
| Max. Bewertung | Die maximal zulässige Bewertung für das aktuelle Konto | Nein | Ermöglicht es Programmierern, Inhalte herauszufiltern, die für Konten nicht geeignet sind.<br><br> Hat MPAA- oder VCHIP-Bewertungen |
| Kanalaufteilung | Liste der im Paket des Benutzers verfügbaren Kanäle | Nein | Dient zum schnellen Ermöglichen/Entfernen verschiedener Kanäle aus Portalen, die mehrere Netzwerke aggregieren</br></br> *Bitte beachten Sie, dass die Preflight-Autorisierung in der Regel mehr Flexibilität für diesen Anwendungsfall ermöglicht und stattdessen verwendet werden sollte* <br><br>Die OLCA-Spezifikation ermöglicht dies als AttributStatement in der AuthN-Antwort |
| HBA-Status | Gibt an, ob die Authentifizierung über HBA erfolgt ist | Nein |     |
| Postleitzahl | Postleitzahl der Rechnungsstellung des Benutzers | Ja | Wird für Übertragungs- oder Sporting-Ereignisse verwendet<br><br>Kann auch mit der AuthZ-Antwort für schnelle Aktualisierungen bereitgestellt werden<br><br>Sensible Daten, erfordert MVPD-rechtliche Vereinbarungen |
| Verschlüsselte Postleitzahl | Postleitzahl der Rechnungsstellung des Benutzers (Comcast) | Ja | Wie oben, aber durch Comcast verschlüsselt |
| Sprache | Spracheinstellungen für Benutzer | Nein | Wird verwendet, um Nachrichten gemäß den Benutzereinstellungen anzuzeigen. |
| Mirroring zulassen | Gibt an, ob die Bildschirmspiegelung für dieses Gerät zulässig ist oder nicht | Nein |     |



## Verfügbare Metadaten {#available_metadata}

In der folgenden Tabelle wird der aktuelle Status der Benutzermetadaten im Ökosystem &quot;Adobe Pass-Authentifizierung&quot;beschrieben:


|     | **Legal **<br><br>**Agreement **<br><br>**Signed (nur zip)** | **Benutzer-ID **<br><br>**auf AuthN** | **Postleitzahl **<br><br>**auf AuthN/Z** | **Bewertung **<br><br>**auf AuthN/Z** | **Haushalts **<br><br>**ID auf AuthN/Z** | **Kanal-ID auf AuthN** | **Leiter des Haushalts auf AuthN** | **Type ID on AuthN** | **Primäre OID auf AuthN** | Sprache | Upstream UserID **auf AuthN** | HBA-Status | OnNet | inHome | Spiegelungen in AuthZ zulassen | **Notizen** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Formaler Name** | Nicht zutreffend | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | language | UpstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. Für AuthN: Der OiosamlMetadataParser muss so geändert werden, dass dieses neue Attribut für alle Parser <br>2 aktiviert ist.  Für AuthZ - Es gibt keine generische Methode, da die Autorisierungsimplementierung MVPD-spezifisch ist. |
| **Verschlüsselung erforderlich** | Nicht zutreffend | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| **vertraulich** | Nicht zutreffend | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |     |     |
| **Adobe IdP** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Ja** | **Ja** | **Ja** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Es ist keine rechtliche Vereinbarung erforderlich, kann aktiviert werden. |
| **synacor** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Rechtliche Vereinbarung, die nicht alle proximierten MVPDs abdeckt.**   <br>Dies ist allgemeine Unterstützung für Synacor - möglicherweise nicht für alle MVPDs. |
| Dish | **Nein** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Gibt dieselbe Liste wie alle Synacor-MVPDs und UpstreamUserID frei. |
| Comcast | **Nein** | **Ja** | **Nein** | **Ja (nur AuthZ)** | **Ja (nur AuthZ)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** |     |
| **AT&amp;T** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Unterzeichnete Rechtsvereinbarung. |
| **Cablevision** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Unterzeichnete Rechtsvereinbarung. |
| **HTC** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| **Proxy-Massilon** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Unterzeichnete Rechtsvereinbarung. |
| **Proxy Clearleap** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthZ)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Unterzeichnete Rechtsvereinbarung. |
| Rogers | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| RCN | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Charta | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Verizon | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** |     |
| Ostlink | **Nein** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Proxy GLDS | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| DTV | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| COX | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| Kogeco | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| videotron | **Nein** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja*** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Stellt die Haushaltkennung mit demselben Wert wie die userID bereit |
| Funkfrequenzen | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Ja** |     |
| **Alle anderen **<br><br>**MVPDs** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Noch keine gültige Vereinbarung, vertrauliche Metadaten sind nicht für die Produktion verfügbar.** <br>Für alle MVPDs ist die Benutzer-ID ohne zusätzliche Arbeit verfügbar. |


Die Liste der Benutzer-Metadatentypen wird erweitert, sobald neue Typen verfügbar gemacht und zum Adobe Pass-Authentifizierungssystem hinzugefügt werden.

## Codebeispiele {#code_samples}

- [Codebeispiel 1](#code_sample1)
- [Codebeispiel 2 (nachahmen von getMetadata)](#code_sample2)


### Codebeispiel 1 {#code_sample1}

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


### Codebeispiel 2 (nachahmen von getMetadata) {#code_sample2}

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
