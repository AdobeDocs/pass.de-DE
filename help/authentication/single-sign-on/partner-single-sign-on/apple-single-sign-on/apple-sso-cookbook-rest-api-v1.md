---
title: Apple SSO-Cookbook (REST API V1)
description: Apple SSO-Cookbook (REST API V1)
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1473'
ht-degree: 0%

---

# Apple SSO-Cookbook (REST API V1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

Die Adobe Pass Authentication REST API V1 unterstützt Single Sign-On (SSO) für Endbenutzer von Clientanwendungen, die auf iOS, iPadOS oder tvOS ausgeführt werden.

Dieses Dokument dient als Erweiterung der vorhandenen REST API V1-Dokumentation, die Sie [hier](/help/authentication/rest-api-reference.md) finden.

## Cookbook {#apple-sso-cookbook-rest-api-v1-cookbook}

Um das Apple SSO-Benutzererlebnis nutzen zu können, muss die Anwendung das von Apple entwickelte [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) integrieren, während für die Kommunikation mit der Adobe Pass Authentication REST API V1 die unten dargestellte Schrittfolge befolgt werden muss.

### Berechtigung {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>Pro Tipp:</u>** Die Streaming-Anwendung muss den Zugriff auf die auf Geräteebene gespeicherten Abonnementinformationen des Benutzers anfordern, für die der Benutzer der Anwendung die Berechtigung zum Fortfahren erteilen muss, ähnlich wie bei der Bereitstellung des Zugriffs auf die Kamera oder das Mikrofon des Geräts. Diese Berechtigung muss pro Anwendung mit dem [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) von Apple angefordert werden, und das Gerät speichert die Benutzerauswahl.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Es wird empfohlen, Benutzer, die sich weigern, Zugriff auf Abonnementinformationen zu gewähren, dazu anzuregen, die Vorteile des Single-Sign-On-Benutzererlebnisses für Apple zu erläutern. Beachten Sie jedoch, dass der Benutzer seine Entscheidung ändern kann, indem er zu den Anwendungseinstellungen (Zugriffsberechtigung für TV-Anbieter) oder zu *`Settings -> TV Provider`* unter iOS und iPadOS oder *`Settings -> Accounts -> TV Provider`* unter tvOS navigiert.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Es wird empfohlen, die Berechtigung des Benutzers anzufordern, wenn die Anwendung in den Vordergrundzustand wechselt, da die Anwendung jederzeit nach [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers suchen kann, bevor eine Benutzerauthentifizierung erforderlich ist.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)

### Authentifizierung {#apple-sso-cookbook-rest-api-v1-authentication}

* [Gibt es ein gültiges Adobe-Authentifizierungstoken?](#step1)
* [Ist der Benutzer über Partner SSO angemeldet?](#step2)
* [Adobe-Konfiguration abrufen](#step3)
* [Initiieren des Partner SSO-Workflows mit der Adobe-Konfiguration](#step4)
* [Ist die Benutzeranmeldung erfolgreich?](#step5)
* [Abrufen einer Profilanfrage von Adobe für das ausgewählte MVPD](#step6)
* [Weiterleiten der Adobe-Anfrage an Partner SSO zum Abrufen des Profils](#step7)
* [Austausch des Partner SSO-Profils für ein Adobe-Authentifizierungstoken](#step8)
* [Wird Adobe-Token erfolgreich generiert?](#step9)
* [Starten des regelmäßigen Authentifizierungs-Workflows](#step10)
* [Fahren Sie mit den Autorisierungsabläufen fort.](#step11)

![](../../../assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### Schritt: &quot;Gibt es ein gültiges Adobe-Authentifizierungstoken?&quot; {#step1}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den API-Dienst für Adobe Pass-Authentifizierung [Check Authentication Token](/help/authentication/check-authentication-token.md) .

#### Schritt: &quot;Ist der Benutzer über Partner SSO angemeldet?&quot; {#step2}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das Medium [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* Die Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
* Die Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
* Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.

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

#### Schritt: &quot;Adobe-Konfiguration abrufen&quot; {#step3}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den API-Dienst für die Adobe Pass-Authentifizierung [Bereitstellung des MVPD List](/help/authentication/provide-mvpd-list.md)-API-Diensts.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Beachten Sie die MVPD-Eigenschaften: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* und achten Sie besonders auf die Kommentare, die in Codefragmenten aus anderen Schritten vorgestellt werden.

#### Schritt &quot;Initiieren des Partner SSO-Workflows mit Adobe-config&quot; {#step4}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das Medium [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* Die Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
* Die Anwendung muss einen [Delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) für den VSAccountManager bereitstellen.
* Die Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
* Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.

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

#### Schritt: &quot;Ist die Benutzeranmeldung erfolgreich?&quot; {#step5}

>[!TIP]
>
> **<u>Pro Tipp:</u>** Bitte beachten Sie das Codefragment aus dem Schritt &quot;[&quot;Initiate Partner SSO workflow with Adobe config&quot;](#step4)&quot;. Die Benutzeranmeldung ist erfolgreich, wenn *`vsaMetadata!.accountProviderIdentifier`* einen gültigen Wert enthält und das aktuelle Datum den Wert *`vsaMetadata!.authenticationExpirationDate`* nicht überschritten hat.

#### Schritt &quot;Abrufen einer Profilanfrage von Adobe für den ausgewählten MVPD&quot; {#step6}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den API-Dienst für die Adobe Pass-Authentifizierung [Profilanfrage](/help/authentication/retrieve-profilerequest.md) .

>[!TIP]
>
> **<u>Pro Tipp:</u>** Beachten Sie, dass die vom Video Subscriber Account Framework abgerufene Provider-ID den *`platformMappingId`* in Bezug auf die Adobe Pass-Authentifizierungskonfiguration darstellt. Daher muss die Anwendung den MVPD ID-Eigenschaftswert mithilfe des Werts *`platformMappingId`* über den API-Dienst für die Adobe Pass-Authentifizierung [MVPD-Liste bereitstellen](/help/authentication/provide-mvpd-list.md) ermitteln.

#### Schritt: &quot;Weiterleiten der Adobe-Anfrage an Partner SSO zum Abrufen des Profils&quot; {#step7}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über das Medium [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).


* Die Anwendung muss die [Berechtigung für den Zugriff auf die Abonnementinformationen des Benutzers überprüfen und nur dann fortfahren, wenn der Benutzer dies zulässt.](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)
* Die Anwendung muss eine [Anfrage](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) für Abonnentenkontoinformationen einreichen.
* Die Anwendung muss warten und die [Metadaten](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-Informationen verarbeiten.

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

#### Schritt: &quot;Austausch des Partner SSO-Profils für ein Adobe-Authentifizierungstoken&quot; {#step8}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über den API-Dienst für die Adobe Pass-Authentifizierung [Token Exchange](/help/authentication/token-exchange.md) .

>[!TIP]
>
> **<u>Pro Tipp:</u>** Bitte beachten Sie das Codefragment aus dem Schritt &quot;[&quot;Weiterleiten der Adobe-Anfrage an Partner SSO, um das-Profil zu erhalten&quot;](#step7) . Dieser *`vsaMetadata!.samlAttributeQueryResponse!`* stellt den *`SAMLResponse`* dar, der an [Token Exchange](/help/authentication/token-exchange.md) übergeben werden muss und eine Zeichenfolgenbearbeitung und -kodierung erfordert (*Base64* -kodiert und *URL* -kodiert danach), bevor der Aufruf ausgeführt wird.

#### Schritt: &quot;Wird das Adobe-Token erfolgreich generiert?&quot; {#step9}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die erfolgreiche Adobe Pass-Authentifizierungsmethode [Token Exchange](/help/authentication/token-exchange.md), bei der es sich um eine &quot;*`204 No Content`*&quot;-Antwort handelt, die angibt, dass das Token erfolgreich erstellt wurde und für die Autorisierungsflüsse verwendet werden kann.

#### Schritt: &quot;Starten Sie den regulären Authentifizierungs-Workflow&quot; {#step10}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die API-Dienste Adobe Pass Authentication [Registration Code Request](/help/authentication/registration-code-request.md), [Initiate Authentication](/help/authentication/initiate-authentication.md) und [Retrieve Authentication Token](/help/authentication/retrieve-authentication-token.md) oder [Check Authentication Token](/help/authentication/check-authentication-token.md).

>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die tvOS-Implementierung/en durchzuführen.

* Die Anwendung muss [einen Registrierungs-Code ](/help/authentication/registration-code-request.md) abrufen und ihn dem Endbenutzer auf dem ersten Gerät (Bildschirm) präsentieren.
* Die Anwendung muss [abfragen starten, um den Authentifizierungsstatus](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) zu bestätigen, nachdem der Registrierungs-Code abgerufen wurde.
* Eine andere Anwendung muss [Authentifizierung initiieren](/help/authentication/initiate-authentication.md) auf einem zweiten Gerät (Bildschirm), wenn der Registrierungs-Code verwendet wird.
* Die Anwendung muss [Polling](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) stoppen, wenn das Authentifizierungstoken generiert wird.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die iOS/iPadOS-Implementierung/iPadOS zu implementieren.

* Die Anwendung muss [einen Registrierungs-Code ](/help/authentication/registration-code-request.md) abrufen, der dem Endbenutzer nicht auf dem ersten Gerät (Bildschirm) angezeigt werden sollte.
* Die Anwendung muss [Authentifizierung initiieren](/help/authentication/initiate-authentication.md) auf dem ersten Gerät (Bildschirm) unter Verwendung des Registrierungs-Codes und einer [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) - oder einer [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -Komponente.
* Die Anwendung muss [Abruf starten, um den Authentifizierungsstatus](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) zu ermitteln, nachdem die Komponente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) oder [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) geschlossen wurde.
* Die Anwendung muss [Polling](/help/authentication/retrieve-authentication-token.md) auf dem ersten Gerät (Bildschirm) stoppen, wenn das Authentifizierungstoken generiert wird.

#### Schritt: &quot;Mit Autorisierungsflüssen fortfahren&quot; {#step11}

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die API-Dienste Adobe Pass-Authentifizierung [Initiate Authorization](/help/authentication/initiate-authorization.md) und [Get Short Media Token](/help/authentication/obtain-short-media-token.md) .

### Abmelden {#apple-sso-cookbook-rest-api-v1-logout}

Das [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) bietet keine API zum programmgesteuerten Abmelden von Personen, die sich auf Geräteebene bei ihrem TV-Anbieterkonto angemeldet haben. Damit die Abmeldung vollständig wirksam wird, muss sich der Endbenutzer daher explizit von *`Settings -> TV Provider`* bei iOS/iPadOS oder *`Settings -> Accounts -> TV Provider`* bei tvOS abmelden. Die andere Option, die der Benutzer hätte, besteht darin, die Berechtigung zum Zugriff auf die Abonnementinformationen des Benutzers aus dem Abschnitt für die spezifischen Anwendungseinstellungen (TV Provider-Zugriff) zu entziehen.

>[!TIP]
>
> **<u>Tipp:</u>** Implementieren Sie dies über die API-Dienste Adobe Pass-Authentifizierung [Benutzer-Metadatenaufruf](/help/authentication/user-metadata.md) und [Abmelden](/help/authentication/initiate-logout.md) .

>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die tvOS-Implementierung/en durchzuführen.

* Die Anwendung muss mithilfe der &quot;*tokenSource&quot;* [Benutzermetadaten](/help/authentication/user-metadata.md) vom Adobe Pass-Authentifizierungsdienst ermitteln, ob die Authentifizierung durch eine Anmeldung über die Partner-SSO erfolgt ist oder nicht.
* Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von *`Settings -> Accounts -> TV Provider`* unter tvOS **nur** abzumelden, falls der Wert *&quot;tokenSource&quot;* gleich &quot;*Apple&quot; ist.*
* Die Anwendung muss [den Abmeldevorgang vom Adobe Pass-Authentifizierungsdienst mithilfe eines direkten HTTP-Aufrufs starten](/help/authentication/initiate-logout.md). Dies würde die Sitzungsbereinigung auf der MVPD-Seite nicht erleichtern.

>[!TIP]
>
> **<u>Pro Tipp:</u>** Gehen Sie wie folgt vor, um die iOS/iPadOS-Implementierung/iPadOS zu implementieren.

* Die Anwendung muss mithilfe der &quot;*tokenSource&quot;* [Benutzermetadaten](/help/authentication/user-metadata.md) vom Adobe Pass-Authentifizierungsdienst ermitteln, ob die Authentifizierung durch eine Anmeldung über die Partner-SSO erfolgt ist oder nicht.
* Die Anwendung muss den Benutzer anweisen/auffordern, sich explizit von *`Settings -> TV Provider`* bei iOS/iPadOS **nur** abzumelden, falls der Wert *&quot;tokenSource&quot;* gleich *&quot;Apple&quot;* ist.
* Die Anwendung muss [ den Abmeldevorgang vom Adobe Pass-Authentifizierungsdienst mit einer [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) - oder einer [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -Komponente initiieren. ](/help/authentication/initiate-logout.md) Dies würde die Sitzungsbereinigung auf der MVPD-Seite erleichtern.
