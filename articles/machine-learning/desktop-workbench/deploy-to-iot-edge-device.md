---
title: Az Azure Machine Learning modell rendszerbe állítása Azure IoT peremhálózati eszköz |} Microsoft Docs
description: Ez a dokumentum azt ismerteti, hogyan telepíthetők Azure Machine Learning modellek Azure IoT peremeszközök.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.topic: article
ms.date: 2/1/2018
ms.openlocfilehash: 1dffdee032c5b079aa5b81284cebe8f6471efebd
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833638"
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Az Azure Machine Learning modell Azure IoT peremhálózati eszköz telepítése

Docker-alapú webszolgáltatásként indexelése Azure Machine Learning modellek is Azure IoT peremeszközök futtathatja. További parancsfájlok és utasítások megtalálhatók a [Azure IoT peremhálózati AI eszköztára](http://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Azok a modell
Üzembe helyezheti modelljét utasításait követve [Azure Machine Learning modell felügyeleti webes szolgáltatástelepítés](model-management-service-deploy.md) Docker lemezkép létrehozása a modell.

## <a name="deploy-to-azure-iot-edge"></a>Az Azure IoT peremhálózati telepítése
Az Azure IoT peremhálózati felhő elemzés és üzleti logika áthelyezése eszközök. Minden gépi tanulási modellek IoT peremeszközök is futtathatók. IoT peremhálózati eszköz beállítását, és hozzon létre egy központi telepítési dokumentációjában található [aka.ms/azure-iot-edge-doc](https://aka.ms/azure-iot-edge-doc).

Az alábbiakban további ügyeljen a következőkre.

### <a name="add-registry-credentials-to-the-edge-runtime-on-your-edge-device"></a>Adja hozzá a peremhálózati eszköz a peremhálózati runtime beállításjegyzék hitelesítő adatokat
A számítógépen, ahol az IoT-Edge, amelyen vegye fel a hitelesítő adatokat, a beállításjegyzék, a futtatókörnyezet is hozzáférhet a tároló lekéréses.

Windowson futtassa az alábbi parancsot:
```cmd/sh
iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```
Linuxon futtassa az alábbi parancsot:
```cmd/sh
sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```

### <a name="find-the-machine-learning-container-image-location"></a>A Machine Learning tároló kép keresése
A Machine Learning-tároló lemezkép helyét van szüksége. A tároló kép keresése:

1. Jelentkezzen be az [Azure Portalra](http://portal.azure.com/).
2. Az a **Azure tároló beállításjegyzék**, válassza ki a megvizsgálni kívánt beállításjegyzék.
3. Kattintson a beállításjegyzékben **Tárházak** a tárolóhelyekkel és a képek listájának megjelenítéséhez.













