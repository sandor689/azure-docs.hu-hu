---
title: 'Oktatóanyag: Azure Active Directory-integráció az BlueJeans |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és BlueJeans között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: 2ec94217a8df2efaa23eb3cc2c9d5a80e8037615
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425987"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Oktatóanyag: Azure Active Directory-integráció az BlueJeans

Ebben az oktatóanyagban elsajátíthatja, hogyan BlueJeans integrálása az Azure Active Directory (Azure AD).

BlueJeans integrálása az Azure ad-ben nyújt a következő előnyökkel jár:

- Szabályozhatja, hogy ki férhet hozzá BlueJeans Azure AD-ben
- Engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett BlueJeans (egyszeri bejelentkezés) az Azure AD-fiókjukkal
- Kezelheti a fiókokat, egyetlen központi helyen – az Azure Portalon

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

BlueJeans az Azure AD-integráció konfigurálásához a következőkre van szükség:

- Az Azure AD-előfizetéshez
- Egy BlueJeans egyszeri bejelentkezéses engedélyezett előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, beszerezheti a egy egy havi próbalehetőség [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. BlueJeans hozzáadása a katalógusból
1. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-bluejeans-from-the-gallery"></a>BlueJeans hozzáadása a katalógusból
Az Azure AD integrálása a BlueJeans konfigurálásához hozzá kell BlueJeans a katalógusból a felügyelt SaaS-alkalmazások listájára.

**BlueJeans hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra.

    ![Active Directory][1]

1. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![Alkalmazások][2]

1. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

1. A Keresés mezőbe írja be a **BlueJeans**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bluejeans-tutorial/tutorial_bluejeans_search.png)

1. Az eredmények panelen válassza ki a **BlueJeans**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés
Ebben a szakaszban konfigurálja, és a teszt "Britta Simon." nevű felhasználó BlueJeans az Azure AD egyszeri bejelentkezés tesztelése

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó BlueJeans mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó BlueJeans hivatkozás kapcsolata kell létrehozni.

BlueJeans, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezés az BlueJeans tesztelése és konfigurálása, hogy hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
1. **[Az Azure ad-ben tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
1. **[BlueJeans tesztfelhasználó létrehozása](#creating-a-bluejeans-test-user)**  - a-megfelelője a Britta Simon BlueJeans a felhasználó Azure ad-ben reprezentációja kapcsolódó rendelkezik.
1. **[Az Azure ad-ben tesztfelhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
1. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és BlueJeans alkalmazását az egyszeri bejelentkezés konfigurálása.

**Szeretné konfigurálni az Azure AD egyszeri bejelentkezés BlueJeans, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **BlueJeans** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása][4]

1. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.

    ![Egyszeri bejelentkezés konfigurálása](./media/bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

1. Az a **BlueJeans tartomány és URL-címek** szakaszban, hajtsa végre az alábbi lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. Az a **bejelentkezési URL-** szövegmezőbe írja be a következő minta használatával URL-címe: `https://<companyname>.BlueJeans.com`

    b. Az a **azonosító** szövegmezőbe írja be a következő minta használatával URL-címe: `https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > Ezek a értékei nem valódi. Ezek az értékek frissítse a tényleges bejelentkezési URL- és azonosító. Kapcsolattartó [BlueJeans ügyfél-támogatási csapatának](https://support.bluejeans.com/contact) beolvasni ezeket az értékeket.

1. Az a **SAML-aláíró tanúsítvány** területén kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

1. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/bluejeans-tutorial/tutorial_general_400.png)

1. Az a **BlueJeans konfigurációs** területén kattintson **konfigurálása BlueJeans** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **kijelentkezéses URL-cím, jelszó URL-Címének módosítása és SAML egyszeri bejelentkezési szolgáltatás URL-cím** származó a **gyors útmutató szakaszban.**

    ![Egyszeri bejelentkezés konfigurálása](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

1. Egy másik böngészőablakban, jelentkezzen be a **BlueJeans** rendszergazdaként a vállalati webhely.

1. Lépjen a **rendszergazdai \> csoportbeállítások \> biztonsági**.

   ![Rendszergazdai](./media/bluejeans-tutorial/IC785868.png "rendszergazda")

1. Az a **biztonsági** szakaszban, hajtsa végre az alábbi lépéseket:

   ![Egyszeri bejelentkezési SAML](./media/bluejeans-tutorial/IC785869.png "SAML egyszeri bejelentkezés")

   a. Válassza ki **egyszeri bejelentkezési SAML**.

   b. Válassza ki **automatikus kiépítés engedélyezése**.

1. Helyezze az alábbi lépéseket követve:

    ![Tanúsítvány-elérési út](./media/bluejeans-tutorial/IC785870.png "tanúsítvány elérési útja")

    a. Kattintson a **fájl kiválasztása**, majd töltse fel a letöltött tanúsítvány.

    b. Beillesztés **SAML egyszeri bejelentkezési szolgáltatás URL-cím** be a **bejelentkezési URL-cím** szövegmezőbe.

    c. Beillesztés **jelszó URL-Címének módosítása** be a **URL-Címének jelszó módosítása** szövegmezőbe.

    d. Beillesztés **kijelentkezéses URL-cím** be a **kijelentkezési URL-címe** szövegmezőbe.

1. Helyezze az alábbi lépéseket követve:

    ![Módosítások mentése](./media/bluejeans-tutorial/IC785874.png "módosítások mentése")

    a. Az a **felhasználóazonosító** szövegmezőbe írja be `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. Az a **E-mail** szövegmezőbe írja be `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Kattintson a **módosítások mentése**.

### <a name="creating-an-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó létrehozása
Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **az Azure portal**, a bal oldali navigációs panelén kattintson **Azure Active Directory** ikonra.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bluejeans-tutorial/create_aaduser_01.png)

1. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok** kattintson **minden felhasználó**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bluejeans-tutorial/create_aaduser_02.png)

1. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** a párbeszédpanel tetején.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bluejeans-tutorial/create_aaduser_03.png)

1. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/bluejeans-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőbe írja be a **e-mail-cím** BrittaSimon az.

    c. Válassza ki **jelszó megjelenítése** és jegyezze fel az értékét a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.

### <a name="creating-a-bluejeans-test-user"></a>BlueJeans tesztfelhasználó létrehozása

Ez a szakasz célja BlueJeans Britta Simon nevű felhasználó létrehozásához. BlueJeans támogatja a felhasználók automatikus átadása, amely alapértelmezés szerint van engedélyezve. További részleteket talál [Itt](bluejeans-provisioning-tutorial.md) konfigurálásának a felhasználók automatikus átadása.

**Hozza létre a felhasználó manuálisan kell, ha hajtsa végre a következő lépéseket:**

1. Jelentkezzen be a **BlueJeans** rendszergazdaként a vállalati webhely.

1. Lépjen a **rendszergazdai \> felhasználók kezelése \> felhasználó hozzáadása**.

   ![Rendszergazdai](./media/bluejeans-tutorial/IC785877.png "rendszergazda")

   >[!IMPORTANT]
   >A **felhasználó hozzáadása** lap csak akkor érhető el, ha az a **Biztonság lap**, **automatikus kiépítés engedélyezése** nincs bejelölve. 

1. Az a **felhasználó hozzáadása** szakaszban, hajtsa végre az alábbi lépéseket:

    ![Felhasználó hozzáadása](./media/bluejeans-tutorial/IC785886.png "felhasználó hozzáadása")

    a. Adjon meg egy **BlueJeans felhasználónév**, egy **E-mail-cím**, amely egy **BlueJeans értekezlet azonosító**, amely egy **Moderator PIN-kód**, amely egy **teljes neve** , a **vállalati** egy érvényes AAD-fióknevet, amelyet a kapcsolódó szövegmezőkben létrehozásához.

    b. Kattintson a **felhasználó hozzáadása**.

>[!NOTE]
>Bármely más BlueJeans felhasználói fiók létrehozása eszközöket használhatja, vagy az aad-ben a felhasználói fiókok kiépítését BlueJeans által biztosított API-k.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés BlueJeans Azure egyszeri bejelentkezés használatára.

![Felhasználó hozzárendelése][200]

**Britta Simon rendel BlueJeans, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201]

1. Az alkalmazások listájában jelölje ki a **BlueJeans**.

    ![Egyszeri bejelentkezés konfigurálása](./media/bluejeans-tutorial/tutorial_bluejeans_app.png)

1. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202]

1. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

1. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

1. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

1. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha a hozzáférési panelen a BlueJeans csempére kattint, BlueJeans alkalmazás bejelentkezési oldalának szerezheti be.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)
* [Felhasználók átadásának konfigurálása](bluejeans-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
