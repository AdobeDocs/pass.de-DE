---
title: Apple SSO-Cookbook (REST-API)
description: Apple SSO-Cookbook (REST-API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---

# Apple SSO-Cookbook (REST-API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Einführung {#Introduction}

Die Adobe Pass Authentication REST-API kann die SSO-Authentifizierung (Platform Single Sign-On) für Endbenutzer von Clientanwendungen unterstützen, die auf iOS, iPadOS oder tvOS ausgeführt werden. Dies erfolgt über den so genannten Apple SSO-Workflow.

Beachten Sie, dass dieses Dokument als Erweiterung der vorhandenen REST-API-Dokumentation dient, die Sie [hier](/help/authentication/rest-api-reference.md) finden.

## Cookbooks {#Cookbooks}

Um von der Apple SSO-Benutzererfahrung zu profitieren, muss eine Anwendung das von Apple entwickelte Framework für das [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) integrieren, während sie in Bezug auf die Kommunikation mit der Adobe Pass Authentication REST API die unten dargestellte Tippsequenz befolgen muss.

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


#### Schritt: &quot;Gibt es ein gültiges Adobe-Authentifizierungstoken?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den Dienst [Adobe Pass-Authentifizierung](/help/authentication/check-authentication-token.md) .


#### Schritt: &quot;Ist der Benutzer über Platform SSO angemeldet?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das Framework des [Video-Abonnentenkontos](https://developer.apple.com/documentation/videosubscriberaccount) .

- Die Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
- Die Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
- Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.



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

#### Schritt: &quot;Adobe-Konfiguration abrufen&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den Dienst [Adobe Pass-Authentifizierung](/help/authentication/provide-mvpd-list.md) .


>[!TIP]
>
> **<u>Pro Tipp:</u>** Beachten Sie die MVPD-Eigenschaften: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* und achten Sie besonders auf die Kommentare, die in Codefragmenten aus anderen Schritten vorgestellt werden.

#### Schritt &quot;Starten des Platform SSO-Workflows mit Adobe config&quot; {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das Framework des [Video-Abonnentenkontos](https://developer.apple.com/documentation/videosubscriberaccount) .

- Die Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
- Die Anwendung muss einen [Delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für den VSAccountManager bereitstellen.
- Die Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
- Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.



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
> **<u>Pro Tipp:</u>** Bitte beachten Sie das Codefragment aus dem Schritt &quot;[&quot;Initiate Platform SSO workflow with Adobe config&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config)&quot;. Die Benutzeranmeldung ist erfolgreich, wenn *`vsaMetadata!.accountProviderIdentifier`* einen gültigen Wert enthält und das aktuelle Datum den Wert *`vsaMetadata!.authenticationExpirationDate`* nicht überschritten hat.

#### Schritt &quot;Abrufen einer Profilanfrage von Adobe für den ausgewählten MVPD&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den Adobe Pass-Authentifizierungsdienst [Profilanfrage](/help/authentication/retrieve-profilerequest.md) .

>[!TIP]
>
> **<u>Pro Tipp:</u>** Beachten Sie, dass die vom Video Subscriber Account Framework abgerufene Provider-ID den *`platformMappingId`* in Bezug auf die Adobe Pass-Authentifizierungskonfiguration darstellt. Daher muss die Anwendung den Wert der MVPD ID-Eigenschaft mithilfe des Werts *`platformMappingId`* über den Dienst Adobe Pass Authentication [Provide MVPD List](/help/authentication/provide-mvpd-list.md) ermitteln.

#### Schritt: &quot;Weiterleiten der Adobe-Anfrage an Platform SSO zum Abrufen des Profils&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das Framework des [Video-Abonnentenkontos](https://developer.apple.com/documentation/videosubscriberaccount) .


- Die Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
- Die Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
- Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.



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
> **<u>Tipp:</u>** Implementieren Sie dies über den Adobe Pass-Authentifizierungsdienst [Token Exchange](/help/authentication/token-exchange.md) .


>[!TIP]
>
> **<u>Pro Tipp:</u>** Bitte beachten Sie das Codefragment aus dem Schritt &quot;[&quot;Weiterleiten der Adobe-Anfrage an Platform SSO, um das-Profil zu erhalten&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile) . Dieser *`vsaMetadata!.samlAttributeQueryResponse!`* stellt den *`SAMLResponse`* dar, der an [Token Exchange](/help/authentication/token-exchange.md) übergeben werden muss und eine Zeichenfolgenbearbeitung und -kodierung erfordert (*Base64* -kodiert und *URL* -kodiert danach), bevor der Aufruf ausgeführt wird.

</br>

#### Schritt: &quot;Wird das Adobe-Token erfolgreich generiert?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die erfolgreiche Antwort &quot;Adobe Pass Authentication [Token Exchange](/help/authentication/token-exchange.md)&quot;, bei der es sich um eine &quot;*`204 No Content`*&quot;-Meldung handelt, die angibt, dass das Token erfolgreich erstellt wurde und für die Autorisierungsflüsse verwendet werden kann.

</br>

#### Schritt: &quot;Workflow für die Authentifizierung am zweiten Bildschirm starten&quot; {#Initiate_second_screen_authentication_workflow}

**Wichtig:** Die Terminologie &quot;Zweiter Workflow für die Bildschirmauthentifizierung&quot;ist für Apple TV-Geräte geeignet, während die Terminologie &quot;Workflow für die Erstbildschirmauthentifizierung&quot;/ &quot;Regelmäßiger Authentifizierungsworkflow&quot; für iPhones und iPads besser geeignet wäre.


>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierung.

[Registrierungs-Code-Anfrage](/help/authentication/registration-code-request.md), [Initiate Authentication](/help/authentication/initiate-authentication.md) und [REST API Retrieve Authentication Token](/help/authentication/retrieve-authentication-token.md) oder [Check Authentication Token](/help/authentication/check-authentication-token.md) Dienste.


>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die tvOS-Implementierung/en durchzuführen.

- Die Anwendung muss [einen Registrierungs-Code ](/help/authentication/registration-code-request.md) abrufen und ihn dem Endbenutzer auf dem ersten Gerät (Bildschirm) präsentieren.
- Die Anwendung muss [abfragen starten, um den Authentifizierungsstatus](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) zu bestätigen, nachdem der Registrierungs-Code abgerufen wurde.
- Eine andere Anwendung muss [Authentifizierung initiieren](/help/authentication/initiate-authentication.md) auf einem zweiten Gerät (Bildschirm), wenn der Registrierungs-Code verwendet wird.
- Die Anwendung muss [Polling](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) stoppen, wenn das Authentifizierungstoken generiert wird.



>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die iOS/iPadOS-Implementierung/iPadOS zu implementieren.

- Die Anwendung muss [einen Registrierungs-Code ](/help/authentication/registration-code-request.md) abrufen, der dem Endbenutzer nicht auf dem ersten Gerät (Bildschirm) angezeigt werden sollte.
- Die Anwendung muss [Authentifizierung initiieren](/help/authentication/initiate-authentication.md) auf dem ersten Gerät (Bildschirm) unter Verwendung des Registrierungs-Codes und einer [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) - oder einer [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -Komponente.
- Die Anwendung muss [Abruf starten, um den Authentifizierungsstatus](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) zu ermitteln, nachdem die Komponente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) oder [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) geschlossen wurde.
- Die Anwendung muss [Polling](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) stoppen, wenn das Authentifizierungstoken generiert wird.

</br>

#### Schritt: &quot;Mit Autorisierungsflüssen fortfahren&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierungsdienste [Initiate Authorization](/help/authentication/initiate-authorization.md) und [Get Short Media Token](/help/authentication/obtain-short-media-token.md) .

</br>

### Abmelden {#Logout}

Das Framework [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) bietet keine API zum programmatischen Abmelden von Personen, die sich auf Geräteebene bei ihrem TV-Anbieterkonto angemeldet haben. Damit die Abmeldung vollständig wirksam wird, muss sich der Endbenutzer daher explizit von *`Settings -> TV Provider`* bei iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* bei tvOS abmelden. Die andere Option, die der Benutzer hätte, besteht darin, die Berechtigung zum Zugriff auf die Abonnementinformationen des Benutzers aus dem Abschnitt für die spezifischen Anwendungseinstellungen (TV Provider-Zugriff) zu entziehen.

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die Adobe Pass-Authentifizierungsdienste [Benutzer-Metadatenaufruf](/help/authentication/user-metadata.md) und [Abmelden](/help/authentication/initiate-logout.md) .


>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die tvOS-Implementierung/en durchzuführen.


- Die Anwendung muss mithilfe der &quot;*tokenSource&quot;* [Benutzermetadaten](/help/authentication/user-metadata.md) vom Adobe Pass-Authentifizierungsdienst ermitteln, ob die Authentifizierung aufgrund einer Anmeldung über die Plattform-SSO erfolgte oder nicht.
- Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von *`Settings -> Accounts -> TV Provider`* unter tvOS **nur** abzumelden, falls der Wert *&quot;tokenSource&quot;* gleich &quot;*Apple&quot; ist.*
- Die Anwendung muss [den Abmeldevorgang vom Adobe Pass-Authentifizierungsdienst mithilfe eines direkten HTTP-Aufrufs starten](/help/authentication/initiate-logout.md). Dies würde die Sitzungsbereinigung auf der MVPD-Seite nicht erleichtern.



>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die iOS/iPadOS-Implementierung/iPadOS zu implementieren.

- Die Anwendung muss mithilfe der &quot;*tokenSource&quot;* [Benutzermetadaten](/help/authentication/user-metadata.md) vom Adobe Pass-Authentifizierungsdienst ermitteln, ob die Authentifizierung durch eine Anmeldung über die Plattform-SSO erfolgt ist oder nicht.
- Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von *`Settings -> TV Provider`* bei iOS/iPadOS **nur** abzumelden, falls der Wert *&quot;tokenSource&quot;* gleich *&quot;Apple&quot;* ist.
- Die Anwendung muss [ den Abmeldevorgang vom Adobe Pass-Authentifizierungsdienst mit einer [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) - oder einer [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -Komponente initiieren. ](/help/authentication/initiate-logout.md) Dies würde die Sitzungsbereinigung auf der MVPD-Seite erleichtern.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
