---
title: fájl belefoglalása
description: fájl belefoglalása
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 605533f25b36a92a660301d28aa63cb2ecdd44f4
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37910014"
---
Egy virtuális hálózati átjáró létrehozásakor meg kell adni a használni kívánt termékváltozatot. Válassza ki a számítási feladatok, az átviteli sebesség, a funkciók és a szolgáltatói szerződés igényeinek megfelelő termékváltozatot.

###  <a name="benchmark"></a>Átjáró-termékváltozatok alagút, kapcsolat és átviteli sebesség szerint

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

###  <a name="feature"></a>Átjáró-termékváltozatok által a szolgáltatás beállítása

VPN-átjárók új Termékváltozatai leegyszerűsítheti az átjárókon elérhető szolgáltatáskészleteket:

| **Termékváltozat**| **Szolgáltatások**|
| ---    | ---         |
|**Alapszintű** (*)   | **Útvonalalapú VPN**: 10 alagút P2S-sel; p2s RADIUS-hitelesítés nélkül IKEv2 for P2S nélkül<br>**Házirend-alapú VPN** (IKEv1): 1 alagút, P2S nélkül|
| **VpnGw1, VpnGw2 és VpnGw3** | **Útvonalalapú VPN**: legfeljebb 30 alagút (*), P2S, BGP, aktív-aktív, egyéni IPsec/IKE-házirend, ExpressRoute/VPN együttes használata |
|        |             |

( * ) A „PolicyBasedTrafficSelectors” paraméter konfigurálásával egy útvonalalapú VPN-átjárót (VpnGw1, VpnGw2, VpnGw3) több helyszíni, házirendalapú tűzfaleszközhöz is csatlakoztathat. További részletekért tekintse meg a [VPN-átjárók több helyszíni házirendalapú VPN-eszközhöz való csatlakoztatása a PowerShellel](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) című cikket.

(\*\*) Az alapszintű Termékváltozat számít egy örökölt Termékváltozat. Az alapszintű Termékváltozat bizonyos funkció korlátozásokkal rendelkezik. Egy átjáróval, amely az új átjáró-termékváltozatok egyik alapszintű Termékváltozat nem méretezhetők át, az új Termékváltozatra, amely magában foglalja a törlését és újbóli létrehozását a VPN-átjáró inkább módosítania.

###  <a name="workloads"></a>Átjáró-termékváltozatok - éles vs. Dev-Test számítási feladatok

SLA-k és szolgáltatások eltérései miatt fejlesztési-tesztelési és éles környezetben az alábbi termékváltozatokat javasoljuk:

| **Számítási feladat**                       | **Termékváltozatok**               |
| ---                                | ---                    |
| **Termelés, kritikus fontosságú számítási feladatok** | VpnGw1, VpnGw2, VpnGw3 |
| **Dev-test vagy a koncepció igazolása**   | Alapszintű (*)                 |
|                                    |                        |

(\*\*) Az alapszintű Termékváltozat örökölt Termékváltozat számít, és a funkció korlátozásokkal rendelkezik. Győződjön meg arról, hogy támogatott-e a szolgáltatás, amely van szüksége, az alapszintű Termékváltozat használata előtt.

Ha a régi termékváltozatok (örökölt) használ, a a termelési termékváltozatnak a Standard és HighPerformance. További információk és útmutatás a régi termékváltozatokról: [átjáró-termékváltozatok (örökölt)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).
