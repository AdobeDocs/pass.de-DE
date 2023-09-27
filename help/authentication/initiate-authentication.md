---
title: Authentifizierung initiieren
description: Authentifizierung initiieren
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# Authentifizierung initiieren {#initiate-authentication}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## REST-API-Endpunkte {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Beschreibung {#description}

Initiiert den Authentifizierungsprozess durch Information über ein MVPD-Auswahlereignis. Erstellt einen Datensatz in der Adobe Pass-Authentifizierungsdatenbank, der abgestimmt wird, wenn eine erfolgreiche Antwort vom MVPD empfangen wird.



| Endpunkt | aufgerufen  </br>von | Eingabe   </br>Parameter | HTTP  </br>Methode | Reaktion | HTTP  </br>Reaktion |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate | AuthN-Modul | 1. requestor_id (erforderlich)</br>2.  mso_id (erforderlich)</br>3.  reg_code (erforderlich)</br>4.  domain_name (erforderlich)</br>5.  noflash=true -  </br>    (Obligatorisch, Restparameter)</br>6.  no_iframe=true (Obligatorisch, Restparameter)</br>7.  Zusätzliche Parameter (optional)</br>8.  redirect_url (erforderlich) | GET | Die Anmelde-Webanwendung wird zur MVPD-Anmeldeseite weitergeleitet. | 302 für vollständige Umleitungsimplementierungen |

{style="table-layout:auto"}


| Eingabeparameter | Beschreibung |
| --- | --- |
| requestor_id | Der Programmierer-Anforderer, für den dieser Vorgang gültig ist. |
| mso_id | Die MVPD-ID, für die dieser Vorgang gültig ist. |
| reg_code | Der vom Reggie-Dienst generierte Registrierungscode. |
| domain_name | Die Ursprungsdomäne. |
| redirect_url | Die Umleitungs-URL der Webapp-Anmeldung nach Abschluss der Authentifizierung. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Wichtig: Erforderliche Parameter -** Unabhängig von der clientseitigen Implementierung sind alle oben genannten Parameter obligatorisch.
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
>Der Aufruf kann auch optionale Parameter enthalten, die andere Funktionen wie:
>
> * generic\_data - ermöglicht die Verwendung von [Promotional TempPass](/help/authentication/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Hinweise** {#notes}

* Der Wert der `domain_name` muss auf einen der bei der Adobe Pass-Authentifizierung registrierten Domänennamen festgelegt werden. Weitere Informationen finden Sie unter [Registrierung und Initialisierung](/help/authentication/programmer-overview.md).

* [Vermeiden Sie die Verwendung von &quot;&amp;&#39;reg\_code&quot;in /authenticate request (Tech Note)](/help/authentication/clientless-avoid-using-reg-code-in-authenticate-request.md)

* Die `redirect_url` -Parameter muss der letzte sein in der Reihenfolge

* Der Wert der `redirect_url` Der Parameter muss URL-kodiert sein
