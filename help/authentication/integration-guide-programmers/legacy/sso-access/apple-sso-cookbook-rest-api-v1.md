---
title: Apple SSO-Cookbook (REST API v1)
description: Apple SSO-Cookbook (REST API v1)
exl-id: 072a011f-e1bb-4d3e-bcb5-697f2d1739cc
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# (Legacy) Apple SSO Cookbook (REST API v1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

Die Adobe Pass-Authentifizierungs-REST-API V1 unterstützt das Partner Single Sign-On (SSO) für Endbenutzer von Client-Anwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden.

Dieses Dokument dient als Erweiterung der vorhandenen REST API V1-Dokumentation, die Sie ([) ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md).

## Cookbook {#apple-sso-cookbook-rest-api-v1-cookbook}

Um von dem Apple SSO-Benutzererlebnis zu profitieren, muss die Anwendung das von Apple entwickelte [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) integrieren, während sie für die Adobe Pass Authentication REST API V1-Kommunikation die unten beschriebene Reihenfolge der Schritte befolgen muss.

### Erlaubnis {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Die Streaming-Anwendung muss Zugriff auf die auf Geräteebene gespeicherten Abonnementinformationen des Benutzers anfordern, für die der Benutzer der Anwendung die Erlaubnis zum Fortfahren erteilen muss, ähnlich wie bei der Bereitstellung von Zugriff auf die Kamera oder das Mikrofon des Geräts. Diese Berechtigung muss pro Anwendung über das Apple-[Video-Abonnementkonto-Framework](https://developer.apple.com/documentation/videosubscriberaccount) angefordert werden und das Gerät speichert die Benutzerauswahl.

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Wir empfehlen, Benutzern, die sich weigern, Berechtigungen für den Zugriff auf Abonnementinformationen zu erteilen, Anreize zu bieten, indem sie die Vorteile des Apple-Single-Sign-On-Benutzererlebnisses erklären. Beachten Sie jedoch, dass die Benutzenden ihre Entscheidung ändern können, indem sie zu den Anwendungseinstellungen gehen (Zugriff auf TV-Anbieter) oder auf iOS und iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS *`Settings -> TV Provider`*.

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Es wird empfohlen, die Berechtigung des Benutzers anzufordern, wenn die Anwendung in den Vordergrundstatus wechselt, da die Anwendung jederzeit auf die Abonnementinformationen [Zugriffsberechtigung“ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) Benutzers prüfen kann, bevor eine Benutzerauthentifizierung erforderlich wird.

### Authentifizierung {#apple-sso-cookbook-rest-api-v1-authentication}

* [Gibt es ein gültiges Adobe-Authentifizierungstoken?](#step1)
* [Ist der Benutzer über Partner-SSO angemeldet?](#step2)
* [Adobe-Konfiguration abrufen](#step3)
* [Starten des Partner-SSO-Workflows mit der Adobe-Konfiguration](#step4)
* [Ist die Benutzeranmeldung erfolgreich?](#step5)
* [Profilanfrage von Adobe für die ausgewählte MVPD abrufen](#step6)
* [Leiten Sie die Adobe-Anfrage an Partner-SSO weiter, um das Profil zu erhalten.](#step7)
* [Austausch des Partner-SSO-Profils gegen ein Adobe-Authentifizierungstoken](#step8)
* [Wird das Adobe-Token erfolgreich generiert?](#step9)
* [Starten eines regulären Authentifizierungs-Workflows](#step10)
* [Fortfahren mit Autorisierungsflüssen](#step11)

![](../../../assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### Schritt: „Gibt es ein gültiges Adobe-Authentifizierungstoken?“ {#step1}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den API-Service zur Authentifizierung [Authentifizierungstoken ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) Adobe Pass.

#### Schritt: „Ist der Benutzer über Partner-SSO angemeldet?“ {#step2}

>[!TIP]
>
> **<u>Tipp</u>** Implementieren Sie dies über das [Video-Abonnementkonto-Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* Die Anwendung muss die Abonnementinformationen des Benutzers auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen und nur fortfahren, wenn der Benutzer dies zulässt.
* Der Antrag müsste eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
* Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.

>[!TIP]
>
> **<u>Profi-Tipp</u>** Folgen Sie dem Code-Snippet und achten Sie besonders auf die Kommentare.

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Initiate regular authentication workflow" step.
                ...
            }
}
...  
```

#### Schritt: &quot;Adobe-Konfiguration abrufen“ {#step3}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den API-Service zur Adobe Pass-Authentifizierung [Bereitstellung ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) MVPD-Liste .

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Bitte beachten Sie die MVPD-Eigenschaften: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* und achten Sie besonders auf die Kommentare, die in Codeausschnitten aus anderen Schritten präsentiert werden.

#### Schritt „Initiieren des Partner-SSO-Workflows mit Adobe-Konfiguration“ {#step4}

>[!TIP]
>
> **<u>Tipp</u>** Implementieren Sie dies über das [Video-Abonnementkonto-Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* Die Anwendung muss die Abonnementinformationen des Benutzers auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen und nur fortfahren, wenn der Benutzer dies zulässt.
* Die Anwendung muss einen &quot;[&quot; für ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) VSAccountManager bereitstellen.
* Der Antrag müsste eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
* Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.

>[!TIP]
>
> **<u>Profi-Tipp</u>** Folgen Sie dem Code-Snippet und achten Sie besonders auf die Kommentare.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate regular authentication workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate regular authentication workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate regular authentication workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate regular authentication workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Schritt: „Ist die Benutzeranmeldung erfolgreich?“ {#step5}

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Beachten Sie das Code-Fragment aus dem [-Schritt „Initiieren des Partner-SSO-Workflows mit Adobe-Konfiguration](#step4) . Die Benutzeranmeldung ist erfolgreich, wenn die *`vsaMetadata!.accountProviderIdentifier`* einen gültigen Wert enthält und das aktuelle Datum den *`vsaMetadata!.authenticationExpirationDate`* nicht überschritten hat.

#### Schritt „Profilanfrage von Adobe für die ausgewählte MVPD abrufen“ {#step6}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den API-Service zur Authentifizierung [Profilanfrage](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md) von Adobe Pass.

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Beachten Sie, dass die vom Framework für Videoabonnentenkonten erhaltene Provider-Kennung die *`platformMappingId`* in Bezug auf die Adobe Pass-Authentifizierungskonfiguration darstellt. Daher muss die Anwendung den Eigenschaftswert der MVPD-ID mithilfe des *`platformMappingId`*-Werts über den API-Service für die Adobe Pass-Authentifizierung [MVPD-Liste bereitstellen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) ermitteln.

#### Schritt: „Weiterleiten der Adobe-Anfrage an Partner-SSO, um das Profil zu erhalten“ {#step7}

>[!TIP]
>
> **<u>Tipp</u>** Implementieren Sie dies über das [Video-Abonnementkonto-Framework](https://developer.apple.com/documentation/videosubscriberaccount).


* Die Anwendung muss die Abonnementinformationen des Benutzers auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) überprüfen und nur fortfahren, wenn der Benutzer dies zulässt.
* Der Antrag müsste eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
* Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.

>[!TIP]
>
> **<u>Profi-Tipp</u>** Folgen Sie dem Code-Snippet und achten Sie besonders auf die Kommentare.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Partner SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Continue with the "Initiate regular authentication workflow" step.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Schritt: „Partner-SSO-Profil gegen ein Adobe-Authentifizierungstoken austauschen“ {#step8}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den API-Service zur Adobe Pass-Authentifizierung [Token-](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)).

>[!TIP]
>
> **<u>Profi-Tipp:</u>** Beachten Sie das Code-Fragment aus dem [ Schritt „Weiterleiten der Adobe-Anfrage an Partner-SSO zum Abrufen des Profils](#step7) . Dieser *`vsaMetadata!.samlAttributeQueryResponse!`* stellt den *`SAMLResponse`* dar, der an den [Token Exchange](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) übergeben werden muss und vor dem Aufruf eine Zeichenfolgenmanipulation und -codierung erfordert (*Base64*-codiert und *URL*-codiert).

#### Schritt: „Wird das Adobe-Token erfolgreich generiert?“ {#step9}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die erfolgreiche Antwort der Adobe Pass-Authentifizierung [Token-Austausch](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md), bei der es sich um eine *`204 No Content`* handelt, die angibt, dass das Token erfolgreich erstellt wurde und für die Autorisierungsflüsse verwendet werden kann.

#### Schritt: „Initiieren eines regulären Authentifizierungs-Workflows“ {#step10}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die API-Services [ Adobe Pass-Authentifizierung Registrierungs-Code](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md), [Authentifizierung ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) und [Authentifizierungstoken abrufen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) oder [Authentifizierungstoken überprüfen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) .

>[!TIP]
>
> **<u>Profi-Tipp</u>** Führen Sie die folgenden Schritte für die tvOS-Implementierung(en) aus.

* Die Anwendung müsste [einen Registrierungscode abrufen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) und ihn dem Endbenutzer auf dem 1. Gerät (Bildschirm) präsentieren.
* Die Anwendung müsste auf dem 1[ Gerät (Bildschirm) nach Erhalt des Registrierungs](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)Codes mit der Abfrage beginnen, um den Authentifizierungsstatus zu bestätigen.
* Eine andere Anwendung müsste [Authentifizierung initiieren](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) auf einem zweiten Gerät (Bildschirm), wenn der Registrierungs-Code verwendet wird.
* Die Anwendung muss den [ (Polling](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) stoppen, wenn das Authentifizierungs-Token generiert wird.

>[!TIP]
>
> **<u>Profi-Tipp</u>** Führen Sie die folgenden Schritte für die Implementierung(en) von iOS/iPadOS aus.

* Die Anwendung muss [einen Registrierungs-Code erhalten](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) der dem Endbenutzer nicht auf dem ersten Gerät (Bildschirm) angezeigt werden sollte.
* Die Anwendung müsste [Authentifizierung initiieren](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) auf dem ersten Gerät (Bildschirm) mithilfe des Registrierungs-Codes und einer [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)- oder [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller)-Komponente.
* Die Anwendung muss auf dem 1. Gerät ([) mit dem Abruf beginnen, um den Authentifizierungsstatus zu ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md), nachdem die Komponente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) oder [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) geschlossen wurde.
* Die Anwendung muss den [ (Polling](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) stoppen, wenn das Authentifizierungs-Token generiert wird.

#### Schritt: „Fortfahren mit Autorisierungsflüssen“ {#step11}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierung [Autorisierung initiieren](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) und [Short Media Token erhalten](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)-API-Services.

### Abmelden {#apple-sso-cookbook-rest-api-v1-logout}

Das [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) stellt keine API zum programmgesteuerten Abmelden von Personen bereit, die sich auf Gerätesystemebene bei ihrem TV Provider-Konto angemeldet haben. Damit die Abmeldung ihre volle Wirkung entfalten kann, muss sich der Endbenutzer daher explizit von *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS abmelden. Die andere Option, die der Benutzer haben könnte, besteht darin, dem Benutzer die Berechtigung zum Zugriff auf die Abonnementinformationen des Benutzers über den Abschnitt mit den spezifischen Anwendungseinstellungen zu entziehen (TV Provider-Zugriff).

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierung [Benutzermetadaten-Aufruf](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) und [Abmelde](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)-API-Services.

>[!TIP]
>
> **<u>Profi-Tipp</u>** Führen Sie die folgenden Schritte für die tvOS-Implementierung(en) aus.

* Die Anwendung muss mithilfe von &quot;*tokenSource“ (Benutzermetadaten) aus dem Adobe Pass-Authentifizierungsdienst feststellen, ob die Authentifizierung als Ergebnis einer Anmeldung über [&#128279;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) Partner-SSO erfolgt* .
* Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von *`Settings -> Accounts -> TV Provider`* unter tvOS abzumelden **nur**, wenn der *„tokenSource“* Wert &quot;*Apple&quot; entspricht.*
* Die Anwendung müsste [Abmelden) vom Adobe Pass-Authentifizierungs](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)Service über einen direkten HTTP-Aufruf starten. Dies würde die Sitzungsbereinigung auf MVPD-Seite nicht erleichtern.

>[!TIP]
>
> **<u>Profi-Tipp</u>** Führen Sie die folgenden Schritte für die Implementierung(en) von iOS/iPadOS aus.

* Die Anwendung muss mithilfe von &quot;*tokenSource“ (Benutzermetadaten) aus dem Adobe Pass-Authentifizierungsdienst feststellen, ob die Authentifizierung als Ergebnis einer Anmeldung über [&#128279;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) Partner-SSO erfolgt* .
* Die Anwendung müsste den Benutzer anweisen/auffordern, sich explizit von *`Settings -> TV Provider`* auf iOS/iPadOS abzumelden **nur** wenn der *„tokenSource“* Wert *&quot;Apple&quot;*.
* Die Anwendung müsste [Abmelden) vom Adobe Pass-Authentifizierungsdienst ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) einer [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)- oder [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller)-Komponente initiieren. Dies würde die Sitzungsbereinigung auf MVPD-Seite erleichtern.
