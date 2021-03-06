---
title: Az Azure Search szolgáltatásban az indexek lekérdezése a keresési ablak |} A Microsoft Docs
description: Ismerje meg, hogyan használhatja a keresési ablak az Azure Search szolgáltatásban az indexek lekérdezéséhez.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 520d9e7b1899c54d922ff6fb77e0901f9609b029
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004133"
---
# <a name="how-to-use-search-explorer-to-query-indexes-in-azure-search"></a>Az Azure Search lekérdezési indexeket a keresési ablak használatával 

Ez a cikk bemutatja, hogyan kérdezhet le egy meglévő Azure Search-indexeket a **keresési ablak** az Azure Portalon. A keresési ablak használatával nyújt egyszerű vagy teljes Lucene lekérdezési karakterláncokat küldhet bármely meglévő index a szolgáltatásban.

## <a name="open-the-service-dashboard"></a>A szolgáltatás irányítópultjának megnyitása
1. Az [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) bal oldali gyorselérési sávján kattintson a **Minden erőforrás** elemre.
2. Válassza ki az Azure Search szolgáltatást.

## <a name="select-an-index"></a>Index kiválasztása

Válassza ki az **Indexek** csempén azt az indexet, amelyből keresni kíván.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Nyissa meg a keresési ablak

Kattintson a keresési ablak csempére a keresősáv megnyitásához a és az eredmények ablaktáblán.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Indítson keresést

A keresési ablak használatakor megadhatja [lekérdezési paramétereket](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) határozhatja meg a lekérdezést.

1. A **Lekérdezési sztring** területen írjon be egy lekérdezést, majd kattintson a **Keresés** gombra. 

   A rendszer automatikusan átalakítja a lekérdezési sztringet a megfelelő kérelmi URL-címmé, és HTTP-kérelmet küldjön az Azure Search REST API-nak.   
   
   A kérelem létrehozásához bármilyen érvényes egyszerű vagy teljes Lucene lekérdezési szintaxist használhat. A `*` karakter üres vagy nem meghatározott keresésnek felel meg, amely az összes dokumentumot visszaadja véletlenszerű sorrendben.

2. Az **Eredmények** területen a lekérdezési eredmények nyers JSON formátumban jelennek meg, hasonlóan a HTTP-választörzsekben visszaadott hasznos adatokhoz, amikor programozott módon ad ki kéréseket.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>További lépések

Az alábbi forrásokban további tudnivalókat és példákat találhat a lekérdezési szintaxisokról.

 + [Egyszerű lekérdezési szintaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene lekérdezési szintaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Lucene lekérdezési szintaxis példái](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [OData szűrési kifejezés szintaxisa](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 