---
title: Abrufen von Client-Anmeldeinformationen
description: Dynamische Client-Registrierungs-API - Abrufen von Client-Anmeldeinformationen
exl-id: 0b39768b-25b8-47b9-8080-59c56fb829fb
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# Abrufen von Client-Anmeldeinformationen {#retrieve-client-credentials}

>[!IMPORTANT]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

>[!IMPORTANT]
>
> Die Implementierung der dynamischen Client-Registrierungs-API ist an die Dokumentation [Drosselungsmechanismus](/help/authentication/integration-guide-programmers/throttling-mechanism.md) gebunden.

## Anfrage {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Pfad</td>
      <td>/o/client/register</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Methode</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Hauptteilparameter</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">software_statement</td>
      <td>
            Die mit der registrierten Anwendung verknüpfte Softwareanweisung, die vom <a href="https://experience.adobe.com/#/pass/authentication">Adobe Pass TVE Dashboard</a> erstellt und heruntergeladen wurde.
            <br/><br/>
            Die Verwaltung registrierter Anwendungen wird in der Dokumentation <a href="../dynamic-client-registration-overview.md">Übersicht über die Dynamic Client-Registrierung</a> beschrieben.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirect_uri</td>
      <td>Der Umleitungs-URI, der mit dem Speicherort verknüpft ist, zu dem der Benutzeragent navigiert, wenn der Authentifizierungsfluss abgeschlossen ist.</td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>
         Der akzeptierte Medientyp für die gesendeten Ressourcen.
         <br/><br/>
         Es muss application/json sein.
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">x-device-info</td>
      <td>
         Die Erzeugung der Payload mit Geräteinformationen wird in der Dokumentation <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> beschrieben.
         <br/><br/>
         Es wird dringend empfohlen, sie immer dann zu verwenden, wenn die Geräteplattform der Anwendung die explizite Bereitstellung gültiger Werte zulässt.
         <br/><br/>
         Wenn angegeben, führt das Backend für die Adobe Pass-Authentifizierung explizit eingestellte Werte mit extrahierten Werten implizit zusammen (standardmäßig).
         <br/><br/>
         Wenn keine Angabe gemacht wird, verwendet das Backend für die Adobe Pass-Authentifizierung implizit die extrahierten Werte (standardmäßig).
      </td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Akzeptieren</td>
      <td>
         Der von der Client-Anwendung akzeptierte Medientyp.
         <br/><br/>
         Wenn angegeben, muss es application/json sein.
      </td>
      <td>fakultativ</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">user-agent</td>
      <td>Der Benutzeragent der Client-Anwendung.</td>
      <td>fakultativ</td>
   </tr>
</table>

## Antwort {#response}

### Erfolg {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         JSON-Objekt mit den folgenden Attributen:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_id</td>
               <td>Die Zeichenfolge der Client-Anwendungskennung.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_secret</td>
               <td>Die geheime Zeichenfolge des Client-Programms.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_id_issued_at</td>
               <td>Der Zeitpunkt, zu dem die Client-Anwendungskennung ausgegeben wurde.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">redirect_uris</td>
               <td>Das Array von Umleitungs-URI-Zeichenfolgen, das die Client-Anwendung in umleitungsbasierten Flüssen verwenden kann.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">grant_types</td>
               <td>Die Zeichenfolgen vom Typ „grant“, die die Client-Anwendung für den Client-Token-Endpunkt verwenden kann.</td>
               <td><i>required</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Bereiche</td>
               <td>Die Bereichszeichenfolgen, die die Adobe Pass-Authentifizierungs-APIs definieren, die die Client-Anwendung verwenden kann.</td>
               <td><i>required</i></td>
            </tr>
         </table>
      </td>
      <td><i>required</i></td>
</table>

### Fehler {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Kopfzeilen</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">content-type</td>
      <td>application/json</td>
      <td><i>required</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Textkörper</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Fehler</td>
      <td>
        Die möglichen Werte sind:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Wert</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">Invalid_request</td>
               <td>
                    Die Anfrage ist aus einem der folgenden Gründe ungültig:
                    <ul>
                        <li>In der Anfrage fehlt ein erforderlicher Parameter.</li>
                        <li>Die Anfrage enthält einen nicht unterstützten Parameterwert.</li>
                        <li>Die Anfrage wiederholt einen Parameter.</li>
                        <li>Die Anfrage ist fehlerhaft.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_redirect_uri</td>
               <td>Die Anfrage enthält einen ungültigen Wert für den Umleitungs-URI.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_software_statement</td>
               <td>Die Anfrage enthält einen Wert für die Software-Anweisung, der ungültig ist.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_software_statement</td>
               <td>Die Anfrage enthält einen -Wert für die Softwareanweisung, der nicht für die Verwendung durch den Adobe Pass-Authentifizierungsserver genehmigt ist.</td>
            </tr>
         </table>
      </td>
      <td><i>required</i></td>
   </tr>
</table>

## Beispiele {#samples}

### Abrufen von Client-Anmeldeinformationen {#samples-retrieve-client-credentials}

>[!BEGINTABS]

>[!TAB Anfrage]

```HTTPS
POST /o/client/register HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/json
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

{
    "software_statement": "eyJhbGciOiJSUzI1NiJ9.
        eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
        bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
        aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
        GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
        zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
        5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
        fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
        U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
        IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
    "redirect_uri": "adobepass://com.programmer"  
 }
```

>[!TAB Antwort - Erfolg]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
    "client_id": "s6BhdRkqt3",
    "client_secret": "t7AkePiru4",
    "redirect_uris": [
        "app://com.programmer.adobe#sdasdsadas"
    ],
    "grant_types": [
        "client_credentials"
    ],
    "scopes": [
        "api:client:v2"
    ],
    "client_id_issued_at": 1723227212
}
```

>[!TAB Antwort - Fehler]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
