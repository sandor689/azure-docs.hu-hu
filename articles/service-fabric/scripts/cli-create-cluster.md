---
title: Azure CLI telepítési példaszkriptje
description: Biztonságos Service Fabric-fürt létrehozása az Azure-ban az Azure CLI használatával.
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 79125beac6ab34b88c951a2fb9c601b93ae36e20
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642206"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Biztonságos Service Fabric-fürt létrehozása az Azure-ban

Ez a parancs létrehoz egy önaláírt tanúsítványt, hozzáadja egy kulcstartóhoz, és helyileg letölti a tanúsítványt.  A rendszer az új tanúsítványt a fürt védelmére használja a telepítést követően.  Meglévő tanúsítványt is használhat egy új létrehozása helyett.  A tanúsítvány tulajdonosnevének mindkét esetben egyeznie kell a Service Fabric-fürthöz való hozzáféréshez használt tartománnyal. Erre a megfeleltetésre azért van szükség, hogy a rendszer SSL-kapcsolatot biztosíthasson a fürt HTTPS-felügyeleti végpontjai és a Service Fabric Explorer számára. Nem szerezhető be SSL-tanúsítvány hitelesítésszolgáltatótól (CA) a `.cloudapp.azure.com` tartomány számára. Egyéni tartománynevet kell beszereznie a fürt számára. Amikor tanúsítványt igényel egy hitelesítésszolgáltatótól, a tanúsítvány tulajdonosnevének meg kell felelnie a fürthöz használt egyéni tartománynévnek.

Amennyiben szükséges, telepítse az [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) felületet.

## <a name="sample-script"></a>Példaszkript

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A példaszkript futtatása után a következő paranccsal távolítható el az erőforráscsoport, a fürt és az összes kapcsolódó erőforrás.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Szkript ismertetése

A szkript a következő parancsokat használja. A táblázatban lévő összes parancs a hozzá tartozó dokumentációra hivatkozik.

| Parancs | Megjegyzések |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Létrehoz egy új Service Fabric-fürtöt.  |

## <a name="next-steps"></a>További lépések

A [Service Fabric parancssori felület példái](../samples-cli.md) között további Service Fabric parancssori felületi példákat talál az Azure Service Fabrichez.
