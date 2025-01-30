---
title: Medien-Token
description: Medien-Token
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# Medien-Token {#media-tokens}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

Das Medien-Token ist ein Token, das von der Adobe Pass-Authentifizierung [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) als Ergebnis einer Autorisierungsentscheidung generiert wird, die darauf abzielt, Anzeigezugriff auf geschützte Inhalte (Ressourcen) zu gewähren. Das Medien-Token ist für einen begrenzten und kurzen Zeitraum (einige Minuten) gültig, der zum Zeitpunkt der Ausgabe angegeben ist. Es gibt die Zeit an, die es von der Client-Anwendung überprüft und verwendet werden muss.

Das Medien-Token besteht aus einer signierten Zeichenfolge, die auf der als Klartext gesendeten Public Key Infrastructure (PKI) basiert. Mit dem PKI-basierten Schutz wird das Token mit einem asymmetrischen Schlüssel signiert, der von einer Zertifizierungsstelle (Certification Authority, CA) zum Adobe ausgestellt wird.

Das Medien-Token wird an den Programmierer übergeben, der es dann mithilfe des Media Token Verifier validieren kann, bevor der Video-Stream gestartet wird, um die Zugriffssicherheit für diese Ressource sicherzustellen.

Der Media Token Verifier ist eine von der Adobe Pass-Authentifizierung verteilte Bibliothek, die für die Überprüfung der Authentizität eines Medien-Tokens zuständig ist.

## Medien-Token-Verifizierer {#media-token-verifier}

Die Adobe Pass-Authentifizierung empfiehlt, dass Programmierer das Medien-Token an einen Backend-Service senden, der die Media Token Verifier-Bibliothek integriert, um einen sicheren Zugriff sicherzustellen, bevor der Video-Stream gestartet wird. Die Time-to-Live (TTL) des Medien-Tokens wurde entwickelt, um potenzielle Probleme mit der Uhrensynchronisierung zwischen dem Token-generierenden Server und dem validierenden Server zu berücksichtigen.

Die Adobe Pass-Authentifizierung rät dringend davon ab, das Medien-Token zu analysieren und seine Daten direkt zu extrahieren, da das Token-Format nicht garantiert ist und sich in Zukunft ändern kann. Die Media Token Verifier-Bibliothek sollte das einzige Tool sein, das zum Analysieren des Token-Inhalts verwendet wird.

Die Media Token Verifier-Bibliothek kann über den folgenden Link heruntergeladen werden:

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

Die Media Token Verifier-Bibliothek erfordert JDK Version 1.5 oder höher und unterstützt die Verwendung eines bevorzugten JCE-Anbieters (Java Cryptography Extension) für den Signaturalgorithmus (`SHA256WithRSA`).

Die Media Token Verifier-Bibliothek, die durch das `mediatoken-verifier-VERSION.jar` Java-Archiv dargestellt wird, umfasst:

* Öffentlichen Adobe-Schlüssel.
* Token Verification API (`ITokenVerifier.java`).
* Referenzimplementierung (`com.adobe.entitlement.test.EntitlementVerifierTest.java`).
* Abhängigkeiten und Zertifikatschlüsselspeicher.

>[!IMPORTANT]
> 
> Das Standardkennwort für den enthaltenen Zertifikatschlüsselspeicher lautet `123456`.

### Methoden {#methods}

Die `ITokenVerifier`-Klasse definiert die folgenden Methoden:

* Die `isValid()` Methode, die zum Überprüfen des Medien-Tokens verwendet wird. Sie akzeptiert ein einziges Argument, die [Ressourcenkennung](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier). Wenn die angegebene Ressourcenkennung `null` ist, validiert die Methode nur die Authentizität und den Gültigkeitszeitraum des Medien-Tokens.

  Die `isValid()`-Methode gibt einen der folgenden Statuswerte zurück:

  | VALID_TOKEN | Token-Validierungen erfolgreich |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | Tokenformat ungültig |
  | INVALID_SIGNATURE | Token-Authentifizierung konnte nicht validiert werden |
  | TOKEN_EXPIRED | Token-TTL ist ungültig |
  | INVALID_RESOURCE_ID | Token für angegebene Ressource ungültig |
  | ERROR_UNKNOWN | Token wurde noch nicht validiert |

* Die `getResourceID()` Methode, die verwendet wird, um die Ressourcenkennung abzurufen, die mit dem Medien-Token verknüpft ist, und sie mit der Kennung zu vergleichen, die von der Autorisierungsentscheidungs-Antwort zurückgegeben wird.

* Die `getTimeIssued()` Methode, die zum Abrufen des Zeitpunkts verwendet wird, zu dem das Medien-Token ausgegeben wurde.

* Die zum Abrufen der TTL des Medien-Tokens verwendete `getTimeToLive()`-Methode.

* Die `getUserSessionGUID()` Methode zum Abrufen einer anonymisierten GUID, die von der MVPD festgelegt wird.

* Die `getMvpdId()` Methode, die zum Abrufen der Kennung der MVPD verwendet wird, die den Benutzer authentifiziert hat.

* Die `getProxyMvpdId()` Methode, die zum Abrufen der Kennung der Proxy-MVPD verwendet wird, die den Benutzer authentifiziert hat.

### Beispiel {#sample}

Das Archiv „Media Token Verifier“ enthält eine Referenzimplementierung (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) und ein Beispiel für den Aufruf der API mit der Testklasse. Dieses Beispiel (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) veranschaulicht die Integration der Media Token Verifier-Bibliothek in einen Media-Server.

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## REST-API V2 {#rest-api-v2}

Das Medien-Token kann mit der folgenden API abgerufen werden:

* [Abrufen von Autorisierungsentscheidungen mithilfe bestimmter MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Weitere Informationen zur Struktur von Autorisierungsentscheidungen und Medien **Token finden Sie in** Abschnitten zu „Antwort **und** Beispiele“ der obigen API.

Weitere Informationen zur Integration der oben genannten API sowie dazu, wann diese integriert werden sollte, finden Sie im folgenden Dokument:

* [Grundlegender Autorisierungsfluss innerhalb der primären Anwendung](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!IMPORTANT]
>
> Die Client-Anwendung muss zur Validierung den `serializedToken`-Wert aus der zurückgegebenen `token` an [Media Token Verifier](#media-token-verifier) übergeben.
