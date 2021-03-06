---
title: Az Azure VirtualNetworkCombo felhasználói felületi elem |} Microsoft Docs
description: Azure-portálon a Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem ismerteti.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 2c2553d9ffb1dfbe032385fb77e234a8b96cb239
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110065"
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem
Egy új vagy meglévő virtuális hálózat kiválasztásához vezérlők egy csoportja.

## <a name="ui-sample"></a>Felhasználói felület minta
Amikor a felhasználó egy új virtuális hálózat választja, a felhasználó minden alhálózat nevét és a címelőtag szabhatja testre. Alhálózatok konfigurálása nem kötelező megadni.

![Új Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-new.png)

Amikor a felhasználó egy meglévő virtuális hálózatot választja, a felhasználó a központi telepítési sablont igényel minden egyes alhálózatot kell társítani egy létező alhálózatot. Alhálózatok konfigurálása ebben az esetben szükség.

![Meglévő Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-existing.png)

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- Ha meg van adva, az első mozaikként, átfedés nélkül cím előtag méretű `defaultValue.addressPrefixSize` a felhasználó az előfizetéshez létező virtuális hálózatok automatikusan alapján történik.
- Az alapértelmezett érték `defaultValue.name` és `defaultValue.addressPrefixSize` van **null**.
- `constraints.minAddressPrefixSize` meg kell adni. Meglévő virtuális hálózatok egy kisebb, mint a megadott érték címtartománnyal rendelkező kijelölésnél érhetők el.
- `subnets` meg kell adni, és `constraints.minAddressPrefixSize` meg kell adni az egyes alhálózatokon.
- Amikor új virtuális hálózat létrehozása minden alhálózati cím előtag alapján van kiszámítva automatikusan a virtuális hálózat címelőtagjához és a megfelelő `addressPrefixSize`.
- Ha egy meglévő virtuális hálózat, kisebb, mint a megfelelő alhálózatok `constraints.minAddressPrefixSize` kijelölés nem érhetők el. Továbbá ha meg van adva, alhálózatok, amelyek nem rendelkeznek legalább `minAddressCount` elérhető címei nem lehet kiválasztani. Az alapértelmezett érték **0**. Győződjön meg arról, hogy a rendelkezésre álló címek összefüggő, adja meg a **igaz** a `requireContiguousAddresses`. Az alapértelmezett érték **igaz**.
- Alhálózatok létrehozása a meglévő virtuális hálózat nem támogatott.
- Ha `options.hideExisting` van **igaz**, a felhasználó egy meglévő virtuális hálózat nem választható. Az alapértelmezett érték **hamis**.

## <a name="sample-output"></a>Példa kimenet

```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>További lépések
* A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](create-uidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](create-uidefinition-elements.md).
