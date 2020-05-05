---
title: Problemen met CMPivot oplossen
titleSuffix: Configuration Manager
description: Meer informatie over het oplossen van problemen met CMPivot in Configuration Manager.
ms.date: 10/07/2019
ms.prod: configuration-manager
ms.technology: configmgr-core
ms.topic: conceptual
ms.assetid: 36385bea-f05e-4300-947f-cb3927b3bac5
author: mestew
ms.author: mstewart
manager: dougeby
ms.openlocfilehash: 69178f9ac1c1acb1ee2a2931c88a55a0784435b8
ms.sourcegitcommit: bbf820c35414bf2cba356f30fe047c1a34c5384d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "81723507"
---
# <a name="troubleshoot-cmpivot"></a>Problemen met CMPivot oplossen

CMPivot is een hulp programma dat toegang biedt tot een real-time status van de apparaten in uw omgeving. CMPivot voert een query uit op alle momenteel verbonden apparaten in de doel verzameling en retourneert de resultaten.

Het kan voor komen dat u CMPivot moet oplossen. Als een status bericht van een client naar CMPivot bijvoorbeeld beschadigd raakt, kan de site server het bericht niet verwerken. Dit artikel helpt u bij het begrijpen van de informatie stroom voor CMPivot.

## <a name="troubleshoot-cmpivot-in-version-1902-and-later"></a><a name="bkmk_CMPivot-1902"></a>Problemen met CMPivot oplossen in versie 1902 en hoger

In Configuration Manager versies 1902 en hoger kunt u CMPivot uitvoeren vanaf de centrale beheer site (CAS) in een-hiërarchie. De primaire site verwerkt nog steeds de communicatie met de client.

Wanneer u CMPivot uitvoert vanuit CAS, wordt het kanaal voor het abonnements bericht met hoge snelheid gebruikt om te communiceren met de primaire site. CMPivot maakt geen gebruik van standaard SQL-replicatie tussen sites. Als uw SQL Server-exemplaar of uw SQL-provider extern is, of als u SQL Server always on gebruikt, hebt u een ' Double hop scenario ' voor CMPivot. Voor informatie over het definiëren van beperkte delegering voor een ' Double hop scenario ', Zie [CMPivot vanaf versie 1902](cmpivot.md#bkmk_cmpivot1902).

>[!IMPORTANT]
> Als u problemen met CMPivot oplost, schakelt u uitgebreide logboek registratie in op uw beheer punten (MPs) en op de SMS_MESSAGE_PROCESSING_ENGINE van de site server voor meer informatie. Als de uitvoer van de client groter is dan 80 KB, schakelt u uitgebreide logboek registratie in het beheerders-en het SMS_STATE_SYSTEM onderdeel van de site server in. Zie [Opties voor logboek registratie van de site server](../../plan-design/hierarchy/about-log-files.md#bkmk_reg-site)voor meer informatie over het inschakelen van uitgebreide logboek registratie.

### <a name="get-information-from-the-site-server"></a>Informatie ophalen van de site server

De logboek bestanden van de site server bevinden zich standaard `C:\Program Files\Microsoft Configuration Manager\logs`in. Deze locatie kan verschillen als u een niet-standaard installatie directory of Offloaded items hebt opgegeven als de SMS-provider naar een andere server. Als u CMPivot uitvoert vanuit de certificerings instantie, bevinden de logboeken zich op de primaire site server.

`smsprov.log` Zoek naar de volgende regels:

- Configuration Manager versie 1906:
  <pre><code lang="Log">Auditing: User &ltusername> initiated client operation 145 to collection &ltCollectionId>. </code></pre>

- Configuration Manager versie 1902:
  <pre><code lang="Log">Type parameter is 135.
  Auditing: User &ltusername> ran script 7DC6B6F1-E7F6-43C1-96E0-E1D16BC25C14 with hash dc6c2ad05f1bfda88d880c54121c8b5cea6a394282425a88dd4d8714547dc4a2 on collection &ltCollectionId>. </code></pre>

 `7DC6B6F1-E7F6-43C1-96E0-E1D16BC25C14`is de script-GUID voor CMPivot. U kunt deze GUID ook zien in [CMPivot controle status berichten](cmpivot.md#cmpivot-audit-status-messages).

Zoek vervolgens de ID in het venster CMPivot. Deze ID is de `ClientOperationID`.

![CMPivot-venster met ClientOperationID gemarkeerd](media/cmpivot-client-operationid-1902.png)

Zoek de `TaskID` in de tabel ClientAction. De `TaskID` correspondeert met `UniqueID` de in de ClientAction-tabel.

``` SQL
select * from ClientAction where ClientOperationId=<id>
```

Zoek `BgbServer.log`in de `TaskID` bij SQL verzamelde en Let op de `PushID`. De `TaskID` heeft de naam `TaskGUID`. Bijvoorbeeld:

<pre><code lang="Log">Starting to send push task (<b>PushID: 9</b> TaskID: 12 <b>TaskGUID: 9A4E59D2-2F5B-4067-A9FA-B99602A3A4A0</b> TaskType: 15 TaskParam: PFNjcmlwdENvbnRlbnQgU2NyaXB0R3VpZD0nN0RDNkI2RjEtRTdGNi00M0MxL (truncated log entry)
Finished sending push task (<b>PushID: 9</b> TaskID: 12) to 2 clients
</code></pre>

### <a name="client-logs"></a>Client logboeken

Nadat u de informatie van de site server hebt, controleert u de logboeken van de client. De client logboeken bevinden zich standaard in `C:\Windows\CCM\Logs`.

Zoek `CcmNotificationAgent.log`in logboek vermeldingen die eruitzien als de volgende regels:  

<pre><code lang="Log">Receive task from server with <b>pushid=9</b>, taskid=12, <b>taskguid=9A4E59D2-2F5B-4067-A9FA-B99602A3A4A0</b>, tasktype=15 and taskParam=PFNjcmlwdEhhc2ggU2NyaXB0SGF (truncated log entry)
Send Task response message &ltBgbResponseMessage TimeStamp="2019-09-13T17:29:09Z"><b>&ltPushID>5</b>&lt/PushID>&ltTaskID>4&lt/TaskID>&ltReturnCode>1&lt/ReturnCode>&lt/BgbResponseMessage> successfuly.
 </code></pre>

Controleer `Scripts.log` op de `TaskID`. In het volgende voor beeld ziet `Task ID` `{9A4E59D2-2F5B-4067-A9FA-B99602A3A4A0}`u:  

<pre><code lang="Log">Sending script state message (fast): <b>{9A4E59D2-2F5B-4067-A9FA-B99602A3A4A0}</b>
Result are sent for ScriptGuid: 7DC6B6F1-E7F6-43C1-96E0-E1D16BC25C14 and <b>TaskID: {9A4E59D2-2F5B-4067-A9FA-B99602A3A4A0}</b>
</code></pre>

> [!NOTE]
> Als u ' (snel) ' niet ziet in de `Scripts.log`, zijn de gegevens waarschijnlijk meer dan 80 KB. In dit geval wordt de informatie verzonden naar de site server als een status bericht. Gebruik client `StateMessage.log` en de site server `Statesys.log`.

### <a name="review-messages-on-the-site-server"></a>Berichten op de site server controleren

Als [uitgebreide logboek registratie](../../plan-design/hierarchy/about-log-files.md#bkmk_reg-client) is ingeschakeld op het beheer punt, kunt u zien hoe binnenkomende client berichten worden verwerkt. Zoek `MP_RelayMsgMgr.log`in de `TaskID`.

In het `MP_RelayMsgMgr.log` voor beeld ziet u de client-id `(GUID:83F67728-2E6D-4E4F-8075-ED035C31B783)` en de. `Task ID {9A4E59D2-2F5B-4067-A9FA-B99602A3A4A0}` Er wordt een bericht-ID toegewezen aan het antwoord van de client voordat deze wordt verzonden naar de engine voor bericht verwerking:

<pre><code lang="Log">MessageKey: GUID:83F67728-2E6D-4E4F-8075-ED035C31B783<b>{9A4E59D2-2F5B-4067-A9FA-B99602A3A4A0}</b>
Create message succeeded for <b>message id 22f00adf-181e-4bad-b35e-d18912f39f89</b>
Add message payload succeeded for message id 22f00adf-181e-4bad-b35e-d18912f39f89
Put message succeeded for message id 22f00adf-181e-4bad-b35e-d18912f39f89
CRelayMsgMgrHandler::HandleMessage(): ExecuteTask() succeeded
</code></pre>

Als [uitgebreide logboek registratie](../../plan-design/hierarchy/about-log-files.md#bkmk_logoptions) is ingeschakeld `SMS_MESSAGE_PROCESSING_ENGINE.log`, worden de client resultaten verwerkt. Gebruik de bericht-ID die u hebt `MP_RelayMsgMgr.log`gevonden in de. De vermeldingen in het verwerkings logboek zijn vergelijkbaar met het volgende voor beeld:

<pre><code lang="Log">Processing 2 messages with type Instant and IDs <b>22f00adf-181e-4bad-b35e-d18912f39f89[19]</b>, 434d80ae-09d4-4d84-aebf-28a4a29a9852[20]...
Processed 2 messages with type Instant. Failed to process 0 messages. All message IDs <b>22f00adf-181e-4bad-b35e-d18912f39f89[19]</b>, 434d80ae-09d4-4d84-aebf-28a4a29a9852[20]
</code></pre>

> [!TIP]
> Als u een uitzonde ring krijgt tijdens de verwerking, kunt u deze bekijken door de volgende SQL-query uit te voeren en de kolom uitzonde ring te bekijken. Nadat het bericht is verwerkt, bevindt het zich niet meer `MPE_RequestMessages_Instant` in de tabel.
>
> ```SQL
> select * from MPE_RequestMessages_Instant where MessageID=<ID from SMS_MESSAGE_PROCESSING_ENGINE.log>
> ```

Zoek `BgbServer.log`in de `PushID` om het aantal clients te zien dat heeft gerapporteerd of mislukt.

<pre><code lang="Log">Generated BGB task status report c:\ConfigMgr\inboxes\bgb.box\Bgb5c1db.BTS at 09/16/2019 16:46:39. (<b>PushID: 9</b> ReportedClients: 2 FailedClients: 0)
</code></pre>

Controleer de weer gave controle voor CMPivot van SQL met behulp van de `TaskID`.

``` SQL
select * from vSMS_CMPivotStatus where TaskID='{9A4E59D2-2F5B-4067-A9FA-B99602A3A4A0}'
```

[![CMPivot SQL-query's voor het oplossen van problemen in versie 1902](media/cmpivot-sql-queries-1902.png)](media/cmpivot-sql-queries-1902.png#lightbox)

## <a name="troubleshoot-cmpivot-in-1810-and-earlier"></a><a name="bkmk_CMPivot-1810"></a>Problemen met CMPivot oplossen in 1810 en eerdere versies

In Configuration Manager versies 1810 en eerder, verwerkt de site server de communicatie met de client.

### <a name="get-information-from-the-site-server"></a>Informatie ophalen van de site server

De logboek bestanden van de site server bevinden zich standaard `C:\Program Files\Microsoft Configuration Manager\logs`in. Deze locatie kan verschillen als u een niet-standaard installatie directory of Offloaded items hebt opgegeven als de SMS-provider naar een andere server.

Zoeken in `smsprov.log` voor deze regel:

<pre><code lang="Log">Auditing: User <username> initiated client operation 135 to collection &ltCollectionId>.
</code></pre>

Zoek de ID in het venster CMPivot. Deze ID is de `ClientOperationID`.

![CMPivot-venster met ClientOperationID gemarkeerd](media/cmpivot-clientoperationid.png)

Zoek de `TaskID` in de tabel ClientAction. De `TaskID` correspondeert met `UniqueID` de in de ClientAction-tabel.

``` SQL
select * from ClientAction where ClientOperationId=<id>
```

Zoek `BgbServer.log`in de `TaskID` bij SQL verzamelde. Het label `TaskGUID`. Bijvoorbeeld:

<pre><code lang="Log">Starting to send push task (PushID: 260 TaskID: 258 TaskGUID: <b>F8C7C37F-B42B-4C0A-B050-2BB44DF1098A</b> TaskType: 15
TaskParam: PFNjcmlwdEhhc2ggU2NyaXB0SGF...truncated...to 5 clients with throttling (strategy: 1 param: 42)
Finished sending push task (PushID: 260 TaskID: 258) to 5 clients
</code></pre>

### <a name="client-logs"></a>Client logboeken

Nadat u de informatie van de site server hebt, controleert u de logboeken van de client. De client logboeken bevinden zich standaard in `C:\Windows\CCM\Logs`.

Zoek `CcmNotificationAgent.log`in naar logboeken die vergelijkbaar zijn met de volgende vermelding:  

<pre><code lang="Log"><b>Error! Bookmark not defined.</b>+PFNjcmlwdEhhc2ggU2NyaXB0SGFzaEFsZz0nU0hBMjU2Jz42YzZmNDY0OGYzZjU3M2MyNTQyNWZiNT
g2ZDVjYTIwNzRjNmViZmQ1NTg5MDZlMWI5NDRmYTEzNmFiMDE0ZGNjPC9TY3JpcHRIYXNoPjxTY3Jp (truncated log entry)
</code></pre>

Zoek naar `Scripts.log` voor de `TaskID`. In het volgende voor beeld ziet `Task ID {F8C7C37F-B42B-4C0A-B050-2BB44DF1098A}`u:

<pre><code lang="Log">Sending script state message: 7DC6B6F1-E7F6-43C1-96E0-E1D16BC25C14
State message: Task Id <b>{F8C7C37F-B42B-4C0A-B050-2BB44DF1098A}</b>
</code></pre>

Zoeken in `StateMessage.log`. In het volgende voor beeld ziet u dat `TaskID` onder aan het bericht wordt weer gegeven naast `<Param>`:

``` XML
StateMessage body: <?xml version="1.0" encoding="UTF-16"?>
<Report><ReportHeader><Identification><Machine><ClientInstalled>1</ClientInstalled><ClientType>1
</ClientType><ClientID>GUID:DBAC52C9-57E6-47D7-A8D6-E0A5A64B57E6</ClientID><ClientVersion>5.00.8670.1000</ClientVersion>
<NetBIOSName>R613924</NetBIOSName><CodePage>437</CodePage>
<SystemDefaultLCID>1033</SystemDefaultLCID><Priority>0</Priority></Machine></Identification>
<ReportDetails><ReportContent>State Message Data</ReportContent><ReportType>Full</ReportType>
<Date>20180703184447.673000+000</Date><Version>1.0</Version><Format>1.0</Format>
</ReportDetails></ReportHeader><ReportBody><StateMessage MessageTime="20180703184447.517000+000"><Topic ID="7DC6B6F1-E7F6-43C1-96E0-E1D16BC25C14" Type="9003" IDType="0" User="" UserSID=""/><State ID="1" Criticality="0"/>
<StateDetails Type="1"><![CDATA["PAA/AHgAbQBsACAAdgBlAHIAcwBpAG8AbgA9ACIAMQAuADAAIgAgAGUAbgBjAG8AZABpAG4AZwA9ACIAdQB0AGYALQAxADYAIgA/AD4APAByAGUAcwB1AGwAdAAgAFIAZQBzAHUAbAB0AEMAbwBkAGUAPQAiADAAIgA+ADwAZQAgAE4AYQBtAGUAPQAiAEkAbgB0AGUAbAAoAFIAKQAgAFgAZQBvAG4AKABSACkAIABDAFAAVQAgAEUANQAtADIANgA3ADMAIAB2ADQAIABAACAAMgAuADMAMABHAEgAegAiACAATQBhAG4AdQBmAGEAYwB0AHUAcgBlAHIAPQAiAEEAbQBlAHIAaQBjAGEAbgAgAE0AZQBnAGEAdAByAGUAbgBkAHMAIABJAG4AYwAuACIAIABWAGUAcgBzAGkAbwBuAD0AIgBWAFIAVABVAEEATAAgAC0AIAA2ADAAMAAxADcAMAAyACIAIABSAGUAbABlAGEAcwBlAEQAYQB0AGUAPQAiADIAMAAxADcALQAwADYALQAwADIAIAAwADAAOgAwADAAOgAwADAAIgAgAFMAZQByAGkAYQBsAE4AdQBtAGIAZQByAD0AIgAwADAAMAAwAC0AMAAwADEAOAAtADMANgA4ADIALQA0ADcAMAA4AC0ANwA2ADQAMAAtADcANgAwADAALQAzADMAIgAgAFMATQBCAEkATwBTAEIASQBPAFMAVgBlAHIAcwBpAG8AbgA9ACIAMAA5ADAAMAAwADcAIAAiACAALwA+ADwALwByAGUAcwB1AGwAdAA+AA=="~~]]></StateDetails><UserParameters Flags="0" Count="2">
<Param>{F8C7C37F-B42B-4C0A-B050-2BB44DF1098A}</Param><Param>0</Param></UserParameters></StateMessage></ReportBody></Report>

Successfully forwarded State Messages to the MP StateMessage 7/3/2018 11:44:47 AM 5036 (0x13AC)
```

### <a name="review-messages-on-the-site-server"></a>Berichten op de site server controleren

Open `statesys.log` om te zien of het bericht is ontvangen en verwerkt. In het volgende voor beeld ziet `TaskID` u onder aan het bericht naast. `<Param>` Schakel [uitgebreide logboek registratie](../../plan-design/hierarchy/about-log-files.md#bkmk_logoptions) in op het SMS_STATE_SYSTEM onderdeel om deze logboek vermeldingen weer te geven.

``` XML
CMessageProcessor - the cmdline to DB exec dbo.spProcessStateReport N'?<?xml version="1.0" encoding="UTF-
16"?>~~<Report><ReportHeader><Identification><Machine><ClientInstalled>1</ClientInstalled><ClientType>1
</ClientType><ClientID>GUID:DBAC52C9-57E6-47D7-A8D6-E0A5A64B57E6</ClientID><ClientVersion>5.00.8670.1000</ClientVersion>
<NetBIOSName>R613924</NetBIOSName><CodePage>437</CodePage>
<SystemDefaultLCID>1033</SystemDefaultLCID><Priority>0</Priority></Machine></Identification>
<ReportDetails><ReportContent>State Message Data</ReportContent><ReportType>Full</ReportType>
<Date>20180703184447.673000+000</Date><Version>1.0</Version><Format>1.0</Format>
</ReportDetails></ReportHeader><ReportBody><StateMessage MessageTime="20180703184447.517000+000"><Topic ID="7DC6B6F1-E7F6-43C1-96E0-E1D16BC25C14" Type="9003" IDType="0" User="" UserSID=""/><State ID="1" Criticality="0"/>
<StateDetails Type="1"><![CDATA["PAA/AHgAbQBsACAAdgBlAHIAcwBpAG8AbgA9ACIAMQAuADAAIgAgAGUAbgBjAG8AZABpAG4AZwA9ACIAdQB0AGYALQAxADYAIgA/AD4APAByAGUAcwB1AGwAdAAgAFIAZQBzAHUAbAB0AEMAbwBkAGUAPQAiADAAIgA+ADwAZQAgAE4AYQBtAGUAPQAiAEkAbgB0AGUAbAAoAFIAKQAgAFgAZQBvAG4AKABSACkAIABDAFAAVQAgAEUANQAtADIANgA3ADMAIAB2ADQAIABAACAAMgAuADMAMABHAEgAegAiACAATQBhAG4AdQBmAGEAYwB0AHUAcgBlAHIAPQAiAEEAbQBlAHIAaQBjAGEAbgAgAE0AZQBnAGEAdAByAGUAbgBkAHMAIABJAG4AYwAuACIAIABWAGUAcgBzAGkAbwBuAD0AIgBWAFIAVABVAEEATAAgAC0AIAA2ADAAMAAxADcAMAAyACIAIABSAGUAbABlAGEAcwBlAEQAYQB0AGUAPQAiADIAMAAxADcALQAwADYALQAwADIAIAAwADAAOgAwADAAOgAwADAAIgAgAFMAZQByAGkAYQBsAE4AdQBtAGIAZQByAD0AIgAwADAAMAAwAC0AMAAwADEAOAAtADMANgA4ADIALQA0ADcAMAA4AC0ANwA2ADQAMAAtADcANgAwADAALQAzADMAIgAgAFMATQBCAEkATwBTAEIASQBPAFMAVgBlAHIAcwBpAG8AbgA9ACIAMAA5ADAAMAAwADcAIAAiACAALwA+ADwALwByAGUAcwB1AGwAdAA+AA=="~~]]></StateDetails><UserParameters Flags="0" Count="2">
<Param>{F8C7C37F-B42B-4C0A-B050-2BB44DF1098A}</Param><Param>0</Param></UserParameters></StateMessage></ReportBody></Report>~~'
```

Als het bericht niet is verwerkt, controleert u het postvak in van de status berichten. De standaard locatie voor Postvak `C:\Program Files\Microsoft Configuration Manager\inboxes\auth\statesys.box\`in is. Zoek naar de bestanden op deze locaties:

- Binnenkomende
- Raakt
- Proces

Controleer de weer gave controle voor CMPivot van SQL met `TaskID`behulp van de.

``` SQL
select * from vSMS_CMPivotStatus where TaskID='{F8C7C37F-B42B-4C0A-B050-2BB44DF1098A}'
```

>[!NOTE]
>Voor clients die versie 1810 of hoger gebruiken, wordt status berichten niet gebruikt tenzij de uitvoer groter is dan 80 KB. Bij het oplossen van problemen met CMPivot in deze gevallen kunt u meer informatie krijgen wanneer u uitgebreide logboek registratie inschakelt op uw MPs en de SMS_MESSAGE_PROCESSING_ENGINE van de site server. Zie [Opties voor logboek registratie van de site server](../../plan-design/hierarchy/about-log-files.md#bkmk_reg-site)voor meer informatie over het inschakelen van uitgebreide logboek registratie.
>
> Raadpleeg de volgende logboeken om problemen op te lossen:
>
> - `MP_Relay.log`
> - `SMS_MESSAGE_PROCESSING_ENGINE.log`

## <a name="next-steps"></a>Volgende stappen

- [CMPivot gebruiken](cmpivot.md)
- [PowerShell-scripts maken en uitvoeren](../../../apps/deploy-use/create-deploy-scripts.md)