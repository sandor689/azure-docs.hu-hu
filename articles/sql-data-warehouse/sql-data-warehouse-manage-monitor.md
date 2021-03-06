---
title: A dinamikus felügyeleti nézetek használatával számítási feladat figyeléséhez |} Microsoft Docs
description: 'Útmutató: a dinamikus felügyeleti nézetek használatával számítási feladat figyeléséhez.'
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 887fa4b9f950531438986269d041189d45cdecb2
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/18/2018
ms.locfileid: "31522472"
---
# <a name="monitor-your-workload-using-dmvs"></a>Monitor your workload using DMVs
A cikkből megtudhatja, hogyan lehet a számítási feladat figyeléséhez dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) segítségével. Ez magában foglalja az Azure SQL Data Warehouse lekérdezés-végrehajtás vizsgálja.

## <a name="permissions"></a>Engedélyek
Ebben a cikkben a dinamikus felügyeleti nézetek lekérdezéséhez adatbázis állapotának megtekintése vagy a vezérlő engedéllyel kell rendelkezni. Általában támogatást nyújtó ADATBÁZIST a rendszer az előnyben részesített engedély, mert az sokkal jobban korlátozó.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>A figyelő kapcsolatok
Az SQL Data Warehouse minden bejelentkezések a rendszer naplózza [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].  Ez a DMV az utolsó 10 000 bejelentkezések tartalmazza.  A session_id az elsődleges kulcs, és minden egyes új bejelentkezést a egymás után hozzá van rendelve.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>A figyelő a lekérdezés-végrehajtás
Az SQL Data Warehouse végrehajtott összes lekérdezések naplózása [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].  Ez a DMV végrehajtott utolsó 10 000 lekérdezések tartalmazza.  A request_id egyedi módon azonosítja az egyes lekérdezés és a DMV elsődleges kulcsa.  A request_id minden új lekérdezés egymás után hozzá van rendelve, és van előtagként QID jelző lekérdezés azonosítóját.  Ez az egy adott session_id DMV lekérdezése jeleníti meg egy adott bejelentkezési összes lekérdezését.

> [!NOTE]
> Tárolt eljárások több kérelem azonosítókat használjon.  Kérelem azonosítók sorrendben vannak hozzárendelve. 
> 
> 

Az alábbiakban lépést kell végrehajtania, vizsgálja meg a lekérdezés végrehajtási terveket és egy adott lekérdezés idejét.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>1. lépés: A vizsgálni kívánt lekérdezés azonosítása
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Az előző lekérdezés eredménybe **jegyezze fel a kérelem azonosítója** a lekérdezés, amely meg szeretné vizsgálni.

A lekérdezi a **felfüggesztett** állapot éppen sorba feldolgozási korlátok miatt. Ezeket a lekérdezéseket is megjelennek a sys.dm_pdw_waits vár lekérdezés UserConcurrencyResourceType típusú. A feldolgozási korlátok információkért lásd: [teljesítmény rétegek](performance-tiers.md) vagy [erőforrás-osztályok a munkaterhelés felügyeleti](resource-classes-for-workload-management.md). Lekérdezéseket is, amíg a többi okai többek között objektum zárolások.  Ha a lekérdezés arra vár, hogy egy erőforrást, lásd: [Várakozás erőforrásokra lekérdezések kivizsgálása] [ Investigating queries waiting for resources] további le a cikkben.

A Keresés a sys.dm_pdw_exec_requests tábla lekérdezés egyszerűsítése érdekében használja a [címke] [ LABEL] Megjegyzés hozzárendelése a lekérdezést, amely a sys.dm_pdw_exec_requests nézetben lehet keresni.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>2. lépés:, Vizsgálja meg a lekérdezésterv
A Kérelemazonosító segítségével beolvasni a lekérdezés elosztott SQL (DSQL) terv a [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Egy DSQL terv a vártnál tovább tart, amikor az OK lehet egy összetett terv sok DSQL lépést vagy hosszú ideig egyetlen lépésben.  Ha a csomag több áthelyezési műveletek sok lépést, érdemes optimalizálása a táblázat azokat a terjesztéseket adatmozgás csökkentése érdekében. A [tábla terjesztési] [ Table distribution] a cikk azt ismerteti, miért adatok oldja meg a lekérdezés át kell helyezni, és elmagyarázza, néhány terjesztési stratégiák adatmozgás minimalizálása érdekében.

További egyetlen lépésben adatait kell vizsgálni a *operation_type* oszlopa a hosszan futó lekérdezés lépést, és vegye figyelembe a **lépés Index**:

* Folytassa a 3/a. lépés a **SQL-műveletek**: OnOperation, RemoteOperation, ReturnOperation.
* 3b. lépés a folytatásához **adatok mozgási műveletek**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>3/a. lépés: az elosztott adatbázisok SQL vizsgálata
A kérelem azonosítója és a lépés Index használja az adatok beolvasása [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], amely információkat tartalmaz az végrehajtási lekérdezési lépés az összes elosztott adatbázisok.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

A lekérdezés lépés futtatásakor [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] az SQL Server becsült terv lekérése a lépés egy adott terjesztési futó SQL Server gyorsítótárban használható.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>3b. lépés: az elosztott adatbázisok adatátvitel vizsgálata
A kérelem azonosítója és a lépés Index használatával egy adatok adatátviteli lépés az egyes terjesztési futó információkat kérjen le [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Ellenőrizze a *total_elapsed_time* oszlopban tekintheti meg, ha egy adott terjesztési adatátvitelt jelölik a többinél jelentősen tovább tart.
* A hosszan futó terjesztési tekintse meg a *rows_processed* oszlopban tekintheti meg, hogy-e jelentősen nagyobb, mint a többire, hogy a terjesztési mozgatásának sorok száma. Ha igen, a Keresés az alapjául szolgáló adatok eltérésére utalhat.

Ha a lekérdezés fut, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] az SQL Server becsült terv lekérdezni az SQL Server gyorsítótárban a jelenleg futó SQL lépés belül egy adott terjesztési használható.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>A figyelő várakozási lekérdezések
Ha azt tapasztalja, hogy a lekérdezés nem készít folyamatban, mert egy erőforrás vár, ez a lekérdezés, amely tartalmazza az összes erőforrást lekérdezést vár.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Ha a lekérdezés aktívan Várakozás egy másik lekérdezés erőforrások, akkor az állapot lesz **AcquireResources**.  Ha a lekérdezés rendelkezik a szükséges erőforrásokat, akkor az állapot lesz **engedélyezve**.

## <a name="monitor-tempdb"></a>A figyelő a tempdb
A tempdb magas kihasználtsága lehet az alapvető ok, a lassú teljesítmény és a kimenő memóriaproblémák léptek fel. Fontolja meg az adatraktár skálázás, ha a tempdb teljes kapacitásukkal működjenek elérése a lekérdezés végrehajtása közben. Az alábbi adatokat minden egyes csomóponton lekérdezésenként tempdb használatának azonosítása ismerteti. 

Hozzon létre a következő nézetet társítja a megfelelő csomópont sys.dm_pdw_sql_requests Azonosítóját. Ha a csomópont-azonosító lehetővé teszi a más csatlakoztatott dinamikus felügyeleti nézetek használja, és ezek a táblák sys.dm_pdw_sql_requests illesztése.

```sql
-- sys.dm_pdw_sql_requests with the correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
A tempdb figyeléséhez, futtassa a következő lekérdezést:

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a>A figyelő memória

Memória lehet az alapvető ok, a lassú teljesítmény és a kimenő memóriaproblémák léptek fel. Fontolja meg az adatraktár skálázás, ha SQL Server memóriahasználatának teljes kapacitásukkal működjenek elérése a lekérdezés végrehajtása közben.

A következő lekérdezés visszaadja az SQL Server használatát és Memóriaterhelést csomópontonként:   
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a>A figyelő tranzakciós napló mérete
A következő lekérdezés a tranzakciós napló mérete minden terjesztési adja vissza. Ha egy, a naplófájlok 160 GB közel jár, érdemes a példány vertikális felskálázásával, vagy a tranzakció mérete korlátozza. 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
```
## <a name="monitor-transaction-log-rollback"></a>A figyelő tranzakciós napló visszaállítása
Ha a lekérdezések sikertelenek lesznek, vagy folytatja sok időbe telik, ellenőrizze, és figyelése, ha a tranzakciók visszaállítása folyamatban van.
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a>További lépések
További információ a dinamikus felügyeleti nézetek: [rendszernézetek][System views].


<!--Image references-->

<!--Article references-->
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
