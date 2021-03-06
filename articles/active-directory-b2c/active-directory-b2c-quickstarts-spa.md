---
title: Rövid útmutató – Bejelentkezés beállítása egyoldalas alkalmazáshoz az Azure Active Directory B2C használatával | Microsoft Docs
description: Azure Active Directory B2C-alapú fiókbejelentkezést támogató egyoldalas mintaalkalmazás futtatása.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.date: 7/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 155cdaf51ac5725a315259a0d809ba644f64110c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048916"
---
# <a name="quickstart-set-up-sign-in-for-a-single-page-app-using-azure-active-directory-b2c"></a>Rövid útmutató: Bejelentkezés beállítása egyoldalas alkalmazáshoz az Azure Active Directory B2C használatával

Az Azure Active Directory (Azure AD) B2C felhőalapú identitáskezelést nyújt az alkalmazás, az üzlet és az ügyfelek védelme érdekében. Az Azure AD B2C nyílt szabványú protokollokkal teszi lehetővé az alkalmazások hitelesítését közösségi hálózati és vállalati fiókokon.

Ebben a gyors útmutatóban az Azure AD B2C-t használó egyoldalas mintaalkalmazással jelentkezik be egy közösségi identitásszolgáltatót használva, és az Azure AD B2C által védett webes API-t hív meg.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Előfeltételek

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) az **ASP.NET és webfejlesztési** számítási feladattal.
* [Node.js](https://nodejs.org/en/download/) telepítése
* Egy Facebook-fiók.

## <a name="download-the-sample"></a>A minta letöltése

[Töltse le a zip-fájlt](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip), vagy a klónozza a mintául szolgáló webalkalmazást a GitHubról.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="run-the-sample-application"></a>A mintaalkalmazás futtatása

A minta futtatása a Node.js parancssorból: 

```
cd active-directory-b2c-javascript-msal-singlepageapp
npm install && npm update
node server.js
```

A Node.js-alkalmazás megadja a helyi gazdagépen figyelt port számát.

```
Listening on port 6420...
```

Nyissa meg az alkalmazás `http://localhost:6420` URL-címét egy webböngészőben.

![Mintaalkalmazás a böngészőben](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

## <a name="create-an-account"></a>Fiók létrehozása

Kattintson a **Login** (Bejelentkezés) gombra az Azure AD B2C **regisztrációs vagy bejelentkezési** munkafolyamatának elindításához egy Azure AD B2C-szabályzat alapján. 

A minta több regisztrációs beállítást is hivatott támogatni, beleértve helyi fiók e-mail-címmel való létrehozását. Ebben a rövid útmutatóban egy Facebook-fiókot használunk. 

### <a name="sign-up-using-a-social-identity-provider"></a>Regisztráció közösségi identitásszolgáltatóval

Az Azure AD B2C a minta-webalkalmazáshoz egy Wingtip Toys nevű fiktív márka egyéni bejelentkezési lapját jeleníti meg. 

1. Ha közösségi identitásszolgáltatóval szeretne regisztrálni, kattintson a Facebook-identitásszolgáltató gombjára.

    Hitelesíti magát (bejelentkezik) a közösségi fiók hitelesítő adataival, és feljogosítja az alkalmazást, hogy beolvassa a közösségi fiók adatait. A hozzáférés biztosításával az alkalmazás profiladatokat kérhet le a közösségi fiókból, például a nevét és a települését. 

2. A hitelesítő adatai megadásával fejezze be az identitásszolgáltató bejelentkezési folyamatát.

    Az új fiókprofil részletei előre ki vannak töltve a közösségi fiókja adataival. 

3. Frissítse a Display Name (Megjelenített név), a Job Title (Beosztás) és a City (Város) mezőt, majd kattintson a **Continue** (Folytatás) gombra.  A rendszer a beírt értékeket használja az Azure AD B2C felhasználói fiókprofilhoz.

    Sikeresen létrehozott egy új, identitásszolgáltatót használó Azure AD B2C felhasználói fiókot. 

## <a name="access-a-protected-web-api-resource"></a>Védett webes API-erőforrás elérése

Kattintson a **Call Web API** (Webes API hívása) gombra a megjelenítendő név JSON-objektumként való visszaadásához a webes API meghívásából. 

![Webes API válasza](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

Az egyoldalas mintaalkalmazás egy Azure AD hozzáférési jogkivonatot tartalmaz a védett webes API-erőforrás felé, a JSON-objektum visszaadására szolgáló művelet végrehajtására irányuló kérésben.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Az Azure AD B2C-bérlőt ahhoz is használhatja, ha más Azure AD B2C gyors útmutatókat vagy oktatóanyagokat is ki szeretne próbálni. Ha már nincs szüksége rá, akkor [törölheti az Azure AD B2C-bérlőt](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>További lépések

Ebben a gyors útmutatóban az Azure AD B2C-t használó, mintául szolgáló ASP.NET-es alkalmazással bejelentkezett egy egyéni bejelentkezési oldalon, bejelentkezett egy közösségi identitásszolgáltatót használva, létrehozott egy Azure AD B2C-fiókot, és meghívott egy, az Azure AD B2C által védett webes API-t. 

A következő lépés egy saját Azure AD B2C-bérlő létrehozása és a minta konfigurálása a bérlővel való futtatáshoz. 

> [!div class="nextstepaction"]
> [Azure Active Directory B2C-bérlő létrehozása az Azure Portalon](tutorial-create-tenant.md)