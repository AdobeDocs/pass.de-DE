---
title: Benutzer-Metadatenzertifikat für Verschlüsselung
description: Benutzer-Metadatenzertifikat für Verschlüsselung
exl-id: 6f5d9a31-945e-418b-a9df-985bdbf29dff
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Benutzer-Metadatenzertifikat für Verschlüsselung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Für die Integration der Adobe Pass-Authentifizierung mit verschlüsselten Benutzermetadaten benötigen Sie ein Schlüsselpaar aus privatem/öffentlichem Schlüssel.

In diesem Dokument wird ein Prozess zum Generieren von Zertifikaten mit öffentlichem Schlüssel für die Verwendung in der Adobe Pass-Authentifizierung beschrieben. Der hier beschriebene Prozess nutzt das OpenSSL Toolkit.

## Anleitung zur Zertifikatgenerierung (#generation)

1. Laden Sie das OpenSSL-Toolkit herunter und installieren Sie es (http://www.openssl.org).

1. Certificate Signing Request (CSR) generieren:

   * Erzeugen Sie ein Schlüsselpaar.  Öffnen Sie ein Befehlsfenster / Terminal-Fenster und führen Sie den folgenden Befehl aus:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * CSR generieren. Führen Sie in der Befehlszeile Folgendes aus:

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

1. Die Zertifizierungsstelle sendet Ihnen das Zertifikat im .p7b-Format (PKCS#7, Cryptographic Message Syntax Standard)

1. Stellen Sie das .p7b-Zertifikat bereit. Konvertieren Sie die PKCS#7-Datei (.p7b) mithilfe Ihres privaten Schlüssels in eine PKCS#12-Datei (PFX-Datei, Personal Information Exchange Syntax Standard) und generieren Sie die PEM-Datei (verkettete Zertifikatcontainerdatei):

   * Konvertieren Sie die PKCS#7-Datei in eine temporäre PEM-Datei. Führen Sie in der Befehlszeile Folgendes aus:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Konvertieren Sie die temporäre PEM-Datei in eine PFX-Datei.  Führen Sie in der Befehlszeile Folgendes aus:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Konvertieren Sie die temporäre PEM-Datei in eine endgültige PEM-Datei. Führen Sie in der Befehlszeile Folgendes aus:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Senden Sie die endgültige PEM-Datei zur Konfiguration an Adobe.

   * Die Person, die die PEM-Datei schließlich erhalten muss, ist der Adobe-Aktivierungstechniker, der Ihrer Integration/Validierung zugewiesen ist. Wenn Sie nicht direkt mit dieser Person arbeiten, können Sie über Ihren Adobe-Support-Mitarbeiter herausfinden, an wen Sie die Datei senden sollen.
   * Adobe unterstützt sowohl ein Primärzertifikat als auch ein Sicherungszertifikat. Wenn Ihr Primärzertifikat in irgendeiner Weise kompromittiert wird, können Sie es widerrufen und zum sekundären Zertifikat wechseln, um die Anforderer-ID in Ihrer App zu signieren. Dies gewährleistet einen reibungslosen Übergang zwischen Zertifikaten in der Produktion bei minimaler Auswirkung auf den Kunden.
   * Sobald Adobe die PEM-Datei erhält, fügt der Authentifizierungs-Engineer sie Server-seitig zu Ihrer Konfiguration hinzu und bestätigt den Empfang der Datei.
