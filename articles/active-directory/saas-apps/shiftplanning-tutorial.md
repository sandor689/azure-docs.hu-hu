---
title: 'Oktatóanyag: Azure Active Directory-integráció az emberiség |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és az emberiség között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 5858300027d77b6057e059960f1c997b4bfc1e56
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429008"
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a>Oktatóanyag: Azure Active Directory-integráció az emberiség

Ebben az oktatóanyagban elsajátíthatja, hogyan emberiség integrálása az Azure Active Directory (Azure AD).

Emberiség integrálása az Azure ad-ben nyújt a következő előnyökkel jár:

- Szabályozhatja, hogy ki férhet hozzá emberiség Azure AD-ben
- Engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett, el az emberiség (egyszeri bejelentkezés) az Azure AD-fiókjukat
- Kezelheti a fiókokat, egyetlen központi helyen – az Azure Portalon

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása az emberiség, a következőkre van szükség:

- Az Azure AD-előfizetéshez
- Egy emberiség egyszeri bejelentkezéses engedélyezett előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, beszerezheti a egy egy havi próbalehetőség [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. Emberiség hozzáadása a katalógusból
1. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-humanity-from-the-gallery"></a>Emberiség hozzáadása a katalógusból
Az Azure ad-ben emberiség integráció konfigurálásához, hozzá kell emberiség a galériából a felügyelt SaaS-alkalmazások listájára.

**Emberiség hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Active Directory][1]

1. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![Alkalmazások][2]
    
1. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

1. A Keresés mezőbe írja be a **emberiség**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/shiftplanning-tutorial/tutorial_humanity_search.png)

1. Az eredmények panelen válassza ki a **emberiség**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezés az emberiség egy "Britta Simon." nevű tesztelési felhasználó alapján

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó emberiség mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó emberiség hivatkozás kapcsolata kell létrehozni.

Emberiség, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Emberiség az Azure AD egyszeri bejelentkezés tesztelése és konfigurálása, hogy hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
1. **[Az Azure ad-ben tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
1. **[Emberiség tesztfelhasználó létrehozása](#creating-a-humanity-test-user)**  – egy megfelelője a Britta Simon emberiség, amely a felhasználó Azure ad-ben ábrázolása van csatolva van.
1. **[Az Azure ad-ben tesztfelhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
1. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és az emberiség alkalmazás egyszeri bejelentkezés konfigurálása.

**Az Azure AD egyszeri bejelentkezés konfigurálása az emberiség, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **emberiség** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása][4]

1. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/shiftplanning-tutorial/tutorial_humanity_samlbase.png)

1. Az a **emberiség tartomány és URL-címek** szakaszban, hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/shiftplanning-tutorial/tutorial_humanity_url.png)

    a. Az a **bejelentkezési URL-** szövegmezőbe írja be a következő minta használatával URL-címe: `https://company.humanity.com/includes/saml/`

    b. Az a **azonosító** szövegmezőbe írja be a következő minta használatával URL-címe: `https://company.humanity.com/app/`

    > [!NOTE] 
    > Ezek a értékei nem valódi. Ezek az értékek frissítse a tényleges bejelentkezési URL- és azonosító. Kapcsolattartó [emberiség ügyfél-támogatási csapatának](https://www.humanity.com/support/) beolvasni ezeket az értékeket. 
 
1. Az a **SAML-aláíró tanúsítvány** területén kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/shiftplanning-tutorial/tutorial_humanity_certificate.png) 

1. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/shiftplanning-tutorial/tutorial_general_400.png)

1. Az a **emberiség konfigurációs** területén kattintson **konfigurálása emberiség** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **SAML egyszeri bejelentkezési szolgáltatás URL-CÍMÉT, és kijelentkezéses URL-cím** származó a **gyors útmutató szakaszban.**

    ![Egyszeri bejelentkezés konfigurálása](./media/shiftplanning-tutorial/tutorial_humanity_configure.png) 

1. Egy másik böngészőablakban, jelentkezzen be a **emberiség** rendszergazdaként a vállalati webhely.

1. A felső menüben kattintson **rendszergazdai**.
   
    ![Rendszergazdai](./media/shiftplanning-tutorial/iC786619.png "rendszergazda")

1. A **integrációs**, kattintson a **egyszeri bejelentkezés**.
   
    ![Egyszeri bejelentkezés](./media/shiftplanning-tutorial/iC786620.png "egyszeri bejelentkezés")

1. Az a **egyszeri bejelentkezés** szakaszban, hajtsa végre az alábbi lépéseket:
   
    ![Egyszeri bejelentkezés](./media/shiftplanning-tutorial/iC786905.png "egyszeri bejelentkezés")
   
    a. Válassza ki **SAML engedélyezett**.

    b. Válassza ki **bejelentkezési jelszó engedélyezése**.

    c. Illessze be a **SAML egyszeri bejelentkezési szolgáltatás URL-cím** be értéket a **SAML kiállítójának URL-címe** szövegmezőbe.

    d. Illessze be a **kijelentkezéses URL-cím** be értéket a **távoli kijelentkezési URL-címe** szövegmezőbe.
   
    e. Nyissa meg a base-64 kódolású tanúsítványt a Jegyzettömbben, a tartalmát a vágólapra másolja és illessze be azt a **X.509-tanúsítvány** szövegmezőbe.

1. Kattintson a **beállítások mentése**.

> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó létrehozása
Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **az Azure portal**, a bal oldali navigációs panelén kattintson **Azure Active Directory** ikonra.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/shiftplanning-tutorial/create_aaduser_01.png) 

1. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/shiftplanning-tutorial/create_aaduser_02.png) 

1. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** a párbeszédpanel tetején.
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/shiftplanning-tutorial/create_aaduser_03.png) 

1. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/shiftplanning-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőbe írja be a **e-mail-cím** BrittaSimon az.

    c. Válassza ki **jelszó megjelenítése** és jegyezze fel az értékét a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-humanity-test-user"></a>Emberiség tesztfelhasználó létrehozása

Ahhoz, hogy az Azure AD-felhasználók emberiség bejelentkezni, akkor ki kell építeni az emberiség. Emberiség esetén kiépítése a manuális feladat.

**Üzembe helyez egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be a **emberiség** rendszergazdaként a vállalati webhely.

1. Kattintson a **rendszergazdai**.
   
    ![Rendszergazdai](./media/shiftplanning-tutorial/iC786619.png "rendszergazda")

1. Kattintson a **személyzet**.
   
    ![Személyzet](./media/shiftplanning-tutorial/ic786623.png "személyzet")

1. A **műveletek**, kattintson a **adja hozzá az alkalmazottak**.
   
    ![Adja hozzá az alkalmazottak](./media/shiftplanning-tutorial/iC786624.png "alkalmazottak hozzáadása")

1. Az a **adja hozzá az alkalmazottak** szakaszban, hajtsa végre az alábbi lépéseket:
   
    ![Mentse az alkalmazottak](./media/shiftplanning-tutorial/iC786625.png "alkalmazottak mentése")
   
    a. Írja be a **Utónév**, **Vezetéknév**, és **E-mail** egy érvényes AAD-fióknevet, amelyet a kapcsolódó szövegmezőkben létrehozásához.

    b. Kattintson a **alkalmazottak mentése**.

>[!NOTE]
>Bármely más emberiség felhasználói fiók létrehozása eszközöket használhatja, vagy az aad-ben a felhasználói fiókok kiépítését emberiség által biztosított API-k.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés emberiség Azure egyszeri bejelentkezés használatára.

![Felhasználó hozzárendelése][200] 

**Britta Simon rendel emberiség, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

1. Az alkalmazások listájában jelölje ki a **emberiség**.

    ![Egyszeri bejelentkezés konfigurálása](./media/shiftplanning-tutorial/tutorial_humanity_app.png) 

1. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

1. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

1. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

1. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

1. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha a hozzáférési panelen a emberiség csempére kattint, akkor kell lekérése automatikusan bejelentkezett az emberiség alkalmazáshoz.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/shiftplanning-tutorial/tutorial_general_203.png

