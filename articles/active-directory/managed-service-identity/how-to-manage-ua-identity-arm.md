---
title: Hogyan hozhat létre, és törli a felhasználó hozzárendelt Felügyeltszolgáltatás-identitás az Azure Resource Managerrel
description: Lépésről lépésre való létrehozása és törlése a felhasználó hozzárendelt Felügyeltszolgáltatás-identitás használatával az Azure-erőforrás.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 42d5b55e0bddf2d027810bfdf146de9bfee8a0fb
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188131"
---
# <a name="create-list-and-delete-a-user-assigned-identity-using-azure-resource-manager"></a>Lista létrehozása, és a egy felhasználóhoz hozzárendelt identitás, az Azure Resource Manager törlése

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Felügyeltszolgáltatás-identitás egy felügyelt identitás, az Azure Active Directory Azure-szolgáltatásokat biztosít. Ez az identitás használatával, amelyek támogatják az Azure AD-hitelesítés, anélkül, hogy hitelesítő adatok a kód a szolgáltatásokhoz való hitelesítéséhez. 

Ez a cikk egy Azure Resource Manager használatával felügyelt identitás hozzárendelt felhasználó hoz létre.

Nem alkalmas listázása és törlése egy a felhasználóhoz hozzárendelt identitás, az Azure Resource Manager-sablon használatával.  Hozhat létre, és a egy felhasználóhoz hozzárendelt identitás listája a következő cikkekben talál:

- [A lista felhasználóhoz hozzárendelt identitás](how-to-manage-ua-identity-cli.md#list-user-assigned-identities)
- [A felhasználóhoz hozzárendelt identitás törlése](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-identity)
## <a name="prerequisites"></a>Előfeltételek

- Ha még nem ismeri a Felügyeltszolgáltatás-identitást, tekintse meg a [áttekintés szakaszban](overview.md). **Ne feledje el áttekinteni a [különbség egy rendszer által hozzárendelt, és a felhasználóhoz hozzárendelt identitás](overview.md#how-does-it-work)**.
- Ha még nincs Azure-fiókja, a folytatás előtt [regisztráljon egy ingyenes fiókra](https://azure.microsoft.com/free/).
- Ebben a cikkben a műveletek végrehajtásához a fióknak rendelkeznie kell a következő szerepkör-hozzárendelés:
    - [Felügyelt identitások Közreműködője](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) szerepkör létrehozása, olvasása (lista), frissítése és törlése a felhasználóhoz hozzárendelt identitás.

## <a name="template-creation-and-editing"></a>Sablon létrehozása és szerkesztése

Ahogy az az Azure portal és a parancsfájlok, az Azure Resource Manager-sablonok lehetővé teszi, hogy üzembe helyezése az Azure-erőforráscsoport által meghatározott új vagy megváltozott erőforrásokat. Több lehetőség is elérhető, és a helyi és portálalapú, beleértve a központi telepítési sablon Szerkesztés:

- Használatával egy [egyéni sablont az Azure Marketplace-ről](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), amely lehetővé teszi, hogy a sablon létrehozása az alapoktól, vagy egy meglévő közös alapul vagy [gyorsindítási sablon](https://azure.microsoft.com/documentation/templates/).
- Sablon exportálása vagy egy meglévő erőforráscsoportot, a Származtatás [az eredeti üzembe helyezés](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), vagy a [az üzemelő példány aktuális állapotát](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
- Egy helyi [JSON-szerkesztővel (például a VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), majd feltöltését és üzembe helyezése a PowerShell vagy parancssori felület használatával.
- A Visual Studio használatával [Azure erőforráscsoport-projekt](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hozzon létre és helyezhet üzembe sablont is. 

## <a name="create-a-user-assigned-identity"></a>Felhasználóhoz rendelt identitás létrehozása 

A felhasználóhoz hozzárendelt identitás létrehozásához használja az alábbi sablont. Cserélje le a `<USER ASSIGNED IDENTITY NAME>` értéket a saját értékeire:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "resourceName": {
          "type": "string",
          "metadata": {
            "description": "<USER ASSIGNED IDENTITY NAME>"
          }
        }
  },
  "resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('resourceName')]",
      "apiVersion": "2015-08-31-PREVIEW",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('resourceName')]"
      }
  }
}
```
## <a name="related-content"></a>Kapcsolódó tartalom

Egy felhasználóhoz hozzárendelt identitás hozzárendelése egy Azure virtuális gép használatával egy Azure Resource Manager sablon lásd: információ [konfigurálása a virtuális gépek Felügyeltszolgáltatás-identitását egy sablon használatával](qs-configure-template-windows-vm.md).


 
