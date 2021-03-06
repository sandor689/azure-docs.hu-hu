---
title: Licenc Azure Active Directory önkiszolgáló jelszó-
description: Az Azure AD önkiszolgáló jelszó-visszaállítási licencelési követelményeket
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 83054c505689768c14d168841764a4557c3e1f8b
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158998"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Az Azure AD önkiszolgáló jelszó-licencelési követelményei alaphelyzetbe állítása

Az Azure Active Directory (Azure AD) négy változatban érhető el: ingyenes, alap-, prémium P1 és prémium P2 szintű. Számos különböző szolgáltatásokat, amelyek be új jelszó önkiszolgáló kérésének, módosítás, például alaphelyzetbe állítása, zárolásának feloldásához és a jelszóvisszaíró, az Azure AD különböző kiadásai által biztosított vannak. Ez a cikk próbál szemléltetik az eltéréseket. Mindegyik Azure AD-kiadás szolgáltatásait, további részletek találhatók a [Azure Active Directory díjszabását ismertető lapon](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Kiadások és szolgáltatások összehasonlítása

Az Azure AD önkiszolgáló jelszó-visszaállítás van licenccel rendelkezniük, a jövőben is megfelel a szervezetek a megfelelő licenc hozzárendelése a felhasználók szükségesek.

* Önkiszolgáló jelszómódosítás felhőfelhasználók számára
   * Én vagyok egy **csak felhőalapú felhasználói** és ismernie kell a jelszót.
      * Szeretném, ha **módosítása** egy új jelszó.
   * Ez a funkció az Azure AD minden kiadása tartalmazza.

* Önkiszolgáló jelszóátállítás felhőfelhasználók számára
   * Én vagyok egy **csak felhőalapú felhasználói** és elfelejtette a jelszót.
      * Szeretném, ha **alaphelyzetbe** valami tudom a jelszavam.
   * Ez a funkció az alapszintű Azure AD, prémium P1 vagy P2 kiadás része.

* Az önkiszolgáló jelszó alaphelyzetbe állítása/módosítás /-Zárolásfeloldás **a helyszíni visszaírással**
   * Én vagyok egy **hibrid felhasználói** saját helyszíni Active Directory felhasználói fiók szinkronizálva van az Azure AD-fiókot az Azure AD Connect használatával. Szeretnék saját jelszó módosítása, elfelejti a jelszót, vagy zárolva lett.
      * Szeretném, ha a jelszó módosítása vagy visszaállítása, valami I ismeri, vagy saját fiók feloldása **és** , hogy szinkronizálja vissza módosítása a helyi Active Directoryban.
   * Ez a funkció Azure AD Premium P1 vagy Premium P2 kiadás tartalmazza.

> [!WARNING]
> Önálló Office 365 licencelési csomagok **nem támogatják a jelszóvisszaírást** , és az Azure AD Premium P1 vagy Premium P2 kiadás esetében ez a funkció működéséhez szükséges.
>

További licencelési információk, beleértve a költségek, a következő lapokon található:

* [Az Azure Active Directory-hely díjszabása](https://azure.microsoft.com/pricing/details/active-directory/)
* [Az Azure Active Directory funkciók és képességek](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [A Microsoft 365 nagyvállalati verzió](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Engedélyezze a csoport- vagy felhasználói alapú licencelése

Most már Azure ad-ben támogatja a Csoportalapú licencelés. A rendszergazdák tömeges licenceket rendelhet a felhasználók, mint az őket egyenként csoportja számára. További információkért lásd: [hozzárendelése, ellenőrzése és licencek kapcsolatos problémák megoldásához](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Nem minden Microsoft-szolgáltatás érhető el minden területen. Mielőtt egy úgy lehet licencet a felhasználóhoz, a rendszergazdának meg kell adnia a **a felhasználási hely** tulajdonság a felhasználóra. Licencek hozzárendelése alapján teheti meg a **felhasználói** > **profil** > **beállítások** szakaszban az Azure Portalon. *A licenc-hozzárendelés használatakor bármely felhasználó felhasználás helyének megadása nélkül örökli a könyvtár helye.*

## <a name="next-steps"></a>További lépések

* [Hogyan végezhető el az SSPR sikeres bevezetése?](howto-sspr-deployment.md)
* [Jelszó visszaállítása vagy módosítása](../user-help/active-directory-passwords-update-your-own-password.md)
* [Regisztráció önkiszolgáló jelszó-visszaállításra](../user-help/active-directory-passwords-reset-register.md)
* [Milyen adatokat használ az SSPR, és milyen adatokat kell kitöltenie a felhasználók számára?](howto-sspr-authenticationdata.md)
* [Milyen hitelesítési módszerek érhetők el a felhasználók számára?](concept-sspr-howitworks.md#authentication-methods)
* [Mik az SSPR szabályzatbeállításai?](concept-sspr-policy.md)
* [Mi a jelszóvisszaíró, és miért fontos?](howto-sspr-writeback.md)
* [Hogyan készíthető jelentés az SSPR-ben végzett tevékenységekről?](howto-sspr-reporting.md)
* [Mik az SSPR beállításai, és mit jelentenek?](concept-sspr-howitworks.md)
* [Azt hiszem, hogy valami nem működik. Hogyan háríthatom el az SSPR hibáit?](active-directory-passwords-troubleshoot.md)
* [Olyan kérdésem van, amely máshol nem szerepelt](active-directory-passwords-faq.md)
