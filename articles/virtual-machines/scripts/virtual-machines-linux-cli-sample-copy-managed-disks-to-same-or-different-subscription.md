---
title: Azure CLI-példaszkript – Felügyelt lemezek másolása (mozgatása) előfizetésen belül vagy előfizetések között | Microsoft Docs
description: Azure CLI-példaszkript – Felügyelt lemezek másolása (mozgatása) előfizetésen belül vagy előfizetések között
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: dfdbc0563810447a1a214356b5153afe38d9cf2f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/09/2018
ms.locfileid: "29846791"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Felügyelt lemezek másolása előfizetésen belül vagy előfizetések között a CLI használatával

Ez a szkript átmásol egy felügyelt lemezt egy előfizetésen belül vagy egy eltérő, de ugyanabban a régióban található előfizetésbe. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Példaszkript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Szkript ismertetése

A szkript a következő parancsokat használja egy új felügyelt lemez létrehozásához a célul szolgáló előfizetésben a forrásul szolgáló felügyelt lemez azonosítója alapján. A táblázatban lévő összes parancs a hozzá tartozó dokumentációra hivatkozik.

| Parancs | Megjegyzések |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Lekérdezi egy felügyelt lemez összes tulajdonságát a lemez neve és erőforráscsoport-tulajdonságai alapján. Az „Id” tulajdonság a felügyelt lemez másik előfizetésbe való másolásakor használatos.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Lemásol egy felügyelt lemezt úgy, hogy a szülő felügyelt lemez azonosítójának és nevének használatával egy másik előfizetésben hoz létre egy felügyelt lemezt.  |

## <a name="next-steps"></a>További lépések

[Virtuális gép létrehozása felügyelt lemezből](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Az Azure CLI-vel kapcsolatos további információért lásd az [Azure CLI dokumentációját](https://docs.microsoft.com/cli/azure).

A virtuális gépekhez és felügyelt lemezekhez kapcsolódó további CLI-példaszkripteket az [Azure Linux rendszerű virtuális gépekre vonatkozó dokumentációjában](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) találhat.
