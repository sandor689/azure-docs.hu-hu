---
title: fájl belefoglalása
description: fájl belefoglalása
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 26bfeed5315394b9030bff9c471dd5fbedca117f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38732053"
---
[!INCLUDE [resource group intro text](resource-group.md)]

A Cloud Shellben hozzon létre egy erőforráscsoportot az [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) paranccsal. A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot a *West Europe* helyen. Az **Ingyenes** szintű App Service-t támogató összes hely megtekintéséhez futtassa az [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) parancsot.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Az erőforráscsoportot és az erőforrásokat általában a közelében található régiókban hozhatja létre. 

A parancs befejeződésekor a JSON-kimenet megjeleníti az erőforráscsoport tulajdonságait.