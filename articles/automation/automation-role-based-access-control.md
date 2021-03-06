---
title: Szerepköralapú hozzáférés-vezérlés az Azure Automationben
description: A Szerepköralapú hozzáférés-vezérlés (RBAC) hozzáférés-vezérlést biztosít az Azure-erőforrásokhoz. Ez a cikk ismerteti az RBAC beállítását az Azure Automationben.
keywords: automation rbac, szerepköralapú hozzáférés-vezérlés, azure rbac
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fd96a6cfebe44bd02e3f44a44d91119ad1c2c5a9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598752"
---
# <a name="role-based-access-control-in-azure-automation"></a>Szerepköralapú hozzáférés-vezérlés az Azure Automationben

A Szerepköralapú hozzáférés-vezérlés (RBAC) hozzáférés-vezérlést biztosít az Azure-erőforrásokhoz. Használatával [RBAC](../role-based-access-control/overview.md), feladatokat elkülönítse a munkacsoporton belül, és csak olyan mértékű hozzáférést biztosítania a felhasználók, csoportok, és alkalmazásokat, amelyek kell elvégezni a munkájukat. A szerepköralapú hozzáférés az Azure Portal, az Azure parancssori segédeszközök vagy az Azure felügyeleti API-k segítségével adható a felhasználóknak.

## <a name="roles-in-automation-accounts"></a>Szerepkörök az Automation-fiók

Az Azure Automationben a hozzáférés biztosításához a megfelelő RBAC-szerepkörök rendelhetők az Automation-fiók tartományában lévő felhasználókhoz, csoportokhoz és alkalmazásokhoz. Alább láthatók az Automation-fiók által támogatott beépített szerepkörök:

| **Szerepkör** | **Leírás** |
|:--- |:--- |
| Tulajdonos |Tulajdonosi szerepkör lehetővé teszi a hozzáférést minden források és műveletek belül egy Automation-fiók, beleértve a hozzáférés biztosítása más felhasználók, csoportok és alkalmazások kezelése az Automation-fiók. |
| Közreműködő |A közreműködői szerepkör segítségével minden elemet kezelhet, csak más felhasználók Automation-fiókokra vonatkozó hozzáférési jogosultságait nem módosíthatja. |
| Olvasó |Az olvasói szerep lehetővé teszi az összes erőforrás megtekintését egy Automation-fiókban, de módosítás nem hajtható vele végre. |
| Automation-operátor |Az automatizálási operátor szerepkör lehetővé teszi a runbook neve és a tulajdonságok és az Automation-fiók minden forgatókönyve feladatok létrehozásához és kezeléséhez. Ez a szerepkör akkor hasznos, ha meg szeretné akadályozni, hogy az Automation-fiók erőforrásokat, például a hitelesítő adategységeket és a forgatókönyveket megtekintsék vagy módosítsák, de továbbra is engedélyezi a szervezet tagjai számára a forgatókönyvek végrehajtását. |
|Az Automation-feladat operátora|Az Automation-feladat operátor szerepkör lehetővé teszi az Automation-fiók minden forgatókönyve feladatok létrehozásához és kezeléséhez.|
|Az Automation-runbook operátora|Az Automation-Runbook kezelő szerepkör lehetővé teszi a runbook neve és a tulajdonságok megtekintéséhez.|
| Log Analytics közreműködő | A napló Analytics közreműködői szerepkör lehetővé teszi az összes figyelési adatok olvasása és szerkesztése a figyelési beállításokat. A figyelési beállításokat szerkesztése magában foglalja a virtuális gépek, tárfiókkulcsok tudják konfigurálni a naplók az Azure storage gyűjtemény olvasása létrehozása és Automation-fiókok konfigurálása, megoldások hozzáadása és konfigurálása az Azure diagnostics a Virtuálisgép-bővítmény hozzáadása az összes Azure-erőforrások.|
| Log Analytics olvasó | A napló Analytics olvasó szerepkört teszi megtekintése és keresése összes figyelési adatok, valamint a figyelési beállítások megtekintése. Ez magában foglalja az összes Azure-erőforrások Azure diagnostics konfigurációjának megtekintése. |
| Közreműködő figyelése | A figyelés közreműködői szerepkör lehetővé teszi, hogy olvassa el az összes megfigyelési adatok és figyelési beállítások frissítése.|
| Olvasó figyelése | A figyelési olvasó szerepkört lehetővé teszi, hogy olvassa el az összes figyelési adatokat. |
| Felhasználói hozzáférés rendszergazdája |A felhasználói hozzáférés rendszergazdájának szerepköre lehetővé teszi, hogy kezelje a felhasználói hozzáférést az Azure Automation-fiókokhoz. |

## <a name="role-permissions"></a>Szerepkör-engedélyek

Az alábbi táblázatok bemutatják az egyes szerepkörökhöz megadott engedélyeket. Ez magában foglalhatja műveletek, amelyek az engedélyeket, és NotActions, amelyek korlátozzák őket.

### <a name="owner"></a>Tulajdonos

Egy olyan tulajdonost mindent felügyelhetnek, beleértve a hozzáférést. Az alábbi táblázat a szerepkörhöz rendelt engedélyek:

|Műveletek|Leírás|
|---|---|
|Microsoft.Automation/automationAccounts/|Hozzon létre és kezelheti az erőforrásokat bármilyen típusú.|

### <a name="contributor"></a>Közreműködő

A közreműködői hozzáférés kivételével mindent felügyelhetnek. Az alábbi táblázat az engedélyeket kap, és a szerepkör megtagadva:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Hozzon létre és kezelheti az erőforrásokat bármilyen típusú|
|**Nem műveletek**||
|Microsoft.Authorization/*/Delete| Szerepkörök és szerepkör-hozzárendelések törlése.       |
|Microsoft.Authorization/*/Write     |  Szerepkörök és szerepkör-hozzárendelések létrehozása.       |
|Microsoft.Authorization/elevateAccess/Action    | Képes létrehozni a felhasználói hozzáférés adminisztrátora megtagadja.       |

### <a name="reader"></a>Olvasó

Az olvasót egy Automation-fiók összes erőforrást megtekinthetik, de nem módosíthatja.

|**Műveletek**  |**Leírás**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Automation-fiók összes erőforrások megtekintése. |

### <a name="automation-operator"></a>Automation-operátor

Az automatizálási operátor nem létrehozhatja és feladatok kezelése és olvassa el a runbook nevét és az összes runbook Automation-fiók tulajdonságait.  Megjegyzés: Ha azt szeretné, hogy egyes operátor hozzáférésének szabályozására runbookok majd nem állítsa be ezt a szerepkört, és együtt használja a "Automation-feladat Operator" és "Automatizálási Runbook kezelő" szerepkör. Az alábbi táblázat a szerepkörhöz rendelt engedélyek:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|Microsoft.Authorization/*/read|Olvasási engedély.|
|Microsoft.Automation/automationAccounts/jobs/read|A runbook feladatok listáját.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|A felfüggesztett feladat folytatása.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|A folyamatban lévő feladatok megszakítása.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Olvassa el a feladat adatfolyamok és a kimenetet.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|A folyamatban lévő feladat felfüggesztését.|
|Microsoft.Automation/automationAccounts/jobs/write|Hozzon létre feladatokat.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Olvassa el a szerepkörök és szerepkör-hozzárendelések.         |
|Microsoft.Resources/deployments/*      |Hozzon létre és erőforrás-csoport központi telepítések felügyeletéhez szükséges.         |
|Microsoft.Insights/alertRules/*      | Hozzon létre, és a riasztási szabályok kezelése.        |
|Microsoft.Support/* |Hozzon létre és támogatási jegyek kezelése.|

### <a name="automation-job-operator"></a>Az Automation-feladat operátora

Automation-feladat operátor szerepet kap az automatizálási fiók hatókörben. Ez lehetővé teszi az üzemeltető engedélyekkel a fiók minden forgatókönyve a feladatok létrehozásához és kezeléséhez. Az alábbi táblázat a szerepkörhöz rendelt engedélyek:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|Microsoft.Authorization/*/read|Olvasási engedély.|
|Microsoft.Automation/automationAccounts/jobs/read|A runbook feladatok listáját.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|A felfüggesztett feladat folytatása.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|A folyamatban lévő feladatok megszakítása.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Olvassa el a feladat adatfolyamok és a kimenetet.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|A folyamatban lévő feladat felfüggesztését.|
|Microsoft.Automation/automationAccounts/jobs/write|Hozzon létre feladatokat.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Olvassa el a szerepkörök és szerepkör-hozzárendelések.       |
|Microsoft.Resources/deployments/*      |Hozzon létre és erőforrás-csoport központi telepítések felügyeletéhez szükséges.         |
|Microsoft.Insights/alertRules/*      | Hozzon létre, és a riasztási szabályok kezelése.        |
|Microsoft.Support/* |Hozzon létre és támogatási jegyek kezelése.|

### <a name="automation-runbook-operator"></a>Az Automation-runbook operátora

Az Automation-Runbook kezelő szerepkört kap a Runbook hatókörben. Az automatizálási Runbook kezelő tekintheti meg a runbook nevét és tulajdonságait.  Ezzel a szerepkörrel együtt a "Automation-feladat kezelő" szerepkör lehetővé teszi, hogy a kezelőt is létrehozhat és kezelhet a runbookhoz tartozó feladatok. Az alábbi táblázat a szerepkörhöz rendelt engedélyek:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | A runbookok listában.        |
|Microsoft.Authorization/*/read      | Olvasási engedély.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Olvassa el a szerepkörök és szerepkör-hozzárendelések.         |
|Microsoft.Resources/deployments/*      | Hozzon létre és erőforrás-csoport központi telepítések felügyeletéhez szükséges.         |
|Microsoft.Insights/alertRules/*      | Hozzon létre, és a riasztási szabályok kezelése.        |
|Microsoft.Support/*      | Hozzon létre és támogatási jegyek kezelése.        |

### <a name="log-analytics-contributor"></a>Log Analytics közreműködő

A napló Analytics munkatárs összes figyelési adatot olvashatják és figyelési beállítások szerkesztése. Figyelési beállításainak szerkesztése magában foglalja a Virtuálisgép-bővítmény hozzáadása a virtuális gépek; tárfiókkulcsok tudják konfigurálni a naplók az Azure Storage; gyűjtemény olvasása létrehozásáról és konfigurálásáról az Automation-fiók; megoldásokkal; és az összes Azure-erőforrások konfigurálása az Azure diagnostics. Az alábbi táblázat a szerepkörhöz rendelt engedélyek:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|* / olvasása|Olvassa el az erőforrásokat bármilyen típusú, kivéve a titkos kulcsok.|
|Microsoft.Automation/automationAccounts/*|Automation-fiók kezelése.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Hozzon létre, és a virtuálisgép-bővítmények kezelésére.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Klasszikus tárfiókkulcsok listázása.|
|Microsoft.Compute/virtualMachines/extensions/*|Hozzon létre, és a klasszikus virtuálisgép-bővítmények kezelésére.|
|Microsoft.Insights/alertRules/*|Olvasási, írási és törlési riasztási szabályok.|
|Microsoft.Insights/diagnosticSettings/*|Olvasási, írási és törlési diagnosztikai beállítások.|
|Microsoft.OperationalInsights/*|A Naplóelemzési kezelése.|
|Microsoft.OperationsManagement/*|A munkaterületek megoldások kezelése.|
|Microsoft.Resources/deployments/*|Hozzon létre és erőforrás-csoport központi telepítések felügyeletéhez szükséges.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Hozzon létre és erőforrás-csoport központi telepítések felügyeletéhez szükséges.|
|Microsoft.Storage/storageAccounts/listKeys/action|Tárfiókkulcsok listázása.|
|Microsoft.Support/*|Hozzon létre és támogatási jegyek kezelése.|

### <a name="log-analytics-reader"></a>Log Analytics olvasó

A napló Analytics olvasó megtekintheti és összes figyelési adatok keresése, valamint és megtekintése a figyelési beállításokat, beleértve az összes Azure-erőforrások Azure diagnostics konfigurációjának megtekintése. Az alábbi táblázat az engedélyeket adott meg vagy Tagadott a szerepkörhöz:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|* / olvasása|Olvassa el az erőforrásokat bármilyen típusú, kivéve a titkos kulcsok.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|A Naplóelemzési lekérdezések kezelése.|
|Microsoft.OperationalInsights/workspaces/search/action|Log Analytics-adatok keresése.|
|Microsoft.Support/*|Hozzon létre és támogatási jegyek kezelése.|
|**Nem műveletek**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|Nem sikerült a megosztott elérési kulcsok beolvasása.|

### <a name="monitoring-contributor"></a>Közreműködő figyelése

A figyelési munkatárs összes figyelési adatot olvashatják és monitoringbeállítások frissítése. Az alábbi táblázat a szerepkörhöz rendelt engedélyek:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|* / olvasása|Olvassa el az erőforrásokat bármilyen típusú, kivéve a titkos kulcsok.|
|Microsoft.AlertsManagement/alerts/*|Riasztások kezelése.|
|Microsoft.AlertsManagement/alertsSummary/*|A riasztási irányítópult kezelése.|
|Microsoft.Insights/AlertRules/*|Riasztási szabályok kezelése.|
|Microsoft.Insights/components/*|Az Application Insights összetevők kezeléséhez.|
|Microsoft.Insights/DiagnosticSettings/*|A diagnosztikai beállítások kezelését.|
|Microsoft.Insights/eventtypes/*|Tevékenységnapló események (felügyeleti események) egy előfizetésben listában. Ezzel az engedéllyel csak a műveletnapló programozott és a portál eléréséhez alkalmazható.|
|Microsoft.Insights/LogDefinitions/*|Ez az engedély szükség a felhasználók számára, akiknek tevékenységi naplóit a portálon hozzáférésre van szükségük. Tevékenységnapló napló kategóriák listája.|
|Microsoft.Insights/MetricDefinitions/*|Olvassa el a metrikai meghatározásainak (erőforrás elérhető metrika típusok listája).|
|Microsoft.Insights/Metrics/*|Egy erőforrás olvasni.|
|Microsoft.Insights/Register/Action|Regisztrálja a Microsoft.Insights szolgáltatót.|
|Microsoft.Insights/webtests/*|Az Application Insights webalkalmazás-tesztek kezelése.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|A Naplóelemzési megoldás csomagjainak kezelését.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Mentett keresések Naplóelemzési kezelése.|
|Microsoft.OperationalInsights/workspaces/search/action|Keresse meg a Naplóelemzési munkaterület.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|A Naplóelemzési munkaterület kulcsainak listázása.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Log Analytics insight tárolókonfigurációkkal kezelése.|
|Microsoft.Support/*|Hozzon létre és támogatási jegyek kezelése.|
|Microsoft.WorkloadMonitor/workloads/*|Munkaterhelések kezeléséhez.|

### <a name="monitoring-reader"></a>Olvasó figyelése

A figyelés olvasó összes figyelési adatot tud olvasni. Az alábbi táblázat a szerepkörhöz rendelt engedélyek:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|* / olvasása|Olvassa el az erőforrásokat bármilyen típusú, kivéve a titkos kulcsok.|
|Microsoft.OperationalInsights/workspaces/search/action|Keresse meg a Naplóelemzési munkaterület.|
|Microsoft.Support/*|Hozzon létre és támogatási jegyek kezelése|

### <a name="user-access-administrator"></a>Felhasználói hozzáférés rendszergazdája

A felhasználói hozzáférés adminisztrátora kezelheti az Azure-erőforrások felhasználói eléréséhez. Az alábbi táblázat a szerepkörhöz rendelt engedélyek:

|**Műveletek**  |**Leírás**  |
|---------|---------|
|* / olvasása|Minden erőforrás olvasása|
|Microsoft.Authorization/*|Engedélyezési kezelése|
|Microsoft.Support/*|Hozzon létre és támogatási jegyek kezelése|

## <a name="onboarding"></a>Előkészítés

Az alábbi táblázatok megjelenítése a változáskövetési bevezetési virtuális gépekhez szükséges minimális szükséges engedélyekkel, vagy frissítse a megoldásokat.

### <a name="onboarding-from-a-virtual-machine"></a>A virtuális gép bevezetése

|**Művelet**  |**Engedély**  |**Minimális hatókör**  |
|---------|---------|---------|
|Új központi telepítési írása      | Microsoft.Resources/deployments/*          |Előfizetés          |
|Új erőforráscsoport írása      | Microsoft.Resources/subscriptions/resourceGroups/write        | Előfizetés          |
|Hozzon létre új alapértelmezett munkaterület      | Microsoft.OperationalInsights/workspaces/write         | Erőforráscsoport         |
|Új fiók létrehozása      |  Microsoft.Automation/automationAccounts/write        |Erőforráscsoport         |
|Hivatkozás munkaterületet, és a fiók      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Munkaterület</br>Automation-fiók
|Megoldás létrehozása      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write |Erőforráscsoport          |
|Hozzon létre MMA bővítmény      | Microsoft.Compute/virtualMachines/write         | Virtuális gép         |
|Mentett keresés létrehozása      | Microsoft.OperationalInsights/workspaces/write          | Munkaterület         |
|Hatókör-konfiguráció létrehozása      | Microsoft.OperationalInsights/workspaces/write          | Munkaterület         |
|Hatókör-config hivatkozás megoldás      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Megoldás         |
|Bevezetési állapot ellenőrzése - olvasási munkaterület      | Microsoft.OperationalInsights/workspaces/read         | Munkaterület         |
|Bevezetési állapot ellenőrzése - olvasási társított munkaterület tulajdonság fiók     | Microsoft.Automation/automationAccounts/read      | Automation-fiók        |
|Bevezetési állapot ellenőrzése - olvasási megoldás      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Megoldás         |
|Bevezetési állapot ellenőrzése - olvasási méretű VM      | Microsoft.Compute/virtualMachines/read         | Virtuális gép         |
|Ellenőrizze a bevezetési állapot - olvasási      | Microsoft.Automation/automationAccounts/read  |  Automation-fiók   |

### <a name="onboarding-from-automation-account"></a>Bevezetés az Automation-fiók

|**Művelet**  |**Engedély** |**Minimális hatókör**  |
|---------|---------|---------|
|Hozzon létre új központi telepítést     | Microsoft.Resources/deployments/*        | Előfizetés         |
|Új erőforráscsoport létrehozása     | Microsoft.Resources/subscriptions/resourceGroups/write         | Előfizetés        |
|AutomationOnboarding panelről – új munkaterület létrehozása     |Microsoft.OperationalInsights/workspaces/write           | Erőforráscsoport        |
|AutomationOnboarding panelről – olvassa el a csatolt munkaterület     | Microsoft.Automation/automationAccounts/read        | Automation-fiók       |
|AutomationOnboarding panelről – olvassa el a megoldás     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Megoldás        |
|AutomationOnboarding panelről – olvassa el a munkaterület     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Munkaterület        |
|Hivatkozás munkaterület és fiók létrehozása     | Microsoft.OperationalInsights/workspaces/write        | Munkaterület        |
|Írási shoebox fiók      | Microsoft.Automation/automationAccounts/write        | Fiók        |
|Megoldás létrehozása      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write        | Erőforráscsoport         |
|Létrehozása/szerkesztése mentett keresés     | Microsoft.OperationalInsights/workspaces/write        | Munkaterület        |
|Hatókör-konfiguráció létrehozása/szerkesztése     | Microsoft.OperationalInsights/workspaces/write        | Munkaterület        |
|Hatókör-config hivatkozás megoldás      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Megoldás         |
|**2 - bevezetésében lépés több virtuális géphez**     |         |         |
|VMOnboarding panelről – MMA létrehozása bővítmény     | Microsoft.Compute/virtualMachines/write           | Virtuális gép        |
|Létrehozása / szerkesztése a mentett keresés     | Microsoft.OperationalInsights/workspaces/write           | Munkaterület        |
|Hozzon létre / config hatókör szerkesztése  | Microsoft.OperationalInsights/workspaces/write   | Munkaterület|

## <a name="update-management"></a>Frissítéskezelés

Frissítéskezelés eléri több szolgáltatásban biztosítani a szolgáltatást. Az alábbi táblázat a frissítés felügyeleti központi telepítések felügyeletéhez szükséges engedélyeket:

|**Erőforrás**  |**Szerepkör**  |**Hatókör**  |
|---------|---------|---------|
|Automation-fiók     | Log Analytics közreműködő       | Automation-fiók        |
|Automation-fiók    | Virtuális gépek közreműködője        | A fiókhoz tartozó erőforráscsoport        |
|Log Analytics-munkaterület     | Log Analytics közreműködő| Log Analytics-munkaterület        |
|Log Analytics-munkaterület |Log Analytics olvasó| Előfizetés|
|Megoldás     |Log Analytics közreműködő         | Megoldás|
|Virtuális gép     | Virtuális gépek közreműködője        | Virtuális gép        |

## <a name="configure-rbac-for-your-automation-account"></a>Az RBAC konfigurálása az Automation-fiók

A következő szakasz bemutatja, hogyan RBAC konfigurálása az Automation-fiók keresztül a [portal](#configure-rbac-using-the-azure-portal) és [PowerShell](#configure-rbac-using-powershell)

### <a name="configure-rbac-using-the-azure-portal"></a>Az Azure portál használatával RBAC konfigurálása

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/), és nyissa meg az Automation-fiókját az Automation-fiókok lapról.
2. Kattintson a **hozzáférés-vezérlés (IAM)** vezérlő bal felső sarokban lévő. Ekkor megnyílik a **hozzáférés-vezérlés (IAM)** lap, ahol és vehet fel új felhasználók, csoportok, alkalmazásokat kezeléséhez az automatizálási fiókot, és megtekintheti a meglévő szerepkörök, amelyek képesek az Automation-fiókhoz.

   ![Hozzáférés gomb](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Új felhasználó hozzáadása és szerepkör hozzárendelése

1. Az a **hozzáférés-vezérlés (IAM)** kattintson **+ Hozzáadás** megnyitásához a **engedélyek hozzáadása** lap, ahol hozzáadhat egy felhasználó, csoport vagy alkalmazás, és szerepkör hozzárendelése.

2. Válasszon egy szerepkört az elérhető szerepkörök listájáról. Választhat, a rendelkezésre álló beépített szerepkörökben, amely támogatja az Automation-fiók vagy bármilyen definiált egyéni biztonsági szerepkört.

3. Írja be az engedélyt a kívánt felhasználó felhasználóneve a **válasszon** mező. Válassza ki a felhasználót a listából, és kattintson a **mentése**.

   ![Felhasználók hozzáadása](media/automation-role-based-access-control/automation-04-add-users.png)

   Most látnia kell hozzáadni a felhasználót a **felhasználók** , a kijelölt szerepkörhöz hozzárendelt lap

   ![Felhasználók listázása](media/automation-role-based-access-control/automation-05-list-users.png)

   A felhasználóhoz a **Szerepkörök** lapról is hozzárendelhet szerepkört.
4. Kattintson a **szerepkörök** a a **hozzáférés-vezérlés (IAM)** lapon nyissa meg a **szerepkörök** lap. Itt megtekintheti a szerepkör nevét, és az adott szerepkörhöz rendelt felhasználók és csoportok számát.

    ![Szerepkör hozzárendelése a Felhasználók lapról](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > Szerepköralapú hozzáférés-vezérlés csak akkor állítható be, az automatizálási fiók hatókörének és nem minden erőforrás az Automation-fiók alatt.

#### <a name="remove-a-user"></a>Felhasználó eltávolítása

Eltávolíthatja a felhasználók, akik nem kezel az Automation-fiók, vagy akik többé már nem a vállalat számára tartozó hozzáférési engedélyek. Alább láthatók egy felhasználó eltávolításának lépései:

1. Az a **hozzáférés-vezérlés (IAM)** lapon, válassza ki a felhasználói szeretné eltávolítani, és kattintson a **eltávolítása**.
2. A hozzárendelés részleteit megjelenítő lapon kattintson az **Eltávolítás** gombra.
3. Az **Igen** gombra kattintva erősítse meg az eltávolítást.

   ![Felhasználók eltávolítása](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-rbac-using-powershell"></a>PowerShell-lel RBAC konfigurálása

Szerepköralapú hozzáférés-is beállítható úgy, hogy az alábbi Automation-fiók [Azure PowerShell-parancsmagok](../role-based-access-control/role-assignments-powershell.md):

[Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) elérhető az Azure Active Directory minden RBAC-szerepkörök listája. Ezt a parancsot használhatja a **Name** (Név) tulajdonsággal, ha fel szeretné sorolni az összes műveletet, amelyet egy adott szerepkörrel lehet elvégezni.

```azurepowershell-interactive
Get-AzureRmRoleDefinition -Name 'Automation Operator'
```

A következő egy példa a kimenetre:

```azurepowershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action,
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
```

[Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) sorolja fel az Azure AD RBAC szerepkör-hozzárendelések a megadott hatókörben. Paraméterek nélkül ez a parancs az előfizetéshez tartozó összes szerepkör-hozzárendelést visszaadja. Az **ExpandPrincipalGroups** paraméter segítségével felsorolhatja egy adott felhasználó vagy a felhasználó csoportjának összes hozzáférés-hozzárendelését.
    **Példa:** Az alábbi paranccsal felsorolhatja az összes felhasználót és azok szerepkörét egy Automation-fiókon belül.

```azurepowershell-interactive
Get-AzureRMRoleAssignment -scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

A következő egy példa a kimenetre:

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/cc594d39-ac10-46c4-9505-f182a355c41f
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : 15f26a47-812d-489a-8197-3d4853558347
ObjectType         : User
```

[Új AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) hozzáférés hozzárendelése a felhasználók, csoportok és alkalmazások egy adott hatókörhöz.
    **Példa:** az Automation-fiók hatókörének felhasználójának a "Automation-kezelő" szerepkör hozzárendeléséhez a következő paranccsal.

```azurepowershell-interactive
New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

A következő egy példa a kimenetre:

```azurepowershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/25377770-561e-4496-8b4f-7cba1d6fa346
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : f5ecbe87-1181-43d2-88d5-a8f5e9d8014e
ObjectType         : User
```

Használjon [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) hozzáférést egy adott felhasználó, csoport vagy alkalmazás eltávolítása egy adott hatókör.
    **Példa:** távolítsa el a felhasználót az Automation-fiók hatókörének a "Automation-kezelő" szerepkör az alábbi parancs segítségével.

```azurepowershell-interactive
Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Cserélje le a fenti példákban **bejelentkezés Id**, **előfizetési azonosító**, **erőforráscsoport-név**, és **Automation-fiók neve** rendelkező a fiók adatait. Ha a rendszer rákérdez, hogy tényleg szeretné-e törölni a szerepkör-hozzárendelést, kattintson az **Igen** gombra.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>A felhasználó számára az Automation operátori szerepkör - Automation-fiók

Ha egy felhasználó, aki hozzá van rendelve az Automation operátor szerepkör az Automation-fiók hatókörben vannak rendelve az Automation-fiók, csak adatmegjelenítésre runbookok, runbook-feladatok listája megtekintése és létrehozott az automatizálási ütemezések fiók, de nem tekintheti meg a definíciója. Elindíthatják, leállíthatják, felfüggeszthetik, folytathatják és ütemezhetik a runbook-feladatokat. A felhasználó nem férhet hozzá más automatizálási erőforrások, például konfigurációk, hibrid dolgozó csoportok vagy DSC-csomópontok.

![Nincsenek erőforrások elérésére](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-rbac-for-runbooks"></a>A Runbookok RBAC konfigurálása

Azure Automation szolgáltatásbeli lehetővé teszi, hogy bizonyos runbookokra RBAC hozzárendelése. Ehhez futtassa a következő parancsfájl egy felhasználó hozzáadása egy adott runbook. Az alábbi parancsfájlt is futott, egy automatizálási fiókot rendszergazdai vagy bérlői rendszergazda segítségét.

```azurepowershell-interactive
$rgName = "<Resource Group Name>" # Resource Group name for the Automation Account
$automationAccountName ="<Automation Account Name>" # Name of the Automation Account
$rbName = "<Name of Runbook>" # Name of the runbook
$userId = "<User ObjectId>" # Azure Active Directory (AAD) user's ObjectId from the directory

# Gets the Automation Account resource
$aa = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts" -ResourceName $automationAccountName

# Get the Runbook resource
$rb = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts/runbooks" -ResourceName "$automationAccountName/$rbName"

# The Automation Job Operator role only needs to be ran once per user.
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Job Operator" -Scope $aa.ResourceId

# Adds the user to the Automation Runbook Operator role to the Runbook scope
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Runbook Operator" -Scope $rb.ResourceId
```

Egyszer lefutott, jelentkezzen be az Azure-portálon, tekintse meg a felhasználó **összes erőforrás**. A listában akkor jelenik meg a Runbook addig adták hozzá, mint egy **automatizálási Runbook kezelő** számára.

![Runbook RBAC a portálon](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>A felhasználó számára Automation operátori szerepkör - forgatókönyv

Ha egy felhasználó, akinek ki lett osztva a Runbook hatókör nézetek hozzárendeli a Runbook Automation operátor szerepköre, hogy csak elindítja a runbookot, és tekintse meg a runbook feladatokat.

![Csak a start hozzáféréssel rendelkezik](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>További lépések

* Az RBAC for Azure Automation konfigurálásának különböző módjaira vonatkozó további információért lásd [az RBAC Azure PowerShellel folytatott kezelésével](../role-based-access-control/role-assignments-powershell.md) foglalkozó témakört.
* További információk a forgatókönyvek elindításának különböző módjairól: [Forgatókönyv elindítása](automation-starting-a-runbook.md).
* További információk a különböző forgatókönyv-típusokról: [Az Azure Automation forgatókönyveinek típusai](automation-runbook-types.md)
