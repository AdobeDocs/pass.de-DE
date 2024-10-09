---
title: Benutzer-Metadatenzertifikat für Verschlüsselung
description: Benutzer-Metadatenzertifikat für Verschlüsselung
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a2
source-git-commit: 8c5203aa4a897a5e119a9886afc64a1b1556ee4f
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# Benutzer-Metadatenzertifikat für Verschlüsselung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Für die Integration von verschlüsselten Benutzermetadaten mit der Adobe Pass-Authentifizierung benötigen Sie ein privates/öffentliches Schlüsselpaar.

In diesem Dokument wird ein Prozess zum Generieren von Zertifikaten mit öffentlichem Schlüssel zur Verwendung in der Adobe Pass-Authentifizierung beschrieben. Der hier beschriebene Prozess nutzt das OpenSSL-Toolkit.

## Schrittweise Anleitung zum Prozess der Zertifikatgenerierung (#generation)

1. Laden Sie das OpenSSL-Toolkit herunter und installieren Sie es (http://www.openssl.org).

1. Generieren einer Certificate Signing Request (CSR):

   * Generieren Sie ein Schlüsselpaar.  Öffnen Sie ein Befehlsfenster/Terminal-Fenster und führen Sie den folgenden Befehl aus:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Generieren Sie die CSR. Führen Sie in der Befehlszeile Folgendes aus:

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

1. Senden Sie die CSR an eine Zertifizierungsstelle (z. B. Verisign).

1. Die Zertifizierungsstelle sendet Ihnen das Zertifikat im .p7b-Format (PKCS#7, Cryptographic Message Syntax Standard)

1. Stellen Sie das .p7b-Zertifikat bereit. Konvertieren Sie die Datei PKCS#7 (.p7b) mithilfe Ihres privaten Schlüssels in eine PKCS#12-Datei (PFX-Datei, Personal Information Exchange Syntax Standard) und generieren Sie die PEM-Datei (verkettete Zertifikatcontainer-Datei):

   * Konvertieren Sie die Datei PKCS#7 in eine temporäre PEM-Datei. Führen Sie in der Befehlszeile Folgendes aus:

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

1. Senden Sie die endgültige PEM-Datei an Adobe, damit sie konfiguriert werden kann.

   * Die Person, die letztendlich die PEM-Datei erhalten muss, ist der Adobe-Aktivierungstechniker, der Ihrer Integration/Validierung zugewiesen ist. Wenn Sie nicht direkt mit dieser Person arbeiten, können Sie von Ihrem Adobe-Support-Mitarbeiter erfahren, an wen Sie die Datei senden können.
   * Adobe unterstützt sowohl ein primäres als auch ein Backup-Zertifikat. Wenn Ihr primäres Zertifikat auf irgendeine Weise kompromittiert wird, können Sie es widerrufen und zum sekundären Zertifikat wechseln, um die Anforderungs-ID in Ihrer App zu signieren. Dadurch wird ein reibungsloser Übergang zwischen den Zertifikaten in der Produktion gewährleistet und die Auswirkungen auf die Kunden werden minimal sein.
   * Sobald Adobe die PEM-Datei erhält, fügen Authentifizierungstechniker sie Ihrer Konfiguration serverseitig hinzu und bestätigen den Empfang der Datei.
