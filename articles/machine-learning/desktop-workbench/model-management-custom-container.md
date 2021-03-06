---
title: Az Azure ML-modell telepítéséhez használt tároló lemezkép testreszabása |} Microsoft Docs
description: Ez a cikk ismerteti az Azure Machine Learning modellek tároló lemezkép testreszabása
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: mldocs, raymondl
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.topic: article
ms.date: 3/26/2018
ms.openlocfilehash: 715b4c1f02622b015e4118ac38edd9fe6a051ed7
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834709"
---
# <a name="customize-the-container-image-used-for-azure-ml-models"></a>A használt Azure ML modellek tároló lemezkép testreszabása

Ez a cikk ismerteti az Azure Machine Learning modellek tároló lemezkép testreszabása.  Azure ML munkaterület tárolókat használ a machine learning modellek telepítéséről. A modellek telepítve van a függőségeikkel együtt, és az Azure ML összeállít egy lemezkép a modellből, a függőségeket, és a társított fájlok.

## <a name="how-to-customize-the-docker-image"></a>A Docker lemezkép testreszabása
Az Azure ML telepítő használatával Docker lemezkép testreszabása:

1. A `dependencies.yml` fájl: függőségei vannak, amelyek a telepíthető bővítmények kezeléséhez [PyPi]( https://pypi.python.org/pypi), használhatja a `conda_dependencies.yml` a munkaterületet üzemeltető projekt fájlként, vagy hozza létre a sajátját. Ez a pip telepíthető Python függőségek telepítése az ajánlott megközelítés.

   Példa parancssori parancsot:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> -c amlconfig\conda_dependencies.yml
   ```

   Példa conda_dependencies fájl: 
   ```yaml
   name: project_environment
   dependencies:
      - python=3.5.2
      - scikit-learn
      - ipykernel=4.6.1
      
      - pip:
        - azure-ml-api-sdk==0.1.0a11
        - matplotlib
   ```
        
2. A Docker lépések fájl: Ezzel a beállítással testre a telepített lemezképhez, amely nem telepíthető a PyPi függőségek telepítésével. 

   A fájl egy DockerFile hasonló Docker-telepítési lépéseket kell tartalmaznia. A fájl a következő parancsok használhatók: 

    FUTTATÁS ENV, ARG, CÍMKE, TESZI KÖZZÉ

   Példa parancssori parancsot:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> --docker-file <myDockerStepsFileName> 
   ```

   Lemezkép, a jegyzékfájlt és a szolgáltatás parancsokat fogadja el a docker-fájl jelzőt.

   Példa Docker lépéseket fájl:
   ```docker
   # Install tools required to build the project
   RUN apt-get update && apt-get install -y git libxml2-dev
   # Install library dependencies
   RUN dep ensure -vendor-only
   ```

> [!NOTE]
> Az Azure ML tárolókat az alapjául szolgáló lemezképhez Ubuntu, és nem módosítható. Ha megad egy másik alapjául szolgáló lemezképhez, lesz figyelembe véve.

## <a name="next-steps"></a>További lépések
Most, hogy a tároló lemezkép szabta, ezután telepítheti azt egy fürthöz, a nagyméretű használatra.  Webes szolgáltatás központi telepítése egy fürt beállításával kapcsolatos részletekért lásd: [modell Felügyeletkonfigurálási](deployment-setup-configuration.md). 
