---
title: SSO über passive Authentifizierung
description: SSO über passive Authentifizierung
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# SSO über passive Authentifizierung

>[!NOTE]
>
>Der Inhalt dieser Seite dient nur Informationszwecken. Für die Verwendung dieser API ist eine aktuelle Lizenz von Adobe erforderlich. Eine unbefugte Anwendung ist nicht zulässig.


## Einführung

In diesem Dokument wird die Implementierung des passiven Authentifizierungsflusses beschrieben und beschrieben, wie dieser mit unserem Standard-Single-Sign-On-Ansatz funktioniert.

## Anwendungsfälle

Die Adobe Pass-Authentifizierung ermöglicht Single Sign-On (SSO) zwischen Apps/Sites. Nachdem sich ein Benutzer mit seinen MVPD-Anmeldeinformationen angemeldet hat, generiert die Adobe Pass-Authentifizierung ein sicheres Token, das die Authentifizierungssitzung des MVPD darstellt und dieses Token mithilfe einer Geräte-ID an das Gerät des Benutzers bindet. Die Adobe Pass-Authentifizierung speichert das Token/die Geräte-ID entweder auf einem Server oder auf dem Gerät.

Solange das Token noch gültig ist, erscheinen die Benutzer direkt als authentifiziert. Auf diese Weise können Benutzer ihre Anmeldedaten weniger häufig eingeben und gleichzeitig Transaktionen sicher halten.



Der hier beschriebene geschäftliche Anwendungsfall ist eine sehr spezifische Anforderung: Der Benutzer MUSS mindestens einmal für jede besuchte Site authentifiziert werden. Dadurch kann der MVPD Geschäftsregeln für die authN-Sitzung anwenden, die je nach Netzwerk variieren können. Es widerspricht dem aktuellen TVE-Versprechen, dass sich ein Benutzer nur einmal anmelden muss und auf allen Sites authentifiziert wird, die Teil des Adobe Pass-Authentifizierungs-Ökosystems sind.



Um die Geschäftsregel zu pflegen, aber auch ein gutes Benutzererlebnis zu gewährleisten, erfordert der MVPD NICHT, dass ein Benutzer die Anmeldeinformationen manuell bereitstellt. Wir können von dem zuvor festgelegten Sitzungs-Cookie profitieren und versuchen, eine automatische erneute Authentifizierung mithilfe des passiven Flusses durchzuführen. Aus der Sicht des Benutzers erscheint er automatisch als angemeldet.



Um diese Probleme zu lösen, haben wir zwei verschiedene Funktionen implementiert: Authentifizierung pro Netzwerk und passive Authentifizierungsunterstützung. Die MVPDs unterstützen passive SAML-Authentifizierung, bei der ein Benutzer einfach erneut authentifiziert wird, wenn eine authN-Sitzung auf dem IdP vorhanden ist, unabhängig davon, auf welcher Site die Sitzung erstellt wurde.



## Authentifizierung pro Netzwerk

Diese Funktion stellt sicher, dass der MVPD für jede besuchte Site eine Authentifizierungsanfrage erhält. Diese Funktion bedeutet, dass ein Authentifizierungstoken für Adobe Pass-Authentifizierung an die Anforderer-ID gebunden wird, sodass nur für das Netzwerk gültig ist, das die ID angefordert hat. Sobald sich der Benutzer bei Site &quot;A&quot;authentifiziert und anschließend Site &quot;B&quot;besucht, muss er sich authentifizieren.



Beachten Sie, dass der Benutzer auf dem MVPD IdP bereits authentifiziert ist und keine Anmeldedaten angeben muss, sondern stattdessen einfach von Site &quot;B&quot; zu MVPD IdP und dann zurück umgeleitet wird. Derselbe Benutzer wird weiterhin auf Site &quot;A&quot;authentifiziert, wenn er zurückkehrt.



Der folgende Ablauf zeigt die grundlegende Authentifizierungsfunktion pro Netzwerk:





## Passive Authentifizierung

Das Ziel besteht darin, die erneute Authentifizierung im Hintergrund durchzuführen, ohne dass eine vollständige Browser-Umleitung erforderlich ist und die Auswahl angezeigt wird. Daher wird ein Benutzer, der von Site A zu Site B wechselt, automatisch authentifiziert.



Aus UX-Sicht gibt es keinen Unterschied zwischen diesem Fluss und einem Fluss, der mit einem regulären MVPD ausgeführt wird. Der Benutzer sieht, dass nach der Eingabe von Anmeldedaten als Ergebnis des Besuchs von Site A diese automatisch auf Site B authentifiziert wird.



Um diesen Fluss funktionsfähig zu machen, muss der MVPD die passive Authentifizierung unterstützen, damit der ausgeblendete iframe nicht auf der Anmeldeseite &quot;hängenbleibt&quot;, falls der MVPD keine Sitzung mehr hat. Dies erfolgt über das standardmäßige Attribut &quot;isPassive&quot;.



Das folgende Diagramm zeigt den verbesserten Fluss und die passive Authentifizierung &quot;hinter den Kulissen&quot;:





SAML-Anforderungsbeispiel
Hier finden Sie ein SAML-Anforderungsbeispiel für den passiven authN-Fluss:


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

MVPDs weisen bestimmte SSO-Scoping-Domänenbeschränkungen auf. So könnten beispielsweise nur einige Domänen von einigen MVPDs zugelassen werden (SSO für dasselbe Medienunternehmen, aber nicht unternehmensübergreifend).
Einige MVPDs erfordern möglicherweise unterschiedliche Authentifizierungsregeln, um durchgesetzt zu werden. Beispielsweise können MVPDs je nach Netzwerk unterschiedliche Authentifizierungs-TTLs aufweisen. MVPDs können auch für einige Netzwerke eine häusliche Authentifizierung ermöglichen, für andere jedoch nicht (Anwendungsfälle der elterlichen Kontrolle sind hier stark vertreten).


Bei diesen Geschäftsanforderungen sollte berücksichtigt werden, dass der Hauptanwendungsfall darin besteht, dass der Benutzer nach erfolgreicher Anmeldung mit seinem MVPD nicht verpflichtet werden sollte, sich erneut anzumelden.

Dies kann durch die Verwendung der Authentifizierung pro Netzwerk mit passiver authN-Markierung erreicht werden.



## Bekannte Einschränkungen

iOS - Aufgrund der Art des lokalen Speichers in iOS funktionieren SSO-Flüsse nicht für Anwendungen, die von verschiedenen Anbietern entwickelt wurden. Weitere Informationen zur einmaligen Anmeldung in iOS 8 und höher finden Sie in dieser technischen Anmerkung.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
