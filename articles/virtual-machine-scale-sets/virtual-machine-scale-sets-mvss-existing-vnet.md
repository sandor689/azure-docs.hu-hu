---
title: Egy már meglévő virtuális hálózatot az Azure méretezési készlet sablonban hivatkozhat |} Microsoft Docs
description: 'Útmutató: virtuális hálózat hozzáadása egy meglévő Azure virtuálisgép-méretezési csoport sablon'
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: negat
ms.openlocfilehash: eb35975de5864e129f97b614a61487456dd972ef
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/20/2017
ms.locfileid: "26782371"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Adjon hozzá egy meglévő virtuális hálózathoz való hivatkozást Azure méretezési készlet sablonban

Ez a cikk bemutatja, hogyan lehet módosítani a [minimális életképes méretezési sablon](./virtual-machine-scale-sets-mvss-start.md) való üzembe helyezés helyett egy új létrehozása meglévő virtuális hálózat.

## <a name="change-the-template-definition"></a>Módosítsa a sablon-definíciót

A minimális életképes méretezési sablon beállítása látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), és a sablon a méretezési készletben meglévő virtuális hálózatban való üzembe helyezésének látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). Ez a sablon létrehozásához használt különbözeti vizsgáljuk meg (`git diff minimum-viable-scale-set existing-vnet`) adat által adat:

Először adja hozzá a `subnetId` paraméter. Ez a karakterlánc lett átadva a méretezési készlet konfigurációja, amely lehetővé teszi a méretezés, a virtuális gépek telepítéséhez a korábban létrehozott alhálózati azonosító állíthat be. Ez a karakterlánc a következő formátumban kell lennie: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Például a skála telepítendő beállítani egy meglévő virtuális hálózat névvel `myvnet`, alhálózati `mysubnet`, erőforráscsoport `myrg`, és az előfizetés `00000000-0000-0000-0000-000000000000`, a subnetId lenne: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

Ezután törölje a virtuális hálózati erőforrást a `resources` tömbben, nem szükséges telepítenie egy új és meglévő virtuális hálózat használata.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

A virtuális hálózat már létezik a sablon telepítése előtt, nincs szükség a dependsOn záradékot a méretezési készletben, a virtuális hálózathoz meg. Törölje a következő sorokat:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Végül adjon át a `subnetId` a felhasználó által megadott paraméter (használata helyett `resourceId` ahhoz, hogy a virtuális hálózat Azonosítóját azonos környezetben, amely, mi a minimális életképes méretezési sablon does).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>További lépések

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
