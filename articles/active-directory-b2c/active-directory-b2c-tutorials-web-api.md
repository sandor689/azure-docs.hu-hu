---
title: Oktatóanyag – ASP.NET webes API-hoz való hozzáférés engedélyezése egy webalkalmazásból az Azure Active Directory B2C használatával | Microsoft Docs
description: Arra vonatkozó útmutató, hogyan használhatja az Active Directory B2C-t egy ASP.NET webes API védelmére és meghívására egy ASP.NET-webalkalmazásból.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 01/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 469a3662b5bc4db467dde3285d557ac8bbae368e
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39609089"
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-from-a-web-app-using-azure-active-directory-b2c"></a>Oktatóanyag: ASP.NET webes API-hoz való hozzáférés engedélyezése egy webalkalmazásból az Azure Active Directory B2C használatával

Az oktatóanyag azt mutatja be, hogyan hívhat meg egy Azure Active Directory (Azure AD) B2C-vel védett webes API-erőforrást az ASP.NET-webalkalmazásból.

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Webes API regisztrálása az Azure AD B2C-bérlőben
> * Webes API hatóköreinek meghatározása és konfigurálása
> * Alkalmazásengedélyek megadása a webes API számára
> * Mintakód frissítése egy webes API Azure AD B2C-vel történő védelméhez

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Előfeltételek

* [Az Azure Active Directory B2C használata felhasználói hitelesítéshez egy ASP.NET-webalkalmazásban – oktatóanyag](active-directory-b2c-tutorials-web-app.md) elvégzése.
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) telepítése **ASP.NET és webfejlesztési** számítási feladattal.

## <a name="register-web-api"></a>Webes API regisztrálása

A webes API-erőforrásoknak regisztrálva kell lenniük a bérlőben, mielőtt fogadni és válaszolni tudnának az [ügyfélalkalmazások](../active-directory/develop/developer-glossary.md#client-application) által leadott, [védett erőforrásokra vonatkozó kérelmekre](../active-directory/develop/developer-glossary.md#resource-server), amelyekhez egy, az Azure Active Directoryból származó [hozzáférési jogkivonat](../active-directory/develop/developer-glossary.md#access-token) tartozik. A regisztráció meghatározza az [alkalmazás- és szolgáltatásnév-objektumot](../active-directory/develop/developer-glossary.md#application-object) a bérlőben. 

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/) az Azure AD B2C-bérlő globális rendszergazdájaként.

2. Úgy győződhet meg arról, hogy az Azure AD B2C-bérlőt tartalmazó könyvtárt használja, hogy átvált rá az Azure Portal jobb felső sarkában. Jelölje ki előfizetői adatait, majd válassza a **Könyvtárváltás** lehetőséget.

    ![Könyvtár váltása](./media/active-directory-b2c-tutorials-web-api/switch-directories.png)

3. Válassza ki a bérlőjét tartalmazó könyvtárt.

    ![Könyvtár kijelölése](./media/active-directory-b2c-tutorials-web-api/select-directory.png)

4. Válassza az Azure Portal bal felső sarkában található **Minden szolgáltatás** lehetőséget, majd keresse meg és válassza ki az **Azure AD B2C**-t. Ha sikerült, akkor most az előző oktatóanyagban létrehozott bérlőt használja.

5. Válassza az **Alkalmazások**, majd a **Hozzáadás** elemet.

    A mintául szolgáló webes API bérlőben történő regisztrálásához használja a következő beállításokat.
    
    ![Új API hozzáadása](./media/active-directory-b2c-tutorials-web-api/web-api-registration.png)
    
    | Beállítás      | Ajánlott érték  | Leírás                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Name (Név)** | Mintául szolgáló saját webes API | Adjon meg egy olyan **nevet**, amely megfelelően körülírja a webes API-t a felhasználók számára. |
    | **Webalkalmazás vagy webes API szerepeltetése** | Igen | Válassza az **Igen** lehetőséget a webes API-k esetén. |
    | **Implicit folyamat engedélyezése** | Igen | Válassza az **Igen** lehetőséget, mivel az API [OpenID Connect bejelentkezést](active-directory-b2c-reference-oidc.md) használ. |
    | **Válasz URL-cím** | `https://localhost:44332` | A válasz URL-címek olyan végpontok, amelyeken keresztül az Azure AD B2C visszaadja az API által kért jogkivonatokat. Ebben az oktatóanyagban a mintául szolgáló webes API helyileg fut (localhost), és a 44332-es porton figyel. |
    | **Alkalmazásazonosító URI** | myAPISample | Az URI egyedileg azonosítja az API-t a bérlőben. Ez lehetővé teszi, hogy bérlőnként több API-t is regisztráljon. A [hatókörök](../active-directory/develop/developer-glossary.md#scopes) szabályozzák a hozzáférést a védett API-erőforrásokhoz, és alkalmazásazonosító URI-nként vannak meghatározva. |
    | **Natív ügyfél** | Nem | Mivel ez egy webes API, nem pedig egy natív ügyfél, válassza a Nem lehetőséget. |
    
6. Kattintson a **Létrehozás** gombra az API regisztrálásához.

A regisztrált API-k az Azure AD B2C-bérlő alkalmazásainak listájában jelennek meg. Válassza ki webes API-ját a listáról. Ekkor megjelenik a webes API tulajdonságpanelje.

![Webes API – Tulajdonságok](./media/active-directory-b2c-tutorials-web-api/b2c-web-api-properties.png)

Jegyezze fel az **alkalmazás ügyfél-azonosítóját**. Az azonosító egyedi módon azonosítja az API-t, és az oktatóanyag későbbi részében, az alkalmazás konfigurálásakor lesz rá szükség.

A webes API Azure AD B2C-vel végzett regisztrációja egy megbízhatósági kapcsolatot határoz meg. Mivel az API B2C-vel van regisztrálva, megbízhat a más alkalmazásoktól kapott B2C hozzáférési jogkivonatokban.

## <a name="define-and-configure-scopes"></a>Hatókörök meghatározása és konfigurálása

A [hatókörök](../active-directory/develop/developer-glossary.md#scopes) lehetőséget nyújtanak a védett erőforrásokhoz való hozzáférés szabályozására. A hatóköröket a webes API a hatóköralapú hozzáférés-vezérlés megvalósításához használja. A webes API-k bizonyos felhasználói például rendelkezhetnek olvasási és írási hozzáféréssel is, míg mások csak olvasási hozzáféréssel. Ebben az oktatóanyagban hatókörök segítségével határozzuk meg az olvasási és írási engedélyeket a webes API számára.

### <a name="define-scopes-for-the-web-api"></a>A webes API hatóköreinek meghatározása

A regisztrált API-k az Azure AD B2C-bérlő alkalmazásainak listájában jelennek meg. Válassza ki webes API-ját a listáról. Ekkor megjelenik a webes API tulajdonságpanelje.

Kattintson a **Közzétett hatókörök (előzetes verzió)** gombra.

Az API hatóköreinek konfigurálásához adja meg a következő bejegyzéseket. 

![a webes API-ban meghatározott hatókörök](media/active-directory-b2c-tutorials-web-api/scopes-defined-in-web-api.png)

| Beállítás      | Ajánlott érték  | Leírás                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Hatókör** | Hello.Read | Olvasási hozzáférés a Hellóhoz |
| **Hatókör** | Hello.Write | Írásii hozzáférés a Hellóhoz |

Kattintson a **Save** (Mentés) gombra.

A közzétett hatókörök segítségével ügyfélalkalmazás-engedélyeket biztosíthat a webes API-nak.

### <a name="grant-app-permissions-to-web-api"></a>Alkalmazásengedélyek megadása a webes API-nak

Egy védett webes API alkalmazásból történő hívásához alkalmazásengedélyeket kell biztosítania az API számára. Ebben az oktatóanyagban használja azt a webalkalmazást, amely az [Azure Active Directory B2C használata felhasználói hitelesítéshez egy ASP.NET-webalkalmazásban](active-directory-b2c-tutorials-web-app.md) című oktatóanyagban lett létrehozva. 

1. Válassza ki az Azure Portalon az **Azure AD B2C** elemet a szolgáltatások listájáról, majd kattintson az **Alkalmazások** lehetőségre a regisztrált alkalmazások listájának megjelenítéséhez.

2. Válassza ki a **Mintául szolgáló saját webalkalmazás** elemet az alkalmazáslistából, és kattintson az **API-hozzáférés (előzetes verzió)**, majd a **Hozzáadás** gombra.

3. Az **API kiválasztása** legördülő menüben válassza ki a regisztrált webes API-t: **Mintául szolgáló saját webes API**.

4. A **Hatókörök kiválasztása** legördülő menüben válassza ki azt a hatókört, amelyet a webes API regisztrációja során megadott.

    ![alkalmazás hatóköreinek kiválasztása](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. Kattintson az **OK** gombra.

A **Mintául szolgáló saját webalkalmazás** regisztrálva van a védett **Mintául szolgáló saját webes API** hívásához. A webalkalmazás használatához a felhasználó az Azure AD B2C-vel [hitelesíti magát](../active-directory/develop/developer-glossary.md#authentication). A webalkalmazás lekéri az [engedélyezést](../active-directory/develop/developer-glossary.md#authorization-grant) az Azure AD B2C-ből a védett webes API-hoz való hozzáféréshez.

## <a name="update-code"></a>Kód frissítése

Most, hogy regisztrálta a webes API-t és meghatározta a hatóköröket, konfigurálnia kell a webes API kódját az Azure AD B2C-bérlő használatához. Ebben az oktatóanyagban egy mintául szolgáló webes API-t fog konfigurálni. 

A mintául szolgáló webes API-t az előfeltételként megadott oktatóanyag elvégzése során letöltött projekt tartalmazza: [Az Azure Active Directory B2C használata felhasználói hitelesítéshez egy ASP.NET-webalkalmazásban – oktatóanyag](active-directory-b2c-tutorials-web-app.md). Ha még nem végezte el az előfeltételnek számító oktatóanyagot, kerítsen erre sort, mielőtt továbblépne.

Két projekt szerepel a mintául szolgáló megoldásban:

**Mintául szolgáló webalkalmazás (TaskWebApp):** webalkalmazás feladatlista létrehozáshoz és szerkesztéséhez. A webalkalmazás a **regisztrálási vagy bejelentkezési** szabályzatot arra, hogy e-mail-címmel regisztráljon vagy jelentkeztessen be felhasználókat.

**Mintául szolgáló webes API-alkalmazás (TaskService):** webes API, amely támogatja a feladatlista létrehozását, olvasását, frissítését és törlését. A webes API-nak az Azure AD B2C biztosít védelmet, és a webalkalmazással hívható meg.

A mintául szolgáló webalkalmazás és a webes API a konfigurációs értékeket alkalmazásbeállításokként határozza meg az egyes projektek Web.config fájljában.

Nyissa meg a **B2C-WebAPI-DotNet** megoldást a Visual Studióban.

### <a name="configure-the-web-app"></a>A webalkalmazás konfigurálása

1. Nyissa meg a **Web.config** elemet a **TaskWebApp** projektben.

2. Az API helyi futtatásához használja az **api:TaskServiceUrl** localhost-beállítást. Az alábbiak szerint módosítsa a Web.config fájlt: 

    ```C#
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. Konfigurálja az API URI-ját. Ezt az URI-t a webalkalmazás az API-kérelem leadásához használja. A kért engedélyeket is konfigurálja.

    ```C#
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/myAPISample/" />
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

### <a name="configure-the-web-api"></a>A webes API konfigurálása

1. Nyissa meg a **Web.config** elemet a **TaskService** projektben.

2. Konfigurálja az API-t a bérlő használatához.

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. Állítsa be az ügyfél-azonosítót az API regisztrált alkalmazásazonosítójára.

    ```C#
    <add key="ida:ClientId" value="<The Application ID for your web API obtained from the Azure portal>"/>
    ```

4. Frissítse a szabályzat beállítását a regisztrálási és bejelentkezési szabályzat létrehozásakor megadott névvel.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="B2C_1_SiUpIn" />
    ```

5. Konfigurálja a hatókör beállításait, hogy egyezzenek a portálon korábban létrehozottakkal.

    ```C#
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample"></a>Minta futtatása

A **TaskWebApp** és a **TaskService** projektet is futtatnia kell. 

1. A Megoldáskezelőben kattintson a jobb gombbal a megoldásra, és válassza az **Indítási projektek beállítása...** lehetőséget. 
2. Válassza ki a **Több kezdőprojekt** választógombot.
3. Mindkét projektnél módosítsa a **Művelet** értékét **Indításra**.
4. Kattintson az OK gombra a konfiguráció mentéséhez.
5. Nyomja le az **F5** gombot mindkét alkalmazás futtatásához. Mindegyik alkalmazás saját böngészőlapon nyílik meg. A `https://localhost:44316/` a webalkalmazás.
    `https://localhost:44332/` a webes API.

6. A webalkalmazásban kattintson a menüsorban a regisztráció/bejelentkezés hivatkozásra, hogy regisztráljon a webalkalmazásra. Használja a [webalkalmazással foglalkozó oktatóanyagban](active-directory-b2c-tutorials-web-app.md) létrehozott fiókot. 
7. Miután bejelentkezett, kattintson a **Feladatlista** hivatkozásra, és hozzon létre egy feladatlista elemet.

Amikor létrehoz egy feladatlista elemet, a webalkalmazás egy, a feladatlista elem létrehozására vonatkozó kérést küld a webes API-nak. A védett webalkalmazás hívja a védett webes API-t az Azure AD B2C-bérlőben.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Az Azure AD B2C-bérlőt ahhoz is használhatja, ha más Azure AD B2C-oktatóanyagokat is ki szeretne próbálni. Ha már nincs szüksége rá, akkor [törölheti az Azure AD B2C-bérlőt](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>További lépések

Ez a cikk azon vezette végig, hogyan biztosíthat védelmet egy ASP.NET webes API-nak hatókörök Azure AD B2C-ben való regisztrálásával és meghatározásával. A következő oktatóanyag további részleteket tartalmaz a forgatókönyv fejlesztéseivel kapcsolatban, beleértve a kódok bemutatásait is.

> [!div class="nextstepaction"]
> [Azure Active Directory B2C-regisztrációt, -bejelentkezést, -profilszerkesztést és -jelszókérést használó ASP.NET-webalkalmazás létrehozása](active-directory-b2c-devquickstarts-web-dotnet-susi.md)