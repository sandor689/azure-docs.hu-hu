---
title: Használat és számlázás az Azure Stackhez, egy Felhőszolgáltató kezelése |} A Microsoft Docs
description: Regisztrálás az Azure Stack, egy Felhőbeli Felhőszolgáltató (CSP) és a számlázási ügyfelek hozzáadása példakóddal.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: brenduns
ms.reviewer: alfredo
ms.openlocfilehash: 66bab9f4b5099099039aa0f60cd86ef23e73cd1f
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715685"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Használat és számlázás az Azure Stack felhő szolgáltató kezelése 

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek*

Ebben a cikkben bemutatjuk, hogyan regisztrálja az Azure Stack, egy Felhőbeli Felhőszolgáltató (CSP) és felhasználók hozzáadása.

Mint egy CSP-hez használható különféle ügyfelek, az Azure Stack használatával. Minden ügyfél az Azure-ban rendelkezik a CSP-előfizetésekben. Minden felhasználói előfizetésben az Azure Stack használatának közvetlen kell.

Az alábbi ábrán látható, válassza ki a megosztott szolgáltatások fiókját, és regisztrálja az Azure-fiók az Azure Stack-fiókkal kell lépéseit. Regisztrált, segítségével készítheti elő a végfelhasználókat.

**Lépések hozzáadása a használat nyomon követése egy CSP-hez**

![Folyamat használat és a Felhőbeli szolgáltató felügyeletének engedélyezéséhez.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## <a name="create-a-csp-or-cspss-subscription"></a>CSP vagy CSPSS előfizetés létrehozása

### <a name="cloud-service-provider-subscription-types"></a>Szolgáltatói előfizetés felhőtípusok

Válassza ki az Azure Stackhez készült használt fióknak a megosztott szolgáltatások kell. Használható a regisztrációhoz egy több-bérlős Azure Stack-előfizetést a típusok a következők:

 - Felhőszolgáltató 
 - Megosztott szolgáltatások típusú partneri előfizetéshez 

#### <a name="csp-shared-services"></a>Kriptográfiai Szolgáltató megosztott szolgáltatások

Cloud Service Provider megosztott szolgáltatások (CSPSS) előfizetések a regisztráció során egy közvetlen CSP előnyben részesített választás vagy CSP terjesztő működik az Azure Stack.

Megosztott szolgáltatások bérlő CSPSS előfizetések tartoznak. Ha regisztrálja az Azure Stack, meg kell adnia a hitelesítő adatokat, amely az előfizetés tulajdonosa. A fiókja, regisztrálhat az Azure Stack eltérhetnek az üzembe helyezéshez használt rendszergazdai fiók. Továbbá, hajtsa végre a két fiók *nem* kell tartozniuk ugyanabban a tartományban. Más szóval akkor előfordulhat, hogy az üzembe helyezés, amelyek már használják a bérlő. Például akkor lehet, hogy ContosoCSP.onmicrosoft.com használja, majd regisztráljon egy másik bérlőben, például IURContosoCSP.onmicrosoft.com. Ne feledje, hogy jelentkezzen be ContosoCSP.onmicrosoft.com nap-ne az Azure Stack felügyeleti ekkor kell. Amikor bejelentkezik az Azure-ban IURContosoCSP.onmicrosoft.com, amikor szüksége van regisztrációs műveletek végrehajtásához.

Tekintse meg a következő útmutatást, valamint CSPSS előfizetés leírását előfizetés létrehozásával [hozzáadása az Azure partneri megosztott szolgáltatások](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP-előfizetésekben

Cloud Service Provider (CSP) előfizetések a regisztráció során a CSP-viszonteladó előnyben részesített választás, vagy egy végfelhasználói működik az Azure Stack.

## <a name="register-azure-stack"></a>Az Azure Stack regisztrálása

Regisztráljon az Azure Stack használatával, lásd: [regisztrálása az Azure Stack az Azure-előfizetésében](azure-stack-registration.md).

## <a name="add-end-customer"></a>Adja hozzá a végfelhasználó ügyfél

Azure Stack beállítása úgy, hogy egy új bérlőt erőforrást használ, használatuk fog jelenteni lehet a Felhőszolgáltató (CSP) előfizetést: [hozzáadása bérlői használat és számlázás az Azure Stackhez](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>A megfelelő előfizetések díja szerint számítjuk fel

Az Azure Stack regisztrációs nevű szolgáltatást használja. A regisztráció az Azure-ban tárolt objektum. A regisztrációs objektum dokumentumok mely Azure-előfizetés(ek) díja szerint számítjuk fel az egy adott Azure Stack használatával. Ez a szakasz azon regisztrációs fontosságát.

Regisztráció az Azure Stack használatával:
 - Azure Stack-használati adatok továbbítása az Azure kereskedelmi és számlázás az Azure-előfizetéssel.
 - Minden ügyfél használati jelentést egy másik előfizetésben, az Azure Stack több-bérlős telepítésekben. Több-bérlős módhoz lehetővé teszi, hogy az Azure Stack használatával támogatja a különböző szervezetek ugyanazon az Azure Stack-példányon.

Minden egyes Azure stack van egy alapértelmezett előfizetést, és számos bérlői előfizetések. Az alapértelmezett előfizetést az Azure-előfizetéssel, amely akkor kell fizetnie, ha bérlőspecifikus előfizetés nem létezik. Az első az előfizetés regisztrálva kell lennie. A jelentéskészítés működéséhez a több-bérlős használatot az előfizetés CSP vagy CSPSS előfizetést kell lennie.

Ezt követően a regisztráció frissül az Azure-előfizetés minden bérlő, amelyet szeretne az Azure Stack használata. Bérlői előfizetések a CSP-típusúnak kell lennie, és a partnerrel, aki az alapértelmezett előfizetés tulajdonosa összesítő kell. Más szóval valaki más ügyfelek nem lehet regisztrálni.

Azure Stack globális Azure-bA továbbítja a használati adatokat, amikor Azure egy szolgáltatása, consults szintűre frissül a regisztrációt, és minden bérlő használati leképezi a megfelelő bérlői előfizetéshez. Ha a bérlő nincs regisztrálva, az alapértelmezett előfizetést az Azure Stack-példányhoz jut, hogy használat kerül.

Mivel bérlői előfizetések CSP-előfizetésekben, számlán küld a CSP-partner, és használati adatok nem látható a végfelhasználók számára.

## <a name="next-steps"></a>További lépések

 - A CSP program kapcsolatos további információkért lásd: [Cloud Solution Provider program](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
 - Erőforrás-használati adatok lekérése az Azure Stack kapcsolatos további tudnivalókért lásd: [használat és számlázás az Azure Stackben](azure-stack-billing-and-chargeback.md).
