---
title: Az Azure SQL Database szolgáltatás – a virtuális mag |} A Microsoft Docs
description: A Virtuálismag-alapú vásárlási modell lehetővé teszi, hogy egymástól függetlenül méretezheti a számítási és tárolási erőforrások, a helyszíni teljesítmény és optimalizálás ár.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 68343f3fcdd2275012207d7ac5a5f3bcdc71d1b8
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414374"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Virtuális mag szolgáltatásszint kiválasztása, számítási, memória, tárolási és i/o-erőforrások

Szolgáltatásszintek teljesítményszintjei, a magas rendelkezésre állás kialakítása, a hibák elszigetelését, a tárolási típust kínál számos és i/o-tartomány különbözteti meg. Az ügyfél külön-külön kell konfigurálnia a szükséges tárolási és a megőrzési időszak a biztonsági mentésekhez. A Virtuálismag-modell, az önálló adatbázisok és rugalmas készletek jogosultak az fel arra, hogy a 30 %-os megtakarítást a [SQL Serverhez készült Azure Hybrid Use Benefit](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Az alábbi táblázat segít a két szintek közötti különbségeket:

||**Általános célú**|**Üzletileg kritikus**|
|---|---|---|
|A következőkre alkalmas|A legtöbb üzleti számítási feladathoz. Ajánlatok költségvetés-orientált elosztott és skálázható számítási és tárolási lehetőségek.|Magas I/O-igényű üzleti alkalmazások. Több elkülönített replika használatával ez biztosítja a legmagasabb hibatűrést.|
|Compute|1 és 80 virtuális mag, Gen4 és Gen5 |1 és 80 virtuális mag, Gen4 és Gen5|
|Memory (Memória)|Gen4: 7 GB / mag<br>Gen5: 5.5-ös GB / mag | Gen4: 7 GB / mag<br>Gen5: 5.5-ös GB / mag |
|Storage|Prémium szintű távtároló, 5 GB – 4 TB-ig|Helyi SSD-tárolóval, 5 GB – 4 TB-ig|
|IO-átviteli sebesség (becsült)|A 7000-es maximális IOPS / virtuális mag 500 IOPS|5000 IOPS magonként 200000 maximális iops|
|Rendelkezésre állás|1 replika, nincs olvasási szintű|3 replika, 1 [olvasási szintű](sql-database-read-scale-out.md), redundáns magas rendelkezésre ÁLLÁSÚ. zóna|
|Biztonsági másolatok|RA-GRS, 7 – 35 nap (alapértelmezés szerint 7 nap)|RA-GRS, 7 – 35 nap (alapértelmezés szerint 7 nap)|
|A memóriában|–|Támogatott|
|||

> [!IMPORTANT]
> Ha a számítási kapacitás kevesebb mint 1 virtuális magra van szüksége, használja a DTU-alapú vásárlási modell.

Lásd: [SQL Database: gyakori kérdések](sql-database-faq.md) kapcsolatos gyakori kérdésekre adott válaszokat. 

## <a name="storage-considerations"></a>A tárterülettel kapcsolatos szempontok

A következőket ajánljuk figyelmébe:
- A lefoglalt tárolót adatfájlok (MDF) és a naplófájlok fájlok (LDF) használja.
- Mindegyik teljesítményszint támogatja az adatbázis maximális méretét, egy alapértelmezett 32 GB maximális mérettel.
- Amikor konfigurálja a szükséges adatbázis mérete (MDF mérete), 30 %-a további tárhely automatikusan hozzáadott LDF támogatása
- Kiválaszthatja, hogy bármilyen adatbázis mérete 10 GB-os és a támogatott maximális érték között
 - Standard szintű tárolóra vonatkozó növelése, vagy 10 GB-os lépésekben méretének csökkentése
 - A Premium storage növelheti, vagy 250 GB-os lépésekben méretének csökkentése
- Az általános célú szolgáltatásszinten lévő `tempdb` használ egy csatlakoztatott SSD és a tárolási költségek a virtuális mag díja tartalmazza.
- Az üzletileg kritikus szolgáltatási rétegben található `tempdb` megosztások a csatlakoztatott SSD az MDF és az LDF-fájlok és a TempDB adatbázisban a tárolási költségek a virtuális mag díja tartalmazza.

> [!IMPORTANT]
> A teljes tárterület MDF és az LDF-lefoglalt díjkötelesek.

A jelenlegi teljes mérete MDF és az LDF-monitorozásához használja [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Az egyes MDF és az LDF-fájlok aktuális mérete monitorozásához használja [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> Bizonyos körülmények között szükség lehet az adatbázis nem használt terület felszabadítását zsugorítani. További információkért lásd: [kezelése az Azure SQL Database területe](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Biztonsági mentés és tárolás

Az adatbázis biztonsági másolatait lefoglalta a pont támogatja az SQL Database idő visszaállításához (PITR) és a hosszú távú adatmegőrzést (LTR) képességeit. Ez a tároló lefoglalt külön-külön az egyes adatbázisok, és két külön adatbázis-díj terheli. 

- **PITR**: önálló adatbázis biztonsági másolatok másolódnak RA-GRS tároló a program automatikusan. A tároló mérete dinamikusan növeli, amikor az új biztonsági mentéseket hoz létre.  A tárolót a heti teljes biztonsági mentésekhez, a napi differenciális biztonsági mentésekhez, illetve a tranzakciónaplók 5 percenként másolt biztonsági másolataihoz használja a rendszer. A tárhelyhasználat attól függ, hogy az adatbázis és a megőrzési időszak a változási gyakoriság. Beállíthatja, hogy minden egyes adatbázis 7 és 35 nap közötti, egy különálló megőrzési ideje. Egy minimális tárolókapacitás egyenlő 1 x-re az adatok méretének külön díjfizetés nélkül van megadva. A legtöbb adatbázisok esetében ez a mennyiség ez elegendő 7 nap a biztonsági mentések tárolásához.
- **Az LTR**: SQL-adatbázis a teljes biztonsági mentések hosszú távú megőrzésének konfigurálása akár 10 évig lehetőséget kínál. Az LTR-szabályzat engedélyezve van, ha a következő biztonsági másolatai RA-GRS tároló automatikusan, de szabályozhatja, hogy milyen gyakran a biztonsági másolatok másolódnak. A különböző megfelelőségi követelménynek megfelel, heti, havi és/vagy éves biztonsági mentések különböző megőrzési időtartamú választhat. Ez a konfiguráció meghatározzuk, mennyi tárhelyre lesz az LTR biztonsági mentéseket. Használhatja az LTR-díjkalkulátor LTR tárolási költségek becsléséhez. További információkért lásd: [Hosszú távú megőrzés](sql-database-long-term-retention.md).

## <a name="azure-hybrid-use-benefit"></a>Azure Hybrid Use Benefit

A Virtuálismag-alapú vásárlási modell, az exchange is kedvezményes díjszabást kínál az SQL Database-adatbázishoz a meglévő licenceit a [SQL Serverhez készült Azure Hybrid Use Benefit](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Az Azure-értékelemek lehetővé teszi, hogy a helyszíni SQL Server-licenceivel akár 30 %-os mentése az Azure SQL Database használatával a helyszíni SQL Server-licenceit frissítési garanciával rendelkező.

![díjszabás](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-of-single-databases-with-geo-replication-links"></a>Georeplikációs hivatkozásokat az önálló adatbázisok migrálása

DTU-alapú modell Virtuálismag-alapú modellre való áttérés hasonlít a frissítést, vagy alacsonyabb verziójúra változtatása a Standard és prémium szintű adatbázisok közötti georeplikációs kapcsolatot. A georeplikáció, de a felhasználó megszakítást okozó meg kell vizsgálni az alkalmazás-előkészítés szabályok nem igényel. Verzióra, először frissítse a másodlagos adatbázist, és az utána frissítse az elsődleges. Ha alacsonyabb szolgáltatásszintre, fordított sorrendben: kell az elsődleges adatbázis alacsonyabbra először, és ezután alacsonyabbra a másodlagos. 

Két, rugalmas készletek közötti georeplikációs használatakor javasoljuk, hogy jelöljön ki egy készletet, az elsődleges és a többi – másodlagos. Rugalmas készletek áttelepítése ebben az esetben kövesse az azonos útmutatókat.  Azonban nem, technikailag lehetséges, hogy a rugalmas készlet tartalmazza az elsődleges és másodlagos adatbázisok. Ebben az esetben áttelepítés megfelelően kell kezelni az "elsődleges" a magasabb kihasználtság a készlet, és kövesse az alkalmazás-előkészítés szabályok ennek megfelelően.  

A következő táblázat tartalmazza az adott áttelepítési forgatókönyvre vonatkozó útmutatást: 

|Aktuális szolgáltatási rétegben|Cél szolgáltatásszint|Áttelepítés típusa|Felhasználói műveletek|
|---|---|---|---|
|Standard|Általános célú|Oldalirányú|Áttelepítése bármilyen sorrendben is, de kell, hogy a megfelelő virtuális mag méretezési *|
|Prémium|Üzletileg kritikus|Oldalirányú|Áttelepítése bármilyen sorrendben is, de kell, hogy a megfelelő virtuális mag méretezési *|
|Standard|Üzletileg kritikus|Frissítés|Át kell telepítenie a másodlagos először|
|Üzletileg kritikus|Standard|Visszalépés|Át kell telepítenie a elsődleges először|
|Prémium|Általános célú|Visszalépés|Át kell telepítenie a elsődleges először|
|Általános célú|Prémium|Frissítés|Át kell telepítenie a másodlagos először|
|Üzletileg kritikus|Általános célú|Visszalépés|Át kell telepítenie a elsődleges először|
|Általános célú|Üzletileg kritikus|Frissítés|Át kell telepítenie a másodlagos először|
||||

\* Minden 100 DTU standard szintű csomag szükséges legalább 1 virtuális mag, és minden egyes 125 DTU prémium szintű csomag szükséges legalább 1 virtuális mag

## <a name="migration-of-failover-groups"></a>Feladatátvételi csoportok áttelepítése 

Több adatbázis feladatátvételi csoportok áttelepítése az elsődleges és másodlagos adatbázisok az egyes áttelepítési van szükség. A folyamat során a Ha a fenti szempontok és műveleti sorrend vonatkozik. Az adatbázisok konvertálja a Virtuálismag-alapú modellbe, miután a feladatátvételi csoport érvényben marad az ugyanazon házirend-beállításokkal. 

## <a name="creation-of-a-geo-replication-secondary"></a>A georeplikáció másodlagos létrehozása

Csak a geo-secondary elsődlegesként ugyanazon a szolgáltatásszinten használatával hozhat létre. Adatbázis magas log sebességet javasolt, hogy a másodlagos az elsődleges teljesítményszint jön létre. Geo-secondary egyetlen elsődleges adatbázishoz a rugalmas készlet létrehozásakor, ezért javasoljuk, hogy rendelkezik-e a készlet a `maxVCore` beállítása, amely megfelel az elsődleges adatbázishoz tartozó teljesítményszintre. Geo-secondary a egy elsődleges egy másik rugalmas készletben található rugalmas készlet létrehozásakor, ezért javasoljuk, hogy a címkészleteknek azonos `maxVCore` beállításai

## <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Adatbázis másolatának használata a Virtuálismag-alapú adatbázis DTU-alapú adatbázis konvertálása.

A Virtuálismag-alapú teljesítményű szintű korlátozások nélkül, vagy speciális alkalmazás-előkészítés, amennyiben a cél teljesítményszintjét támogatja az adatbázis maximális mérete a forrásadatbázis egy adatbázis bármilyen adatbázis DTU-alapú teljesítményszinttel másolhatja. Ez azért, mert az adatbázis-másolat a másolási művelet kezdési időpontja adatokat pillanatképet készít, és nem hajt végre a forrás- és a cél közötti adatszinkronizálás. 

## <a name="next-steps"></a>További lépések

- További információ a meghatározott teljesítményszintet és a tároló mérete lehetőségek önálló adatbázis elérhető: [SQL Database Virtuálismag-alapú erőforráskorlátok az önálló adatbázisok](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels)
- További tudnivalók az adott teljesítményszintjei és tárolási mérete választható rugalmas készletek számára elérhető: [SQL Database Virtuálismag-alapú erőforráskorlátok a rugalmas készletek](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
