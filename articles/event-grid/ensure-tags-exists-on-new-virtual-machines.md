---
title: Az Azure Automation integrálása az Event Griddel | Microsoft Docs
description: Ismerje meg, hogyan adhat hozzá automatikusan címkéket az új virtuális gépek létrehozásakor, valamint küldhet értesítést a Microsoft Teamsbe.
keywords: automation, runbook, teams, event grid, virtuális gép, VM
services: automation
documentationcenter: ''
author: eamonoreilly
manager: ''
editor: ''
ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2017
ms.author: eamono
ms.openlocfilehash: 6270f8bad893798f46d8db91e7b1140b6a125350
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049864"
---
# <a name="integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Az Azure Automation integrálása az Event Grid és a Microsoft Teams szolgáltatással

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Event Grid-runbookminta importálása.
> * Opcionális Microsoft Teams-webhook létrehozása.
> * Webhook létrehozása a runbookhoz.
> * Event Grid-előfizetés létrehozása.
> * A runbookot aktiváló virtuális gép létrehozása.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag elvégzéséhez egy [Azure Automation-fiók](../automation/automation-offering-get-started.md) szükséges a runbook tárolásához, amelyet az Azure Event Grid-előfizetésből fogunk aktiválni.

* Az `AzureRM.Tags` modult be kell tölteni az Automation-fiókba. A modulok az Azure Automationbe való importálásával kapcsolatban lásd [a modulok az Azure Automationbe való importálását](../automation/automation-update-azure-modules.md) bemutató cikket.

## <a name="import-an-event-grid-sample-runbook"></a>Event Grid-runbookminta importálása

1. Válassza ki az Automation-fiókot, és lépjen a **Runbookok** lapra.

   ![Runbookok kiválasztása](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. Kattintson a **Tallózás a katalógusban** gombra.

3. Keressen rá az **Event Grid** kifejezésre, és válassza **Az Azure Automation integrálása az Event Griddel** lehetőséget.

    ![Katalógusrunbook importálása](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Válassza az **Importálás** lehetőséget, és nevezze el a **Watch-VMWrite** néven.

5. Az importálás befejeztével válassza a **Szerkesztés** lehetőséget a runbook forrásának megtekintéséhez. Válassza ki a **Közzététel** gombot.

> [!NOTE]
> A szkript 74. sorát le kell cserélni a következőre: `Update-AzureRmVM -ResourceGroupName $VMResourceGroup -VM $VM -Tag $Tag | Write-Verbose`. A `-Tags` paraméter a `-Tag` paraméterre változott.

## <a name="create-an-optional-microsoft-teams-webhook"></a>Opcionális Microsoft Teams-webhook létrehozása

1. A Microsoft Teamsben válassza a **További lehetőségek** elemet a csatorna neve mellett, majd válassza az **Összekötők** lehetőséget.

    ![Microsoft Teams-kapcsolatok](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Görgesse végig az összekötők listáját a **Bejövő webhook** elemig, és válassza a **Hozzáadás** lehetőséget.

3. Adja a webhooknak az **AzureAutomationIntegration** nevet, és válassza a **Létrehozás** lehetőséget.

4. Másolja a webhookot a vágólapra, és mentse. A rendszer a webhook URL-címének segítségével információkat küld a Microsoft Teamsnek.

5. A webhook mentéséhez válassza a **Kész** lehetőséget.

## <a name="create-a-webhook-for-the-runbook"></a>Webhook létrehozása a runbookhoz

1. Nyissa meg a Watch-VMWrite runbookot.

2. Válassza a **Webhookok** lehetőséget, és kattintson a **Webhook hozzáadása** gombra.

3. Adja a webhooknak a **WatchVMEventGrid** nevet. Másolja az URL-címet a vágólapra, és mentse.

    ![Webhook nevének konfigurálása](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. Válassza a **Paraméterek és futtatási beállítások konfigurálása** lehetőséget, és adja meg a Microsoft Teams-webhook URL-címét a **CHANNELURL** mezőben. Hagyja a **WEBHOOKDATA** mezőt üresen.

    ![Webhook paramétereinek konfigurálása](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Az Automation-runbook webhookjának létrehozásához kattintson az **OK** gombra.


## <a name="create-an-event-grid-subscription"></a>Event Grid-előfizetés létrehozása
1. Az **Automation-fiók** áttekintő lapján válassza az **Event Grid** lehetőséget.

    ![Event Grid kiválasztása](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. Kattintson az **+ Esemény-előfizetés** gombra.

3. Konfigurálja az előfizetést az alábbi információkkal:

    *   Adja az előfizetésnek az **AzureAutomation** nevet.
    *   A **Témakörtípus** mezőben válassza az **Azure-előfizetések** lehetőséget.
    *   Törölje az **Előfizetés az összes eseménytípusra** jelölőnégyzet jelölését.
    *   Az **Eseménytípusok** mezőben válassza az **Erőforrás írása sikeres** lehetőséget.
    *   Az **Előfizető végpontja** mezőben adja meg a Watch-VMWrite runbook webhookjának URL-címét.
    *   Az **Előtagszűrő** mezőben adja meg azt az előfizetést és az erőforráscsoportot, ahol az újonnan létrehozott virtuális gépeket figyelni szeretné. Ennek így kell kinéznie: `/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`

4. Kattintson a **Létrehozás** gombra az Event Grid-előfizetés mentéséhez.

## <a name="create-a-vm-that-triggers-the-runbook"></a>A runbookot aktiváló virtuális gép létrehozása
1. Hozzon létre egy új virtuális gépet az Event Grid-előfizetés előtagszűrőjében megadott erőforráscsoportban.

2. A Watch-VMWrite runbookot meg kell hívni, és egy új címkét hozzá kell adni a virtuális géphez.

    ![Virtuális gép címkéje](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. A rendszer egy új üzenetet küld a Microsoft Teams-csatornára.

    ![Microsoft Teams-értesítés](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>További lépések
Ebben az oktatóanyagban az Event Grid és az Automation közötti integrációt állította be. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Event Grid-runbookminta importálása.
> * Opcionális Microsoft Teams-webhook létrehozása.
> * Webhook létrehozása a runbookhoz.
> * Event Grid-előfizetés létrehozása.
> * A runbookot aktiváló virtuális gép létrehozása.

> [!div class="nextstepaction"]
> [Egyéni események létrehozása és átirányítása az Event Griddel](../event-grid/custom-event-quickstart.md)
