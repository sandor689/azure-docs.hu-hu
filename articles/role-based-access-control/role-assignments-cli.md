---
title: Rbac-RÓL és az Azure CLI-hozzáférés kezelése |} A Microsoft Docs
description: Ismerje meg, hogyan kezelheti a felhasználók, csoportok és alkalmazások, a szerepköralapú hozzáférés-vezérlés (RBAC) és az Azure CLI használatával hozzáférését. Ez magában foglalja a listázási hozzáférés, hozzáférést biztosít, és távolítsa el a hozzáférést.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 6d1e64c7630f3fd35124e6671476174ddfc16bb6
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437099"
---
# <a name="manage-access-using-rbac-and-azure-cli"></a>Rbac-RÓL és az Azure CLI-hozzáférés kezelése

A [szerepköralapú hozzáférés-vezérlés (RBAC)](overview.md) az erőforrásokhoz való hozzáférés kezelésének a módja az Azure-ban. Ez a cikk bemutatja, hogyan kezelheti a felhasználók, csoportok és alkalmazások RBAC és az Azure CLI használatával hozzáférését.

## <a name="prerequisites"></a>Előfeltételek

Hozzáférés kezelése a következők szükségesek:

* [Az Azure Cloud Shell bash](/azure/cloud-shell/overview)
* [Azure CLI](/cli/azure)

## <a name="list-roles"></a>Szerepkörök felsorolása

Az összes rendelkezésre álló szerepkör-definíciók listájában, használja a [az role definition listájában](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list
```

Az alábbi példa felsorolja a nevét és leírását, az összes rendelkezésre álló szerepkör-definíciók:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

Az alábbi példa felsorolja a beépített szerepkör-definíciók mindegyikét:

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role"></a>A szerepkörök listája műveletek

A műveletek egy szerepkör-definíció listázásához használja [az role definition listájában](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list --name <role_name>
```

A következő példa listákat a *közreműködői* szerepkör-definíció:

```azurecli
az role definition list --name "Contributor"
```

```Output
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

A következő példa listákat a *műveletek* és *notActions* , a *közreműködői* szerepkör:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

Az alábbi példa felsorolja a műveleteket a *virtuális gépek Közreműködője* szerepkör:

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="list-access"></a>Hozzáférések felsorolása

Az RBAC lista hozzáférés listázása a szerepkör-hozzárendeléseket.

### <a name="list-role-assignments-for-a-user"></a>Felhasználó szerepkör-hozzárendeléseinek felsorolása

Egy adott felhasználó szerepkör-hozzárendelések listájában, használja a [az szerepkör-hozzárendelés lista](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --assignee <assignee>
```

Alapértelmezés szerint csak a hatókörön belüli előfizetéshez hozzárendelések jelenik meg. Erőforrás vagy egy csoport hatóköre hozzárendelések megtekintéséhez használja `--all`.

Az alábbi példa felsorolja a közvetlenül hozzárendelt szerepkör-hozzárendeléseket a *patlong@contoso.com* felhasználói:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>Erőforráscsoport szerepkör-hozzárendeléseinek felsorolása

A szerepkör-hozzárendeléseket létező erőforráscsoport listájában, használja a [az szerepkör-hozzárendelés lista](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --resource-group <resource_group>
```

Az alábbi példa felsorolja a szerepkör-hozzárendeléseit a *pharma – értékesítés-projectforecast* erőforráscsoportot:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}

...
```

## <a name="grant-access"></a>Hozzáférés biztosítása

Az RBAC-ben a hozzáférés biztosítása egy szerepkör-hozzárendelés létrehozásával történik.

### <a name="create-a-role-assignment-for-a-user"></a>Egy felhasználó szerepkör-hozzárendelés létrehozása

Az erőforrás-csoport hatókörben egy felhasználó szerepkör-hozzárendelés létrehozásához használja [az szerepkör-hozzárendelés létrehozása](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

Az alábbi példa a *virtuális gépek Közreműködője* szerepkör *patlong@contoso.com* felhasználója a *pharma – értékesítés-projectforecast* erőforrás csoport hatóköre:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="create-a-role-assignment-for-a-group"></a>Szerepkör-hozzárendelés egy csoport létrehozása

Szerepkör-hozzárendelés csoport létrehozásához használja [az szerepkör-hozzárendelés létrehozása](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Az alábbi példa a *olvasó* szerepkört a *Reino Mack csapat* azonosító 22222222-2222-2222-2222-222222222222 egy előfizetésre a csoporthoz. A csoport Azonosítójának lekéréséhez használhatja [az ad-csoportok listája](/cli/azure/ad/group#az-ad-group-list) vagy [az ad-csoport megjelenítése](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

Az alábbi példa a *virtuális gépek Közreműködője* szerepkört a *Reino Mack csapat* azonosító 22222222-2222-2222-2222-222222222222 nevűvirtuálishálózatierőforráshatókörreacsoport*pharma-értékesítés-projekt – hálózati*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="create-a-role-assignment-for-an-application"></a>Alkalmazás szerepkör-hozzárendelés létrehozása

Az alkalmazás-szerepkör létrehozásához használja a [az szerepkör-hozzárendelés létrehozása](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Az alábbi példa a *virtuális gépek Közreműködője* alkalmazáshoz való hozzárendelésként az objektum azonosítója 44444444-4444-4444-4444-444444444444 a szerepkör a *pharma – értékesítés-projectforecast* erőforráscsoport a hatókör. Az alkalmazás objektum Azonosítójának lekéréséhez használhatja [az ad app list](/cli/azure/ad/app#az-ad-app-list) vagy [az ad app show](/cli/azure/ad/app#az-ad-app-show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Hozzáférés eltávolítása

RBAC, a hozzáférést, akkor egy szerepkör-hozzárendelés eltávolítása használatával [az szerepkör-hozzárendelés törlése](/cli/azure/role/assignment#az-role-assignment-delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

A következő példában eltávolítjuk a *virtuális gépek Közreműködője* a szerepkör-hozzárendelés a *patlong@contoso.com* felhasználója a *pharma – értékesítés-projectforecast* erőforrás csoport:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

A következő példa eltávolítja a *olvasó* a szerepkör a *Reino Mack csapat* azonosító 22222222-2222-2222-2222-222222222222 egy előfizetésre a csoporthoz. A csoport Azonosítójának lekéréséhez használhatja [az ad-csoportok listája](/cli/azure/ad/group#az-ad-group-list) vagy [az ad-csoport megjelenítése](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>További lépések

- [Oktatóanyag: Azure CLI-vel egyéni szerepkör létrehozása](tutorial-custom-role-cli.md)
- [Azure-erőforrások és -erőforráscsoportok kezelése az Azure CLI használatával](../azure-resource-manager/xplat-cli-azure-resource-manager.md)