---
title: Profil für bestimmten Code abrufen
description: REST API V2 - Profil für bestimmten Code abrufen
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---


# Profil für bestimmten Code abrufen {#retrieve-profile-for-specific-code}

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Anfrage {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">path</td>
      <td>/api/v2/{serviceProvider}/profiles/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Pfadparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Die interne eindeutige Kennung, die dem Service Provider während des Onboarding-Prozesses zugeordnet ist.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">code</td>
      <td>Der Authentifizierungscode, der nach der Erstellung der Authentifizierungssitzung auf dem Streaming-Gerät abgerufen wurde.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorisierung</td>
      <td>Die Erstellung der Payload des Trägertokens wird in der Dokumentation <a href="../../../dynamic-client-registration-api.md">Dynamische Client-Registrierung</a> beschrieben.</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Die IP-Adresse des Streaming-Geräts.
         <br/><br/>
         Es wird dringend empfohlen, ihn immer für Server-zu-Server-Implementierungen zu verwenden, insbesondere wenn der Aufruf vom Programmierer-Dienst und nicht vom Streaming-Gerät erfolgt.
         <br/><br/>
         Bei Client-zu-Server-Implementierungen wird die IP-Adresse des Streaming-Geräts implizit gesendet.
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         Der Medientyp, der von der Clientanwendung akzeptiert wird.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>Der Benutzeragent der Clientanwendung.</td>
      <td>optional</td>
   </tr>
</table>

## Reaktion {#response}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">Code</th>
      <th style="background-color: #EFF2F7; width: 20%;">Text</th>
      <th style="background-color: #EFF2F7;">Beschreibung</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Der Antworttext enthält eine Zuordnung gültiger Profile, die leer sein können.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Ungültige Anfrage</td>
      <td>
        Die Anfrage ist ungültig, der Client muss die Anfrage korrigieren und es erneut versuchen. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Unerlaubt</td>
      <td>
        Das Zugriffstoken ist ungültig. Der Client muss ein neues Zugriffstoken abrufen und es erneut versuchen. Weitere Informationen finden Sie in der Dokumentation zur <a href="../../../dynamic-client-registration-api.md">dynamischen Client-Registrierung</a> .
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Methode nicht zulässig</td>
      <td>
        Die HTTP-Methode ist ungültig. Der Client muss eine HTTP-Methode verwenden, die für die angeforderte Ressource zulässig ist, und es erneut versuchen. Weitere Informationen finden Sie im Abschnitt <a href="#request">Anfrage</a> .
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Interner Server-Fehler</td>
      <td>
        Auf der Serverseite ist ein Problem aufgetreten. Der Antworttext kann Fehlerinformationen enthalten, die der Dokumentation <a href="../../../enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.
      </td>
   </tr>
</table>

### Erfolg {#success}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">body</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">profiles</td>
      <td>
        JSON mit einer Zuordnung von Schlüssel-/Wert-Paaren.
        <br/><br/>
        Das Schlüsselelement wird durch den folgenden Wert definiert:
        <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Wert</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Die interne eindeutige Kennung, die dem Identitäts-Provider während des Onboarding-Prozesses zugeordnet ist.</td>
               <td><i>erforderlich</i></td>
            </tr>
         </table>
         Das value-Element wird durch die folgenden Attribute definiert:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Der Zeitstempel, vor dem das Profil nicht gültig ist.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Der Zeitstempel, nach dem das Profil nicht gültig ist.</td>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Emittent</td>
               <td>
                  Die Entität, der das Profil gehört.
                  <br/><br/>
                  Mögliche Werte sind:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Wert</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">mvpd<br/><br/>z. B. Spektrum, Kabel usw.</td>
                        <td>
                            Das Profil wurde wie folgt erstellt:
                            <ul>
                                <li>Grundlegende Authentifizierung</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Adobe</td>
                        <td>
                            Das Profil wurde wie folgt erstellt:
                            <ul>
                                <li>Geringfügiger Zugriff</li>
                                <li>Temporärer Zugriff</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">type</td>
               <td>
                  Der Profiltyp.
                  <br/><br/>
                  Mögliche Werte sind:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Wert</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">regulär</td>
                        <td>
                            Das Profil wurde wie folgt erstellt:
                            <ul>
                                <li>Grundlegende Authentifizierung</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">abgebaut</td>
                        <td>
                            Das Profil wurde wie folgt erstellt:
                            <ul>
                                <li>Geringfügiger Zugriff</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">temporär</td>
                        <td>
                            Das Profil wurde wie folgt erstellt:
                            <ul>
                                <li>Temporärer Zugriff</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>erforderlich</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">attributes</td>
               <td>
                    Die Liste der Benutzermetadatenattribute.
                    <br/><br/>
                    Diese Attribute können:
                    <ul>
                        <li>Obligatorisch, z. B. "userId"</li>
                        <li>Nicht obligatorisch, z. B. "zip", "budgetId", "maxRating" usw.</li>
                    </ul>
                    Die Werte für die Attribute können:
                    <ul>
                        <li>einfach</li>
                        <li>Liste</li>
                        <li>map</li>
                    </ul>
               </td>
               <td><i>erforderlich</i></td>
            </tr>
         </table>
      </td>
      <td><i>erforderlich</i></td>
</table>

### Fehler {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>erforderlich</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">body</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">error</td>
      <td>Der Fehler enthält zusätzliche Informationen, die der Dokumentation <a href="../../../enhanced-error-codes.md">Verbesserte Fehlercodes</a> entsprechen.</td>
      <td><i>erforderlich</i></td>
   </tr>
</table>

## Stichproben {#samples}

### 1. Vorhandene und gültige authentifizierte Profile auf einem sekundären Gerät abrufen, nachdem eine einfache Authentifizierung durchgeführt wurde

>[!BEGINTABS]

>[!TAB Anfrage]

```JSON
GET /api/v2/REF30/profiles/Cablevision/XTC98W
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
```

>[!TAB Antwort]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Cablevision",
            "type" : "regular",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                },
                "householdId" : {
                    "value" : "BASE64_value_householdId",
                    "state" : "plain"
                },
                "zip" : {
                    "value" : "BASE64_value_zip",
                    "state" : "enc"
                },
                "parental-controls" : {
                    "value" : BASE64_value_parental-controls,
                    "state" : "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]
