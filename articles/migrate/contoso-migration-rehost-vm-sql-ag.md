---
title: A Contoso alkalmazás újratárolása migrálással, az Azure virtuális gépek és az SQL Server AlwaysOn rendelkezésre állási csoporthoz |} A Microsoft Docs
description: Ismerje meg, hogyan Contoso egy helyszíni alkalmazás újratárolása migrálással, az Azure virtuális gépek és az SQL Server AlwaysOn rendelkezésre állási csoporthoz
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: raynew
ms.openlocfilehash: 0cfb583f9d16039249aaffe18f71039e91dc3705
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/31/2018
ms.locfileid: "39359206"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-azure-vms-and-sql-server-alwayson-availability-group"></a>Contoso áttelepítése: az Azure virtuális gépek és az SQL Server AlwaysOn rendelkezésre állási csoport helyi alkalmazás Újratárolása

Ez a cikk bemutatja, hogyan a Contoso áthelyezi a SmartHotel alkalmazást az Azure-ban. Egy Azure virtuális gép és az alkalmazás-adatbázis az Azure SQL Server virtuális Gépet, az SQL Server AlwaysOn rendelkezésre állási gGroups Windows Server feladatátvevő fürtben való az alkalmazás előtérbeli virtuális gép át azokat.

Ez a dokumentum az egyik, a cikkeket, amelyek megmutatják, hogyan a fiktív Contoso áttelepíti a Microsoft Azure felhőbe helyszíni erőforrásait. A sorozat része a háttér-információkat és forgatókönyvek, amelyek a migrálás infrastruktúra beállításával, az áttelepítéshez a helyszíni erőforrások értékelése és áttelepítések különböző típusú futtató mutatják be. Forgatókönyvek egyre összetettebbé válnak, és idővel további cikkek adunk hozzá.

**Cikk** | **Részletek** | **Állapot**
--- | --- | ---
[1. cikk: áttekintés](contoso-migration-overview.md) | Contoso-áttelepítési stratégia, a cikk sorozat és a mintaalkalmazások használjuk áttekintést nyújt. | Elérhető
[2. cikk: Egy Azure-infrastruktúra üzembe helyezése](contoso-migration-infrastructure.md) | Ismerteti, hogyan Contoso előkészíti a helyszíni és az Azure-infrastruktúra az áttelepítéshez. Az összes migrálásról szóló cikksorozat ugyanazon az infrastruktúrán használható. | Elérhető
[3. cikk: A helyszíni erőforrások értékelése](contoso-migration-assessment.md)  | Bemutatja, hogyan Contoso fut a VMware-en futó helyszíni kétrétegű SmartHotel alkalmazás értékelése. Contoso alkalmazás rendelkező virtuális gépek értékeli a [Azure Migrate](migrate-overview.md) szolgáltatás és az alkalmazás SQL Server-adatbázisnak a [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Elérhető
[4. cikk: Áthelyezési egy alkalmazást az Azure virtuális gépeken és a egy felügyelt SQL-példány](contoso-migration-rehost-vm-sql-managed-instance.md) | Bemutatja, hogyan Contoso fut egy lift-and-shift-migrálás az Azure-bA az SmartHotel alkalmazás. Contoso áttelepíti az alkalmazás előtérbeli virtuális gép használatával [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), és az alkalmazás-adatbázis SQL felügyelt példányra, használja a [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Elérhető
[5. cikk: Áthelyezési egy alkalmazást az Azure-beli virtuális gépeken](contoso-migration-rehost-vm.md) | Bemutatja, hogyan a Contoso át a SmartHotel app virtuális gépek csak a Site Recovery használatával. | Elérhető
A cikk 6: Egy alkalmazást az Azure virtuális gépek és az SQL Server Always On rendelkezésre állási csoport áthelyezési | Bemutatja, hogyan telepíti át a Contoso a SmartHotel alkalmazást. Contoso számára, hogy az alkalmazás virtuális gépeit és a Database Migration service, az alkalmazás-adatbázis migrálása az AlwaysOn rendelkezésre állási csoport által védett SQL Server-fürtöt a Site Recovery használja. | Ez a cikk
[7. cikk: Áthelyezési egy Linux-alkalmazást az Azure-beli virtuális gépeken](contoso-migration-rehost-linux-vm.md) | Bemutatja, hogyan Contoso hajtja végre a Linux osTicket alkalmazás lift-and-shift áttelepítés Azure virtuális gépekre, a Site Recovery | Elérhető
[A cikk 8: Áthelyezési egy Linux-alkalmazást az Azure virtuális gépeken és az Azure MySQL-kiszolgáló](contoso-migration-rehost-linux-vm-mysql.md) | Bemutatja, hogyan Contoso áttelepíti a Linux-osTicket alkalmazás Azure-beli virtuális gépek Site Recovery használatával, és az alkalmazás-adatbázis áttelepítése Azure MySQL Server-példány, a MySQL Workbench használatával. | Elérhető
[9. cikk: Újrabontás egy alkalmazást az Azure Web Apps és az Azure SQL database](contoso-migration-refactor-web-app-sql.md) | Bemutatja, hogyan Contoso a SmartHotel alkalmazást áttelepíti egy Azure-webalkalmazást, és az alkalmazás-adatbázis áttelepítése az Azure SQL Server-példány | Elérhető
[10. cikk: Újrabontás egy Linux-alkalmazás Azure Web Apps és az Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Bemutatja, hogyan Contoso áttelepíti a Linux-osTicket alkalmazás Azure Web Apps több helyen, a folyamatos készregyártás a GitHub integrálva. Az alkalmazás-adatbázis nekik át egy Azure-beli MySQL-példányt. | Elérhető
[11. cikk: Újrabontás a TFS-t a vsts-ben](contoso-migration-tfs-vsts.md) | Bemutatja, hogyan telepíti át a Contoso a saját helyi Team Foundation Server (TFS) központi, migrálás, a Visual Studio Team Services (VSTS) az Azure-ban. | Elérhető
[A cikk 12: Azure-tárolók és az Azure SQL Database az alkalmazás újratervezése](contoso-migration-rearchitect-container-sql.md) | Bemutatja, hogyan Contoso áttelepíti, és rearchitects SmartHotel alkalmazás az Azure-bA. Az alkalmazás webes réteg egy Windows-tárolót, és a egy Azure SQL Database-ben az alkalmazás-adatbázis újratervezése azokat. | Elérhető
[Cikk 13: Építse újra az alkalmazást az Azure-bA](contoso-migration-rebuild.md) | Bemutatja, hogyan építse újra a Contoso az SmartHotel alkalmazás számos Azure-szolgáltatások és szolgáltatások, beleértve az App Services, Azure-beli Kubernetes, az Azure Functions, a Cognitive services és a Cosmos DB használatával. | Elérhető



Ebben a cikkben a Contoso a kétrétegű SmartHotel Windows .NET alkalmazás a VMware virtuális gépeken futó Azure-bA telepíti át. Ha szeretné használni ezt az alkalmazást, nyílt forráskódú nyújtja, és töltheti le a [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>A stratégiai

Az informatikai vezetőségi szorosan együttműködik az üzleti partnerek megértéséhez, amit szeretnének való áttérés eléréséhez:

- **Üzleti növekedés cím**: Contoso nő, és ennek eredményeképpen nincs a helyszíni rendszerek és infrastruktúra nyomás.
- **Növelheti a hatékonyságot**: Contoso cégnek szüksége van, távolítsa el a felesleges eljárásokat, és egyszerűsíthetők a folyamatok a fejlesztők és a felhasználók számára.  Az üzleti igények informatikai gyorsan, és nem Hulladékmennyiség idő vagy költséget takaríthat meg, így gyorsabban továbbítása az ügyfelek igényei.
- **A gyorsaság növeléséhez**: Contoso IT kell lennie a rugalmasabb ügyfélkapcsolatok kialakítását teszi az üzleti igényeinek. Reagáljon gyorsabban a Marketplace-en, a versenyképes sikeres engedélyezése a módosításokat, képesnek kell lennie.  Azt nem a módon, vagy egy üzleti blocker válnak.
- **Méretezési csoport**: a vállalat növekedésével sikeres, a Contoso informatikai kell megadnia rendszerek, amelyek képesek a ugyanolyan ütemben nő.

## <a name="migration-goals"></a>Áttelepítési célok

A Contoso felhőalapú csapat az áttelepítés célok le van rögzítve. Ezen célok lett felhasználva megállapítható hogy melyik a leginkább megfelelő áttelepítési módszer:

- Az áttelepítés után az alkalmazás az Azure-ban kell teljesítmény ugyanazokat a lehetőségeket, mint jelenleg VMWare-ben.  Az alkalmazás kritikus jelzéssel a felhőben, a helyszínen marad.
- Nem a Contoso szeretné fektethet be ezt az alkalmazást.  Fontos, hogy az üzleti, de a jelenlegi formájában egyszerűen szeretnék biztonságosan áthelyezése a felhőbe.
- Az alkalmazás a helyi adatbázis rendelkezésre állási problémák esetében. Ezek szeretné látni, telepített Azure-ban magas rendelkezésre állású fürtbe, a feladatátvételt is használhat.
- A Contoso biztosítani szeretné a jelenlegi platform az SQL Server 2008 R2 verzióról az SQL Server 2017.
- A Contoso nem szeretné használni ezt az alkalmazást az Azure SQL Database és lehetőségeket keres.

## <a name="proposed-architecture"></a>Javasolt architektúra

Ebben a forgatókönyvben:

- Az alkalmazás többszintű (WEBVM és SQLVM) két virtuális gép között.
- VMware ESXi-gazdagépen található virtuális gépek **contosohost1.contoso.com** (6.5-ös verzió)
- A VMware-környezet kezeli a vCenter Server 6.5-ös (**vcenter.contoso.com**), egy virtuális gépen futó.
- Contoso rendelkezik egy helyszíni adatközpont (contoso-datacenter), egy helyszíni tartományvezérlővel (**contosodc1**).
- A helyszíni virtuális gépeket a Contoso-datacenter elvesznek az áttelepítés befejezése után.

![Forgatókönyv-architektúra](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="azure-services"></a>Azure-szolgáltatások

**Szolgáltatás** | **Leírás** | **Költségek**
--- | --- | ---
[Adatbázis-kezelési szolgáltatás](https://docs.microsoft.com/azure/dms/dms-overview) | Contoso migráljuk az alkalmazás adatrétegének DMS használatával. A DMS a helyszíni SQLVM géphez összekapcsolásához site-to-site VPN, és több adatbázisforrásokból az Azure-beli adat-platformokra, minimális állásidővel DMS lehetővé teszi, hogy zökkenőmentes áttelepítések át. | Ismerje meg [támogatott régiók](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) DMS és a get [díjszabás](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Contoso lift-and-shift-áttelepítés, az alkalmazás előtérbeli virtuális gép Site Recovery használja. A Site Recovery koordinálja a migrálás és vész-helyreállítási kezeli az Azure virtuális gépek és a helyszíni virtuális gépek és fizikai kiszolgálók.  | Az Azure-ba, során Azure Storage-díjat számítunk fel.  Azure virtuális gépek jönnek létre, és a költségek, amikor feladatátvételt hajt végre. [További](https://azure.microsoft.com/pricing/details/site-recovery/) kapcsolatos díjakat és díjszabás.

 

## <a name="migration-process"></a>Áttelepítési folyamat

- Contoso az alkalmazás virtuális gépeit áttelepíti az Azure-bA.
- Azure virtuális gép Site Recovery használatával fog nekik az előtérbeli virtuális gép át:
    - Első lépésként azok fog előkészítése beállítása az Azure-összetevőket, és a helyszíni VMware-infrastruktúra előkészítése.
    - Az előkészített mindent elindíthatják a virtuális gép replikálása.
    - Után a replikáció engedélyezve van és működik, azok áttelepítése a virtuális gép – Azure feladatátvétele.
- SQL Server-fürtöt az Azure-ban, a Data Migration Service (DMS) használatával lesz nekik az adatbázis át.
    - Első lépésként azok fog kell kiépíteni az SQL Server virtuális gépek az Azure-ban, állítsa be a fürt és a egy belső terheléselosztót, és AlwaysOn rendelkezésre állási csoportok konfigurálása.
    - A helyen, az adatbázis migrálása
- Az áttelepítés után a használatukkal fogja az adatbázis AlwaysOn védelmét.

![Áttelepítési folyamat](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>Előfeltételek

Itt látható, milyen meg (és Contoso) kell futtatni az ebben a forgatókönyvben:

**Követelmények** | **Részletek**
--- | ---
**Azure-előfizetés** | Kell már létrehozott egy előfizetést az értékelés az oktatóanyag-sorozatban az első cikkben végrehajtását. Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Ha ingyenes fiókot hoz létre, Ön lesz az előfizetés rendszergazdája, és minden műveletet végrehajthat.<br/><br/> Ha egy meglévő előfizetést használ, és Ön nem a rendszergazda, kérjen a rendszergazdától tulajdonosi vagy közreműködői jogosultságot rendelhet, szeretne.<br/><br/> Ha részletesebb engedélyek van szüksége, tekintse át a [Ez a cikk](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastruktúra** | [Ismerje meg, hogyan](contoso-migration-infrastructure.md) Contoso beállítása az Azure-infrastruktúrát.<br/><br/> További tudnivalók a specifikus [hálózati](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) és [tárolási](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) a Site Recovery követelményeinek.
**A Site recovery (helyszíni)** | A helyszíni vCenter server 5.5-ös, 6.0-s vagy 6.5-ös verzió kell futnia<br/><br/> Egy 5.5-ös, 6.0-s vagy 6.5-ös verziójú ESXi-gazdagép<br/><br/> Egy vagy több futtató VMware virtuális gépeket az ESXi-gazdagépen.<br/><br/> Meg kell felelnie a virtuális gépek [Azure-követelmények](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Támogatott [hálózati](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) és [tárolási](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) konfigurációja.<br/><br/> Meg kell felelnie a replikálni kívánt virtuális gépek [Azure-követelmények](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).
**A DMS** | A DMS szüksége lesz egy [kompatibilis a helyszíni VPN-eszköz](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Konfigurálhatja a helyszíni VPN-eszköznek kell lennie. Egy kifelé irányuló, nyilvános IPv4-címmel kell rendelkeznie, és a cím nem található a NAT-eszköz mögött.<br/><br/> Ellenőrizze, hogy hozzáférése van a helyszíni SQL Server-adatbázisát.<br/><br/> A Windows tűzfal eléréséhez a forrás adatbázismotor képesnek kell lennie. [További információk](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Ha a tűzfal elé adatbázis számítógépe, engedélyezi a hozzáférést az adatbázishoz, és a fájlok SMB 445-ös porton keresztül szabályok hozzáadása.<br/><br/> A forrás SQL-kiszolgáló csatlakozik, és a cél felügyelt példány használt hitelesítő adatokat a sysadmin (rendszergazda) kiszolgálói szerepkör tagjának kell lennie.<br/><br/> Kell egy hálózati megosztás DMS használatával a forrás-adatbázis biztonsági mentése a helyi adatbázisban.<br/><br/> Győződjön meg arról, hogy az a forrás SQL Server-példányt futtató szolgáltatásfiók írási jogosultságokkal rendelkezik a hálózati megosztáson.<br/><br/> Jegyezze fel a Windows-felhasználó (és jelszó), amely teljes körű jogosultságokkal rendelkezik a hálózati megosztáson. Az Azure Database Migration Service megszemélyesíti a felhasználói hitelesítő adatokat a biztonsági mentési fájlok feltöltése az Azure storage-tárolóba.<br/><br/> Az SQL Server Express telepítési folyamat beállítja a TCP/IP protokoll **letiltott** alapértelmezés szerint. Győződjön meg arról, hogy engedélyezve van.


## <a name="scenario-steps"></a>A forgatókönyv lépései

Itt látható, hogyan Contoso fog futni az áttelepítési:

> [!div class="checklist"]
> * **1. lépés: Az SQL Server virtuális gépek létrehozása az Azure-ban**: magas rendelkezésre állás érdekében a Contoso szeretné az Azure-ban fürtözött adatbázis üzembe helyezése. Két SQL Server virtuális gépek és az Azure belső terheléselosztó üzembe helyezése.
> * **2. lépés: A fürt üzembe helyezéséhez**: az SQL Server virtuális gépek üzembe helyezése után az Azure SQL Server-fürt előkészítéséhez.  Ezek a database fogjuk áttelepíteni az előre létrehozott fürt.
> * **3. lépés: Készítse elő a DMS**: készítse elő a DMS regisztrálják az adatbázis-Migrálás szolgáltató, hozzon létre egy DMS-példány és a egy projektet. Akkor állítsa be a közös hozzáférésű jogosultságkód (SAS) egységes erőforrás-azonosító (URI). A DMS az SA URI-t használ, amelyhez a szolgáltatás az SQL Server biztonsági mentési fájlokat tölt fel a tárfiók tárolójának eléréséhez.
> * **4. lépés: Készítse elő az Azure Site Recovery**: hoznak létre, amely tárolja a replikált adatok, és a egy Recovery Services-tároló Azure storage-fiókkal.
> * **5. lépés: A Site Recovery a helyszíni VMware előkészítése**: a virtuális gépek felderítése és az ügynök telepítési fiókokat készít elő, és készítse elő a helyszíni virtuális gépeket, hogy képesek csatlakozni az Azure virtuális gépek a feladatátvételt követően.
> * **6. lépés: A gépek replikálása**: replikációs beállítások konfigurálása, és engedélyezze a virtuális gép replikációja.
> * **7. lépés: A DMS-adatbázis áttelepítése**: nekik át az adatbázist.
> * **8. lépés: A Site Recovery a virtuális gépek áttelepítése az**: először győződjön meg arról, hogy minden megfelelően működik, a virtuális gép migrálásához teljes feladatátvétel követ, a feladatátvételi teszt futtatásához.


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>1. lépés: Készítse elő az SQL Server AlwaysOn rendelkezésre állási csoport fürt

A Contoso biztosítani szeretné az adatbázis egy fürt az Azure-beli üzembe helyezésével tervez. Ehhez a fürthöz tartozó adatbázisok indítására használhatja. 

- Az SQL Server virtuális gépek telepíti (éles erőforrásait használt) ContosoRG az erőforráscsoportban.
- Egyedi nevek szereplőkkel mindkét virtuális gép használja ugyanazokat a beállításokat.
- A belső terheléselosztó lesz telepítve, a ContosoNetworkingRG (használt hálózati erőforrások).
- Virtuális gépek Windows Server 2016-ban működik az SQL Server 2017 Enterprise Edition. Contoso nem rendelkezik licenccel az operációs rendszer, így azok képet fogja használni az Azure Marketplace-en, amely biztosítja az Azure nagyvállalati szerződéssel rendelkező elkötelezettségüket díj a licenc.

Itt látható, hogy Contoso hogyan állította be a fürt:

1. Akkor hozzon létre két SQL Server virtuális gépek válassza ki az SQL Server 2017 Enterprise Windows Server 2016 rendszerképet az Azure Marketplace-en. 

    ![AZ SQL VIRTUÁLIS GÉP TERMÉKVÁLTOZATA](media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. Az a **virtuális gép varázsló létrehozása** > **alapjai**, azok konfigurálása:

    - A virtuális gépek nevei: **SQLAOG1** és **SQLAOG2**.
    - Mivel gépek üzleti szempontból kritikus fontosságú, azok SSD engedélyezése esetén a virtuális merevlemez típusa.
    - Akkor adja meg a gép hitelesítő adatainak.
    - Ezek az elsődleges USA keleti RÉGIÓJA 2 virtuális gépek üzembe helyezése régió ContosoRG az erőforráscsoportban.

3. A **mérete**, mindkét virtuális gépek indítása D2s_V3 termékváltozatú. Ezek később szükség szerint skálázható.
4. A **beállítások**, akkor tegye a következőket:

    - Mivel ezek a virtuális gépek az alkalmazás foglalja magába, felügyelt lemezeket használnak.
    - Ezek a gépet helyez az éles hálózati környezetben, az USA keleti RÉGIÓJA 2 elsődleges régió (**VNET-ÉLES-EUS2**), az adatbázis-alhálózat (**ÉLES-DB-EUS2**).
    - Akkor hozzon létre egy új rendelkezésre állási csoportot: **SQLAOGAVSET**, a két tartalék tartomány és öt frissítési tartománnyal.

    ![SQL VIRTUÁLIS GÉP](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. A **SQL Server-beállítások**, korlátozhatja a virtuális hálózathoz (személyes), SQL-kapcsolat az alapértelmezett 1433-as porton. A hitelesítési, használhatja ugyanazokat a hitelesítő adatokat használnak a helyszíni (**contosoadmin**).

    ![SQL VIRTUÁLIS GÉP](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**További segítségre van szüksége?**

- [Segítség kérése](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) egy SQL Server rendszerű virtuális gép kiépítése.
- [Ismerje meg](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) konfigurálása a virtuális gépek különböző SQL Server-termékváltozatok esetében.

## <a name="step-2-deploy-the-failover-cluster"></a>2. lépés: A feladatátvevő fürt üzembe helyezése

Itt látható, hogy Contoso hogyan állította be a fürt:

1. Állítsa be Azure storage-fiók, a felhőbeli tanúsító funkcionálni.
2. Az SQL Server virtuális gépek megnövelik a Contoso a helyi adatközpontban az Active Directory-tartományhoz.
3. Ezek a fürt létrehozása az Azure-ban.
4. A felhőbeli tanúsító azokat.
5. Végül SQL Always On rendelkezésre állási csoportok lehetővé teszik.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Felhőbeli tanúsító a storage-fiók beállítása

Felhőbeli tanúsító beállításával kapcsolatban a Contoso cégnek szüksége van az Azure Storage-fiókot, amely tartalmazza majd a fürt választottbírósági használt blob fájlt. Ugyanazt a tárfiókot a felhőbeli tanúsító több fürt beállításához használható. 

Contoso hoz létre egy storage-fiókot a következő:

1. A fiók egy felismerhető nevet kell adnia (**contosocloudwitness**).
2. Az LRS egy általános univerzális fiók azokat üzembe.
3. A fiók, helyezze a harmadik régiót – USA déli középső Régiójában. Ezek helyezze el az elsődleges és másodlagos régión kívül, hogy a regionális hiba esetén elérhető marad.
4. Helyezze az erőforráscsoportban, tároló - infrastruktúra-erőforrások **ContosoInfraRG**.

    ![Felhőbeli tanúsító](media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. Amikor hoznak létre a tárfiók elsődleges és másodlagos elérési kulcsot hoz létre, a. Az elsődleges elérési kulcs létrehozásához a felhőbeli tanúsító van szükségük. A tárfiók neve megjelenik a kulcs > **Tárelérési kulcsok**.

    ![Hozzáférési kulcs](media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>SQL Server virtuális gépek hozzáadása a Contoso tartományhoz

1. Contoso SQLAOG1 és SQLAOG2 hozzáadja a contoso.com tartományhoz.
2. Ezt követően az egyes virtuális Gépeken, a Windows feladatátvételi fürtszolgáltatás és eszközök telepítése.

## <a name="set-up-the-cluster"></a>A fürt beállítása

Mielőtt beállítaná a fürt, Contoso pillanatképet készít az egyes gépek az operációsrendszer-lemez.

![pillanatkép](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

2. Ezután futnak, hogy helyezze parancsfájl a Windows feladatátvevő fürt létrehozása.

    ![Fürt létrehozása](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

3. Miután a fürt az általuk létrehozott, azok győződjön meg arról, hogy a virtuális gépek fürtcsomópontokként jelennek meg.

     ![Fürt létrehozása](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>A felhőbeli tanúsító konfigurálása

1. Contoso konfigurálása a felhőbeli tanúsító – segítségével a **fürtkvórum konfigurálása varázsló** a Feladatátvevőfürt-kezelőben.
2. A varázsló felhőbeli tanúsító létrehozása a storage-fiók kiválasztásával.
3. A felhőbeli tanúsító konfigurálása után jelenik meg a Feladatátvevőfürt-kezelő beépülő modulban.

    ![Felhőbeli tanúsító](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

## <a name="enable-sql-server-always-on-availability-groups"></a>SQL Server Always On rendelkezésre állási csoportok engedélyezése

Most már engedélyezheti a Contoso Always On:

1. Az SQL Server Configuration Manager, a használatukkal **AlwaysOn rendelkezésre állási csoportok** számára a **SQL Server (MSSQLSERVER)** szolgáltatás.

    ![Engedélyezze az AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. A szolgáltatás a módosítások érvénybe léptetéséhez újraindítás.

AlwaysOn engedélyezés, a Contoso az AlwaysOn rendelkezésre állási csoport, amely védeni fogja a SmartHotel adatbázis is beállíthatja.

**További segítségre van szüksége?**

- [További információ](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) felhőbeli tanúsító és a hozzá tartozó tárfiók beállítása.
- [Útmutatásért](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) -fürt beállítása és a egy rendelkezésre állási csoport létrehozása.

## <a name="step-3-deploy-the-azure-load-balancer"></a>3. lépés: Az Azure Load Balancer üzembe helyezése

Contoso most szeretne telepíteni egy belső terheléselosztót, amely a fürt csomópontok előtt helyezkedik el. A load balancer figyeli a forgalmat, és átirányítja a megfelelő csomópontra.

![Terheléselosztás](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Ezek a terheléselosztót a következőképpen hozhat létre:

1. Az Azure portal > **hálózatkezelés** > **Load Balancer**, azokat egy új belső terheléselosztó beállítása: **ILB-ÉLES-DB-EUS2-SQLAOG**.
2. Az éles hálózati környezetben, helyezze a terheléselosztó **VNET-ÉLES-EUS2**, az adatbázis-alhálózat **ÉLES-DB-EUS2**.
3. Ezek a statikus IP-címet rendeljen hozzá: 10.245.40.100.
4. Hálózati elem, mint a hálózati erőforráscsoporthoz tartozik, a terheléselosztó üzembe **ContosoNetworkingRG**.

    ![Terheléselosztás](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

A belső terheléselosztó üzembe helyezését követően Contoso kell állítsa be. Ezek hozza létre a háttércímkészletet, állítsa be az állapotfigyelő mintavételező, és konfiguráljon egy terheléselosztási szabályt.

### <a name="add-a-backend-pool"></a>Háttérkészlet hozzáadása

Ossza el a forgalmat a virtuális gépek a fürt, a Contoso beállítása egy háttér címkészletet, amely tartalmazza a hálózati adapter IP-címeket a virtuális gépek, amelyek a terheléselosztóról érkező hálózati forgalom fog kapni.

1. A load balancer beállításokat a portálon, a Contoso egy háttérkészlet hozzáadása: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. Azok a készlet rendelkezésre állási csoport SQLAOGAVSET társítani. A virtuális gépet a csoportban (**SQLAOG1** és **SQLAOG2**) hozzáadódnak a készlet.

    ![Háttérkészlet](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Állapotminta létrehozása

Contoso hoz létre egy állapotmintát, hogy a terheléselosztó az alkalmazás állapotát figyelhesse. A mintavétel dinamikusan eltávolítása vagy virtuális gépek load balancer elforgatási, állapot-ellenőrzések válaszoljanak hogyan alapján.

Ezek a mintavétel a következőképpen hozhat létre: 

1. A load balancer beállításokat a portálon, a Contoso egy állapotmintát hoz létre: **SQLAlwaysOnEndPointProbe**.
2. Akkor állítsa be a mintavétel 59999 TCP-porton lévő virtuális gépek figyelése.
3. Minták és a egy 2-es küszöbértéket 5 másodpercenként időközt állítsa azokat. Ha két mintavételek meghibásodik, a virtuális gép nem megfelelő állapotú akkor minősül.

    ![Mintavétel](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>Forgalom fogadására a load balancer konfigurálása


Most Contoso igényeinek megfelelően, egy terheléselosztó-szabályt defins forgalom hogyan ossza el a virtuális gépeket.

- Az előtér-IP-cím a bejövő forgalmat kezeli.
- A háttérbeli IP-címkészlet fogadja a forgalmat.

Akkor hozza létre a szabályt a következő:

1. A load balancer beállításokat a portálon, az új terheléselosztási szabály hozzáadása: **SQLAlwaysOnEndPointListener**.
2. Contoso beállítja a TCP 1433-as bejövő SQL ügyfél forgalom fogadásához egy előtér-figyelőt.
3. Adatforgalom lesznek irányítva, és a portot, amelyen a virtuális gépek figyeli a forgalmat a háttérkészlet kell adnia.
4. Contoso lehetővé teszi, hogy a fix IP-Címek (közvetlen kiszolgálói válasz). Ez mindig szükség az SQL AlwaysOn-hoz.

    ![Mintavétel](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**További segítségre van szüksége?**

- [Áttekintés](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) Azure Load Balancer.
- [Ismerje meg](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) terheléselosztó létrehozása.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>4. lépés: A Site Recovery szolgáltatással az Azure előkészítése

Az alábbiakban a Contoso cégnek szüksége van, a Site Recovery üzembe helyezése az Azure-összetevők:

- Egy virtuális hálózatot, amelyben virtuális gépeket fogja tartalmazni, amikor a feladatátvétel során hoznak létre.
- A replikált adatok tárolásához Azure storage-fiókkal. 
- Az Azure Recovery Services-tárolóba.

Hogy ezek az alábbiak szerint állíthatja:

1.  Már létrehozott egy Site Recovery használhatnak hálózat/alhálózat contoso amikor azok [üzembe helyezve az Azure-infrastruktúra](contoso-migration-rehost-vm-sql-ag.md).

    - A SmartHotel alkalmazás éles alkalmazások, és WEBVM migrálása az Azure éles hálózati (VNET-ÉLES-EUS2) az elsődleges régióban USA 2. keleti régiója.
    - WEBVM kerülnek a ContosoRG erőforráscsoportban, amely használható éles erőforrásait, és az éles alhálózat (ÉLES-FE-EUS2).

2. Contoso az elsődleges régióban hoz létre egy Azure storage-fiók (contosovmsacc20180528).

    - Egy általános célú fiók, és standard szintű storage, LRS-replikációval használnak.
    - A fióknak és a tárolónak ugyanabban a régióban kell lennie.

    ![Site Recovery-tároló](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. A helyen lévő hálózati és tárolási fiókkal, akkor most hozzon létre egy Recovery Services-tároló (**ContosoMigrationVault**), és elhelyezheti a **ContosoFailoverRG** erőforráscsoport, az elsődleges régióban USA keleti RÉGIÓJA 2 .

    ![Recovery Services-tároló](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**További segítségre van szüksége?**

[Ismerje meg](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) beállítása az Azure Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>4. lépés: A Site Recovery a helyszíni VMware előkészítése

Mi Contoso előkészíti a helyszíni:

- Egy fiók a vCenter-kiszolgáló vagy vSphere ESXi-gazdagép, virtuális gépek felderítésének automatizálásához.
- Egy fiók, amely lehetővé teszi, hogy az automatikus telepíteni a mobilitási szolgáltatást a replikálni kívánt VMware virtuális gépeket.
- A helyszíni virtuális gép beállításait, úgy, hogy a Contoso az Azure a replikált virtuális gép a feladatátvételt követően csatlakozhatnak.


### <a name="prepare-an-account-for-automatic-discovery"></a>Fiók előkészítése automatikus felderítéshez

A Site Recoverynek hozzáféréssel kell rendelkeznie a VMware-kiszolgálókhoz az alábbiak érdekében:

- A virtuális gépek automatikus felderítése. 
- Replikáció, feladatátvétel és feladat-visszavétel vezénylése.
- Szükség van legalább egy csak olvasási jogokat biztosító fiókra. Futtathatja a műveletek, például a létrehozása és eltávolítása a lemezeket, és ne tudják bekapcsolni a virtuális gépek fiók szükséges.

Contoso a következőképpen állítja be a fiók:

1. Contoso egy szerepkört a vCenter-szinten hoz létre.
2. Contoso majd hozzárendeli a szerepkört a szükséges engedélyekkel.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Fiók előkészítése a mobilitási szolgáltatás telepítéséhez

A mobilitási szolgáltatás minden virtuális Géphez telepítve kell lennie.

- A Site Recovery egy automatikus ügyfélleküldéses telepítés összetevő teheti meg, amikor a replikáció engedélyezve van a virtuális gép.
- A Site Recovery eléri a virtuális Gépet, a leküldéses telepítéshez használható fiók szükséges. Ezt a fiókot adja meg, ha az Azure-konzolon a replikáció beállítása.
- A fiók lehet tartomány vagy helyi, amely engedéllyel rendelkezik a virtuális Gépre telepíteni.




### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Felkészülés az Azure virtuális gépekhez való kapcsolódásra a feladatátvételt követően

A feladatátvételt követően a Contoso biztosítani szeretné az Azure virtuális géphez való kapcsolódásra. Ehhez, tegye a következőket áttelepítés előtt:

1. Az interneten keresztüli eléréshez azokat:

 - A feladatátvétel előtt a helyszíni virtuális gép RDP engedélyezése
 - Győződjön meg arról, hogy a TCP és UDP-szabályokat a adják a **nyilvános** profilt.
 - Ellenőrizze, hogy az RDP engedélyezve van-e **Windows tűzfal** > **engedélyezett alkalmazások** az összes profil számára.
 
2. Site-to-site VPN, keresztüli eléréshez azokat:

 - Engedélyezze az RDP a helyszíni gépen.
 - Az RDP engedélyezése a **Windows tűzfal** -> **engedélyezett alkalmazások és szolgáltatások**, a **tartomány és privát** hálózatok.
 - Az operációs rendszer TÁROLÓHÁLÓZATI szabályzatát állítsa a helyszíni virtuális gép **OnlineAll**.

Emellett a feladatátvétel futtatásakor szükségük ellenőrizze az alábbiakat:

- Lehetnek nincsenek függőben lévő Windows-frissítések a virtuális gép feladatátvétel indítása során. Ha vannak, be tud jelentkezni a virtuális Gépet a frissítés befejeződéséig nem.
- A feladatátvételt követően ellenőrizheti **rendszerindítási diagnosztika** , a virtuális gép képernyőképének megtekintéséhez. Ha ez sem működik, akkor ellenőrizze, hogy a virtuális gép fut, és tekintse át a [hibaelhárítási tippek](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**További segítségre van szüksége?**

- [Ismerje meg](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) létrehozása és hozzárendelése egy szerepkört a automatikus felderítése.
- [Ismerje meg](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) hozzon létre egy fiókot a mobilitási szolgáltatás leküldéses telepítéséhez.


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>5. lépés: A helyszíni virtuális gépek replikálása az Azure Site recoveryvel

Mielőtt azok lefutnának a migrálás az Azure-ba, Contoso kell beállítani, és engedélyezze a replikációt.

### <a name="set-a-replication-goal"></a>Állítsa be a replikációs cél

1. A tárolóban, a tároló nevét (ContosoVMVault) alatt, replikációs cél kiválasztása (**bevezetés** > **Site Recovery** > **infrastruktúra előkészítése** .
2. Akkor adja meg, hogy gépeik a helyszínen található, VMware-en futó, és az Azure-bA replikálja.

    ![Replikációs cél](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Az üzembe helyezés megtervezésének megerősítése

A folytatáshoz győződjön meg arról, hogy elvégezték-üzembe helyezés megtervezése, kiválasztásával szükségük **Igen, elvégeztem**. Ebben a forgatókönyvben Contoso csak a virtuális gép áttelepítése, és nem kell az üzembe helyezés megtervezése.

### <a name="set-up-the-source-environment"></a>A forráskörnyezet beállítása

A forráskörnyezet beállítása a Contoso cégnek szüksége van. Ehhez, egy OVF-sablon letöltése és használatával a Site Recovery konfigurációs kiszolgálónak a magas rendelkezésre állásúként telepíteni, a helyszíni VMware virtuális gép. Miután a konfigurációs kiszolgáló üzembe helyezéséig, azok regisztrálja a tárolóban.

A konfigurációs kiszolgálón néhány összetevő fut:

- A konfigurációs kiszolgáló összetevő koordinálja a helyszíni és Azure közötti kommunikációt, és felügyeli az adatreplikációt.
- A folyamatkiszolgáló replikációs átjáróként üzemel. Fogadja a replikációs adatokat, gyorsítótárazással, tömörítéssel és titkosítással optimalizálja őket, majd továbbítja az adatokat az Azure-tárolónak.
- A folyamatkiszolgáló ezenfelül telepíti a mobilitási szolgáltatást a replikálni kívánt virtuális gépekre, és elvégzi a helyszíni VMware virtuális gépek automatikus felderítését.

Contoso hajtsa végre ezeket a lépéseket a következő:


1. A tárolóban, az OVF-sablon letöltése **infrastruktúra előkészítése** > **forrás** > **konfigurációs kiszolgáló**.
    
    ![OVF letöltése](./media/contoso-migration-rehost-vm-sql-ag/add-cs.png)

2. Ezek a sablon létrehozása és üzembe helyezése a virtuális gép VMware importálja.

    ![OVF-sablon](./media/contoso-migration-rehost-vm-sql-ag/vcenter-wizard.png)

3. A virtuális gép első bekapcsolásakor, a bekapcsolásakor egy Windows Server 2016 telepítési folyamatot. Fogadja el a licencszerződést, és adjon meg egy rendszergazdai jelszót.
4. A telepítés befejezését követően, jelentkezzen be rendszergazdaként a virtuális géphez. Első bejelentkezéskor az Azure Site Recovery Configuration Tool alapértelmezés szerint fut.
5. Az eszközben kell adnia egy nevet a konfigurációs kiszolgáló regisztrálása a tárolóban használni.
6. Az eszköz ellenőrzi, hogy a virtuális gép tud-e csatlakozni az Azure-hoz. A kapcsolat létrejötte után bejelentkeznek az Azure-előfizetést. Olyan hitelesítő adatokra van szükség, amelyekkel hozzá lehet férni a tárolóhoz, amelyben regisztrálni kívánja a konfigurációs kiszolgálót.

    ![Konfigurációs kiszolgáló regisztrálása](./media/contoso-migration-rehost-vm-sql-ag/config-server-register2.png)

7. Az eszköz végrehajt néhány konfigurációs feladatot, majd újraindul.
8. Bejelentkeznek a gép újra, és a konfigurációs kiszolgáló felügyeleti varázslója automatikusan elindul.
9. A varázsló, válassza ki a hálózati Adaptert replikációs forgalom fogadására. Ez a beállítás a konfigurálás után nem módosítható.
10. Akkor válassza ki az előfizetést, erőforráscsoportot és a tároló, amelyben a konfigurációs kiszolgálót regisztrálja.
        ![Tároló](./media/contoso-migration-rehost-vm-sql-ag/cswiz1.png) 

10. Majd töltse le és telepítse a MySQL-kiszolgáló és a VMWare powercli-t. 
11. Ellenőrzést hogy a vCenter-kiszolgáló vagy vSphere-gazdagép teljes Tartománynevét vagy IP-címét adja meg. Hagyja bejelölve az alapértelmezett portot, és adja meg a vCenter-kiszolgáló rövid nevét.
12. A fiók automatikus felderítéshez létrehozott és a hitelesítő adatok a mobilitási szolgáltatás automatikus telepítéséhez használt kell adnia. Windows-gépek a fióknak a virtuális gépek helyi rendszergazdai jogosultsággal kell rendelkeznie.

    ![vCenter](./media/contoso-migration-rehost-vm-sql-ag/cswiz2.png)

7. Az Azure Portalon, a regisztráció befejezését követően dupla Contoso ellenőrzi, hogy a konfigurációs kiszolgáló és a VMware-kiszolgáló szerepel a a **forrás** lap a tárolóban. 15 percig vagy tovább is tarthat a felderítés. 
8. Ezután a Site Recovery VMware-kiszolgálókat a megadott beállításokkal csatlakozik, és felderíti a virtuális gépeket.

### <a name="set-up-the-target"></a>A cél beállítása

Most a Contoso megadja a cél replikációs beállításokat.

1. A **infrastruktúra előkészítése** > **cél**, akkor válassza ki a célbeállítások.
2. A Site Recovery ellenőrzi, hogy nincs-e egy Azure storage-fiók és a célként megadott hálózat.

### <a name="create-a-replication-policy"></a>Replikációs házirend létrehozása

Most a Contoso egy replikációs házirendet hozhat létre.

1. A **infrastruktúra előkészítése** > **replikációs beállítások** > **replikációs házirend** >  **létrehozás és Társítsa**, akkor hozzon létre egy házirendet **ContosoMigrationPolicy**.
2. Az alapértelmezett beállítások használata:
    - **Helyreállítási Időkorlát küszöbértéke**: alapértelmezett 60 perc. Ez az érték határozza meg, hogy milyen gyakran jönnek létre helyreállítási pontok. A rendszer riasztást ad, ha a folyamatos replikáció túllépi ezt a korlátot.
    - **Helyreállítási pont megőrzése**. Alapértelmezett 24 órányi. Ez az érték határozza meg, mennyi ideig őrzi az egyes helyreállítási pontok. A replikált virtuális gépek ezen az időtartamon belül bármikor helyreállíthatók.
    - **Alkalmazáskonzisztens pillanatkép gyakorisága**. Alapértelmezés szerint egy óra. Ez az érték, amellyel jönnek létre alkalmazáskonzisztens pillanatképek gyakorisága határozza meg.
 
        ![Replikációs házirend létrehozása](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. A szabályzat automatikusan társítva lesz a konfigurációs kiszolgálóval. 

    ![Replikációs házirend társítása](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>A replikáció engedélyezése

Most már Contoso is replikáljon WebVM.

1. A **alkalmazás replikálása** > **forrás** > **+ replikálás** , válassza ki az adatforrás-beállítások.
2. Ezek azt jelzik, hogy szeretnének-e a virtuális gépek engedélyezéséhez válassza ki a vCenter-kiszolgáló és a konfigurációs kiszolgáló.

    ![A replikáció engedélyezése](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Most akkor adja meg a cél beállításai, többek között az erőforráscsoportot és a virtuális hálózat és a tárfiókot, amely a replikált adatokat tárolni szeretné.

     ![A replikáció engedélyezése](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Contoso kiválasztja a replikáció WebVM, ellenőrzi a replikációs házirendet, és lehetővé teszi a replikációt. A Site Recovery telepíti a mobilitási szolgáltatást a virtuális gépen, ha a replikáció engedélyezve van.
 
    ![A replikáció engedélyezése](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. A replikáció állapotát nyomon követik **feladatok**. A **Védelem véglegesítése** feladat befejeződését követően a gép készen áll a feladatátvételre.
5. A **Essentials** az Azure Portalon, a Contoso a struktúra látható a virtuális gépek replikálása Azure-bA.

    ![Infrastruktúra-nézet](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**További segítségre van szüksége?**

- Tudjon meg egy teljes forgatókönyv az alábbi lépéseket a [vészhelyreállítás beállítása helyszíni VMware virtuális gépek](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Részletes útmutatás segítségével érhetők el [a forráskörnyezet beállítása](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [használt konfigurációs kiszolgáló telepítése](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), és [replikációs beállítások konfigurálása](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- További információ [replikálást](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-install-the-database-migration-assistant-dma"></a>5. lépés: A Data Migration Assistant (DMA) telepítése

Contoso lesz a SmartHotel adatbázis migrálása az Azure virtuális gép **SQLAOG1** a DMA segítségével. Ezek DMA az alábbiak szerint állíthatja:

1. Töltse le az eszközt a [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) , a helyszíni SQL Server rendszerű virtuális gép (**SQLVM**).
2. A virtuális gépen futnak a telepítés (DownloadMigrationAssistant.msi).
3. Az a **Befejezés** lapon válasszon **indítsa el a Microsoft Data Migration Assistant** a varázsló befejezése előtt.

## <a name="step-6-migrate-the-database-with-dma"></a>6. lépés: A DMA-adatbázis áttelepítése

1. A DMA-ban futnak, egy új migrálást - **SmartHotel**.
2. Akkor válassza ki a **célkiszolgáló típusa** , **SQL Server Azure virtuális gépeken**. 

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-1.png)

3. Az áttelepítési adatokat hozzáadása **SQLVM** a forráskiszolgálóéval, és **SQLAOG1** célként. Az egyes gépek hitelesítő adatokat kell adnia.

     ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-2.png)

4. Akkor hozzon létre egy helyi megosztás, adatbázis- és konfigurációs adatok. Elérhető-e írási hozzáférése az SQL szolgáltatás fiókonként SQLVM és SQLAOG1 kell lennie.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-3.png)

5. Contoso a bejelentkezések kell áttelepíteni, és az áttelepítést elindító választja ki. Befejezését követően a DMA az áttelepítés sikeres jeleníti meg.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-4.png)

6. Ezek a cégek ellenőrzik, hogy az adatbázis fut-e a **SQLAOG1**.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-5.png)

A DMS a Contoso-datacenter és az Azure között helyek közötti VPN-kapcsolat a helyszíni SQL Server rendszerű virtuális gép csatlakozik, és ezután áttelepíti az adatbázisban.

## <a name="step-7-protect-the-database"></a>7. lépés: Az adatbázis védelmét.

A futó alkalmazás adatbázissal **SQLAOG1**, Contoso mostantól védhetik AlwaysOn rendelkezésre állási csoportok használatával. Konfigurálja az AlwaysOn az SQL Management Studio használatával, és rendelje hozzá egy figyelőt, Windows-fürtszolgáltatás használatával. 

### <a name="create-an-alwayson-availability-group"></a>AlwaysOn rendelkezésre állási csoport létrehozása

1. Az SQL Management Studióban, kattintson a jobb gombbal a **mindig a magas rendelkezésre állású** elindításához a **új rendelkezésre állási csoport varázsló**.
2. A **beállítások megadása**, akkor nevezze el a rendelkezésre állási csoport **SHAOG**. A **adatbázisok válassza**, akkor válassza ki, hogy a SmartHotel adatbázist.

    ![AlwaysOn rendelkezésre állási csoport](media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. A **replikák megadása**, adja hozzá a két csomópont SQL rendelkezésre állási másodpéldányok, és konfigurálhatja azokat a szinkron véglegesítés automatikus feladatátvételt biztosít.

     ![AlwaysOn rendelkezésre állási csoport](media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. Azok a csoportot egy figyelő konfigurálása (**SHAOG**) és a portot. A belső terheléselosztó IP-címet egy statikus IP-cím (10.245.40.100) van hozzáadva.

    ![AlwaysOn rendelkezésre állási csoport](media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. A **adatszinkronizálás kiválasztása**, a használatukkal az automatikus összehangolása. Ezzel a beállítással az SQL Server hozza létre automatikusan a másodlagos replikák minden adatbázis a csoport így Contoso manuálisan biztonsági mentése és visszaállítása ezeket nem kell. Ellenőrzést követően a rendelkezésre állási csoport jön létre.

    ![AlwaysOn rendelkezésre állási csoport](media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Contoso hibába ütközött egy problémát, a csoport létrehozásakor. Nem használ Active Directory Windows beépített biztonsági, és így kell az SQL-bejelentkezés a Windows feladatátvevő fürt szerepkörök létrehozásához szükséges engedélyek biztosítása.

    ![AlwaysOn rendelkezésre állási csoport](media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

6. A csoport létrehozása után a Contoso láthatják őket az SQL Management Studióban.

### <a name="configure-a-listener-on-the-cluster"></a>Egy figyelő konfigurálása a fürtön

Az SQL-telepítés beállítása az utolsó lépésben a Contoso a belső terheléselosztó konfigurálja a figyelőt a fürtön, és elérhetővé teszi a figyelő online. Egy parancsfájlt ennek használnak.

![Fürt-figyelő](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>Ellenőrizze

Mindent, a Contoso most már rendelkezik egy funkcionális rendelkezésre állási csoportot az Azure-ban, amely a migrált adatbázis használja. Ez a belső terheléselosztó az SQL Management Studióban való csatlakozással azok ellenőrzése.

![ILB-csatlakozás](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**További segítségre van szüksége?**
- Létrehozásának elsajátítása egy [rendelkezésre állási csoport](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) és [figyelő](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- Manuálisan [a fürt beállítása a terheléselosztó IP-címe használandó](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address).
- [További](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) létrehozásáról és a SAS használatával.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>8. lépés: A Site Recovery a virtuális gép áttelepítése

Contoso futtatása egy gyors feladatátvételi teszt, és ezután migrálja a virtuális Gépet.

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

Feladatátvételi teszt futtatása gondoskodik arról, hogy minden a várt módon működik az áttelepítés előtt. 

1. Contoso fut egy feladatátvételi tesztet a legújabb elérhető pontra idő (**legutóbb feldolgozott**).
2. Kiválasztják **gép leállítása a feladatátvétel megkezdése előtt**, hogy a Site Recovery megkísérli a forrásoldali virtuális gép leállítása a feladatátvétel indítása előtt. A feladatátvételi akkor is folytatódik, ha a leállítás meghiúsul. 
3. Teszt feladatátvétel futtatása: 

    - Ellenőrizze, hogy az áttelepítéshez szükséges feltételek vannak érvényben lefuttatja az előfeltételek ellenőrzését.
    - A feladatátvétel feldolgozza az adatokat, hogy az Azure-beli virtuális gép létrehozható legyen. Ha a legutóbbi helyreállítási pont van kiválasztva, a rendszer egy helyreállítási pontot hoz létre az adatokból.
    - A rendszer létrehoz egy Azure-beli virtuális gépet az előző lépésben feldolgozott adatok használatával.

3. A feladatátvétel befejezését követően a replika Azure virtuális gép megjelenik az Azure Portalon. Contoso ellenőrzi, hogy a virtuális gép a megfelelő méret, amely a megfelelő hálózathoz csatlakoztatva van és fut-e. 
4. Miután ellenőrizte, Contoso tisztítása a feladatátvétel, és jegyezze fel, és mentse a kapcsolatos megfigyelések. 

### <a name="run-a-failover"></a>Feladatátvétel futtatása

1. Miután ellenőrizte, hogy a feladatátvételi teszt a várt módon dolgozni, a Contoso hozzon létre egy helyreállítási terv áttelepítése és WEBVM ad hozzá a csomagot.

     ![Helyreállítási terv](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Feladatátvétel a terv számítógépen futnak. Válassza ki a legutóbbi helyreállítási pontot, és adja meg, hogy a Site Recovery kísérelje meg a helyszíni virtuális gép leállítása a feladatátvétel indítása előtt.

    ![Feladatátvétel](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. A feladatátvétel után, győződjön meg arról, hogy az Azure virtuális gép megjelenik-e a várt módon az Azure Portalon.

    ![Helyreállítási terv](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Miután ellenőrizte a virtuális gép az Azure-ban, ezek az áttelepítéshez a migrálási folyamat befejezéséhez, állítsa le a virtuális gép replikálását, és állítsa le a virtuális gép Site Recovery-számlázását.

    ![Feladatátvétel](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>A kapcsolati karakterlánc frissítése

Az áttelepítési folyamat utolsó lépését, mint a Contoso frissítse a kapcsolati karakterláncot, az alkalmazás a migrált adatbázis, a SHAOG figyelő futó mutasson. Ez a konfiguráció az Azure-ban már futó WEBVM a módosulnak.  Ez a konfiguráció a Web.config fájl az ASP-alkalmazás található. 

1. Keresse meg a fájlban a következő C:\inetpub\SmartHotelWeb\web.config.  Módosítsa megfelelően a AOG teljes Tartománynevét a kiszolgáló nevét: shaog.contoso.com.

    ![Feladatátvétel](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. A fájl frissítése és a mentés után, indítsa újra az IIS a WEBVM. Ezek ehhez az IISRESET /RESTART cmd parancsot a parancssorba.
2. Az IIS újraindítása után, az alkalmazás most már használ az adatbázis az SQL Buszpéldány futtat.


**További segítségre van szüksége?**

- [Ismerje meg](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) feladatátvételi teszt futtatása. 
- [Ismerje meg,](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) a helyreállítási terv létrehozása.
- [Ismerje meg](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) – Azure feladatátvétele.

## <a name="clean-up-after-migration"></a>Áttelepítés utáni karbantartás

Az áttelepítés után a SmartHotel app-beli virtuális gépen fut, és a SmartHotel adatbázis az Azure SQL-fürt található.

Most már van szüksége a Contoso karbantartása a lépések elvégzéséhez:  

- Távolítsa el a helyszíni virtuális gépek a vCenter-készlet.
- Távolítsa el a virtuális gépek helyi biztonsági mentési feladatok.
- Frissítés megjelenítése az új helyek és IP-címek a virtuális gépek belső dokumentációjában.
- Tekintse át az erőforrásokat, amelyek interakciót folytatni a leszerelt virtuális gépeket, és frissítse a bármely vonatkozó beállítások vagy dokumentáció, hogy tükrözzék az új konfigurációt.
- Adja hozzá az éles rendszerek figyelése (SQLAOG1 és SQLAOG2) a két új virtuális gépet fel kell venni.

## <a name="review-the-deployment"></a>Tekintse át a központi telepítés

Az áttelepített erőforrások az Azure-ban, a Contoso cégnek szüksége van, teljes mértékben üzembe helyezése, és tegye biztonságossá a új infrastruktúrára.

### <a name="security"></a>Biztonság

A Contoso biztonsági csapat áttekinti az Azure-beli virtuális gépek WEBVM, SQLAOG1 és SQLAOG2 bármilyen biztonsági problémák meghatározása érdekében. 

- Azok a hálózati biztonsági csoportok (NSG-k) tekintse át a virtuális géphez való hozzáférés szabályozása. Az NSG-k segítségével győződjön meg arról, hogy csak az alkalmazás engedélyezett forgalom adhat át.
- Fontolgatják biztonságossá tétele a KeyVault és az Azure Disk Encryption használatával a lemezen lévő adatokat.
- Azok kell kiértékelni a transzparens adattitkosítás (TDE), és engedélyez az SmartHotel adatbázisban az új SQL-AOG futó. [További információk](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017).

[További információ](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) virtuális gépek biztonsági eljárásairól.

### <a name="backups"></a>Biztonsági másolatok

Az adatok biztonsági mentése WEBVM SQLAOG1 és SQLAOG2 lévő contoso fogja az Azure Backup szolgáltatással. [További információk](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licencelési és a költségek optimalizálása

1. A Contoso azok WEBVM meglévő licencelésből rendelkezik, és használja az Azure Hybrid Benefit.  Váltás lesz a meglévő Azure virtuális gépek, a díjszabás előnyeinek kihasználása érdekében.
2. Contoso lehetővé teszi a Cloudyn, a Microsoft egy leányvállalata által licencelt Azure Cost Managementbe. Egy többfelhős költségkezelő felügyeleti megoldás, amely segítséget nyújt az Azure és egyéb felhőerőforrások kezelése, és a.  [További](https://docs.microsoft.com/azure/cost-management/overview) kapcsolatos Azure Cost Managementbe. 

## <a name="conclusion"></a>Összegzés

Ebben a cikkben a Contoso rehosted által az alkalmazás előtérbeli virtuális gép migrálása az Azure-bA a Site Recovery szolgáltatással az SmartHotel alkalmazást az Azure-ban. Az alkalmazás-adatbázis áttelepítése az SQL Server-fürt kiépítése az Azure-ban, és védi, egy SQL Server AlwaysOn rendelkezésre állási csoportban.

## <a name="next-steps"></a>További lépések

A sorozat következő cikkben hogy megmutatjuk, hogyan a Contoso a linuxon futó szolgáltatás desk osTicket alkalmazás újratárolása, és egy MySQL-adatbázis telepítve.


