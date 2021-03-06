---
title: 'Oktatóanyag: Azure Active Directory-integráció az Jitbit segélyszolgálat |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és Jitbit segélyszolgálat között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 94ded0ef1bf77de20973a87a1ca2d6d1dd3fdf3f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426652"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Oktatóanyag: Azure Active Directory-integráció az Jitbit segélyszolgálat

Ebben az oktatóanyagban elsajátíthatja, hogyan Jitbit segélyszolgálat integrálása az Azure Active Directory (Azure AD).

Jitbit segélyszolgálat integrálása az Azure ad-ben nyújt a következő előnyökkel jár:

- Szabályozhatja, hogy ki férhet hozzá Jitbit segélyszolgálat Azure AD-ben
- Az Azure AD-fiókjukat engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett Jitbit követően az ügyfélszolgálathoz (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egyetlen központi helyen – az Azure Portalon

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Jitbit segélyszolgálat konfigurálni az Azure AD-integráció, a következőkre van szükség:

- Az Azure AD-előfizetéshez
- A segélyszolgálat Jitbit egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, beszerezheti a egy egy havi próbalehetőség [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. Jitbit segélyszolgálat hozzáadása a katalógusból
1. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Jitbit segélyszolgálat hozzáadása a katalógusból
Az Azure AD integrálása a segélyszolgálat Jitbit konfigurálásához hozzá kell Jitbit segélyszolgálat a katalógusból a felügyelt SaaS-alkalmazások listájára.

**Jitbit segélyszolgálat hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Active Directory][1]

1. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![Alkalmazások][2]
    
1. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

1. A Keresés mezőbe írja be a **Jitbit segélyszolgálat**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

1. Az eredmények panelen válassza ki a **Jitbit segélyszolgálat**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés
Ebben a szakaszban konfigurálja, és Jitbit ügyfélszolgálat az Azure AD egyszeri bejelentkezés tesztelése egy "Britta Simon" nevű tesztelési felhasználó alapján.

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó Jitbit segélyszolgálat mi egy felhasználó számára az Azure ad-ben. Más szóval Azure AD-felhasználót és a kapcsolódó felhasználó Jitbit ügyfélszolgálati hivatkozás kapcsolata kell létrehozni.

Jitbit segélyszolgálat, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezés az Jitbit segélyszolgálat tesztelése és konfigurálása, hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
1. **[Az Azure ad-ben tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
1. **[Jitbit segélyszolgálat tesztfelhasználó létrehozása](#creating-a-jitbit-helpdesk-test-user)**  - a-megfelelője a Britta Simon szerepel, amely kapcsolódik a felhasználó Azure ad-ben reprezentációja segélyszolgálat Jitbit.
1. **[Az Azure ad-ben tesztfelhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
1. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és Jitbit segélyszolgálat alkalmazását az egyszeri bejelentkezés konfigurálása.

**Szeretné konfigurálni az Azure AD egyszeri bejelentkezés Jitbit segélyszolgálat, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **Jitbit segélyszolgálat** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása][4]

1. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

1. Az a **Jitbit segélyszolgálat tartomány és URL-címek** szakaszban, hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    a. Az a **bejelentkezési URL-** szövegmezőbe írja be a következő minta használatával URL-címe: 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Az érték nem valódi. Ez az érték frissítse a tényleges bejelentkezési URL-CÍMÉT. Kapcsolattartó [Jitbit segélyszolgálat ügyfél-támogatási csapatának](https://www.jitbit.com/support/) lekérni ezt az értéket. 
    
    b.  Az a **azonosító** szövegmezőbe írja be a következő URL-cím: `https://www.jitbit.com/web-helpdesk/`

    
 


1. Az a **SAML-aláíró tanúsítvány** területén kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

1. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/jitbit-helpdesk-tutorial/tutorial_general_400.png)

1. Az a **Jitbit segélyszolgálat konfigurációs** területén kattintson **konfigurálása Jitbit segélyszolgálat** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **SAML egyszeri bejelentkezési szolgáltatás URL-cím** származó a **gyors útmutató szakaszban.**

    ![Egyszeri bejelentkezés konfigurálása](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

1. Egy másik böngészőablakban jelentkezzen be a segélyszolgálat Jitbit vállalati hely rendszergazdaként.

1. A felső eszköztáron kattintson **felügyeleti**.
   
    ![Felügyeleti](./media/jitbit-helpdesk-tutorial/ic777681.png "felügyelete")

1. Kattintson a **általános beállítások**.
   
    ![Felhasználók, a vállalatok és az engedélyek](./media/jitbit-helpdesk-tutorial/ic777680.png "felhasználókat, a vállalatok és engedélyek")

1. Az a **hitelesítési beállítások** konfigurációs szakaszban, hajtsa végre az alábbi lépéseket:
   
    ![Hitelesítési beállítások](./media/jitbit-helpdesk-tutorial/ic777683.png "hitelesítési beállítások")
    
    a. Válassza ki **engedélyezése SAML 2.0-s egyszeri bejelentkezés**, jelentkezzen be az egyszeri bejelentkezés (SSO), **OneLogin**.

    b. Az a **végponti URL-cím** szövegmező, illessze be az értéket a **SAML egyszeri bejelentkezési szolgáltatás URL-cím** Azure Portalról másolt.

    c. Nyissa meg a **base-64** kódolású Jegyzettömbben-tanúsítványt, a tartalmát a vágólapra másolja és illessze be azt a **X.509-tanúsítvány** szövegmező

    d. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó létrehozása
Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **az Azure portal**, a bal oldali navigációs panelén kattintson **Azure Active Directory** ikonra.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/jitbit-helpdesk-tutorial/create_aaduser_01.png) 

1. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/jitbit-helpdesk-tutorial/create_aaduser_02.png) 

1. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** a párbeszédpanel tetején.
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/jitbit-helpdesk-tutorial/create_aaduser_03.png) 

1. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőbe típus néven **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőbe írja be a **e-mail-cím** BrittaSimon az.

    c. Válassza ki **jelszó megjelenítése** és jegyezze fel az értékét a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a>Jitbit segélyszolgálat tesztfelhasználó létrehozása

Ahhoz, hogy az Azure AD-felhasználók Jitbit segélyszolgálat-ba való bejelentkezéshez, akkor ki kell építeni Jitbit segélyszolgálat be.  Jitbit segélyszolgálat, esetén kiépítése a manuális feladat.

**Üzembe helyez egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be a **Jitbit segélyszolgálat** bérlő.

1. A felső menüben kattintson **felügyeleti**.
   
    ![Felügyeleti](./media/jitbit-helpdesk-tutorial/ic777681.png "felügyelete")

1. Kattintson a **felhasználók, a vállalatok és az engedélyek**.
   
    ![Felhasználók, a vállalatok és az engedélyek](./media/jitbit-helpdesk-tutorial/ic777682.png "felhasználókat, a vállalatok és engedélyek")

1. Kattintson a **felhasználó hozzáadása**.
   
    ![Felhasználó hozzáadása](./media/jitbit-helpdesk-tutorial/ic777685.png "felhasználó hozzáadása")
   
1. A létrehozás területen írja be az adatokat az Azure AD-fiók kíván létrehozni a következő:

    ![Hozzon létre](./media/jitbit-helpdesk-tutorial/ic777686.png "létrehozása")
   
   a. Az a **felhasználónév** szövegmezőbe írja be **BrittaSimon**, a felhasználó neve, mint az Azure Portalon.

   b. Az a **E-mail** szövegmezőbe írja be e-mail a felhasználó például **BrittaSimon@contoso.com**.

   c. Az a **Utónév** szövegmező, mint például a felhasználó utóneve típus **Britta**.

   d. Az a **Vezetéknév** szövegmezőbe írja be például a felhasználó vezetékneve **Simon**.
   
   e. Kattintson a **Create** (Létrehozás) gombra.

>[!NOTE]
>Jitbit segélyszolgálat felhasználói fiók létrehozása eszközöket és Jitbit segélyszolgálat által biztosított API-k segítségével az Azure AD-felhasználói fiókok kiépítése.
> 
        

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Jitbit segélyszolgálat Azure egyszeri bejelentkezés használatára.

![Felhasználó hozzárendelése][200] 

**Britta Simon rendel Jitbit segélyszolgálat, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

1. Az alkalmazások listájában jelölje ki a **Jitbit segélyszolgálat**.

    ![Egyszeri bejelentkezés konfigurálása](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

1. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

1. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

1. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

1. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

1. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha a hozzáférési panelen a segélyszolgálat Jitbit csempére kattint, Jitbit segélyszolgálat alkalmazás bejelentkezési lapján szerezheti be.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/jitbit-helpdesk-tutorial/tutorial_general_203.png

