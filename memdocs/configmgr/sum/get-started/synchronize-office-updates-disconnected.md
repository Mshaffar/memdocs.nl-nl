---
title: Office 365-updates synchroniseren zonder Internet verbinding
titleSuffix: Configuration Manager
description: Synchroniseer Office 365-updates op het hoogste niveau van het software-update punt dat niet is verbonden met internet.
ms.date: 04/21/2020
ms.topic: conceptual
ms.prod: configuration-manager
ms.technology: configmgr-sum
ms.assetid: a8fa7e7a-bf55-42de-b0c2-c56777dc1508
manager: dougeby
author: mestew
ms.author: mstewart
ms.openlocfilehash: 3627d2f7772b7b9e133d742b0ee4f94dba6e457a
ms.sourcegitcommit: 2cafbba6073edca555594deb99ae29e79cd0bc79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2020
ms.locfileid: "82110352"
---
# <a name="synchronize-office-365-updates-from-a-disconnected-software-update-point"></a><a name="bkmk_O365"></a>Office 365-updates synchroniseren vanaf een niet-verbonden software-update punt

*Van toepassing op: Configuration Manager (huidige vertakking)*
<!--4065163-->
Vanaf Configuration Manager versie 2002 kunt u met een hulp programma Office 365-updates importeren van een met internet verbonden WSUS-server naar een niet-verbonden Configuration Manager omgeving. Als u eerder meta gegevens voor software die is bijgewerkt in omgevingen zonder verbinding hebt geëxporteerd en geïmporteerd, kunt u geen Office 365-updates implementeren. Office 365-updates vereisen aanvullende meta gegevens die zijn gedownload van een Office-API en het Office CDN, wat niet mogelijk is met niet-verbonden omgevingen.

> [!Note]
> Vanaf 21 april 2020, wordt de naam van Office 365 ProPlus gewijzigd in **Microsoft 365 apps voor bedrijven**. Zie [name wijzigen voor Office 365 ProPlus](https://docs.microsoft.com/deployoffice/name-change)voor meer informatie. Mogelijk ziet u nog steeds verwijzingen naar de oude naam in de Configuration Manager-console en de ondersteunende documentatie terwijl de-console wordt bijgewerkt.

## <a name="prerequisites"></a>Vereisten

- Een met internet verbonden WSUS-server met mini maal Windows Server 2012.
- De WSUS-server moet verbinding hebben met deze twee Internet eindpunten:
   - `officecdn.microsoft.com`
   - `config.office.com`
- Kopieer het hulp programma OfflineUpdateExporter en de afhankelijkheden ervan naar het Internet Connected WSUS-server.
  - Het hulp programma en de bijbehorende afhankelijkheden bevinden zich in de ** &lt;ConfigMgrInstallDir>/tools/offlineupdateexporter** -map.
- De gebruiker die het hulp programma uitvoert, moet deel uitmaken van de groep **WSUS-Administrators** .
- De map die is gemaakt voor het opslaan van de meta gegevens en inhoud van Office update moet de juiste toegangs beheer lijsten (Acl's) hebben om de bestanden te beveiligen.
    - Deze map moet ook leeg zijn.
- Gegevens die van de online-WSUS-server naar de niet-verbonden omgeving worden verplaatst, moeten veilig worden verplaatst.

> [!IMPORTANT]
> Inhoud wordt gedownload voor alle Office 365-talen. Elke update kan ongeveer 10 GB aan inhoud hebben.

## <a name="synchronize-then-decline-unneeded-office-365-updates"></a>Niet-benodigde Office 365-updates afwijzen

1. Open de WSUS-console op uw Internet verbinding met WSUS.
1. Selecteer **Opties** en vervolgens op **producten en classificaties**.
1. Op het tabblad **producten** selecteert u **Office 365-client** en selecteert u **updates** op het tabblad **classificaties** . [ ![producten en classificaties voor Office 365-updates in WSUS](./media/4065163-o365-updates-product-classification.png)](./media/4065163-o365-updates-product-classification.png#lightbox)
1. Ga naar **synchronisaties** en selecteer **Nu synchroniseren** om de Office 365-updates in WSUS op te halen.
1. Wanneer de synchronisatie is voltooid, weigert u alle Office 365-updates die u niet wilt implementeren met Configuration Manager. U hoeft Office 365-updates niet goed te keuren om ze te kunnen downloaden.  
   - Als u ongewenste Office 365-updates in WSUS afwijst, worden deze niet gestopt tijdens het exporteren van WsusUtil. exe, maar wordt het OfflineUpdateExporter-hulp programma niet meer gebruikt om de inhoud te downloaden.
   - Het OfflineUpdateExporter-hulp programma downloadt Office 365-updates voor u. Andere producten moeten nog steeds worden goedgekeurd om te worden gedownload als u updates voor hen wilt exporteren.
    - Maak een [nieuwe update weergave in WSUS](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/manage/viewing-and-managing-updates#to-create-a-new-update-view-on-wsus) om onnodige Office 365-updates in WSUS te bekijken en af te wijzen.
1. Als u andere product updates wilt goed keuren voor downloaden en exporteren, wacht u totdat de inhoud is gedownload voordat u WsusUtil. exe uitvoert en de inhoud van de map WSUSContent exporteert. Zie [software-updates synchroniseren vanaf een niet-verbonden software-update punt](synchronize-software-updates-disconnected.md) voor meer informatie

## <a name="exporting-the-office-365-updates"></a>De Office 365-updates exporteren

1. Kopieer de map OfflineUpdateExporter van Configuration Manager naar het Internet Connected WSUS-server.
    - Het hulp programma en de bijbehorende afhankelijkheden bevinden zich in de ** &lt;ConfigMgrInstallDir>/tools/offlineupdateexporter** -map.
1. Voer het hulp programma uit vanaf een opdracht prompt op het Internet Connected WSUS-server met het volgende gebruik: **OfflineUpdateExporter. exe- &lt;O-D doelpad>**

   |OfflineUpdateExporter-para meter|Beschrijving|
   |---|---|
   |**-O**|  **-Office**. Hiermee wordt het product opgegeven voor het exporteren van updates is Office 365|
   |**-D**|**-Bestemming**. Doel is een vereiste para meter en het volledige pad naar de doelmap is nodig.|

   - Het hulp programma **OfflineUpdateExporter** voert de volgende handelingen uit:
      - Maakt verbinding met WSUS
      - Leest de Office 365 update-meta gegevens in WSUS
      - Downloadt de inhoud en eventuele aanvullende meta gegevens die nodig zijn voor de Office 365-updates naar de doelmap

1. Ga in de opdracht prompt op het Internet Connected WSUS-server naar de map die WsusUtil. exe bevat. Het hulp programma bevindt zich standaard in%*Program Files*% \ update Services\Tools. Als het hulpprogramma zich op de standaardlocatie bevindt, typ dan bijvoorbeeld **cd %ProgramFiles%\Update Services\Tools**.
   - Als u Windows Server 2012 gebruikt, moet u ervoor zorgen dat [KB2819484](https://support.microsoft.com/help/2819484/cab-file-that-is-exported-by-using-the-wsusutil-exe-command-is-display) is geïnstalleerd op de WSUS-servers.
   - De gebruiker die het WsusUtil-hulp programma uitvoert, moet lid zijn van de lokale groep Administrators op de server.

1. Typ het volgende om de meta gegevens van software-updates te exporteren naar een GZIP-bestand:  

    *Bestand* **WsusUtil. exe export***pakketnaam*      

    Bijvoorbeeld:  

    **WsusUtil. exe exporteren export. XML. gz export. log**

1. Kopieer het bestand **export. XML. gz** naar de WSUS-server op het hoogste niveau op het niet-verbonden netwerk.
1. Als u updates voor andere producten hebt goedgekeurd, kopieert u de inhoud van de map WSUSContent naar de map WSUSContent van de niet-verbonden WSUS-server.
1. Kopieer de doelmap die wordt gebruikt voor de **OfflineUpdateExporter** naar het hoogste niveau Configuration Manager site server op het niet-verbonden netwerk.

## <a name="import-the-office-365-updates"></a>De Office 365-updates importeren

1. Importeer op de niet-verbonden WSUS-server op het hoogste niveau de meta gegevens van de update uit de **export. XML. gz** die u hebt gegenereerd op de met internet verbonden WSUS-server.
   
    Bijvoorbeeld:  

    **WsusUtil. exe import. XML. gz import. log**
    
    Het hulp programma WsusUtil. exe bevindt zich standaard in%*Program Files*% \ update Services\Tools.

1. Zodra het importeren is voltooid, moet u een eigenschap site besturings element configureren op de niet-verbonden Configuration Manager site server op het hoogste niveau. Deze configuratie wijziging wijst Configuration Manager de inhoud voor Office 365. De configuratie van de eigenschap wijzigen:
   1. Kopieer het [Power shell-script O365OflBaseUrlConfigured](#bkmk_o365_script) naar de niet-verbonden Configuration Manager site server op het hoogste niveau.
   1. Ga `"D:\Office365updates\content"` naar het volledige pad van de gekopieerde map met de Office-inhoud en meta gegevens die zijn gegenereerd door OfflineUpdateExporter.
      > [!IMPORTANT]
      > Alleen lokale paden werken voor de eigenschap O365OflBaseUrlConfigured.
   1. Sla het script op als`O365OflBaseUrlConfigured.ps1`
   1. Voer `.\O365OflBaseUrlConfigured.ps1`vanuit een Power shell-venster met verhoogde bevoegdheden uit op de niet-verbonden Configuration Manager site server op het hoogste niveau.
   1. Start de **SMS_Executive** -service opnieuw op de site server.
1. Ga in de **Configuration Manager** -console naar **beheer** > **site configuratie** > **sites**.
1. Klik met de rechter muisknop op de site op het hoogste niveau en selecteer vervolgens **site onderdelen** > configureren**Software-update punt**.
1. Selecteer *updates*op het tabblad **classificaties** . Op het tabblad **Products** selecteert u *Office 365 client*.
1. [Software-updates synchroniseren](synchronize-software-updates.md#manually-start-software-updates-synchronization) voor Configuration Manager
1. Wanneer de synchronisatie is voltooid, gebruikt u uw normale proces om Office 365-updates te implementeren.

## <a name="proxy-configuration"></a><a name="bkmk_O365_ki"></a>Proxy configuratie

- Proxy configuratie is niet ingebouwd in het hulp programma. Als proxy is ingesteld in de Internet opties op de server waarop het hulp programma wordt uitgevoerd, wordt dit in theorie gebruikt en moet het goed functioneren.
   - Voer `netsh winhttp show proxy` vanaf een opdracht prompt uit om de geconfigureerde proxy weer te geven.



## <a name="modify-o365oflbaseurlconfigured-property"></a><a name="bkmk_o365_script"></a>Eigenschap O365OflBaseUrlConfigured wijzigen

```powershell
# Name: O365OflBaseUrlConfigured.ps1
#
# Description: This sample sets the O365OflBaseUrlConfigured property for the SMS_WSUS_CONFIGURATION_MANAGER component on the top-level site.
# This script must be run on the disconnected top-level Configuration Manager site server
#
# Replace "D:\Office365updates\content" with the full path to the copied directory containing all the Office metadata and content generated by the OfflineUpdateExporter tool.
# Only local paths work for the O365OflBaseUrlConfigured property.

$PropertyValue = "D:\Office365updates\content"

# Don't change any of the lines below
$PropertyName = "O365OflBaseUrlConfigured"

# Get provider instance
$providerMachine = Get-WmiObject -namespace "root\sms" -class "SMS_ProviderLocation"

if($providerMachine -is [system.array])
{
    $providerMachine=$providerMachine[0]
}

$SiteCode = $providerMachine.SiteCode

$component = gwmi -ComputerName $providerMachine.Machine -namespace root\sms\site_$SiteCode -query 'select comp.* from sms_sci_component comp join SMS_SCI_SiteDefinition sdef on sdef.SiteCode=comp.SiteCode where sdef.ParentSiteCode="" and comp.componentname="SMS_WSUS_CONFIGURATION_MANAGER"'
$properties = $component.props

Write-host "Updating $PropertyName property for site " $SiteCode

foreach ($property in $properties)
{
  if ($property.propertyname -eq $PropertyName) 
  {
    Write-host "Current value for $PropertyName is $($property.value2)"
    $property.value2 = $PropertyValue
    Write-host "Updating value for $PropertyName to $($property.value2)"
    break
  }
}

$component.props = $properties
$component.put()
```

## <a name="next-steps"></a>Volgende stappen

[Software-updates toevoegen aan een updategroep](../deploy-use/add-software-updates-to-an-update-group.md)