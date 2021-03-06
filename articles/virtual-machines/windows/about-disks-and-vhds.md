---
title: Körülbelül nem felügyelt (lapblobok) és a managed disks storage a Microsoft Azure Windows virtuális gépek |} A Microsoft Docs
description: További információ a alapjait nem felügyelt (lapblobok) és a felügyelt lemezes tárolás az Azure Windows virtual machines.
services: virtual-machines-windows,storage
author: roygara
ms.service: virtual-machines-windows
ms.tgt_pltfrm: windows
ms.topic: article
ms.date: 11/15/2017
ms.author: rogarana
ms.component: disks
ms.openlocfilehash: 4fa8341b4d1953e3c59d345f45853f4c9a4a2941
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715454"
---
# <a name="about-disks-storage-for-azure-windows-vms"></a>Az Azure Windows virtuális gépek disks storage-ról
Minden olyan számítógépre, mint az Azure-beli virtuális gépek lemezeket használnak egy olyan hely operációs rendszert, alkalmazásokat és adatokat szeretne tárolni. Az összes Azure-beli virtuális gépek legalább két lemezt – egy Windows operációs rendszert tartalmazó lemez és a egy ideiglenes lemezzel rendelkezik. Az operációsrendszer-lemez jön létre egy rendszerképből, és az operációsrendszer-lemez és a kép is tárolt virtuális merevlemezek (VHD) egy Azure storage-fiókban. Virtuális gépek is rendelkezhet egy vagy több adatlemezt, is tárolt VHD-ként. 

Ebben a cikkben fogja különböző felhasználási módja a lemezeket, és, amelyet majd ismertetjük a különböző típusú is létrehozhat és használhat lemezeket. Ez a cikk érhető el is [Linux rendszerű virtuális gépek](../linux/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Virtuális gépek által használt lemezek

Vessünk egy pillantást a lemezeket a virtuális gépek általi felhasználási módjáról.

### <a name="operating-system-disk"></a>Operációsrendszer-lemez
Minden virtuális gép rendelkezik egy csatlakoztatott operációsrendszer-lemez. Regisztrált egy SATA-meghajtót, és alapértelmezés szerint a C: meghajtón kerülhetnek. Ez a lemez maximális kapacitása 2048 gigabájt (GB). 

### <a name="temporary-disk"></a>Ideiglenes lemez
Minden virtuális gép egy ideiglenes lemezt tartalmaz. Az ideiglenes lemez rövid távú tárolást biztosít az alkalmazások és folyamatok, és csak az adatok, például lapozófájlokat tárolására szolgál. Az ideiglenes lemezen lévő adatok elveszhetnek során egy [karbantartási esemény](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) , vagy ha Ön [egy virtuális gép ismételt üzembe](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Során a virtuális gép sikeres standard újraindítása az ideiglenes meghajtón lévő adatok megmaradnak. 

Az ideiglenes lemez a D: meghajtó címkével alapértelmezés szerint és azt pagefile.sys tárolására szolgál. Tekintse meg ezt a lemezt, más meghajtóbetűt újramegfeleltetése, [a Windows ideiglenes lemez meghajtóbetűjelét](change-drive-letter.md). Az ideiglenes lemez méretének a függvénye a virtuális gép méretét. További információkért lásd: [méretek a Windows virtuális gépek](sizes.md).

Az Azure általi az ideiglenes lemez további információkért lásd: [az ideiglenes meghajtó a Microsoft Azure Virtual Machines ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)


### <a name="data-disk"></a>Adatlemez
Adatlemez egy alkalmazás vagy egyéb adatok továbbra is szeretné tárolni a virtuális géphez csatolt virtuális merevlemez. Adatlemezek SCSI-meghajtók, regisztrálva van, és betűvel, Ön által választott vannak ellátva. Az egyes adatlemezek kapacitása maximum 4095 GB-ot. A virtuális gép mérete határozza meg, hány adatlemez csatolható, és a tárolás típusát használhatja a lemezek.

> [!NOTE]
> Virtuális gépek kapacitások kapcsolatos további információkért lásd: [méretek a Windows virtuális gépek](sizes.md).
> 

Az Azure létrehozza az operációsrendszer-lemez egy rendszerképből egy virtuális gép létrehozásakor. Adatlemezek tartalmazó lemezképek használata, ha az Azure az, amikor létrehozza a virtuális gép is létrehoz az adatlemezeket. Ellenkező esetben hozzáadhat adatlemezeket a virtuális gép létrehozása után.

Adhat hozzá adatlemezeket a virtuális gépek bármikor, az **csatolása** a lemezt a virtuális géphez. Egy virtuális Merevlemezt, amelyet feltöltött, vagy másolja a tárfiók használata, vagy használjon egy üres VHD-t, amely az Azure létrehozza az Ön számára. A virtuális gép adatlemezt csatol társítja a VHD-fájlt a címbérlet helyez a VHD-t, így azt nem törölhető a tárolás közben továbbra is csatlakoztatva van.


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Egy utolsó javaslat: TRIM standard szintű nem felügyelt lemezek használata 

Ha nem felügyelt a standard szintű lemezek (HDD) használja, engedélyeznie kell a vágás. TRIM elveti a nem használt blokkolja a lemezen, tehát csak a ténylegesen használt tárolási lesznek számlázva. Ez érhet el költségmegtakarítást Ha nagyméretű fájlokat hoz létre, és törölje őket. 

Ez a parancs vágás beállítás ellenőrzéséhez futtathatja. Nyissa meg a Windows virtuális gép, és írja be a parancssorba:


```
fsutil behavior query DisableDeleteNotify
```

A parancs a 0 értéket adja vissza, ha TRIM megfelelően engedélyezve van. Ha a visszaadott érték 1, TRIM engedélyezéséhez a következő parancsot:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Megjegyzés: A vágás támogatása elindítja a Windows Server 2012 vagy Windows 8 és újabb, lásd: [új API lehetővé teszi, hogy az alkalmazások "TRIM és megfeleltetésének törlése" mutatók küldendő adathordozók](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>További lépések
* [Lemez csatolása](attach-disk-portal.md) , adjon hozzá további tárterületet a virtuális géphez.
* [Pillanatkép létrehozása](snapshot-copy-managed-disk.md).
* [Konvertálás felügyelt lemezekké](convert-unmanaged-to-managed-disks.md).


