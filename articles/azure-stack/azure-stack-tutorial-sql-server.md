---
title: SQL-adatbázisok Azure verem felhasználók számára történő elérhetővé |} Microsoft Docs
description: Az oktatóanyag segítséget nyújt az SQL Server erőforrás-szolgáltató telepítéséhez, és hozzon létre kínál, amelyekkel Azure verem felhasználók SQL adatbázisok létrehozására.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/05/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: b9ba2bb89bb0d7e16a28a165cf14530a7a10f71b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234750"
---
# <a name="tutorial-make-sql-databases-available-to-your-azure-stack-users"></a>Oktatóanyag: SQL-adatbázisok felhasználók számára elérhetővé az Azure-verem

Rendszergazdaként Azure verem felhő ajánlatokat, amelyek segítségével a felhasználók hozhat létre (bérlőkkel), amely a felhő-natív alkalmazások, webhelyek és a munkaterhelések használhatják SQL-adatbázisok létrehozására. Egyéni, igény szerinti, felhőalapú adatbázist biztosít a felhasználók számára, mentheti azokat időt és erőforrásokat. Ennek beállításához a fogja végrehajtani:

> [!div class="checklist"]
> * Az erőforrás-szolgáltató SQL Server telepítése
> * Ajánlat létrehozása
> * Az ajánlat tesztelése

## <a name="deploy-the-sql-server-resource-provider"></a>Az erőforrás-szolgáltató SQL Server telepítése

A telepítési folyamat részletes leírása a [Azure verem cikken használja az SQL adatbázisok](azure-stack-sql-resource-provider-deploy.md), és a következő elsődleges lépésekből áll:

1. [Az SQL erőforrás-szolgáltató telepítéséhez](azure-stack-sql-resource-provider-deploy.md).
2. [A telepítés ellenőrzése](azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Adja meg a kapacitás üzemeltetési SQL-kiszolgálójához. További információkért lásd: [üzemeltető kiszolgálók hozzáadása](azure-stack-sql-resource-provider-hosting-servers.md)

## <a name="create-an-offer"></a>Ajánlat létrehozása

1.  [Állítsa be a kvóta](azure-stack-setting-quotas.md) és adjon neki nevet *SQLServerQuota*. Válassza ki **Microsoft.SQLAdapter** a a **Namespace** mező.
2.  [Hozzon létre egy csomagot](azure-stack-create-plan.md). Nevezze el *TestSQLServerPlan*, jelölje be a **Microsoft.SQLAdapter** szolgáltatás, és **SQLServerQuota** kvótát.

    > [!NOTE]
    > Ahhoz, hogy a felhasználók más-alkalmazásai létrehozására, a terv más szolgáltatások akkor lehet szükség. Például az Azure Functions van szükség a **Microsoft.Storage** szolgáltatással a tervet, amíg Wordpress szükséges **Microsoft.MySQLAdapter**.

3.  [Hozzon létre egy ajánlatot](azure-stack-create-offer.md), adjon neki nevet **TestSQLServerOffer** válassza ki a **TestSQLServerPlan** terv.

## <a name="test-the-offer"></a>Az ajánlat tesztelése

Most, hogy az SQL Server erőforrás-szolgáltató telepítése után, és létrehozott egy ajánlatot, bármikor beléphet egy olyan felhasználó nevében, az ajánlat előfizetni, és hozzon létre egy adatbázist.

### <a name="subscribe-to-the-offer"></a>Az ajánlat előfizetés

1. Jelentkezzen be a verem Azure-portálra (https://portal.local.azurestack.external) hez bérlőként.
2. Válassza ki **egy előfizetés** és írja be **TestSQLServerSubscription** alatt **megjelenített név**.
3. Válassza ki **válasszon egy ajánlatot** > **TestSQLServerOffer** > **létrehozása**.
4. Válassza ki **további szolgáltatások** > **előfizetések** > **TestSQLServerSubscription** > **erőforrás szolgáltatók**.
5. Válassza ki **regisztrálása** mellett a **Microsoft.SQLAdapter** szolgáltató.

### <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

1. Válassza ki **+**  >  **adatok + tárolás** > **SQL-adatbázis**.
2. Használja az alapértelmezett értékeket, vagy használjon ezekben a példákban a következő mezőket:
    - **Adatbázis neve**: SQLdb
    - **Maximális méretét megabájtban**: 100
    - **Előfizetés**: TestSQLOffer
    - **Erőforráscsoport**: SQL-rg-n
3. Válassza ki **bejelentkezési beállítások**, adja meg az adatbázis hitelesítő adatait, és válassza **OK**.
4. Válassza ki **SKU** > Válassza ki a létrehozott üzemeltető SQL Server SQL SKU > majd **OK**.
5. Kattintson a **Létrehozás** gombra.

## <a name="next-steps"></a>További lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Az erőforrás-szolgáltató SQL Server telepítése
> * Ajánlat létrehozása
> * Az ajánlat tesztelése

Előzetes tovább az oktatóanyaghoz, megtudhatja, hogyan:

> [!div class="nextstepaction"]
> [Webes, mobil és API-alkalmazások számára elérhetővé a felhasználóknak]( azure-stack-tutorial-app-service.md)