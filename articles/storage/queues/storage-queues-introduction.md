---
title: Az Azure Queue storage bemutatása |} A Microsoft Docs
description: Az Azure Queue storage bemutatása
services: storage
author: tamram
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.component: queues
ms.openlocfilehash: d2d4a31097c4050ba9193fc9d6fa076fe9c6e27f
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524831"
---
# <a name="introduction-to-queues"></a>Az üzenetsorok bemutatása

Az Azure Queue Storage szolgáltatás üzenetek nagy számban történő tárolására szolgál, amelyek HTTP- vagy HTTPS-kapcsolattal, hitelesített hívásokon keresztül a világon bárhonnan elérhetők. Egyetlen üzenetsor akár 64 KB méretű is lehet, és a tárfiók maximális kapacitásán belül több millió üzenetet tartalmazhat.

## <a name="common-uses"></a>Gyakori használati mód

A Queue Storage gyakori használati módjai:

* Hátralékos munkák létrehozása aszinkron feldolgozáshoz
* Üzenetek átadása egy Azure webes szerepkörről egy Azure feldolgozói szerepkörnek

## <a name="queue-service-concepts"></a>A Queue szolgáltatás alapfogalmai

A Queue szolgáltatás az alábbi összetevőkből áll:

![Üzenetsor-kapcsolatos fogalmak](./media/storage-queues-introduction/queue1.png)

* **URL-formátum:** Az üzenetsorok a következő URL-formátummal érhetők el:   
    https://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Az ábra egyik üzenetsora a következő URL-címmel érhető el:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Tárfiók:** minden, az Azure Storage-hozzáférés tárfiókon keresztül történik. A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (Az Azure Storage méretezhetőségi és teljesítménycéljai).

* **Üzenetsor:** Az üzenetsorok üzenetek készleteit tartalmazzák. Az összes üzenetnek üzenetsorban kell lennie. Vegye figyelembe, hogy az üzenetsor neve csak kisbetűket tartalmazhat. Az üzenetsorok elnevezésével kapcsolatos információkat lásd: [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Üzenetsorok és metaadatok elnevezése).

* **Üzenet:** Egy legfeljebb 64 KB méretű, tetszőleges méretű üzenet. A maximális időtartam, egy üzenet a várólistában lévő maradhat a hét nap.

## <a name="next-steps"></a>További lépések

* [Tárfiók létrehozása](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Ismerkedés az Üzenetsorokkal .NET használatával](storage-dotnet-how-to-use-queues.md)
