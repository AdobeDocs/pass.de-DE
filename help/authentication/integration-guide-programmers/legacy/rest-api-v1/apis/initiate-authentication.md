---
title: Authentifizierung starten
description: Authentifizierung starten
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# (Legacy) Authentifizierung starten {#initiate-authentication}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Achten Sie darauf, über die neuesten Ankündigungen zu Produkten der Adobe Pass-Authentifizierung und Stilllegungszeitpläne auf der Seite [Produktankündigungen](/help/authentication/product-announcements.md) auf dem Laufenden zu bleiben.

>[!NOTE]
>
> Die REST-API-Implementierung wird durch [Drosselungsmechanismus) &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST-API-Endpunkte {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Beschreibung {#description}

Startet den Authentifizierungsprozess durch Benachrichtigung über ein MVPD-Auswahlereignis. Erstellt einen Datensatz in der Adobe Pass-Authentifizierungsdatenbank, der abgeglichen wird, wenn eine erfolgreiche Antwort von der MVPD empfangen wird.



| Endpunkt | Called </br>by | Eingabe   </br>Parameter | HTTP </br>Methode | Antwort | HTTP </br>Antwort |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/Authenticate | AuthN-Modul | &#x200B;1. request_id (obligatorisch)</br>2.  mso_id (obligatorisch)</br>3.  reg_code (obligatorisch)</br>4.  domain_name (obligatorisch)</br>5.  noflash=true - </br>    (Obligatorisch, Restparameter)</br>6.  no_iframe=true (obligatorisch, Restparameter)</br>7.  Zusätzliche Parameter (optional)</br>8.  redirect_url (obligatorisch) | GET | Die Anmelde-Web-App wird zur Anmeldeseite von MVPD weitergeleitet. | 302 für vollständige Umleitungsimplementierungen |

{style="table-layout:auto"}


| Eingabeparameter | Beschreibung |
| --- | --- |
| Requestor_id | Der Anforderer des Programmierers, für den dieser Vorgang gültig ist. |
| mso_id | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| reg_code | Der vom Reggie-Service generierte Registrierungs-Code. |
| domain_name | Die Ursprungs-Domain. |
| redirect_url | Die Anmelde-Web-App-Umleitungs-URL nach Abschluss der Authentifizierung. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Wichtig: Obligatorische Parameter -** Unabhängig von der Client-seitigen Implementierung sind alle oben genannten Parameter obligatorisch.
>
>
>Beispiel:
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**Wichtig: Optionale Parameter**
>
>Der Aufruf kann auch optionale Parameter enthalten, die andere Funktionen ermöglichen, z. B.:
>
> * generic\_data - Aktiviert die Verwendung von [Werbe-TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Hinweise** {#notes}

* Der Wert des `domain_name` muss auf einen der Domain-Namen festgelegt sein, die bei der Adobe Pass-Authentifizierung registriert sind.

* [Vermeiden Sie die Verwendung von &#39;&amp;&#39;reg\_code in der /Authenticate-Anfrage (Technische Anmerkung).](/help/authentication/integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* Der `redirect_url` muss der letzte Parameter in der richtigen Reihenfolge sein

* Der Wert des `redirect_url` muss URL-kodiert sein
