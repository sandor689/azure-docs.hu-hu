---
title: Virtuálisgép-méretek támogatott az Azure Stackben |} A Microsoft Docs
description: A támogatott Virtuálisgép-méretek az Azure Stack-hivatkozás.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.openlocfilehash: bfdfc2ff879db11f5a8ba35898069b2b937c49b0
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928981"
---
# <a name="virtual-machine-sizes-supported-in-azure-stack"></a>Az Azure Stackben támogatott virtuálisgép-méretek

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Ez a cikk a virtuális gép (VM) méretek az Azure Stackben elérhető sorolja fel.

## <a name="general-purpose"></a>Általános célú

Általános célú virtuális gépek méreteit adjon meg egy kiegyensúlyozott Processzor-memória arány. Ezek szolgálnak tesztelési-fejlesztési feladatokhoz, kis és közepes méretű adatbázisokhoz, és a kis és közepes adatforgalmú webkiszolgálókhoz.

### <a name="basic-a"></a>Alapszintű A
|Méret – méret\név |vCPU     |Memory (Memória) | Ideiglenes lemez max. mérete | Az operációs rendszer maximális adatátviteli sebességet: (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. teljesítménye (IOPS) | Hálózati adapterek maximális száma |    
|-----------------|-----|---------|---------|-----|------|-----------|----|
|**A0\Basic_A0**  |1    |768 MB   | 20 GB   |300  | 300  |1 / 1x300  |1   |
|**A1\Basic_A1**  |1    |1,75 GB  | 40 GB   |300  | 300  |2 / 2x300  |1   |
|**A2\Basic_A2**  |2    |3,5 GB   | 60 GB   |300  | 300  |4 / 4x300  |1   |
|**A3\Basic_A3**  |4    |7 GB     | 120 GB  |300  | 300  |8 / 8x300  |1   |
|**A4\Basic_A4**  |8    |14 GB    | 240 GB  |300  | 300  |16 / 16 X 300 |1   |

### <a name="standard-a"></a>Standard A 
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |    
|----------------|--|------|----|----|----|-------|---------|
|**Standard_A0** |1 |0.768 |20  |500 |500 |1x500  |1 / 100  |
|**Standard_A1** |1 |1.75  |70  |500 |500 |2x500  |1 / 500  |
|**Standard_A2** |2 |3.5   |135 |500 |500 |4x500  |1 / 500  |
|**Standard_A3** |4 |7     |285 |500 |500 |8x500  |2 / 1000 |
|**Standard_A4** |8 |14    |605 |500 |500 |16x500 |4 / 2000 |
|**Standard_A5** |2 |14    |135 |500 |500 |4x500  |2 / 500  |
|**Standard_A6** |4 |28    |285 |500 |500 |8x500  |2 / 1000 |
|**Standard_A7** |8 |56    |605 |500 |500 |16x500 |4 / 2000 |

### <a name="av2-series"></a>Av2-sorozat
*Azure Stack 1804 vagy újabb verziójára van szükség*

|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|-----------------|----|----|-----|-----|------|--------------|---------|
|**Standard_A1_v2**  |1   |2   |10   |500 |1000  |2 / 2x500   |2 / 250  |
|**Standard_A2_v2**  |2   |4   |20   |500 |2000  |4 / 4x500   |2 / 500  |
|**Standard_A4v2**   |4   |8   |40   |500 |4000  |8 / 8x500   |4 / 1000 |
|**Standard_A8_v2**  |8   |16  |80   |500 |8000  |16 / 16x500 |8 / 2000 |
|**Standard_A2m_v2** |2   |16  |20   |500 |2000  |4 / 4x500   |2 / 500  |
|**Standard_A4m_v2** |4   |32  |40   |500 |4000  |8 / 8x500   |4 / 1000 |
|**Standard_A8m_v2** |8   |64  |80   |500 |8000  |16 / 16x500 |8 / 2000 |

### <a name="d-series"></a>D-sorozat
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|----------------|----|----|-----|----|------|------------|---------|
|**Standard_D1** |1   |3.5 |50   |500 |3000  |4 / 4x500   |1 / 500  |
|**Standard_D2** |2   |7   |100  |500 |6000  |8 / 8x500   |2 / 1000 |
|**Standard_D3** |4   |14  |200  |500 |12000 |16 / 16x500 |4 / 2000 |
|**Standard_D4** |8   |28  |400  |500 |24000 |32 / 32x500 |8 / 4000 |


### <a name="ds-series"></a>DS-sorozat
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|-----------------|----|----|-----|-----|------|-------------|---------|
|**Standard_DS1** |1   |3.5 |7    |1000 |4000  |4 / 4x2300   |1 / 500  |
|**Standard_DS2** |2   |7   |14   |1000 |8000  |8 / 8x2300   |2 / 1000 |
|**Standard_DS3** |4   |14  |28   |1000 |16000 |16 / 16x2300 |4 / 2000 |
|**Standard_DS4** |8   |28  |56   |1000 |32000 |32 / 32x2300 |8 / 4000 |

### <a name="dv2-series"></a>Dv2-sorozat
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3.5 |50   |500 |3000  |4 / 4x500   |1 / 500  |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8 / 8x500   |2 / 1000 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16 / 16x500 |4 / 2000 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32 / 32x500 |8 / 4000 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64 / 64x500 |8 / 8000 |

### <a name="dsv2-series"></a>DSv2-sorozat
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3.5 |7   |1000 |4000  |4 / 4x2300   |1 / 750  |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8 / 8x2300   |2 / 1500 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16 / 16x2300 |4 / 3000 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32 / 32x2300 |8 / 6000 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64 / 64x2300 |8 / 10000 |


## <a name="compute-optimized"></a>Számításra optimalizált
### <a name="f-series"></a>F-sorozat
*Azure Stack 1804 vagy újabb verziójára van szükség*

|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|-----------------|----|----|-----|----|------|------------|---------|
|**Standard_F1**  |1   |2   |16   |500 |3000  |4 / 4x500   |2 / 750  |
|**Standard_F2**  |2   |4   |32   |500 |6000  |8 / 8x500   |2 / 1500 |
|**Standard_F4**  |4   |8   |64   |500 |12000 |16 / 16x500 |4 / 3000 |
|**Standard F8 méretű**  |8   |16  |128  |500 |24000 |32 / 32x500 |8 / 6000 |
|**Standard f16 méretű** |16  |32  |256  |500 |48000 |64 / 64x500 |8 / 6000 - 12000  |


### <a name="fs-series"></a>Fs sorozat
*Azure Stack 1804 vagy újabb verziójára van szükség*  

|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|------------------|----|----|----|-----|------|-------------|---------|
|**Standard_F1s**  |1   |2   |4   |1000 |4000  |4 / 4x2300   |2 / 750  |
|**Standard_F2s**  |2   |4   |8   |1000 |8000  |8 / 8x2300   |2 / 1500 |
|**Standard_F4s**  |4   |8   |16  |1000 |16000 |16 / 16x2300 |4 / 3000 |
|**Standard_F8s**  |8   |16  |32  |1000 |32000 |32 / 32x2300 |8 / 6000 |
|**Standard_F16s** |16  |32  |64  |1000 |64000 |64 / 64x2300 |8 / 6000 - 12000  |


### <a name="fsv2-series"></a>Fsv2-sorozat
*Azure Stack 1804 vagy újabb verziójára van szükség* 

|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_F2s_v2**  |2   |4   |16   |1000 |4000   |4 / 4x2300    |Mérsékelt |
|**Standard_F4s_v2**  |4   |8   |32   |1000 |8000   |8 / 8x2300    |Mérsékelt |
|**Standard_F8s_v2**  |8   |16  |64   |1000 |16000  |16 / 16x2300  |Magas     |
|**Standard_F16s_v2** |16  |32  |128  |1000 |32000  |32 / 32x2300  |Magas     |
|**Standard_F32s_v2** |32  |64  |256  |1000 |64000  |32 / 32x2300  |Magas  |
|**Standard_F64s_v2** |64  |128 |512  |1000 |128000 |32 / 32x2300  |Rendkívül magas  |


## <a name="memory-optimized"></a>Memóriaoptimalizált

A memória optimalizált Virtuálisgép-méretek adja meg a magas memória – Processzor arány célja, hogy olyan relációs adatbázis-kiszolgálókhoz, közepes és nagy gyorsítótárakhoz és memóriabeli elemzésekhez.

### <a name="mo-d"></a>A D-sorozat
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|------------------|---|----|----|--------|------|------------|---------|
|**Standard_D11**  |2  |14  |100 |500     |6000  |8 / 8x500   |2 / 1000 |
|**Standard_D12**  |4  |28  |200 |500     |12000 |16 / 16x500 |4 / 2000 |
|**Standard_D13**  |8  |56  |400 |500     |24000 |32 / 32x500 |8 / 4000 |
|**Standard_D14**  |16 |112 |800 |500     |48000 |64 / 64x500 |8 / 8000 |

### <a name="mo-ds"></a>DS-sorozat
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|-------------------|---|----|----|--------|------|-------------|---------|
|**Standard_DS11**  |2  |14  |28  |1000    |8000  |8 / 8x2300   |2 / 1000 |
|**Standard_DS12**  |4  |28  |56  |1000    |12000 |16 / 16x2300 |4 / 2000 |
|**Standard_DS13**  |8  |56  |112 |1000    |32000 |32 / 32x2300 |8 / 4000 |
|**Standard_DS14**  |16 |112 |224 |1000    |64000 |64 / 64x2300 |8 / 8000 |

### <a name="mo-dv2"></a>A Dv2 sorozat
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8 / 8x500    |2 / 1500 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16 / 16x500  |4 / 3000 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32 / 32x500  |8 / 6000 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64 / 64x500  |8 / 10000 |


### <a name="mo-dsv2"></a>DSv2 sorozat
|Méret     |vCPU     |Memória (GiB) | Ideiglenes tárhely (GB)  | Az operációs rendszer maximális lemezteljesítményét (IOPS) | Ideiglenes tárterület maximális teljesítménye (IOPS) | Adatlemezek max. száma / átviteli sebesség (IOPS) | Hálózati adapterek maximális száma / várt hálózati sávszélesség (Mbps) |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |8 / 8x2300    |2 / 1500 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |16 / 16x2300  |4 / 3000 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |32 / 32x2300  |8 / 6000 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |64 / 64x2300  |8 / 10000 |


## <a name="next-steps"></a>További lépések

[Szempontok a virtuális gépek az Azure Stackben](azure-stack-vm-considerations.md)
