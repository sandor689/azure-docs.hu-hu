---
title: 'Oktatóanyag: Azure Active Directory-integráció a Háttérkép Online |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és a Háttérkép Online között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4fd6b29b-1b46-4fd1-9f5e-16b1c9d892cd
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 3e0da76410c00b8ec0865b094d15237f4f932fb5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421390"
---
# <a name="tutorial-azure-active-directory-integration-with-bgs-online"></a>Oktatóanyag: Azure Active Directory-integráció az Online Háttérkép

Ebben az oktatóanyagban elsajátíthatja, hogyan integrálható a Háttérkép Online az Azure Active Directoryval (Azure AD).

Az Azure AD integrálása Háttérkép Online nyújt a következő előnyökkel jár:

- Szabályozhatja, hogy ki férhet hozzá a Háttérkép Online az Azure AD-ben
- Az Azure AD-fiókjukat engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett a Háttérkép Online (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egyetlen központi helyen – az Azure Portalon

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása a Háttérkép Online, a következőkre van szükség:

- Az Azure AD-előfizetéshez
- A Háttérkép Online egyszeri bejelentkezéses engedélyezett előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, beszerezheti a egy egy havi próbalehetőség [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. A katalógusból Online Háttérkép hozzáadása
1. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-bgs-online-from-the-gallery"></a>A katalógusból Online Háttérkép hozzáadása
Az Azure AD-be Háttérkép online integráció konfigurálásához, hozzá kell Háttérkép Online a galériából a felügyelt SaaS-alkalmazások listájára.

**Adja hozzá a Háttérkép Online a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Active Directory][1]

1. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![Alkalmazások][2]
    
1. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

1. A Keresés mezőbe írja be a **Háttérkép Online**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bgsonline-tutorial/tutorial_bgsonline_search.png)

1. Az eredmények panelen válassza ki a **Háttérkép Online**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bgsonline-tutorial/tutorial_bgsonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés Háttérkép Online egy "Britta Simon." nevű tesztelési felhasználó alapján

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó a Háttérkép Online mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó a Háttérkép Online hivatkozás kapcsolatát kell létrehozni.

A Háttérkép Online hozzárendelése értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Konfigurálása és az Azure AD egyszeri bejelentkezés Háttérkép online-nal való teszteléséhez hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
1. **[Az Azure ad-ben tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
1. **[Háttérkép Online tesztfelhasználó létrehozása](#creating-a-bgs-online-test-user)**  - a-megfelelője a Britta Simon rendelkezik a Háttérkép online-ban, amely kapcsolódik az Azure AD felhasználói ábrázolása.
1. **[Az Azure ad-ben tesztfelhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
1. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és a Háttérkép Online alkalmazás egyszeri bejelentkezés konfigurálása.

**Az Azure AD egyszeri bejelentkezés konfigurálásához a Háttérkép Online, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **Háttérkép Online** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása][4]

1. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/bgsonline-tutorial/tutorial_bgsonline_samlbase.png)

1. Az a **Háttérkép Online tartomány és URL-címek** szakaszban, hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/bgsonline-tutorial/tutorial_bgsonline_url.png)

    a. Az a **azonosító** szövegmezőbe írja be a következő minta használatával URL-címe:

    Az éles környezetben használja ezt a mintát `https://<company name>.millwardbrown.report` 

    A tesztkörnyezetben használja ezt a mintát `https://millwardbrown.marketingtracker.nl/mt5/`

    b. Az a **válasz URL-cím** szövegmezőbe írja be a következő minta használatával URL-címe:
    
    Az éles környezetben használja ezt a mintát `https://<company name>.millwardbrown.report/sso/saml/AssertionConsumerService.aspx` 
      
    A tesztkörnyezetben használja ezt a mintát `https://millwardbrown.marketingtracker.nl/mt5/sso/saml/AssertionConsumerService.aspx`

    > [!NOTE] 
    > Ezek a értékei nem valódi. Ezek az értékek frissítse a tényleges azonosítóját és a válasz URL-cím. Kapcsolattartó [Háttérkép Online támogatási csoport](mailTo:bgsdashboardteam@millwardbrown.com) beolvasni ezeket az értékeket.
 

1. Az a **SAML-aláíró tanúsítvány** területén kattintson **metaadatainak XML** , és mentse a metaadat-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/bgsonline-tutorial/tutorial_bgsonline_certificate.png) 

1. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/bgsonline-tutorial/tutorial_general_400.png)

1. Az a **Háttérkép Online konfigurációs** területén kattintson **Háttérkép Online konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **SAML egyszeri bejelentkezési szolgáltatás URL-cím** származó a **gyors útmutató szakaszban.**

    ![Egyszeri bejelentkezés konfigurálása](./media/bgsonline-tutorial/tutorial_bgsonline_configure.png) 

1. Az egyszeri bejelentkezés konfigurálása **Háttérkép Online** oldalon kell küldenie a letöltött **metaadatainak XML** és **SAML egyszeri bejelentkezési szolgáltatás URL-cím** való [Háttérkép Online támogatási csoport](mailto:bgsdashboardteam@millwardbrown.com). 


> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó létrehozása
Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **az Azure portal**, a bal oldali navigációs panelén kattintson **Azure Active Directory** ikonra.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bgsonline-tutorial/create_aaduser_01.png) 

1. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bgsonline-tutorial/create_aaduser_02.png) 

1. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** a párbeszédpanel tetején.
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bgsonline-tutorial/create_aaduser_03.png) 

1. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bgsonline-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőbe írja be a **e-mail-cím** BrittaSimon az.

    c. Válassza ki **jelszó megjelenítése** és jegyezze fel az értékét a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-bgs-online-test-user"></a>Háttérkép Online tesztfelhasználó létrehozása

Ebben a szakaszban egy a Háttérkép Online Britta Simon nevű felhasználó hoz létre. Együttműködve [Háttérkép Online támogatási csoport](mailto:bgsdashboardteam@millwardbrown.com) a felhasználók hozzáadása a Háttérkép Online platform.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon Háttérkép online-hoz való hozzáférés biztosításával az Azure egyszeri bejelentkezés használatára.

![Felhasználó hozzárendelése][200] 

**Britta Simon rendel Online Háttérkép, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

1. Az alkalmazások listájában jelölje ki a **Háttérkép Online**.

    ![Egyszeri bejelentkezés konfigurálása](./media/bgsonline-tutorial/tutorial_bgsonline_app.png) 

1. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

1. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

1. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

1. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

1. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés konfigurációja a hozzáférési panelen.

Ha a hozzáférési panelen a Háttérkép Online csempére kattint, meg kell lekérése automatikusan bejelentkezett a Háttérkép Online alkalmazásba.

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bgsonline-tutorial/tutorial_general_01.png
[2]: ./media/bgsonline-tutorial/tutorial_general_02.png
[3]: ./media/bgsonline-tutorial/tutorial_general_03.png
[4]: ./media/bgsonline-tutorial/tutorial_general_04.png

[100]: ./media/bgsonline-tutorial/tutorial_general_100.png

[200]: ./media/bgsonline-tutorial/tutorial_general_200.png
[201]: ./media/bgsonline-tutorial/tutorial_general_201.png
[202]: ./media/bgsonline-tutorial/tutorial_general_202.png
[203]: ./media/bgsonline-tutorial/tutorial_general_203.png

