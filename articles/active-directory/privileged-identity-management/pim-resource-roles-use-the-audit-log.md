---
title: Erőforrás-szerepkörökkel az Azure-erőforrások naplózása a Privileged Identity Management használatával |} A Microsoft Docs
description: Ismerteti az összes szerepkör tevékenység képet kaphat az egy adott erőforráshoz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 8f328a609d696886ed452589d3cdfb5f45aec62a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621283"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Erőforrás-szerepkörökkel az Azure-erőforrások naplózása a Privileged Identity Management használatával 

Erőforrás naplózása megtekintheti az összes szerepkör-tevékenység az erőforrás. Szűrheti az adatokat egy előre definiált dátum- vagy egyéni tartomány használatával.
![Adatok](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Erőforrás naplózása is egy felhasználói tevékenység részletei gyors hozzáférést biztosít. A **naplózás típusa**válassza **aktiválás**. Válassza ki **(tevékenységet)** annak a felhasználónak a műveletek az Azure-erőforrások megtekintéséhez.
![Tevékenység részletei](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Több tevékenység részletei](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Saját naplózás

Saját naplózás megtekintheti a felhasználó személyes szerepkör tevékenység. Szűrheti az adatokat egy előre definiált dátum- vagy egyéni tartomány használatával.
![Személyes szerepkör tevékenység](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Aktiválás és az Azure Resource-tevékenységek megtekintése

Milyen műveletek egy adott felhasználó tartott a különböző erőforrások megtekintéséhez áttekintheti az Azure-erőforrás tevékenység, amely egy adott aktiválási időszak van társítva. Első lépésként válassza ki a felhasználó a **tagok** nézet vagy egy adott szerepkör tagjai listájából. Az eredmény megjelenítése a felhasználói műveletek az Azure-erőforrások dátum szerint. Azt is bemutatja a legutóbbi szerepkör-aktiválások, hogy ugyanazon időszakra.

![Felhasználói adatok](media/azure-pim-resource-rbac/rbac-user-details.png)

Egy adott szerepkör-aktiválás kiválasztásával tekinthet meg a szerepkör aktiválásának részletei és a megfelelő Azure-erőforrás tevékenység, amely történt, miközben a felhasználó volt aktív.

![Válassza ki a szerepkör aktiválása](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

