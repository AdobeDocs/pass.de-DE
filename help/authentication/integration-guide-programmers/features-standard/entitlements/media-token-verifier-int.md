---
title: Integrieren von Media Token Verifier
description: Integrieren von Media Token Verifier
exl-id: 1688889a-2e30-4d66-96ff-1ddf4b287f68
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '887'
ht-degree: 0%

---

# Integrieren von Media Token Verifier

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Über den Media Token Verifier {#about-media-token-verifier}

Wenn die Autorisierung erfolgreich ist, erstellt die Adobe Pass-Authentifizierung ein langlebiges Autorisierungs-Token (AuthZ).  Das AuthZ-Token wird je nach Client-Plattform entweder an die Client-Seite übergeben oder auf der Server-Seite gespeichert.  (Informationen [ Speichern von Token auf verschiedenen Clientsystemen ](/help/authentication/kickstart/programmer-overview.md#understanding-tokens) Sie unter „Grundlegendes zu Token“ zusammen mit anderen Details.)


Ein AuthZ-Token autorisiert den Benutzer der Site, eine bestimmte Ressource anzuzeigen.  Normalerweise beträgt die Time-to-Live (TTL) 6 bis 24 Stunden. Danach läuft das Token ab. **Für den tatsächlichen Anzeigezugriff verwendet die Adobe Pass-Authentifizierung das AuthZ-Token, um ein kurzlebiges Medien-Token zu generieren, das Sie abrufen und an Ihren Medienserver übergeben**. Diese kurzlebigen Medien-Token haben eine sehr kurze TTL (in der Regel einige Minuten).


Bei AccessEnabler-Integrationen erhalten Sie das kurzlebige Medien-Token über den `setToken()`-Callback. Bei Client-losen API-Integrationen erhalten Sie das kurzlebige Medien-Token mit dem `<SP_FQDN>/api/v1/tokens/media`-API-Aufruf. Das Token ist eine Zeichenfolge, die als Klartext gesendet und von Adobe signiert wird. Dabei wird der Token-Schutz basierend auf PKI (Public Key Infrastructure) verwendet. Bei diesem PKI-basierten Schutz wird das Token mit einem asymmetrischen Schlüssel signiert, der von einer Zertifizierungsstelle an Adobe ausgegeben wird.


Da das Token Client-seitig nicht validiert wird, kann ein böswilliger Benutzer Tools verwenden, um falsche `setToken()`-Aufrufe einzuschleusen. Daher **man sich bei** Entscheidung, ob ein Benutzer berechtigt ist oder nicht, nicht einfach auf die Tatsache verlassen, dass `setToken()` ausgelöst wurde. Sie müssen überprüfen, ob das kurzlebige Token selbst legitim ist. Das Tool zum Ausführen der Validierung ist die Media Token Verifier-Bibliothek.


>[!TIP]
>
>Sie müssen die gesamte Länge der zurückgegebenen Token-Zeichenfolge zur Validierung an den Media Token Verifier übergeben.

## Validieren von kurzlebigen Token mit dem Media Token Verifier {#validate-short-livedttokens}

Es wird empfohlen, dass Programmierer das Token an einen Webservice senden, der die Media Token Verifier-Bibliothek verwendet, um das Token zu validieren, bevor sie den Video-Stream tatsächlich starten. Die sehr kurze TTL der kurzlebigen Medien-Token ist so definiert, dass sie lang genug ist, um Taktsynchronisierungsprobleme zwischen dem Server, der das Token generiert, und dem Server, der das Token validiert, zuzulassen, jedoch nicht mehr.



Die [Media Token Verifier Library](https://adobeprimetime.zendesk.com/auth/v2/login/signin?return_to=https%3A%2F%2Ftve.zendesk.com%2Fhc%2Fen-us%2Farticles%2F204963159-Media-Token-Verifier-library&amp;theme=hc&amp;locale=en-us&amp;brand_id=343429&amp;auth_origin=343429%2Cfalse%2Ctrue){target=_blank} ist für Adobe Pass-Authentifizierungspartner verfügbar.



Die Media Token Verifier-Bibliothek ist im Java-`mediatoken-verifier-VERSION.jar` enthalten. Die Bibliothek definiert:

* Eine Token-Verifizierungs-API (`ITokenVerifier`) mit JavaDoc-Dokumentation
* Der öffentliche Adobe-Schlüssel, mit dem überprüft wird, ob das Token tatsächlich von Adobe stammt
* Eine Referenzimplementierung (`com.adobe.entitlement.test.EntitlementVerifierTest.java`), die zeigt, wie die Verifier-API verwendet wird und wie der in der Bibliothek enthaltene öffentliche Adobe-Schlüssel zur Überprüfung seiner Herkunft verwendet wird


Das Archiv enthält alle Abhängigkeiten und Zertifikatschlüsselspeicher. Das Standardkennwort für den enthaltenen Zertifikatschlüsselspeicher lautet „123456“.

* Für die Verifizierungsbibliothek ist JDK Version 1.5 oder höher erforderlich.
* Verwenden Sie Ihren bevorzugten JCE-Anbieter für den Signaturalgorithmus „SHA256WithRSA“.


**Die Verifier-Bibliothek muss die einzige Möglichkeit sein, den Token-Inhalt zu analysieren. Programmierer sollten das Token nicht parsen und die Daten selbst extrahieren, da das Token-Format nicht garantiert ist und sich in Zukunft ändern kann.** Nur die Verifier-API funktioniert garantiert ordnungsgemäß. Das direkte Parsen der Zeichenfolge kann vorübergehend funktionieren, verursacht jedoch in Zukunft Probleme, wenn sich das Format ändert. Die Verifier-API ruft Informationen vom Token ab, z. B.:

* Ist das Token gültig (die `isValid()` Methode)?
* Die mit dem Token verknüpfte Ressourcen-ID (die `getResourceID()`-Methode). Diese kann mit dem anderen Parameter des Callbacks der `setToken()`-Funktion verglichen werden (und sollte mit diesem übereinstimmen). Wenn sie nicht übereinstimmt, kann dies auf betrügerisches Verhalten hindeuten.
* Der Zeitpunkt, zu dem das Token ausgegeben wurde (`getTimeIssued()` Methode).
* Die TTL (`getTimeToLive()`).
* Eine von MVPD empfangene anonymisierte Authentifizierungs-GUID (`getUserSessionGUID()`).
* Die ID des Distributors, der den Benutzer authentifiziert hat, und falls ja, der Proxy-MVPD, der die Authentifizierung für den Distributor bereitgestellt hat.

## Verwenden der Verifier-API {#using-verifier-api}

Die `ITokenVerifier` definiert Methoden, mit denen Sie die Token-Authentizität für eine bestimmte Ressource überprüfen. Verwenden Sie die `ITokenVerifier` Methoden, um ein Token zu analysieren, das als Antwort auf eine `setToken()`-Anfrage empfangen wurde.


Die `isValid()` Methode ist das primäre Mittel zur Validierung eines Tokens. Dazu wird ein Argument, eine Ressourcen-ID, benötigt. Wenn Sie eine Null-Ressourcen-ID übergeben, validiert die Methode nur die Token-Authentizität und den Gültigkeitszeitraum.


Die `isValid()`-Methode gibt einen der folgenden Statuswerte zurück:



| VALID_TOKEN | Alle Validierungen erfolgreich |
|--------------------|-----------------------------------------|
| INVALID_TOKEN_FORMAT | Tokenformat ungültig |
| INVALID_SIGNATURE | Token-Authentifizierung konnte nicht validiert werden |
| TOKEN_EXPIRED | Token-TTL ist ungültig |
| INVALID_RESOURCE_ID | Token für angegebene Ressource ungültig |
| ERROR_UNKNOWN | Token wurde noch nicht validiert |

Zusätzliche Methoden bieten spezifischen Zugriff auf die Ressourcen-ID, die zugewiesene Zeit und die Time-to-Live für ein bestimmtes Token.

* Verwenden Sie `getResourceID()` , um die mit dem Token verknüpfte Ressourcen-ID abzurufen und sie mit der ID zu vergleichen, die von der setToken()-Anforderung zurückgegeben wird.
* Verwenden Sie `getTimeIssued()` , um die Zeit abzurufen, zu der das Token ausgegeben wurde.
* Verwenden Sie `getTimeToLive()`, um die TTL abzurufen.
* Verwenden Sie `getUserSessionGUID()` , um eine anonymisierte GUID abzurufen, die von der MVPD festgelegt wird.
* Verwenden Sie `getMvpdId()` , um die ID der MVPD abzurufen, die die Benutzerin oder den Benutzer authentifiziert hat.
* Verwenden Sie `getProxyMvpdId()` , um die ID der Proxy-MVPD abzurufen, die die Benutzerin oder den Benutzer authentifiziert hat.

## Beispielcode {#sample-code}

Das Archiv „Media Token Verifier“ enthält eine Referenzimplementierung (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) und ein Beispiel für den Aufruf der API mit der Testklasse. Dieses Beispiel (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) veranschaulicht die Integration der Token-Überprüfungsbibliothek in einen Medienserver.


```Java
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
