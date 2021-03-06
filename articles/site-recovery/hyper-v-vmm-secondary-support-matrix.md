---
title: Támogatási mátrixa a VMM-ben a Hyper-V virtuális gépek replikálása másodlagos helyre az Azure Site Recovery felhők |} A Microsoft Docs
description: Hyper-V virtuális gép replikációja egy másodlagos helyre az Azure Site Recovery VMM-felhőkben támogatását összegzi.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 13b4dc6e49c67ae9fd880ab64880966835242dec
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921499"
---
# <a name="support-matrix-for-replication-of-hyper-v-vms-to-a-secondary-site"></a>A Hyper-V virtuális gépek replikálása másodlagos helyre támogatási mátrix

Ez a cikk összefoglalja, mit támogat használatakor a [Azure Site Recovery](site-recovery-overview.md) szolgáltatást, hogy a Hyper-V virtuális gépek replikálása másodlagos helyre a System Center Virtual Machine Manager (VMM) felhőkben felügyelt. Ha azt szeretné, a Hyper-V virtuális gépek replikálása az Azure-ba, tekintse át a [a támogatási mátrix](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Csak replikálhatja egy másodlagos helyre, ha a Hyper-V-gazdagépek a VMM-felhőkben felügyelt.

  

## <a name="host-servers"></a>Gazdagép-kiszolgálónak

**Operációs rendszer** | **Részletek**
--- | ---
Windows Server 2012 R2 | Kiszolgálók futnia kell a legújabb frissítéseket.
Windows Server 2016 |  A Windows Server 2016 és 2012 R2-állomások felhőket a VMM 2016 jelenleg nem támogatottak.<br/><br/> A System Center 2016 a System Center 2012 R2 VMM 2012 R2 verzióról frissített központi telepítések jelenleg nem támogatottak.


## <a name="replicated-vm-support"></a>A replikált virtuális gépek támogatása

A következő táblázat összefoglalja a Site Recovery szolgáltatásba replikált gépek operációs rendszerének támogatása. Bármilyen számítási feladatot futtathat a támogatott operációs rendszeren.

**Windows-verzió** | **A Hyper-V (VMM-mel)**
--- | ---
Windows Server 2016 | Bármely vendég operációs rendszer [Hyper-V által támogatott](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) Windows Server 2016-on 
Windows Server 2012 R2 | Bármely vendég operációs rendszer [Hyper-V által támogatott](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) Windows Server 2012 R2 rendszeren

## <a name="linux-machine-storage"></a>Linux rendszerű gép tárolójának

A következő tároló csak Linux rendszerű gépek replikálhatók:

- Fájlrendszer (EXT3, ETX4, ReiserFS, XFS).
- A többutas szoftver-eszköz Eseményleképező.
- Kötetek kezelése (LVM2).
- A HP CCISS vezérlő storage fizikai kiszolgálók nem támogatottak.
- SUSE Linux Enterprise Server 11 SP3 csak támogatott a ReiserFS fájlrendszer.

## <a name="network-configuration---hostguest-vm"></a>Hálózati konfiguráció - gazdagép vagy Vendég virtuális gép

**Konfigurálás** | **Támogatott**  
--- | --- 
Gazdagép - hálózati adapterek összevonása | Igen 
Gazdagép - VLAN | Igen 
Gazdagép - IPv4 | Igen 
Gazdagép - IPv6 | Nem 
Virtuálisgép - Vendég hálózati adapterek összevonása | Nem
Vendég virtuális Gépen – IPv4 | Igen
Vendég virtuális Gépen – IPv6 | Nem
Gues virtuális gép – Windows/Linux - statikus IP-cím | Igen
Vendég virtuális gép – több hálózati Adapterrel | Igen


## <a name="storage"></a>Storage

### <a name="host-storage"></a>Tárolók

**Storage (gazdagép)** | **Támogatott**
--- | --- 
NFS | –
SMB 3.0 |  Igen
TÁROLÓHÁLÓZAT (ISCSI) | Igen
Több elérési út (MPIO) | Igen

### <a name="guest-or-physical-server-storage"></a>Vendég- és fizikai kiszolgáló

**Konfigurálás** | **Támogatott**
--- | --- | 
VMDK |  –
VHD/VHDX | Igen (legfeljebb 16 lemez)
Velikost haldy 2 virtuális gép | Igen
A megosztott fürtlemezre | Nem
Titkosított lemez | Nem
UEFI| –
NFS | Nem
SMB 3.0 | Nem
RDM | –
> 1 TB-os lemez | Igen
A csíkozott lemez > 1 TB-os kötetet<br/><br/> LVM | Igen
A tárolóhelyek | Igen
Gyakori elérésű hozzáadása/eltávolítása lemez | Nem
Lemez kizárása | Igen
Több elérési út (MPIO) | Igen

## <a name="vaults"></a>Tárolók

**Művelet** | **Támogatott**
--- | --- 
Helyezze át a tárolók több erőforráscsoportban (belül vagy előfizetések között) |  Nem
Át a tárterületet, hálózati, Azure-beli virtuális gépek több erőforráscsoportban (belül vagy előfizetések között) | Nem

## <a name="azure-site-recovery-provider"></a>Az Azure Site Recovery-szolgáltató

A szolgáltató koordinálja a VMM-kiszolgálók közötti kommunikációt. 

**Legfrissebb** | **Frissítések**
--- | --- | --- | --- | ---
5.1.19 ([portálról érhető el](http://aka.ms/downloaddra) | [Legújabb funkciókkal és javításokkal](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>További lépések

[VMM-felhőkben futó Hyper-V virtuális gépek replikálása másodlagos helyre](tutorial-vmm-to-vmm.md)

