---
title: Az Azure SQL Database erőforrás korlátozza – áttekintés |} A Microsoft Docs
description: Ezen a lapon azt ismerteti, hogy néhány gyakori DTU-alapú erőforráskorlátok az Azure SQL Database önálló adatbázisok számára.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 6f6fa1ebc086530f138d32ee5a9c799b5bfbbdeb
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412110"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Azure SQL Database erőforrás-korlátozások áttekintése 

Ez a cikk az Azure SQL Database erőforrás-áttekintés korlátozza, és mi történik, ha ezek erőforráskorlátok találati vagy túllépte a vonatkozó információkat biztosít.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Mi az a kiszolgálók és adatbázisok maximális száma?

| Maximum | Érték |
| :--- | :--- |
| Adatbázisok kiszolgálónként | 5000 |
| Kiszolgálók száma előfizetésenként bármelyik régióban alapértelmezett száma | 20 |
| Kiszolgálók száma előfizetésenként bármelyik régióban maximális száma | 200 |
| Dtu-k vagy eDTU-kvóta kiszolgálónként | 54,000 |
| virtuális mag kvóta kiszolgálónként | 540 |
| Maximális készletek kiszolgálónként | attól függ, dtu-k vagy virtuális magok száma |
|||

> [!NOTE]
> További /eDTU DTU-kvótába, virtuális mag kvóta vagy további kiszolgálókat, mint az alapértelmezett érték beszerzéséhez egy új támogatási kérelmet az Azure Portalon, a probléma típusa "Kvóta" az előfizetés beküldhető. A dtu-k / eDTU kvóta- és adatbázis-korlát kiszolgálónként kiszolgálónként rugalmas készletek száma korlátozza. 

> [!IMPORTANT]
> Adatbázisok száma megközelíti a korlátot, egy kiszolgálón, mert a következő fordulhat elő:
> - Növelje a várakozási ideje a lekérdezések futtatása a master adatbázison.  Ez magában foglalja az erőforrás-kihasználtsági statisztikáinak például sys.resource_stats nézetét.
> - A felügyeleti műveletek késése növelése, és a renderelési enumerálni a kiszolgálón lévő adatbázisokat érintő portál modelladatok között.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Mi történik az adatbázis erőforrás-korlátok elérésekor?

### <a name="compute-dtus-and-edtus--vcores"></a>COMPUTE (dtu-król és edtu-k / virtuális mag)

(Dtu-k és edtu-k vagy virtuális magok szerint mért) adatbázis számítási mértéke túlságosan megnő, a késés növekedése lekérdezése és még akkor is, időtúllépés is. Ezen feltételek mellett a lekérdezések előfordulhat, hogy a szolgáltatás várólistára kerül, és biztosítják az erőforrások erőforrásként végrehajtás ingyenes válnak.
Amikor magas számítási kihasználtságát, kockázatcsökkentési lehetőségek a következők:

- Az adatbázis vagy a rugalmas készlet további számítási erőforrásokat biztosít az adatbázis teljesítményszintjének növelését. Lásd: [egyetlen adatbázis-erőforrások skálázása](sql-database-single-database-scale.md) és [méretezhető rugalmas adatbáziskészlet erőforrásainak](sql-database-elastic-pool-scale.md).
- Az erőforrás-használatot, az egyes lekérdezések csökkentése érdekében a lekérdezések optimalizálását. További információkért lásd: [lekérdezés hangolása/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Ha használt adatbázis-terület eléri a maximális méretkorlátot, adatbázis szúr be és frissítéseket, amelyek az adatok méretének növelése sikertelen, és azok az ügyfelek kapnak egy [hibaüzenet](sql-database-develop-error-messages.md). Adatbázis-KIVÁLASZTJA és TÖRLÉSEK továbbra is sikeres legyen.

Amikor magas lemezterület-kihasználás, kockázatcsökkentési lehetőségek a következők:

- Az adatbázisához vagy rugalmas maximális méretének növelését tárolókészlet, vagy vegyen fel további tárolókat. Lásd: [egyetlen adatbázis-erőforrások skálázása](sql-database-single-database-scale.md) és [méretezhető rugalmas adatbáziskészlet erőforrásainak](sql-database-elastic-pool-scale.md).
- Ha az adatbázis egy rugalmas készletben, majd azt is megteheti az adatbázis áthelyezhetők a készlet kívül, hogy a tárolóhely nincsenek megosztva, más adatbázisok.
- Az adatbázis nem használt terület felszabadítását zsugorítani. További információkért lásd: [területe az Azure SQL Database kezelése](sql-database-file-space-management.md)

### <a name="sessions-and-workers-requests"></a>A munkamenetek és feldolgozók (kérelmek) 

A munkamenetek és a feldolgozók maximális száma határozza meg a szolgáltatási szint és a teljesítmény szint (dtu-król és edtu-k). Új kérelmek azért lettek elutasítva munkamenet vagy feldolgozói korlát elérésekor, és az ügyfelek hibaüzenetet kaphat. Bár a rendelkezésre álló kapcsolatok számát az alkalmazás által szabályozható, egyidejű feldolgozók száma nehezebb, gyakran becslése és ellenőrzését. Ez különösen igaz csúcsidőszakokban terhelés amikor adatbázis erőforráskorlátok eléri és feldolgozók egymásra hosszabb ideig futó lekérdezések miatt. 

Amikor magas munkamenet vagy feldolgozói kihasználtság, kockázatcsökkentési lehetőségek a következők:
- Növelése a szolgáltatási szint teljesítményi szintjének vagy az adatbázisához vagy rugalmas készletéhez. Lásd: [egyetlen adatbázis-erőforrások skálázása](sql-database-single-database-scale.md) és [méretezhető rugalmas adatbáziskészlet erőforrásainak](sql-database-elastic-pool-scale.md).
- A számítási erőforrások optimalizálása minden egyes lekérdezés az erőforrás-használat csökkentésére, ha a megnövekedett feldolgozó kihasználtsági oka miatt a versengés a lekérdezéseket. További információkért lásd: [lekérdezés hangolása/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>További lépések

- Lásd: [SQL Database: gyakori kérdések](sql-database-faq.md) kapcsolatos gyakori kérdésekre adott válaszokat.
- Azure – általános korlátozások kapcsolatos információkért lásd: [Azure-előfizetés és a szolgáltatások korlátozásai, kvótái és megkötései](../azure-subscription-service-limits.md).
- További információ a dtu-król és edtu-k: [dtu-król és edtu-k](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- A tempdb méretbeli korlátokat kapcsolatos információkért lásd: https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
