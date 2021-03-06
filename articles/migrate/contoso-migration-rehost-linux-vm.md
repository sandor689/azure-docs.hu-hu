---
title: Áthelyezési át, és a egy helyszíni Linux alkalmazás újratárolása az Azure virtuális gépek |} A Microsoft Docs
description: Ismerje meg, hogyan Contoso egy helyszíni Linux alkalmazás újratárolása az Azure virtuális gépek áttelepítése révén.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 6c96beee347a7a36a3dc04ecf8cd994484fd6bb7
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007250"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Contoso áttelepítési: egy a helyszíni Linux alkalmazás Újratárolása az Azure virtuális gépek

Ez a cikk bemutatja, hogyan Contoso van áthelyezését a helyszíni Linux-alapú szolgáltatás desk alkalmazás (**osTicket**), az Azure IaaS virtuális gépeire.

Ez a dokumentum egyike egy sorozat, amely dokumentálja a fiktív Contoso hogyan a helyszíni erőforrásokkal áttelepíti a Microsoft Azure felhőbe. A sorozat része a háttér-információkat és forgatókönyvek csoportja, amely azt ábrázolja, hogyan egy áttelepítési infrastruktúra beállítását, és futtassa a különböző típusú áttelepítéseket. Forgatókönyvek egyre összetettebbé válnak, és adunk hozzá további cikkek idővel.

**Cikk** | **Részletek** | **Állapot**
--- | --- | ---
[1. cikk: áttekintés](contoso-migration-overview.md) | Contoso-áttelepítési stratégia, a cikk sorozat és a mintaalkalmazások használjuk áttekintést nyújt. | Elérhető
[2. cikk: Egy Azure-infrastruktúra üzembe helyezése](contoso-migration-infrastructure.md) | Ismerteti, hogyan Contoso előkészíti a helyszíni és az Azure-infrastruktúra az áttelepítéshez. Az összes Contoso áttelepítési forgatókönyvek ugyanazon az infrastruktúrán használható. | Elérhető
[3. cikk: A helyszíni erőforrások értékelése](contoso-migration-assessment.md)  | Bemutatja, hogyan Contoso fut a VMware-en futó helyszíni kétrétegű SmartHotel alkalmazás értékelése. Mérje fel az alkalmazás virtuális gépek a [Azure Migrate](migrate-overview.md) szolgáltatás és az alkalmazás SQL Server-adatbázisnak a [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Elérhető
[4. cikk: Áthelyezési Azure virtuális gépek és a egy felügyelt SQL-példány](contoso-migration-rehost-vm-sql-managed-instance.md) | Bemutatja, hogyan Contoso áttelepíti az Azure-bA a SmartHotel alkalmazást. Az alkalmazás előtérbeli virtuális gépet át [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), és az adatbázis használatával a [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) szolgáltatás migrálása a felügyelt SQL-példányt. | Elérhető
[Cikk 5: Az Azure virtuális gépeken áthelyezési](contoso-migration-rehost-vm.md) | Bemutatja, hogyan Contoso SmartHotel alkalmazásaikról virtuális gépek áttelepítése Azure virtuális gépeken. | Elérhető
[A cikk 6: Újratárolás az Azure virtuális gépek és az SQL Server rendelkezésre állási csoportok](contoso-migration-rehost-vm-sql-ag.md) | Bemutatja, hogyan telepíti át a Contoso a SmartHotel alkalmazást. A Site Recovery számára, hogy az alkalmazás virtuális gépeit és a egy SQL Server rendelkezésre állási csoportot az alkalmazás-adatbázis áttelepítése a Database Migration service használnak. | Elérhető
7. cikk: Egy Linux alkalmazás Újratárolása az Azure virtuális gépek | Bemutatja, hogyan telepíti át a Contoso osService Linux alkalmazás az Azure Site Recovery használatával. | Ez a cikk.
[A cikk 8: Egy Linux alkalmazás Újratárolása az Azure virtuális gépek és az Azure MySQL-kiszolgáló](contoso-migration-rehost-linux-vm-mysql.md) | Bemutatja, hogyan Contoso áttelepíti a osService Linux-alkalmazás, virtuális gépek migrálása esetében a Site Recovery és a MySQL Workbench használatával történő áttelepítésének (az Azure MySQL Server-példány. | Elérhető
[9. cikk: Újrabontás egy alkalmazást az Azure Web Apps és az Azure SQL database](contoso-migration-refactor-web-app-sql.md) | Bemutatja, hogyan Contoso a SmartHotel alkalmazást áttelepíti egy Azure-webalkalmazást, és az alkalmazás-adatbázis áttelepítése az Azure SQL Server-példány | Elérhető
[10. cikk: Újrabontás egy Linux-alkalmazás Azure Web Apps és az Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Bemutatja, hogyan Contoso áttelepíti a Linux-osTicket alkalmazás Azure Web Apps több helyen, a folyamatos készregyártás a GitHub integrálva. Az alkalmazás-adatbázis nekik át egy Azure-beli MySQL-példányt. | Elérhető
[11. cikk: Újrabontás a TFS-t a vsts-ben](contoso-migration-tfs-vsts.md) | Bemutatja, hogyan telepíti át a Contoso a saját helyi Team Foundation Server (TFS) központi, migrálás, a Visual Studio Team Services (VSTS) az Azure-ban. | Elérhető
[A cikk 12: Azure-tárolók és az Azure SQL Database az alkalmazás újratervezése](contoso-migration-rearchitect-container-sql.md) | Bemutatja, hogyan Contoso áttelepíti, és rearchitects SmartHotel alkalmazás az Azure-bA. Az alkalmazás webes réteg egy Windows-tárolót, és a egy Azure SQL Database-ben az alkalmazás-adatbázis újratervezése azokat. | Elérhető
[Cikk 13: Építse újra az alkalmazást az Azure-ban](contoso-migration-rebuild.md) | Bemutatja, hogyan építse újra a Contoso SmartHotel alkalmazás számos Azure-szolgáltatások és szolgáltatások, beleértve az App Services, Azure-beli Kubernetes, az Azure Functions, a Cognitive services és a Cosmos DB használatával. | Elérhető

Ebben a cikkben a Contoso áttelepíti a kétrétegű **osTicket** alkalmazást, a Linuxos Apache MySQL PHP (LAMP) futtató Azure-bA. Az alkalmazás virtuális gépek áttelepíthetők az Azure Site Recovery szolgáltatással. Ha szeretné a nyílt forráskódú alkalmazás használja, letöltheti azt [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>A stratégiai

Az informatikai vezetőségi szorosan együttműködik az üzleti partnerek megértéséhez, amit szeretnének való áttérés eléréséhez:

- **Üzleti növekedés cím**: Contoso nő, és ennek eredményeképpen nincs nyomást a helyszíni rendszerek és infrastruktúra.
- **Korlátozza a kockázati**: A szolgáltatás desk app fontos a Contoso vállalat. Szeretné áthelyezni az Azure-bA nulla kockázattal rendelkező.
- **Kiterjesztheti**: ezek nem szeretné, hogy az alkalmazás most módosítani. Egyszerűen csak szeretné, hogy stabil.


## <a name="migration-goals"></a>Áttelepítési célok

A Contoso felhőalapú csapat rendelkezik rögzített le az áttelepítés, a leginkább megfelelő áttelepítési módszer meghatározásához célok:

- Az áttelepítés után az alkalmazás az Azure-ban kell teljesítmény ugyanazokat a lehetőségeket, mint jelenleg helyszíni VMWare környezetben.  Az alkalmazás kritikus jelzéssel a felhőben, a helyszínen marad. 
- Nem a Contoso szeretné fektethet be ezt az alkalmazást.  Fontos, hogy az üzleti, de a jelenlegi formájában egyszerűen szeretnék biztonságosan áthelyezése a felhőbe.
- Contoso nem szeretné módosítani az alkalmazás a ops modellt. Szeretné mobiljelentésre annak a felhőben ugyanúgy, mint korábban.
- Contoso nem szeretné módosítani az alkalmazás működéséhez. Az alkalmazás csak a hely változik.
- Windows app áttelepítések néhány befejezését követően, a Contoso biztosítani szeretné egy Linux-alapú infrastruktúra használata az Azure-ban.

## <a name="proposed-architecture"></a>Javasolt architektúra

Ebben a forgatókönyvben:

- Az alkalmazás többszintű (OSTICKETWEB és OSTICKETMYSQL) két virtuális gép között.
- VMware ESXi-gazdagépen található virtuális gépek **contosohost1.contoso.com** (6.5-ös verzió).
- A VMware-környezet kezeli a vCenter Server 6.5-ös (**vcenter.contoso.com**), egy virtuális gépen futó.
- Contoso rendelkezik egy helyszíni adatközpont (**contoso-datacenter**), egy helyszíni tartományvezérlővel (**contosodc1**).
- Mivel az alkalmazás egy éles környezetbeli számítási feladatokra, a virtuális gépek az Azure-ban helyezkednek el az éles erőforráscsoportban **ContosoRG**.
- A virtuális gépek lesz áttelepítve az elsődleges régió (USA keleti RÉGIÓJA 2), és elhelyezni a termelési hálózat (VNET-ÉLES-EUS2):
    - A webkiszolgáló virtuális gép az előtérben levő alhálózathoz (ÉLES-FE-EUS2) helyezkednek el.
    - A virtuális gép adatbázis az adatbázis-alhálózat (ÉLES-DB-EUS2) helyezkednek el.
- A helyszíni virtuális gépeket a Contoso-datacenter elvesznek az áttelepítés befejezése után.

![Forgatókönyv-architektúra](./media/contoso-migration-rehost-linux-vm/architecture.png) 

## <a name="migration-process"></a>Áttelepítési folyamat

Contoso migráljuk az alábbiak szerint:

1. Első lépésként Contoso állítja be az Azure és a helyszíni Site Recovery üzembe helyezéséhez szükséges infrastruktúrát.
2. Után az Azure és helyszíni összetevők előkészítése, ezek beállítása és a virtuális gépek replikálásának engedélyezése.
3. Miután a replikálás működik, azok áttelepítése a virtuális gépek feladatátvétele őket az Azure-bA.

![Áttelepítési folyamat](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Azure-szolgáltatások

**Szolgáltatás** | **Leírás** | **Költségek**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | A szolgáltatás koordinálja a migrálás és vész-helyreállítási kezeli az Azure virtuális gépek és a helyszíni virtuális gépek és fizikai kiszolgálók.  | Az Azure-ba, során Azure Storage-díjat számítunk fel.  Azure virtuális gépek jönnek létre, és a költségek, amikor feladatátvételt hajt végre. [További](https://azure.microsoft.com/pricing/details/site-recovery/) kapcsolatos díjakat és díjszabás.

 
## <a name="prerequisites"></a>Előfeltételek

Itt látható, milyen meg (és Contoso) kell ehhez a forgatókönyvhöz.

**Követelmények** | **Részletek**
--- | ---
**Azure-előfizetés** | Kell már létrehozott egy előfizetés során korai cikkek az oktatóanyag-sorozatban. Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Ha ingyenes fiókot hoz létre, Ön lesz az előfizetés rendszergazdája, és minden műveletet végrehajthat.<br/><br/> Ha egy meglévő előfizetést használ, és Ön nem a rendszergazda, kérjen a rendszergazdától tulajdonosi vagy közreműködői jogosultságot rendelhet, szeretne.<br/><br/> Ha részletesebb engedélyek van szüksége, tekintse át a [Ez a cikk](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastruktúra** | A Contoso beállítása az Azure-infrastruktúra leírtak szerint [áttelepítése az Azure-infrastruktúra](contoso-migration-infrastructure.md).<br/><br/> További tudnivalók a specifikus [hálózati](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) és [tárolási](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) a Site Recovery követelményeinek.
**Helyszíni kiszolgálók** | A helyszíni vCenter server 5.5-ös, 6.0-s vagy 6.5-ös verzió kell futnia<br/><br/> Egy 5.5-ös, 6.0-s vagy 6.5-ös verziójú ESXi-gazdagép<br/><br/> Egy vagy több futtató VMware virtuális gépeket az ESXi-gazdagépen.
**A helyszíni virtuális gépek** | [Tekintse át a Linux rendszerű gépek](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) , amely a Site Recovery áttelepítése támogatott.<br/><br/> Győződjön meg arról támogatott [Linux-fájl- és tárolási rendszerek](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Meg kell felelnie a virtuális gépek [Azure-követelmények](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>A forgatókönyv lépései

Itt látható az Azure hogyan hajtsa végre az áttelepítést:

> [!div class="checklist"]
> * **1. lépés: Készítse elő az Azure Site Recovery**: a replikált adatok tárolásához Azure storage-fiók létrehozása, és hozzon létre egy Recovery Services-tárolót.
> * **2. lépés: A Site Recovery a helyszíni VMware előkészítése**: készítse elő a virtuális gép felderítés és az ügynök telepítéséhez használandó fiókok, és készítse elő az Azure virtuális géphez való kapcsolódásra a feladatátvételt követően.
> * **3. lépés: A gépek replikálása**: azok a forrás és cél migrálási környezetet, replikációs házirend létrehozása és indítsa el a virtuális gépek replikálása az Azure storage-bA.
> * **4. lépés: A Site Recovery a virtuális gépek áttelepítése az**:, győződjön meg arról, hogy minden megfelelően működik, a feladatátvételi teszt futtatásához, és futtassa a teljes feladatátvételt az a virtuális gépek áttelepítése az Azure-bA.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1. lépés: A Site Recovery szolgáltatással az Azure előkészítése

Contoso cégnek szüksége van a több Azure-összetevőket a Site Recovery:

- Egy virtuális hálózatot, amelyben átadta a feladatait erőforrások találhatók (a Contoso fog használni az éles virtuális hálózat már telepített)
- Új Azure storage-fiókot a replikált adatok tárolásához. 
- Az Azure Recovery Services-tárolóba.

A VNet során már létrehozott contoso [Azure-infrastruktúra üzembe helyezési](contoso-migration-infrastructure.md), így csak létre kell hozniuk egy tárfiókot és tárolót.

1. Contoso (contosovmsacc20180528) Azure storage-fiókot az USA keleti RÉGIÓJA 2 régióban hoz létre.

    - A tárfióknak és a Recovery Services-tárolónak ugyanabban a régióban kell elhelyezkednie.
    - Egy általános célú fiók, és standard szintű storage, LRS-replikációval használja azokat.

    ![Site Recovery-tároló](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. A hálózati és tárolási fiók helyen, a Contoso hozzon létre egy tárolót (ContosoMigrationVault), és helyezze el a a **ContosoFailoverRG** erőforráscsoport az USA keleti RÉGIÓJA 2 elsődleges régióban.

    ![Recovery Services-tároló](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**További segítségre van szüksége?**

[Ismerje meg](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) beállítása az Azure Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2. lépés: A Site Recovery a helyszíni VMware előkészítése

Contoso előkészíti a helyszíni VMware-infrastruktúra a következő:

- Hozzon létre egy fiókot a vCenter-kiszolgáló vagy vSphere ESXi-gazdagép, virtuális gépek felderítésének automatizálásához.
- Hozzon létre egy fiókot, amely lehetővé teszi, hogy az automatikus telepíteni a mobilitási szolgáltatást a replikálni kívánt VMware virtuális gépeket.
- Készítse elő a helyszíni virtuális gépek esetében, úgy, hogy csatlakozhassanak az Azure virtuális gépeire az áttelepítés után létrehozott.


### <a name="prepare-an-account-for-automatic-discovery"></a>Fiók előkészítése automatikus felderítéshez

A Site Recoverynek hozzáféréssel kell rendelkeznie a VMware-kiszolgálókhoz az alábbiak érdekében:

- A virtuális gépek automatikus felderítése. Szükség van legalább egy csak olvasási jogokat biztosító fiókra.
- Replikáció, feladatátvétel és feladat-visszavétel vezénylése. Futtathatja a műveletek, például a létrehozása és eltávolítása a lemezeket, és ne tudják bekapcsolni a virtuális gépek fiók szükséges.

Contoso a következőképpen állítja be a fiók:

1. Contoso egy szerepkört a vCenter-szinten hoz létre.
2. Contoso majd hozzárendeli a szerepkört a szükséges engedélyekkel.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Fiók előkészítése a mobilitási szolgáltatás telepítéséhez

A mobilitási szolgáltatásnak telepítve kell lennie, amely a Contoso áttelepítése a Linux rendszerű virtuális gépeken:

- A Site Recovery összetevő egy automatikus ügyfélleküldéses telepítés hajthatja végre, ha engedélyezi a virtuális gépek replikációját.
- Automatikus ügyfélleküldéses telepítéshez meg kell készíteni egy fiókot, amely a Site Recovery a virtuális gépek eléréséhez fog használni.
- Fiókok részletek replikációs telepítéskor adjon meg vannak. 
- A fiók lehet tartományi vagy helyi fiók, az engedélyek virtuális gépekre kíván telepíteni.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Felkészülés az Azure virtuális gépekhez való kapcsolódásra a feladatátvételt követően

Az Azure-bA a feladatátvételt követően a Contoso biztosítani szeretné az Azure-ban a replikált virtuális gépek csatlakozni tudnak. Ehhez néhány dolgot kell tennie azokat van:

- Hozzáférhet az interneten keresztül, akkor engedélyezi az SSH a helyszíni linuxos virtuális gép áttelepítése előtt.  Ubuntu rendszerre készült ez hajtható végre a következő parancsot: **Sudo apt-get ssh telepítése -y**.
- Az áttelepítés (feladatátvétel) futtatása, miután azok ellenőrizze **rendszerindítási diagnosztika** , a virtuális gép képernyőképének megtekintéséhez.
- Ha ez sem működik, akkor ellenőrizze, hogy a virtuális gép fut, és tekintse át a [hibaelhárítási tippek](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**További segítségre van szüksége?**

- [Ismerje meg](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) létrehozása és hozzárendelése egy szerepkört a automatikus felderítése.
- [Ismerje meg](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) hozzon létre egy fiókot a mobilitási szolgáltatás leküldéses telepítéséhez.


## <a name="step-3-replicate-the-on-premises-vms"></a>3. lépés: A helyszíni virtuális gépek replikálása

A webkiszolgáló virtuális gép áttelepítése, lehetséges, az Azure-ba, mielőtt a Contoso állít be, és lehetővé teszi a replikációt.

### <a name="set-a-protection-goal"></a>Egy védelmi cél beállítása

1. A tárolóban, a tároló nevét (ContosoVMVault) alatt, állítsa be a replikációs cél (**bevezetés** > **Site Recovery** > **infrastruktúraelőkészítése**.
2. Akkor adja meg, hogy a gépek a helyszínen található, az, hogy azok VMware virtuális gépek és az Azure-bA replikálni kívánt.
    ![Replikációs cél](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Az üzembe helyezés megtervezésének megerősítése

A folytatáshoz, győződjön meg arról, hogy azok befejezte üzembe helyezés megtervezése, kiválasztásával **Igen, elvégeztem**. Contoso csak az ebben a forgatókönyvben egyetlen virtuális gép áttelepítése, és nem kell az üzembe helyezés megtervezése.

### <a name="set-up-the-source-environment"></a>A forráskörnyezet beállítása

A forráskörnyezet beállítása a Contoso cégnek szüksége van. Ehhez, egy OVF-sablon letöltése és használatával a Site Recovery konfigurációs kiszolgálónak a magas rendelkezésre állásúként telepíteni, a helyszíni VMware virtuális gép. Miután a konfigurációs kiszolgáló üzembe helyezéséig, azok regisztrálja a tárolóban.

A konfigurációs kiszolgálón néhány összetevő fut:

- A konfigurációs kiszolgáló összetevő koordinálja a helyszíni és Azure közötti kommunikációt, és felügyeli az adatreplikációt.
- A folyamatkiszolgáló replikációs átjáróként üzemel. Fogadja a replikációs adatokat, gyorsítótárazással, tömörítéssel és titkosítással optimalizálja őket, majd továbbítja az adatokat az Azure-tárolónak.
- A folyamatkiszolgáló ezenfelül telepíti a mobilitási szolgáltatást a replikálni kívánt virtuális gépekre, és elvégzi a helyszíni VMware virtuális gépek automatikus felderítését.

Contoso hajtsa végre ezeket a lépéseket a következő:

1. Az OVF-sablon letöltése **infrastruktúra előkészítése** > **forrás** > **konfigurációs kiszolgáló**.
    
    ![OVF letöltése](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Importálja a VMware virtuális gép létrehozása, és telepítse a virtuális Gépet.

    ![OVF-sablon](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. A virtuális gép első bekapcsolásakor, a bekapcsolásakor egy Windows Server 2016 telepítési folyamatot. Fogadja el a licencszerződést, és adjon meg egy rendszergazdai jelszót.
4. A telepítés befejezését követően, jelentkezzen be rendszergazdaként a virtuális Gépet. Első bejelentkezéskor az Azure Site Recovery Configuration Tool alapértelmezés szerint fut.
5. Az eszközben kell adnia egy nevet a konfigurációs kiszolgáló regisztrálása a tárolóban használni.
6. Az eszköz ellenőrzi, hogy a virtuális gép tud-e csatlakozni az Azure-hoz. A kapcsolat létrejötte után bejelentkeznek az Azure-előfizetést. Olyan hitelesítő adatokra van szükség, amelyekkel hozzá lehet férni a tárolóhoz, amelyben regisztrálni kívánja a konfigurációs kiszolgálót.

    ![Konfigurációs kiszolgáló regisztrálása](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. Az eszköz végrehajt néhány konfigurációs feladatot, majd újraindul.
8. Bejelentkeznek a gép újra, és a konfigurációs kiszolgáló felügyeleti varázslója automatikusan elindul.
9. A varázsló, válassza ki a hálózati Adaptert replikációs forgalom fogadására. Ez a beállítás a konfigurálás után nem módosítható.
10. Akkor válassza ki az előfizetést, erőforráscsoportot és a tároló, amelyben a konfigurációs kiszolgálót regisztrálja.

    ![Tároló](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. Majd töltse le és telepítse a MySQL-kiszolgáló és a VMWare powercli-t. 
12. Ellenőrzést hogy a vCenter-kiszolgáló vagy vSphere-gazdagép teljes Tartománynevét vagy IP-címét adja meg. Hagyja bejelölve az alapértelmezett portot, és adja meg a vCenter-kiszolgáló rövid nevét.
13. A fiók automatikus felderítéshez létrehozott és a hitelesítő adatok a mobilitási szolgáltatás automatikus telepítéséhez használandó kell adnia.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Az Azure Portalon, a regisztráció befejezését követően a Contoso ellenőrzi, hogy a konfigurációs kiszolgáló és a VMware-kiszolgáló szerepel a a **forrás** lap a tárolóban. 15 percig vagy tovább is tarthat a felderítés. 
15. A Site Recovery ezután csatlakozik a VMware-kiszolgálókhoz, és felderíti a virtuális gépeket.

### <a name="set-up-the-target"></a>A cél beállítása

Most már a Contoso cél replikációs beállításait konfigurálja.

1. A **infrastruktúra előkészítése** > **cél**, akkor válassza ki a célbeállítások.
2. A Site Recovery ellenőrzi, hogy nincs-e egy Azure storage-fiók és a célként megadott hálózat.

### <a name="create-a-replication-policy"></a>Replikációs házirend létrehozása

A forrás és cél beállítása után Contoso készen áll a replikációs szabályzat létrehozásához.

1. A **infrastruktúra előkészítése** > **replikációs beállítások** > **replikációs házirend** >  **létrehozás és Társítsa**, akkor hozzon létre egy házirendet **ContosoMigrationPolicy**.
2. Az alapértelmezett beállítások használata:
    - **Helyreállítási Időkorlát küszöbértéke**: alapértelmezett 60 perc. Ez az érték határozza meg, hogy milyen gyakran jönnek létre helyreállítási pontok. A rendszer riasztást ad, ha a folyamatos replikáció túllépi ezt a korlátot.
    - **Helyreállítási pont megőrzése**. Alapértelmezett 24 órányi. Ez az érték határozza meg, mennyi ideig őrzi az egyes helyreállítási pontok. A replikált virtuális gépek ezen az időtartamon belül bármikor helyreállíthatók.
    - **Alkalmazáskonzisztens pillanatkép gyakorisága**. Alapértelmezés szerint egy óra. Ez az érték, amellyel jönnek létre alkalmazáskonzisztens pillanatképek gyakorisága határozza meg.
 
        ![Replikációs házirend létrehozása](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. A szabályzat automatikusan társítva lesz a konfigurációs kiszolgálóval. 

    ![Replikációs házirend társítása](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**További segítségre van szüksége?**

- Tudjon meg egy teljes forgatókönyv az alábbi lépéseket a [vészhelyreállítás beállítása helyszíni VMware virtuális gépek](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Részletes útmutatás segítségével érhetők el [a forráskörnyezet beállítása](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [használt konfigurációs kiszolgáló telepítése](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), és [replikációs beállítások konfigurálása](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [További](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) a Linuxhoz készült Azure Vendég ügynök kapcsolatban.

**További segítségre van szüksége?**

- Tudjon meg egy teljes forgatókönyv az alábbi lépéseket a [vészhelyreállítás beállítása helyszíni VMware virtuális gépek](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Részletes útmutatás segítségével érhetők el [a forráskörnyezet beállítása](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [használt konfigurációs kiszolgáló telepítése](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), és [replikációs beállítások konfigurálása](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [További](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) a Linuxhoz készült Azure Vendég ügynök kapcsolatban.



### <a name="enable-replication-for-osticketweb"></a>OSTICKETWEB a replikáció engedélyezése

Most a Contoso is replikáljon az **OSTICKETWEB** virtuális Gépet.

1. A **alkalmazás replikálása** > **forrás** > **+ replikálás** , válassza ki az adatforrás-beállítások.
2. Akkor válassza ki, hogy szeretnének-e a virtuális gépek engedélyezéséhez válassza ki az adatforrás beállításai, többek között a vCenter-kiszolgáló és a konfigurációs kiszolgálón.

    ![A replikáció engedélyezése](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Akkor adja meg a cél beállításai, többek között az erőforráscsoportot és a virtuális hálózatot, amelyben az Azure virtuális Gépen található feladatátvétel után és a tárfiókot, amely a replikált adatokat tárolni szeretné.

     ![A replikáció engedélyezése](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Contoso kiválasztja **OSTICKETWEB** replikálásra. 

    - Ezen a ponton Contoso választja ki csak **OSTICKETWEB** mert virtuális hálózatot és alhálózatot kell kijelölni, és a virtuális gépek nem ugyanazon az alhálózaton.
    - A Site Recovery automatikusan telepíti a mobilitási szolgáltatást, amikor a replikáció engedélyezve van a virtuális gép számára.

    ![A replikáció engedélyezése](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. A virtuális gép tulajdonságait, a Contoso választja ki a fiókot, amellyel a folyamatkiszolgáló automatikusan telepíti a mobilitási szolgáltatást a gépen.

     ![Mobilitási szolgáltatás](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. a **replikációs beállítások** > **replikációs beállítások konfigurálása**, akkor ellenőrizze, hogy a megfelelő replikációs szabályzat-e a alkalmazott, és válassza **Replikációengedélyezése**.
6.  A replikáció állapotát nyomon követik **feladatok**. A **Védelem véglegesítése** feladat befejeződését követően a gép készen áll a feladatátvételre.



### <a name="enable-replication-for-osticketmysql"></a>OSTICKETMYSQL a replikáció engedélyezése

Most a Contoso is replikáljon **OSTICKETMYSQL**.

1. A **alkalmazás replikálása** > **forrás** > **+ replikálás** , válassza ki a forrás- és beállításokat.
2. Contoso kiválasztja **OSTICKETMYSQL** replikációhoz, és kiválasztja a fiókot szeretné használni a mobilitási szolgáltatás telepítése.

    ![A replikáció engedélyezése](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Contoso OSTICKETWEB használták, és lehetővé teszi, hogy a replikációs replikációs szabályzat vonatkozik.  

**További segítségre van szüksége?**

Tudjon meg egy teljes forgatókönyv az alábbi lépéseket a [engedélyezze a replikációt](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>4. lépés: A virtuális gépek áttelepítése 

Contoso futtatása egy gyors feladatátvételi teszt, majd utána áttelepíteni az a virtuális gépek.

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

Feladatátvételi teszt futtatása gondoskodik arról, hogy minden a várt módon működik az áttelepítés előtt. 

1. Contoso fut egy feladatátvételi tesztet a legújabb elérhető pontra idő (**legutóbb feldolgozott**).
2. Kiválasztják **gép leállítása a feladatátvétel megkezdése előtt**, hogy a Site Recovery megkísérli a forrásoldali virtuális gép leállítása a feladatátvétel indítása előtt. A feladatátvételi akkor is folytatódik, ha a leállítás meghiúsul. 
3. Teszt feladatátvétel futtatása: 
    - Ellenőrizze, hogy az áttelepítéshez szükséges feltételek vannak érvényben lefuttatja az előfeltételek ellenőrzését.
    - A feladatátvétel feldolgozza az adatokat, hogy az Azure-beli virtuális gép létrehozható legyen. Ha a legutóbbi helyreállítási pontot választja, a helyreállítási pont létrehozása az adatokból.
    - A rendszer létrehoz egy Azure-beli virtuális gépet az előző lépésben feldolgozott adatok használatával.
3. A feladatátvétel befejezését követően a replika Azure virtuális gép megjelenik az Azure Portalon. Ellenőrizze, hogy a virtuális gép a megfelelő méret, amely a megfelelő hálózathoz csatlakoztatva van és fut-e. 
4. Miután ellenőrizte, azok a feladatátvételt, karbantartása és jegyezheti fel és mentheti kapcsolatos megfigyelések.

### <a name="create-and-customize-a-recovery-plan"></a>Hozzon létre, és a helyreállítási terv testreszabása

 Miután ellenőrizte, hogy a feladatátvételi teszt a várt módon dolgozni, a Contoso áttelepítés helyreállítási terv létrehozásához. 

- A helyreállítási terv sorrendben adja meg a feladatátvételt akkor következik be, hogyan Azure virtuális gépeken futó állapotba kerül az Azure-ban.
- Szeretnék áttelepíteni egy kétszintű alkalmazás, mivel azok szabhatja testre a helyreállítási tervbe, hogy az előtér (WEBVM) előtt elindul a virtuális gép (SQLVM) adatok.


1. A **helyreállítási tervek (Site Recovery)** > **+ a helyreállítási terv**, hozzon létre egy csomagot, és a hozzá tud adni a virtuális gépeket.

    ![Helyreállítási terv](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Miután létrehozta a tervet, akkor válassza ki a testreszabáshoz (**helyreállítási tervek** > **OsTicketMigrationPlan** > **Testreszabás**.
3.  Azok eltávolítása **OSTICKETWEB** a **csoport 1: Start**.  Ez biztosítja, hogy az első indítási műveletet érinti **OSTICKETMYSQL** csak.

    ![Helyreállítási csoport](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  A **+ csoport** > **adja hozzá a védett elemek**, adnak hozzá **OSTICKETWEB** való **csoport 2: Start**.  Contoso cégnek szüksége van, ezek két különféle csoportokba.

    ![Helyreállítási csoport](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>A virtuális gépek áttelepítése


Contoso készen áll a helyreállítási tervbe, hogy a virtuális gépek áttelepítése a feladatátvételt.

1. Akkor válassza ki a csomagot > **feladatátvételi**.
2.  Átadja a feladatokat a legutóbbi helyreállítási pontot, és adja meg, hogy a Site Recovery a helyszíni virtuális gép leállítása a feladatátvétel indítása előtt próbálja válassza ki. A feladatátvételi folyamatot követve a **feladatok** lapot.

    ![Feladatátvétel](./media/contoso-migration-rehost-vm/failover1.png)

3. A feladatátvétel során a vCenter-kiszolgáló problémák állítsa le a két virtuális gép az ESXi-gazdagépen futó parancsokat.

    ![Feladatátvétel](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. A feladatátvétel után a Contoso ellenőrizze, hogy az Azure virtuális gép megjelenik-e a várt módon az Azure Portalon.

    ![Feladatátvétel](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Miután ellenőrizte a virtuális gép az Azure-ban, ezek az áttelepítéshez minden virtuális géphez az áttelepítési folyamat befejezéséhez. Ez a virtuális gép replikálását, és leállítja a virtuális gép Site Recovery-számlázását.

    ![Feladatátvétel](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>A virtuális gép kapcsolódni az adatbázishoz

Az áttelepítési folyamat utolsó lépéseként, Contoso úgy, hogy a futó alkalmazás-adatbázis mutasson az alkalmazás a kapcsolati karakterlánc frissítése az **OSTICKETMYSQL** virtuális Gépet. 

1. Egy SSH-kapcsolatot vállalnak a **OSTICKETWEB** virtuális Gépet a Putty használatával vagy egy másik SSH-ügyfelet. A virtuális gép nem nyilvános, így a privát IP-cím használatával csatlakoznak.

    ![Csatlakozás adatbázishoz](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![Csatlakozás adatbázishoz](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Győződjön meg arról, hogy van szükségük a **OSTICKETWEB** virtuális gépek kommunikálhatnak a **OSTICKETMYSQL** virtuális Gépet. A konfiguráció jelenleg szoftveresen kötött IP-címmel a helyszíni 172.16.0.43.

    **A frissítés előtt**
    
    ![IP Update](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **A frissítés után**
    
    ![IP Update](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. A szolgáltatás az újraindítás **systemctl indítsa újra az apache2**.

    ![Újraindítás](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Végül frissítse a DNS-rekordok **OSTICKETWEB** és **OSTICKETMYSQL**, a Contoso tartományvezérlők egyik.

    ![DNS frissítése](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![DNS frissítése](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**További segítségre van szüksége?**

- [Ismerje meg](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) feladatátvételi teszt futtatása. 
- [Ismerje meg,](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) a helyreállítási terv létrehozása.
- [Ismerje meg](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) – Azure feladatátvétele.

## <a name="clean-up-after-migration"></a>Áttelepítés utáni karbantartás

Az áttelepítés befejeződött a osTicket alkalmazás szinten futnak az Azure virtuális gépekhez.

Most Contoso szüksége van bizonyos karbantartása:  

- A helyszíni virtuális gépek azok eltávolítása a vCenter-készlet.
- Távolítsa el a helyszíni virtuális gépek helyi biztonsági mentési feladatok.
- Belső dokumentációját az új hely megjelenítéséhez frissítse, és az IP-címeket OSTICKETWEB és OSTICKETMYSQL.
- Tekintse át az olyan erőforrások, a virtuális gépek kommunikáljanak, és minden olyan vonatkozó beállítások vagy dokumentáció, hogy az új konfiguráció frissítése.
- Contoso és függőségi leképezés az Azure Migrate szolgáltatás segítségével felmérheti a migrálásra a virtuális gépek. Akkor érdemes eltávolítani, a Microsoft Monitoring Agent, valamint a függőségi ügynököt, azok telepítve erre a célra, a virtuális gépről.

## <a name="review-the-deployment"></a>Tekintse át a központi telepítés

Az alkalmazás most már fut, a Contoso cégnek szüksége van, teljes mértékben üzembe helyezése, és tegye biztonságossá a új infrastruktúrára.

### <a name="security"></a>Biztonság

A Contoso biztonsági csapat a OSTICKETWEB és OSTICKETMYSQLVMs bármilyen biztonsági problémák meghatározása érdekében tekintse át.

- Azok a hálózati biztonsági csoportok (NSG-k) tekintse át a virtuális gépek elérésének szabályozására. Az NSG-k segítségével győződjön meg arról, hogy csak az alkalmazás engedélyezett forgalom adhat át.
- Fontolgatják azt is, a Virtuálisgép-lemezek lemeztitkosítás és az Azure KeyVault használata az adatok védelme.

[További információ](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) virtuális gépek biztonsági eljárásairól.

### <a name="backups"></a>Biztonsági másolatok

Contoso a virtuális gépek az Azure Backup szolgáltatás használatával biztonsági mentést az adatokat. [További információk](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licencelési és a költségek optimalizálása

- Után üzembe helyezni erőforrásokat, a Contoso Azure címkéket rendel, során meghatározott a [Azure-infrastruktúra üzembe helyezési](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso licencelési problémamentesen működjön a Ubuntu Server rendelkezik.
- Contoso lehetővé teszi a Cloudyn, a Microsoft egy leányvállalata által licencelt Azure Cost Managementbe. Egy többfelhős költségkezelő felügyeleti megoldás, amely segítséget nyújt az Azure és egyéb felhőerőforrások kezelése, és a.  [További](https://docs.microsoft.com/azure/cost-management/overview) kapcsolatos Azure Cost Managementbe. 


## <a name="next-steps"></a>További lépések

Ebben a cikkben megmutattuk, hogyan Contoso át a helyszíni szolgáltatás desk alkalmazás rétegzett két Linux rendszerű virtuális gépeken az Azure IaaS virtuális gépeire, Azure Site Recovery használatával.

A sorozat, a következő cikkben bemutatjuk, hogyan Contoso áttelepítése Azure-bA szolgáltatás desk ugyanazt az alkalmazást. Ennek során Contoso használ a Site Recovery az alkalmazás az előtérbeli virtuális gép áttelepítése, valamint a biztonsági mentéssel alkalmazás-adatbázis migrálása és visszaállítása az Azure Database for MySQL, a MySQL workbench eszközzel. [Első lépések](contoso-migration-rehost-linux-vm-mysql.md).
