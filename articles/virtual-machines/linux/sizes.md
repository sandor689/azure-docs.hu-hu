---
title: Az Azure-beli Linuxos virtuális gépek méreteit |} A Microsoft Docs
description: A Linux rendszerű virtuális gépek az Azure-ban elérhető különböző méretű sorolja fel.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: 5ee3d29ceada238c5e4a633b501f63ed307ba76e
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900867"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Az Azure-ban Linux rendszerű virtuális gépek méretei
Ez a cikk ismerteti az elérhető méretek és a beállításokat az Azure virtuális gépeket, a Linux-alkalmazások és számítási feladatok futtatására használhatja. Érdemes figyelembe vennie, ha tervezi az ezekkel az erőforrásokkal való telepítésével kapcsolatos megfontolások is tartalmazza. Ez a cikk érhető el is [Windows virtuális gépek](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| Típus                     | Méretek           |    Leírás       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Általános célú](sizes-general.md)          | B, Dsv3, Dv3, DSv2, a Dv2, az Av2  | Kiegyensúlyozott processzor-memória arány. Ideális választás tesztelési-fejlesztési feladatokhoz, kis és közepes méretű adatbázisokhoz, valamint kis és közepes adatforgalmú webkiszolgálókhoz. |
| [Számításra optimalizált](sizes-compute.md)        | F Fsv2, az Fs-ben             | Magas processzor-memória arány. Alkalmas közepes adatforgalmú webkiszolgálók, hálózati berendezések, kötegfolyamatok és alkalmazáskiszolgálók számára.        |
| [Memóriaoptimalizált](sizes-memory.md)         | Az Esv3, Ev3, M, GS, G, DSv2, a Dv2  | Magas memória – Processzor arány. Ideális választás relációs adatbázis-kiszolgálókhoz, közepes és nagy gyorsítótárakhoz és memóriabeli elemzésekhez.                 |
| [Tárolásra optimalizált](sizes-storage.md)        | Ls                | Magas lemez-adatátviteli és I/O-műveleti jellemzők. Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.                                                         |
| [GPU](sizes-gpu.md)            | NV, NC, NCv2, az NCv3, SD            | Specializált virtuális gépek grafikai renderelésre és videószerkesztésre célzott, valamint modell a betanítási vagy következtetési (ND) használatával deep learninget. Elérhető egy vagy több gpu-kkal.       |
| [Nagy teljesítményű számítás](sizes-hpc.md) | H       | A leggyorsabb és leghatékonyabb processzorral rendelkező virtuális gépeink, választható nagy átviteli sebességű (távoli közvetlen memória-hozzáférést lehetővé tevő) hálózati adapterrel. 

<br>

- A különböző fürtméretekkel járó díjszabással kapcsolatos információkért lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- A Virtuálisgép-méretek az Azure-régiók rendelkezésre állás érdekében tekintse meg a [elérhető termékek régiók szerint](https://azure.microsoft.com/regions/services/).
- Általános korlátozások az Azure virtuális gépekhez, olvassa el [Azure-előfizetés és a szolgáltatások korlátozásai, kvótái és megkötései](../../azure-subscription-service-limits.md).
- Tudjon meg többet [Azure számítási egységek (ACU)](acu.md) Azure-termékváltozatok hasonlítsa össze a számítási teljesítményt nyújt segítséget.


## <a name="rest-api"></a>REST API

Információk a Virtuálisgép-méretek lekérdezéshez REST API használatával tekintse meg a következőket:

- [Átméretezéséhez elérhető virtuálisgép-méretek listázása](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Egy előfizetéshez tartozó elérhető virtuálisgép-méretek listázása](https://docs.microsoft.com/rest/api/compute/virtualmachines/listall)
- [Egy rendelkezésre állási csoportban elérhető virtuálisgép-méretek listázása](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Tudjon meg többet [Azure számítási egységek (ACU)](acu.md) Azure-termékváltozatok hasonlítsa össze a számítási teljesítményt nyújt segítséget.

## <a name="benchmark-scores"></a>Teljesítményteszt pontszámai

Ismerje meg bővebben számítási teljesítmény, a Linux rendszerű virtuális gépek a [CoreMark számításiteljesítmény-mérési pontszámok](compute-benchmark-scores.md).

## <a name="next-steps"></a>További lépések

További információ a különböző Virtuálisgép-méretek érhetők el:
- [Általános célú](sizes-general.md)
- [Számításra optimalizált](sizes-compute.md)
- [Memóriaoptimalizált](sizes-memory.md)
- [Tárolásra optimalizált](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Nagy teljesítményű számítás](sizes-hpc.md)
- Ellenőrizze a [előző generációs](sizes-previous-gen.md) lapját A Standard, a Dv1 (D1-4 és D11-14 v1), és az A8 – A11-sorozat



