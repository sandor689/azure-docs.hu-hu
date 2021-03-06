---
title: Hibaelhárítás az Azure virtuális hálózati átjáró és kapcsolatok – Azure CLI 2.0 használatával |} A Microsoft Docs
description: Jelen lap bemutatja, hogyan használható az Azure Network Watcher hibaelhárítása az Azure CLI 2.0 használatával
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 5f843b42a108968e2fbefacddcd22f331a04691e
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091101"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a>Végezzen hibaelhárítást a virtuális hálózati átjáró és kapcsolatok az Azure Network Watcher Azure CLI 2.0 használatával

> [!div class="op_single_selector"]
> - [Portál](diagnose-communication-problem-between-networks.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [Azure CLI](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

A Network Watcher számos funkciót kínál a ismertetése az Azure-ban a hálózati erőforrások vonatkozik. Ezek a képességek egyik erőforrás hibaelhárítás. Erőforrások hibaelhárítása a portal, PowerShell, CLI vagy REST API használatával is meghívható. Meghívni, Network Watcher megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat állapotát, és a csapatával az eredményeket adja vissza.

Ez a cikk használja felületek következő generációját képviseli CLI a resource management üzemi modellhez, az Azure CLI 2.0 használatával, amely Windows, Mac és Linux rendszereken érhető el.

Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv azt feltételezi, hogy már követte a lépéseket a [hozzon létre egy Network Watcher](network-watcher-create.md) egy Network Watcher létrehozásához.

Látogasson el a támogatott átjáró típusok, listáját [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Áttekintés

Erőforrás hibaelhárítás lehetővé teszi a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához. A hibaelhárítási erőforrás a kérelem elküldésekor folyamatban van-e a naplók kérdezhető le, és megvizsgálni. Ha az ellenőrzés befejeződött, a rendszer visszairányítja az eredményeket. Erőforrás-kérelmek hibaelhárítási sokáig futó kérelmek, amelyek több percig is eltarthat adja vissza az eredményt. Hibaelhárítás a naplók vannak tárolva egy tárolót a storage-fiók van megadva.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>A virtuális hálózati átjáró kapcsolat beolvasása

Ebben a példában erőforrás hibaelhárítás folyamatban futtatta a kapcsolatot. Is átadhatja azt a virtuális hálózati átjáró. A következő parancsmag listája a vpn-kapcsolatok egy erőforráscsoportban.

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

Miután a kapcsolat neve, futtathatja a következő paranccsal kérhet le az erőforrás-azonosító:

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a>Tárfiók létrehozása

Erőforrás hibaelhárítás adja vissza az erőforrás állapotára vonatkozó adatok, naplók vizsgálni a storage-fiókba is menti. Ebben a lépésben hozunk létre egy storage-fiókot, ha egy meglévő tárfiók létezik használhatja azt.

1. A tárfiók létrehozása

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. A storage-fiók kulcsainak lekérése

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. A tároló létrehozása

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Futtassa a Network Watcher erőforrás hibaelhárítás

Hibaelhárítás az erőforrásokat a `az network watcher troubleshooting` parancsmagot. Adjuk át a parancsmag az erőforráscsoport nevét a Network Watcher, a kapcsolat, a storage-fiók azonosítóját azonosítója, és a hibaelhárítás tárolni a blob elérési útja eredményez.

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

A parancsmag futtatása után a Network Watcher áttekinti az erőforrás állapotának ellenőrzése. Visszaadja az eredményeket a rendszerhéj és a megadott tárfiók az eredmények naplókat tárolja.

## <a name="understanding-the-results"></a>Az eredmények ismertetése

A művelet szöveg nyújt általános útmutatást a probléma megoldásához. Egy műveletet elvégezhet a problémára, ha egy hivatkozást a további útmutatást. Abban az esetben, ha nincsenek további útmutatást, a válasz biztosít támogatási eset nyitása URL-címét.  A válasz és foglalt tulajdonságaival kapcsolatos további információkért látogasson el a [Network Watcher hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)

Fájlok letöltése az azure storage-fiókokra vonatkozó utasításokért tekintse meg [.NET használatával az Azure Blob storage használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Egy másik eszköz használható a Storage Explorer. További információ a Storage Explorer itt található, a következő hivatkozásra: [Storage Explorerrel](http://storageexplorer.com/)

## <a name="next-steps"></a>További lépések

Ha-beállításai lettek módosítva lett, hogy állítsa le VPN-kapcsolat, tekintse meg [hálózati biztonsági csoportok kezelése](../virtual-network/manage-network-security-group.md) nyomon követheti a hálózati biztonsági csoport és biztonsági szabályok, amelyek az adott lehetnek.
