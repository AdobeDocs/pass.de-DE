---
title: Apple SSO-Cookbook (REST-API)
description: Apple SSO-Cookbook (REST-API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 0%

---

# Apple SSO-Cookbook (REST-API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#Introduction}

Die Adobe Pass Authentication REST-API kann die SSO-Authentifizierung (Platform Single Sign-On) für Endbenutzer von Clientanwendungen unterstützen, die auf iOS, iPadOS oder tvOS ausgeführt werden. Dies erfolgt über den so genannten Apple SSO-Workflow.

Bitte beachten Sie, dass dieses Dokument als Erweiterung der vorhandenen REST-API-Dokumentation dient, die Sie hier finden [here](/help/authentication/rest-api-reference.md).

</br>

## Cookbooks {#Cookbooks}

Um das Apple SSO-Benutzererlebnis nutzen zu können, muss eine Anwendung die [Video-Abonnentenkonto](https://developer.apple.com/documentation/videosubscriberaccount) -Framework, das von Apple entwickelt wurde, während es hinsichtlich der Adobe Pass Authentication REST API-Kommunikation die unten dargestellte Tippsequenz befolgen muss.

</br>

### Authentifizierung {#Authentication}

- [Gibt es ein gültiges Adobe-Authentifizierungstoken?](#Is_there_a_valid_Adobe_authentication_token)
- [Ist der Benutzer über Platform SSO angemeldet?](#Is_the_user_logged_in_via_Platform_SSO)
- [Adobe-Konfiguration abrufen](#Fetch_Adobe_configuration)
- [Starten des Platform SSO-Workflows mit der Adobe-Konfiguration](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [Ist die Benutzeranmeldung erfolgreich?](#Is_user_login_successful)
- [Abrufen einer Profilanfrage von Adobe für das ausgewählte MVPD](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [Weiterleiten der Adobe-Anfrage an Platform SSO zum Abrufen des Profils](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Platform SSO-Profil für ein Adobe-Authentifizierungstoken austauschen](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [Wird Adobe-Token erfolgreich generiert?](#Is_Adobe_token_generated_successfully)
- [Workflow für die Authentifizierung im zweiten Bildschirm starten](#Initiate_second_screen_authentication_workflow)
- [Fahren Sie mit den Autorisierungsabläufen fort.](#Proceed_with_authorization_flows)



![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)

</br>

#### Schritt: &quot;Gibt es ein gültiges Adobe-Authentifizierungstoken?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das [Adobe Pass-Authentifizierung](/help/authentication/check-authentication-token.md) -Dienst.

</br>

#### Schritt: &quot;Ist der Benutzer über Platform SSO angemeldet?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das [Video-Abonnentenkonto](https://developer.apple.com/documentation/videosubscriberaccount) Framework.

- Der Antrag müsste auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) die Abonnementinformationen des Benutzers ein und wird nur fortgesetzt, wenn der Benutzer dies erlaubt hat.
- Der Antrag müsste eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Informationen zum Konto des Abonnenten.
- Die Anwendung müsste warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) Informationen.



>[!TIP]
>
> **<u>Pro Tipp:</u>** Befolgen Sie das Codefragment und achten Sie besonders auf die Kommentare.

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
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

</br>

#### Schritt: &quot;Adobe-Konfiguration abrufen&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das [Adobe Pass-Authentifizierung](/help/authentication/provide-mvpd-list.md) -Dienst.


>[!TIP]
>
> **<u>Pro Tipp:</u>** Beachten Sie die MVPD-Eigenschaften: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* und achten Sie besonders auf die Kommentare, die in Code-Snippets aus anderen Schritten vorgestellt werden.

</br>

#### Schritt &quot;Starten des Platform SSO-Workflows mit Adobe config&quot; {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das [Video-Abonnentenkonto](https://developer.apple.com/documentation/videosubscriberaccount) Framework.

- Der Antrag müsste auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) die Abonnementinformationen des Benutzers ein und wird nur fortgesetzt, wenn der Benutzer dies erlaubt hat.
- Der Antrag müsste eine [delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für den VSAccountManager.
- Der Antrag müsste eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Informationen zum Konto des Abonnenten.
- Die Anwendung müsste warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) Informationen.



>[!TIP]
>
> **<u>Pro Tipp:</u>** Befolgen Sie das Codefragment und achten Sie besonders auf die Kommentare.


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
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
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
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
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
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Schritt: &quot;Ist die Benutzeranmeldung erfolgreich?&quot; {#Is_user_login_successful}

>[!TIP]
>
> **<u>Pro Tipp:</u>** Beachten Sie das Codefragment aus der [&quot;Starten des Platform SSO-Workflows mit Adobe config&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config) Schritt. Die Benutzeranmeldung ist erfolgreich, falls die *`vsaMetadata!.accountProviderIdentifier`* einen gültigen Wert enthält und das aktuelle Datum nicht über die *`vsaMetadata!.authenticationExpirationDate`* -Wert.

</br>

#### Schritt &quot;Abrufen einer Profilanfrage von Adobe für den ausgewählten MVPD&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierung [Profilanfrage](/help/authentication/retrieve-profilerequest.md) -Dienst.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Bitte beachten Sie, dass die vom Framework für Videoponnentenkonten abgerufene Provider-ID den *`platformMappingId`* in Bezug auf die Adobe Pass-Authentifizierungskonfiguration. Daher muss die Anwendung den Wert der MVPD ID-Eigenschaft mithilfe der *`platformMappingId`* -Wert über das Medium Adobe Pass-Authentifizierung [MVPD-Liste bereitstellen](/help/authentication/provide-mvpd-list.md) -Dienst.

</br>

#### Schritt: &quot;Weiterleiten der Adobe-Anfrage an Platform SSO zum Abrufen des Profils&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das [Video-Abonnentenkonto](https://developer.apple.com/documentation/videosubscriberaccount) Framework.


- Der Antrag müsste auf [Zugriffsberechtigung](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) die Abonnementinformationen des Benutzers ein und wird nur fortgesetzt, wenn der Benutzer dies erlaubt hat.
- Der Antrag müsste eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Informationen zum Konto des Abonnenten.
- Die Anwendung müsste warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) Informationen.



>[!TIP]
>
> **<u>Pro Tipp:</u>** Befolgen Sie das Codefragment und achten Sie besonders auf die Kommentare.

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
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Schritt: &quot;Ersetzen des Platform SSO-Profils durch ein Adobe-Authentifizierungstoken&quot; {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierung [Token Exchange](/help/authentication/token-exchange.md) -Dienst.


>[!TIP]
>
> **<u>Pro Tipp:</u>** Beachten Sie das Codefragment aus der [&quot;Weiterleiten der Adobe-Anfrage an Platform SSO zum Abrufen des Profils&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile) Schritt. Diese *`vsaMetadata!.samlAttributeQueryResponse!`* steht für *`SAMLResponse`*, die weitergegeben werden muss [Token Exchange](/help/authentication/token-exchange.md) und erfordert Zeichenfolgenbearbeitung und -kodierung (*Base64* kodiert und *URL* codiert danach), bevor der Aufruf erfolgt.

</br>

#### Schritt: &quot;Wird das Adobe-Token erfolgreich generiert?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das Medium Adobe Pass-Authentifizierung [Token Exchange](/help/authentication/token-exchange.md) eine erfolgreiche Antwort, die *`204 No Content`*, der angibt, dass das Token erfolgreich erstellt wurde und für die Autorisierungsflüsse verwendet werden kann.

</br>

#### Schritt: &quot;Workflow für die Authentifizierung am zweiten Bildschirm starten&quot; {#Initiate_second_screen_authentication_workflow}

**Wichtig:** Die Terminologie &quot;Zweiter Workflow für die Bildschirmauthentifizierung&quot;eignet sich für AppleTV, während die Terminologie &quot;Workflow für die erste Bildschirmauthentifizierung&quot;/ &quot;Workflow für die regelmäßige Authentifizierung&quot; für iPhones und iPads besser geeignet wäre.


>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierung

[Registrierungs-Code-Anfrage](/help/authentication/registration-code-request.md), [Authentifizierung initiieren](/help/authentication/initiate-authentication.md) und [REST API-Authentifizierungstoken abrufen](/help/authentication/retrieve-authentication-token.md) oder [Authentifizierungstoken überprüfen](/help/authentication/check-authentication-token.md) Dienste.


>[!TIP]
>
> **<u>Pro Tipp:</u>** Führen Sie die folgenden Schritte für die tvOS-Implementierung/-Implementierungen aus.

- Der Antrag müsste [Registrierungscode abrufen](/help/authentication/registration-code-request.md) und präsentieren sie dem Endbenutzer auf dem ersten Gerät (Bildschirm).
- Die Anwendung müsste beginnen [Abfrage zur Bestätigung des Authentifizierungsstatus](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm), nachdem der Registrierungs-Code abgerufen wurde.
- Eine andere Anwendung müsste [Authentifizierung initiieren](/help/authentication/initiate-authentication.md) auf einem zweiten Gerät (Bildschirm), wenn der Registrierungs-Code verwendet wird.
- Der Antrag müsste [Abruf](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm), wenn das Authentifizierungstoken generiert wird.



>[!TIP]
>
> **<u>Pro Tipp:</u>** Führen Sie die folgenden Schritte für die iOS/iPadOS-Implementierung aus.

- Der Antrag müsste [Registrierungscode abrufen](/help/authentication/registration-code-request.md) , die dem Endbenutzer nicht auf dem ersten Gerät (Bildschirm) angezeigt werden sollte.
- Der Antrag müsste [Authentifizierung initiieren](/help/authentication/initiate-authentication.md) auf dem ersten Gerät (Bildschirm) mit dem Registrierungs-Code und einem [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) oder [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -Komponente.
- Die Anwendung müsste beginnen [Abfrage zum Authentifizierungsstatus](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) nach dem [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) oder [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -Komponente geschlossen.
- Der Antrag müsste [Abruf](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm), wenn das Authentifizierungstoken generiert wird.

</br>

#### Schritt: &quot;Mit Autorisierungsflüssen fortfahren&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierung [Autorisierung initiieren](/help/authentication/initiate-authorization.md) und [Short Media Token abrufen](/help/authentication/obtain-short-media-token.md) Dienste.

</br>

### Abmelden {#Logout}

Die [Video-Abonnentenkonto](https://developer.apple.com/documentation/videosubscriberaccount) Framework stellt keine API zum programmatischen Abmelden von Personen bereit, die sich auf Gerätesystemebene bei ihrem TV-Anbieterkonto angemeldet haben. Damit die Abmeldung vollständig wirksam wird, muss sich der Endbenutzer daher explizit von abmelden *`Settings -> TV Provider`* auf iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* auf tvOS. Die andere Option, die der Benutzer hätte, besteht darin, die Berechtigung zum Zugriff auf die Abonnementinformationen des Benutzers aus dem Abschnitt für die spezifischen Anwendungseinstellungen (TV Provider-Zugriff) zu entziehen.

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierung [Benutzer-Metadatenaufruf](/help/authentication/user-metadata.md) und [Abmelden](/help/authentication/initiate-logout.md) Dienste.


>[!TIP]
>
> **<u>Pro Tipp:</u>** Führen Sie die folgenden Schritte für die tvOS-Implementierung/-Implementierungen aus.


- Die Anwendung muss mithilfe des Felds &quot;&quot;feststellen, ob die Authentifizierung aufgrund einer Anmeldung über die Plattform-SSO erfolgte oder nicht.*tokenSource&quot;* [Benutzermetadaten](/help/authentication/user-metadata.md) vom Adobe Pass-Authentifizierungsdienst aus.
- Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von abzumelden *`Settings -> Accounts -> TV Provider`* auf tvOS **only** im Fall der *&quot;tokenSource&quot;* Wert gleich &quot;*Apple&quot;.*
- Der Antrag müsste [Logout starten](/help/authentication/initiate-logout.md) über den Adobe Pass-Authentifizierungsdienst mithilfe eines direkten HTTP-Aufrufs. Dies würde die Sitzungsbereinigung auf der MVPD-Seite nicht erleichtern.



>[!TIP]
>
> **<u>Pro Tipp:</u>** Führen Sie die folgenden Schritte für die iOS/iPadOS-Implementierung aus.

- Die Anwendung muss mithilfe der &quot;&quot; ermitteln, ob die Authentifizierung aufgrund einer Anmeldung über die Plattform-SSO erfolgte oder nicht.*tokenSource&quot;* [Benutzermetadaten](/help/authentication/user-metadata.md) vom Adobe Pass-Authentifizierungsdienst aus.
- Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von abzumelden *`Settings -> TV Provider`* auf iOS/iPadOS **only** im Fall der *&quot;tokenSource&quot;* Wert ist gleich *&quot;Apple&quot;*.
- Der Antrag müsste [Logout starten](/help/authentication/initiate-logout.md) vom Adobe Pass-Authentifizierungsdienst mithilfe einer [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) oder [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -Komponente. Dies würde die Sitzungsbereinigung auf der MVPD-Seite erleichtern.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
