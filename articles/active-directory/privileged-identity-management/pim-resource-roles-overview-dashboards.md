---
title: 'Áttekintés: Privileged Identity Management hozzáférési felülvizsgálat végrehajtása az Azure-erőforrások |} A Microsoft Docs'
description: Ez a dokumentum ismerteti, hogyan hajthat végre a hozzáférési felülvizsgálat a PIM az Azure-erőforrásokhoz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 40fe6a4b0bec2540afee635b16ee3482a182a2df
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621488"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review"></a>Egy erőforrás Irányítópult segítségével a hozzáférési felülvizsgálat végrehajtása

Egy erőforrás Irányítópult segítségével a Privileged Identity Management (PIM) hozzáférési felülvizsgálat végrehajtása az Azure-erőforrásokhoz. Az adminisztrátori nézet irányítópult három elsődleges összetevőből áll:

- Szerepkör-aktiválások erőforrás grafikus ábrázolását.
- Két diagram, amely a szerepkör-hozzárendelések terjesztési hozzárendelés-típus szerinti megjelenítéséhez.
- Egy új szerepkör-hozzárendelések kérelmekre vonatkozó adatok területen.

![Képernyőkép az adminisztrátori nézet irányítópult gráfokat és diagramokat](media/azure-pim-resource-rbac/rbac-overview-top.png)

![Képernyőfelvétel a felügyelet nézet irányítópult adatok listája:](media/azure-pim-resource-rbac/role-settings.png)

A grafikus ábrázolását erőforrás szerepkör-aktiválások az elmúlt hét napban ismerteti. Ezeket az adatokat a kiválasztott erőforrás hatókörét, és megjeleníti a leggyakrabban használt szerepkörök (tulajdonos, közreműködő, felhasználói hozzáférés rendszergazdája), és az összes szerepköre kombinált aktiválások.

Az aktiválások graph jobbra két diagram megjelenítéséhez a szerepkör-hozzárendelések terjesztési hozzárendelési típust, felhasználókat és csoportokat is. Az érték a százalék (vagy fordítva), a diagram szelet kiválasztásával módosíthatja.

Alább a diagramok és a felhasználóknak és csoportoknak az új szerepkör-hozzárendelések száma az elmúlt 30 napban, valamint a szerepkörök (csökkenő sorrendben) hozzárendelések száma szerint rendezett listáját láthatja.


