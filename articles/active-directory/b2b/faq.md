---
title: Az Azure Active Directory B2B-együttműködés – gyakori kérdések |} A Microsoft Docs
description: Válaszok az Azure Active Directory B2B-együttműködés – gyakori kérdések.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 1ff2332e2867f99492d3da254282498d1627a5bf
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059899"
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Az Azure Active Directory B2B-együttműködés – gyakori kérdések

Ezek az Azure Active Directory (Azure AD)-vállalatközi (B2B) együttműködés kapcsolatos gyakori kérdések (GYIK) rendszeres időközönként frissülnek az új témaköröket tartalmazza.

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Hogy testre a bejelentkezési oldalát, hogy a B2B együttműködési vendégfelhasználók számára intuitívabb legyen?
Feltétlenül! Tekintse meg a [funkcióról szóló blogbejegyzést](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). A munkahelyi bejelentkezési lap testreszabásával kapcsolatos további információkért lásd: [adja hozzá a vállalati arculat megjelenítése a bejelentkezés és a hozzáférési Panel oldalakon](../fundamentals/customize-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>A SharePoint Online és onedrive vállalati verzió hozzáférhet B2B együttműködési felhasználókat?
Igen. Van azonban arra, hogy keresse meg a SharePoint online-ban meglévő vendégfelhasználókat a személyválasztója segítségével **ki** alapértelmezés szerint. Kapcsolja be a beállítást meglévő vendég felhasználók kereséséhez, állítsa **ShowPeoplePickerSuggestionsForGuestUsers** való **a**. Ez a beállítás bekapcsolhatja vagy bérlői szinten vagy a gyűjtemény szintjén. Ez a beállítás a Set-SPOTenant és Set-SPOSite parancsmagok használatával módosíthatja. Ezeket a parancsmagokat a tagok kereshet az összes meglévő vendégfelhasználókat a címtárban. A bérlői hatókörben módosítása nem érinti a SharePoint Online-webhelyekkel már kiépített.

### <a name="is-the-csv-upload-feature-still-supported"></a>A fürt megosztott kötetei szolgáltatás feltöltési funkció továbbra is támogatott?
Igen. A .csv fájl feltöltési funkció használatával kapcsolatos további információkért lásd: [a PowerShell-mintát](code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Hogyan szabhatja testre a meghívó e-mailt?
Testre szabhatja a meghívó folyamatra vonatkozó szinte mindent használatával a [B2B meghívó API-k](customize-invitation-api.md).

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Vendégfelhasználók alaphelyzetbe állíthatja a multi-factor Authentication hitelesítési módszert?
Igen. Vendégfelhasználók alaphelyzetbe állíthatja a multi-factor authentication módszer ugyanúgy, ahogy a normál felhasználók hogyan használják.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Melyik szervezetben felelős a multi-factor authentication licencek?
A meghívó szervezetet a multi-factor Authentication hitelesítést hajt végre. A meghívó szervezetet ellenőrizze, hogy a szervezete rendelkezik-e elegendő licenccel a B2B-felhasználók számára a többtényezős hitelesítés használata esetén.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Mi történik, ha egy fiókpartner-szervezet már rendelkezik multi-factor authentication szolgáltatás beállítása? Is azt a multi-factor authentication megbízható, és nem a saját multi-factor authentication használata?
Ez a funkció egy későbbi kiadásban tervezünk, akkor választhatja ki kizárandó konkrét partnerek, a többtényezős hitelesítés (a meghívó szervezetet meg).

### <a name="how-can-i-use-delayed-invitations"></a>Hogyan használható a késleltetett felkérések?
Egy szervezet hozzáadása a B2B-együttműködés felhasználók, alkalmazások szükség szerint építse és majd a Meghívók küldése kíván. A B2B-együttműködés meghívó API segítségével testre szabhatja a regisztrációs munkafolyamat.

### <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Készíthetek vendégfelhasználók látható az Exchange globális címlista?
Igen. Alapértelmezés szerint Vendég objektumok nem láthatók a szervezet globális címlista, de használhatja az Azure Active Directory PowerShell láthatóvá tegye őket. További információkért lásd: **lehet Vendég objektumok a globális címlista látható?** a [vendég hozzáférés az Office 365-csoportok](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ).

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Tehetők a vendégfelhasználó korlátozott rendszergazda?
Abszolút. További információkért lásd: [vendég felhasználók hozzáadása szerepkörökhöz](add-guest-to-role.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Azure AD B2B együttműködés lehetővé teszi a B2B-felhasználók az Azure portal eléréséhez?
Hacsak egy felhasználó a korlátozott rendszergazda vagy a globális rendszergazdai szerepkör van hozzárendelve, B2B együttműködési felhasználókat az Azure Portalon való hozzáférés nem szükséges. Azonban a B2B együttműködés a felhasználók, akik a korlátozott rendszergazda vagy a globális rendszergazdai szerepkörhöz vannak hozzárendelve a portálon hozzáférhetnek. Ezenkívül ha vendégfelhasználó, aki nincs hozzárendelve egyik rendszergazdai szerepkör hozzáfér a portálon, a felhasználó lehet érhessék el az egyes részeit a felhasználói élményt. A Vendég felhasználói szerep néhány engedélyt a címtárban.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Letilthatom az Azure Portalon a vendégfelhasználók számára a hozzáférést?
Igen! Ez a szabályzat konfigurálásakor kell arra, hogy elkerülje a tagjának és rendszergazdájának véletlenül letiltja a hozzáférést.
A vendégfelhasználó való hozzáférés letiltását a [az Azure portal](https://portal.azure.com), használja a feltételes hozzáférési szabályzat a Windows Azure klasszikus üzemi modell API-ban:
1. Módosítsa a **minden felhasználó** csoportot, hogy csak a tagok tartalmaz.
  ![módosíthatja a csoport képernyőképe](media/faq/modify-all-users-group.png)
2. Vendégfelhasználók tartalmazó dinamikus csoport létrehozása.
  ![képernyőfelvétel a csoport létrehozása](media/faq/group-with-guest-users.png)
3. Állítsa be feltételes hozzáférési szabályzat vendég felhasználók számára a portál hozzáférjen az alábbi videóban látható módon:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>A multi-factor authentication és a fogyasztói e-mail-fiókokat támogatja az Azure AD B2B együttműködés?
Igen. Többtényezős hitelesítés és a fogyasztói e-mail-fiókokat az Azure AD B2B együttműködés támogatottak.

### <a name="do-you-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Tervezi támogatja az új jelszó kérését az Azure AD B2B együttműködési felhasználókat?
Igen. Itt találhatók az önkiszolgáló jelszó-visszaállítás (SSPR) a B2B-felhasználó, akit meghívtak az egy fiókpartner-szervezet fontos részletei:
 
* SSPR csak a B2B-felhasználó szolgáltatásidentitás bérlője történik.
* Ha a szolgáltatásidentitás bérlője egy Microsoft-fiókkal, a Microsoft-fiókot az SSPR mechanizmus szolgál.
* Ha a szolgáltatásidentitás bérlője egy – igény (szerinti JIT) vagy "vírusos" bérlői, egy jelszó alaphelyzetbe állítása e-mailt küld.
* Más bérlők számára a standard szintű SSPR folyamat után B2B-felhasználók számára következik. Például a B2B-felhasználók, az erőforrás kontextusában SSPR tagot bérlős üzemmód le van tiltva. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Jelszó alaphelyzetbe áll egy – igény (szerinti JIT) vendég felhasználók vagy a "vírusos" bérlő számára elfogadható meghívót a munkahelyi vagy iskolai e-mail-cím, de akiknek nem kell egy már meglévő Azure AD-fiókot?
Igen. A jelszó alaphelyzetbe állítása mail elküldött, amely lehetővé teszi a felhasználóknak új jelszót kérnek az igény szerinti bérlet.

### <a name="does-microsoft-dynamics-365-provide-online-support-for-azure-ad-b2b-collaboration"></a>Nem a Microsoft Dynamics 365 online-támogatást nyújt az Azure AD B2B együttműködés?
Igen, Dynamics 365 (online) támogatást nyújt az Azure AD B2B együttműködés. További információkért tekintse meg a Dynamics 365 cikket [meghívása a felhasználók az Azure AD B2B együttműködés](https://docs.microsoft.com/dynamics365/customer-engagement/admin/invite-users-azure-active-directory-b2b-collaboration).

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Mi az, hogy egy újonnan létrehozott B2B együttműködés felhasználó kezdeti jelszavát élettartama?
Az Azure AD rendelkezik egy rögzített karakter, a jelszó erősségét és a fiók zárolása követelmények egyaránt érvényesek minden Azure ad felhőalapú felhasználói fiókok. Felhőalapú felhasználói fiókok olyan fiókok, nem összevont más identitásszolgáltatóval, például 
* Microsoft-fiók
* Facebook
* Active Directory összevonási szolgáltatások
* Egy másik felhőalapú bérlőre (a B2B-együttműködés)

Összevont fiókok esetében jelszóházirendet a házirendet, amely a helyszíni bérlős és a felhasználó Microsoft-fiók beállításainak alkalmazása függ.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Egy szervezet szüksége lehet a különböző környezeteket bérlői és vendég felhasználók az alkalmazásokba. A standard szintű útmutatást van? Az identitásszolgáltató jelenléte jogcím a megfelelő modellt?
 Vendégfelhasználó használhatja bármely identitásszolgáltató hitelesítésére. További információkért lásd: [B2B együttműködés felhasználói tulajdonságok](user-properties.md). Használja a **UserType** tulajdonság határozza meg a felhasználói élmény. A **UserType** jogcím jelenleg nem szerepel a jogkivonatban. A felhasználó számára a címtár lekérdezésére, és a UserType beolvasásához, alkalmazások a Graph API-t kell használnia.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>Hol található a B2B együttműködés közösségi megoldások megosztása és ötleteket szeretne elküldeni?
Folyamatosan figyelünk a B2B-együttműködés javítása érdekében a visszajelzés. Felkérjük, hogy a felhasználó megosztása forgatókönyvek, ajánlott eljárások és mi tetszik Önnek az Azure AD B2B együttműködés. Csatlakozzon a beszélgetéshez a [a Microsoft technikai Közösség](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
Azt is meghívhat, akkor küldje be az ötletek és a későbbi funkciókkal, szavazzon [B2B együttműködés ötleteket](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>Is küldünk egy meghívást arra, hogy automatikusan beváltott, úgy, hogy a felhasználó csak "készen áll"? Vagy a felhasználó mindig rendelkezik, kattintással lépjen a beváltási URL-cím?
Egy meghívót küldő személy meghívhatja a a fiókpartner-szervezet többi felhasználója a felhasználói felület, a PowerShell-parancsprogramok használatával vagy API-k. Ezt követően a meghívót küldő személy küldhet a vendégfelhasználó egy közvetlen hivatkozást egy megosztott alkalmazás. A legtöbb esetben már nem látható az e-mailes meghívót megnyitásához, majd kattintson a beváltási URL-cím szükséges. További információkért lásd: [Azure Active Directory B2B együttműködés vendégmeghívás beváltása](redemption-experience.md).

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Hogyan működik a B2B-együttműködés a Ha a meghívott partner összevonási segítségével adja hozzá a saját helyszíni hitelesítéssel?
Ha a partner van összevonva a helyszíni hitelesítési infrastruktúráját az Azure AD-bérlő, a helyszíni egyszeri bejelentkezés (SSO) automatikusan érhető el. Ha a partner Azure AD-bérlő nem rendelkezik, az Azure AD-fiók jön létre az új felhasználók számára. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>E úgy Gondoltuk, az Azure AD B2B nem fogadja el a gmail.com és az Outlook.com-os e-mail-címeket, és a B2C használatát az ilyen típusú fiókokat?
B2B és üzleti – fogyasztói (B2C) együttműködés szempontjából, amely az identitások támogatottak közötti különbségekről megszüntetjük. Az identitásnak nem választhat B2B használatával, vagy pedig a B2C jó oka áll. Az együttműködési lehetőség kiválasztásával kapcsolatban további információkért lásd: [összehasonlítása B2B-együttműködés és az Azure Active Directory B2C](compare-with-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Milyen alkalmazásokat és szolgáltatásokat támogatják az Azure B2B vendégfelhasználók?
Minden Azure AD-val integrált alkalmazásokat támogatja az Azure B2B vendégfelhasználókat. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Hogy kényszerítheti a többtényezős hitelesítés B2B vendégfelhasználók Ha partnerei nem rendelkeznek a multi-factor authentication?
Igen. További információkért lásd: [feltételes hozzáférés B2B-együttműködés felhasználók](conditional-access.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>A SharePoint megadhatja a külső felhasználók számára egy "engedélyezheti" vagy "Elutasítás" listában. Hogy ehhez az Azure-ban?
Igen. Az Azure AD B2B együttműködés támogatja engedélyezési listák, és letiltási listáját. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Milyen licencek szükségünk van az Azure AD B2B használata?
Milyen licencek, a szervezet kell használni az Azure AD B2B kapcsolatos információkért lásd: [licencelési útmutató Azure Active Directory B2B együttműködés](licensing-guidance.md).

### <a name="next-steps"></a>További lépések

- [Mi az az Azure AD B2B együttműködés?](what-is-b2b.md)

