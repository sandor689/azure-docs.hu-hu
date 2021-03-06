---
title: Azure Functions Runtime áttekintése |} A Microsoft Docs
description: Az Azure Functions Runtime előzetes verziójának áttekintése
services: functions
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: 2be04c7037dd26755300cf8b7794678a6a958278
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38488460"
---
# <a name="azure-functions-runtime-overview-preview"></a>Az Azure Functions Runtime áttekintése (előzetes verzió)

Az Azure Functions Runtime (előzetes verzió), hogy az egyszerűség és rugalmasság az Azure Functions egy új módon biztosít a programozási modell helyszíni. Az Azure Functions, az azonos nyílt forráskódú gyökerek épülő Azure Functions Runtime üzembe helyezett helyszíni majdnem azonos fejlesztési élmény érdekében a felhőszolgáltatáshoz.

![Az Azure Functions Runtime betekintő portálon][1]

Az Azure Functions Runtime biztosítja, hogy az Azure Functions élményt a felhőben véglegesítése előtt. Ezzel a módszerrel a kód eszközök létrehozhat majd elvégezhet, a felhőbe áttelepítésekor.  A futtatókörnyezet új beállítások, például a kötegelt folyamatok éjszaka futtatni a tartalék számítási teljesítményt a helyszíni számítógépek használatával is megnyílik. Is használhatja eszközök a szervezeten belüli más rendszerekre, mind a helyszíni és felhőbeli feltételesen adatküldéshez.

Az Azure Functions Runtime két részből áll:

* Az Azure Functions Runtime felügyeleti szerepkör
* Az Azure Functions Runtime feldolgozói szerepkör

## <a name="azure-functions-management-role"></a>Az Azure Functions felügyeleti szerepkör

Az Azure Functions felügyeleti szerepkör biztosít egy gazdagépet a függvények helyi felügyeletéhez. Ez a szerepkör az alábbi feladatokat hajtja végre:

* Az Azure Functions felügyeleti portálján, amely megegyezik egy jelenik meg, a üzemeltetési a [az Azure portal](https://portal.azure.com). A portál, amely lehetővé teszi a függvények fejlesztése ugyanúgy, mint az Azure Portalon az egységes élményt nyújt.
* Definíciófrissítés-terjesztés funkciók feldolgozó funkciók között.
* A közzététel végpontja biztosít, így közzéteheti a függvényeket közvetlenül a Microsoft Visual Studio letöltésével és a közzétételi profil importálása.

## <a name="azure-functions-worker-role"></a>Az Azure Functions feldolgozói szerepkör

Az Azure Functions feldolgozói szerepkörök Windows-tárolók üzemelnek, és, ahol a függvénykódban hajt végre.  Több feldolgozói szerepkört telepítheti a szervezetben, és ezt a beállítást, amelyben tehetik legfontosabb úgy tartalék számítási teljesítményt használja.  Egy tartalék számítási számos szervezet esetén például a gépek folyamatosan bekapcsolt, de nem használt nagy e-mail-címen.

## <a name="minimum-requirements"></a>Minimális követelmények

Ismerkedés az Azure Functions Runtime, egy gép Windows Server 2016 vagy Windows 10 alkotói frissítés az SQL Server-példányra hozzáféréssel kell rendelkeznie.

## <a name="next-steps"></a>További lépések

Telepítse a [Azure Functions Runtime előzetes verziója](https://aka.ms/azafrdoc)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
