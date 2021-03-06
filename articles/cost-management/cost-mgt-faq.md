---
title: Gyakori kérdések az Azure Cost Managementben |} A Microsoft Docs
description: Itt az Azure Cost Management kapcsolatos gyakori kérdésekre adott válaszokat.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 07/20/2018
ms.topic: troubleshooting
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 255056390cdbdbee49eba47f8168618929b386c8
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187257"
---
# <a name="frequently-asked-questions-for-azure-cost-management"></a>Gyakori kérdések az Azure Cost Management

Ez a cikk foglalkozik az Azure Cost Management (más néven Cloudyn) kapcsolatos gyakori kérdésekre. Cost Management kapcsolatos kérdése van, ha megkérheti csatlakoztatásukról [– gyakori kérdések az Azure Cost Management](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-azure-cost-management-by-cloudyn?forum=Cloudyn).

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Hogyan oldhatom fel a közvetett nagyvállalati telepítő kapcsolatos gyakori problémák?

A Cloudyn portál első használatakor, ha Ön egy vállalati szerződésben, vagy a Cloud Solution Provider (CSP) felhasználó láthatja az alábbi üzenetek:

- "A megadott API-kulcs nem legfelső szintű regisztrációs kulcs" megjelennek a **állítsa be az Azure Cost Management** varázsló.
- "A közvetlen regisztráció – nem" a nagyvállalati szerződés portálon jelenik meg.
- "Nem használati adatok az elmúlt 30 napban nem található. A Cloudyn portálon lépjen kapcsolatba a terjesztőt, hogy ellenőrizze, hogy jelölőnyelvi engedélyezve lett, az Azure-fiók"jelenik meg.

Az előző üzenetek arra utalnak, hogy egy viszonteladón vagy felhőszolgáltatón keresztül vásárolt Azure Nagyvállalati szerződést. Engedélyeznie kell a viszonteladóhoz vagy a CSP _jelölőnyelvi_ számára, hogy a Cloudyn megtekintheti az adatokat az Azure-fiók.

A problémák megoldása:

1. A viszonteladónak engedélyeznie kell a _korrektúrát_ a fiókjában. Útmutatás: [Közvetett ügyfeleknek szóló előkészítési útmutató](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. Az Azure nagyvállalati szerződéssel kulcs a Cloudyn segítségével hozza létre. Útmutatásért lásd: [hozzáadása az Azure nagyvállalati szerződéssel rendelkező](https://support.cloudyn.com/hc/en-us/articles/210429585-Adding-Your-AZURE-EA) vagy [keresse meg az EA regisztrációs azonosító és API-kulcs](https://youtu.be/u_phLs_udig).

A Cost Management szolgáltatást csak egy Azure-szolgáltatásadminisztrátor engedélyezheti. A társadminisztrátori jogosultság ehhez nem elegendő.

Mielőtt beállításához a Cloudyn Azure nagyvállalati szerződés API-kulcsot is létrehozhat, engedélyeznie kell az Azure Billing API webhelyen található utasításokat követve:

- [Jelentéskészítő API-k Enterprise-ügyfeleknek – áttekintés](../billing/billing-enterprise-api.md)
- [Microsoft Azure Enterprise Portal jelentéskészítő API](https://ea.azure.com/helpdocs/reportingAPI) **Az adatok az API-hoz való hozzáférésének engedélyezése** területen


Előfordulhat, hogy a részlegek rendszergazdáinak, a fióktulajdonosoknak és a nagyvállalati rendszergazdáknak is engedélyt kell adnia a _díjak megtekintéséhez_ a számlázási API segítségével.

## <a name="why-dont-i-see-optimizer-recommendations"></a>Miért nem látom, hogy optimalizáló javaslatok?

Az ajánlás információk csak aktív fiókok érhetők el. Nem jelenik meg minden javaslat információ **optimalizáló** kategóriák fiókok, amelyek jelentést *időközét adja meg*, többek között:

- Optimalizálás Manager
- Méretezés optimalizálása
- Hatékonysági

Ha bármely optimalizáló javaslat adatai nem tekinthetők meg, akkor nagy valószínűséggel rendelkezik fiókok, amelyek időközét adja meg. Egy fiók aktiválásához szüksége regisztrálja az Azure hitelesítő adatait.

Egy fiók aktiválása:

1.  A Cloudyn portálon kattintson a **Settings** (Beállítások) gombra a jobb felső sarokban, és válassza a **Cloud Accounts** (Felhőbeli fiókok) lehetőséget.
2.  A Microsoft Azure-fiókok lapon keresse meg a fiókokat, amelyeket egy **aktivált** előfizetés.
3.  Kattintson a jobb oldalán egy aktivált fiókhoz, a **szerkesztése** szimbólum, amely hasonlít egy ceruza.
4.  A bérlő azonosítója és sebessége azonosítója automatikusan észleli. Kattintson a **Tovább** gombra.
5.  A program átirányítja az Azure Portalon. Jelentkezzen be a portálra, és engedélyezze a Cloudyn gyűjtő el az Azure-beli adatait.
6.  Ezután a program átirányítja a Cloudyn-fiókok kezelése lapon, és az előfizetésben **aktív** fiók állapota. Egy zöld pipa szimbólumot jeleníti meg.
7.  Ha egy zöld pipa szimbólumot egy vagy több előfizetés nem jelenik meg, az azt jelenti, hogy egy levélolvasó alkalmazás (CloudynCollector) az előfizetés létrehozásához szükséges engedélyek nem rendelkeznek. Egy magasabb szintű jogosultságokkal rendelkező felhasználónak kell ismételje meg a 3. és 4.  

Miután elvégezte az előző lépéseket, megtekintheti a optimalizáló javaslatok egy-két napon belül. Azonban mielőtt érhető el teljes optimalizálási adatok legfeljebb öt nappal is igénybe vehet.


## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Hogyan engedélyezhetem a felfüggesztett vagy zárolt felhasználók?

Először tekintsük át azokat a leggyakoribb forgatókönyvet, amelynek hatására a felhasználói fiókok beolvasásához *initiallySuspended*.

> Rendszergazda1 a Microsoft Cloud Solution Provider és a nagyvállalati szerződés felhasználó lehet. A szervezet készen áll a Cost Management használatához.  Ő regisztrálja az Azure Portalon keresztül, és bejelentkezik a Cloudyn portálon. A személy, aki regisztrál a Cost Management szolgáltatást, és bejelentkezhet a Cloudyn portált, mint az ő válik a *elsődleges rendszergazdai*. Rendszergazda1 nem hoz létre az összes felhasználói fiókot. Azonban a Cloudyn portált használja, akkor létrehozása az Azure-fiókok és állít be egy entitáshierarchiára. Rendszergazda1 Admin2, egy Bérlői rendszergazda, hogy Regisztráljon a Cost Management, és jelentkezzen be a Cloudyn portálon kell figyelmeztet.

> Admin2 regisztrálja az Azure Portalon keresztül. Azonban ha ő próbál bejelentkezni a Cloudyn portálon, kapjuk fiókját próbálnak hiba **felfüggesztve**. Az elsődleges rendszergazdai rendszergazda1, értesítést kap, a fiók felfüggesztését. Rendszergazda1 kell aktiválni Admin2 a fiókot, és biztosítson számára *rendszergazdai entitás hozzáférés* a megfelelő entitások, és lehetővé teszi a felhasználói hozzáférés-kezelési és aktív felhasználói fiók.


Riasztás, hozzáférés engedélyezése egy felhasználó kérést kap, ha a felhasználói fiók aktiválásához szüksége.

A felhasználói fiók aktiválása:

1. Jelentkezzen be a Cloudyn, amellyel a Cloudyn beállítása az Azure rendszergazdai felhasználói fiók használatával. Vagy jelentkezzen be egy felhasználói fiókot, amely rendszergazdai hozzáférést.
2. Válassza a fogaskerék ikonra a jobb felső sarokban, és válassza ki **felhasználókezelés**.
3. Keresse meg a felhasználót, és válassza ki a ceruzaikonra szerkesztheti a felhasználó.
4. Alatt **felhasználói állapot**, módosítsa az állapot **felfüggesztett** való **aktív**.

A Cloudyn felhasználói fiókokat létesíthetnek egyszeri bejelentkezés az Azure-ból. Ha egy felhasználó mistypes a jelszavát, akkor előfordulhat, hogy zárja ki magát a Cloudyn, annak ellenére, hogy továbbra is hozzáférhet az Azure.

Ha módosítja az alapértelmezett cím, az Azure-ban az e-mail-címét a Cloudyn, a fiók kizárva is. Ez előfordulhat, hogy megjelenítése "állapot initiallySuspended." Ha a felhasználói fiók zárolva van, forduljon a alaphelyzetbe állítja a fiók egy másik rendszergazda.

Azt javasoljuk, hogy legalább két Cloudyn rendszergazdai fiókokat hozhat létre abban az esetben az egyik fiók lekérdezi zárolva.

Ha nem jelentkezhet be a Cloudyn portálra, győződjön meg arról, hogy a Cloudyn bejelentkezni a megfelelő Azure Cost Management URL-CÍMÉT használja. Használat [ https://azure.cloudyn.com ](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade).

Kerülje a Cloudyn közvetlen URL-cím https://app.cloudyn.com.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Hogyan lehet aktiválni az Azure-beli hitelesítő időközét adja meg fiókokat?

Amint az Azure-fiókok a Cloudyn által felfedezett költségadatok azonnal nyújtott költség-alapú jelentésekhez. Azonban a Cloudyn használati és teljesítményadatokat biztosít, akkor regisztrálnia kell a fiókok Azure hitelesítő adatait. Útmutatásért lásd: [hozzáadása Azure Resource Manager](https://support.cloudyn.com/hc/en-us/articles/212784085-Adding-Azure-Resource-Manager).

Az Azure a hitelesítő adatait, a Cloudyn portálon hozzáadásához válassza a Szerkesztés szimbólum jobb oldalán a fiók neve, nem az előfizetés.

Az Azure-beli hitelesítő hozzá vannak adva a Cloudyn, amíg a fiók megjelenik _nem aktivált_.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Hogyan adhatok hozzá több fiókhoz és entitások egy meglévő előfizetéshez?

További entitásokat segítségével további nagyvállalati szerződések hozzáadása a Cloudyn-előfizetéshez. Az alábbi hivatkozások további entitások hozzáadása ismertetik:

- [Entitás hozzáadása](https://support.cloudyn.com/hc/en-us/articles/212016145-Adding-an-Entity) cikk
- [A hierarchiában a költségelemek meghatározása](https://support.cloudyn.com/hc/en-us/articles/115005142529-Video-Defining-your-hierarchy-with-Cost-Entities) videó

A CSP-k:

Egy entitás további CSP-fiókok hozzáadásához válassza **MSP hozzáférés** helyett **vállalati** az új entitás létrehozásakor. Ha a fiók regisztrálva van, a nagyvállalati szerződés, és szeretne hozzáadni a CSP hitelesítő adatokat, a Cloudyn támogatási csoporthoz előfordulhat, hogy módosítania kell a fiók beállításait. Ha Ön egy fizetős Azure-előfizető, létrehozhat egy új támogatási kérelmet az Azure Portalon. Válassza ki **súgó + támogatás**, majd válassza ki **új támogatási kérelem**.

## <a name="currency-symbols-in-cloudyn-reports"></a>A Cloudyn jelentésekben pénznemszimbólum

Előfordulhat, hogy különböző pénznemek használatával több Azure-fiókokon. A Cloudyn cost jelentések azonban ne jelenjen meg egynél több pénznem típusa jelentésenként.

Ha különböző pénznemek használatával több előfizetéssel rendelkezik, egy szülőentitás és annak gyermek entitás pénznemek USD-ben jelennek **$**. A javasolt ajánlott eljárás, hogy kerülje a különböző pénznemek entitás ugyanabban a hierarchiában. Más szóval egy entitás struktúrájából összes előfizetés az adott pénznemben kell használnia.

A Cloudyn automatikusan észleli a nagyvállalati szerződés előfizetés pénznem és mutat be megfelelően a jelentésekben.  Azonban a Cloudyn csak jeleníti USD **$** CSP és az Azure web-direct-fiókok esetében.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Mik a Cloudyn adatok frissítése az ütemtervek?

A Cloudyn rendelkezik a következő adatok frissítési ütemtervet:

- **Kezdeti**: Miután beállította, költségadatok megtekintéséhez a Cloudyn akár 24 órát is igénybe vehet. A Cloudyn elegendő adatgyűjtéshez méretezési javaslatok megjelenítése akár 10 nap is is igénybe vehet.
- **Napi**: a tizedik naponta vagy minden hónap végén, a Cloudyn jelenít meg az adatokat az előző nap után UTC + 3 kapcsolatban a következő nap naprakész.
- **Havi**: a tizedik nap minden hónap első napjától kezdve a Cloudyn megjelenítése előfordulhat, hogy az adatok csak az előző hónap végéig.

A Cloudyn feldolgozza az adatokat az előző napi, amikor az előző nap adatainak teljes elérhetőnek kell lennie. Az előző nap adatainak a Cloudyn által kapcsolatos UTC + 3 általában érhető el minden nap. Egyes adatok, például címkék, feldolgozni további 24 órát is igénybe vehet.

Az aktuális hónaphoz adatok nem érhető el minden hónap elején a gyűjteményhez. Az időszakban a szolgáltatók azok az előző havi használati díjának felszámolása véglegesítése. Az előző hónap adatai jelennek meg a Cloudyn 5-10 nappal minden hónap kezdete után. Ebben az időszakban az előző hónap csak amortizált költségek merülhetnek fel. Ön nem láthatja a napi számlázási és használati adatokat. Amikor elérhetővé válik az adatok a Cloudyn, a feldolgozásuk visszamenőlegesen. A feldolgozás után a havi adatok között az ötödik és a tizedik minden hónap napja jelenik meg.

Ha az Azure-ból történő adatküldés a Cloudyn késés, adatok továbbra is keletkezik az Azure-ban. Az adatok átkerülnek a Cloudyn, ha a kapcsolat helyreáll.

## <a name="cost-fluctuations-in-cloudyn-cost-reports"></a>Cloudyn-Költségjelentésekben ingadozása költség

Költségjelentésben költség ingadozások által megkövetelt jeleníti meg, amikor a felhőszolgáltatók számlázási frissített fájlok küldése. Változó költségű fordulhat elő, ha új fájlokat a felhőszolgáltató a szokásos módon kívül a napi és havi ütemezés reporting érkezik. A Cost módosítások Cloudyn újraszámítás nem eredménye. 

A hónap során a felhőszolgáltató által küldött minden elszámolási fájlok a napi költségek becslése. Néha adatok gyakran frissülő – alkalmanként többször naponta. Frissítések állnak az AWS üzemezésnél gyakoribb Azure. Költség tekintetében stabil maradjon, ha az előző havi számlázási kiszámítása befejeződött, és a végső számlázási fájl érkezik. Általában a hónap 10.

Változások történnek, amikor költség korrekciók kapott a felhőszolgáltató. Egy példa a kreditek fogadására. Módosítások hónapok fordulhat elő a releváns hónap volt bezárása után. Módosítások jelennek meg, ha az újraszámítást végezni a felhőszolgáltató által. A Cloudyn frissíti az előzményadatok, győződjön meg arról, hogy újraszámít minden módosítását. Azt is ellenőrzi, hogy költségeket pontosan azt jelenti, hogy.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Hogyan egy közvetlen CSP konfigurálja a Cloudyn hozzáférést közvetett CSP-ügyfelek és partnerek számára?

Útmutatásért lásd: [közvetett CSP-hozzáférés konfigurálása a Cloudyn](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).

## <a name="what-causes-the-optimizer-menu-item-to-appear"></a>Az optimalizáló menüpont jelennek meg, mi okozza?

Miután hozzáadta az Azure Resource Manager-hozzáférés és a gyűjtött, kell megjelennie a **optimalizáló** lehetőséget. Tekintse meg az Azure Resource Manager-hozzáférésének aktiválásához [hogyan lehet aktiválni az Azure-beli hitelesítő időközét adja meg fiókokat?](#how-do-i-activate-unactivated-accounts-with-azure-credentials)

## <a name="is-cost-managementcloudyn-agent-based"></a>Cost Management/Cloudyn ügynök alapján?

Nem. Ügynökök nem használ. A Microsoft insights-beli API-beli virtuális gépek az Azure virtuális gép metrikaadatok összegyűjtött. Ha azt szeretné, az Azure virtuális gépek metrikai adatok gyűjtését, szükségük a diagnosztikai beállítások engedélyezve van.

## <a name="do-cloudyn-reports-show-more-than-one-ad-tenant-per-report"></a>Hajtsa végre a Cloudyn jelentésekben meg a jelentés egynél több AD-bérlő?

Igen. Is [hozzon létre egy megfelelő felhő entitással](tutorial-user-access.md#create-and-manage-entities) minden AD-bérlő, amely rendelkezik. Majd megtekintheti az összes Azure AD-bérlő adatait és más platform felhőszolgáltatók, például az Amazon Web Services és a Google Cloud Platform.
