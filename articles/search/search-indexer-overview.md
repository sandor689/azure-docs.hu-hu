---
title: Indexelők az Azure Search szolgáltatásban | Microsoft Docs
description: Feltérképezi az Azure SQL Database, az Azure Cosmos DB vagy az Azure Storage tárolókat a kereshető adatok kinyeréséhez és az Azure Search-index feltöltéséhez.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: heidist
ms.openlocfilehash: 2164e0b7cc973969e39f5708bb6509c1ed5f636a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/02/2018
ms.locfileid: "34641135"
---
# <a name="indexers-in-azure-search"></a>Indexelők az Azure Search szolgáltatásban

Egy *indexelő* az Azure Search egy olyan webbejáró, amely kinyeri a kereshető adatok és metaadatok egy külső Azure adatforrásból és tölti fel az indexet alapján mező mező leképezések az index és az adatforrás között. Ezt a megközelítést néha „lekérési modellnek” is nevezik, mivel a szolgáltatás anélkül végzi az adatok lekérését, hogy az adatokat az indexbe küldő kód írására szükség lenne.

Az indexelők adatforrástípusokon vagy -platformokon alapulnak. Az SQL Server egyéni indexelővel rendelkezik az Azure, Cosmos DB, Azure Table Storage, Blob Storage stb. esetében.

Az indexelőt használhatja kizárólag adatfeldolgozásra, illetve olyan kombinált műveletekben is, amelyek részeként az indexelő használatával csak az index bizonyos mezőinek betöltése történik meg.

Az indexelők futtatása történhet igény szerint vagy ismétlődő adatfrissítési ütemterv alapján, akár 15 perces gyakorisággal is. Az ennél gyakoribb frissítésekhez olyan leküldési modellre van szükség, amely egyszerre frissíti az adatokat az Azure Search szolgáltatásban és a külső adatforrásban is.

## <a name="approaches-for-creating-and-managing-indexers"></a>Indexelők létrehozásának és kezelésének módszerei

Az indexelők létrehozása és kezelése a következő módszerekkel történhet:

* [Portál &gt; Adatok importálása varázsló ](search-import-data-portal.md)
* [Szolgáltatás REST API-ja](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Az új indexelőket először előzetes verziójú funkcióként vezetjük be. Az előzetes verziójú funkciók API-kban (REST és .NET) kerülnek bemutatásra, majd később, azok általánosan elérhetővé tétele után integráljuk őket a portál rendszerével. Új indexelő kiértékelése esetén érdemes számolni azzal, hogy kódírásra is sor fog kerülni.


<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Támogatott adatforrások

Az indexelők adattárak feltérképezi az Azure-ban.

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)


## <a name="basic-configuration-steps"></a>Alapszintű konfigurációs lépések
Az indexelők az adott adatforrások esetében egyedi funkciókat biztosítanak. Ezért az indexelő- vagy az adatforrás-konfiguráció egyes szempontjai az indexelő típusától függően változnak. Az alapvető felépítés és követelmények azonban minden indexelő esetében azonosak. Az alábbiakban az összes indexelőre érvényes lépések láthatóak.

### <a name="step-1-create-a-data-source"></a>1. lépés: Adatforrás létrehozása
Az indexelők olyan *adatforrásokból* kérnek le adatokat, amelyek különböző adatokat, például kapcsolati sztringekat és esetleg hitelesítő adatokat is tartalmaznak. Hívja a [adatforrás létrehozása](https://docs.microsoft.com/rest/api/searchservice/create-data-source) REST API vagy [DataSource osztály](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource) az erőforrás létrehozásához.

Az adatforrások konfigurálása és kezelése az azokat használó indexelőktől függetlenül történik, ami azt jelenti, hogy egy adatforrást több indexelő is használhat egyidejűleg, egynél több index betöltésére.

### <a name="step-2-create-an-index"></a>2. lépés: Index létrehozása
Az indexelők automatizálni tudják az adatfeldolgozáshoz kapcsolódó bizonyos feladatokat, de az indexek létrehozása nem tartozik ezek közé. Előfeltételként olyan előre meghatározott indexre van szükség, amelynek mezői egyeznek a külső adatforrás mezőivel. További információk az indexek strukturálásáról: [(Azure Search REST API) Index létrehozása](https://docs.microsoft.com/rest/api/searchservice/Create-Index) vagy [osztály Index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index). A mezőtársításokkal kapcsolatos további információkért lásd [az Azure Search indexelők mezőleképezéseivel](search-indexer-field-mappings.md) foglalkozó témakört.

> [!Tip]
> Az indexelők nem tudnak indexet létrehozni Önnek, de a portál **Adatok importálása** varázslója a segítségére lehet ebben. A legtöbb esetben a varázsló következtetni tud az indexsémára a forrás meglévő metaadataiból, és előállít egy olyan előzetes indexsémát, amely beágyazott módon szerkeszthető mindaddig, amíg a varázsló aktív. Miután létrejött az index a szolgáltatásban, a további szerkesztés a portálon a legtöbb esetben új mezők hozzáadására van korlátozva. A varázsló használatát érdemes megfontolnia az indexek létrehozásakor, de az áttekintésükkor nem. A gyakorlati tanuláshoz végezze el a [portál útmutatójában](search-get-started-portal.md) foglalt lépéseket.

### <a name="step-3-create-and-schedule-the-indexer"></a>3. lépés: Az indexelő létrehozása és ütemezése
Az indexelődefiníció egy olyan konstrukció, amely megadja az indexet, az adatforrást és az ütemezést. Az indexelők egy másik szolgáltatásból is hivatkozhatnak egy adatforrásra, ha az adott adatforrás ugyanabból az előfizetésből származik. További információk az indexelők strukturálásáról: [Indexelő létrehozása (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

## <a name="next-steps"></a>További lépések
Az alapok megismerése után következő lépés a követelmények és az egyes adatforrástípusokra jellemző feladatok áttekintése.

* [Azure SQL Database vagy SQL Server egy Azure virtuális gépen](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [CSV-blobok indexelése az Azure Search Blob indexelőjével](search-howto-index-csv-blobs.md)
* [JSON-blobok indexelése az Azure Search Blob indexelőjével](search-howto-index-json-blobs.md)
