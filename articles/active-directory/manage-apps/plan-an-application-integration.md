---
title: Ismerkedés az Azure AD integrálása-alkalmazásokba |} A Microsoft Docs
description: Ez a cikk az első lépésekről szóló útmutatót az Azure Active Directory (AD) integrálása a helyszíni alkalmazások és a felhőalapú alkalmazások.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: a7060f9204690e5e7b84693042cecb164c36b45b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366337"
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory integrálása az alkalmazások első lépések útmutató
## <a name="overview"></a>Áttekintés
Ez a témakör célja, hogy egy ütemtervet az alkalmazások integrálása az Azure Active Directory (AD). Az alábbi szakaszokban mindegyike tartalmaz egy részletesebb témakör rövid összefoglalása, így azonosíthatja a kezdeti lépéseket ismertető útmutató mely részei az Ön számára.  Kövesse a hivatkozásokat részletesebben megismerni, minden témában.

## <a name="before-you-begin-take-inventory"></a>Mielőtt elkezdené, leltározása
Mielőtt megkezdi a munkát az alkalmazások integrálása az Azure ad-ben való, fontos tudnia, hol áll, és hol szeretne belépni.  Az alábbi kérdések célja, hogy segítsen, gondolja át az Azure AD-integrációs projektet.

### <a name="application-inventory"></a>Alkalmazásleltár
* Hol találhatók az alkalmazásokat? Kié őket?
* Milyen típusú hitelesítést igényelnek az alkalmazások?
* Ki kell, hogy mely alkalmazásokhoz való hozzáférés?
* Szeretné üzembe helyezni egy új alkalmazást?
  * Lesz, házon belül hozhat létre és hogyan telepítheti egy Azure-beli számítási példányon?
  * Fog használni, amely az Azure Alkalmazásgyűjteményben érhető el?

### <a name="user-and-group-inventory"></a>Felhasználó- és szoftverleltár
* Hol találhatók a a felhasználói fiókokat?
  * Helyszíni Active Directory
  * Azure AD
  * Egy különálló alkalmazás-adatbázis, amely a saját belül
  * Nem engedélyezett alkalmazások
  * A fentiek mindegyikét
* Milyen engedélyeket és a szerepkör-hozzárendeléseket az egyes felhasználók jelenleg rendelkezik? Van szüksége, tekintse át a hozzáférésüket, vagy arra, hogy a felhasználói hozzáférés és a szerepkör-hozzárendelések megfelelő most már Ön?
* Csoportok már meghatározott a helyszíni Active Directoryban?
  * Hogyan vannak rendszerezve a csoportok?
  * Kik a csoport tagjai?
  * Milyen engedélyek/szerepkör-hozzárendeléseit a csoportok jelenleg rendelkezik?
* Kell felhasználó/csoport adatbázisok karbantartása előtt integrálása?  (Ez a kérdés meglehetősen fontos. Szemétgyűjtési a szemétgyűjtési ki.)

### <a name="access-management-inventory"></a>Hozzáférés – leltár
* Hogyan jelenleg kezelhetők az alkalmazásokhoz való felhasználói hozzáférést? Nem kell módosítani?  Tekintette kezelésére, mint például a más módon [RBAC](../../role-based-access-control/role-assignments-portal.md) például?
* Akik milyen hozzáférésre van szüksége?

Talán nem rendelkezik az összes ezekre a kérdésekre adott válaszokat meghozni, de ez nem probléma.  Ez az útmutató segítségével kérdések megválaszolásához és néhány megalapozottabb döntéseket hozhat.

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés és Azure Active Directory címtárhoz.  Ha még nem rendelkezik Azure-előfizetéssel, kipróbálhatja az Azure 30 napig ingyenesen. [Próbálja ki!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Az Azure AD integrációja
### <a name="finding-unsanctioned-cloud-applications-with-cloud-discovery"></a>A Cloud Discovery a keresés nem engedélyezett felhőalkalmazások
Ahogy említettük, az alkalmazásokat, amelyek még nem lett az eddig a szervezet által felügyelt lehet.  A hardverleltározási folyamatának részeként nem engedélyezett felhőalkalmazások megkereséséhez lehetőség. Lásd: [Cloud Discovery beállítása](/cloud-app-security/set-up-cloud-discovery).

### <a name="authentication-types"></a>Hitelesítési típusok
Különböző hitelesítési követelmények vonatkozhatnak a kulcsláncot. Az Azure AD-aláíró tanúsítványok használható SAML 2.0, WS-Federation, vagy OpenID Connect protokollok, valamint jelszó az egyszeri bejelentkezést használó alkalmazások. Alkalmazással kapcsolatos további információk használható az Azure AD-hitelesítési típusok: [tanúsítványok kezelése az összevont egyszeri bejelentkezés az Azure Active Directoryban](manage-certificates-for-federated-single-sign-on.md) és [jelszó alapján egyszeri bejelentkezési](what-is-single-sign-on.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Egyszeri bejelentkezés az Azure AD-alkalmazásproxy engedélyezése
A Microsoft Azure AD-alkalmazásproxy található alkalmazások futnak a magánhálózaton belülről biztonságosan, bárhonnan, bármilyen eszközről hozzáférést biztosíthat. A környezetben egy alkalmazásproxy-összekötő telepítését követően, könnyen konfigurálható az Azure ad-ben.

### <a name="integrating-applications-with-azure-ad"></a>Alkalmazások integrálása az Azure ad-ben
Az alábbi cikkekben különböző módokon alkalmazások integrálása az Azure ad-ben, és útmutatást.

* [Amely meghatározza, hogy mely Active Directory használata](../fundamentals/active-directory-administer.md)
* [Az Azure alkalmazásgyűjteményben alkalmazások használata](what-is-single-sign-on.md)
* [Integrálása az SaaS-alkalmazások oktatóanyagok listáját](../saas-apps/tutorial-list.md)

## <a name="managing-access-to-applications"></a>Alkalmazásokhoz való hozzáférés kezelése
Az alábbi cikkek ismertetik, hogyan alkalmazásokhoz való hozzáférést kezelheti az Azure AD-bA az Azure AD-összekötők és az Azure AD integrálása után.

* [Az Azure AD-vel az alkalmazásokhoz való hozzáférés kezelése](what-is-access-management.md)
* [Az Azure AD-összekötők automatizálása](../active-directory-saas-app-provisioning.md)
* [Felhasználók hozzárendelése egy alkalmazáshoz](../active-directory-applications-guiding-developers-assigning-users.md)
* [Csoportok hozzárendelése egy alkalmazáshoz](../active-directory-applications-guiding-developers-assigning-groups.md)
* [Fiókok megosztása](../active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Egyéni alkalmazások integrálása
Ha egy új alkalmazást és a kívánt a fejlesztők kihasználva az Azure AD-ben talál [Guiding fejlesztők](../active-directory-applications-guiding-developers-for-lob-applications.md).

Ha azt szeretné, adja hozzá az egyéni alkalmazást az Azure Alkalmazásgyűjteményben, lásd: ["Saját alkalmazások használata" Azure AD önkiszolgáló SAML-konfigurációja az](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/).

## <a name="see-also"></a>Lásd még
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](../active-directory-apps-index.md)

