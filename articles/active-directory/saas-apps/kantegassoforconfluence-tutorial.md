---
title: 'Oktatóanyag: Azure Active Directory-integráció az való összefolyás felett Kantega SSO-val |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés és Kantega egyszeri Bejelentkezéssel való összefolyás felett az Azure Active Directory között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: fd53a6814649b529e301c3135fb491c51a13bcb1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449226"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Oktatóanyag: Azure Active Directory-integráció az való összefolyás felett Kantega SSO-val

Ebben az oktatóanyagban elsajátíthatja, hogyan való összefolyás felett a Kantega SSO integrálása az Azure Active Directory (Azure AD).

Való összefolyás felett a Kantega SSO integrálása az Azure AD nyújt a következő előnyökkel jár:

- Szabályozhatja, hogy ki férhet hozzá Kantega egyszeri Bejelentkezéssel való összefolyás felett az Azure AD-ben
- Az Azure AD-fiókjukat engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett Kantega SSO számára (egyszeri bejelentkezés) való összefolyás felett
- Kezelheti a fiókokat, egyetlen központi helyen – az Azure Portalon

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása az való összefolyás felett Kantega SSO-val, a következőkre van szükség:

- Az Azure AD-előfizetéshez
- Egy Kantega egyszeri Bejelentkezéssel való összefolyás felett az egyszeri bejelentkezés az előfizetés engedélyezve van

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, beszerezheti a egy egy havi próbalehetőség [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. Kantega egyszeri Bejelentkezéssel való összefolyás felett a Hozzáadás a katalógusból
1. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-kantega-sso-for-confluence-from-the-gallery"></a>Kantega egyszeri Bejelentkezéssel való összefolyás felett a Hozzáadás a katalógusból
Az Azure AD-be való összefolyás felett Kantega SSO-integráció konfigurálásához, hozzá kell Kantega egyszeri Bejelentkezéssel való összefolyás felett az a galériából a felügyelt SaaS-alkalmazások listájára.

**Kantega egyszeri Bejelentkezéssel való összefolyás felett az hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Active Directory][1]

1. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![Alkalmazások][2]
    
1. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

1. A Keresés mezőbe írja be a **Kantega egyszeri Bejelentkezéssel való összefolyás felett az**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

1. Az eredmények panelen válassza ki a **Kantega egyszeri Bejelentkezéssel való összefolyás felett az**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezés az úgynevezett "Britta Simon" tesztfelhasználó alapján való összefolyás felett Kantega SSO-val.

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó Kantega egyszeri Bejelentkezéssel való összefolyás felett a mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó Kantega egyszeri Bejelentkezéssel való összefolyás felett a hivatkozás kapcsolata kell létrehozni.

Való összefolyás felett Kantega egyszeri Bejelentkezést, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezés az való összefolyás felett Kantega SSO-val tesztelése és konfigurálása, hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
1. **[Az Azure ad-ben tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
1. **[Egy Kantega egyszeri bejelentkezés az való összefolyás felett tesztfelhasználó létrehozása](#creating-a-kantega-sso-for-confluence-test-user)**  – egy megfelelője a Britta Simon Kantega egyszeri Bejelentkezést a felhasználó Azure ad-ben reprezentációja kapcsolódó való összefolyás felett van.
1. **[Az Azure ad-ben tesztfelhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
1. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és a Kantega egyszeri Bejelentkezéssel való összefolyás felett alkalmazáshoz az egyszeri bejelentkezés konfigurálása.

**Az Azure AD egyszeri bejelentkezés konfigurálása az való összefolyás felett Kantega SSO-val, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **Kantega egyszeri Bejelentkezéssel való összefolyás felett az** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása][4]

1. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

1. A **Identitásszolgáltató** módban indul a **Kantega egyszeri Bejelentkezéssel való összefolyás felett tartomány és URL-címek** szakasz hajtsa végre a következő lépést:

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    a. Az a **azonosító** szövegmezőbe írja be a következő minta használatával URL-címe: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. Az a **válasz URL-cím** szövegmezőbe írja be a következő minta használatával URL-címe: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

1. A **SP** kezdeményezett mód, a jelölőnégyzet **speciális URL-beállítások megjelenítése** , és hajtsa végre a következő lépést:

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    Az a **bejelentkezési URL-** szövegmezőbe írja be a következő minta használatával URL-címe: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Ezek a értékei nem valódi. Frissítse a tényleges azonosítóját, válasz URL-cím és bejelentkezési URL-ezeket az értékeket. Ezek az értékek fogadásának való összefolyás felett beépülő modul, az oktatóanyag későbbi részében ismertetett konfigurálása során.

1. Az a **SAML-aláíró tanúsítvány** területén kattintson **metaadatainak XML** , és mentse a metaadat-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

1. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
1. Egy másik böngészőablakban, jelentkezzen be a **felügyeleti portálon való összefolyás felett** rendszergazdaként.

1. Vigye a mutatót a fogaskerék alakú ikonjára, majd kattintson a **bővítmények**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon1.png)

1. A **ATLASSIAN MARKETPLACE** lapra, majd **új bővítmények keresése**. 

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon.png)

1. Keresés **Kantega egyszeri bejelentkezési SAML Kerberos való összefolyás felett** kattintson **telepítése** gombra az új SAML beépülő modul telepítéséhez.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon2.png)

1. A beépülő modul telepítése elindul.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon3.png)

1. A telepítés befejezése után. Kattintson a **Bezárás** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon33.png)

1.  Kattintson a **Kezelés** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon34.png)
    
1. Kattintson a **konfigurálása** a új beépülő modul konfigurálásához.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon35.png)

1. Az új beépülő modult is területen található **felhasználók és biztonsági** fülre.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon36.png)
    
1. Az a **SAML** szakaszban. Válassza ki **Azure Active Directory (Azure AD)** származó a **Hozzáadás identitásszolgáltató** legördülő listából.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon4.png)

1. Válassza ki az előfizetés szintjén, **alapszintű**.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon5.png)     

1. Az a **alkalmazás tulajdonságai** szakaszban, hajtsa végre a következő lépéseket: 

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon6.png)

    a. Másolás a **Alkalmazásazonosító URI-t** értékét, és használja azt **azonosítóját, a válasz URL-cím és a bejelentkezési URL-** a a **Kantega egyszeri Bejelentkezéssel való összefolyás felett tartomány és URL-címek** szakaszban az Azure Portalon.

    b. Kattintson a **Tovább** gombra.

1. Az a **metaadatok importálása** szakaszban, hajtsa végre a következő lépéseket: 

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon7.png)

    a. Válassza ki **metaadatait tartalmazó fájl a számítógépen**, és az Azure Portalról letöltött feltöltési metaadatait tartalmazó fájl.

    b. Kattintson a **Tovább** gombra.

1. Az a **nevét és az egyszeri bejelentkezés helyét** szakaszban, hajtsa végre a következő lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon8.png)
    
    a. Adja hozzá az identitásszolgáltató nevét **identitásszolgáltató neve** (például: az Azure AD) szövegmezőbe.

    b. Kattintson a **Tovább** gombra.

1. Ellenőrizze az aláíró tanúsítványt, és kattintson a **tovább**.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon9.png)

1. Az a **való összefolyás felett felhasználói fiókok** szakaszban, hajtsa végre a következő lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon10.png)

    a. Válassza ki **felhasználók való összefolyás felett a belső könyvtár létrehozása a szükség esetén** , és adja meg a felhasználók számára a megfelelő nevet a csoport (lehet több nem. a vesszővel elválasztott csoportok).

    b. Kattintson a **Tovább** gombra.

1. Kattintson a **Befejezés** gombra.    

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon11.png)

1. Az a **ismert tartományok az Azure ad** szakaszban, hajtsa végre a következő lépéseket: 

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/addon12.png)

    a. Válassza ki **tartományok ismert** az oldal bal oldali panelen.

    b. Adja meg a tartomány nevét a **tartományok ismert** szövegmezőbe.

    c. Kattintson a **Save** (Mentés) gombra. 

> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó létrehozása
Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **az Azure portal**, a bal oldali navigációs panelén kattintson **Azure Active Directory** ikonra.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/kantegassoforconfluence-tutorial/create_aaduser_01.png) 

1. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/kantegassoforconfluence-tutorial/create_aaduser_02.png) 

1. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** a párbeszédpanel tetején.
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/kantegassoforconfluence-tutorial/create_aaduser_03.png) 

1. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőbe írja be a **e-mail-cím** BrittaSimon az.

    c. Válassza ki **jelszó megjelenítése** és jegyezze fel az értékét a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a>Létrehozás egy Kantega egyszeri Bejelentkezéssel való összefolyás felett tesztfelhasználó számára

Ahhoz, hogy az Azure AD-felhasználók való összefolyás felett bejelentkezni, akkor ki kell építeni be való összefolyás felett. Esetén való összefolyás felett Kantega egyszeri Bejelentkezést kiépítés a manuális feladat.

**Üzembe helyez egy felhasználói fiókot, hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be a Kantega egyszeri Bejelentkezéssel való összefolyás felett vállalati hely rendszergazdaként.

1. Vigye a mutatót a fogaskerék alakú ikonjára, majd kattintson a **felhasználókezelés**.

    ![Alkalmazott hozzáadása](./media/kantegassoforconfluence-tutorial/user1.png) 

1. A felhasználók szakaszban kattintson a **felhasználó hozzáadása** fülre. Az a **"Egy felhasználó hozzáadása"** párbeszédpanel lapon, a következő lépésekkel:

    ![Alkalmazott hozzáadása](./media/kantegassoforconfluence-tutorial/user2.png) 

    a. Az a **felhasználónév** szövegmezőbe írja be az e-mailt, felhasználó, például Brittasimon@contoso.com.

    b. Az a **teljes fájlvisszaállítási név** szövegmezőbe írja be például a Britta Simon felhasználó teljes nevét.

    c. Az a **E-mail** szövegmezőbe írja be az e-mail-cím, felhasználó, például Brittasimon@contoso.com.

    d. Az a **jelszó** szövegmezőbe írja be a felhasználó jelszava.

    e. Kattintson a **jelszó megerősítése** írja be újból a jelszót.
    
    f. Kattintson a **Hozzáadás** gombra.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Kantega egyszeri Bejelentkezéssel való összefolyás felett az Azure egyszeri bejelentkezés használatára.

![Felhasználó hozzárendelése][200] 

**Britta Simon rendel az való összefolyás felett Kantega SSO, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

1. Az alkalmazások listájában jelölje ki a **Kantega egyszeri Bejelentkezéssel való összefolyás felett az**.

    ![Egyszeri bejelentkezés konfigurálása](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

1. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

1. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

1. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

1. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

1. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha rákattint a Kantega egyszeri Bejelentkezéssel való összefolyás felett csempe a hozzáférési panelen, kell lekérése automatikusan bejelentkezett a Kantega egyszeri bejelentkezés az alkalmazáshoz való összefolyás felett.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforconfluence-tutorial/tutorial_general_203.png

