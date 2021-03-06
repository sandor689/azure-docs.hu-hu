---
title: Azure-erőforrások áthelyezése új vagy erőforráscsoportonként csoportba |} A Microsoft Docs
description: Azure Resource Manager segítségével az erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/02/2018
ms.author: tomfitz
ms.openlocfilehash: 69614fe84941ea2003d39de165c692b812d10785
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503580"
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe

Ez a cikk bemutatja, hogyan kell helyeznie az erőforrásokat, vagy egy új előfizetést, vagy egy új erőforráscsoport ugyanabban az előfizetésben. A portal, PowerShell, az Azure CLI vagy a REST API segítségével erőforrás áthelyezése. Ez a cikk az áthelyezési műveletek lesznek elérhetők minden olyan segítség nélkül az Azure-támogatás.

Erőforrások áthelyezésekor mind a forrás és a cél csoport zárolva vannak a művelet során. Írási és törlési műveletek az áthelyezés befejezéséig az erőforráscsoportok elakad. A zárolás azt jelenti, hogy a nem hozzáadása, frissítése vagy törlése az erőforráscsoportok erőforrásaihoz, de ez nem jelenti azt, az erőforrások szüneteltetve legyenek. Ha például egy SQL Server és az adatbázis áthelyezése egy új erőforráscsoportot, ha nem az adatbázist használó alkalmazások teljesen állásidő nélkül. Továbbra is olvasni és írni az adatbázisba.

Az erőforrás helye nem módosítható. Erőforrások áthelyezése csak áthelyezi egy új erőforráscsoportot. Az új erőforráscsoportot egy másik helyre azonban lehet, hogy az erőforrás helye nem változik.

> [!NOTE]
> Ez a cikk ismerteti, hogyan helyezheti át egy meglévő Azure-erőforrások fiók ajánlatát. Ha valójában módosítani szeretné az Azure-fiókkal (például frissítése használatalapú fizetésre, és Előre fizetés) ajánlat miközben továbbra is a meglévő erőforrásokkal, lásd: [Váltás másik ajánlatra az Azure-előfizetés](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Erőforrások áthelyezése előtti ellenőrzőlistát

Néhány fontos lépést végre kell hajtani az erőforrások áthelyezése előtt. Ezen feltételek ellenőrzésével a hibák elkerülhetőek.

1. A forrás- és az előfizetések léteznie kell az előfizetésen belül [Azure Active Directory-bérlő](../active-directory/develop/quickstart-create-new-tenant.md). Ellenőrizze, hogy mindkét előfizetéshez tartozik-e az azonos bérlő azonosítója, használja az Azure PowerShell vagy az Azure CLI.

  Azure PowerShell esetén használja:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Azure CLI esetén használja az alábbi parancsot:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Ha a bérlőazonosítók a forrás- és előfizetés esetében nem ugyanaz, a következő módszerek használatával a bérlőazonosítók egyeztetése:

  * [Azure-előfizetés tulajdonjogának átruházása másik fiókra](../billing/billing-subscription-transfer.md)
  * [Társításával vagy Azure-előfizetés hozzáadása az Azure Active Directoryhoz](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

2. A szolgáltatásnak lehetővé kell tennie az erőforrások áthelyezését. Ez a cikk az erőforrások áthelyezését engedélyező szolgáltatásokat, és mely szolgáltatások nem engedélyezi az erőforrások áthelyezése sorolja fel.
3. A cél előfizetést regisztrálni kell az áthelyezett erőforrás erőforrás-szolgáltatóján. Ha nem, akkor megjelenik egy hibaüzenet, arról, hogy a **az előfizetés nincs regisztrálva az erőforrástípushoz**. Ez a probléma erőforrások új előfizetésre történő áthelyezésekor fordulhat elő, ha az előfizetést még nem használták az adott erőforrástípushoz.

  A PowerShell a következő parancsok használatával a regisztrációs állapot lekérdezése:

  ```powershell
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Erőforrás-szolgáltató regisztrálásához használja:

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
  ```

  Azure CLI-hez a következő parancsok használatával a regisztrációs állapot lekérdezése:

  ```azurecli-interactive
  az account set -s <destination-subscription-name-or-id>
  az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
  ```

  Erőforrás-szolgáltató regisztrálásához használja:

  ```azurecli-interactive
  az provider register --namespace Microsoft.Batch
  ```

4. Erőforrások áthelyezése a fióknak legalább a következő engedélyekkel kell rendelkeznie:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** a a forrásként szolgáló erőforráscsoportot.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** a cél erőforráscsoport található.

5. Erőforrások áthelyezése előtt tekintse át az erőforrásokat az előfizetés az előfizetési kvóták. Erőforrások áthelyezése azt jelenti, hogy az előfizetés túl fogja lépni a teljesítménye korlátait, ha meg kell tekintenie a e kérheti a kvótájának növelését. Korlátok, és hogyan lehet növelni listáját lásd: [Azure-előfizetés és a szolgáltatások korlátozásai, kvótái és megkötései](../azure-subscription-service-limits.md).

5. Ha lehetséges, nagy break helyezi át a külön áthelyezési műveleteket. Erőforrás-kezelő azonnal egyetlen művelettel több mint 800 erőforrások áthelyezéséhez kísérlet sikertelen lesz. Azonban legalább 800 erőforrások áthelyezése is meghiúsulhat időtúllépés által.

## <a name="when-to-call-support"></a>Ha az ügyfélszolgálat

Továbbléphet a legtöbb erőforrásai a jelen cikkben ismertetett önkiszolgáló műveletekhez. Az önkiszolgáló műveletekhez való használata:

* Resource Manager-erőforrások áthelyezése.
* Klasszikus erőforrások áthelyezése a [klasszikus üzembe helyezési korlátozásoknak](#classic-deployment-limitations).

Kapcsolattartó [támogatja](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) kell tennie:

* Erőforrások áthelyezése új Azure-fiókba (és az Azure Active Directory-bérlő), és az utasítások az előző szakaszban segítségre van szüksége.
* Klasszikus erőforrások áthelyezésekor merülnek fel a korlátozások.

## <a name="services-that-can-be-moved"></a>Áthelyezhető szolgáltatások

A szolgáltatások, amelyek lehetővé teszik egy új erőforráscsoportot és az előfizetés áthelyezése a következők:

* API Management
* Az App Service apps (webalkalmazások) – lásd: [App Service korlátai](#app-service-limitations)
* App Service-tanúsítvány
* Application Insights
* Analysis Services
* Automation
* Azure Active Directory B2C
* Azure Cosmos DB
* Azure Maps
* Azure Relay
* Az Azure Stack - regisztrációk
* Azure Migrate
* Batch
* BizTalk Services
* Robotszolgáltatás
* Tartalomkézbesítési hálózat (CDN)
* Cloud Services – lásd: [klasszikus üzembe helyezési korlátozásoknak](#classic-deployment-limitations)
* Cognitive Services
* Container Registry
* Tartalommoderátor
* Data Catalog
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Event Grid
* Event Hubs
* Tekintse meg a HDInsight-fürtök – [HDInsight korlátozások](#hdinsight-limitations)
* IoT Hubok
* Key Vault
* Terheléselosztók – lásd: [Load Balancer-korlátozások](#lb-limitations)
* Log Analytics
* Logic Apps
* Machine Learning - webszolgáltatások helyezheti át egy erőforráscsoport ugyanabban az előfizetésben, de nem egy másik előfizetésben található Machine Learning Studióban. Egyéb Machine Learning-erőforrások áthelyezhetők, előfizetések között.
* Media Services
* Mobile Engagement
* Notification Hubs
* Operational Insights
* Operations Management
* Portál irányítópultok
* A Power BI - mind a Power BI Embedded és a Power BI-munkaterület-csoport
* Nyilvános IP-Címek – lásd: [nyilvános IP-korlátozások](#pip-limitations)
* Redis Cache
* Scheduler
* Keresés
* Service Bus
* Service Fabric
* SignalR Service
* Storage
* Tekintse meg a tároló (klasszikus) – [klasszikus üzembe helyezési korlátozásoknak](#classic-deployment-limitations)
* Stream Analytics - feladatok nem lehet áthelyezni, ha a futó Stream Analytics állapot.
* Az SQL Database server - adatbázis és a kiszolgáló ugyanabban az erőforráscsoportban kell lennie. Ha áthelyezi SQL-kiszolgáló, az összes hozzá tartozó adatbázisok is kerülnek. Ez a viselkedés az Azure SQL Database és az Azure SQL Data Warehouse-adatbázisok vonatkozik.
* Time Series Insights
* Traffic Manager
* Virtuális gépek – a felügyelt lemezekkel rendelkező virtuális gépeket nem lehet áthelyezni. Lásd: [virtuális gépek korlátozások](#virtual-machines-limitations)
* Tekintse meg a virtuális gépek (klasszikus) – [klasszikus üzembe helyezési korlátozásoknak](#classic-deployment-limitations)
* Tekintse meg a Virtual Machine Scale Sets – [virtuális gépek korlátozások](#virtual-machines-limitations)
* Tekintse meg a virtuális hálózatok - [virtuális hálózatok korlátozások](#virtual-networks-limitations)
* A Visual Studio Team Services - a VSTS-fiókok nem a Microsofttól kiterjesztésű vásárol kell [megszakítja a vásárlások](https://go.microsoft.com/fwlink/?linkid=871160) előtt azok is a fiók áthelyezése előfizetések között.
* VPN Gateway

## <a name="services-that-cannot-be-moved"></a>Szolgáltatások, amelyek nem lehet áthelyezni

A szolgáltatások, amelyek jelenleg nem engedélyezi az erőforrások áthelyezése a következők:

* AD Domain Services
* Hibrid AD Állapotfigyelő szolgáltatás
* Application Gateway
* Azure Database for MySQL
* Azure Database for PostgreSQL
* Az Azure adatbázis-Migrálás
* Azure Databricks
* Batch AI
* Tanúsítványok – App Service-tanúsítványok is áthelyezhetők, de a feltöltött tanúsítványok [korlátozások](#app-service-limitations).
* Container Service
* Dynamics LCS
* Express Route
* Kubernetes-szolgáltatást
* A Lab Services – áthelyezése új erőforráscsoportba ugyanahhoz az előfizetéshez engedélyezve van, de az előfizetés közötti áthelyezése nem engedélyezett.
* Terheléselosztók – lásd: [Load Balancer-korlátozások](#lb-limitations)
* Felügyelt alkalmazások
* Tekintse meg a Managed Disks - [virtuális gépek korlátozások](#virtual-machines-limitations)
* Microsoft Genomics
* Nyilvános IP-Címek – lásd: [nyilvános IP-korlátozások](#pip-limitations)
* Recovery Services-tároló – még nincs társítva a Recovery Services-tároló a számítási, hálózati és tárolási erőforrások áthelyezése, lásd: [Recovery Servicesre vonatkozó korlátozásokat](#recovery-services-limitations).
* Azure-beli SAP HANA-szolgáltatás
* Biztonság
* Site Recovery
* StorSimple-Eszközkezelő
* Tekintse meg a virtuális hálózatok (klasszikus) – [klasszikus üzembe helyezési korlátozásoknak](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Virtuális gépek korlátozások

Felügyelt lemezek nem támogatják az áthelyezési. Ez a korlátozás, az azt jelenti, hogy néhány kapcsolódó erőforrások túl nem lehet áthelyezni. Nem helyezhető át:

* Felügyelt lemezek
* A felügyelt lemezekkel rendelkező virtuális gépek
* A felügyelt lemezekről létrehozott rendszerképek
* A pillanatképek a felügyelt lemezek létrehozása
* A felügyelt lemezekkel rendelkező virtuális gépek rendelkezésre állási csoportok

Bár a felügyelt lemez nem helyezhető át, hozzon létre egy másolatot, és ezután hozzon létre egy új virtuális gépet a meglévő felügyelt lemezről. További információkért lásd:

* Felügyelt lemezek másolása a ugyanahhoz az előfizetéshez vagy a másik előfizetésben [PowerShell](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md) vagy [Azure CLI-vel](../virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md)
* A meglévő felügyelt operációsrendszer-lemezt használó virtuális gép létrehozása [PowerShell](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md) vagy [Azure CLI-vel](../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md).

A csatolt tervek Piactéri erőforrások alapján létrehozott virtuális gépeken nem lehet áthelyezni, erőforráscsoport vagy előfizetés között. Az aktuális előfizetésben a virtuális gép megszüntetése, és telepítse újra az új előfizetés.

Egy új erőforráscsoport ugyanabban az előfizetésben, de az előfizetések között nem helyezheti át virtuális gépeket a Key Vault-tanúsítvánnyal.

## <a name="virtual-networks-limitations"></a>Virtuális hálózatok korlátozások

Virtuális hálózat áthelyezésekor is át kell helyeznie a tőle függő erőforrások. Ha például helyezze át a virtuális hálózati átjárók.

Egy virtuális Társhálózat áthelyezni, előbb le kell tiltania a virtuális hálózatok közötti társviszony. Ha le van tiltva, áthelyezheti a virtuális hálózat. Az áthelyezés után a virtuális hálózatok közötti társviszony újraengedélyezni.

Virtuális hálózat nem helyezhetők át másik előfizetésbe való, ha a virtuális hálózati erőforrás-navigációs hivatkozásaival egy alhálózatot tartalmaz. Például ha egy Redis Cache-erőforrást egy alhálózatában van üzembe helyezve, az alhálózatra, erőforrás-navigációs hivatkozást.

Virtuális hálózat nem helyezhetők át másik előfizetésbe való, ha a virtuális hálózat egy egyéni DNS-kiszolgálót tartalmaz. Szeretné áthelyezni a virtuális hálózat, állítsa be alapértelmezett (Azure által biztosított) DNS-kiszolgálóra. Az áthelyezés után konfigurálja újra az egyéni DNS-kiszolgáló.

## <a name="app-service-limitations"></a>Az App Service korlátai

Az App Service-erőforrások áthelyezésére korlátozások eltérő Ha áthelyez egy előfizetésen belül vagy egy új előfizetést az erőforrások alapján.

Feltöltött tanúsítványok, a nem App Service-tanúsítványok az ezekben a szakaszokban ismertetett korlátozások vonatkoznak. Továbbléphet az App Service-tanúsítványok új erőforráscsoportba vagy előfizetésbe korlátozások nélkül. Ha több web apps, az azonos App Service-tanúsítványt használja, először helyezze át a web apps helyezze át a tanúsítványt.

### <a name="moving-within-the-same-subscription"></a>Az egy előfizetésen belüli áthelyezése

Ha a webalkalmazás áthelyezése _ugyanazon az előfizetésen belül_, a feltöltött SSL-tanúsítványok nem helyezhetők át. Azonban áthelyezheti egy webalkalmazást az új erőforráscsoport áthelyezése a feltöltött SSL-tanúsítvány nélkül, és az alkalmazás SSL funkció továbbra is működik.

Ha szeretné helyezni az SSL-tanúsítványt a webalkalmazással, kövesse az alábbi lépéseket:

1.  Törölje a feltöltött tanúsítvány megjelenjen a webalkalmazásból.
2.  Helyezze át a webes alkalmazás.
3.  Töltse fel a tanúsítványt a áthelyezett webalkalmazáshoz.

### <a name="moving-across-subscriptions"></a>Áthelyezése előfizetések között

Ha a webalkalmazás áthelyezése _előfizetésekben_, az alábbi korlátozások érvényesek:

- A cél erőforráscsoport nem kell rendelkeznie minden olyan meglévő App Service-erőforrásokat. App Service-erőforrások a következők:
    - Web Apps
    - App Service-csomagok
    - Feltöltött vagy importált SSL-tanúsítványok
    - App Service-környezetek
- Az erőforráscsoportban lévő összes App Service-erőforrások együtt lehessen áthelyezni.
- App Service-erőforrások csak akkor helyezhető el az erőforráscsoportból, amelyben eredetileg létrehozták őket. Ha egy App Service erőforrás már nem az eredeti erőforráscsoportba, azt kell vissza kell helyezni az eredeti erőforráscsoport először, és ezután áthelyezhető előfizetések között.

## <a name="classic-deployment-limitations"></a>Klasszikus üzembe helyezési korlátozásoknak

A beállításokat a klasszikus modellben telepített erőforrások áthelyezéséhez eltérőek Ha áthelyez egy előfizetésen belül vagy egy új előfizetést az erőforrások alapján.

### <a name="same-subscription"></a>Ugyanabban az előfizetésben

Erőforrások erőforráscsoportok közötti áthelyezése ugyanazon az előfizetésen belül egy másik erőforráscsoportba, amikor a következő korlátozások vonatkoznak:

* Nem lehet áthelyezni a virtuális hálózatok (klasszikus).
* A felhőszolgáltatás virtuális gépek (klasszikus) kell áthelyezni.
* A felhőalapú szolgáltatás csak akkor helyezhető, ha az áthelyezés tartalmazza az összes virtuális gép.
* Csak egy felhőalapú szolgáltatás egyszerre áthelyezhető.
* Csak egy tárfiók (klasszikus) egyszerre áthelyezhető.
* A tárfiók (klasszikus) nem lehet áthelyezni a virtuális gép vagy felhőszolgáltatás műveletben.

Klasszikus erőforrások áthelyezése ugyanazon az előfizetésen belül egy új erőforráscsoportot, a standard szintű áthelyezési műveleteket keresztül használja a [portál](#use-portal), [Azure PowerShell-lel](#use-powershell), [Azure CLI-vel](#use-azure-cli), vagy [REST API-val](#use-rest-api). Resource Manager-erőforrások áthelyezése, ahogy használhatja ugyanazokat a műveleteket.

### <a name="new-subscription"></a>Új előfizetés

Ha az erőforrások áthelyezése új előfizetésre, a következő korlátozások vonatkoznak:

* Az előfizetés összes klasszikus erőforrást műveletben kell áthelyezni.
* A célként megadott előfizetés nem tartalmazhat bármilyen egyéb klasszikus erőforrások.
* Az áthelyezés csak igényelni lehet klasszikus áthelyezését a külön REST API-n keresztül. A standard szintű Resource Manager áthelyezés parancsok nem működnek, ha a klasszikus erőforrások áthelyezése új előfizetésre.

Klasszikus erőforrások áthelyezése új előfizetést, használja a REST-műveletek, konkrétan a klasszikus erőforrások. A REST használata, hajtsa végre az alábbi lépéseket:

1. Ellenőrizze, hogy ha a forrás-előfizetés részt vehetnek-e egy előfizetések közötti áthelyezés. Használja a következő műveletet:

  ```HTTP
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     A kérelem törzsében a következők:

  ```json
  {
    "role": "source"
  }
  ```

     A választ az érvényesítés művelet a következő formátumban kell megadni:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Ellenőrizze, hogy ha a cél előfizetést részt vehetnek-e egy előfizetések közötti áthelyezés. Használja a következő műveletet:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     A kérelem törzsében a következők:

  ```json
  {
    "role": "target"
  }
  ```

     A válasz a forrás-előfizetés érvényesítése ugyanebben a formátumban van.
3. Ha mindkét előfizetés érvényesítési sikeresek, minden hagyományos erőforrás áthelyezése egy előfizetésből egy másik előfizetéshez a következő műveletet:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    A kérelem törzsében a következők:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

A művelet több percig futtathatnak.

## <a name="recovery-services-limitations"></a>Recovery Services-korlátozások

Az Azure Site Recovery vész-helyreállítási beállításához használt tárolási, hálózati és számítási erőforrások áthelyezése nincs engedélyezve.

Például tegyük fel, hogy beállította-e a tárfiókhoz (Storage1) a helyszíni gépek replikálását, és szeretné a védett gép merülnek fel a feladatátvételt követően az Azure-ba (Network1) virtuális hálózathoz csatolt virtuális gépként (VM1 –). Ön nem helyezhetők át a következő Azure erőforrások - Storage1 VM1 és Network1 - ugyanazon az előfizetésen belüli erőforráscsoportok között vagy előfizetések között.

A regisztrált virtuális gép áthelyezése a **az Azure backup** erőforráscsoportok között:
 1. Ideiglenesen állítsa le a biztonsági mentés és a biztonsági másolatok adatainak megőrzése
 2. A virtuális gép áthelyezése a céloldali erőforráscsoport
 3. Állítsa a felhasználók visszaállíthatják a az áthelyezési művelet előtt létrehozott rendelkezésre álló helyreállítási pontokból azonos vagy új tároló alatt.
Ha a felhasználó a biztonsági másolatban szereplő virtuális gép előfizetések között, az 1 és 2. lépés ugyanaz marad. A 3. lépésben a felhasználónak kell csoportban található / a célul szolgáló előfizetésben létrehozott egy új tárolót a virtuális gép védelmét. Recovery Services-tároló közötti előfizetés biztonsági mentések nem támogatja.

## <a name="hdinsight-limitations"></a>HDInsight-korlátozások

HDInsight-fürtök áthelyezheti egy új előfizetést, vagy az erőforráscsoportot. Azonban nem helyezhetők át a hálózati erőforrások (például a virtuális hálózathoz, a hálózati adapter vagy a terheléselosztó) a HDInsight-fürthöz társított előfizetésekben. Emellett nem helyezhető át egy új erőforráscsoportot egy hálózati Adaptert, amely a fürt egy virtuális géphez van csatolva.

Amikor új előfizetésbe való áthelyezését egy HDInsight-fürtöt, először helyezze át más erőforrások (például a storage-fiók). Ezután helyezze át a HDInsight-fürt önmagában.

## <a name="search-limitations"></a>Keresés korlátozások

Egyszerre helyezni különböző régiókban lévő több keresési erőforrások nem helyezhetők át.
Ebben az esetben kell külön áthelyezni őket.

## <a name="lb-limitations"></a> Load Balancer korlátozások

Alapszintű Termékváltozatú terheléselosztó helyezhetők.
Standard Termékváltozatú terheléselosztó nem lehet áthelyezni.

## <a name="pip-limitations"></a> Nyilvános IP-korlátozások

Alapszintű Termékváltozat nyilvános IP-cím is áthelyezhetők.
Standard Termékváltozat nyilvános IP-cím nem lehet áthelyezni.

## <a name="use-portal"></a>A portál használatával

Erőforrások áthelyezése, válassza ki ezeket az erőforrásokat tartalmazó erőforráscsoportot, és válassza a **áthelyezése** gombra.

![erőforrások áthelyezése](./media/resource-group-move-resources/select-move.png)

Válassza ki, hogy az erőforrások telepít át egy új erőforráscsoportot, vagy egy új előfizetést.

Válassza ki az áthelyezni kívánt erőforrások és a cél erőforráscsoport. Tudomásul veszi, hogy szeretne-e frissíteni a következő ezeket az erőforrásokat a szkripteket, és válassza ki **OK**. Ha az előfizetés szerkesztési ikonra az előző lépésben, a cél előfizetést is választania kell.

![cél kiválasztása](./media/resource-group-move-resources/select-destination.png)

A **értesítések**, láthatja, hogy fut-e az áthelyezési művelet.

![Áthelyezés állapot megjelenítése](./media/resource-group-move-resources/show-status.png)

Ha befejeződött, értesítést kap arról, az eredmény.

![Áthelyezés eredmény megjelenítése](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>A PowerShell használata

Meglévő erőforrások áthelyezése egy másik erőforráscsoportba vagy előfizetésbe, használja a [Move-AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) parancsot. Az alábbi példa bemutatja, hogyan több erőforrást áthelyezése egy új erőforráscsoportot.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Át egy új előfizetést, adjon meg egy értéket a `DestinationSubscriptionId` paraméter.

## <a name="use-azure-cli"></a>Az Azure parancssori felület használatával

Meglévő erőforrások áthelyezése egy másik erőforráscsoportba vagy előfizetésbe, használja a [az erőforrás-áthelyezési](/cli/azure/resource?view=azure-cli-latest#az-resource-move) parancsot. Adja meg az erőforrás-azonosítókat az erőforrások áthelyezése. Az alábbi példa bemutatja, hogyan több erőforrást áthelyezése egy új erőforráscsoportot. Az a `--ids` paramétert, adja meg az erőforrás-azonosítók áthelyezése szóközzel elválasztott listáját.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Szeretne áthelyezni egy új előfizetést, adja meg a `--destination-subscription-id` paraméter.

## <a name="use-rest-api"></a>A REST API használata

Meglévő erőforrások áthelyezése egy másik erőforráscsoportba vagy előfizetésbe, futtassa:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

A kérelem törzsében szereplő adja meg a céloldali erőforráscsoport és erőforrások áthelyezéséhez. Az áthelyezési REST művelet kapcsolatos további információkért lásd: [erőforrások áthelyezése](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>További lépések

* Az előfizetés kezelése a PowerShell-parancsmagokkal kapcsolatos további információkért lásd: [Azure PowerShell használatával a Resource Managerrel](powershell-azure-resource-manager.md).
* Az előfizetés kezelése az Azure CLI-parancsokkal kapcsolatos további információkért lásd: [az Azure CLI használatával a Resource Manager](xplat-cli-azure-resource-manager.md).
* Az előfizetés kezelésére szolgáló portál funkciókkal kapcsolatos tudnivalókért lásd: [erőforrások kezelése az Azure portal használatával](resource-group-portal.md).
* Az erőforrások logikus alkalmazásával kapcsolatos tudnivalókért lásd: [az erőforrások rendszerezése címkék használatával](resource-group-using-tags.md).
