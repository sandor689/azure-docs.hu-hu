---
title: 'Oktatóanyag: Azure Active Directory-integráció az Getabstract |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és Getabstract között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 2b63d048-b529-4fad-9e90-f244323409dd
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: 1bb43f65bd77315be398a9c22e7fc1500de07754
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424820"
---
# <a name="tutorial-azure-active-directory-integration-with-getabstract"></a>Oktatóanyag: Azure Active Directory-integráció az Getabstract

Ebben az oktatóanyagban elsajátíthatja, hogyan Getabstract integrálása az Azure Active Directory (Azure AD).

Getabstract integrálása az Azure ad-ben nyújt a következő előnyökkel jár:

- Szabályozhatja, ki férhet hozzá Getabstract Azure AD-ben.
- Engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett Getabstract (egyszeri bejelentkezés), az Azure AD-fiókjukat.
- A fiókok egyetlen központi helyen – az Azure Portalon kezelheti.

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Getabstract az Azure AD-integráció konfigurálásához a következőkre van szükség:

- Az Azure AD-előfizetéshez
- Egy Getabstract egyszeri bejelentkezéses engedélyezett előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. Getabstract hozzáadása a katalógusból
1. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-getabstract-from-the-gallery"></a>Getabstract hozzáadása a katalógusból
Az Azure AD integrálása a Getabstract konfigurálásához hozzá kell Getabstract a katalógusból a felügyelt SaaS-alkalmazások listájára.

**Getabstract hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Az Azure Active Directory gomb][1]

1. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![A vállalati alkalmazások panelen][2]
    
1. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Az új alkalmazás gomb][3]

1. A Keresés mezőbe írja be a **Getabstract**válassza **Getabstract** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az eredmények listájában Getabstract](./media/getabstract-tutorial/tutorial_getabstract_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés Getabstract a teszt "Britta Simon" nevű felhasználó.

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó Getabstract mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó Getabstract hivatkozás kapcsolata kell létrehozni.

Getabstract, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezés az Getabstract tesztelése és konfigurálása, hogy hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
1. **[Hozzon létre egy Azure ad-ben tesztfelhasználót](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
1. **[Hozzon létre egy Getabstract tesztfelhasználót](#create-a-getabstract-test-user)**  – egy megfelelője a Britta Simon Getabstract, amely a felhasználó Azure ad-ben ábrázolása van csatolva van.
1. **[Rendelje hozzá az Azure ad-ben tesztfelhasználó](#assign-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
1. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és Getabstract alkalmazását az egyszeri bejelentkezés konfigurálása.

**Szeretné konfigurálni az Azure AD egyszeri bejelentkezés Getabstract, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **Getabstract** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezési hivatkozás konfigurálása][4]

1. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezési párbeszédpanel](./media/getabstract-tutorial/tutorial_getabstract_samlbase.png)

1. Az a **Getabstract tartomány és URL-címek** területén kövesse az alábbi lépéseket, ha az alkalmazás a konfigurálni kívánt **Identitásszolgáltató** kezdeményezett mód:

    ![Getabstract tartomány és URL-címeket egyetlen bejelentkezési adatait](./media/getabstract-tutorial/tutorial_getabstract_url.png)

    a. Az a **azonosító** szövegmezőbe írja be az URL-cím:

    A fázis/pre_production: `https://int.getabstract.com`

    Éles környezetben: `https://www.getabstract.com`

    b. Az a **válasz URL-cím** szövegmezőbe írja be az URL-cím:
    
    A fázis/pre_production: `https://int.getabstract.com/ACS.do`
    
    Éles környezetben: `https://www.getabstract.com/ACS.do`

1. Ellenőrizze **speciális URL-beállítások megjelenítése** , és hajtsa végre a következő lépést, ha az alkalmazás a konfigurálni kívánt **SP** kezdeményezett mód:

    ![Getabstract tartomány és URL-címeket egyetlen bejelentkezési adatait](./media/getabstract-tutorial/tutorial_getabstract_url1.png)

    Az a **bejelentkezési URL-** szövegmezőbe írja be a következő minta használatával URL-címe:
    
    A fázis/pre_production: `https://int.getabstract.com/portal/<org_username>`
    
    Éles környezetben: `https://www.getabstract.com/portal/<org_username>`

    > [!NOTE] 
    > Az érték nem valódi. Ez az érték frissítse a tényleges bejelentkezési URL-CÍMÉT. Kapcsolattartó [Getabstract ügyfél-támogatási csapatának](https://www.getabstract.com/en/contact) lekérni ezt az értéket.

1. Az a **SAML-aláíró tanúsítvány** területén kattintson **metaadatainak XML** , és mentse a metaadat-fájlt a számítógépen.

    ![A tanúsítvány letöltési hivatkozás](./media/getabstract-tutorial/tutorial_getabstract_certificate.png) 

1. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gomb konfigurálása](./media/getabstract-tutorial/tutorial_general_400.png)
    
1. Az egyszeri bejelentkezés konfigurálása **Getabstract** oldalon kell küldenie a letöltött **metaadatainak XML** való [Getabstract támogatási csapatának](https://www.getabstract.com/en/contact). Akkor állítsa ezt a beállítást, hogy a SAML SSO-kapcsolat megfelelően állítsa be mindkét oldalon.

> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure ad-ben tesztfelhasználó számára

Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

   ![Hozzon létre egy Azure ad-ben tesztfelhasználó számára][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon, a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.

    ![Az Azure Active Directory gomb](./media/getabstract-tutorial/create_aaduser_01.png)

1. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/getabstract-tutorial/create_aaduser_02.png)

1. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** felső részén a **minden felhasználó** párbeszédpanel bezárásához.

    ![A Hozzáadás gombra.](./media/getabstract-tutorial/create_aaduser_03.png)

1. Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![A felhasználó párbeszédpanel](./media/getabstract-tutorial/create_aaduser_04.png)

    a. Az a **neve** mezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** mezőbe írja be a felhasználó Britta Simon e-mail-címét.

    c. Válassza ki a **jelszó megjelenítése** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-getabstract-test-user"></a>Getabstract tesztfelhasználó létrehozása

Ez a szakasz célja Getabstract Britta Simon nevű felhasználó létrehozásához. Getabstract támogatja a just-in-time-kiépítés, amely alapértelmezésben engedélyezve van. Nincs meg ebben a szakaszban a művelet elem. Új felhasználó jön létre az Getabstract elérésére, ha még nem létezik tett kísérlet során.
>[!Note]
>Ha manuálisan hozzon létre egy felhasználót, lépjen kapcsolatba kell [Getabstract támogatási csoport](https://www.getabstract.com/en/contact)


### <a name="assign-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Getabstract Azure egyszeri bejelentkezés használatára.

![A felhasználói szerepkör hozzárendelése][200] 

**Britta Simon rendel Getabstract, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

1. Az alkalmazások listájában jelölje ki a **Getabstract**.

    ![Az alkalmazások listáját a Getabstract hivatkozásra](./media/getabstract-tutorial/tutorial_getabstract_app.png)  

1. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![A "Felhasználók és csoportok" hivatkozásra][202]

1. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![A hozzárendelés hozzáadása panel][203]

1. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

1. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

1. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Az egyszeri bejelentkezés vizsgálata

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha a hozzáférési panelen a Getabstract csempére kattint, meg kell lekérése automatikusan bejelentkezett az Getabstract alkalmazáshoz.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/getabstract-tutorial/tutorial_general_01.png
[2]: ./media/getabstract-tutorial/tutorial_general_02.png
[3]: ./media/getabstract-tutorial/tutorial_general_03.png
[4]: ./media/getabstract-tutorial/tutorial_general_04.png

[100]: ./media/getabstract-tutorial/tutorial_general_100.png

[200]: ./media/getabstract-tutorial/tutorial_general_200.png
[201]: ./media/getabstract-tutorial/tutorial_general_201.png
[202]: ./media/getabstract-tutorial/tutorial_general_202.png
[203]: ./media/getabstract-tutorial/tutorial_general_203.png

