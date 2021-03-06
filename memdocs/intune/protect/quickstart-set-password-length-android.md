---
title: 'Snelstart: Beleid voor wachtwoordcompatibiliteit voor Android-apparaten'
titleSuffix: Microsoft Intune
description: In deze quickstart gaat u Microsoft Intune gebruiken om de wachtwoordlengte in te stellen die is vereist voor Android-apparaten.
keywords: ''
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 11/21/2019
ms.topic: quickstart
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: high
ms.technology: ''
ms.assetid: 81b4fa08-5333-4c54-9f49-8db5f6984ed2
ms.reviewer: ''
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2e2d5fb2f698d7e0b544dbdbd4ab05f2b94b7ea
ms.sourcegitcommit: 7f17d6eb9dd41b031a6af4148863d2ffc4f49551
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "80325462"
---
# <a name="quickstart-create-a-password-compliance-policy-for-android-devices"></a>Snelstartgids: Een beleid voor wachtwoordcompatibiliteit maken voor Android-apparaten

In deze quickstart gaat u Microsoft Intune gebruiken om de gebruikers van Android onder uw medewerkers te verplichten een wachtwoord van een voorgeschreven lengte in te voeren om toegang te krijgen tot informatie op hun Android-apparaten.

Een Intune-beleid voor apparaatcompatibiliteit bepaalt de regels en instellingen waaraan apparaten moeten voldoen om als compatibel te worden beschouwd. U kunt compatibiliteitsbeleid met voorwaardelijke toegang gebruiken om toegang tot bedrijfsresources toe te staan of te blokkeren. U kunt ook apparaatrapporten krijgen en maatregelen nemen voor niet-naleving.

> [!IMPORTANT]
> Naast wachtwoordinstellingen moet u ook overwegen andere systeembeveiligingsinstellingen te gebruiken om uw medewerkers te beveiligen. Zie [Systeembeveiligingsinstellingen](compliance-policy-create-android-for-work.md) voor meer informatie.

Als u niet over een Intune-abonnement beschikt, kunt u [zich registreren voor een gratis proefaccount](../fundamentals/free-trial-sign-up.md).

## <a name="sign-in-to-intune"></a>Aanmelden bij Intune

Meld u aan bij het [Microsoft Endpoint Manager-beheercentrum](https://go.microsoft.com/fwlink/?linkid=2109431) als een [globale beheerder](../fundamentals/users-add.md#types-of-administrators) of als een [servicebeheerder](../fundamentals/users-add.md#types-of-administrators) voor Intune.

## <a name="create-a-device-compliance-policy"></a>Een nalevingsbeleid voor apparaten maken

Maak een nalevingsbeleid voor apparaten om uw medewerkers die Android gebruiken, te verplichten een wachtwoord van een voorgeschreven lengte in te voeren om toegang te krijgen tot informatie op hun Android-apparaten.

1. Selecteer in Intune **Apparaten** > **Nalevingsbeleid** > **Beleid maken**.

2. Voeg **Android-naleving** toe als **naam**. Voeg ook een **beschrijving** toe.

3. Selecteer voor **Platform** de optie **Android Enterprise**.

4. Selecteer voor **Profieltype** **Werkprofiel**.

5. Selecteer **Instellingen** > **Systeembeveiliging** om de Android-blade **Systeembeveiliging** weer te geven.

6. Voor **Wachtwoord vereisen voor het ontgrendelen van mobiele apparaten** selecteert u **Vereisen**.

7. Voor **Vereist wachtwoordtype** selecteert u **Ten minste numeriek**.

8. Voor **Minimale wachtwoordlengte** voert u **6** in.

    ![Schermopname van het maken van een groep in Microsoft Intune](./media/quickstart-set-password-length-android/quickstart-set-password-length-android-01.png)

9. Wanneer u klaar bent, selecteert u **OK** > **OK** > **Maken** om het beleid te maken.

Als het beleid is gemaakt, wordt dit weergegeven in de lijst Apparaatnaleving.

## <a name="clean-up-resources"></a>Resources opschonen

Als u het beleid niet meer nodig hebt, kunt u het verwijderen. Hiervoor selecteert u het compatibiliteitsbeleid en klikt u op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u Intune gebruikt om een nalevingsbeleid te maken voor de Android-apparaten van uw werknemers om een wachtwoord van ten minste zes tekens lang te vereisen. Zie [Aan de slag met compatibiliteitsbeleid voor apparaten in Intune](device-compliance-get-started.md) voor meer informatie over het maken van compatibiliteitsbeleid voor apparaten.

Ga door naar de volgende snelstartgids om deze serie met snelstartgidsen voor Intune te volgen.

> [!div class="nextstepaction"]
> [Snelstartgids: Meldingen verzenden naar niet-compatibele apparaten](quickstart-send-notification.md)
