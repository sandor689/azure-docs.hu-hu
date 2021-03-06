---
title: Azure Automation-fiók kezelése
description: Ez a cikk azt ismerteti, hogyan kezelheti az Automation-fiók konfigurációját, például hogyan újíthatja meg és törölheti a tanúsítványokat, és mit tehet hibás konfiguráció esetén.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5fbdccf4e14ce1201b21f0490e9c890c77c3e2f0
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39577755"
---
# <a name="manage-azure-automation-account"></a>Azure Automation-fiók kezelése
Az Automation-fiók lejárata előtt meg kell újítania a tanúsítványt. Ha úgy véli, hogy a futtató fiók biztonsága sérült, akkor törölheti, majd újra létrehozhatja a fiókot. Ebben a részben ezeknek a műveleteknek a végrehajtását ismertetjük.

## <a name="cert-renewal"></a>Önaláírt tanúsítvány megújítása
A futtató fiókhoz létrehozott önaláírt tanúsítvány a létrehozás dátumától számítva egy év múlva jár le. A tanúsítványt bármikor meg lehet újítani a lejárata előtt. A megújításkor a jelenleg érvényes tanúsítványt megőrzi a rendszer, hogy a művelet ne érintse hátrányosan a várólistán lévő vagy aktívan futó runbookokat, amelyek hitelesítést végeznek a futtató fiókkal. A tanúsítvány a lejárati dátumáig érvényes marad.

> [!NOTE]
> Ha úgy konfigurálta az Automation futtató fiókot, hogy a vállalati hitelesítésszolgáltató által kibocsátott tanúsítványt használja, és ezt a lehetőséget alkalmazza, a vállalati tanúsítvány egy önaláírt tanúsítványra lesz lecserélve.

A tanúsítvány megújításához tegye a következőket:

1. Az Azure Portalon nyissa meg az Automation-fiókot.

2. Az **Automation-fiók** panelen 
3. a **Fióktulajdonságok** ablaktábla **Fiókbeállítások** területén válassza a **Futtató fiókok** lehetőséget.

    ![Az Automation-fiók tulajdonságpanelje](media/automation-manage-account/automation-account-properties-pane.png)
3. A **Futtató fiókok** tulajdonságlapján válassza ki azt a futtató fiókot vagy klasszikus futtató fiókot, amelynek a tanúsítványát meg kívánja újítani.

4. A kiválasztott fiók **Tulajdonságok** paneljén kattintson a **Tanúsítvány megújítása** elemre.

    ![Futtató fiók tanúsítványának megújítása](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. A tanúsítvány megújítása során a menü **Értesítések** részén nyomon követheti a folyamat állapotát.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Futtató fiók vagy klasszikus futtató fiók törlése
Ez a témakör ismerteti, hogyan törölhet és hozhat újra létre futtató fiókot vagy klasszikus futtató fiókot. A művelet során a rendszer megőrzi az Automation-fiókot. A törölt futtató fiókot vagy klasszikus futtató fiókot újra létrehozhatja az Azure Portalon.

1. Az Azure Portalon nyissa meg az Automation-fiókot.

2. Az **Automation-fiók** oldalon válassza a **Futtató fiókok** lehetőséget.

3. A **Futtató fiókok** tulajdonságlapján válassza ki azt a futtató fiókot vagy klasszikus futtató fiókot, amelyet törölni kíván. Ezt követően a kiválasztott fiók **Tulajdonságok** paneljén kattintson a **Törlés** elemre.

 ![Futtató fiók törlése](media/automation-manage-account/automation-account-delete-runas.png)

4. A fiók törlése során a menü **Értesítések** részén nyomon követheti a folyamat állapotát.

5. A törlés után újra létrehozhatja a fiókot a **Futtató fiókok** tulajdonságlapon az **Azure-alapú futtató fiók** lehetőség kiválasztásával.

 ![Automation futtató fiók újbóli létrehozása](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>Hibás konfiguráció
Előfordulhat, hogy a futtató fiók vagy klasszikus futtató fiók megfelelő működéséhez szükséges valamelyik konfigurációs elem törlődött, illetve nem megfelelően hozták létre az első beállításkor. Ilyen elemek például a következők:

* Tanúsítványobjektum
* Kapcsolatobjektum
* A futtató fiók el lett távolítva a közreműködői szerepkörből
* Egyszerű szolgáltatás vagy alkalmazás az Azure AD-ben

Az előző és más hibás konfigurációk esetében az Automation-fiók észleli a módosításokat, és *Hiányos* állapotot jelez a fiók **Futtató fiókok** tulajdonságpaneljén.

![Hiányos futtatófiók-konfigurációs állapot](media/automation-manage-account/automation-account-runas-incomplete-config.png)

Ha kiválasztja a futtató fiókot, a fiók **Tulajdonságok** paneljén a következő hibaüzenet jelenik meg:

![Hiányos futtatófiók-konfigurációra figyelmeztető üzenet](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png).

A futtató fiókkal kapcsolatos hasonló problémákat gyorsan elháríthatja a fiók törlésével és ismételt létrehozásával.

## <a name="next-steps"></a>További lépések
* Az Egyszerű szolgáltatásokkal kapcsolatos további információkért lásd: [Alkalmazásobjektumok és egyszerű szolgáltatási objektumok](../active-directory/develop/app-objects-and-service-principals.md).
* Az Azure Automation szerepköralapú hozzáférés-vezérlési funkciójáról további információkért lásd: [Az Azure Automation szerepköralapú hozzáférés-vezérlése](automation-role-based-access-control.md).
* A tanúsítványokkal és az Azure-szolgáltatásokkal kapcsolatos részletes információkért lásd: [Tanúsítványok áttekintése az Azure Cloud Servicesben](../cloud-services/cloud-services-certs-create.md).