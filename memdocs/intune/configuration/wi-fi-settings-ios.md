---
title: Wi-Fi-instellingen voor iOS-/iPadOS-apparaten configureren in Microsoft Intune - Azure | Microsoft Docs
titleSuffix: ''
description: Een configuratieprofiel voor een Wi-Fi-apparaat toevoegen of maken voor iOS-/iPadOS-apparaten. Zie de verschillende instellingen, zoals voor het toevoegen van certificaten, voor het kiezen van een EAP-type en het selecteren van een verificatiemethode in Microsoft Intune.
keywords: ''
author: MandiOhlinger
ms.author: mandia
manager: dougeby
ms.date: 03/20/2020
ms.topic: reference
ms.service: microsoft-intune
ms.subservice: configuration
ms.localizationpriority: medium
ms.technology: ''
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27a37642891693f59c8dc38aa9bb047b251084ca
ms.sourcegitcommit: 7f17d6eb9dd41b031a6af4148863d2ffc4f49551
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "80327356"
---
# <a name="add-wi-fi-settings-for-ios-and-ipados-devices-in-microsoft-intune"></a>Wi-Fi-instellingen toevoegen voor iOS-/iPadOS-apparaten in Microsoft Intune

U kunt een profiel maken met specifieke Wi-Fi-instellingen en dit profiel vervolgens implementeren op uw iOS-/iPadOS-apparaten. Microsoft Intune biedt veel functies, waaronder het verifiëren bij het netwerk, het toevoegen van een PKCS- of SCEP-certificaat en meer.

Deze Wi-Fi-instellingen worden in twee categorieën onderverdeeld: Basisinstellingen en instellingen op zakelijk niveau.

In dit artikel worden deze instellingen beschreven.

## <a name="before-you-begin"></a>Voordat u begint

[Maak een apparaatprofiel](wi-fi-settings-configure.md).

> [!NOTE]
> Deze instellingen zijn beschikbaar voor alle inschrijvingstypen. Zie [iOS-/iPadOS-inschrijving](../enrollment/ios-enroll.md) voor meer informatie over de inschrijvingstypen.

## <a name="basic-profiles"></a>Basic-profielen

- **Wi-Fi-type**: Selecteer **Basic**.
- **Netwerknaam**: Voer een naam in voor deze Wi-Fi-verbinding. Deze waarde krijgen gebruikers te zien in de lijst met beschikbare verbindingen op hun apparaat.
- **SSID**: Afkorting voor **serviceset-id**. Deze eigenschap is de echte naam van het draadloze netwerk waarmee apparaten verbinding maken. Gebruikers zien echter alleen de netwerknaam die u hebt geconfigureerd wanneer ze de verbinding kiezen.
- **Automatisch verbinding maken**: Kies **Inschakelen** om automatisch verbinding te maken met dit netwerk wanneer het apparaat binnen het bereik daarvan is. Kies **Uitschakelen** om te voorkomen dat apparaten automatisch verbinding maken.
- **Verborgen netwerk**: Kies **Inschakelen** als de SSID van het netwerk niet wordt uitgezonden. Kies **Uitschakelen** als de SSID van het netwerk wordt uitgezonden en zichtbaar is.
- **Beveiligingstype**: Selecteer het beveiligingsprotocol voor de verificatie bij het Wi-Fi-netwerk. Uw opties zijn:

  - **Open (geen verificatie)** : Gebruik deze optie alleen als het netwerk niet beveiligd is.
  - **WPA/WPA2 - persoonlijk**: voer het wachtwoord in in de **vooraf gedeelde sleutel**. Wanneer het netwerk van uw organisatie is ingesteld of geconfigureerd, wordt er ook een wachtwoord of netwerksleutel geconfigureerd. Voer dit wachtwoord of deze netwerksleutel in voor de PSK-waarde.
  - **WEP**

- **Proxyinstellingen**: Uw opties zijn:
  - **Geen**: Er zijn geen proxyinstellingen geconfigureerd.
  - **Handmatig**: Voer het **adres van de proxyserver** als IP-adres in, samen met het bijbehorende **poortnummer**.
  - **Automatisch**: Gebruik een bestand om de proxyserver te configureren. Voer de **URL van de proxyserver** (bijvoorbeeld `http://proxy.contoso.com`) in die het configuratiebestand bevat.

## <a name="enterprise-profiles"></a>Enterprise-profielen

- **Wi-Fi-type**: Kies **Enterprise**.
- **SSID**: Afkorting voor **serviceset-id**. Deze eigenschap is de echte naam van het draadloze netwerk waarmee apparaten verbinding maken. Gebruikers zien echter alleen de netwerknaam die u hebt geconfigureerd wanneer ze de verbinding kiezen.
- **Automatisch verbinding maken**: Kies **Inschakelen** om automatisch verbinding te maken met dit netwerk wanneer het apparaat binnen het bereik daarvan is. Kies **Uitschakelen** om te voorkomen dat apparaten automatisch verbinding maken.
- **Verborgen netwerk**: Kies **Inschakelen** om te voorkomen dat dit netwerk op het apparaat wordt weergegeven in de lijst met beschikbare netwerken. De SSID wordt niet verzonden. Kies **Uitschakelen** om dit netwerk in de lijst met beschikbare netwerken op het apparaat weer te geven.
- **Beveiligingstype**: Selecteer het beveiligingsprotocol voor de verificatie bij het Wi-Fi-netwerk. Uw opties zijn:
  - **WPA - Enterprise**
  - **WPA/WPA2 - Enterprise**

- **EAP-type**: Kies het type Extensible Authentication Protocol (EAP) dat wordt gebruikt voor het verifiëren van beveiligde draadloze verbindingen. Uw opties zijn:

  - **EAP-FAST**: Voer de **PAC-instellingen (Protected Access Credential)** in. Deze optie gebruikt beveiligde toegangsreferenties om een geverifieerde tunnel tussen de client en de verificatieserver te maken. Uw opties zijn:
    - **(PAC) niet gebruiken**
    - **(PAC) gebruiken**: Als er een al PAC-bestand bestaat, gebruik dit dan.
    - **PAC gebruiken en inrichten**: Maak het PAC-bestand en voeg dit toe aan uw apparaten.
    - **PAC anoniem inrichten en gebruiken**: maak het PAC-bestand en voeg het toe aan uw apparaten zonder verificatie met de server.

  - **EAP-SIM**

  - **EAP-TLS**: Voer ook in:

    - **Vertrouwelijke server** - **Namen van certificaatservers**: **Voeg een of meer algemene namen toe** die worden gebruikt in de certificaten die door uw vertrouwde certificeringsinstantie (CA) zijn uitgegeven aan uw toegangsservers voor draadloze netwerken. Voeg bijvoorbeeld `mywirelessserver.contoso.com` of `mywirelessserver` toe. Wanneer u deze informatie invoert, kunt u het venster Dynamisch vertrouwen negeren dat wordt weergegeven op apparaten van gebruikers wanneer zij verbinding maken met dit Wi-Fi-netwerk.
    - **Basiscertificaat voor servervalidatie**: Kies een bestaand profiel voor een vertrouwd basiscertificaat. Met dit certificaat kan de client het certificaat van de server voor toegang tot het draadloze netwerk vertrouwen.

    - **Clientverificatie** Kies een **verificatiemethode**. Uw opties zijn:

      - **Afgeleide referentie**: gebruik een certificaat dat is afgeleid van de smartcard van een gebruiker. Als er geen uitgever voor afgeleide referenties is geconfigureerd, wordt u door Intune gevraagd om er een toe te voegen. Zie [Afgeleide referenties gebruiken in Microsoft Intune](../protect/derived-credentials.md) voor meer informatie.

      - **Certificaten**: Kies het profiel van het SCEP- of PKCS-clientcertificaat dat ook op het apparaat is geïmplementeerd. Dit certificaat is de identiteit die door het apparaat wordt gepresenteerd aan de server om de verbinding te verifiëren.

    - **Identiteitsprivacy (externe identiteit)** : Voer de tekst in die wordt verzonden in antwoord op een EAP-identiteitsaanvraag. Deze tekst kan elke waarde hebben, zoals `anonymous`. Tijdens verificatie wordt deze anonieme identiteit in eerste instantie verzonden en wordt deze gevolgd door de echte identificatie in een beveiligde tunnel.

  - **EAP-TTLS**: Voer ook in:

    - **Vertrouwelijke server** - **Namen van certificaatservers**: **Voeg een of meer algemene namen toe** die worden gebruikt in de certificaten die door uw vertrouwde certificeringsinstantie (CA) zijn uitgegeven aan uw toegangsservers voor draadloze netwerken. Voeg bijvoorbeeld `mywirelessserver.contoso.com` of `mywirelessserver` toe. Wanneer u deze informatie invoert, kunt u het venster Dynamisch vertrouwen negeren dat wordt weergegeven op apparaten van gebruikers wanneer zij verbinding maken met dit Wi-Fi-netwerk.
    - **Basiscertificaat voor servervalidatie**: Kies een bestaand profiel voor een vertrouwd basiscertificaat. Met dit certificaat kan de client het certificaat van de server voor toegang tot het draadloze netwerk vertrouwen.

    - **Clientverificatie**: kies een **verificatiemethode**. Uw opties zijn:

      - **Afgeleide referentie**: gebruik een certificaat dat is afgeleid van de smartcard van een gebruiker. Als er geen uitgever voor afgeleide referenties is geconfigureerd, wordt u door Intune gevraagd om er een toe te voegen. Zie [Afgeleide referenties gebruiken in Microsoft Intune](../protect/derived-credentials.md) voor meer informatie.

      - **Gebruikersnaam en wachtwoord**: De gebruiker wordt gevraagd om een gebruikersnaam en wachtwoord om de verbinding te verifiëren. Voer ook in:
        - **Niet-EAP-methode (interne identiteit)** : Kies hoe de verbinding moet worden geverifieerd. Zorg ervoor dat u hetzelfde protocol kiest dat op uw Wi-Fi-netwerk is geconfigureerd.

          Uw opties zijn: **Niet-versleuteld wachtwoord (PAP)** , **Challenge Handshake Authentication Protocol (CHAP)** , **Microsoft CHAP (MS-CHAP)** of **Microsoft CHAP versie 2 (MS-CHAP v2)**

      - **Certificaten**: Kies het profiel van het SCEP- of PKCS-clientcertificaat dat ook op het apparaat is geïmplementeerd. Dit certificaat is de identiteit die door het apparaat wordt gepresenteerd aan de server om de verbinding te verifiëren.

      - **Identiteitsprivacy (externe identiteit)** : Voer de tekst in die wordt verzonden in antwoord op een EAP-identiteitsaanvraag. Deze tekst kan elke waarde hebben, zoals `anonymous`. Tijdens verificatie wordt deze anonieme identiteit in eerste instantie verzonden en wordt deze gevolgd door de echte identificatie in een beveiligde tunnel.

  - **LEAP**

  - **PEAP**: Voer ook in:

    - **Vertrouwelijke server** - **Namen van certificaatservers**: **Voeg een of meer algemene namen toe** die worden gebruikt in de certificaten die door uw vertrouwde certificeringsinstantie (CA) zijn uitgegeven aan uw toegangsservers voor draadloze netwerken. Voeg bijvoorbeeld `mywirelessserver.contoso.com` of `mywirelessserver` toe. Wanneer u deze informatie invoert, kunt u het venster Dynamisch vertrouwen negeren dat wordt weergegeven op apparaten van gebruikers wanneer zij verbinding maken met dit Wi-Fi-netwerk.
    - **Basiscertificaat voor servervalidatie**: Kies een bestaand profiel voor een vertrouwd basiscertificaat. Met dit certificaat kan de client het certificaat van de server voor toegang tot het draadloze netwerk vertrouwen.

    - **Clientverificatie**: kies een **verificatiemethode**. Uw opties zijn:

      - **Afgeleide referentie**: gebruik een certificaat dat is afgeleid van de smartcard van een gebruiker. Als er geen uitgever voor afgeleide referenties is geconfigureerd, wordt u door Intune gevraagd om er een toe te voegen. Zie [Afgeleide referenties gebruiken in Microsoft Intune](../protect/derived-credentials.md) voor meer informatie.

      - **Gebruikersnaam en wachtwoord**: De gebruiker wordt gevraagd om een gebruikersnaam en wachtwoord om de verbinding te verifiëren. 

      - **Certificaten**: Kies het profiel van het SCEP- of PKCS-clientcertificaat dat ook op het apparaat is geïmplementeerd. Dit certificaat is de identiteit die door het apparaat wordt gepresenteerd aan de server om de verbinding te verifiëren.

      - **Identiteitsprivacy (externe identiteit)** : Voer de tekst in die wordt verzonden in antwoord op een EAP-identiteitsaanvraag. Deze tekst kan elke waarde hebben, zoals `anonymous`. Tijdens verificatie wordt deze anonieme identiteit in eerste instantie verzonden en wordt deze gevolgd door de echte identificatie in een beveiligde tunnel.

- **Proxyinstellingen**: Uw opties zijn:
  - **Geen**: Er zijn geen proxyinstellingen geconfigureerd.
  - **Handmatig**: Voer het **adres van de proxyserver** als IP-adres in, samen met het bijbehorende **poortnummer**.
  - **Automatisch**: Gebruik een bestand om de proxyserver te configureren. Voer de **URL van de proxyserver** (bijvoorbeeld `http://proxy.contoso.com`) in die het configuratiebestand bevat.

## <a name="next-steps"></a>Volgende stappen

Het profiel is gemaakt, maar er gebeurt niets. Vervolgens moet u [dit profiel toewijzen](device-profile-assign.md) en [de status ervan controleren](device-profile-monitor.md).

Configureer Wi-Fi-instellingen op apparaten met [Android](wi-fi-settings-android.md), [Android Enterprise](wi-fi-settings-android-enterprise.md), [macOS](wi-fi-settings-macos.md) en [Windows 10](wi-fi-settings-windows.md).
