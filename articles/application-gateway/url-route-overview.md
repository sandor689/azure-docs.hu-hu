---
title: Azure Application Gateway URL-alapú tartalom-útválasztás áttekintése
description: Ez a cikk áttekintés nyújt az Application Gateway URL-alapú tartalom-útválasztási lehetőségeiről, az UrlPathMap-konfigurációról és a PathBasedRouting szabályról.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/23/2018
ms.author: victorh
ms.openlocfilehash: f706e267675dbb3d5465977157f40824efc96e48
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389242"
---
# <a name="azure-application-gatewaty-url-path-based-routing-overview"></a>Azure Application Gateway URL-alapú tartalom-útválasztás

Az URL-alapú útválasztás lehetővé teszi, hogy a kérésben szereplő URL-cím alapján irányítsa a forgalmat a háttér-kiszolgálókészlethez. 

Az egyik lehetőség az, hogy a különböző típusú tartalmakra vonatkozó kéréseket a megfelelő háttér-kiszolgálókészlethez irányítja.

Az alábbi példában az alkalmazásátjáró a contoso.com webhelyet szolgálja ki a VideoServerPool, az ImageServerPool és a DefaultServerPool háttér-kiszolgálókészlettel.

![imageURLroute](./media/url-route-overview/figure1.png)

A http://contoso.com/video/* URL-hez kapcsolódó kérések a VideoServerPool, a http://contoso.com/images/* URL-hez kapcsolódóak pedig az ImageServerPool készlethez lesznek átirányítva. Ha a kérés egyik elérésiút-kategóriába sem sorolható, a DefaultServerPool az alapértelmezett kiszolgáló.

> [!IMPORTANT]
> A szabályok abban a sorrendben vannak feldolgozva, amelyben a portálon szerepelnek. Alapszintű figyelő konfigurálása előtt határozottan ajánlott többhelyes figyelőket konfigurálni.  Ez biztosítja, hogy a forgalom a megfelelő háttérbe legyen irányítva. Ha előbb egy alapszintű figyelő szerepel a listában, és az megfelel egy bejövő kérésnek, a figyelő feldolgozza azt.

## <a name="urlpathmap-configuration-element"></a>Az UrlPathMap konfigurációs elem

Az UrlPathMap elem elérésiút-minták meghatározására szolgál a háttér-kiszolgálókészletek leképezésében. Az alábbi kódpélda a sablonfájlból származó urlPathMap elem kódrészlete.

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

> [!NOTE]
> PathPattern: ez a beállítás tartalmazza az elérésiút-minták listáját. Minden mintának a / jellel kell kezdődnie, a „*” jel pedig kizárólag a mintavégi „/” jel után állhat. Az elérésiút-megfeleltetőben megadott sztring nem tartalmaz szöveget az első ? vagy # után, és ezek a karakterek itt nem megengedettek.

További információért tekintse át az [URL-alapú átirányításhoz készült Resource Manager-sablonokat](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing).

## <a name="pathbasedrouting-rule"></a>PathBasedRouting szabály

A PathBasedRouting típusú RequestRoutingRule szabály arra szolgál, hogy egy figyelőt az adott urlPathMap elemhez kössön. Minden kérés, amely ehhez a figyelőhöz kapcsolódik, az urlPathMap elemben meghatározott irányelvek alapján lesz átirányítva.
A PathBasedRouting szabály kódrészlete:

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a>További lépések

Miután elsajátította az URL-alapú tartalom-átirányításról szóló ismereteket, látogasson el [az URL-alapú átirányítást használó alkalmazásátjáró létrehozását bemutató témakörhöz](tutorial-url-route-powershell.md).
