---
title: Header - AP-Partner-Framework-Status
description: REST API v2 - Kopfzeile - AP-Partner-Framework-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: 5c912bbbe97fff65d38dbade32cd4554ad8c2fac
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# Header - AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.

## Überblick {#overview}

Der <b>AP-Partner-Framework-Status</b>-Anfrage-Header enthält Statusinformationen, die von einem Partner-Framework abgerufen wurden, um Single Sign-on (SSO) zu erzielen.

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;partner_framework_status_information&gt;</td>
   </tr>
   <tr>
      <td>Kopfzeilentyp</td>
      <td>Anfrage-Header</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Nein</td>
   </tr>
</table>

## Anweisungen {#directives}

<b>&lt;PARTNER_FRAMEWORK_STATUS_INFORMATION></b>

Der `Base64-encoded` des JSON-Elements, das die folgenden Attribute enthält:

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Dies ist ein erforderliches Attribut.
         <br/><br/>
         Die vom Partner-Framework zurückgegebenen und von der Anwendung verarbeiteten Statusinformationen zu Benutzerberechtigungen.
         <br/><br/>
         Dies ist ein JSON-Element mit den folgenden Attributen:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  Dies ist ein erforderliches Attribut.
                  <br/><br/>
                  Dies ist eine Auflistung mit den folgenden möglichen Werten:
                  <br/>
                  <ul>
                     <li><b>granted</b><br/>Der Benutzer hat der Anwendung erlaubt, auf Abonnementinformationen zuzugreifen.</li>
                     <li><b>verweigert</b><br/> Der Benutzer hat der Anwendung den Zugriff auf Abonnementinformationen verweigert.</li>
                     <li><b>Ausstehend</b><br/> Der Benutzer hat noch nicht ausgewählt, ob die Anwendung auf Abonnementinformationen zugreifen darf.</li>
                     <li><b>notDetermined</b><br/>Die Anwendung darf nicht auf Abonnementinformationen zugreifen.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>Fehler</td>
               <td>
                  Dies ist ein optionales Attribut.
                  <br/><br/>
                  Dies kann verwendet werden, um den Partner-Framework-Fehler zu übergeben, falls beim Abfragen von Statusinformationen zu Benutzerberechtigungen ein Fehler ausgelöst wird.
                  <br/><br/>
                  Dies ist ein JSON-Element mit den folgenden Attributen:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>Code</td>
                        <td>Eine Zeichenfolge, die den Fehler eindeutig identifiziert, wie vom Partner-Framework definiert.</td>
                     </tr>
                     <tr>
                        <td>Nachricht</td>
                        <td>Eine Zeichenfolge, die die Beschreibung des Fehlers enthält, wie vom Partner-Framework definiert.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         Dies ist ein erforderliches Attribut.
         <br/><br/>
         Die vom Partner-Framework zurückgegebenen und von der Anwendung verarbeiteten Provider-Anmeldeinformationen.
         <br/><br/>
         Dies ist ein JSON-Element mit den folgenden Attributen:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>ID</td>
               <td>
                  Dies ist ein erforderliches Attribut.
                  <br/><br/>
                  Dies ist die mappingId, die die MVPD identifiziert, die während des Authentifizierungsflusses auf Partnerframework-Ebene verwendet wurde.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Dies ist ein erforderliches Attribut.
                  <br/><br/>
                  Dies ist das Ablaufdatum des authentifizierten Benutzerprofils, falls der Benutzer sich erfolgreich mit einer unterstützten MVPD auf Partnerframework-Ebene angemeldet hat.
                  <br/><br/>
                  Dies muss ein Zeitstempel in Millisekunden seit der Unix-Epoche sein (z. B. „1735689600000„), ausgedrückt als Zeichenfolge.
               </td>
            </tr>
            <tr>
               <td>Fehler</td>
               <td>
                  Dies ist ein optionales Attribut.
                  <br/><br/>
                  Dies kann verwendet werden, um den Partner-Framework-Fehler zu übergeben, falls bei der Abfrage nach Statusinformationen der Provider-Anmeldung ein Fehler ausgelöst wird.
                  <br/><br/>
                  Dies ist ein JSON-Element mit den folgenden Attributen:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>Code</td>
                        <td>Eine Zeichenfolge, die den Fehler eindeutig identifiziert, wie vom Partner-Framework definiert.</td>
                     </tr>
                     <tr>
                        <td>Nachricht</td>
                        <td>Eine Zeichenfolge, die die Beschreibung des Fehlers enthält, wie vom Partner-Framework definiert.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## Beispiele {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
