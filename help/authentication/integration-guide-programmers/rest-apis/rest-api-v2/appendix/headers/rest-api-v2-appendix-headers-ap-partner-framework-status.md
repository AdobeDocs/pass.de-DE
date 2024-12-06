---
title: Header - AP-Partner-Framework-Status
description: REST API V2 - Header - AP-Partner-Framework-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# Header - AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.

## Übersicht {#overview}

Der Anforderungsheader <b>AP-Partner-Framework-Status</b> enthält Statusinformationen, die von einem Partner-Framework abgerufen wurden, um Single Sign-On (SSO) zu erzielen.

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;partner_framework_status_information&gt;</td>
   </tr>
   <tr>
      <td>Kopfzeilentyp</td>
      <td>Anforderungs-Header</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Nein</td>
   </tr>
</table>

## Richtlinien {#directives}

<b>&lt;partner_framework_status_information></b>

Der `Base64-encoded` -Wert des JSON-Elements, das die folgenden Attribute enthält:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Dies ist ein erforderliches Attribut.
         <br/><br/>
         Die Benutzerberechtigungsstatus-Informationen, die vom Partner-Framework zurückgegeben und von der Anwendung verarbeitet werden.
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
                     <li>provided - Der Benutzer hat der Anwendung Zugriff auf Abonnementinformationen gewährt.</li>
                     <li>verweigert - Der Benutzer verweigerte der Anwendung den Zugriff auf Abonnementinformationen.</li>
                     <li>Ausstehend - Der Benutzer hat sich noch nicht dafür entschieden, der Anwendung den Zugriff auf Abonnementinformationen zu erlauben.</li>
                     <li>notDetermined - Die Anwendung kann nicht auf Abonnementinformationen zugreifen.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>error</td>
               <td>
                  Dies ist ein optionales Attribut.
                  <br/><br/>
                  Dies kann verwendet werden, um den Fehler des Partner-Frameworks weiterzugeben, falls einer beim Abfragen nach Statusinformationen für Benutzerberechtigungen ausgelöst wird.
                  <br/><br/>
                  Dies ist ein JSON-Element mit den folgenden Attributen:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>code</td>
                        <td>Eine Zeichenfolge, die den Fehler eindeutig identifiziert, wie vom Partner-Framework definiert.</td>
                     </tr>
                     <tr>
                        <td>message</td>
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
         Die vom Partner-Framework zurückgegebenen und von der Anwendung verarbeiteten Statusinformationen des Anbieters.
         <br/><br/>
         Dies ist ein JSON-Element mit den folgenden Attributen:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  Dies ist ein erforderliches Attribut.
                  <br/><br/>
                  Dies ist die mappingId , die den MVPD identifiziert, der während des Authentifizierungsflusses auf der Partner-Framework-Ebene verwendet wird.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Dies ist ein erforderliches Attribut.
                  <br/><br/>
                  Dies ist das Ablaufdatum des authentifizierten Benutzerprofils, falls der Benutzer sich erfolgreich mit einem unterstützten MVPD auf der Partner-Framework-Ebene angemeldet hat.
               </td>
            </tr>
            <tr>
               <td>error</td>
               <td>
                  Dies ist ein optionales Attribut.
                  <br/><br/>
                  Dies kann verwendet werden, um den Fehler des Partner-Frameworks zu übergeben, falls einer bei der Abfrage nach Statusinformationen der Provider-Anmeldung ausgelöst wird.
                  <br/><br/>
                  Dies ist ein JSON-Element mit den folgenden Attributen:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>code</td>
                        <td>Eine Zeichenfolge, die den Fehler eindeutig identifiziert, wie vom Partner-Framework definiert.</td>
                     </tr>
                     <tr>
                        <td>message</td>
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
