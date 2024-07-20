---
title: Standard-Metadatenattribute
description: Standard-Metadatenattribute
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# Standard-Metadatenattribute {#std-metadata-attributes}

Diese Seite soll eine umfassende Liste von Metadatenattributen bereitstellen, die der Dienst zur Überwachung der Parallelität verarbeiten kann und die als Grundlage für Richtlinien verwendet werden können, die implementiert werden können. Die standardmäßigen Metadatenattribute können wie folgt kategorisiert werden:

* Vom Design enthaltene Attribute (die bei jedem Sitzungsinitialisierungsaufruf gesendet werden, da sie im URL-Pfad erforderlich sind). Ohne diese Werte kann kein gültiger Aufruf durchgeführt werden.
* Metadatenattribute: Werte, die während des Sitzungsinitialisierungsaufrufs als Formulardaten übergeben werden müssen (sofern die Backend-Richtlinien ihre Werte erfordern).

## Für das Design erforderliche Attribute {#attr-req-by-design}

Die API zur Überwachung der Parallelität zwingt die Clients dazu, die folgenden Werte als Teil eines gültigen Initialisierungsaufrufs zu senden: [Aufrufe zur Sitzungsinitiierung](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Feldname | Beispielwert | Verwendungszweck | Erhalten von |
|-------------|---------------------------|--------------------|------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | Autorisierungs-Header | Zendesk-Ticket bei der Integration |
| mvpdName | Sample_MVPD | URI-Pfad | Adobe Pass-Authentifizierung vom config-Endpunkt, wenn der Benutzer den MVPD auswählt |
| accountId | 12345 | URI-Pfad | Adobe Pass-Authentifizierung UpstreamUserID-Metadaten nach der Benutzeranmeldung [User Metadata UpstreamUserID - Adobe Pass Authentication](/help/authentication/user-metadata-feature.md) |


## Metadatenattribute {#metadata-attr}

Die Felder in der folgenden Tabelle können von Programmierern und MVPDs verwendet werden, um Richtlinien zu erstellen, die in der Überwachung der Parallelität implementiert werden.

Wenn bei [API v2.0](http://docs.adobeptime.io/cm-api-v2/) eines dieser Attribute von den definierten Richtlinien benötigt wird, führt ein Sitzungsinit-Versuch ohne dieses Attribut zu einer 400-fehlerhaften Anfrage.


| Entität | Attributname | Datentyp | Beschreibung | Externe Referenz (z. B.: EIDR, OATC) | Beispielwert | Validierungsregeln |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Medienunternehmen | programmerName | Zeichenfolge | Der Name des Programmierers |                                                   | ProgrammerX |                                                                                   |
| Ressource | channel | Zeichenfolge | Der Fernsehkanal |                                                   | ChannelY |                                                                                   |
|                 | assetId | Zeichenfolge | Der &quot;benutzerfreundliche&quot;oder für Verbraucher lesbare Titel, der für diesen Inhalt angezeigt werden soll | [EIDR 2.0-Datenfelderreferenz](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | type | enumeration | Ein Wert, der den allgemeinen Inhaltstyp beschreibt, der durch das TveItem dargestellt wird. Zu den aufgezählten Werten gehören: movie broadcastEpisode nonBroadcastEpisode musicVideo awardsShow Clip concert Conference newsEvent sportingEvent trailer | [Empfohlene Vorgehensweise für OATC-Metadaten-Feed ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | Das Feld muss einem der Elemente in der Auflistung entsprechen |
|                 | contentType | Zeichenfolge | Dieses Feld bestimmt, ob der angeforderte Inhalt live oder VOD ist. | Nicht zutreffend | live, vod | live oder vod |
|                 | Genre | Zeichenfolge | Das Genre des gestreamten Inhalts. Beschreibt den allgemeinen Programmierungstyp | [Empfohlener OATC-Metadaten-Feed ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} Praktik | Comedy | Gültiger Genre-Typ |
|                 | duration | number | Die Dauer des Medienelements in Sekunden | [Empfohlene Vorgehensweise für OATC-Metadaten-Feed ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | Zahlenfolge |
| Gerät/Browser | deviceId | Zeichenfolge | Die eindeutige Gerätekennung. | [Eigenschaften von Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | Zeichenfolge | Der Anzeigename dieses Geräts. |                                                   | Joe&#39;s iPad |                                                                                   |
|                 | marketingName | Zeichenfolge | Der Marketing-Name (oder der benutzerfreundliche Name) für ein Gerät | [Eigenschaften von Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | gültiger Marketingname |
|                 | mobileDevice | boolean | &quot;True&quot;, wenn das Gerät für die Verwendung in Bewegung vorgesehen ist | [Eigenschaften von Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | Zeichenfolge | Der Modellname des Geräts, Browsers oder einer anderen Komponente | [Eigenschaften von Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | Tablet, Telefon, Xbox. Set-Top-Box | gültiger Gerätemodellname |
|                 | osName | Zeichenfolge | Das Betriebssystem, unter dem das Gerät ausgeführt wird | [Device Atlas - Vordefinierte Betriebssystemeigenschaftswerte](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Weitere Hinweise: Sie müssen mit einem Benutzernamen und Kennwort in Device Atlas angemeldet sein, um die Eigenschaftswerte anzuzeigen | der erwartete Wert einer der Werte in den vordefinierten Eigenschaften des Device Atlas ist |
|                 | browserName | Zeichenfolge | Der Name oder Typ des Browsers auf dem Gerät | [Device Atlas - Browservordefinierte Eigenschaftswerte](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | Der Name oder Typ des Browsers auf dem Gerät.  Hinweis: Sie müssen mit einem Benutzernamen und Kennwort in Device Atlas angemeldet sein, um die Eigenschaftswerte anzuzeigen | der erwartete Wert einer der Werte in den vordefinierten Eigenschaften des Device Atlas ist |
|                 | browserVersion | Zeichenfolge | Die Browserversion auf dem Gerät | [Eigenschaften von Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | Die Browserversion auf dem Gerät |                                                                                   |
| Anwendung | applicationName | Zeichenfolge | Der &quot;benutzerfreundliche&quot;oder für Verbraucher lesbare Name der Anwendung | Nicht zutreffend | Sample_Application |                                                                                   |
|                 | applicationId | Zeichenfolge | Die Anwendungs-ID, die eine Clientanwendung eindeutig identifiziert. | Nicht zutreffend | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | Zeichenfolge | Die native Plattform der Anwendung | Nicht zutreffend | iOS, Android |                                                                                   |
|                 | applicationVersion | Zeichenfolge | Dieser Wert kann für Analysezwecke verwendet werden. | Nicht zutreffend | 1.0, 2.0 |                                                                                   |
| Betreff | accountId | Zeichenfolge | Die Konto-ID des Betreffs &quot;Überwachung der Parallelität&quot;(im Rahmen des MVPD) | Nicht zutreffend | Testkonto |                                                                                   |
|                 | contractType | Zeichenfolge | Grundprämie. Die Kunden können dies als benutzerdefinierte Metadaten hinzufügen und in ihren eigenen Bereichen verwenden | Nicht zutreffend | Grundprämie |                                                                                   |
| Benutzer | name | Zeichenfolge | Einige MVPDs bieten Informationen zu dem spezifischen Benutzer, der Inhalte wiedergibt. | Nicht zutreffend |                                                                                                                                                         |                                                                                   |
|                 | hba | boolean | Identifiziert, ob der Benutzer versucht, den Stream von seinem Stammspeicherort aus zu starten | Nicht zutreffend | true, false | true oder false |
| Standort | Kontinent | Zeichenfolge | Der Kontinent, von dem die deviceID, die die Wiedergabeanforderung sendet, stammt | Nicht zutreffend | Nordamerika | gültiger KontinNamensname |
|                 | country | Zeichenfolge | Das Land, aus dem die deviceID, die die Wiedergabeanforderung sendet, stammt | Nicht zutreffend | USA | gültiger Ländername |
|                 | state | Zeichenfolge | Der Status, aus dem die deviceID, die die Wiedergabeanforderung sendet, stammt | Nicht zutreffend | CA | gültiger Statusname |
|                 | city | Zeichenfolge | Der Ort, von dem die deviceID, die die Wiedergabeanforderung sendet, stammt | Nicht zutreffend | Cupertino | gültiger Stadtname |
|                 | Postleitzahl | number | Der Postleitzahl, aus dem die deviceID, die die Wiedergabeanforderung sendet, stammt | Nicht zutreffend | 95014 | Gültige Postleitzahl |
| Stream | streamId | Zeichenfolge | Wird vom CM-Dienst und nicht von der Kundenkontrolle generiert. Wird implizit verwendet, wenn Regeln des Typs &quot;maxstreams&quot;definiert sind. | Nicht zutreffend | Nicht zutreffend | Nicht zutreffend |
|                 | streamCDN | Zeichenfolge | gibt das CDN an, aus dem der Stream abgerufen wurde | Nicht zutreffend | Nicht zutreffend | Nicht zutreffend |

## Beispiele für die Verwendung von Metadatenattributen zum Erstellen von Richtlinien {#examples-metadata-attr}

Die Standard-Metadatenfelder können zum Definieren von serverseitigen Richtlinien basierend auf ihren Feldwerten verwendet werden:

* Sie können eine Richtlinie so konfigurieren, dass sie nur auf bestimmte Feldwerte angewendet wird (z. B. eine dedizierte iOS-Richtlinie: `osType` ist `iOS`)
* Sie können die Anzahl unterschiedlicher Werte für ein bestimmtes Feld begrenzen. Einige Beispiele sind:
   * maximal X verschiedene Geräte: `HAVING DISTINCT COUNT(deviceId) <= 2`
   * maximal X unterschiedliche Postleitzahlen: `HAVING DISTINCT COUNT(zipcode) <= 3`
* Sie können die Anzahl der aktiven Streams pro Feldwert begrenzen. Einige Beispiele sind:
   * maximal X aktive Streams für einen einzelnen Gerätetyp: `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * maximal X aktive Streams für Streams mit Live-Inhalt: `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Wenden Sie sich an das Team für die Überwachung der Parallelität, indem Sie [ein Ticket in Zendesk erstellen](mailto:tve-support@adobe.com) und angeben, welche Richtlinien Sie implementieren möchten.

Weitere Beispiele für Richtlinien und Integrations-Cookbooks finden Sie im Folgenden:

* [Policy Decision Point](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [API-Konsole - Überwachung der Adobe-Parallelität](http://docs.adobeptime.io/cm-api-v2/)
