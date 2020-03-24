---
title: Beveiligingsbeleidsinstellingen voor Android-apps
titleSuffix: Microsoft Intune
description: In dit onderwerp worden de app-beveiligingsbeleidsinstellingen voor Android-apparaten beschreven.
keywords: ''
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 02/28/2020
ms.topic: conceptual
ms.service: microsoft-intune
ms.subservice: apps
ms.localizationpriority: high
ms.technology: ''
ms.assetid: 9e9ef9f5-1215-4df1-b690-6b21a5a631f8
ms.reviewer: demerson
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5dbe2abf7a23be9f9e0051a2ff26590e749f98c
ms.sourcegitcommit: 3d895be2844bda2177c2c85dc2f09612a1be5490
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/13/2020
ms.locfileid: "79341929"
---
# <a name="android-app-protection-policy-settings-in-microsoft-intune"></a>Instellingen voor beveiligingsbeleid voor apps voor Android in Microsoft Intune
In dit artikel worden de app-beveiligingsbeleidsinstellingen voor Android-apparaten beschreven. De beleidsinstellingen die worden beschreven, kunnen worden [geconfigureerd](app-protection-policies.md) voor een app-beveiligingsbeleid in het deelvenster **Instellingen** in Azure Portal.
Er zijn drie soorten beleidsinstellingen: gegevensbeschermingsinstellingen, toegangsvereisten en voorwaardelijk starten. In dit artikel verwijst de term *door beleid beheerde apps* naar apps die zijn geconfigureerd met een app-beveiligingsbeleid.

> [!IMPORTANT]
> De Intune-bedrijfsportal-app is vereist op het apparaat om app-beveiligingsbeleid voor Android-apparaten te ontvangen. Raadpleeg de [Appvereisten voor Intune-bedrijfsportaltoegang](../fundamentals/end-user-mam-apps-android.md) voor meer informatie.

## <a name="data-protection"></a>Gegevensbescherming 
### <a name="data-transfer"></a>Gegevensoverdracht
| Instelling | Gebruik | Standaardwaarde |
|------|------|------|
| **Back-up van organisatiegegevens maken naar de Android-back-upservices** | Selecteer **Blokkeren** om te voorkomen dat deze app back-ups maakt van gegevens voor werk of school in de [Android-back-upservice](https://developer.android.com/google/backup/index.html).<br><br> Selecteer **Toestaan** om toe te staan dat deze app back-ups maakt van gegevens voor werk of school.| **Toestaan** |
| **Organisatiegegevens naar andere apps verzenden** | Geef aan welke apps gegevens uit deze app kunnen ontvangen: <ul><li> **Door beleid beheerde apps**: overdracht alleen toestaan naar andere door beleid beheerde apps.</li> <li>**Alle apps**: overdracht naar alle apps toestaan. </li> <li>**Geen**: geen gegevensoverdracht naar apps toestaan, met inbegrip van andere door beleid beheerde apps.</li></ul> <p>Er zijn enkele uitzonderings-apps en -services waarnaar Intune standaard gegevens mag overbrengen. U kunt bovendien uw eigen uitzonderingen maken als u wilt toestaan dat gegevens worden overgedragen naar een app die geen ondersteuning biedt voor Intune-apps. Zie [Uitzonderingen voor gegevensoverdracht](app-protection-policy-settings-android.md#data-transfer-exemptions) voor meer informatie.<p>Dit beleid kan ook van toepassing zijn op koppelingen naar Android-apps.  Algemene webkoppelingen worden beheerd door de beleidsinstelling **App-koppelingen openen in Intune Managed Browser**.<p><div class="NOTE"><p>Opmerking</p><p>Intune biedt momenteel geen ondersteuning voor de functie Android Instant Apps. Via Intune worden gegevensverbindingen naar of van de app geblokkeerd. Zie [Android Instant Apps](https://developer.android.com/topic/instant-apps/index.html) in de documentatie voor Android-ontwikkelaars voor meer informatie.</p><p>Als **Organisatiegegevens naar andere apps verzenden** is ingesteld op **Alle apps**, worden er via het besturingssysteem mogelijk nog steeds tekstgegevens overgedragen naar het klembord.</p></div> | **Alle apps** | 
|<ul><ui>**Apps selecteren die moeten worden uitgesloten** | Deze optie is beschikbaar wanneer u bij de vorige optie *Door beleid beheerde apps* selecteert. | |
|<ul><ui>**Kopieën van de organisatiegegevens opslaan** | Kies **Blokkeren** als u het gebruik van de optie Opslaan als wilt uitschakelen in deze app. Kies **Toestaan** als u het gebruik van de optie Opslaan als wilt toestaan. **Opmerking:** *Deze instelling wordt ondersteund voor Microsoft Excel, OneNote, PowerPoint en Word. Het wordt mogelijk ook ondersteund door externe apps en LOB-apps.*| **Toestaan** |  
|<ul><ui>**Gebruiker toestemming geven om kopieën naar geselecteerde services op te slaan** |Gebruikers kunnen naar de geselecteerde services opslaan (OneDrive voor Bedrijven, SharePoint en lokale opslag). Alle andere services worden geblokkeerd.  | **0 geselecteerd** |
| **Gegevens ontvangen van andere apps** | Geef aan welke apps gegevens naar deze app kunnen overdragen: <ul><li>**Door beleid beheerde apps**: overdracht alleen toestaan vanuit andere door beleid beheerde apps.</li><li>**Alle apps**: gegevensoverdracht vanuit alle apps toestaan.</li><li>**Geen**: geen gegevensoverdracht vanuit apps toestaan, met inbegrip van andere door beleid beheerde apps. </li></ul> <p>Er zijn enkele uitzonderingsapps en -services van waaruit Intune gegevensoverdracht kan toestaan. Zie [Uitzonderingen voor gegevensoverdracht](app-protection-policy-settings-android.md#data-transfer-exemptions) voor een volledige lijst met apps en services. | **Alle apps** |
|<ul><ui>**Gegevens openen in organisatiedocumenten** | Deze optie is beschikbaar wanneer u bij de vorige optie *Door beleid beheerde apps* selecteert. U kunt kiezen uit: <ul><li>**Blokkeren**: het gebruik uitschakelen van de optie *Openen* of andere opties voor het delen van gegevens tussen accounts in deze app. </li><li>**Toestaan**: het gebruik toestaan van *Openen* of andere opties voor het delen van gegevens tussen accounts in deze app.||
|<ul><ui>**Gebruikers toestaan om gegevens van geselecteerde services te openen** | Deze optie is beschikbaar wanneer u bij de vorige optie *Blokkeren* selecteert. Selecteer opslagservices voor toepassingen waaruit gebruikers gegevens kunnen openen. Alle andere services worden geblokkeerd. Als u geen services selecteert, voorkomt u dat gebruikers gegevens kunnen openen.| |
| **Knippen, kopiëren en plakken tussen andere apps beperken** | Geef op wanneer knip-, kopieer- en plakbewerkingen voor deze app kunnen worden gebruikt. U kunt kiezen uit: <ul><li>**Geblokkeerd**:  geen knip-, kopieer- en plakbewerkingen toestaan tussen deze app en andere apps.</li><li>**Door beleid beheerde apps**: alleen knip-, kopieer- en plakbewerkingen toestaan tussen deze app en andere door beleid beheerde apps.</li><li>**Door beleid beheerde apps met Plakken in**: knippen en kopiëren toestaan tussen deze app en andere door beleid beheerde apps. Gegevens uit alle apps mogen in deze app worden geplakt.</li><li>**Elke app**: geen beperkingen voor knip-, kopieer- en plakbewerkingen vanuit en naar deze app. | **Elke app** |
| <ul><ui>**De tekenlimiet voor knippen en kopiëren voor elke app** | Hiermee geeft u het aantal tekens op dat kan worden geknipt of gekopieerd van gegevens van de organisatie en de accounts.  Hiermee kunt u het opgegeven aantal tekens delen, wanneer dit anders door de instelling Knippen, kopiëren en plakken met andere apps beperken wordt geblokkeerd.<p>Standaardwaarde = 0<p>**Opmerking**: Hiervoor is Intune-bedrijfsportal versie 5.0.4364.0 of hoger vereist.  | **0** |
| **Schermopname en Google Assistant** | Selecteer **Blokkeren** om schermopnamen en gebruik van de **Google Assistent**-mogelijkheden van het apparaat te blokkeren bij gebruik van deze app. Als u **Toestaan** kiest, wordt ook de voorbeeldafbeelding van Appwisselaar vervaagd bij gebruik van deze app in combinatie met een werk- of schoolaccount.| **Blokkeren** |
| **Goedgekeurde toetsenborden**  | Selecteer *Vereisen* en geef een lijst met goedgekeurde toetsenborden voor dit beleid op. <p>Aan gebruikers die geen goedgekeurd toetsenbord gebruiken, wordt gevraagd om een goedgekeurd toetsenbord te downloaden en te installeren voordat ze de beveiligde app kunnen gebruiken. Voor deze instelling moet de app de Intune SDK voor Android versie 6.2.0 of hoger hebben. | **Niet vereist** |
| <ul><ui>**Toetsenborden selecteren voor goedkeuring** | Deze optie is beschikbaar wanneer u bij de vorige optie *Vereisen* selecteert. Kies *Selecteren* om de lijst met toetsenborden en invoermethoden te beheren die kunnen worden gebruikt met apps die door dit beleid worden beveiligd. U kunt extra toetsenborden toevoegen aan de lijst en de standaardopties naar wens verwijderen. U moet ten minste één goedgekeurd toetsenbord hebben om de instelling op te slaan. Als u een toetsenbord wilt toevoegen, geeft u het volgende op: <ul><li>**Naam**: Een beschrijvende naam waarmee het toetsenbord wordt aangeduid en die zichtbaar is voor de gebruiker. </li><li>**Pakket-id**:  De Pakket-id in van de app in Google Play. Als de URL voor de app in Google Play bijvoorbeeld `https://play.google.com/store/details?id=com.contoskeyboard.android.prod` is, is de Pakket-id `com.contosokeyboard.android.prod`. Deze pakket-id wordt aan de gebruiker aangeboden als een eenvoudige koppeling om het toetsenbord uit Google Play te downloaden. <p><div class="NOTE"><p>Opmerking</p><p>Een gebruiker waaraan meerdere beleidsregels voor app-beveiliging zijn toegewezen, mag alleen de toetsenborden gebruiken die voor alle beleidsregels zijn goedgekeurd.</p> | |

### <a name="encryption"></a>Versleuteling
| Instelling | Gebruik | Standaardwaarde |
|------|------|------|
| **Organisatiegegevens versleutelen** | Kies **Vereisen** om versleuteling van werk- of schoolgegevens in deze app in te schakelen. Intune gebruikt een 256-bit OpenSSL-AES-versleutelingsmethode samen met het Android Keystore-systeem om appgegevens veilig te versleutelen. Gegevens worden synchroon versleuteld tijdens I/O-taken voor bestanden. Inhoud van de apparaatopslag wordt altijd versleuteld. Nieuwe bestanden worden versleuteld met 256-bits sleutels. Bestaande bestanden met 128-bits versleuteling ondergaan een migratiepoging naar 256-bits versleuteling, maar het proces wordt niet gegarandeerd. Bestanden die met 128-bits sleutels zijn versleuteld, blijven leesbaar. <br><br> De versleutelingsmethode is met FIPS 140-2 gevalideerd. Zie [OpenSSL FIPS Library and Android Guide](https://wiki.openssl.org/images/7/76/OpenSSL_FIPS_Library_and_Android_Guide.pdf) (OpenSSL FIPS-bibliotheek en Android-handleiding) voor meer informatie.     |  **Vereisen**|  
| <ul><ui>**Organisatiegegevens op ingeschreven apparaten versleutelen** | Selecteer **Vereist** om op alle apparaten versleuteling van organisatiegegevens af te dwingen met Intune-versleuteling op app-niveau. Selecteer **Niet vereist** om op ingeschreven apparaten versleuteling van organisatiegegevens niet af te dwingen met Intune-versleuteling op app-niveau.| **Vereisen** |


### <a name="functionality"></a>Functionaliteit
| Instelling | Gebruik | Standaardwaarde |
|------|------|------|
| **App synchroniseren met systeemeigen app Contactpersonen** | Kies **Blokkeren** om te voorkomen dat de app gegevens opslaat in de systeemeigen app Contactpersonen op het apparaat. Als u **Toestaan** kiest, kan de app gegevens opslaan in de systeemeigen app Contactpersonen op het apparaat. <br><br>Als u selectief wilt wissen om werk- of schoolgegevens uit de app te verwijderen, worden contactpersonen verwijderd die rechtstreeks vanuit de app met de systeemeigen Contactpersonen-app worden gesynchroniseerd. Contactpersonen die vanuit het systeemeigen adresboek zijn gesynchroniseerd met een andere externe bron, kunnen niet worden gewist. Dit is momenteel alleen van toepassing op de Microsoft Outlook-app. | **Toestaan** |
| **Organisatiegegevens afdrukken** | Kies **Blokkeren** om te voorkomen dat via de app werk- of schoolgegevens worden afgedrukt. Als u deze instelling op de standaardwaarde **Toestaan** laat staan, kunnen gebruikers alle organisatiegegevens exporteren en afdrukken. | **Toestaan** |
|**Overdracht van webinhoud naar andere apps beperken** | Geef aan hoe webinhoud (http-/https-koppelingen) wordt geopend in door beleid beheerde toepassingen. U kunt kiezen uit: <ul><li>**Elke app**: hiermee staat u webkoppelingen toe in elke app.</li><li>**Intune Managed Browser**: hiermee mag webinhoud alleen in de Intune Managed Browser worden geopend. Deze browser wordt via beleid beheerd.</li><li>**Microsoft Edge**: hiermee mag webinhoud alleen in Microsoft Edge worden geopend. Deze browser wordt via beleid beheerd.</li><li>**Niet-beheerde browser**: toestaan dat webinhoud alleen kan worden geopend in de niet-beheerde browser die is gedefinieerd door de instelling **Protocol onbeheerde browser**. De webinhoud blijft niet-beheerd in de doelbrowser.<br>**Opmerking**: Hiervoor is Intune-bedrijfsportal versie 5.0.4415.0 of hoger vereist.</li><br><br>**Door beleid beheerde browsers**<br>Op Android kunnen uw eindgebruikers meerdere andere door beleid beheerde apps kiezen die http-/https-koppelingen ondersteunen als Intune Managed Browser en Microsoft Edge beide niet zijn geïnstalleerd.<p>Als een door beleid beheerde browser wel is vereist maar niet is geïnstalleerd, wordt uw eindgebruikers gevraagd Microsoft Edge te installeren.<p>Als een door beleid beheerde browser is vereist, worden koppelingen van Android-apps beheerd door de beleidsinstelling **App mag gegevens overdragen naar ander apps**.<p>**Registratie van Intune-apparaten**<br>Als u Intune gebruikt voor het beheer van uw apparaten, raadpleegt u [Internettoegang beheren met beheerde-browserbeleid met Microsoft Intune](app-configuration-managed-browser.md).<p>**Door beleid beheerde Microsoft Edge-browser**<br>De Microsoft Edge-browser voor mobiele apparaten (iOS/iPadOS en Android) biedt ondersteuning voor het app-beveiligingsbeleid van Intune. Gebruikers die zich in de Microsoft Edge-browsertoepassing aanmelden met hun zakelijke Azure AD-account worden beveiligd door Intune. De Microsoft Edge-browser integreert de APP-SDK en ondersteunt al het bijbehorende beleid voor gegevensbeveiliging, behalve het verhinderen van:<br><ul><li>**Opslaan-als**: de Microsoft Edge-browser staat niet toe dat een gebruiker directe, in-app-verbindingen naar cloudopslagproviders (zoals OneDrive) toevoegt.</li><li>**Contactpersonen synchroniseren**: de Microsoft Edge-browser slaat de systeemeigen lijsten met contactpersonen niet op.</li></ul>**Opmerking:** *Er kan met de APP-SDK niet worden bepaald of een doel-app een browser is. Op Android-apparaten zijn andere beheerde browser-apps die de http/https-intentie ondersteunen, toegestaan.* | **Niet geconfigureerd** |
|<ul><ui>**Niet-beheerde browser-id** | Voer de toepassings-id voor één browser in. Webinhoud (http-/https-koppelingen) van door beleid beheerde toepassingen wordt geopend in de opgegeven browser.  De webinhoud blijft niet-beheerd in de doelbrowser. | **Leeg** |
|<ul><ui>**Naam van niet-beheerde browser** | Voer de toepassingsnaam voor de browser in die is gekoppeld aan de **niet-beheerde browser-id**. De naam wordt aan gebruikers weergegeven als de opgegeven browser niet is geïnstalleerd.  | **Leeg** |
| **Meldingen van organisatiegegevens** | Geef op hoeveel organisatiegegevens worden gedeeld via besturingssysteemmeldingen voor organisatieaccounts. Deze beleidsinstelling is van invloed op het lokale apparaat en alle verbonden apparaten, zoals draagbare apparaten en slimme luidsprekers. Met apps kunnen extra besturingselementen worden geboden om het gedrag van meldingen aan te passen. Er kan ook voor worden gekozen om niet aan alle waarden te voldoen. Kies uit: <ul><li>**Blokkeren**:  Geen meldingen delen.</li><ul><li>Als dit niet wordt ondersteund door de toepassing, worden meldingen toegestaan.</li></ul><li>**Organisatiegegevens blokkeren**: Deel geen organisatiegegevens in meldingen. Bijvoorbeeld 'U hebt nieuwe e-mail'; 'U hebt een vergadering'</li><UL><li>Als dit niet wordt ondersteund door de toepassing, worden meldingen geblokkeerd.</li></ul><li>**Toestaan**: Hiermee worden organisatiegegevens in de meldingen gedeeld</li></ul> <p>**Opmerking**: *Deze instelling vereist ondersteuning van apps. Op dit moment wordt deze instelling ondersteund in Outlook voor Android 4.95.0 of hoger, die naar verwachting wordt uitgebracht in de week van 16 december 2019.* | **Toestaan**   |

## <a name="data-transfer-exemptions"></a>Uitzonderingen voor gegevensoverdracht

Er zijn een aantal uitzonderingsapps en -platformservices waar Intune beveiligingsbeleidsregels voor apps gegevensoverdracht naar en van kan toestaan. Alle door Intune beheerde apps voor Android moeten bijvoorbeeld gegevens kunnen overdragen van en naar Google Text-to-Speech, zodat tekst op het scherm van uw mobiele apparaat hardop kan worden voorgelezen. Deze lijst kan worden gewijzigd en reflecteert de services en apps die als nuttig voor beveiligde productiviteit worden beschouwd.

### <a name="full-exemptions"></a>Volledige uitzonderingen

  Voor deze apps en services is gegevensoverdracht naar en van Intune-beheerde apps volledig toegestaan.

  |Naam van app/service | Beschrijving |
  | ------ | ---- |
  | com.android.phone | Systeemeigen phone-app
  | com.android.vending | Google Play Store |
  | com.android.documentsui | Android Documentkiezer|
  | com.google.android.webview | [WebView](https://developer.android.com/reference/android/webkit/WebView.html), dat nodig is voor veel apps, inclusief Outlook. |
  | com.android.webview |[WebView](https://developer.android.com/reference/android/webkit/WebView.html), dat nodig is voor veel apps, inclusief Outlook.|
  | com.google.android.tts | Google Tekst naar spraak |
  | com.android.providers.settings | Systeeminstellingen Android |
  | com.android.settings | Systeeminstellingen Android |
  | com.azure.authenticator | Azure Authenticator-app, die vereist is voor een geslaagde verificatie in veel scenario's. |
  | com.microsoft.windowsintune.companyportal | Intune Bedrijfsportal|

### <a name="conditional-exemptions"></a>Voorwaardelijke uitzonderingen
  Voor deze apps en services is gegevensoverdracht naar en van Intune-beheerde apps onder bepaalde voorwaarden toegestaan.

  |Naam van app/service | Beschrijving | Uitzonderingsvoorwaarde|
  | ------ | ---- | --- |
  | com.android.chrome | Google Chrome-browser | Chrome wordt gebruikt voor een aantal webweergaveonderdelen op Android 7.0+ en is nooit verborgen. Gegevensstroom naar en van de app is echter altijd beperkt.  |
  | com.skype.raider | Skype | Voor de Skype-app zijn alleen bepaalde acties die in een telefonische oproep resulteren toegestaan. |
  | com.android.providers.media | Android media-inhoudsprovider | Voor de media-inhoudsprovider is alleen de actie beltoonselectie toegestaan. |
  | com.google.android.gms; com.google.android.gsf | Google Play-Services-pakketten | Voor deze pakketten zijn Google Cloud Messaging-acties zoals pushmeldingen toegestaan. |
  | com.google.android.apps.maps | Google Maps | Adressen zijn toegestaan voor navigatie |

Zie [Uitzonderingen voor gegevensoverdrachtsbeleid voor apps](app-protection-policies-exception.md) voor meer informatie.

## <a name="access-requirements"></a>Vereisten voor toegang

| Instelling | Gebruik |  
|------|------| 
| **Pincode voor toegang** | Selecteer **Vereisen** om een pincode te vereisen voor gebruik van deze app. De eerste keer dat de gebruiker de app uitvoert in een aan werk of school gerelateerde context, wordt de gebruiker gevraagd deze pincode in te stellen. <br><br> Standaardwaarde = **Vereisen**<br><br> U kunt de sterkte van de pincode configureren met behulp van de instellingen die beschikbaar zijn in het gedeelte Pincode voor toegang.
| <ul><ui>**Pincodetype** | stel een vereiste in voor een numerieke pincode of wachtwoordcode voordat een app wordt gebruikt waarop app-beveiligingsbeleid is toegepast. Bij een numerieke pincode hoeven alleen cijfers worden ingevoerd, terwijl bij een wachtwoordcode kan worden vereist dat ten minste één letter uit het alfabet **of** ten minste één speciaal teken wordt ingevoerd. <br><br> Standaardwaarde = **Numeriek**<br><br> **Opmerking:** de toegestane speciale tekens zijn de speciale tekens en symbolen op het Engelse toetsenbord van Android. |
| <ul><ui> **Eenvoudige pincode** | Selecteer **Toestaan** als gebruikers eenvoudige pincodes mogen gebruiken, zoals *1234*, *1111*, *abcd* of *aaaa*. Selecteer **Blokkeringen** als ze geen eenvoudige tekenreeksen mogen gebruiken. Eenvoudige reeksen worden gecontroleerd in sliding windows van drie tekens. Als **Blokkeren** is geconfigureerd, wordt 1235 of 1112 niet geaccepteerd als door de eindgebruiker ingestelde pincode, maar 1122 bijvoorbeeld wel. <br><br>Standaardwaarde = **Toestaan** <br><br>**Opmerking:** Als de pincode als wachtwoordcode is geconfigureerd en Eenvoudige pincode toestaan is ingesteld op Ja, moet de gebruiker voor de pincode ten minste één letter **of** ten minste één speciaal teken gebruiken. Als de pincode als wachtwoordcode is geconfigureerd en Eenvoudige pincode toestaan is ingesteld op Nee, moet de gebruiker in de pincode ten minste één cijfer **en** één letter, **en** ten minste één speciaal teken gebruiken. </li> |
| <ul><ui> **Minimale lengte pincode selecteren** | geef het minimale aantal cijfers op waaruit een pincode moet bestaan. <br><br>Standaardwaarde = **4** | 
| <ul><ui> **Vingerafdruk in plaats van pincode voor toegang (Android 6.0+)** | Selecteer **Toestaan** als de gebruiker in plaats van een pincode [vingerafdrukverificatie](https://developer.android.com/about/versions/marshmallow/android-6.0.html#fingerprint-authentication) mag gebruiken voor toegang tot apps. <br><br>Standaardwaarde = **Toestaan** <br><br>**Opmerking:** deze functie biedt ondersteuning voor algemene controles voor biometrie op Android-apparaten. OEM-specifieke biometrische instellingen, zoals Samsung Pass, *worden niet ondersteund.* <br><br>In Android kunt u gebruikers hun identiteit laten aantonen met behulp van [Android-vingerafdrukverificatie](https://developer.android.com/about/versions/marshmallow/android-6.0.html#fingerprint-authentication) in plaats van een pincode. Wanneer gebruikers deze app proberen te gebruiken via hun werk- of schoolaccount, moeten ze hun vingerafdruk in plaats van een pincode gebruiken. <br><br> Ingeschreven apparaten met Android-werkprofielen vereisen dat er een afzonderlijke vingerafdruk wordt geregistreerd voor het beleid **Vingerafdruk in plaats van een pincode voor toegang**. Dit beleid geldt alleen voor door beleid beheerde apps die zijn geïnstalleerd in het Android-werkprofiel. U moet de afzonderlijke vingerafdruk registreren op het apparaat nadat het Android-werkprofiel is gemaakt door het in te schrijven in de bedrijfsportal. Zie [Uw werkprofiel vergrendelen](https://support.google.com/work/android/answer/7029958) voor meer informatie over werkprofielvingerafdrukken met Android-werkprofielen. |
| <ul><ui>**Vingerafdruk overschrijven met pincode na time-out**| Als u deze instelling wilt gebruiken, selecteert u **Vereisen**en configureert u vervolgens een time-out bij inactiviteit. <br><br>Standaardwaarde = **Vereisen** |
| <ul><ui><ul><ui> **Time-out (minuten van inactiviteit)**| Geef de tijd in minuten op waarna het gebruik van een vingerafdruk wordt overschreven door een wachtwoordcode of numerieke pincode (zoals geconfigureerd). Deze time-outwaarde moet groter zijn dan de waarde die onder 'Toegangsvereisten opnieuw controleren na (minuten van inactiviteit)' is opgegeven.<br><br>Standaardwaarde = **30** |
| <ul><ui>**Pincode na aantal dagen opnieuw instellen** | Selecteer **Ja** om te vereisen dat gebruikers hun pincode wijzigen na een bepaalde periode, in dagen.  <br><br>Wanneer dit is ingesteld op *Ja*, kunt u vervolgens het aantal dagen configureren voordat de pincode opnieuw moet worden ingesteld. <br><br> Standaardwaarde = **Nee**  |  
| <ul><ui><ul><ui> **Aantal dagen** | Configureer het aantal dagen voordat de pincode opnieuw moet worden ingesteld. <br><br> Standaardwaarde = **90**  |
| <ul><ui>**Aantal vorige pincodes selecteren dat moet worden onderhouden** | Met deze instelling bepaalt u het aantal eerdere pincodes dat door Intune wordt onderhouden. Nieuwe pincodes moeten verschillen van de pincodes die door Intune worden onderhouden. <br><br> Standaardwaarde = **0** | 
| <ul><ui>**Pincode voor de app wanneer een apparaatpincode is ingesteld** | Selecteer **Niet vereist** om de pincode voor het apparaat uit te schakelen wanneer een apparaatvergrendeling wordt gedetecteerd op een geregistreerd apparaat waarop Bedrijfsportal is geconfigureerd. <br><br> Standaardwaarde = **Vereisen**. | 
| **Aanmeldingsgegevens voor werk-of schoolaccount voor toegang** | Selecteer **Vereisen** om te vereisen dat gebruikers zich aanmelden met hun werk- of schoolaccount in plaats van een pincode voor toegang tot apps. Als deze optie is ingesteld op **Vereisen** en prompts voor pincode of biometrische gegevens zijn ingeschakeld, worden zowel de bedrijfsreferenties als de prompts voor de pincode of biometrische gegevens weergegeven. <br><br>Standaardwaarde = **Niet vereist** |
| **Toegangsvereisten opnieuw controleren na (minuten van inactiviteit)** | Configureer de volgende instellingen: <ul><li>**Time-out**: het aantal minuten waarna de (eerder in het beleid gedefinieerde) toegangsvereisten opnieuw worden gecontroleerd. Voorbeeld: een beheerder schakelt de functie voor pincodes in en blokkeert geroote apparaten in het beleid. Een gebruiker opent een door Intune beheerde app, moet een pincode invoeren, en moet de app gebruiken op een niet-geroot apparaat. Wanneer u deze instelling gebruikt, hoeft de gebruiker geen pincode in te voeren of nog een controle op rootdetectie te ondergaan in een door Intune beheerde app zolang de geconfigureerde tijdsduur nog niet is verstreken.  <br><br>Deze indeling voor beleidsinstelling ondersteunt een positief geheel getal. <br><br> Standaardwaarde = **30 minuten** <br><br> **Opmerking:** in Android wordt de pincode gedeeld tussen alle apps die in Intune worden beheerd. De timer van de pincode wordt opnieuw ingesteld zodra de app de voorgrond van het apparaat verlaat. De gebruiker hoeft voor de duur van de time-out die is opgegeven met deze instelling, geen pincode in te voeren voor een in Intune beheerde app die de pincode deelt. <br><br></li> |

> [!NOTE]  
> Zie [Veelgestelde vragen over Intune MAM](mam-faq.md) en [Selectively wipe data using app protection policy access actions in Intune](app-protection-policies-access-actions.md) (Gegevens selectief wissen met toegangsacties van beveiligingsbeleid voor apps in Intune) voor meer informatie over hoe meerdere Intune-app-beveiligingsinstellingen die in de sectie Toegang worden geconfigureerd, kunnen worden toegepast op dezelfde reeks apps en gebruikers in Android.


## <a name="conditional-launch"></a>Voorwaardelijk starten
Configureer instellingen voor voorwaardelijk starten om beveiligingsvereisten voor aanmelden in te stellen voor toegangsbeveiligingsbeleid. 

Standaard wordt voorzien in verschillende instellingen met vooraf geconfigureerde waarden en acties. Enkele van deze instellingen kunt u verwijderen, bijvoorbeeld de *minimale versie van het besturingssysteem*. U kunt ook aanvullende instellingen selecteren in de vervolgkeuzelijst **Selecteer een optie**. 

| Instelling | Gebruik |  
|---------|------------| 
| **Maximum aantal pincodepogingen** | Geef het aantal pogingen voor de gebruiker op om de pincode in te voeren voordat de geconfigureerde actie wordt uitgevoerd. Deze indeling voor beleidsinstelling ondersteunt een positief geheel getal. De *acties* omvatten: <br><ul><li>**Pincode opnieuw instellen**: de gebruiker moet de pincode opnieuw instellen.</li></ul> <ul><li>**Gegevens wissen**: het gebruikersaccount dat is gekoppeld aan de toepassing wordt van het apparaat gewist.  </li></ul> </li></ul> Standaardwaarde = **5** |
| **Offlinerespijtperiode** | Het aantal minuten dat MAM-apps offline kunnen worden uitgevoerd. Geef de periode (in minuten) op waarna de toegangsvereisten voor de app opnieuw worden gecontroleerd. De *acties* omvatten: <br><ul><li>**Toegang blokkeren (minuten)** : het aantal minuten dat MAM-apps offline kunnen worden uitgevoerd. Geef de periode (in minuten) op waarna de toegangsvereisten voor de app opnieuw worden gecontroleerd. Nadat deze periode is verlopen, vereist de app gebruikersverificatie met Azure Active Directory (Azure AD), zodat de app kan worden uitgevoerd. <br><br>Deze indeling voor beleidsinstelling ondersteunt een positief geheel getal. <br><br>Standaardwaarde = **720** minuten (12 uur) </li></ul> <ul><li>**Gegevens wissen (dagen)** : nadat de app het ingestelde (door de beheerder bepaalde) aantal dagen offline is uitgevoerd, moet de gebruiker verbinding maken met het netwerk en opnieuw gebruikersverificatie uitvoeren. Als de gebruiker is geverifieerd, kan deze opnieuw toegang krijgen tot de gegevens en wordt het offline-interval opnieuw ingesteld.  Als de gebruiker niet kan worden geverifieerd, worden het gebruikersaccount en de gebruikersgegevens selectief gewist. Zie [Alleen zakelijke gegevens wissen uit door Intune beheerde apps](apps-selective-wipe.md) voor meer informatie.</li></ul> Deze indeling voor beleidsinstelling ondersteunt een positief geheel getal. <br><br>  Standaardwaarde = **90 dagen** </li></ul> <br><br>  Dit item kan meerdere keren worden weergegeven. Elk exemplaar ondersteunt een andere actie. |   
| **Apparaten die zijn opengebroken of geroot** |Voor deze instelling kan geen waarde worden ingesteld. De *acties* omvatten: <br><ul><li>**Toegang blokkeren**: voorkom dat deze app wordt uitgevoerd op apparaten die zijn opengebroken of geroot. De gebruiker kan deze app nog steeds gebruiken voor privétaken maar moet voor het openen van werk- of schoolgegevens in deze app een ander apparaat gebruiken.</li></ul> <ul><li>**Gegevens wissen**: het gebruikersaccount dat is gekoppeld aan de toepassing wordt van het apparaat gewist.  </li></ul> |
| **Minimale versie van het besturingssysteem** | Geef de minimumversie van het Android-besturingssysteem op die is vereist voor het gebruik van deze app. De *acties* omvatten: <br><ul><li>**Waarschuwen**: de gebruiker ziet een melding als de Android-versie op het apparaat niet aan de vereiste voldoet. De gebruiker kan deze melding negeren.  </li></ul> <ul><li>**Toegang blokkeren**: toegang door de gebruiker wordt geblokkeerd als de Android-versie op het apparaat niet aan de vereiste voldoet.</li></ul> <ul><li>**Gegevens wissen**: het gebruikersaccount dat is gekoppeld aan de toepassing wordt van het apparaat gewist.  </li></ul> </li></ul>Deze indeling voor beleidsinstelling ondersteunt major.minor, major.minor.build en major.minor.build.revision. |
| **Minimale versie van de app** | Geef een waarde op voor de minimale versie van het besturingssysteem. De *acties* omvatten: <br><ul><li>**Waarschuwen**: de gebruiker ziet een melding als de app-versie op het apparaat niet aan de vereiste voldoet. De gebruiker kan deze melding negeren.</li></ul> <ul><li>**Toegang blokkeren**: toegang door de gebruiker wordt geblokkeerd als de app-versie op het apparaat niet aan de vereiste voldoet. </li></ul> <ul><li>**Gegevens wissen**: het gebruikersaccount dat is gekoppeld aan de toepassing wordt van het apparaat gewist. </li></ul> </li></ul> Omdat apps vaak verschillende versieschema's hebben, moet u een beleid maken waarin minimaal één app-versie gericht is op één app (bijvoorbeeld *Outlook-versiebeleid*).<br><br> Dit item kan meerdere keren worden weergegeven. Elk exemplaar ondersteunt een andere actie.<br><br> Deze indeling voor beleidsinstelling ondersteunt major.minor, major.minor.build en major.minor.build.revision.<br><br> Daarnaast kunt u configureren **waar** uw eindgebruikers een bijgewerkte versie van een LOB-app (Line-Of-Business) kunnen downloaden. Eindgebruikers zien deze functie in het dialoogvenster **Minimale app-versie** van de functie voor voorwaardelijk starten, waarin eindgebruikers wordt gevraagd om bij te werken naar een minimale versie van de LOB-app. In Android maakt deze functie gebruik van de bedrijfsportal. Als u wilt configureren waar een eindgebruiker een LOB-app kan bijwerken, moet er [configuratiebeleid voor beheerde apps](app-configuration-policies-managed-app.md) worden verzonden naar de app met de sleutel `com.microsoft.intune.myappstore`. De verzonden waarde bepaalt uit welke store de eindgebruiker de app kan downloaden. Als de app wordt geïmplementeerd via de Bedrijfsportal-app, moet de waarde `CompanyPortal` zijn. Voor iedere andere store moet u een volledige URL opgeven. |
| **Minimale patchversie** | Vereis dat apparaten beschikken over een minimale Android-beveiligingspatch die is vrijgegeven door Google.  <br><ul><li>**Waarschuwen**: de gebruiker ziet een melding als de Android-versie op het apparaat niet aan de vereiste voldoet. De gebruiker kan deze melding negeren.  </li></ul> <ul><li>**Toegang blokkeren**: toegang door de gebruiker wordt geblokkeerd als de Android-versie op het apparaat niet aan de vereiste voldoet.</li></ul> <ul><li>**Gegevens wissen**: het gebruikersaccount dat is gekoppeld aan de toepassing wordt van het apparaat gewist.  </li></ul></li></ul> Deze beleidsinstelling ondersteunt de datumnotatie *JJJJ-MM-DD*. |
| **Apparaatfabrikant(en)** | Geef een door puntkomma's gescheiden lijst met fabrikant(en) op. Deze waarden zijn niet hoofdlettergevoelig. De *acties* omvatten: <br><ul><li>**Opgegeven toestaan (niet opgegeven blokkeren)** : de app kan alleen worden uitgevoerd op apparaten van de opgegeven fabrikant. Alle andere apparaten worden geblokkeerd. </li></ul> <ul><li>**Opgegeven toestaan (niet opgegeven wissen)** : het gebruikersaccount dat is gekoppeld aan de toepassing wordt van het apparaat gewist. </li></ul> Zie [Acties voor voorwaardelijk starten](app-protection-policies-access-actions.md#android-policy-settings) voor meer informatie over het gebruik van deze instelling. |
| **SafetyNet-attestation voor apparaat** | App-beveiligingsbeleid biedt ondersteuning voor API's van Google Play Protect. Met name via deze instelling wordt SafetyNet-Attestation van Google op apparaten van de eindgebruiker geconfigureerd. Geef **basisintegriteit** of **basisintegriteit en gecertificeerde apparaten** op. **Basisintegriteit** biedt informatie over de algemene integriteit van het apparaat. Basisintegriteit werkt niet met geroote apparaten, emulators, virtuele apparaten en apparaten met sporen van manipulatie. **Basisintegriteit en gecertificeerde apparaten** biedt informatie over de compatibiliteit van het apparaat met services van Google. Alleen ongewijzigde apparaten die zijn gecertificeerd door Google kunnen deze controle doorstaan. De *acties* omvatten: <br><ul><li>**Waarschuwen**: de gebruiker ziet een melding als het apparaat niet aan de door SafetyNet-Attestation van Google uitgevoerde scan voldoet op basis van de waarde die is geconfigureerd. De gebruiker kan deze melding negeren. </li></ul><ul><li>**Toegang blokkeren**: de toegang wordt geblokkeerd voor de gebruiker als het apparaat niet aan de door SafetyNet-Attestation van Google uitgevoerde scan voldoet op basis van de waarde die is geconfigureerd. </li></ul> <ul><li>**Gegevens wissen**: het gebruikersaccount dat is gekoppeld aan de toepassing wordt van het apparaat gewist. </li></ul> </li></ul> Zie [Veelgestelde vragen over MAM en app-beveiliging](mam-faq.md#app-experience-on-android) voor antwoorden op veelgestelde vragen met betrekking tot deze instelling. |
| **Bedreigingsscan voor apps vereisen** | App-beveiligingsbeleid biedt ondersteuning voor API's van Google Play Protect. Met name deze instelling zorgt ervoor dat de scanfunctie voor Apps controleren van Google wordt ingeschakeld voor eindgebruikerapparaten. Als deze instelling is geconfigureerd, wordt de toegang geblokkeerd voor de eindgebruiker zolang deze de scanfunctie van Google voor apps niet inschakelt op het Android-apparaat. De *acties* omvatten: <br><ul><li>**Waarschuwen**: de gebruiker ziet een melding als de scanfunctie voor Apps controleren van Google niet is ingeschakeld op het apparaat. De gebruiker kan deze melding negeren. </li></ul><ul><li>**Toegang blokkeren**: de toegang wordt geblokkeerd voor de gebruiker als de scanfunctie voor Apps controleren van Google niet is ingeschakeld op het apparaat. </li></ul></li></ul> Resultaten van de scanfunctie voor Apps controleren van Google worden weergegeven in het rapport **Potentieel schadelijke Apps** in de console. |
| **Minimale versie bedrijfsportal** | Met behulp van **Minimale versie bedrijfsportal** kunt u een minimale versie van de bedrijfsportal opgeven die wordt afgedwongen op het apparaat van de eindgebruiker. Met deze instelling voor voorwaardelijk starten kunt u waarden instellen voor **Toegang blokkeren**, **Gegevens wissen** en **Waarschuwen** als mogelijke acties wanneer niet aan alle waarden wordt voldaan. De mogelijke notaties voor deze waarde volgen het patroon *[major].[minor]* , *[major].[minor].[build]* , of *[major].[minor].[build].[revision]* . Omdat sommige eindgebruikers mogelijk niet de voorkeur geven aan een geforceerde update van apps, kan de optie 'Waarschuwen' ideaal zijn bij het configureren van deze instelling. De Google Play Store is zo slim om alleen de verschilgegevens voor app-updates te verzenden. Dit kan echter nog steeds een grote hoeveelheid gegevens zijn die gebruikers mogelijk niet willen aannemen, bijvoorbeeld als ze op het moment van de update via mobiele gegevens werken. Het afdwingen van een update en het downloaden van een bijgewerkte app kan leiden tot onverwachte gegevenskosten op het moment van de update. Zie [Beleidsinstellingen voor Android](app-protection-policies-access-actions.md#android-policy-settings) voor meer informatie. |
| **Maximaal toegestaan bedreigingsniveau van apparaat** | App-beveiligingsbeleid kan gebruikmaken van de Intune-MTD-connector. Geef een maximaal bedreigingsniveau op dat is toegestaan voor het gebruik van deze app. Bedreigingen worden bepaald door de gekozen leverancier-app voor Mobile Threat Defense (MTD) op het apparaat van de eindgebruiker. Geef *Beveiligd*, *Laag*, *Gemiddeld* of *Hoog* op. Voor *Beveiligd* zijn geen bedreigingen op het apparaat vereist en het is de meest beperkende configureerbare waarde, terwijl voor *Hoog* in feite een actieve Intune-naar-MTD-verbinding vereist is. De *acties* omvatten: <br><ul><li>**Toegang blokkeren**: toegang wordt voor de gebruiker geblokkeerd als het bedreigingsniveau dat wordt bepaald door de gekozen toepassing voor Mobile Threat Defense (MTD) op het apparaat van de eindgebruiker niet aan deze vereiste voldoet.</li></ul> <ul><li>**Gegevens wissen**: het gebruikersaccount dat is gekoppeld aan de toepassing wordt van het apparaat gewist.</li></ul>Voor meer informatie over het gebruik van deze instelling, zie [Mobile Threat Defense-connector in Intune inschakelen voor niet-ingeschreven apparaten](../protect/mtd-enable-unenrolled-devices.md). |