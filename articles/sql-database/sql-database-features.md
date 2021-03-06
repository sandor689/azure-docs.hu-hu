---
title: Az Azure SQL Database funkcióinak összehasonlítása |} A Microsoft Docs
description: Ez a cikk összehasonlítja az SQL Server elérhető szolgáltatásokat az Azure SQL Database különböző változataira jellemző.
services: sql-database
author: jovanpop-msft
ms.reviewer: bonova, carlrab
ms.service: sql-database
ms.topic: conceptual
ms.date: 07/02/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: 0901abe6f973d525220c948f8c32c0b4f342b11a
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092086"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Szolgáltatások összehasonlítása: Azure SQL Database, és az SQL Server összehasonlítása 

Az Azure SQL Database egy általános kódbázis közös SQL Server. Az Azure SQL Database által támogatott SQL Server funkcióit az Ön által létrehozott Azure SQL database-típustól függnek. Az Azure SQL Database, vagy létrehozhat egy adatbázist részeként egy [felügyelt példány](sql-database-managed-instance.md) (jelenleg nyilvános előzetes verzióban), vagy létrehozhat egy adatbázist, amely része a logikai kiszolgáló, és igény szerint elhelyezett rugalmas készletben. 

A Microsoft továbbra is biztosítja a funkciók az Azure SQL Database. Látogasson el a szolgáltatásfrissítések weblap az Azure-hoz a legújabb frissítések ezekkel a szűrőkkel:

* Szűrjön [SQL Database szolgáltatásra](https://azure.microsoft.com/updates/?service=sql-database).
* Szűrjön az általános elérhetőséggel kapcsolatos [bejelentésekre](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) az SQL Database funkcióira vonatkozóan.

## <a name="sql-server-feature-support-in-azure-sql-database"></a>Az Azure SQL Database az SQL Server-funkciók támogatása

A következő táblázat az SQL Server legfontosabb funkcióit, és a szolgáltatás részlegesen vagy teljesen támogatja-e, és a egy a szolgáltatásra vonatkozó bővebb információira mutató hivatkozásokat nyújt információt. 

| **Az SQL szolgáltatás** | **Támogatott az Azure SQL Database logikai kiszolgáló** | **Támogatott az Azure SQL Database/Managed Instance (előzetes verzió)** |
| --- | --- | --- |
| [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Igen – lásd: [tanúsítványtár](sql-database-always-encrypted.md) és [Key vault](sql-database-always-encrypted-azure-key-vault.md) | Igen – lásd: [tanúsítványtár](sql-database-always-encrypted.md) és [Key vault](sql-database-always-encrypted-azure-key-vault.md) |
| [Always On rendelkezésre állási csoportok](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Magas rendelkezésre állású](sql-database-high-availability.md) minden adatbázis részét képezi. Vész-helyreállítási a következő cikkben [az Azure SQL Database üzletmenet-folytonossági funkcióinak áttekintése](sql-database-business-continuity.md) | [Magas rendelkezésre állású](sql-database-high-availability.md) minden adatbázis részét képezi. Vész-helyreállítási a következő cikkben [az Azure SQL Database üzletmenet-folytonossági funkcióinak áttekintése](sql-database-business-continuity.md) |
| [Adatbázis csatolása](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nem | Nem |
| [Alkalmazási szerepkörök](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Igen | Igen |
|[Naplózás](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Igen](sql-database-auditing.md)| [Igen](sql-database-managed-instance-auditing.md) |
| [Az automatikus biztonsági mentés](sql-database-automated-backups.md) | Igen | Igen |
| [Automatikus hangolás (lekérdezésterv)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Igen](sql-database-automatic-tuning.md)| [Igen](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Automatikus hangolás (indexek)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Igen](sql-database-automatic-tuning.md)| Nem |
| [BACPAC-fájl (Exportálás)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Igen – lásd: [SQL Database-adatbázis exportálása](sql-database-export.md) | Nem |
| [BACPAC-fájl (importálás)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Igen – lásd: [SQL-adatbázis importálása](sql-database-import.md) | Nem |
| [Biztonsági MENTÉSI parancs](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nem, csak rendszer által kezdeményezett automatikus biztonsági mentések - látható [automatizált biztonsági mentések](sql-database-automated-backups.md) | Rendszer által kezdeményezett automatikus biztonsági másolatokat és a felhasználó által kezdeményezett csak másolatot biztonsági mentések – lásd: [különbségek biztonsági mentése](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Beépített függvények](https://docs.microsoft.com/sql/t-sql/functions/functions) | Most – tekintse meg az egyes függvények | Igen – lásd: [tárolt eljárások, függvények, eseményindítók különbségek](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Adatváltozások rögzítése](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nem | Igen |
| [Változások követése](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Igen |Igen |
| [Rendezési utasítások](https://docs.microsoft.com/sql/t-sql/statements/collations) | Igen | Igen |
| [Az Oszlopcentrikus indexek](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Igen – [prémium szintű, a Standard szint – S3-as vagy újabb, általános célú csomagban és a kritikus fontosságú üzleti szint](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Igen |
| [Közös nyelvi futtatókörnyezet (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nem | Igen – lásd: [CLR-beli különbségek](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Tartalmazott adatbázisok](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Igen | Igen |
| [Tartalmazott felhasználók](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Igen | Igen |
| [Vezérlőnyelvi kulcsszavak ellenőrzése](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Igen | Igen |
| [Adatbázisközi lekérdezések](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nem – lásd: [rugalmas lekérdezések](sql-database-elastic-query-overview.md) | Igen, valamint [rugalmas lekérdezések](sql-database-elastic-query-overview.md) |
| [Adatbázisközi tranzakciók](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nem | Igen – lásd: [csatolt kiszolgáló különbségek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#linked-servers) |
| [A kurzorok](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Igen |Igen | 
| [Az adattömörítés](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Igen |Igen |
| [Az adatbázisbeli levelezés](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nem | Igen |
| [Data Migration Service (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Igen | Igen |
| [Az adatbázis-tükrözés](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nem | Nem |
| [Adatbázis-konfigurációs beállítások](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Igen | Igen |
| [A Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nem | Nem |
| [Adatbázis-pillanatképek](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nem | Nem |
| [Adattípusok](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Igen |Igen |
| [DBCC-utasítások](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | A legtöbb – tekintse meg az egyes utasításokat | Igen – lásd: [DBCC különbségek](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [DDL-utasítások](https://docs.microsoft.com/sql/t-sql/statements/statements) | A legtöbb – tekintse meg az egyes utasításokat | Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md) |
| [DDL-triggerek](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Csak adatbázis |  Igen |
| [A partíció az elosztott nézetek](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Nem | Igen |
| [Elosztott tranzakciók – az MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Nem – lásd: [rugalmas tranzakciók](sql-database-elastic-transactions-overview.md) |  Nem – lásd: [rugalmas tranzakciók](sql-database-elastic-transactions-overview.md) |
| [DML-utasítások](https://docs.microsoft.com/sql/t-sql/queries/queries) | Igen | Igen |
| [DML-trigger](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | A legtöbb – tekintse meg az egyes utasításokat |  Igen |
| [DMV-k](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Most – tekintse meg az egyes DMV-kkel |  Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md) |
|[Dinamikus adatmaszkolás](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Igen](sql-database-dynamic-data-masking-get-started.md)| [Igen](sql-database-dynamic-data-masking-get-started.md) |
| [Rugalmas készletek](sql-database-elastic-pool.md) | Igen | Beépített – egyetlen felügyelt példány, amelyek ugyanazt a készletet az erőforrások több adatbázis is rendelkezhet. |
| [Eseményértesítések](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Nem – lásd: [riasztások](sql-database-insights-alerts-portal.md) | Igen |
| [Kifejezések](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Igen | Igen |
| [Bővített események](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Néhány – lásd [bővített események SQL Database-ben](sql-database-xevent-db-diff-from-svr.md) | Igen – lásd: [bővített események különbségek ](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Bővített tárolt eljárások](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nem | Nem |
[Fájlok és fájlcsoportok](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Elsődleges fájlcsoport | Igen |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nem | Nem |
| [Teljes szöveges keresés](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Külső szóhatároló nem támogatottak. |Külső szóhatároló nem támogatottak. |
| [Functions](https://docs.microsoft.com/sql/t-sql/functions/functions) | Most – tekintse meg az egyes függvények | Igen – lásd: [tárolt eljárások, függvények, eseményindítók különbségek](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Geo-restore](sql-database-recovery-using-backups.md#geo-restore) | Igen | Nem állíthatja vissza COPY_ONLY tekintse meg a teljes biztonsági mentést, hogy rendszeres időközönként - [biztonsági mentési különbségek](sql-database-managed-instance-transact-sql-information.md#backup) és [különbségek visszaállítása](sql-database-managed-instance-transact-sql-information.md#restore-statement). |
| [Georeplikáció](sql-database-geo-replication-overview.md) | Igen | Nem |
| [Graph-feldolgozás](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Igen | Igen |
| [Memóriabeli optimalizálás](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Igen – [csak a prémium és üzletileg kritikus szintet](sql-database-in-memory.md) | Nem |
| [JSON-adatok támogatása](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Igen](https://docs.microsoft.com/azure/sql-database/sql-database-json-features) | [Igen](https://docs.microsoft.com/azure/sql-database/sql-database-json-features) |
| [Nyelvi elemei](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Most – tekintse meg az egyes elemek |  Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md) |
| [Társított kiszolgálók](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nem – lásd: [rugalmas lekérdezés](sql-database-elastic-query-horizontal-partitioning.md) | Csak az SQL Server és SQL Database |
| [Naplóküldés](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Magas rendelkezésre állású](sql-database-high-availability.md) minden adatbázis részét képezi. Vész-helyreállítási a következő cikkben [az Azure SQL Database üzletmenet-folytonossági funkcióinak áttekintése](sql-database-business-continuity.md) |[Magas rendelkezésre állású](sql-database-high-availability.md) minden adatbázis részét képezi. Vész-helyreállítási a következő cikkben [az Azure SQL Database üzletmenet-folytonossági funkcióinak áttekintése](sql-database-business-continuity.md) |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nem | Nem |
| [Minimális naplózás tömeges importálás során](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nem | Nem |
| [Rendszeradatok módosítása](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nem | Igen |
| [Online Indexműveletek](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Igen | Igen |
| [INDEX](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Nem|Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Igen|Igen|
| [OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Nem|Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Nem|Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Igen|Igen|
| [Operátorok](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | A legtöbb – tekintse meg az egyes operátorokat |Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md) |
| [A particionálás](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Igen | Igen |
| [Időponthoz kötött adatbázis visszaállítása](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Igen – lásd: [SQL adatbázis-helyreállítás](sql-database-recovery-using-backups.md#point-in-time-restore) | Igen – lásd: [SQL adatbázis-helyreállítás](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [A Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nem | Nem |
| [Házirendalapú felügyelet](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nem | Nem |
| [Predikátumokat](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Igen | Igen |
| [R-szolgáltatások](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Előzetes kiadás; Lásd: [machine learning újdonságai](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Nem |
| [Erőforrás-vezérlő](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nem | Igen |
| [RESTORE utasítások](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nem | Igen – lásd: [különbségek visszaállítása](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Adatbázis visszaállítása biztonsági másolatból](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Automatikus biztonsági másolatokat csak - látható [SQL adatbázis-helyreállítás](sql-database-recovery-using-backups.md) | Automatikus biztonsági mentésekből – lásd: [SQL-adatbázis helyreállítási](sql-database-recovery-using-backups.md) és a teljes biztonsági mentés – lásd: [különbségek biztonsági mentése](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Sorszintű biztonság](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Igen | Igen |
| [Szemantikai keresés](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nem | Nem |
| [Sorozatszámok száma](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Igen | Igen |
| [Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nem | Igen – lásd: [Szolgáltatásátvitelszervező-eltérések](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Kiszolgáló konfigurációs beállításai](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nem | Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md) |
| [Utasítások megadása](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | A legtöbb – tekintse meg az egyes utasításokat | Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | Igen | Igen |
| [Térbeli](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Igen | Igen |
| [SQL Data Sync](sql-database-get-started-sql-data-sync.md) | Igen | Nem |
| [SQL Operations Studio](https://docs.microsoft.com/sql/sql-operations-studio/what-is) | Igen | Igen |
| [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nem – lásd: [rugalmas feladatok](sql-database-elastic-jobs-getting-started.md) | Igen – lásd: [különbségek az SQL Server Agent](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nem – lásd: [az Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) | Nem – lásd: [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [SQL Server-naplózás](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Nem – lásd: [SQL Database naplózási szolgáltatásával](sql-database-auditing.md) | Igen – lásd: [különbségek naplózása](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Data Tools (SSDT)] (https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Igen | Igen |
| [Az SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Igen, az Azure Data Factory (ADF) környezetben, ahol csomagok találhatók az SSISDB üzemelteti az Azure SQL Database és az Azure SSIS integrációs modul (IR) végrehajtott, egy felügyelt SSIS lásd [Azure-SSIS integrációs modul létrehozása az ADF-ben](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Hasonlítsa össze az SSIS-funkciók az SQL Database felügyelt példány, lásd: [hasonlítsa össze az SQL Database és a felügyelt példány (előzetes verzió)](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview). | Igen, az Azure Data Factory (ADF) környezetben, ahol csomagok találhatók az SSISDB felügyelt példány által üzemeltetett és az Azure SSIS integrációs modul (IR) végrehajtott, egy felügyelt SSIS lásd [Azure-SSIS integrációs modul létrehozása az ADF-ben](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Hasonlítsa össze az SSIS-funkciók az SQL Database felügyelt példány, lásd: [hasonlítsa össze az SQL Database és a felügyelt példány (előzetes verzió)](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview). |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Igen | Igen |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Igen | Igen |
| [Az SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nem – lásd: [bővített események](sql-database-xevent-db-diff-from-svr.md) | Igen |
| [SQL Server-replikáció](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Kizárólag tranzakciós és pillanatkép-replikációs előfizetők](sql-database-cloud-migrate.md) | Igen – [replikáció az SQL Database felügyelt példányain (nyilvános előzetes verzió)](http://review.docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance) |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nem - [tekintse meg a Power BI](https://docs.microsoft.com/power-bi/) | Nem - [tekintse meg a Power BI](https://docs.microsoft.com/power-bi/) |
| [Tárolt eljárások](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Igen | Igen |
| [A rendszer tárolt függvényei](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Most – tekintse meg az egyes függvények | Igen – lásd: [tárolt eljárások, függvények, eseményindítók különbségek](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Rendszerszintű tárolt eljárásokra](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Néhány – lásd: az egyes tárolt eljárások | Igen – lásd: [tárolt eljárások, függvények, eseményindítók különbségek](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Rendszertáblák](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Néhány – lásd: az egyes táblák | Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md) |
| [Rendszerkatalógus-nézetek](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Néhány – lásd: az egyes nézetek | Igen – lásd: [a T-SQL eltérései](sql-database-managed-instance-transact-sql-information.md) |
| [ideiglenes táblákkal](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | A helyi és adatbázishoz kötődő globális ideiglenes táblák | Helyi és a példány hatókörű globális ideiglenes táblák |
| [Historikus táblák](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Igen](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables) | [Igen](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables) |
|Fenyegetések észlelése|  [Igen](sql-database-threat-detection.md)|[Igen](sql-database-managed-instance-threat-detection.md)|
| [Nyomkövetési jelzők](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Nem | Nem |
| [Változók](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Igen | Igen |
| [Transzparens adattitkosítás (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Igen | Részlegesen, csak a szolgáltatás által felügyelt titkosítás segítségével |
[Virtuális hálózat](../virtual-network/virtual-networks-overview.md) | Részben – lásd: [VNET-végpontok](sql-database-vnet-service-endpoint-rule-overview.md) | Igen, csak a Resource Manager-modell |
| [Windows Server feladatátvételi fürtszolgáltatás](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Magas rendelkezésre állású](sql-database-high-availability.md) minden adatbázis részét képezi. Vész-helyreállítási a következő cikkben [az Azure SQL Database üzletmenet-folytonossági funkcióinak áttekintése](sql-database-business-continuity.md) | [Magas rendelkezésre állású](sql-database-high-availability.md) minden adatbázis részét képezi. Vész-helyreállítási a következő cikkben [az Azure SQL Database üzletmenet-folytonossági funkcióinak áttekintése](sql-database-business-continuity.md) |
| [XML-indexek](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Igen | Igen |

## <a name="next-steps"></a>További lépések

- Az Azure SQL Database szolgáltatással kapcsolatos tudnivalók: [Mi az SQL Database?](sql-database-technical-overview.md)
- A felügyelt példánnyal kapcsolatos további információkért lásd: [mit jelent a felügyelt példány?](sql-database-managed-instance.md).
