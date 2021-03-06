---
title: 'Az Azure Cosmos DB: .NET módosítási hírcsatorna processzor API, SDK és -erőforrások |} A Microsoft Docs'
description: Mindent megtudhat a módosítási hírcsatorna processzor API és az SDK kiadási dátum, a kivezetési dátum és a processzor .NET módosítási hírcsatorna SDK minden verziója között végrehajtott módosítások.
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/21/2018
ms.author: maquaran
ms.openlocfilehash: e8a8edd22fe66df12e9e7327a25e82aa5f07bd1b
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627627"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET módosítási hírcsatorna processzor SDK: Töltse le és kibocsátási megjegyzések
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-módosítási hírcsatorna](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Aszinkron Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST erőforrás-szolgáltató](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor – .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor – Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**SDK letöltése**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API-dokumentáció**|[Módosítsa a Változáscsatorna feldolgozói kódtár API dokumentációja](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Első lépések**|[A módosítási hírcsatorna processzor .NET SDK használatának első lépései](change-feed.md)|
|**Aktuális támogatott keretrendszer**| [Microsoft .NET-keretrendszer 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [A Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="v2-builds"></a>v2 buildek

### <a name="a-name205205"></a><a name="2.0.5"/>2.0.5
* Rögzített versenyhelyzet partition split következik. Bérlet beszerzése azonnal során partition split elvesztése, és a versengés okoz a versenyhelyzet vezethet. Ebben a kiadásban a versenyhelyzet feltétel problémát megoldottuk.

### <a name="a-name204204"></a><a name="2.0.4"/>2.0.4-es
* GA SDK

### <a name="a-name203-prerelease203-prerelease"></a><a name="2.0.3-prerelease"/>2.0.3-prerelease
* Az alábbi problémákat javítja:
  * Ha partition split történik, lehetnek duplikált a felosztás előtt módosított dokumentumok feldolgozása.
  * A GetEstimatedRemainingWork API 0 adott vissza, ha nincs bérleteket sem a bérletek gyűjteményének szerepelt.

* A követező kivételekre nyilvános menjenek végbe. Bővítmények IPartitionProcessor megvalósító nagyvállalat ezeket a kivételeket.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-prerelease
* API-módosítás kisebb:
  * Elavultként megjelölt ChangeFeedProcessorOptions.IsAutoCheckpointEnabled eltávolítva.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-prerelease
* Stabilitási fejlesztések:
  * Jobban kezeli a bérlet tároló inicializálása. Ha bérleti tároló üres, csak egy példánya processzor is inicializálása, a többi várakozik.
  * További stable/hatékony bérlet megújítási felszabadítása. Független megújítását, más az megújítását, és közzétesz egy bérletet egy partíciót. V1-ben, amely azért volt szükség, egymás után az összes partíciót.
* Új v2 API-val:
  * Rugalmas fejlesztés a processzor Builder mintája: ChangeFeedProcessorBuilder osztály.
    * Paraméterek tetszőleges kombinációját is igénybe vehet.
    * Figyelés, illetve a bérlet gyűjtemény (v1-ben nem érhető el) DocumentClient-példányt is igénybe vehet.
  * IChangeFeedObserver.ProcessChangesAsync most már a CancellationToken vesz igénybe.
  * IRemainingWorkEstimator – a fennmaradó munkahelyi estimator külön-külön is használható a processzor.
  * Új bővítési pontok:
    * IParitionLoadBalancingStrategy – egyéni terheléselosztás a partíciók a processzor példányai között.
    * ILease, ILeaseManager – az egyéni partícióbérlés-kezeléssel.
    * IPartitionProcessor - partíción egyéni feldolgozási módosításokat.
* Használja a naplózás – [LibLog](https://github.com/damianh/LibLog) könyvtár.
* 100 %-os visszamenőlegesen kompatibilisek a v1 API-val.
* Új kódbázis.
* Kompatibilis [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.21.1 verzió vagy újabb verzió.

### <a name="v1-builds"></a>a V1-buildek

### <a name="a-name133133"></a><a name="1.3.3"/>1.3.3
* További naplózás hozzáadva.
* A DocumentClient adatszivárgást rögzített, több alkalommal a folyamatban lévő munka-becslésére hívásakor.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* A folyamatban lévő munka-becslésére javításairól.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Jobb stabilitás.
  * Javítsa ki a megszakított feladatok probléma, amely az egyes partíciók vezethet leállított megfigyelőket kezelésére.
* Manuális ellenőrzőpontok támogatása.
* Kompatibilis [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.21 verzió vagy újabb verzió.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* A .NET Standard 2.0 támogat. A csomag most már támogatja a `netstandard2.0` és `net451` keretrendszer zsargonkifejezését.
* Kompatibilis [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.17.0 verzió vagy újabb verzió.
* Kompatibilis [SQL .NET Core SDK](sql-api-sdk-dotnet-core.md) 1.5.1 verzió vagy újabb verzió.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Javít egy problémát a számítás a becsült hátralévő munka, ha üres a módosítási hírcsatorna volt, vagy nincs függőben lévő nem.
* Kompatibilis [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.13.2 verzió vagy újabb verzió.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Becsült hátralévő munkahelyi használatát a dolgozhatók fel a módosítási hírcsatorna beszerzése metódus hozzáadása.
* Kompatibilis [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.13.2 verzió vagy újabb verzió.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK
* Kompatibilis [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.14.1 verziók és az alábbi.


## <a name="release--retirement-dates"></a>Állapot tárolá & kivezetési dátum
A Microsoft legalább értesítést küldenek **12 hónapig** kivonása egy SDK-t kiegyenlítse az a és újabb támogatott verzióra váltás előtt.

Új szolgáltatások és funkciók és optimalizálási lehetőségek csak hozzá az aktuális SDK-hoz, ezért javasoljuk, hogy mindig a legújabb SDK verzióra frissít leghamarabb lehető. 

Cosmos DB-hez a kivont SDK használatával bármilyen kérelmet a rendszer elutasítja a szolgáltatás által.

<br/>

| Verzió | Kiadás dátuma | Visszavonás dátuma |
| --- | --- | --- |
| [1.3.3](#1.3.3) |08 2018. május |--- |
| [1.3.2](#1.3.2) |2018. április 18. |--- |
| [1.3.1](#1.3.1) |2018. március 13. |--- |
| [1.2.0](#1.2.0) |2017. október 31. |--- |
| [1.1.1](#1.1.1) |2017. augusztus 29-én |--- |
| [1.1.0](#1.1.0) |2017. augusztus 13. |--- |
| [1.0.0](#1.0.0) |2017. július 07. |--- |


## <a name="faq"></a>GYIK
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Lásd még
Cosmos DB kapcsolatos további információkért lásd: [a Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján. 

