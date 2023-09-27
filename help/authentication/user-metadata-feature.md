---
title: Funktion für Benutzermetadaten
description: Funktion für Benutzermetadaten
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1649'
ht-degree: 0%

---

# Benutzermetadaten {#user-metadata}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

</br>
</br>

## Einführung {#intro}

Mit der Funktion &quot;Benutzermetadaten&quot;können Programmierer auf verschiedene Arten benutzerspezifischer Daten zugreifen, die von MVPDs gepflegt werden.  Zu den Metadatentypen für Benutzer gehören Postleitzahlen, elterliche Bewertungen, Benutzer-IDs und mehr.  *Benutzer* Metadaten sind eine Erweiterung der zuvor verfügbaren *statisch* Metadaten (TTL des Authentifizierungstokens, TTL des Autorisierungstokens und Geräte-ID).


Wichtige Punkte zu Benutzermetadaten:

- Wird während der Authentifizierungs- und Autorisierungsflüsse an die Anwendung des Programmierers übergeben
- Werte werden im Token gespeichert
- Werte können normalisiert werden, wenn verschiedene MVPDs Daten in verschiedenen Formaten bereitstellen
- Einige Parameter können mit dem Schlüssel des Programmierers verschlüsselt werden (z. B. Postleitzahl)
- Spezifische Werte werden durch Adobe über eine Konfigurationsänderung bereitgestellt

## Abrufen von Benutzermetadaten {#obtaining}

Benutzermetadaten stehen Programmierern über AccessEnabler zur Verfügung `getMetadata()` und über die `/usermetadata` -Endpunkt in der clientless-API.  Weitere Informationen zur Verwendung von finden Sie in der Dokumentation zur Plattform-API . `getMetadata()` und den zugehörigen Callback `setMetadataStatus()` (oder für die Endpunkte und Parameter, die in der clientless-API verwendet werden).

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
| `householdID` | JSON-Zeichenfolge | &quot;1o7241p&quot; | Haushaltskennung. Wenn der MVPD keine Unterkonten unterstützt, ist dies mit dem Szenario `userID` |
| `maxRating` | JSON-Objekt | { MPAA: &quot;NR&quot;, <br>  VCHIP: &quot;X&quot;,  <br>  URL: &quot;http://manage.my/parental&quot; } | Maximale elterliche Bewertung für den Benutzer |
| `userID` | JSON-Zeichenfolge | &quot;1o7241p&quot; | Die Benutzer-ID. Wenn ein MVPD Subkonten unterstützt und der Benutzer nicht das Hauptkonto ist, `userID` unterscheidet sich `householdID`. |
| `channelID` | JSON-Array | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | Eine Liste der Kanäle, die ein Benutzer anzeigen darf |
| `is_hoh` | JSON-Zeichenfolge | &quot;1&quot; | Eine Markierung, die angibt, ob ein Benutzer Haushaltsleiter ist |
| `encryptedZip` | JSON-Zeichenfolge | &quot;&quot; | Comcast legt die verschlüsselte Postleitzahl offen |
| `typeID` | JSON-Zeichenfolge | Primär | Eine Markierung, die angibt, ob das Benutzerkonto ein primäres/sekundäres Konto ist |
| `primaryOID` | JSON-Zeichenfolge | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Haushaltskennung. Wenn `typeID` ist Primär, enthält den Wert der `userID` |
| hba_status | Boolesch | &quot;true&quot; &quot;false&quot; | Eine boolesche Kennzeichnung, die angibt, ob die Home-basierte Authentifizierung für eine bestimmte Integration aktiviert ist |
| allowMirroring | Boolesch | &quot;true&quot; &quot;false&quot; | Gibt an, ob die Bildschirmspiegelung für dieses Gerät zulässig ist oder nicht |

>[!NOTE]
>
> **Hinweis:** Wenn der Datenparameter verschlüsselt ist, ist dies für die **ZIP-Schlüssel**, ist die Darstellung des Metadatenschlüssels eine JSON-Zeichenfolge anstelle eines Arrays oder Objekts.


**Wichtig:** Die tatsächlichen Benutzermetadaten, die einem Programmierer zur Verfügung stehen, hängen davon ab, was ein MVPD zur Verfügung stellt.  Rechtliche Vereinbarungen müssen mit MVPDs unterzeichnet werden, bevor sensible Benutzermetadaten (wie Postleitzahl) in der Produktionsumgebung verfügbar gemacht werden.

</br>


| Name | Details | Erfordert Verschlüsselung | Kommentare |
| --- | --- | --- | --- |
| Benutzer-ID | Wie vom MVPD bereitgestellt | Nein | Dies ist der Wert, der dann von Adobe gehasht und im Medien-Token und im Rückruf sendTrackingData() verfügbar gemacht wird.<br><br>Das Hashing in diesem Fall erfolgte aus historischen Gründen<br><br>Diese ID kann eine Haushalts-ID oder eine Unterkonto-ID sein. Dies ist normalerweise nicht angegeben, sondern nur die Kennung, die an die damals verwendete Anmeldung gebunden war (bei der es sich um ein primäres Konto oder ein Unterkonto handeln kann). |
| Upstream-Benutzer-ID | Wird vom MVPD bereitgestellt und darf nur für die Überwachung der Parallelität verwendet werden | Nein | Dieser Wert wird verwendet, wenn Parallelitätsbeschränkungen über MVPD- und Programmierer-Sites und -Apps hinweg erzwungen werden. <br><br>Die ID kann auch Überwachungsrichtlinien für gleichzeitige Sitzungen enthalten.<br><br>Bei den meisten MVPDs entspricht dieser Wert der Benutzer-ID |
| Haushalts-Benutzer-ID | Wird vom MVPD bereitgestellt, um hauptsächlich für die elterlichen Kontrollflüsse verwendet zu werden | Nein | ID, die es Programmierern ermöglicht, die Nutzung von Haushalts- und Unterkonten zu verstehen.<br><br>Es wird manchmal als Ersatz für die elterliche Steuerung verwendet, wenn keine echten Bewertungen verfügbar sind (wenn der Benutzer mit dem Haushaltskonto angemeldet war, das er ansehen kann, würde andernfalls kein bewerteter Inhalt angezeigt werden)<br><br>Es gibt viele Unterschiede zwischen den MVPDs, wie dies dargestellt wird - Haushalts-Benutzer-ID, Leiter der Haushalts-ID, Leiter der Haushaltsflagge usw. |
| Leiter der Haushaltsabteilung | Kennzeichnungsanzeige, wenn das Leistungskonto Haushaltsleiter ist oder nicht | Nein | siehe oben |
| Typ-ID / Primäre ID | Kennungen von Haushaltskonten | Nein | AT&amp;T-spezifische Indikatoren für die Haushaltsleiter.<br><br>Typ-ID = Flag, das angibt, ob das Benutzerkonto ein primäres/sekundäres Konto ist<br><br>Primäre OID = Haushaltskennung. Wenn TypeID Primär ist, enthält den Wert der userID |
| Max. Bewertung | Die maximal zulässige Bewertung für das aktuelle Konto | Nein | Ermöglicht es Programmierern, Inhalte herauszufiltern, die für Konten nicht geeignet sind.<br><br>Hat MPA- oder VCHIP-Bewertungen |
| Kanalaufteilung | Liste der im Paket des Benutzers verfügbaren Kanäle | Nein | Dient zum schnellen Ermöglichen/Entfernen verschiedener Kanäle aus Portalen, die mehrere Netzwerke aggregieren</br></br> *Bitte beachten Sie, dass die Vorabgenehmigung im Allgemeinen mehr Flexibilität für diesen Anwendungsfall ermöglicht und stattdessen verwendet werden sollte.* <br><br>Die OLCA-Spezifikation ermöglicht dies als AttributeStatement in der AuthN-Antwort |
| HBA-Status | Gibt an, ob die Authentifizierung über HBA erfolgt ist | Nein |     |
| Postleitzahl | Postleitzahl der Rechnungsstellung des Benutzers | Ja | Wird für Übertragungs- oder Sendeereignisse verwendet<br><br>Kann auch mit der AuthZ-Antwort für schnelle Updates bereitgestellt werden<br><br>Sensible Daten, MVPD-Rechtsvereinbarungen erforderlich |
| Verschlüsselte Postleitzahl | Postleitzahl der Rechnungsstellung des Benutzers (Comcast) | Ja | Wie oben, aber durch Comcast verschlüsselt |
| Sprache | Spracheinstellungen für Benutzer | Nein | Wird verwendet, um Nachrichten gemäß den Benutzereinstellungen anzuzeigen. |
| Mirroring zulassen | Gibt an, ob die Bildschirmspiegelung für dieses Gerät zulässig ist oder nicht | Nein |     |



## Verfügbare Metadaten {#available_metadata}

In der folgenden Tabelle wird der aktuelle Status der Benutzermetadaten im Ökosystem &quot;Adobe Pass-Authentifizierung&quot;beschrieben:


|     | **Legal **<br><br>**Abkommen **<br><br>**Unterschrieben (nur zip)** | **Benutzer-ID **<br><br>**on AuthN** | **Postleitzahl **<br><br>**auf AuthN/Z** | **Bewertung **<br><br>**auf AuthN/Z** | **Haushalt **<br><br>**ID auf AuthN/Z** | **Kanal-ID auf AuthN** | **Leiter des Hauses in AuthN** | **Typ-ID auf AuthN** | **Primäre OID auf AuthN** | Sprache | Upstream UserID **on AuthN** | HBA-Status | OnNet | inHome | Spiegelungen in AuthZ zulassen | **Hinweise** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Name des Formulars** | Nicht zutreffend | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | language | UpstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. Für AuthN: Der OiosamlMetadataParser muss so geändert werden, dass dieses neue Attribut für alle Parser aktiviert ist. <br>2.  Für AuthZ - Es gibt keine generische Methode, da die Autorisierungsimplementierung MVPD-spezifisch ist. |
| **Verschlüsselung erforderlich** | Nicht zutreffend | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| **Sensitiv** | Nicht zutreffend | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |     |     |
| **Adobe IdP** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Ja** | **Ja** | **Ja** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Es ist keine rechtliche Vereinbarung erforderlich, kann aktiviert werden. |
| **Synacor** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Rechtliche Vereinbarung, die nicht alle proximierten MVPDs abdeckt.**   <br>Dies ist allgemeine Unterstützung für Synacor - möglicherweise nicht für alle MVPDs. |
| Dish | **Nein** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja (nur AuthN)** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Gibt dieselbe Liste wie alle Synacor-MVPDs und UpstreamUserID frei. |
| Comcast | **Nein** | **Ja** | **Nein** | **Ja (nur AuthZ)** | **Ja (nur AuthZ)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** |     |
| **AT&amp;T** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Unterzeichnete Rechtsvereinbarung. |
| **Seilbahn** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Unterzeichnete Rechtsvereinbarung. |
| **HTC** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |     |
| **Proxy Massilon** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Unterzeichnete Rechtsvereinbarung. |
| **Proxy-Zeilensprung** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja (nur AuthZ)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Unterzeichnete Rechtsvereinbarung. |
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
| **Alle anderen **<br><br>**MVPDs** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Es gibt noch keine gültige Vereinbarung, sodass vertrauliche Metadaten nicht für die Produktion verfügbar sind.**  <br>Für alle MVPDs ist die Benutzer-ID ohne zusätzliche Arbeit verfügbar. |


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
