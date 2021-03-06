---
title: A Visual Studio Azure verem sablonok telepítése |} Microsoft Docs
description: Útmutató a Visual Studio Azure verem sablonok telepítése.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 6cd722fedc0483e37ce6ee491d74a7c985111353
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605123"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>A Visual Studio használatával Azure verem sablonok telepítése

*A következőkre vonatkozik: Azure verem integrált rendszerek és az Azure verem szoftverfejlesztői készlet*

Visual Studio használatával Azure verem Azure Resource Manager sablonok telepítése.

## <a name="to-deploy-a-template"></a>A sablon telepítéséhez

1. [Telepítse, és csatlakozzon](azure-stack-install-visual-studio.md) a Visual Studio Azure verem.
2. Nyissa meg a Visual Studiót.
3. Válassza ki **fájl**, majd válassza ki **új**. A **új projekt**, jelölje be **Azure erőforráscsoport**.
4. Adjon meg egy **neve** az új projekt, és válassza a **OK**.
5. A **Azure-sablon kiválasztása**, válasszon **Azure verem gyors üzembe helyezés** a legördülő listából.
6. Válassza ki **101--storage-fiók létrehozása**, majd válassza ki **OK**.
7. Az új projekt, bontsa ki a **sablonok** csomópontja **Megoldáskezelőben** a rendelkezésre álló sablonok megtekintéséhez.
8. A **Megoldáskezelőben**, válassza ki a projekt nevét, majd válassza ki **telepítés**. Válassza ki **új központi telepítési**.
9. A **telepítés erőforráscsoportra**, használja a **előfizetés** legördülő lista segítségével válassza ki a Microsoft Azure verem előfizetését.
10. Az a **erőforráscsoport** listában, válasszon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.
11. Az a **erőforráscsoport helye** listában válassza ki azt a helyet, és válassza ki **telepítés**.
12. A **paraméterek szerkesztése**, adjon meg értékeket a paraméterek (amelyek eltérőek lehetnek a sablon függően), majd válassza ki **mentése**.

## <a name="next-steps"></a>További lépések

* [Sablonok üzembe helyezése a parancssorral](azure-stack-deploy-template-command-line.md)
* [Az Azure-verem sablonok fejlesztése](azure-stack-develop-templates.md)
