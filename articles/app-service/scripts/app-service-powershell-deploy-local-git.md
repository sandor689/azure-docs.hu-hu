---
title: Azure PowerShell-példaszkript – Webalkalmazás létrehozása és kód üzembe helyezése helyi Git-adattárból | Microsoft Docs
description: Azure PowerShell-példaszkript – Webalkalmazás létrehozása és kód üzembe helyezése helyi Git-adattárból
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: cdd691d156d182c3d85dcc731ddf1bc62204b072
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324154"
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a>Webalkalmazás létrehozása és kód üzembe helyezése helyi Git-adattárból

Ez a példaszkript egy webalkalmazást hoz létre az App Service-ben a kapcsolódó erőforrásokkal együtt, majd üzembe helyezi a webalkalmazás kódját egy helyi Git-adattárból.

Szükség esetén frissítsen az Azure PowerShell legújabb verziójára az [Azure PowerShell útmutatójának](/powershell/azure/overview) utasításait követve, majd a `Connect-AzureRmAccount` futtatásával hozza létre a kapcsolatot az Azure-ral. Ezenkívül az alkalmazáskódot véglegesíteni kell egy helyi Git-adattárban.

## <a name="sample-script"></a>Példaszkript

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

A példaszkript futtatása után a következő paranccsal távolítható el az erőforráscsoport, a webalkalmazás és az összes kapcsolódó erőforrás.

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a>Szkript ismertetése

A szkript a következő parancsokat használja. A táblázatban lévő összes parancs a hozzá tartozó dokumentációra hivatkozik.

| Parancs | Megjegyzések |
|---|---|
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Létrehoz egy webalkalmazást a szükséges erőforráscsoporttal és App Service-csoporttal. Ha az aktuális könyvtár tartalmaz egy Git-adattárat, egy távoli `azure`-mappát is adjon hozzá. |

## <a name="next-steps"></a>További lépések

Az Azure PowerShell modullal kapcsolatos további információért lásd az [Azure PowerShell dokumentációját](/powershell/azure/overview).

További Azure Powershell-példákat az Azure App Service Web Appshez az [Azure PowerShell-példák](../app-service-powershell-samples.md) között találhat.
