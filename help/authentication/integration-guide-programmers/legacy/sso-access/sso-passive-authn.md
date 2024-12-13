---
title: SSO über passive Authentifizierung
description: SSO über passive Authentifizierung
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '779'
ht-degree: 0%

---

# (Legacy) SSO über passive Authentifizierung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur zu Informationszwecken. Die Verwendung dieser API erfordert eine aktuelle Lizenz von Adobe. Eine unbefugte Nutzung ist nicht zulässig.


## Einführung

In diesem Dokument soll die Implementierung des passiven Authentifizierungsflusses beschrieben werden und wie dies mit unserem standardmäßigen Single-Sign-On-Ansatz funktioniert.

## Anwendungsfälle

Die Adobe Pass-Authentifizierung ermöglicht Single Sign-on (SSO) zwischen Apps/Sites. Nachdem sich ein Benutzer mit seinen MVPD-Anmeldeinformationen angemeldet hat, generiert die Adobe Pass-Authentifizierung ein sicheres Token, das die Authentifizierungssitzung von MVPD darstellt und dieses Token mithilfe einer Geräte-ID an das Gerät des Benutzers bindet. Die Adobe Pass-Authentifizierung speichert das Token/die Geräte-ID entweder auf einem Server oder auf dem Gerät.

Solange das Token noch gültig ist, werden die Benutzer direkt als authentifiziert angezeigt. Dadurch können Benutzer ihre Anmeldeinformationen seltener eingeben und gleichzeitig die Transaktionen sicher halten.



Der hier beschriebene Anwendungsfall für Unternehmen ist eine sehr spezifische Anforderung: Der Benutzer MUSS mindestens einmal für jede besuchte Site authentifiziert werden. Dadurch kann MVPD mit der Authentifizierungssitzung verknüpfte Geschäftsregeln anwenden, die je nach Netzwerk variieren können. Es steht im Konflikt mit der aktuellen TVE-Zusage, dass sich ein Benutzer nur einmal anmelden muss und auf allen Sites authentifiziert wird, die Teil des Adobe Pass-Authentifizierungs-Ökosystems sind.



Um die Geschäftsregel beizubehalten, aber auch ein gutes Benutzererlebnis zu bieten, erfordert die MVPD NICHT, dass Benutzende Anmeldeinformationen manuell bereitstellen. Wir können von dem zuvor eingestellten Sitzungs-Cookie profitieren und versuchen, eine automatische erneute Authentifizierung mit dem passiven Fluss durchzuführen; aus Benutzersicht wird er als automatisch angemeldet erscheinen.



Um diese Probleme zu lösen, haben wir zwei verschiedene Funktionen implementiert: Authentifizierung pro Netzwerk und Unterstützung der passiven Authentifizierung. Die MVPDs unterstützen die passive SAML-Authentifizierung, bei der ein Benutzer einfach erneut authentifiziert wird, wenn eine Authentifizierungssitzung auf dem IdP vorhanden ist, unabhängig davon, auf welcher Site die Sitzung erstellt wurde.



## Authentifizierung pro Netzwerk

Mit dieser Funktion wird sichergestellt, dass die MVPD für jede besuchte Site einmal eine Authentifizierungsanfrage erhält. Diese Funktion bedeutet, dass ein Adobe Pass-Authentifizierungstoken an die RequestorID gebunden ist, sodass es nur für das Netzwerk gültig ist, das es angefordert hat. Wenn sich der Benutzer also erst einmal auf Site „A“ authentifiziert und anschließend Site „B“ besucht hat, muss er sich authentifizieren.



Beachten Sie, dass der Benutzer aufgrund der Tatsache, dass auf der MVPD-IDp bereits authentifiziert ist, keine Anmeldeinformationen angeben muss, sondern der Browser einfach von Site „B“ zu MVPD-IDp und dann zurück umgeleitet wird. Derselbe Benutzer wird weiterhin auf Site „A“ authentifiziert, wenn er zurückkehrt.



Der folgende Ablauf zeigt die grundlegende Funktion „Authentifizierung pro Netzwerk“:





## Passive Authentifizierung

Das Ziel besteht darin, den Prozess der erneuten Authentifizierung im Hintergrund durchzuführen, ohne dass eine vollständige Browser-Umleitung erforderlich ist und die Auswahl angezeigt wird. Daher wird ein Benutzer, der von Site A zu Site B wechselt, automatisch authentifiziert.



Aus UX-Sicht gibt es keinen Unterschied zwischen diesem Fluss und einem Fluss, der mit einem regulären MVPD ausgeführt wird. Der Benutzer sieht, dass er nach der Eingabe der Anmeldeinformationen infolge des Besuchs von Site A automatisch auf Site B authentifiziert wird.



Damit dieser Fluss funktionsfähig ist, muss die MVPD die passive Authentifizierung unterstützen, damit der ausgeblendete iframe nicht auf der Anmeldeseite „stecken bleibt“, falls die MVPD keine Sitzung mehr hat. Dies erfolgt über das standardmäßige Attribut „isPassive“.



Das folgende Diagramm zeigt den verbesserten Fluss und die passive Authentifizierung „hinter den Kulissen“:





Beispiel einer SAML-Anfrage
Hier finden Sie ein Beispiel für eine SAML-Anfrage für den passiven Authentifizierungsfluss:


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## Verfahrensregeln

MVPDs verfügen über bestimmte Einschränkungen der SSO-Scoping-Domain. Beispielsweise könnten einige MVPDs nur einige Domains zulassen (SSO für dasselbe Medienunternehmen, aber nicht firmenübergreifend).
Bei einigen MVPDs müssen möglicherweise andere Authentifizierungsregeln durchgesetzt werden. Beispielsweise können MVPDs unterschiedliche Authentifizierungs-TTLs pro verschiedenen Netzwerken haben. Außerdem können MVPDs die Authentifizierung zu Hause für einige Netzwerke ermöglichen, für andere jedoch nicht (Anwendungsfälle für die elterliche Kontrolle sind hier stark vertreten).


Diese Geschäftsanforderungen sollten bedenken, dass der Hauptanwendungsfall darin besteht, dass die Benutzerin bzw. der Benutzer sich nicht nach erfolgreicher Anmeldung mit ihrer MVPD erneut anmelden muss.

Dies kann durch die Verwendung der Authentifizierung pro Netzwerk mit passivem Authentifizierungs-Flag erreicht werden.



## Bekannte Einschränkungen

iOS - Aufgrund der Beschaffenheit des lokalen iOS-Speichers funktionieren SSO-Flüsse in iOS nicht für Anwendungen, die von verschiedenen Anbietern entwickelt wurden. Weitere Informationen zu SSO in iOS 8 und höher finden Sie in dieser technischen Anmerkung.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
