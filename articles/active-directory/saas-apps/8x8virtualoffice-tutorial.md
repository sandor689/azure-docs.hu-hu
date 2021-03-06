---
title: 'Oktatóanyag: Azure Active Directory-integráció az 8 x 8 virtuális Office |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active könyvtárhoz, majd 8 x 8 virtuális Office között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3a33f9ba0ca744709e21e9e55acc22b657c2adc2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048419"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Oktatóanyag: Azure Active Directory-integráció az 8 x 8 virtuális Office

Ebben az oktatóanyagban elsajátíthatja, hogyan 8 x 8 virtuális Office integrálható az Azure Active Directory (Azure AD).

8 x 8 integrálása az Azure ad-vel virtuális Office nyújt a következő előnyökkel jár:

- Szabályozhatja, hogy ki férhet 8 x 8 virtuális Office Azure AD-ben
- Az Azure AD-fiókjukat engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett 8 x 8 virtuális Office (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egyetlen központi helyen – az Azure Portalon

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása az 8 x 8 virtuális Office, a következőkre van szükség:

- Az Azure AD-előfizetéshez
- Egy 8 x 8 virtuális Office egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, beszerezheti a egy egy havi próbalehetőség [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. 8 x 8 virtuális Office hozzáadása a katalógusból
2. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 virtuális Office hozzáadása a katalógusból
Az Azure AD-be 8 x 8 virtuális Office-integráció konfigurálásához, hozzá kell 8 x 8 virtuális Office a galériából a felügyelt SaaS-alkalmazások listájára.

**8 x 8 virtuális Office hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

4. A Keresés mezőbe írja be a **8 x 8 virtuális Office**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. Az eredmények panelen válassza ki a **8 x 8 virtuális Office**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezés 8 x 8 virtuális Office egy "Britta Simon." nevű tesztelési felhasználó alapján

Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó 8 x 8 virtuális Office, hogy egy felhasználó Azure AD-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó 8 x 8 hivatkozás kapcsolatának virtuális Office kell létrehozni.

8 x 8 virtuális Office, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezés az 8 x 8 virtuális Office tesztelése és konfigurálása, hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
2. **[Az Azure ad-ben tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
3. **[8 x 8 virtuális Office tesztfelhasználó létrehozása](#creating-a-8x8-virtual-office-test-user)**  – a felhasználó Azure ad-ben reprezentációja kapcsolódó 8 x 8 virtuális Office-megfelelője a Britta Simon rendelkeznie.
4. **[Az Azure ad-ben tesztfelhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és 8 x 8 virtuális Office-alkalmazás az egyszeri bejelentkezés konfigurálása.

**Az Azure AD egyszeri bejelentkezés konfigurálása 8 x 8 virtuális Office-, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **8 x 8 virtuális Office** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. Az a **8 x 8 virtuális Office tartomány és URL-címek** szakaszban, hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. Az a **azonosító** szövegmezőbe írja be a következő minta használatával URL-címe:

    | `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |

    b. Az a **válasz URL-cím** szövegmezőbe írja be a következő minta használatával URL-címe:

    | `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|

    > [!NOTE] 
    > Ezek a értékei nem valódi. Ezek az értékek frissítse a tényleges azonosítóját és a válasz URL-cím. Kapcsolattartó [8 x 8 virtuális Office támogatási csoport](https://www.8x8.com/about-us/contact-us) beolvasni ezeket az értékeket.
 


4. Az a **SAML-aláíró tanúsítvány** területén kattintson **tanúsítvány (Raw)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_general_400.png)

6. Az a **8 x 8 virtuális Office konfigurációs** területén kattintson **konfigurálása 8 x 8 virtuális Office** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **kijelentkezéses URL-címe, SAML Entitásazonosító és SAML egyszeri bejelentkezési szolgáltatás URL-cím** származó a **gyors útmutató szakaszban.**

    ![Egyszeri bejelentkezés konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. Bejelentkezés a 8 x 8 virtuális Office bérlői rendszergazdaként.

8. Válassza ki **virtuális Office fiók Mgr** alkalmazás panel.
   
    ![A kiszolgálóoldali alkalmazás konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Válassza ki **üzleti** kezeléséhez, és kattintson a fiók **bejelentkezés** gombra.
   
    ![A kiszolgálóoldali alkalmazás konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Kattintson a **fiókok** lapon a menüben listában.
   
    ![A kiszolgálóoldali alkalmazás konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Kattintson a **az egyszeri bejelentkezést** fiókok listájában.
   
    ![A kiszolgálóoldali alkalmazás konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Válassza ki **az egyszeri bejelentkezést** hitelesítési módszert, majd kattintson a **SAML**.
    
    ![A kiszolgálóoldali alkalmazás konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. Másolás **SAML egyszeri bejelentkezési URL-cím**, **egyszeri kijelentkezési szolgáltatás URL-bejelentkezést** és **kiállítójának URL-címe** az Azure AD-ből **jelentkezzen be az URL-cím**, **kijelentkezési URL-**  és **kiállítójának URL-címe** 8 x 8 virtuális Office. 
    
    ![A kiszolgálóoldali alkalmazás konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. Kattintson a **böngésző** gombra kattintva töltse fel a tanúsítványt, amely az Azure AD-ból letöltött, majd kattintson a **mentése** gombra.

> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó létrehozása
Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **az Azure portal**, a bal oldali navigációs panelén kattintson **Azure Active Directory** ikonra.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** a párbeszédpanel tetején.
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/8x8virtualoffice-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőbe írja be a **e-mail-cím** BrittaSimon az.

    c. Válassza ki **jelszó megjelenítése** és jegyezze fel az értékét a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-8x8-virtual-office-test-user"></a>8 x 8 virtuális Office tesztfelhasználó létrehozása

Ez a szakasz célja egy 8 x 8 virtuális Office Britta Simon nevű felhasználó létrehozásához. 8 x 8 virtuális Office támogatja just-in-time-kiépítés, amely alapértelmezés szerint van engedélyezve.

Nincs meg ebben a szakaszban a művelet elem. Új felhasználó 8 x 8 virtuális Office elérését, ha még nem létezik tett kísérlet során jön létre. 

>[!NOTE]
>Hozzon létre egy felhasználót manuálisan kell, ha kapcsolódni kell a [8 x 8 virtuális Office támogatási csoport](https://www.8x8.com/about-us/contact-us). 

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés 8 x 8 virtuális Office Azure egyszeri bejelentkezés használatára.

![Felhasználó hozzárendelése][200] 

**Britta Simon rendel 8 x 8 virtuális Office, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **8 x 8 virtuális Office**.

    ![Egyszeri bejelentkezés konfigurálása](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha rákattint a 8 x 8 virtuális Office csempét, a hozzáférési panelen, meg kell beolvasása automatikusan bejelentkezett, 8 x 8 virtuális Office-alkalmazás.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/8x8virtualoffice-tutorial/tutorial_general_203.png

