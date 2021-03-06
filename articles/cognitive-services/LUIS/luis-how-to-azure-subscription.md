---
title: Azure-végpont előfizetés kezelése |} A Microsoft Docs
description: Ebben a cikkben a LUIS-fiókját, hogy adja meg a végponthoz a következő fizetési terv korlátlan adatforgalom mért végponthoz kulcsot hoz létre.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: diberry
ms.openlocfilehash: 0b735499ae589e44c2ce5076fce38ec47ddd69c7
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223279"
---
# <a name="manage-azure-endpoint-subscription-keys"></a>Azure-végpont előfizetési kulcsok kezelése

Teszteléshez és prototípus csak az ingyenes (F0) csomagot használja. Éles rendszerek esetén használhatja a [fizetős](https://aka.ms/luis-price-tier) szint. 

> [!NOTE]
> Ne használja a [kulcs létrehozási](luis-concept-keys.md#authoring-key) végpont lekérdezések éles környezetben.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>A LUIS végponti kulcs létrehozása

1. Jelentkezzen be a  **[Microsoft Azure](https://ms.portal.azure.com/)** 
2. Kattintson a zöld **+** jelentkezzen be a bal oldali panel felső, és keressen a "LUIS" a Marketplace-en, majd kattintson a **Language Understanding** , és kövesse a **élmény**  LUIS előfizetés fiók létrehozásához. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Konfigurálja az előfizetés fiók nevét, például beállításokkal árképzési szint stb. 

    ![Az Azure API kiválasztása](./media/luis-azure-subscription/azure-api-choice.png) 

4. Miután létrehozta az intelligens HANGFELISMERÉSI szolgáltatás, megtekintheti a létrehozott hozzáférési kulcsok **erőforrás-kezelés -> kulcsok**.  

    ![Az Azure-kulcsok](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > * Jelentkezzen be az Ön régiójában [LUIS](luis-reference-regions.md) webhely és [adja meg az új LUIS végponthoz kulcsot](luis-how-to-manage-keys.md#assign-endpoint-key). 
    > * Meg kell jegyeznie a név az Azure szolgáltatás hozott létre annak érdekében, hogy válassza ki azt a régióban lévő [LUIS](luis-reference-regions.md) közzéteszi a lapot.  

## <a name="change-luis-pricing-tier"></a>Módosítsa a LUIS-tarifacsomag

1.  A [Azure](https://portal.azure.com), keresse meg a LUIS-előfizetés. Kattintson a LUIS-előfizetésre.
    ![Keresse meg a LUIS-előfizetés](./media/luis-usage-tiers/find.png)
2.  Kattintson a **tarifacsomag** az elérhető tarifacsomagok láthatók. 
    ![Árképzési szintek megtekintése](./media/luis-usage-tiers/subscription.png)
3.  Kattintson a tarifacsomag, és kattintson a **kiválasztása** menteni a módosítást. 
    ![A LUIS fizetési szint módosítása](./media/luis-usage-tiers/plans.png)
4.  Ha a díjszabás módosítása befejeződött, egy előugró ablak ellenőrzi az új tarifacsomag. 
    ![A LUIS támogatási csomag ellenőrzése](./media/luis-usage-tiers/updated.png)
5. Ne felejtse el [rendelje hozzá a végpont kulcs](luis-how-to-manage-keys.md#assign-endpoint-key) a a **közzététel** lapon, és használhatja az összes endpoint lekérdezés. 

## <a name="exceed-pricing-tier-usage"></a>Díjszabási szint használatához lehet
Minden egyes csomaggal meghatározott arányban végpont kéréseket a LUIS-fiókjába. Ha a kérelmek száma nagyobb, mint a megengedett sebesség a díjköteles fiók percenként vagy havonta, a kérelmek, HTTP hibaüzenetet kap "429-es: túl sok kérelem."

Minden egyes csomaggal halmozódnak kérés havonta. Ha az összes kérelmet magasabb, mint az engedélyezett sebességét, a kérelmek, HTTP hibaüzenetet kap "403-as: tiltott".  

## <a name="viewing-summary-usage"></a>Összefoglaló használat megtekintése
A LUIS-használati adatokat megtekintheti az Azure-ban. A **áttekintése** lapon többek között a hívások és hibák legutóbbi összegző információit jeleníti meg. Ha Ön kérést egy LUIS végpontot, majd azonnal tekintse meg a **áttekintőlapján**, akár öt perc alatt jelenik meg a használat engedélyezése.

![Összefoglaló használat megtekintése](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Használati diagramok testreszabása
Metrikák az adatok részletesebb betekintést biztosít.

![Alapértelmezett mérőszámok](./media/luis-usage-tiers/metrics-default.png)

Konfigurálhatja a mérőszámdiagramok időszak és a metrika típusa. 

![Egyéni metrikák](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Összes tranzakciós küszöbértékének riasztási
Ha szeretné tudni, hogy ha egy bizonyos tranzakció küszöbértéket, például 10 000 tranzakció, elérte a riasztás is létrehozhat. 

![Alapértelmezett riasztások](./media/luis-usage-tiers/alert-default.png)

Metrikariasztás hozzáadása a **összes hívás** metrika egy bizonyos ideig. Adja hozzá, amelyek megkapják a riasztás minden személyek e-mail címét. Adja hozzá a webhookok az összes rendszer, amelyre a riasztást kapni. A riasztás akkor aktiválódik, ha egy logikai alkalmazást is futtathatja. 

## <a name="next-steps"></a>További lépések

Ismerje meg, hogyan használható [verziók](luis-how-to-manage-versions.md) a LUIS-alkalmazás kezeléséhez.