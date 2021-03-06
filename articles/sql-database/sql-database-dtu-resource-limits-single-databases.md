---
title: Az Azure SQL Database DTU-alapú erőforrás korlátozza az önálló adatbázisok |} A Microsoft Docs
description: Ezen a lapon azt ismerteti, hogy néhány gyakori DTU-alapú erőforráskorlátok az Azure SQL Database önálló adatbázisok számára.
services: sql-database
author: sachinpMSFT
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: effb09cfc68961065ad0b4e4be52255bcd1fe4e0
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414167"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>A DTU-alapú vásárlási modell segítségével az önálló adatbázisok számára erőforráskorlátok 

Ez a cikk a részletes erőforráskorlátok biztosít az Azure SQL Database rugalmas készletei a DTU-alapú vásárlási modell használatával.

DTU-alapú vásárlási modell erőforráskorlátok a rugalmas készletek, lásd: [DTU-alapú erőforráskorlátok – rugalmas készletek](sql-database-vcore-resource-limits-elastic-pools.md). Virtuálismag-alapú erőforráskorlátok, lásd: [Virtuálismag-alapú erőforráskorlátok – önálló adatbázisok](sql-database-vcore-resource-limits-single-databases.md) és [Virtuálismag-alapú erőforráskorlátok – rugalmas készletek](sql-database-vcore-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> Bizonyos körülmények között szükség lehet az adatbázis nem használt terület felszabadítását zsugorítani. További információkért lásd: [kezelése az Azure SQL Database területe](sql-database-file-space-management.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Önálló adatbázis: tárterületet és teljesítményszintek

Az önálló adatbázisok számára az alábbi táblázatok bemutatják az egyes szolgáltatási csomagot és teljesítményszintet szinten egy önálló adatbázis rendelkezésre álló erőforrások. A szolgáltatási rétegben, a teljesítményszintet és a egy önálló adatbázis a tárolókapacitás is megadhatja a [az Azure portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), a [Azure CLI-vel](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases), vagy a [REST API-val](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="basic-service-tier"></a>Alapszintű szolgáltatásszint
| **Teljesítményszint** | **Basic** |
| :--- | --: |
| DTU-k maximális száma | 5 |
| Belefoglalt tárterület (GB) | 2 |
| Maximális tárolási lehetőségeket (GB) | 2 |
| Memóriában tárolt OLTP-k maximális tárterülete (GB) |– |
| Egyidejű feldolgozók (kérelmek) maximális | 30 |
| Egyidejű munkamenetek maximális száma | 300 |
|||

### <a name="standard-service-tier"></a>Standard szolgáltatásszint
| **Teljesítményszint** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| DTU-k maximális száma | 10 | 20 | 50 | 100 |
| Belefoglalt tárterület (GB) | 250 | 250 | 250 | 250 |
| Maximális tárolási lehetőségeket (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Memóriában tárolt OLTP-k maximális tárterülete (GB) | – | N/A | N/A | – |
| Egyidejű feldolgozók (kérelmek) maximális| 60 | 90 | 120 | 200 |
| Egyidejű munkamenetek maximális száma |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standard szintű szolgáltatáscsomagban (Folytatás)
| **Teljesítményszint** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| DTU-k maximális száma | 200 | 400 | 800 | 1600 | 3000 |
| Belefoglalt tárterület (GB) | 250 | 250 | 250 | 250 | 250 |
| Maximális tárolási lehetőségeket (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Memóriában tárolt OLTP-k maximális tárterülete (GB) | – | N/A | N/A | N/A |– |
| Egyidejű feldolgozók (kérelmek) maximális| 400 | 800 | 1600 | 3200 |6000 |
| Egyidejű munkamenetek maximális száma |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Prémium szolgáltatásszint 
| **Teljesítményszint** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| DTU-k maximális száma | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Belefoglalt tárterület (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| Maximális tárolási lehetőségeket (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Memóriában tárolt OLTP-k maximális tárterülete (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Egyidejű feldolgozók (kérelmek) maximális| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Egyidejű munkamenetek maximális száma | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||


> [!IMPORTANT]
> A következő kivételével minden régióban több mint 1 TB prémium szintű storage jelenleg érhető el: USA nyugati középső Régiója, kelet-Kína, közép-USDoDCentral, Németország, USDoDEast, USA-beli államigazgatás – délnyugati, Északkelet-Németország, USGovIowa, Kínát. Más régiókban a Prémium szinthez tartozó tárterület maximuma 1 TB. Lásd: [P11–P15 – Aktuális korlátozások](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="single-database-change-storage-size"></a>Önálló adatbázis: tároló méretének módosítása

- Önálló adatbázis dtu-k díjszabása tartalmaz egy bizonyos mennyiségű tárolási további költségek nélkül. A csomagban foglalt adatmennyiségen felüli extra tárterület legfeljebb 250 GB-os fel 1 TB-os egységekben, majd, 256 GB 1 TB-os léptékben maximális méretkorlátot díjfizetés mellett bővítheti. Belefoglalt tárterület összegek és a maximális méret korlátok [egyetlen adatbázishoz: tárterületet és teljesítményszintek](#single-database-storage-sizes-and-performance-levels).
- Extra tárterülettel önálló adatbázis kiépítése a maximális méret használatával növelje a [az Azure portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), a [Azure Parancssori felület](/cli/azure/sql/db#az_sql_db_update), vagy a [REST API-val](/rest/api/sql/databases/update).
- Az extra tárterülettel önálló adatbázis szolgáltatás díja az extra tárterület egységára a szolgáltatási rétegben megszorozza extra tárterület keletkezett. Az extra tárterület ára a részletekért lásd: [SQL Database – díjszabás](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Önálló adatbázist: módosítási dtu-k

Miután kiválasztotta a a szolgáltatási rétegben, a teljesítményszint és a tárterület-mennyiség, méretezhetők egy önálló adatbázis felfelé vagy lefelé dinamikusan használatával a tényleges tapasztalatok alapján a [az Azure portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), a [az Azure CLI](/cli/azure/sql/db#az_sql_db_update), vagy a [REST API-val](/rest/api/sql/databases/update). 

A következő videó bemutatja a dinamikus módosítása növelheti a rendelkezésre álló dtu-k egy önálló adatbázis teljesítményszintjét.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Az adatbázis szolgáltatásszintjének és/vagy teljesítményszintjének megváltoztatásakor egy replika jön létre az eredeti adatbázisról az új teljesítményszinten, majd a rendszer átáll a replikához való csatlakozásra. A folyamat során nem történik adatvesztés, de a replikára való átálláskor egy pillanatra az adatbázis felé irányuló kapcsolatok le lesznek tiltva, így lehet, hogy néhány folyamatban lévő tranzakció vissza lesz állítva. Mennyi ideig a kapcsoló-több mint az érték, de kevesebb mint 30 másodperc 99 %-ában. Ha nagy számú tranzakció van folyamatban a kapcsolatok pillanatában le vannak tiltva, akkor a hosszát a kapcsoló-több mint hosszabb lehet. 

A teljes felskálázási folyamat időtartama az adatbázis a módosítás előtti és utáni méretétől és szolgáltatásszintjétől függ. Ha például egy 250 GB-os adatbázis, amely megváltoztatja a, a vagy a Standard szolgáltatásszinten belül hat órán belüli be kell fejeződnie. A vertikális felskálázás három órán belül, amely módosul a prémium szintű szolgáltatási szinten belüli teljesítményszintet azonos méretű adatbázis esetén be kell fejeződnie.

> [!TIP]
> A lévő üzenetekre vonatkozó műveletek figyeléséről lásd:: [SQL REST API használatával műveleteinek kezelésére](/rest/api/sql/Operations/List), [kezelése CLI használatával az operations](/cli/azure/sql/db/op), [T-SQL használatával végzett műveletek monitorozására](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) , és ez a két PowerShell-parancsok: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) és [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Ha egy magasabb szolgáltatási vagy teljesítményszintjének szintre frissít, az adatbázis maximális méretét nem növeli, ha Ön kifejezetten megad egy nagyobb méretű (maxsize).
* Alacsonyabb szolgáltatásszintre váltásához egy adatbázist, a használt adatbázis-terület kisebb, mint a megengedett legnagyobb méretet a célként megadott szolgáltatás szolgáltatásszintet és teljesítményszintet szint kell lennie. 
* Ha alacsonyabb szolgáltatásszintre vált **prémium** , a **Standard** szinten az egy extra tárterület költségeit érvényes, ha mind az adatbázis maximális mérete (1) a cél teljesítményszintjét használata támogatott, és (2) a maximális mérete meghaladja a Belefoglalt tárterület mérete cél teljesítményszintjét. Például 500 GB maximális mérettel P1 adatbázis van downsized S3 szintű adatbázist, ha ezután egy extra tárterület költség érvényes óta S3 támogatja az 500 GB-os maximális méretét, és a belefoglalt tárterület tárterülete 250 GB csak. Tehát az extra tárterület keletkezett az 500 GB-250 GB = 250 GB. Extra tárterület díjszabással kapcsolatos információk: [SQL Database – díjszabás](https://azure.microsoft.com/pricing/details/sql-database/). Ha a tényleges felhasznált lemezterület mérete kisebb, mint a csomagban foglalt tárhely, majd ezzel többletköltség elkerülhető az adatbázis maximális méretének csökkentésével a csomagban foglalt adatmennyiségen. 
* Ha az adatbázis frissítése [georeplikációs](sql-database-geo-replication-portal.md) engedélyezve van, frissítse a másodlagos adatbázisok teljesítményszintjét a kívánt szintre az elsődleges adatbázis (általános útmutatást a legjobb teljesítmény érdekében) a frissítés előtt. Frissíti egy másik, a másodlagos adatbázis frissítése először szükség.
* Ha egy adatbázis alacsonyabb szolgáltatásszintre [georeplikációs](sql-database-geo-replication-portal.md) az elsődleges adatbázisok teljesítményszintjét a kívánt szintre engedélyezve van, alacsonyabbra előtt alacsonyabb verziójúra változtatása a másodlagos adatbázis (általános útmutatást a legjobb teljesítmény érdekében). Ha alacsonyabb szolgáltatásszintre egy másik kiadásra, az elsődleges adatbázis alacsonyabb szolgáltatásszintre először megadása kötelező.
* A visszaállítási szolgáltatásajánlatok eltérőek a különböző szolgáltatásszintek esetében. Ha alacsonyabb szolgáltatásszintre a **alapszintű** szint, van egy alacsonyabb biztonsági másolat megőrzési idejének – lásd: [Azure SQL Database biztonsági másolatainak](sql-database-automated-backups.md).
* Az adatbázis új tulajdonságai csak akkor lesznek alkalmazva, ha a módosítások befejeződtek.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Önálló adatbázis: P11 és p15-ös, amikor a maximális méret 1 TB-nál nagyobb korlátozásai

A P11 és P15-adatbázis a következő régiókban támogatott 1 TB-nál nagyobb a maximális méretet: Kelet-Ausztrália, Délkelet-Ausztrália, Dél-Brazília, közép-Kanada, kelet-Kanada, USA középső RÉGIÓJA, közép-Franciaország, közép-Németország, kelet-japán, Nyugat-japán, Korea középső régiója USA északi középső RÉGIÓJA, Észak-Európa, USA déli középső RÉGIÓJA, Délkelet-Ázsia, Egyesült Királyság déli régiója, Egyesült Királyság nyugati régiója, USA East2, USA nyugati RÉGIÓJA, USA-beli államigazgatás – Virginia és Nyugat-Európa. Nagyobb, mint 1 TB-os maximális mérettel P11 és P15 adatbázisokat az alábbi szempontok és korlátozások vonatkoznak:

- Ha 1 TB-nál nagyobb a maximális méretet (4 TB-os vagy 4096 GB-os érték használata) adatbázis létrehozásakor, a create parancs egy hibaüzenettel meghiúsul, ha az adatbázis egy nem támogatott régióban van kiépítve.
- A már létező P11 és P15 adatbázisok a támogatott régiók egyikében található, a maximális tárhelyet 1 TB, 256 GB-os léptékben növelhető akár 4 TB-ig. Ha egy nagyobb méretű támogatott-e az Ön régiójában megtekintéséhez használja a [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) működik, vagy vizsgálja meg az adatbázis méretét az Azure Portalon. Frissíti egy meglévő P11 és p15-ös adatbázis csak hajtható végre egy kiszolgálószintű fő bejelentkező vagy a dbmanager adatbázis-szerepkör tagjai által. 
- Ha egy támogatott régióban egy frissítési művelet a konfigurációs azonnal frissül. Az adatbázis online állapotban marad, a frissítési folyamat során. Azonban nem használhatja a tárhelyet 1 TB tárterület teljes mennyisége mindaddig, amíg az aktuális adatbázisfájlok frissítettek az új maximális mérete. Hogy mennyi idő szükséges a frissítés alatt áll az adatbázis méretétől függ. 
- Ha a létrehozása vagy frissítése egy P11 és p15-ös adatbázis 1 TB-os és a 4 TB-os maximális méret 256 GB-os lépésekben csak választhat. A P11 és P15 létrehozásakor az alapértelmezett tárolási beállítás 1 TB-os előre kiválasztott. Adatbázisok esetén a támogatott régiók egyikében található a storage maximális, akár 4 TB-ig maximum egy új vagy meglévő önálló adatbázis lehet növelni. Az összes többi régió esetében a maximális mérete 1 TB-nál nagyobb nem növelhető. Az ár nem változik, ha 4 TB-os belefoglalt tárterület választja.
- Ha egy adatbázis maximális mérete értéke nagyobb, mint 1 TB-ot, majd azt nem lehet módosítani 1 TB-os akkor is, ha a használt tényleges tárterület 1 TB-os alatt. Ebből kifolyólag nem válthat a P11-es vagy P15 nagyobb, mint 1 TB-os P11-es vagy 1 TB-os p15-ös vagy kisebb teljesítményt tier, P1 – P6 például az 1 TB-os maximális mérettel). Ez a korlátozás is vonatkozik a visszaállítás és másolási forgatókönyvek esetén az időpontban, beleértve a geo-visszaállítás, hosszú-távú – backup-megőrzés és -adatbázis másolatát. Ha egy adatbázis nagyobb, mint 1 TB-os maximális mérettel van konfigurálva, az adatbázis minden visszaállítási művelet nagyobb, mint 1 TB-os maximális mérettel be a P11 és p15-ös kell futnia.
- Aktív georeplikáció forgatókönyvek esetén:
   - Georeplikáció kapcsolat beállítása: Ha az elsődleges adatbázis P11 és p15-ös, az secondary(ies) is kell lennie a P11 és P15; alacsonyabb teljesítményszintek másodlagos példány hozható létre, a rendszer elutasítja, mivel azok nem képes a több mint 1 TB.
   - Frissítése az elsődleges adatbázis georeplikációs kapcsolatban: az elsődleges adatbázison több mint 1 TB-os maximális méretének módosítása elindítja ezt a változtatást a másodlagos adatbázison. Mindkét frissítése sikeres a módosítás érvénybe léptetéséhez az elsődleges kell lennie. A több mint 1 TB-os lehetőség régió korlátozások érvényesek. Ha a másodlagos régióban, amely nem támogatja a több mint 1 TB-ot, az elsődleges nem frissül.
- A P11 és P15 adatbázisok esetén több mint 1 TB betöltése az Import/Export szolgáltatás használata nem támogatott. Az SqlPackage.exe használata [importálása](sql-database-import.md) és [exportálása](sql-database-export.md) adatokat.

## <a name="next-steps"></a>További lépések

- Lásd: [SQL Database: gyakori kérdések](sql-database-faq.md) kapcsolatos gyakori kérdésekre adott válaszokat.
- Azure – általános korlátozások kapcsolatos információkért lásd: [Azure-előfizetés és a szolgáltatások korlátozásai, kvótái és megkötései](../azure-subscription-service-limits.md).
- További információ a dtu-król és edtu-k: [dtu-król és edtu-k](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- A tempdb méretbeli korlátokat kapcsolatos információkért lásd: https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
