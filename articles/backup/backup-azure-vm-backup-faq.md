---
title: Azure virtuális gép biztonsági mentése – gyakori kérdések
description: 'Válaszok a következő gyakori kérdésekre: hogyan működik az Azure-beli virtuális gépek biztonsági mentése, mik a korlátozások, és mi történik, ha módosítások történnek a szabályzatban'
services: backup
author: trinadhk
manager: shreeshd
keywords: azure-beli virtuális gép biztonsági mentése, azure-beli virtuális gép visszaállítása, biztonsági mentési szabályzat
ms.service: backup
ms.topic: conceptual
ms.date: 7/18/2017
ms.author: trinadhk
ms.openlocfilehash: d637a98029b33be890b31f32c3080650b251f7a8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606375"
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Kérdések az Azure VM Backup szolgáltatással kapcsolatban
A cikk gyakori kérdésekre adott válaszokat tartalmazó szakaszaiban gyorsan áttekinthető az Azure VM Backup összetevőinek működése. Egyes válaszokban részletes információkat tartalmazó cikkekre mutató hivatkozások találhatók. Emellett egy fórumbejegyzésben is feltehet kérdéseket az Azure Backup szolgáltatással kapcsolatban a [vitafórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Biztonsági mentés konfigurálása
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>A Recovery Services-tárolók támogatják a klasszikus vagy a Resource Manager alapú virtuális gépeket? <br/>
A Recovery Services-tárolók mindkét modellt támogatják.  Mind a klasszikus (azaz a klasszikus portálon létrehozott), mind a Resource Manager-alapú (azaz az Azure Portalon létrehozott) virtuális gépekről készíthet biztonsági mentést egy Recovery Services-tárolóba.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>Milyen konfigurációk nem támogatottak az Azure virtuális gép biztonsági mentése?
Lépkedjen végig [támogatott operációs rendszerek](backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup) és [korlátozásai a virtuális gép biztonsági mentése](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Miért nem látom a virtuális gépemet a Biztonsági mentés konfigurálása varázslóban?
A biztonsági mentés varázslóban konfigurálása Azure Backup szolgáltatás csak felsorolja képező virtuális gépek:
  * Már nem védett ellenőrizheti a biztonsági mentés állapotának a virtuális gépek VM panelen lesz, és ellenőrzi a biztonsági másolat állapota-beállítások menüjében. További információ a [Virtuális gép biztonsági mentési állapotának ellenőrzéséről](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-operations-menu)
  * A virtuális géppel azonos régióba tartozik

## <a name="backup"></a>Biztonsági mentés
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Az igény szerinti biztonsági mentési feladatok ugyanazt a megőrzési ütemezést követik, mint az ütemezett biztonsági mentések?
Nem. Adja meg a megőrzés időtartamát, az igény szerinti biztonsági mentéshez. Alapértelmezés szerint 30 napig őrzi meg a portálról indítását. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Nemrég engedélyeztem az Azure Disk Encryption szolgáltatást néhány virtuális gépen. A biztonsági mentések továbbra is működni fognak?
Engedélyeznie kell az Azure Backup szolgáltatásnak a Key Vault elérését. Ezeket az engedélyeket a PowerShellben adhatja meg, a [PowerShell-dokumentáció](backup-azure-vms-automation.md) *Biztonsági mentés engedélyezése* szakaszában található lépések végrehajtásával.

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Egy virtuális gép lemezeit felügyelt lemezekre migráltam. A biztonsági mentések továbbra is működni fognak?
Igen, biztonsági mentések zökkenőmentesen működjön, és nem kell konfigurálnia a biztonsági mentés. 

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>A virtuális gép leállítása. Igény szerinti vagy ütemezett biztonsági mentési munkahelyi fog?
Igen. Akkor is, ha egy gép leállását, biztonsági mentések használhatók, és a helyreállítási pont van megjelölve, összeomlási konzisztens. További részletekért lásd: az adatok konzisztenciájának szakasz [Ez a cikk](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Egy folyamatban lévő biztonsági mentési feladatot is megszakítja?
Igen. Megszakíthatja a biztonsági mentési feladatot, ha "Pillanatkép" szakaszában. **Egy feladat nem szakítható meg, ha folyamatban van az adatátvitelt pillanatképből**. 

### <a name="i-enabled-resource-group-lock-on-my-backed-up-managed-disk-vms-will-my-backups-continue-to-work"></a>Erőforráscsoport lock bekapcsolva a biztonsági másolat felügyelt lemezes virtuális gépek A biztonsági mentések továbbra is működni fognak?
A felhasználó zárolja az erőforráscsoportot, biztonsági mentési szolgáltatás esetén nem tudja törölni a régi helyreállítási pontokat. Emiatt új biztonsági másolatok meghiúsul, a háttérrendszerből meghatározott maximális 18 visszaállítási pontok maximális. Ha a biztonsági mentések sikertelenek belső hiba történt az RG zárolása után, kövesse az alábbi [eltávolítása a visszaállítási lépések pont gyűjtemény](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock).

### <a name="does-backup-policy-take-daylight-saving-timedst-into-account"></a>Biztonsági mentési házirend nem figyelembe vennie nyári mentése Time(DST)?
Nem. Vegye figyelembe, hogy dátum és idő alapján a helyi számítógép megjelenik a helyi idő és a nyári időszámítás aktuális. A beállított ütemezett biztonsági mentések idejét, a helyi idő miatt nyári Időszámítás eltérő lehet.

## <a name="restore"></a>Visszaállítás
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>A lemezek visszaállítását vagy a teljes virtuális gép visszaállítását válasszam?
Az Azure teljes virtuális gép visszaállítási gondol a Gyorslétrehozás beállításként. Visszaállítani a virtuális gép lehetőséget történt módosításokat lemezek, ezek lemezek, a nyilvános IP-címek és a hálózati csatoló nevére által használt tároló nevét. A módosítás szükséges erőforrások a Virtuálisgép-létrehozása során létrehozott egyediségét karbantartása. De azt nem adja hozzá a virtuális gép rendelkezésre állási csoportba. 

A lemezek visszaállítását a következőkhöz használhatja:
  * A virtuális gép méretének módosítása például időpontjának beállítása pontjáról lekérdezi létrehozott testreszabása
  * Adja hozzá a konfigurációkat, amelyek nem találhatók a biztonsági mentés idején 
  * A létrejövő erőforrások elnevezési konvencióinak vezérléséhez
  * Virtuális gép hozzáadása rendelkezésre állási csoporthoz
  * A más beállításokat, amelyek csak a PowerShell vagy a deklaratív sablonok definition használatával érhető el
  
### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>Nem felügyelt lemezes virtuális gép biztonsági mentése segítségével visszaállítást, miután a lemezek felügyelt lemezek verziójára?
Igen, a biztonsági mentés előtt áttelepítése lemezei nem felügyelt kezelt is használhatja. Alapértelmezés szerint visszaállítási VM feladatot hoz létre a virtuális gép nem felügyelt lemezzel. Visszaállítási lemezek funkció használatával állítsa vissza a lemezek, melyekkel egy virtuális gép létrehozása kezelt lemezeken. 

### <a name="what-is-the-procedure-to-restore-a-vm-to-a-restore-point-taken-before-the-conversion-from-unmanaged-to-managed-disks-was-done-for-a-vm"></a>Mi az az eljárás végrehajtása előtt a átalakítása nem felügyelt kódból felügyelt kezelt lemezek végezhető el a virtuális gépek válasszon visszaállítási pontot állítsa helyre a virtuális Gépet?
Ebben a forgatókönyvben alapértelmezés szerint visszaállítási VM feladatot hoz létre egy virtuális gép nem felügyelt lemezek. Felügyelt lemezzel rendelkező virtuális gép létrehozása:
1. [Nem felügyelt lemezek visszaállítása](tutorial-restore-disk.md#restore-a-vm-disk)
2. [Alakítsa át a visszaállított lemezeket felügyelt](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk)
3. [Felügyelt lemezzel rendelkező virtuális gép létrehozása](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk) <br>
Powershell-parancsmagokkal, tekintse meg a [Itt](backup-azure-vms-automation.md#restore-an-azure-vm).

## <a name="manage-vm-backups"></a>Virtuális gép biztonsági mentéseinek kezelése
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Mi történik, ha módosítom a biztonsági mentési szabályzatot a virtuális gépen vagy gépeken?
Egy új házirend alkalmazása esetén a virtuális gép van, ütemezését és az új házirend megőrzési követi. Ha növeli a megőrzési időtartamot, a meglévő helyreállítási pontok az új szabályzatnak megfelelően megmaradnak. Ha csökkenti a megőrzési időtartamot, a helyreállítási pontok a következő tisztítási feladat során törlendőként lesznek megjelölve. 

### <a name="how-can-i-move-a-vm-enrolled-in-azure-backup-between-resource-groups"></a>Hogyan helyezhető át a virtuális gépek közötti erőforráscsoportok az Azure backup-ban regisztrált?
Kövesse a következő lépések végrehajtásával sikeresen biztonsági másolat virtuális gép áthelyezése a célként megadott erőforráscsoportja 
1. Ideiglenesen állítsa le a biztonsági mentés és a biztonsági mentési adatok megőrzése mellett
2. A virtuális gép áthelyezése a célként megadott erőforráscsoportja
3. Azonos vagy új tárolóban védené újra

Az áthelyezés előtt létrehozott rendelkezésre visszaállítási pontok felhasználók állíthatja vissza.


