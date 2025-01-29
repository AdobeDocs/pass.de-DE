---
title: Standard-Metadatenattribute
description: Standard-Metadatenattribute
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# Standard-Metadatenattribute {#std-metadata-attributes}

Auf dieser Seite erhalten Sie eine umfassende Liste von Metadatenattributen, die der Concurrency Monitoring-Service verarbeiten kann und die als Grundlage für Richtlinien verwendet werden können, die implementiert werden können. Die Standard-Metadatenattribute können wie folgt kategorisiert werden:

* Vom Design eingeschlossene Attribute (werden bei jedem Sitzungsinitialisierungsaufruf gesendet, da sie im URL-Pfad erforderlich sind). Ohne diese Werte kann kein gültiger Aufruf ausgeführt werden.
* Metadatenattribute: Werte, die beim Sitzungsinitialisierungsaufruf als Formulardaten übergeben werden müssen (falls die Backend-Richtlinien ihre Werte erfordern).

## Vom Design benötigte Attribute {#attr-req-by-design}

Die Parallelitätsüberwachungs-API zwingt Clients, im Rahmen eines gültigen Initialisierungsaufrufs die folgenden Werte zu senden: [Sitzungsinitiierungsaufrufe](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Feldname | Beispielwert | Verwendungsbereiche | Erhalten von |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | Autorisierungs-Header | Zendesk-Ticket bei der Integration |
| mvpdName | sample_MVPD | URI-Pfad | Adobe Pass-Authentifizierung über den config-Endpunkt, wenn der Benutzer die MVPD auswählt |
| accountId | 12345 | URI-Pfad | Adobe Pass-Authentifizierung upstreamUserID-Metadaten nach Benutzeranmeldung [UserMetadata upstreamUserID - Adobe Pass-Authentifizierung](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## Metadatenattribute {#metadata-attr}

Die Felder in der folgenden Tabelle können von Programmierern und MVPDs verwendet werden, um Richtlinien zu erstellen, die bei der gleichzeitigen Überwachung implementiert werden.

Wenn bei [API v2.](http://docs.adobeptime.io/cm-api-v2/) eines dieser Attribute für die definierten Richtlinien erforderlich ist, führt ein Sitzungsinittierungsversuch ohne dieses Attribut zu einer 400-Fehler-Anfrage.


| Entität | Attributname | Datentyp | Beschreibung | Externe Referenz (z. B. EIDR, OATC) | Beispielwert | Validierungsregeln |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Medienunternehmen | ProgrammerName | Zeichenfolge | Der Name des Programmierers |                                                   | ProgrammerX |                                                                                   |
| Ressource | Kanal | Zeichenfolge | Der TV-Kanal |                                                   | Kanal Y |                                                                                   |
|                 | assetId | Zeichenfolge | Der „benutzerfreundliche“ oder für Verbraucher lesbare Titel, der für diesen Inhalt angezeigt werden soll | [EIDR 2.0-Datenfeld-Referenz](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | Typ | Auflistung | Ein -Wert, der den allgemeinen Inhaltstyp beschreibt, der durch das TV-Element dargestellt wird. Die Aufzählungswerte enthalten: Film broadcastEpisode nonBroadcastEpisode musicVideo AwardsShow Clip Konzert Konferenz newsEvent sportingEvent Trailer | [Empfohlene Vorgehensweise für OATC-Metadaten-Feed](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | Das Feld muss einem der Elemente in der Auflistung entsprechen |
|                 | contentType | Zeichenfolge | Dieses Feld bestimmt, ob der angeforderte Inhalt live oder VOD ist | Nicht zutreffend | Live, VOD | Live oder VOD |
|                 | Genre | Zeichenfolge | Das Genre des gestreamten Inhalts. Beschreibt den allgemeinen Programmiertyp | [OATC-Metadaten-Feed empfohlen](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} Übung | Komödie | Gültiger Genretyp |
|                 | Dauer | Zahl | Die Dauer des Medienelements in Sekunden | [Empfohlene Vorgehensweise für OATC-Metadaten-Feed](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | Zahlenfolge |
| Gerät/Browser | deviceId | Zeichenfolge | Die eindeutige Gerätekennung. | [Geräte-Atlas-Eigenschaften](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | Zeichenfolge | Der Anzeigename dieses Geräts. |                                                   | Joe&#39;s iPad |                                                                                   |
|                 | marketingName | Zeichenfolge | Der Marketing-Name (oder der benutzerfreundliche Name) für ein Gerät | [Geräte-Atlas-Eigenschaften](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | Gültiger Marketing-Name |
|                 | mobileDevice | Boolesch | True, wenn das Gerät für die Verwendung unterwegs vorgesehen ist | [Geräte-Atlas-Eigenschaften](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | Zeichenfolge | Der Modellname des Geräts, Browsers oder einer anderen Komponente | [Geräte-Atlas-Eigenschaften](https://deviceatlas.com/device-data/properties){target=_blank} | Tablet, Smartphone, Xbox. Set-Top-Box | Gültiger Gerätemodellname |
|                 | osName | Zeichenfolge | Das Betriebssystem, auf dem das Gerät ausgeführt wird | [Geräteatlas - Vordefinierte Eigenschaftswerte des Betriebssystems](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, anderer Hinweis: Sie müssen mit einem Benutzernamen und Kennwort in Device Atlas angemeldet sein, um die Eigenschaftswerte anzeigen zu können | Der erwartete Wert ist einer der Werte in den vordefinierten Eigenschaften von Device Atlas . |
|                 | browserName | Zeichenfolge | Der Name oder Typ des Browsers auf dem Gerät | [Geräteatlas - Vordefinierte Eigenschaftswerte des Browsers](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | Der Name oder Typ des Browsers auf dem Gerät.  Hinweis: Sie müssen im Geräteatlas mit einem Benutzernamen und Kennwort angemeldet sein, um die Eigenschaftswerte anzeigen zu können | Der erwartete Wert ist einer der Werte in den vordefinierten Eigenschaften von Device Atlas . |
|                 | browserVersion | Zeichenfolge | Die Browser-Version auf dem Gerät | [Geräte-Atlas-Eigenschaften](https://deviceatlas.com/device-data/properties){target=_blank} | Die Browser-Version auf dem Gerät |                                                                                   |
| Anwendung | applicationName | Zeichenfolge | Der „benutzerfreundliche“ oder von Verbrauchern lesbare Name der Anwendung | Nicht zutreffend | sample_application |                                                                                   |
|                 | applicationId | Zeichenfolge | Die Anwendungs-ID, die eine Client-Anwendung eindeutig identifiziert. | Nicht zutreffend | DE305D54-75B4-431B-ADB2-EB6B9E546013 |                                                                                   |
|                 | applicationPlatform | Zeichenfolge | Die native Plattform des Programms | Nicht zutreffend | iOS, Android |                                                                                   |
|                 | applicationVersion | Zeichenfolge | Dieser Wert kann zu Analysezwecken verwendet werden | Nicht zutreffend | 1.0, 2.0 |                                                                                   |
| Subjekt | accountId | Zeichenfolge | Die Konto-ID des Betreffs der Parallelitätsüberwachung (im Umfang der MVPD) | Nicht zutreffend | Testkonto |                                                                                   |
|                 | ContractType | Zeichenfolge | Premium, Basic. Die Kunden können dies als benutzerdefinierte Metadaten hinzufügen und in ihren eigenen Realms verwenden | Nicht zutreffend | Premium, Standard |                                                                                   |
| Benutzer | name | Zeichenfolge | Einige MVPDs stellen Informationen zu dem spezifischen Benutzer bereit, der Inhalte abspielt. | Nicht zutreffend |                                                                                                                                                         |                                                                                   |
|                 | HBA | Boolesch | Gibt an, ob der Benutzer versucht, den Stream von seinem Startspeicherort aus zu initiieren | Nicht zutreffend | true, false | Wahr oder falsch |
| Speicherort | Kontinent | Zeichenfolge | Der Kontinent, von dem die Geräte-ID, die die Wiedergabeanfrage sendet, stammt | Nicht zutreffend | Nordamerika | Gültiger Kontinentenname |
|                 | Land | Zeichenfolge | Das Land, aus dem die Geräte-ID, die die Wiedergabeanfrage sendet, stammt | Nicht zutreffend | USA | Gültiger Ländername |
|                 | Bundesland | Zeichenfolge | Der Status, aus dem die Geräte-ID, die die Wiedergabeanfrage sendet, stammt | Nicht zutreffend | CA | Gültiger Statusname |
|                 | Stadt | Zeichenfolge | Die Stadt, aus der die Geräte-ID stammt, die die Wiedergabeanfrage sendet | Nicht zutreffend | Cupertino | Gültiger Stadtname |
|                 | Postleitzahl | Zahl | Die Postleitzahl, von der die Geräte-ID stammt, die die Wiedergabeanfrage sendet | Nicht zutreffend | 95014 | gültige Postleitzahl |
| Stream | streamId | Zeichenfolge | Vom CM-Service generiert, nicht im Kundenzugriff. Wird implizit verwendet, wenn Regeln des Typs „maxStreams“ definiert sind. | Nicht zutreffend | Nicht zutreffend | Nicht zutreffend |
|                 | StreamCDN | Zeichenfolge | gibt das CDN an, von dem der Stream abgerufen wurde | Nicht zutreffend | Nicht zutreffend | Nicht zutreffend |

## Beispiele für die Verwendung von Metadatenattributen zum Erstellen von Richtlinien {#examples-metadata-attr}

Die Standard-Metadatenfelder können für die Definition von Server-seitigen Richtlinien basierend auf ihren Feldwerten verwendet werden:

* Sie können eine Richtlinie konfigurieren, die nur auf bestimmte Feldwerte angewendet wird (z. B. eine dedizierte iOS-Richtlinie: , bei der `osType` `iOS` ist)
* Sie können die Anzahl der unterschiedlichen Werte für ein bestimmtes Feld begrenzen. Einige Beispiele:
   * nicht mehr als X verschiedene Geräte: `HAVING DISTINCT COUNT(deviceId) <= 2`
   * höchstens X verschiedene Postleitzahlen: `HAVING DISTINCT COUNT(zipcode) <= 3`
* Sie können die Anzahl der aktiven Streams pro Feldwert begrenzen. Einige Beispiele:
   * nicht mehr als X aktive Streams für einen einzelnen Gerätetyp: `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * nicht mehr als X aktive Streams für Streams von Live-Inhalten: `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Wenden Sie sich an das Team zur Überwachung von [, indem Sie ein Ticket in Zendesk erstellen](mailto:tve-support@adobe.com) und geben Sie an, welche Richtlinien Sie implementieren möchten.

Weitere Beispiele für Richtlinien und Integrations-Cookbooks finden Sie im Folgenden:

* [Entscheidungspunkt der Richtlinie](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [API-Konsole - Adobe-Parallelitätsüberwachung](http://docs.adobeptime.io/cm-api-v2/)
