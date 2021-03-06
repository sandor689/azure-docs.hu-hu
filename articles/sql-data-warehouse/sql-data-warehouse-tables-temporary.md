---
title: Az SQL Data Warehouse az ideiglenes táblák |} Microsoft Docs
description: Alapvető útmutatással szolgál az ideiglenes táblák és a munkamenet szintű ideiglenes táblák ezeket az alapelveket emeli ki.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: a3e06a4074bc7b5cd8612162a624718107a50656
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/18/2018
ms.locfileid: "31518399"
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>Az SQL Data Warehouse az ideiglenes táblák
Ez a cikk alapvető útmutatást nyújt az ideiglenes táblák használata, és kiemeli a munkamenet szintű ideiglenes táblák ezeket az alapelveket. Ebben a cikkben szereplő információk segítségével segítséget nyújt a kódot, újrahasznosításának és a karbantartás a kód egyszerű javítása modularize.

## <a name="what-are-temporary-tables"></a>Mik az ideiglenes táblák?
Az ideiglenes táblák hasznosak, ha az adatfeldolgozás - különösen ha a köztes eredmények átmeneti jellegűek átalakítása során. Az SQL Data Warehouse az ideiglenes táblák létezik, a munkamenet szinten.  Csak azok a munkamenethez, amelyben hozta létre, és automatikusan eldobott munkamenetben kijelentkezésekor látható.  Az ideiglenes táblák teljesítmény előnyt kínálnak, mert az eredményeiket írt helyi ahelyett, hogy a távoli tároló.  Az ideiglenes táblák azok is bárhonnan elérhetők a munkamenetben, beleértve belül és kívül tárolt eljárás belül amelyeket kis mértékben eltér az Azure SQL Data Warehouse az Azure SQL Database.

## <a name="create-a-temporary-table"></a>Hozzon létre egy ideiglenes táblát
Az ideiglenes táblák hozza létre a tábla neve az előtag a `#`.  Példa:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

Az ideiglenes táblák is hozhatja létre a `CTAS` pontosan ugyanezt a megközelítést használ:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

> [!NOTE]
> `CTAS` a hatékony parancs és a hozzáadott előnye, hogy hatékony tranzakciónaplók helyhasználatáról használata során. 
> 
> 

## <a name="dropping-temporary-tables"></a>Ideiglenes táblák
Amikor egy új munkamenetet hoz létre, nem ideiglenes táblák léteznie kell.  Azonban ha hívásakor az azonos tárolt eljárás, amely létrehoz egy ideiglenes ugyanazzal a névvel, annak érdekében, hogy a `CREATE TABLE` kimutatásai sikeres egy egyszerű előtti meglétének ellenőrzése a egy `DROP` használhatja az alábbi példában látható módon:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

A kódolási konzisztencia, célszerű használni ezt a mintát táblák és az ideiglenes táblák esetén.  Akkor is érdemes használni `DROP TABLE` eltávolítása az ideiglenes táblák befejezése után, a kódban.  A tárolt eljárás fejlesztési gyakoriak a drop utasítást egybe kötegelik annak érdekében, hogy ezek az objektumok megtisztítva eljárás végén esetében.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kód
Az ideiglenes táblák felhasználói munkamenet bármely részén látható, mert ez az alkalmazás kódjában modularize segítséget is kihasználható.  Például a következő tárolt eljárás DDL a statisztika nevét az adatbázis összes statisztika frissítése állít elő.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Ebben a szakaszban az egyetlen művelet történt feladata a tárolt eljárás, hogy generatess egy ideiglenes tábla #stats_ddl a DDL-utasításokban.  Ez a tárolt eljárás #stats_ddl csökken, ha már létezik annak érdekében, hogy nem sikerül a Ha-munkameneten belül csak egyszer futtatnia.  Azonban mivel nincs `DROP TABLE` a tárolt eljárás végén a tárolt eljárás befejeződésekor elhagyják a létrehozott tábla, hogy a tárolt eljárás kívül tudja olvasni.  Az SQL Data Warehouse szemben a többi SQL Server-adatbázis is lehet az átmeneti táblázat kívül az eljárást, amely létrehozta.  Az SQL Data Warehouse az ideiglenes táblák használható **bárhol** a munkamenet belül. Ez a vezethet további moduláris és kezelhető is legyen kódot az alábbi példában látható módon:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Ideiglenes tábla korlátozásai
Az SQL Data Warehouse ugyanazok a korlátozások néhány, az ideiglenes táblák végrehajtása során.  Jelenleg csak a munkamenet hatókörű ideiglenes táblák támogatottak.  Globális ideiglenes táblák nem támogatottak.  Emellett nézetek nem hozható létre ideiglenes táblákra.

## <a name="next-steps"></a>További lépések
Táblák fejlesztésével kapcsolatos további tudnivalókért tekintse meg a [tábla áttekintése](sql-data-warehouse-tables-overview.md).

