---
title: Erőforrás-osztályok a munkaterhelés felügyeleti - Azure SQL Data Warehouse |} Microsoft Docs
description: Útmutató a feldolgozási kezelése és számítási erőforrásokat az Azure SQL Data Warehouse lekérdezések erőforrás osztályok használatával.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/26/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 09fd39865a52767195ebf7dad13f24d883af476a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/28/2018
ms.locfileid: "32192781"
---
# <a name="workload-management-with-resource-classes-in-azure-sql-data-warehouse"></a>Munkaterhelés-kezelés az Azure SQL Data warehouse erőforrás osztályok
Az erőforrás-osztályok kezeli a memória és az Azure SQL Data Warehouse lekérdezéseivel párhuzamossági útmutatást.  
 
## <a name="what-is-workload-management"></a>Mi az az alkalmazások és szolgáltatások felügyeleti?
Munkaterhelés-kezelés a teljesítménye az összes lekérdezés optimalizálására való képességet. Egy jól bennünket munkaterhelés lekérdezések és hatékonyan függetlenül, hogy azok számítási igényű vagy IO-igényes terhelés műveletek futtatására szolgál.  Az SQL Data Warehouse többfelhasználós környezetben munkaterhelés funkciókat biztosít. Adatraktár célja nem több-bérlős munkaterhelések.

Egy adatraktár teljesítménye kapacitása határozza meg a [az adatraktár-egység](what-is-a-data-warehouse-unit-dwu-cdwu.md). 

- A memória és a feldolgozási korlátok, a teljesítmény-profilokhoz megtekintése: [memória és a feldolgozási korlátok](memory-and-concurrency-limits.md).
- Teljesítmény kapacitás beállításához is [felfelé vagy lefelé méretezési](quickstart-scale-compute-portal.md).

A teljesítmény a lekérdezés határozza meg, a lekérdezés erőforrásosztály. Ez a cikk fennmaradó erőforrás osztályok vannak, és kiigazítja úgy, hogy miként ismerteti.

## <a name="what-are-resource-classes"></a>Mik azok a erőforrás osztályok?
A teljesítmény a lekérdezés határozza meg, a felhasználó erőforrásosztály.  Erőforrás-osztályok előre erőforrás korlát az Azure SQL Data Warehouse szabályozására, a számítási erőforrásokat és a lekérdezés-végrehajtáshoz párhuzamossági határozzák meg. Erőforrás-osztályok segítségével felügyelhetők a terhelést a párhuzamosan futó lekérdezések és minden egyes lekérdezés rendelt számítási erőforrások számának korlátozása. Van egy kereskedelmi ki memória és a feldolgozási mód között.

- Kisebb erőforrás-osztályok lekérdezésenként maximális memória csökkenti, de növelheti a párhuzamosságot.
- Nagyobb erőforrás osztályok növeli a maximális memóriaméret lekérdezésenként, de egyidejű csökkentése. 

Az erőforrás-osztályok két típusa van:

- Statikus erőforrások osztályok, amely alkalmas a rögzített adatok mérete nagyobb feldolgozási.
- Dinamikus erőforrás osztályok, amelyek kiválóan alkalmas adatok csoportjai számára, amelyek mérete növekszik és a teljesítmény növelése, mint a szolgáltatási szint kiterjesztett van.   

Erőforrás osztályok párhuzamossági üzembe helyezési ponti segítségével mérheti hálózatierőforrás-fogyasztás.  [Párhuzamossági üzembe helyezési ponti](#concurrency-slots) magyarázatát a cikk későbbi részében. 

- Az erőforrás-osztályok az erőforrás-használat megtekintéséhez lásd: [memória és a feldolgozási korlátok](memory-and-concurrency-limits.md#concurrency-maximums).
- Erőforrásosztály beállításához futtassa a lekérdezést egy másik felhasználói vagy [módosítása az aktuális felhasználó erőforrásosztály](#change-a-users-resource-class) tagságát. 

### <a name="static-resource-classes"></a>Statikus erőforrás osztályok
Statikus erőforrás osztályok foglal le a jelenlegi teljesítményszintje, amit a függetlenül azonos memóriamennyiség [az adatraktár-egység](what-is-a-data-warehouse-unit-dwu-cdwu.md). Mivel a lekérdezések ugyanazt a memóriafoglalás beolvasása a teljesítményszintet függetlenül [az adatraktár kiterjesztése](quickstart-scale-compute-portal.md) lehetővé teszi, hogy további lekérdezések erőforrás osztályon belül.  Épp ezért tökéletes választás, ha ismert adatmennyiség és állandó statikus erőforrás osztályokat.

A statikus erőforrás osztályok ezek előre meghatározott adatbázis-szerepkörök vannak megvalósítva:

- staticrc10
- staticrc20
- staticrc30
- staticrc40
- staticrc50
- staticrc60
- staticrc70
- staticrc80

### <a name="dynamic-resource-classes"></a>Dinamikus erőforrás-osztályok
Dinamikus erőforrás-osztályok a változó méretű memória, attól függően, hogy az aktuális szolgáltatási szint lefoglalni. Statikus erőforrás osztályok magasabb feldolgozási és statikus az adatkötetek előnyösek, miközben a dinamikus erőforrás-osztályok alkalmasabbak a növekvő vagy változó méretű adatok.  Vertikális felskálázás nagyobb szolgáltatás szintjét, a lekérdezések automatikusan kapja memóriáját.  

A dinamikus erőforrás-osztályok ezek előre meghatározott adatbázis-szerepkörök vannak megvalósítva:

- smallrc
- mediumrc
- largerc
- xlargerc 

### <a name="gen2-dynamic-resource-classes-are-truly-dynamic"></a>Gen2 dinamikus erőforrás osztályokat valóban dinamikus
Amikor digging dinamikus erőforrás osztályokat a Gen1 részleteinek, van néhány, a nagyobb fokú összetettségével jár hozzáadása azok viselkedését az adatokat:

- A smallrc erőforrásosztály, például a statikus erőforrásosztály egy rögzített méretű memória modell működik.  Smallrc lekérdezések dinamikusan beolvasni több memóriát a szolgáltatási szintje itt magasabb, mint.
- Szolgáltatási szintek módosítása, lépjen a rendelkezésre álló lekérdezés egyidejű felfelé vagy lefelé.
- Szolgáltatások szintjeinek skálázás nem biztosít arányos módosítását a azonos erőforrás osztályok számára fenntartott memória mérete.

A **Gen2 csak**, dinamikus erőforrás osztályokat valóban dinamikus a fent említett pontok címzést.  Az új szabály 3-10-22-70 memória százalékos megosztásának kicsi-medium-nagy-xlarge erőforrás osztályok, a rendszer **függetlenül a szolgáltatási szint**.  Az alábbi táblázat tartalmaz memória foglalási szolgáltatásajánlatokat és a minimális száma párhuzamos lekérdezések, függetlenül a szolgáltatási szint konszolidált részleteit.

| Erőforrásosztály | Memória százalékos aránya | Min egyidejű lekérdezések |
|:--------------:|:-----------------:|:----------------------:|
| smallrc        | 3 %                | 32                     |
| mediumrc       | 10%               | 10                     |
| largerc        | 22 %               | 4                      |
| xlargerc       | 70%               | 1                      |


### <a name="default-resource-class"></a>Alapértelmezett erőforrásosztály
Alapértelmezés szerint minden felhasználó tagja a dinamikus erőforrásosztály **smallrc**. 

A szolgáltatás-rendszergazda erőforrásosztály rögzített, és nem módosítható.  A szolgáltatás-rendszergazda az a felhasználó a telepítési folyamat során létrehozott.

> [!NOTE]
> Felhasználók vagy csoportok Active Directory-rendszergazda meghatározott egyaránt szolgáltatás-rendszergazdák.
>
>

## <a name="resource-class-operations"></a>Erőforrás osztály műveletek

Erőforrás-osztályokat úgy tervezték, hogy az adatok kezelése és adatkezelési tevékenységek teljesítményének. Összetett lekérdezéseket is is kihasználhatja a le egy nagyméretű erőforrásosztály alatti futáshoz. Például a teljesítmény nagy méretű táblákra lekérdezni, és rendezés javíthatja a erőforrásosztály elég nagy ahhoz, hogy a memóriában lefuttatandó lekérdezés esetén.

### <a name="operations-governed-by-resource-classes"></a>Erőforrás-osztályok által szabályozott műveletek

Ezek a műveletek erőforrás osztályok vonatkoznak:

* INSERT SELECT, UPDATE, DELETE
* Válassza ki a (felhasználói táblák lekérdezésekor)
* ALTER INDEX - ÚJRAÉPÍTÉS vagy ÁTSZERVEZ
* ALTER TABLE REBUILD
* INDEX LÉTREHOZÁSA
* HOZZON LÉTRE FÜRTÖZÖTT OSZLOPCENTRIKUS INDEXET
* TABLE AS SELECT (CTAS) LÉTREHOZÁSA
* Az adatok betöltése
* Az adatátviteli műveletek elvégzése az adatok adatátviteli szolgáltatás (DMS)

> [!NOTE]  
> Válassza ki a dinamikus felügyeleti nézetekkel (dinamikus felügyeleti nézetek) utasítás vagy más nézetek nem szabályozza a feldolgozási korlátok bármelyikét rendszer. A rendszer függetlenül lekérdezések végrehajtása a figyelheti.
> 
> 

### <a name="operations-not-governed-by-resource-classes"></a>Erőforrás-osztályok nem szabályozzák műveletek
Néhány lekérdezést mindig futtassa a smallrc erőforrásosztály, annak ellenére, hogy a felhasználó tagja egy nagyobb erőforrásosztály. A következő kivétel lekérdezések nem számítanak bele a feldolgozási korlátot. Például ha a feldolgozási korlátot 16, sok felhasználó is jutni rendszer nézetek a rendelkezésre álló párhuzamossági üzembe helyezési ponti befolyásolása nélkül.

A következő utasítás nem érvényes erőforrás-osztályok a, és mindig smallrc futtassa:

* DROP TABLE vagy létrehozása
* AZ ALTER TABLE... KAPCSOLÓ, a megosztott vagy a partíció EGYESÍTÉSE
* AZ ALTER INDEX LETILTÁSA
* A DROP INDEX
* LÉTREHOZÁSI, frissítési vagy a DROP STATISTICS
* A TRUNCATE TABLE
* AZ ALTER ENGEDÉLYEZÉSI
* BEJELENTKEZÉS LÉTREHOZÁSA
* CREATE, a módosítás és a DROP USER
* CREATE, ALTER vagy DROP ELJÁRÁST
* Vagy DROP NÉZET létrehozása
* ÉRTÉKEK BESZÚRÁSA
* Válassza ki a rendszer nézetek és dinamikus felügyeleti nézetek
* MAGYARÁZÓ
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="concurrency-slots"></a>Párhuzamossági tárhelyek
Párhuzamossági üzembe helyezési ponti kényelmesen nyomon követheti a lekérdezés-végrehajtás rendelkezésre álló erőforrások. Például a munkaállomásokat, egy energiaoptimalizálást egyszerre lefoglalni, mivel a ülési korlátozott megvásárolt jegyek vannak. Párhuzamossági helyek / adatraktár teljes száma a szolgáltatási szint határozza meg. A lekérdezés végrehajtása szükséges, foglaljon le elegendő feldolgozási üzembe helyezési ponti képesnek kell lennie. A lekérdezés befejeztével a feldolgozási üzembe helyezési ponti felszabadít.  

- 10 egyidejű üzembe helyezési ponti a futó lekérdezések férhetnek hozzá a 2 párhuzamossági üzembe helyezési ponti futó lekérdezések mint 5 alkalommal több számítási erőforrással.
- Ha minden lekérdezéshez 10 egyidejű tárhelyek és 40 párhuzamossági tárhelyek, majd csak 4 lekérdezések egyidejűleg is futtathatók.
 
Csak a szabályozott erőforrás lekérdezések párhuzamossági üzembe helyezési ponti felhasználását. Rendszer lekérdezések és az egyes triviális lekérdezések nem igényelnek tárolóhely. A lekérdezés erőforrásosztály felhasznált párhuzamossági tárhelyek pontos számát határozza meg.

## <a name="view-the-resource-classes"></a>Az erőforrás-osztályok megtekintése

Erőforrás osztályok, előre meghatározott adatbázis-szerepkörök vannak megvalósítva. Erőforrás-osztályok két típusa van: a dinamikus és statikus. Az erőforrás-osztályok listájának megtekintéséhez használja a következő lekérdezést:

```sql
SELECT name 
FROM   sys.database_principals
WHERE  name LIKE '%rc%' AND type_desc = 'DATABASE_ROLE';
```

## <a name="change-a-users-resource-class"></a>A felhasználó erőforrásosztály módosítása

Erőforrás-osztályok rendelhet a felhasználói adatbázis-szerepkörök valósíthatók meg. Amikor egy felhasználó egy lekérdezést futtatja, a felhasználó erőforrásosztály fut, a lekérdezés. Például ha egy felhasználó a smallrc vagy staticrc10 adatbázis-szerepkör tagja, a lekérdezések futtatása kis mennyiségű memóriával. Ha adatbázis-felhasználó tagja a xlargerc vagy staticrc80 adatbázis-szerepkörök, a lekérdezések futtatása, nagy mennyiségű memóriával. 

A felhasználó erőforrásosztály növeléséhez használja a következő tárolt eljárás [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql). 

```sql
EXEC sp_addrolemember 'largerc', 'loaduser';
```

Erőforrásosztály csökkentéséhez használja [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql).  

```sql
EXEC sp_droprolemember 'largerc', 'loaduser';
```

## <a name="resource-class-precedence"></a>Erőforrás osztály sorrendje
Felhasználók több erőforrás-osztályok tagjait is lehet. Amikor egy felhasználó tartozik egynél több erőforrásosztály:

- Dinamikus erőforrás-osztályok élveznek statikus erőforrás osztályok. Például ha egy felhasználó mediumrc(dynamic) és staticrc80 (statikus) is tagja, lekérdezések futtatása a mediumrc.
- Nagyobb erőforrás osztályok élveznek kisebb erőforrás-osztályok. Például ha egy felhasználó tagja mediumrc és largerc, lekérdezések futtatása a largerc. Hasonlóképpen ha a felhasználó staticrc20 és statirc80 is tagja, lekérdezések futni staticrc80 erőforrás-hozzárendelések.

## <a name="recommendations"></a>Javaslatok
Javasoljuk, hogy egy adott típusú lekérdezés futtatására van kijelölve a felhasználó létrehozásának vagy betölteni az operations. Adja meg, hogy a felhasználó egy állandó erőforrásosztály gyakran erőforrásosztály módosítása helyett. Fényében, hogy a statikus erőforrás osztályok, melynek értéke a munkaterhelések általános szabályozáshoz is javasoljuk, hogy ezek az első használata előtt annak eldöntéséhez, hogy a dinamikus erőforrás-osztályok.

### <a name="resource-classes-for-load-users"></a>Erőforrás-osztályok a terhelés felhasználók
`CREATE TABLE` használja a fürtözött oszlopcentrikus indexek alapértelmezés szerint. Az adatok tömörítése be egy oszlopcentrikus index a memóriaigényes művelet, és Memóriaterhelést csökkentheti az index minőségére. Ezért akkor valószínűleg egy magasabb erőforrásosztály megkövetelése adatainak betöltésekor. Annak érdekében, hogy nincs elég memória a terhelés, hozzon létre egy olyan felhasználó, terhelések futtatására van kijelölve, és a felhasználót egy magasabb erőforrásosztály kell rendelni.

Terhelések hatékonyan feldolgozásához szükséges memória betöltve a táblázat és az adatok mérete jellegétől függ. Memóriára vonatkozó követelményeknek további információkért lásd: [sorcsoport minőségi maximalizálva](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

Miután megadta, hogy a memóriára vonatkozó követelmény, válasszon, hogy a terhelés felhasználó hozzárendelése egy statikus vagy dinamikus erőforrás-keresésnél.

- A statikus erőforrásosztály használja, ha egy adott tartományba esnek tábla memóriára vonatkozó követelményeknek. Terhelések futtassa a megfelelő memória. Az adatraktár méretezni, ha a terhelést nem kell több memóriát. Statikus erőforrásosztály használatával a memória-foglalásokat állandó marad. A konzisztencia kevés a memória, és lehetővé teszi több lekérdezés egyidejű futtatását. Azt javasoljuk, hogy az új megoldások használja-e a statikus erőforrás osztályokat először ezek nagyobb ellenőrzést biztosít.
- Használja a dinamikus erőforrásosztály, ha a tábla memória követelmények eltérőek lehetnek. Terhelések előfordulhat, hogy az aktuális DWU-nál több memóriát vagy cDWU szintet biztosít. Ezért az adatraktár skálázás ad hozzá több memóriát terhelés műveletek, amely lehetővé teszi a terhelés gyorsabb végrehajtásához.

### <a name="resource-classes-for-queries"></a>Erőforrás-osztályok a lekérdezések

Néhány lekérdezést számítási igényű és nem vannak.  

- Válassza ki a dinamikus erőforrásosztály lekérdezések összetett, de nem kell nagy feldolgozási.  Például naponta vagy hetente jelentéseket erőforrások alkalmanként szükség. A jelentések nagy mennyiségű adat feldolgozás alatt, ha a felhasználó meglévő erőforrásosztály több memóriát az adatraktár skálázás biztosít.
- Erőforrás elvárásainak eltérők lehetnek, napjainkat statikus erőforrásosztály kiválasztása Például egy statikus erőforrásosztály jól működik, ha az adatraktár sokan le kell kérdezni. Az adatraktár méretezés, a felhasználó lefoglalt memória mennyisége nem módosítja. Következésképpen több lekérdezéseket a rendszer párhuzamosan hajtható végre.

A megfelelő memóriaengedély kiválasztásával számos tényezőtől függ, például a lekérdezett adatok mennyisége, a táblasémákat és különböző illesztési, válassza ki, és predikátumok csoportban. Általában további memória lefoglalásakor lehetővé teszi, hogy a lekérdezéseket, amelyekkel gyorsabban befejeződjenek, de csökkenti a teljes feldolgozási. Párhuzamossági darabolása nem okoz problémát, ha az túlzott memória lefoglalásakor nem árt átviteli sebesség. 

Teljesítmény hangolására, használja a különböző erőforrás osztályokat. A következő szakasz biztosít a tárolt eljárás, amelynek segítségével mérje fel, a legjobb erőforrásosztály.

## <a name="example-code-for-finding-the-best-resource-class"></a>A legjobb erőforrásosztály kereséséhez példakód
 
A következő tárolt eljárást használhatja a **Gen1 csak** megállapíthatja, hogy feldolgozási és a memória adja meg egy erőforrás egy adott slo-t, és a legközelebbi legjobb erőforrás osztály intenzív közösségi koordináló intézet műveletek közösségi koordináló intézet nem particionált tábla a memória a megadott erőforrásosztály:

A tárolt eljárás célja van:  
1. Párhuzamossági és adja meg, egy adott SLO erőforrásosztály / memória megtekintéséhez. Felhasználói kell megadni a séma és a tablename NULL, ebben a példában látható módon.  
2. A legközelebbi legjobb erőforrásosztály megtekintéséhez a memóriaigényes közösségi koordináló intézet műveleteket (terhelésétől, a másolási tábla rebuild index stb.) a nem particionált közösségi koordináló intézet tábla adott erőforrás osztályra. A tárolt eljárás tudja meg a szükséges memória biztosítása tábla sémáját használja.

### <a name="dependencies--restrictions"></a>Függőségek és korlátozások:
- Ez a tárolt eljárás nem célja, hogy a tábla particionált közösségi koordináló intézet memóriakövetelményét kiszámításához.    
- Ez a tárolt eljárás nem memóriakövetelményét CTAS/INSERT-VÁLASZTANI SELECT részében figyelembe veszi, és azt feltételezi, hogy azt egy jelöljön ki.
- A tárolt eljárás egy ideiglenes táblát, amelyik elérhető a munkamenet hol jött létre a tárolt eljárás használ.    
- Ez a tárolt eljárás attól függ, az aktuális offerings (például hardverkonfiguráció, DMS config), és ha bármelyik, amely módosítja majd a tárolt eljárás nem működik megfelelően.  
- A tárolt eljárás attól függ, meglévő felajánlott feldolgozási korlátot, és ha megváltozik, majd a tárolt eljárás nem megfelelően fog működni.  
- Ez a tárolt eljárás meglévő erőforrás osztály ajánlatok függ, és ha megváltozik, majd a tárolt eljárás nem megfelelően fog működni.  

>  [!NOTE]  
>  Ha kimeneti után tárolt eljárás végrehajtása a megadott paraméterek nem kap, majd lehet két esetben. <br />1. Vagy DW paraméter értéke érvénytelen SLO <br />2. Vagy a közösségi koordináló intézet művelet a táblán nincs egyező erőforrás osztály van. <br />Például DW100, a legmagasabb memóriabeli ideiglenes 400 MB és táblaséma kiterjedő megfelelő a kereszt-követelmény 400 MB.
      
### <a name="usage-example"></a>Példa a használatra:
Szintaxis:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU: Adja meg az aktuális DWU kinyerése az Adatraktár-adatbázisban, vagy bármely támogatott DWU "DW100" formájában adja meg egy NULL értékű paramétert vagy
2. @SCHEMA_NAME: Adja meg a tábla a séma neve
3. @TABLE_NAME: Adjon meg egy tábla nevét, a fontos

A tárolt eljárás végrehajtása példák:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```
> [!NOTE]
> Megadott jelen verziójában a tárolt eljárás csak a Gen1 vonatkoznak.
>
>

A következő utasítás Table1 használt a fenti példákban hoz létre.
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

### <a name="stored-procedure-definition"></a>Tárolt eljárás meghatározása

```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(@DWU VARCHAR(7),
 @SCHEMA_NAME VARCHAR(128),
 @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temp table (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```




## <a name="next-steps"></a>További lépések
Adatbázis-felhasználók és biztonsági kezelésével kapcsolatos további információkért lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse]. További információ a hogyan nagyobb erőforrás osztályok javíthatja a fürtözött oszlopcentrikus index minőségének, lásd: [memória optimalizálás oszlopcentrikus tömörítési](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Rebuilding indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
