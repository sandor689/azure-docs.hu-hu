---
title: 'Oktatóanyag: Azure Active Directory-integráció az 15Five |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és 15Five között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 3cc0d9122fd7335bc29c7f35c1163cb39d3481b8
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054301"
---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a>Oktatóanyag: Azure Active Directory-integráció az 15Five

Ebben az oktatóanyagban elsajátíthatja, hogyan 15Five integrálása az Azure Active Directory (Azure AD).

15Five integrálása az Azure ad-ben nyújt a következő előnyökkel jár:

- Szabályozhatja, hogy ki férhet hozzá 15Five Azure AD-ben
- Engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett a 15Five (egyszeri bejelentkezés) az Azure AD-fiókjukat
- Kezelheti a fiókokat, egyetlen központi helyen – az Azure Portalon

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

15Five az Azure AD-integráció konfigurálásához a következőkre van szükség:

- Az Azure AD-előfizetéshez
- Egy 15Five egyszeri bejelentkezéses engedélyezett előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, beszerezheti a egy egy havi próbalehetőség [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. 15Five hozzáadása a katalógusból
2. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-15five-from-the-gallery"></a>15Five hozzáadása a katalógusból
Az Azure AD integrálása a 15Five konfigurálásához hozzá kell 15Five a katalógusból a felügyelt SaaS-alkalmazások listájára.

**15Five hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

4. A Keresés mezőbe írja be a **15Five**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/15five-tutorial/tutorial_15five_search.png)

5. Az eredmények panelen válassza ki a **15Five**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/15five-tutorial/tutorial_15five_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés
Ebben a szakaszban konfigurálja, és a teszt "Britta Simon." nevű felhasználó 15Five az Azure AD egyszeri bejelentkezés tesztelése

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó 15Five mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó 15Five hivatkozás kapcsolata kell létrehozni.

15Five, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezés az 15Five tesztelése és konfigurálása, hogy hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
2. **[Az Azure ad-ben tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
3. **[15Five tesztfelhasználó létrehozása](#creating-a-15five-test-user)**  – egy megfelelője a Britta Simon a felhasználó Azure ad-ben reprezentációja kapcsolódó 15Five rendelkeznie.
4. **[Az Azure ad-ben tesztfelhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és 15Five alkalmazását az egyszeri bejelentkezés konfigurálása.

**Szeretné konfigurálni az Azure AD egyszeri bejelentkezés 15Five, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **15Five** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/15five-tutorial/tutorial_15five_samlbase.png)

3. Az a **15Five tartomány és URL-címek** szakaszban, hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/15five-tutorial/tutorial_15five_url.png)

    a. Az a **bejelentkezési URL-** szövegmezőbe írja be a következő minta használatával URL-címe: `https://<companyname>.15five.com`

    b. Az a **azonosító** szövegmezőbe írja be a következő minta használatával URL-címe: `https://<companyname>.15five.com/saml2/metadata/`

    > [!NOTE] 
    > Ezek a értékei nem valódi. Ezek az értékek frissítse a tényleges bejelentkezési URL- és azonosító. Kapcsolattartó [15Five ügyfél-támogatási csapatának](https://www.15five.com/contact/) beolvasni ezeket az értékeket. 
 
4. Az a **SAML-aláíró tanúsítvány** területén kattintson **metaadatainak XML** , és mentse a metaadat-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/15five-tutorial/tutorial_15five_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/15five-tutorial/tutorial_general_400.png)

6. Az egyszeri bejelentkezés konfigurálása **15Five** oldalon kell küldenie a letöltött **metaadatainak XML** való [15Five támogatási csapatának](https://www.15five.com/contact/).

> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó létrehozása
Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **az Azure portal**, a bal oldali navigációs panelén kattintson **Azure Active Directory** ikonra.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/15five-tutorial/create_aaduser_01.png) 

2. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/15five-tutorial/create_aaduser_02.png) 

3. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** a párbeszédpanel tetején.
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/15five-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/15five-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőbe írja be a **e-mail-cím** BrittaSimon az.

    c. Válassza ki **jelszó megjelenítése** és jegyezze fel az értékét a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-15five-test-user"></a>15Five tesztfelhasználó létrehozása

Ahhoz, hogy az Azure AD-felhasználók 15Five jelentkezzen be, akkor ki kell építeni 15Five be. Ha 15Five, kiépítés van egy manuális tevékenység.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Felhasználók átadásának konfigurálása, hajtsa végre az alábbi lépéseket:
1. Jelentkezzen be a **15Five** rendszergazdaként a vállalati webhely.

2. Lépjen a **kezelheti a vállalat**.
   
    ![Kezelheti a vállalat](./media/15five-tutorial/IC784675.png "vállalati felügyelete")

3. Lépjen a **személyek \> személyeket adja hozzá a**.
   
    ![Személyek](./media/15five-tutorial/IC784676.png "személyek")

4. Az új személy adja hozzá a szakaszban a következő lépésekkel:
   
    ![Adja hozzá az új személy](./media/15five-tutorial/IC784677.png "új személy hozzáadása")
   
    a. Írja be a **Utónév**, **Vezetéknév**, **cím**, **E-mail-cím** kíván létrehozni az érvényes Azure Active Directory-fiókok a kapcsolódó szöveges mezőkben.

    b. Kattintson a **Done** (Kész) gombra.
   
    > [!NOTE]
    > Az Azure ad-ben fióktulajdonos kap egy e-mailt és a telepítőre mutató erősítse meg a fiókot, mielőtt aktívvá válik.
   
### <a name="assigning-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon használja az Azure egyszeri bejelentkezés 15Five való hozzáférést.

![Felhasználó hozzárendelése][200] 

**Britta Simon rendel 15Five, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **15Five**.

    ![Egyszeri bejelentkezés konfigurálása](./media/15five-tutorial/tutorial_15five_app.png) 

3. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha a hozzáférési panelen a 15Five csempére kattint, 15Five alkalmazás bejelentkezési oldalának szerezheti be.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/15five-tutorial/tutorial_general_01.png
[2]: ./media/15five-tutorial/tutorial_general_02.png
[3]: ./media/15five-tutorial/tutorial_general_03.png
[4]: ./media/15five-tutorial/tutorial_general_04.png

[100]: ./media/15five-tutorial/tutorial_general_100.png

[200]: ./media/15five-tutorial/tutorial_general_200.png
[201]: ./media/15five-tutorial/tutorial_general_201.png
[202]: ./media/15five-tutorial/tutorial_general_202.png
[203]: ./media/15five-tutorial/tutorial_general_203.png

