---
title: Különbségek és szempontok a virtuális gépek az Azure Stackben |} A Microsoft Docs
description: Különbségek és szempontok ismerje meg az Azure Stack lévő virtuális gépek használatakor.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 6613946D-114C-441A-9F74-38E35DF0A7D7
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: brenduns
ms.openlocfilehash: 4d475ec93bd8bfa5cc84848ed61afa6a9d6e319b
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902180"
---
# <a name="considerations-for-using-virtual-machines-in-azure-stack"></a>Szempontok a virtuális gépek az Azure Stackben

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Az Azure Stack virtuális gépek igény szerinti, méretezhető számítási erőforrás biztosítanak. Mielőtt telepít virtuális gépeket (VM), ismernie kell a virtuális gép az Azure Stackben elérhető funkciókat és a Microsoft Azure közötti különbségeket. Ez a cikk ismerteti a különbségeket, és azonosítja a virtuális gép központi telepítések tervezésének fő szempontjait. Azure Stack és az Azure magas szintű különbségeit kapcsolatos további információkért tekintse meg a [szempontok kulcs](azure-stack-considerations.md) cikk.

## <a name="cheat-sheet-virtual-machine-differences"></a>Hasznos tanácsok: virtuális gép különbségek

| Szolgáltatás | Azure (globális) | Azure Stack |
| --- | --- | --- |
| Virtuálisgép-lemezképek | Az Azure Marketplace-en a lemezképek, amelyek segítségével hozzon létre egy virtuális gépet tartalmaz. Tekintse meg a [Azure Marketplace-en](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) oldal az Azure piactéren elérhető rendszerképek listájának megtekintéséhez. | Alapértelmezés szerint nincsenek képeket elérhető az Azure Stack piactéren. Az Azure Stack-felhő rendszergazdájának kell közzétenni vagy töltse le a rendszerképeket az Azure Stack piactéren felhasználók használatba vétel előtt. |
| Virtuálisgép-méretek | Az Azure számos különböző méretet a virtuális gépeket támogatja. A rendelkezésre álló méretekben és lehetőségekkel kapcsolatos további információkért tekintse meg a [Windows virtuális gépek méretei](../../virtual-machines/virtual-machines-windows-sizes.md) és [Linux virtuálisgép-méretek](../../virtual-machines/linux/sizes.md) témaköröket. | Az Azure Stack az Azure-ban elérhető virtuálisgép-méretek egy részét támogatja. Ha szeretné megtekinteni a támogatott méretek listáját, tekintse meg a [virtuálisgép-méretek](#virtual-machine-sizes) című szakaszát. |
| Virtuális gép kvóták | [Magkvóta korlátozásának](../../azure-subscription-service-limits.md#service-specific-limits) Microsoft-beállításokat | Az Azure Stack-felhő rendszergazdája hozzá kell rendelnie kvóták, mielőtt azok a felhasználók számára a virtuális gépek kínálnak. |
| Virtuálisgép-bővítmények |Az Azure számos különböző virtuálisgép-bővítmények támogatja. Az elérhető bővítmények kapcsolatos további információkért tekintse meg a [virtuális gépi bővítmények és szolgáltatások](../../virtual-machines/windows/extensions-features.md) cikk.| Az Azure Stack az Azure-ban elérhető bővítmények egy részét támogatja, és a bővítmény rendelkező verzióját. Az Azure Stack-felhő rendszergazdája választhat a kiterjesztések lehetővé kell tenni a felhasználók számára. A támogatott bővítmények listájának megtekintéséhez, tekintse meg a [virtuálisgép-bővítmények](#virtual-machine-extensions) című szakaszát. |
| Virtuálisgép-hálózat | A bérlői virtuális géphez rendelt nyilvános IP-címeket az interneten keresztül érhetők el.<br><br><br>Az Azure Virtual Machines rögzített DNS névvel rendelkezik. | A bérlői virtuális géphez társított nyilvános IP-címek csak az Azure Stack Development Kit környezetben érhetők el. A felhasználó az Azure Stack Development Kit használatával hozzáféréssel kell rendelkeznie [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) vagy [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) kapcsolódni egy virtuális gépet, amely jön létre az Azure Stackben.<br><br>Egy adott Azure Stack-példánnyal belül létrehozott virtuális gépeket kell egy DNS-nevet, a felhő rendszergazdája által beállított értéke alapján. |
| Virtuálisgép-tároló | Támogatja a [felügyelt lemezeket.](../../virtual-machines/windows/managed-disks-overview.md) | A felügyelt lemezek még nem támogatottak az Azure Stackben. |
| API-verziók | Az Azure mindig a legújabb API-verziók a virtuális gép minden funkció esetén rendelkezik. | Az Azure Stack konkrét Azure-szolgáltatás és az adott API-verziók támogatja ezeket a szolgáltatásokat. Ha szeretné megtekinteni a támogatott API-verziók listáját, tekintse meg a [API-verziók](#api-versions) című szakaszát. |
|Virtuális gép rendelkezésre állási csoportok|Több tartalék tartomány (2 vagy 3 régiónként)<br>Több frissítési tartomány<br>Felügyelt lemezek támogatása|Több tartalék tartomány (2 vagy 3 régiónként)<br>(Legfeljebb 20) több frissítési tartomány<br>Nem felügyelt lemezes támogatás|
|Virtuálisgép-méretezési csoportok|Automatikus skálázás támogatott|Automatikus skálázás nem támogatott.<br>További példányok hozzáadása egy méretezési csoportot a portálon, a Resource Manager-sablonok vagy a PowerShell használatával.

## <a name="virtual-machine-sizes"></a>Virtuálisgép-méretek

Az Azure Stack közzétételével erőforráskorlátok elkerülése érdekében keresztül használatalapú erőforrások (helyi és a szolgáltatásiszint-kiszolgáló.) Ezek a korlátok a bérlői erőforrás-használat más bérlők által hatásának csökkentése révén javíthatja.

- A hálózati kimenő forgalom a virtuális gépről nincsenek sávszélesség caps helyen. Az Azure Stackben Caps ugyanazok, mint a caps az Azure-ban.
- A tárolási erőforrások az Azure Stack IOPS-korlátok storage tárolóhoz a hozzáférés a bérlők által erőforrások alapszintű overconsumption elkerülése érdekében valósít meg.
- Több csatlakoztatott adatlemezekkel rendelkező virtuális gépek esetén a maximális sebessége az egyes adatlemezek 500 IOPS HDD és SSD-k 2300 IOPS.

Az alábbi táblázat a virtuális gépek által támogatott az Azure Stacken és annak konfigurációját sorolja fel:

| Típus           | Méret          | Támogatott méretek tartományán |
| ---------------| ------------- | ------------------------ |
|Általános célú |Alapszintű A        |[A0 - A4](azure-stack-vm-sizes.md#basic-a)                   |
|Általános célú |Standard A     |[A0 - A7](azure-stack-vm-sizes.md#standard-a)              |
|Általános célú |D-sorozat       |[D1 - D4](azure-stack-vm-sizes.md#d-series)              |
|Általános célú |Dv2-sorozat     |[D1_v2 - D5_v2](azure-stack-vm-sizes.md#ds-series)        |
|Általános célú |DS-sorozat      |[DS1 - DS4](azure-stack-vm-sizes.md#dv2-series)            |
|Általános célú |DSv2-sorozat    |[DS1_v2 - DS5_v2](azure-stack-vm-sizes.md#dsv2-series)      |
|Memóriaoptimalizált|D-sorozat       |[D11 - D14](azure-stack-vm-sizes.md#mo-d)            |
|Memóriaoptimalizált|DS-sorozat      |[DS11 - DS14](azure-stack-vm-sizes.md#mo-ds)|
|Memóriaoptimalizált|Dv2-sorozat     |[D11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dv2)     |
|Memóriaoptimalizált|DSv2 sorozat –  |[DS11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dsv2)    |

Virtuálisgép-méretek és a társított erőforrás mennyiségek konzisztensek az Azure Stack és az Azure között. Ez magában foglalja a memória, a magok számát és a létrehozott adatlemezek száma és mérete. Az azonos méretű virtuális gépek teljesítményét azonban egy adott Azure Stack-környezet alapul szolgáló jellemzőit függ.

## <a name="virtual-machine-extensions"></a>Virtuálisgép-bővítmények

 Az Azure Stack bővítmények egy kis készletét tartalmazza. Frissítések és a további kiterjesztések tartalomtípus-gyűjtési Marketplace-en keresztül érhető el.

A következő PowerShell-parancsfájl használatával érhetők el az Azure Stack környezettel virtuálisgép-bővítmények listájának beolvasása:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

## <a name="api-versions"></a>API-verziók

Virtuális gépek jellemzői az Azure Stack a következő API-verziók támogatják:

![Virtuális gép erőforrástípusok](media/azure-stack-vm-considerations/vm-resoource-types.png)

A következő PowerShell-parancsfájl segítségével az API-verziók lekérése az Azure Stack-környezet által biztosított virtuálisgép-funkciók:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```

A támogatott erőforrástípusok és API-verziók listáját eltérőek lehetnek, ha a felhő üzemeltetője frissíti az Azure Stack-környezet újabb verzióra.

## <a name="windows-activation"></a>Windows-aktiválás

Windows termékek termékhasználati jogosultságoknak és a Microsoft licencfeltételeit összhangban kell felhasználni. Használja az Azure Stack [automatikus Virtuálisgép-aktiválás](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v%3dws.11)) Windows Server virtuális gépeken (VM) aktiválása (AVMA).

- Az Azure Stack gazdagépen az AVMA kulcsokat Windows aktiválja a Windows Server 2016-hoz. Az összes virtuális gépek, amelyek a Windows Server 2012 vagy újabb a rendszer automatikusan aktiválja.
- Futtassa a Windows Server 2008 R2 automatikusan nem aktív, és aktiválni kell a virtuális gépek [MAK-aktiválást](https://technet.microsoft.com/library/ff793438.aspx). A MAK-aktiválást használjon, meg kell adnia a saját termékkulcsot.

A Microsoft Azure Windows virtuális gépek aktiválása KMS-aktiválást használ. Ha áthelyezi egy virtuális Gépet az Azure Stack az Azure és az esetlegesen fellépő problémák aktiválása, lásd: [hibaelhárítása az Azure Windows virtuális gép aktiválási problémák](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-activation-problems). További információt talál a [hibaelhárítása Windows-aktiválási hibák az Azure virtuális gépekhez](https://blogs.msdn.microsoft.com/mast/2017/06/14/troubleshooting-windows-activation-failures-on-azure-vms/) Azure támogatási csapatának blogbejegyzést.

## <a name="next-steps"></a>További lépések

[Windows virtuális gép létrehozása a PowerShell-lel az Azure Stackben](azure-stack-quick-create-vm-windows-powershell.md)
