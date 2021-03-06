---
title: 'Oktatóanyag: Felhasználók automatikus átadása az Azure Active Directoryval Pingboard konfigurálása |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az Azure Active Directoryban történő automatikus kiépítésének és megszüntetésének Pingboard felhasználói fiókokat.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: c5cc20b4f20e3a4f746ab15aa5f139c8e7201a2c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447166"
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Oktatóanyag: Felhasználók automatikus átadása Pingboard konfigurálása

Ez az oktatóanyag célja, a lépéseket kell elvégeznie ahhoz, hogy az automatikus üzembe helyezést és megszüntetést felhasználói fiókok Azure Active Directory (Azure AD) származó, Pingboard mutatni.

## <a name="prerequisites"></a>Előfeltételek

Az ebben az oktatóanyagban ismertetett forgatókönyv feltételezi, hogy Ön már rendelkezik a következőkkel:

*   Az Azure AD-bérlő
*   A bérlő egy Pingboard [Pro-fiókja](https://pingboard.com/pricing) 
*   Pingboard rendszergazdai engedélyekkel rendelkező felhasználói fiók 

> [!NOTE] 
> Azure AD létesítési integrációs támaszkodik a [Pingboard API](`https://your_domain.pingboard.com/scim/v2`), amely érhető el a fiókjához.

## <a name="assign-users-to-pingboard"></a>Felhasználók hozzárendelése Pingboard

Az Azure AD egy fogalom, más néven "hozzárendeléseket" használatával határozza meg, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés. Felhasználók automatikus fióklétesítés kontextusában csak a az Azure AD-alkalmazáshoz hozzárendelt felhasználók szinkronizálása. 

Beállítása, és a kiépítési szolgáltatás engedélyezése előtt el kell döntenie, hogy mely felhasználók Azure AD-ben Pingboard alkalmazásához hozzá kell férniük. Ezután ezek a felhasználók az itt leírt utasításokat követve hozzárendelheti Pingboard alkalmazását:

[Felhasználó hozzárendelése egy vállalati alkalmazás](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Felhasználók hozzárendelése Pingboard fontos tippek

Azt javasoljuk, hogy hozzárendelje egyetlen Pingboard az üzembe helyezési konfiguráció tesztelése az Azure AD-felhasználót. További felhasználók később is hozzárendelhető.

## <a name="configure-user-provisioning-to-pingboard"></a>Felhasználói átadásának konfigurálása, Pingboard 

Ez a szakasz végigvezeti az Azure AD-csatlakozás üzembe helyezési API Pingboard felhasználói fiók. Is konfigurálhatja az eszközkiépítési szolgáltatás létrehozása, frissítése és hozzárendelt felhasználói fiókok a felhasználó hozzárendelések alapján az Azure ad-ben Pingboard letiltása.

> [!TIP]
> SAML-alapú egyszeri bejelentkezés az Pingboard engedélyezéséhez kövesse az utasításokat a [az Azure portal](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható az Automatikus kiépítés függetlenül azonban ezen két funkció kiegészíti egymást.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>Automatikus felhasználói fiók kiépítése Pingboard, az Azure AD konfigurálása

1. Az a [az Azure portal](https://portal.azure.com), keresse meg a **Azure Active Directory** > **vállalati alkalmazások** > **mindenalkalmazás** szakaszban.

1. Ha már konfigurálta az egyszeri bejelentkezés Pingboard, keresse meg a Pingboard példányát a keresőmező használatával. Ellenkező esetben válassza **Hozzáadás** és keressen rá a **Pingboard** az alkalmazás-katalógusában. Válassza ki **Pingboard** a keresési eredmények között, és adja hozzá az alkalmazások listáját.

1. Válassza ki a Pingboard példányát, és válassza a **kiépítési** fülre.

1. Állítsa be **Kiépítési mód** való **automatikus**.

    ![Pingboard kiépítése](./media/pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
1. Alatt a **rendszergazdai hitelesítő adataival** szakaszban, hajtsa végre az alábbi lépéseket:

    a. A **bérlői URL-cím**, adja meg `https://your_domain.pingboard.com/scim/v2`, és a "adott_tartomány" cserélje le tényleges tartományához.

    b. Jelentkezzen be a [Pingboard](https://pingboard.com/) a rendszergazdai fiók használatával.

    c. Válassza ki **bővítmények** > **Integrációk** > **az Azure Active Directory**.

    d. Nyissa meg a **konfigurálása** lapot, majd **engedélyezése a felhasználók átadásának az Azure-ból**.

    e. Másolja a tokent az **OAuth tulajdonosi jogkivonat**, írja be a **titkos jogkivonat**.

1. Az Azure Portalon válassza ki a **kapcsolat tesztelése** annak biztosítása érdekében az Azure AD csatlakozhat Pingboard alkalmazását. Ha a kapcsolat hibája esetén, győződjön meg arról, hogy a Pingboard rendszergazdai engedélyekkel rendelkezzen, majd próbálkozzon a **kapcsolat tesztelése** lépés újra.

1. Adja meg az e-mail-címét egy személy vagy csoport, amely az üzembe helyezési hiba értesítéseket kapni szeretné **értesítő e-mailt**. Jelölje be a jelölőnégyzetet alá.

1. Kattintson a **Mentés** gombra. 

1. Alatt a **leképezések** szakaszban jelölje be **szinkronizálása az Azure Active Directory-felhasználók a Pingboard**.

1. Az a **attribútumleképezések** területen tekintse át a felhasználói attribútumok Pingboard szinkronizálását az Azure ad-ből. A kiválasztott attribútumok **megfelelést kiváltó** tulajdonságok segítségével felel meg a frissítési műveletek Pingboard levő felhasználói fiókokat. Válassza ki **mentése** véglegesíti a módosításokat. További információkért lásd: [attribútumleképezések testreszabása felhasználókiépítés](../active-directory-saas-customizing-attribute-mappings.md).

1. Ahhoz, hogy az Azure AD létesítési szolgáltatás a Pingboard, az a **beállítások** területén módosíthatja **üzembe helyezési állapotra** való **a**.

1. Válassza ki **mentése** Pingboard rendelt felhasználók kezdeti szinkronizálás elindításához.

A kezdeti szinkronizálás végrehajtásához, mint az ezt követő szinkronizálások, amely körülbelül 40 percenként történik, amíg a szolgáltatás fut hosszabb időt vesz igénybe. Használja a **szinkronizálás részleteivel** előrehaladásának figyeléséhez, és kövesse a hivatkozásokat tevékenységeket tartalmazó naplók kiépítés szakaszt. A naplók Pingboard alkalmazásban a kiépítési szolgáltatás által végrehajtott összes műveletet ismerteti.

Az Azure AD létesítési naplók olvasása további információkért lásd: [automatikus felhasználói fiók üzembe helyezésének jelentést](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése a vállalati alkalmazások kezelése](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)
* [Egyszeri bejelentkezés konfigurálása](pingboard-tutorial.md)
