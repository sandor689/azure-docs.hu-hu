---
title: 'Oktatóanyag: Trello-Azure Active Directory-integráció |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és a Trello között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 163d7faa99d362f400581c42974eeb4a466641d9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449162"
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Oktatóanyag: Trello-Azure Active Directory-integráció

Ebben az oktatóanyagban elsajátíthatja, hogyan Trello integrálása az Azure Active Directory (Azure AD).

Trello integrálása az Azure ad-ben nyújt a következő előnyökkel jár:

- Szabályozhatja, ki férhet hozzá a Trello Azure AD-ben.
- Engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett trellóhoz (egyszeri bejelentkezés) az Azure AD-fiókjukat.
- A fiókok egyetlen központi helyen – az Azure Portalon kezelheti.

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása a Trello, a következőkre van szükség:

- Az Azure AD-előfizetéshez
- Egy Trello egyszeri bejelentkezéses engedélyezett előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. Trello hozzáadása a katalógusból
1. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-trello-from-the-gallery"></a>Trello hozzáadása a katalógusból
Az Azure AD integrálása a Trello konfigurálásához hozzá kell Trello a galériából a felügyelt SaaS-alkalmazások listájára.

**Trello hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Az Azure Active Directory gomb][1]

1. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![A vállalati alkalmazások panelen][2]
    
1. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Az új alkalmazás gomb][3]

1. A Keresés mezőbe írja be a **Trello**válassza **Trello** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Trello a találatok listájában](./media/trello-tutorial/tutorial_trello_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés Trello a teszt "Britta Simon" nevű felhasználó.

Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó trello mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó trello hivatkozás kapcsolata kell létrehozni.

Trello, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezés a Trello tesztelése és konfigurálása, hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
1. **[Hozzon létre egy Azure ad-ben tesztfelhasználót](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
1. **[Hozzon létre egy Trello-tesztfelhasználót](#create-a-trello-test-user)**  - a-megfelelője a Britta Simon rendelkezik a trello alkalmazásban, amely kapcsolódik az Azure AD felhasználói ábrázolása.
1. **[Rendelje hozzá az Azure ad-ben tesztfelhasználó](#assign-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
1. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és a Trello alkalmazásban egyszeri bejelentkezés konfigurálása.

>[!NOTE]
>Meg kell kapnia a **\<vállalati\>** trello slug. Ha nem rendelkezik a slug érték, lépjen kapcsolatba [Trello-támogatási csoport](mailto:support@trello.com) vállalati beolvasni a slug az Ön számára.
    > 

**Szeretné konfigurálni az Azure AD egyszeri bejelentkezés Trello, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **Trello** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezési hivatkozás konfigurálása][4]

1. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezési párbeszédpanel](./media/trello-tutorial/tutorial_trello_samlbase.png)

1. Az a **Trello-tartomány és URL-címek** szakaszra, ha az alkalmazás a konfigurálni kívánt **Identitásszolgáltató által kezdeményezett mód**, hajtsa végre az alábbi lépéseket:

    ![Trello-tartomány és URL-címeket egyetlen bejelentkezési adatait](./media/trello-tutorial/tutorial_trello_url.png)
    
    a. Az a **azonosító** szövegmezőbe írja be a következő URL-címe: `https://trello.com/auth/saml/metadata`
    
    b. Az a **válasz URL-cím** szövegmezőbe írja be a következő minta használatával URL-címe: `https://trello.com/auth/saml/consume/<enterprise>`

1. Ha az alkalmazás a konfigurálni kívánt **SP által kezdeményezett mód**, hajtsa végre az alábbi lépéseket:

    ![Trello-tartomány és URL-címeket egyetlen bejelentkezési adatait](./media/trello-tutorial/tutorial_trello_url1.png)

    a. Ellenőrizze **speciális URL-beállítások megjelenítése**.

    b. Az a **bejelentkezési URL-** szövegmezőbe írja be a következő minta használatával URL-címe: `https://trello.com/auth/saml/login/<enterprise>` 

1. Trello-alkalmazás vár a SAML helyességi feltételek adott attribútumok tartalmazza. Konfigurálja a következő attribútumok ehhez az alkalmazáshoz. Ezek az attribútumok értékeinek kezelheti a **"Felhasználói attribútumok"** az alkalmazás. Az alábbi képernyőfelvételen látható erre egy példa látható.

    ![Egyszeri bejelentkezés konfigurálása](./media/trello-tutorial/tutorial_trello_attribute.png)

1. Az a **SAML-jogkivonat attribútumai** párbeszédpanelen, az alábbi táblázatban szereplő minden egyes sorára hajtsa végre az alábbi lépéseket:
 
    | Attribútum neve | Attribútum értéke |
    | --- | --- |
    | User.Email | user.mail |
    | User.FirstName | User.givenName |
    | User.LastName | User.surname |

    a. Kattintson a **attribútum hozzáadása** megnyitásához a **attribútum hozzáadása** párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása](./media/trello-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/trello-tutorial/tutorial_attribute_05.png)

    b. Az a **neve** szövegmezőbe írja be azon attribútum nevét, a sorhoz látható. 

    c. Az a **érték** list, írja be az adott sorhoz feltüntetett attribútumot értéket.
    
    d. Kattintson az **OK** gombra. 

1. Az a **SAML-aláíró tanúsítvány** területén kattintson **tanúsítvány (Base64)** , és mentse a metaadat-fájlt a számítógépen.

    ![A tanúsítvány letöltési hivatkozás](./media/trello-tutorial/tutorial_trello_certificate.png) 

1. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gomb konfigurálása](./media/trello-tutorial/tutorial_general_400.png)
    
1. Az a **Trello konfigurációs** területén kattintson **konfigurálása Trello** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **SAML egyszeri bejelentkezési szolgáltatás URL-cím** származó a **gyors útmutató szakaszban.**

    ![Trello-konfiguráció](./media/trello-tutorial/tutorial_trello_configure.png) 

1. Az egyszeri bejelentkezés konfigurálása **Trello** oldalon kell nyissa meg a [Trello vállalati egyszeri bejelentkezési konfiguráció](https://trello.com/sso-configuration) küldése a letöltött lap **tanúsítvány (Base64)** és  **SAML egyszeri bejelentkezési szolgáltatás URL-cím** való [Trello-támogatási csoport](mailto:support@trello.com). Akkor állítsa ezt a beállítást, hogy a SAML SSO-kapcsolat megfelelően állítsa be mindkét oldalon.

> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure ad-ben tesztfelhasználó számára

Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

   ![Hozzon létre egy Azure ad-ben tesztfelhasználó számára][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon, a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.

    ![Az Azure Active Directory gomb](./media/trello-tutorial/create_aaduser_01.png)

1. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/trello-tutorial/create_aaduser_02.png)

1. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** felső részén a **minden felhasználó** párbeszédpanel bezárásához.

    ![A Hozzáadás gombra.](./media/trello-tutorial/create_aaduser_03.png)

1. Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![A felhasználó párbeszédpanel](./media/trello-tutorial/create_aaduser_04.png)

    a. Az a **neve** mezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** mezőbe írja be a felhasználó Britta Simon e-mail-címét.

    c. Válassza ki a **jelszó megjelenítése** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-trello-test-user"></a>Trello tesztfelhasználó létrehozása

Ez a szakasz célja egy trello Britta Simon nevű felhasználó létrehozásához. Trello támogatja a just-in-time-kiépítés, amely alapértelmezésben engedélyezve van. Nincs meg ebben a szakaszban a művelet elem. Új felhasználó próbál hozzáférni a Trello, ha még nem létezik jön létre.

>[!Note]
>Ha manuálisan hozzon létre egy felhasználót, lépjen kapcsolatba kell [Trello-támogatási csoport](mailto:support@trello.com).


### <a name="assign-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Trello Azure egyszeri bejelentkezés használatára.

![A felhasználói szerepkör hozzárendelése][200] 

**Britta Simon rendel Trello, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

1. Az alkalmazások listájában jelölje ki a **Trello**.

    ![Az alkalmazások listáját a Trello hivatkozásra](./media/trello-tutorial/tutorial_trello_app.png)  

1. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![A "Felhasználók és csoportok" hivatkozásra][202]

1. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![A hozzárendelés hozzáadása panel][203]

1. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

1. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

1. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Az egyszeri bejelentkezés vizsgálata

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha a hozzáférési panelen a Trello csempére kattint, meg kell lekérése automatikusan bejelentkezett a Trello-alkalmazásba.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/trello-tutorial/tutorial_general_01.png
[2]: ./media/trello-tutorial/tutorial_general_02.png
[3]: ./media/trello-tutorial/tutorial_general_03.png
[4]: ./media/trello-tutorial/tutorial_general_04.png

[100]: ./media/trello-tutorial/tutorial_general_100.png

[200]: ./media/trello-tutorial/tutorial_general_200.png
[201]: ./media/trello-tutorial/tutorial_general_201.png
[202]: ./media/trello-tutorial/tutorial_general_202.png
[203]: ./media/trello-tutorial/tutorial_general_203.png

