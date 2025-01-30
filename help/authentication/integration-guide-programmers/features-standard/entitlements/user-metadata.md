---
title: Benutzermetadaten
description: Benutzermetadaten
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 0%

---

# Benutzermetadaten {#user-metadata}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Benutzermetadaten beziehen sich auf benutzerspezifische [Attribute](#attributes) (z. B. Postleitzahlen, elterliche Bewertungen, Benutzer-IDs usw.), die von MVPDs verwaltet und Programmierern über die Adobe Pass-Authentifizierung (REST [ V2) bereitgestellt ](#apis).

Benutzermetadaten können verwendet werden, um die Personalisierung für Benutzende zu verbessern, können jedoch auch für Analysen verwendet werden. Beispielsweise kann ein Programmierer die Postleitzahl eines Benutzers verwenden, um lokalisierte Nachrichten oder Wetteraktualisierungen bereitzustellen oder elterliche Kontrollen durchzusetzen.

Die Adobe Pass-Authentifizierung normalisiert Benutzermetadatenwerte, wenn MVPDs Daten in verschiedenen Formaten bereitstellen. Für bestimmte Attribute (z. B. Postleitzahl) können die Werte [verschlüsselt) ](#encryption) das Zertifikat eines Programmierers verwendet werden.

Die Adobe Pass-Authentifizierung ermöglicht es Programmierern, die in ihren MVPD-Integrationen bereitgestellten Benutzermetadaten zu überprüfen und [verwalten](#management) über das [Adobe Pass TVE-Dashboard](https://experience.adobe.com/#/pass/authentication).

## Benutzer-Metadatenattribute {#attributes}

In der folgenden Tabelle sind einige der Benutzermetadatenattribute aufgeführt, die für Programmierer verfügbar gemacht werden:

| Schlüssel | Typ | Beispiel | Verschlüsselung erforderlich | Beschreibung | Details |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | Zeichenfolge | „1O7241P“ | Nein | Kontokennung | Der Attributwert kann eine Haushaltskennung oder eine Kennung eines Unterkontos sein. Der `userID` unterscheidet sich von der `householdID`, wenn MVPD Unterkonten unterstützt und der aktuelle Benutzer nicht der Hauptkontoinhaber ist. |
| `upstreamUserID` | Zeichenfolge | „1O7241P“ | Nein | Kontokennung für die Parallelitätsüberwachung. | Der Attributwert kann verwendet werden, um Parallelitätsbeschränkungen zwischen MVPD- und Programmierwebsites und -Apps zu erzwingen. Der `upstreamUserID` entspricht dem `userID` für die meisten MVPDs. |
| `householdID` | Zeichenfolge | „1O7241P“ | Nein | Kontokennung für die elterliche Kontrolle. | Der Attributwert kann verwendet werden, um zwischen der Nutzung von Haushalten und Unterkonten zu unterscheiden. Manchmal kann sie als Ersatz für die elterliche Kontrolle verwendet werden, wenn keine echten Bewertungen verfügbar sind, wenn der Benutzer mit dem Haushaltskonto angemeldet war, kann er ansehen, andernfalls würden bewertete Inhalte nicht angezeigt. Die Darstellung in MVPDs variiert stark (z. B. Haushaltsbenutzer-ID, Haushaltsleiter-ID, Haushaltsleiter-ID usw.). Wenn der MVPD keine Unterkonten unterstützt, ist die Darstellung mit der von `userID` identisch. |
| `primaryOID` | Zeichenfolge | „UUIDd1e19ec9-012c-124f-b520-acaf118d16a0“ | Nein | Kontokennung | Das Attribut ist spezifisch für AT&amp;T. Der `primaryOID` ist derselbe wie der `userID`, wenn der `typeID` auf &quot;Primär&quot; gesetzt ist. |
| `typeID` | Zeichenfolge | &quot;Primär&quot; | Nein | Attribut, das angibt, ob der aktuelle Benutzer Inhaber eines primären oder sekundären Kontos ist. | Das Attribut ist spezifisch für AT&amp;T. Der `primaryOID` ist derselbe wie der `userID`, wenn der `typeID` auf &quot;Primär&quot; gesetzt ist. |
| `is_hoh` | Zeichenfolge | „1“ | Nein | Attribut, das angibt, ob der aktuelle Benutzer Haushaltsvorstand ist oder nicht. | Das Attribut ist spezifisch für Synacor. |
| `hba_status` | Boolesch | „TRUE“ | Nein | Attribut, das angibt, ob der aktuelle Benutzer über HBA authentifiziert wurde oder nicht. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | Boolesch | „TRUE“ | Nein | Attribut, das angibt, ob das aktuelle Gerät den Bildschirm spiegeln kann oder nicht. | Das Attribut ist spezifisch für Spektrum. |
| `zip` | Array | \[„77754“, „12345“\] | Ja | Postleitzahl des Benutzers. | Der Attributwert kann verwendet werden, um lokalisierte Nachrichten, Wetteraktualisierungen oder Sportereignisse bereitzustellen. Der `zip` Wert stellt sensible Daten dar, für die rechtliche Vereinbarungen mit der MVPD getroffen werden müssen. Bei der Verschlüsselung wird die Darstellung des `zip` Schlüssels als `String` statt als `Array` dargestellt. |
| `encryptedZip` | Zeichenfolge | &quot;&quot; | Ja | Verschlüsselte Postleitzahl des Benutzers. | Das Attribut ist spezifisch für Comcast. |
| `channelID` | Array | \[„channel-1“, „channel-2“\] | Nein | Liste der Kanäle, die der Benutzer anzeigen darf. | Der Attributwert kann verwendet werden, um verschiedene Kanäle aus Portalen zu filtern, die mehrere Netzwerke aggregieren. Wir empfehlen, die [Preauthorize-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) anstelle dieses Benutzer-Metadatenattributs zu verwenden, um Kanäle auszufiltern, die dem Benutzer nicht zur Verfügung stehen. |
| `maxRating` | Objekt | { MPAA: „NR“, VCHIP: „X“, URL: &quot;http://manage.my/parental&quot; } | Nein | Maximale Kindersicherungsstufe für den aktuellen Benutzer. | Der Attributwert kann zum Filtern von Inhalten verwendet werden, die für aktuelle Benutzende auf der Grundlage von „MPAA“- oder „VCHIP“-Bewertungen nicht geeignet sind. |
| `language` | Zeichenfolge | „Englisch“ | Nein | Spracheinstellungen. | Der Attributwert kann verwendet werden, um Meldungen entsprechend den Spracheinstellungen des Benutzers anzuzeigen. |

Welche Benutzermetadatenattribute einem Programmierer zur Verfügung gestellt werden, hängt davon ab, was ein MVPD bereitstellt. In der folgenden Tabelle sind die Attribute aufgeführt, die von verschiedenen MVPDs zur Verfügung gestellt werden:

|                         | **Rechtsvereinbarung unterzeichnet (nur Zip)** | **Benutzer-ID bei AuthN** | **Upstream-Benutzer-ID bei AuthN** | **Haushalts-ID auf AuthN/Z** | **Primäre OID bei AuthN** | **Typ-ID in AuthN** | **Haushaltsvorstand auf AuthN** | **HBA-Status** | **Spiegelung bei AuthZ zulassen** | **Postleitzahl auf AuthN/Z** | **Kanal-ID bei AuthN** | **Bewertung für AuthN/Z** | **language** | **onNet** | **inHome** | **Hinweise** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **Formaler Name** | Nicht zutreffend | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **Erfordert Verschlüsselung** | Nicht zutreffend | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| **sensitiv** | Nicht zutreffend | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Adobe IdP | **Ja** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja** | **Ja** | **Ja** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | Rechtliche Vereinbarung nicht erforderlich. |
| Synacor | **Ja** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | Rechtsvereinbarung, die nicht alle MVPDs abdeckt. Dies ist allgemeine Unterstützung für Synacor und möglicherweise nicht für alle ihre MVPDs bereitgestellt. |
| Teller | **Nein** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | Es hat dieselbe Liste wie alle Synacor-MVPDs plus `upstreamUserID`. |
| komcast | **Nein** | **Ja** | **Ja** | **Ja (nur AuthZ)** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthZ)** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| AT&amp;T | **Ja** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | Rechtsvereinbarung unterzeichnet. |
| DTV | **Ja** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| COX | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Kabelfernsehen | **Ja** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | Rechtsvereinbarung unterzeichnet. |
| Spektrum | **Ja** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Chartervertrag | **Ja** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Verizon | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| HTC | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Rogers | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| RCN | **Ja** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Ost-Link | **Nein** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Cogeco | **Nein** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Videotron | **Nein** | **Ja** | **Ja** | **Ja*** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | Dadurch werden `householdID` mit dem gleichen Wert wie `userID` verfügbar gemacht. |
| Proxy-Massision | **Ja** | **Ja** | **Ja** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | Rechtsvereinbarung unterzeichnet. |
| Proxy-Leerungssprung | **Ja** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Ja (nur AuthZ)** | **Ja** | **Nein** | **Nein** | Rechtsvereinbarung unterzeichnet. |
| Proxy-GLDS | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Ja (nur AuthN)** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** |                                                                                                                                           |
| Andere MVPDs | **Nein** | **Ja** | **Ja** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | **Nein** | Noch keine rechtliche Vereinbarung, vertrauliche Metadaten nicht für die Produktion verfügbar. Für alle MVPDs ist die `userID` ohne zusätzliche Arbeit verfügbar. |

>[!IMPORTANT]
>
> Rechtliche Vereinbarungen müssen mit MVPDs unterzeichnet werden, bevor sensible Nutzermetadaten (z. B. Postleitzahl) zur Verfügung gestellt werden.

## Verschlüsselung von Benutzermetadaten {#encryption}

Zum Ver- und Entschlüsseln von Benutzermetadatenattributen muss der Programmierer ein Zertifikat (Paar aus öffentlichem/privatem Schlüssel) generieren und [ Zertifikat über das [Adobe Pass TVE Dashboard](#management) selbst konfigurieren](https://experience.adobe.com/#/pass/authentication) oder den öffentlichen Schlüssel mit Adobe Pass-Authentifizierungsbeauftragten teilen.

Gehen Sie wie folgt vor, um sicherzustellen, dass das Zertifikat korrekt generiert und konfiguriert ist:

1. Laden Sie das OpenSSL-Toolkit herunter und installieren Sie es (http://www.openssl.org).

1. Certificate Signing Request (CSR) generieren:

   * Erzeugen Sie ein Schlüsselpaar. Führen Sie auf dem Befehlsterminal Folgendes aus:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * CSR generieren. Führen Sie auf dem Befehlsterminal Folgendes aus:

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     Sie werden aufgefordert, das Kennwort für den privaten Schlüssel einzugeben.

   * Erstellen Sie eine Sicherungskopie Ihres privaten Schlüssels und Kennworts. Beispiel-CSR:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Senden Sie die CSR an eine Zertifizierungsstelle (CA) (z. B. Verisign).

1. Die Zertifizierungsstelle sendet Ihnen das Zertifikat im Format .p7b (PKCS#7, Cryptographic Message Syntax Standard).

1. Stellen Sie das .p7b-Zertifikat bereit. Konvertieren Sie die PKCS#7-Datei (.p7b) mithilfe Ihres privaten Schlüssels in eine PKCS#12-Datei (PFX-Datei, Personal Information Exchange Syntax Standard) und generieren Sie die PEM-Datei (verkettete Zertifikatcontainerdatei):

   * Konvertieren Sie die PKCS#7-Datei in eine temporäre PEM-Datei. Führen Sie in der Befehlszeile Folgendes aus:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Konvertieren Sie die temporäre PEM-Datei in eine PFX-Datei. Führen Sie in der Befehlszeile Folgendes aus:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Konvertieren Sie die temporäre PEM-Datei in eine endgültige PEM-Datei. Führen Sie in der Befehlszeile Folgendes aus:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Verwenden Sie die PEM-Datei[ um das ](#management) über das [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) zu konfigurieren oder senden Sie die PEM-Datei an die Adobe Pass-Authentifizierungsstellen.

   * Im nächsten Abschnitt finden Sie weitere Informationen zum Verwalten von Zertifikaten über das [Adobe Pass TVE-Dashboard](https://experience.adobe.com/#/pass/authentication).

   * Die Adobe Pass-Authentifizierung unterstützt sowohl ein Primär- als auch ein Sicherungszertifikat. Wenn Ihr primäres Zertifikat in irgendeiner Weise kompromittiert wird, können Sie es widerrufen und zum sekundären Zertifikat wechseln. Dadurch wird ein reibungsloser Übergang zwischen Zertifikaten mit minimalen Auswirkungen auf die Kunden gewährleistet.

## Verwalten von Benutzermetadaten {#management}

>[!IMPORTANT]
>
> Falls Sie keinen Zugriff auf das Adobe Pass TVE-Dashboard haben, erstellen Sie ein Ticket über unseren [Zendesk](https://adobeprimetime.zendesk.com) und bitten Sie Ihren Technical Account Manager (TAM), die entsprechenden Änderungen für Sie vorzunehmen.

Das Adobe Pass TVE-Dashboard ist ein Tool für Adobe Pass-Authentifizierungskunden (Programmierer) zum Verwalten ihrer Konfiguration und Daten. Dieses Self-Service-Dashboard ermöglicht eine Reihe von Funktionen, die in der Dokumentation [Benutzerhandbuch für das Adobe Pass TVE-Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) beschrieben werden.

Um die von einer MVPD zur Verfügung gestellten Benutzermetadatenattribute zu überprüfen und zu verwalten, führen Sie die Schritte in der Dokumentation [TVE Dashboard User Guide for Integrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata) aus.

Um die Zertifikate zu überprüfen und zu verwalten, die zum Verschlüsseln von Benutzermetadatenattributen verwendet werden, führen Sie die Schritte im [TVE Dashboard-Benutzerhandbuch für ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) oder im [TVE Dashboard-Benutzerhandbuch für Kanäle](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates) aus.

## REST-API V2 {#rest-api-v2}

Die Benutzermetadatenattribute können mit den folgenden APIs abgerufen werden:

* [Profile abrufen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Abrufen eines Profils für ein bestimmtes MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Abrufen eines Profils für einen bestimmten Code](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Weitere Informationen zur Struktur der Benutzermetadatenattribute finden Sie in **Abschnitten** Antwort **und Beispiele** der oben genannten APIs.

Weitere Informationen zur Integration der oben genannten APIs finden Sie in den folgenden Dokumenten:

* [Fluss von grundlegenden Profilen, der in der primären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluss der grundlegenden Profile, der in der sekundären Anwendung ausgeführt wird](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
