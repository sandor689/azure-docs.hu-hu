---
title: Azure-beli felügyelt alkalmazások a Marketplace piactéren | Microsoft Docs
description: A cikk a Marketplace piactéren elérhető Azure-beli felügyelt alkalmazásokat ismerteti.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 07/10/2018
ms.author: tomfitz
ms.openlocfilehash: 8f35bda8c6925bdc10097ac6d180f5998bd5cf1d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989786"
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Azure-beli felügyelt alkalmazások a Marketplace piactéren

A szállítók Azure-beli felügyelt alkalmazások segítségével kínálhatják megoldásaikat az Azure Marketplace ügyfeleinek mindegyike számára. Ezek a szállítók lehetnek felügyelt szolgáltatások szolgáltatói (MSP-k), független szoftverszállítók (ISV-k) és rendszerintegrátorok (SI-k). A felügyelt alkalmazások csökkentik a karbantartás és a szervizelés miatt az ügyfelekre háruló terheket. A szállítók infrastruktúrát és szoftvert értékesíthetnek a piactéren. Szolgáltatásokat és működtetéshez kapcsolódó támogatást kínálhatnak a felügyelt alkalmazásokkal. További információért tekintse meg a [felügyelt alkalmazások áttekintésével](overview.md) foglalkozó cikket.

Ez a cikk azt ismerteti, hogy hogyan tehet közzé egy alkalmazást a piactéren, és teheti széles körben elérhetővé az ügyfelek számára.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Felügyelt alkalmazás közzétételének előfeltételei

Az oktatóanyag elvégzéséhez szüksége lesz a felügyelt alkalmazás definíciójához tartozó .zip fájlra. További információért tekintse meg a [szolgáltatáskatalógusban elérhető alkalmazás létrehozásával](publish-service-catalog-app.md) foglalkozó cikket.

Emellett több üzleti előfeltételt kell teljesíteni. Ezek a következők:

* A vállalatnak vagy leányvállalatnak olyan országban kell lennie, ahol a piactér támogatja az értékesítést.
* A terméket olyan módon kell licencelni, amely kompatibilis a piactér által támogatott számlázási modellekkel.
* Műszaki támogatást kell biztosítania az ügyfelek számára üzleti szempontból észszerű módon. A támogatás lehet ingyenes, fizetős vagy közösség által nyújtott támogatás.
* Licencelnie kell a szoftvert, valamint a külső szoftverfüggőségeket.
* Olyan tartalmat kell biztosítania, amely teljesíti az ajánlatnak a Marketplace piactéren és az Azure Portalon való meghirdetésének feltételeit.
* El kell fogadnia az Azure Marketplace részvételi szabályzatának és a közzétevői megállapodásnak a feltételeit.
* El kell fogadnia a használati feltételeket, a Microsoft adatvédelmi nyilatkozatát és a Microsoft Azure Certified Program Agreement feltételeit.

## <a name="become-a-publisher"></a>Közzétevővé válás

Ahhoz, hogy közzétevő lehessen az Azure Marketplace piactéren, az alábbiakat kell tennie:

1. Microsoft-azonosító létrehozása – Hozzon létre Microsoft-fiókot egy olyan e-mail-cím használatával, amely a vállalati tartományhoz tartozik, nem pedig egy magánszemélyhez. Ezt az e-mail-címet a Microsoft fejlesztői központ és a Felhőpartnerportál fogja használni. További információért tekintse meg az [Azure Marketplace közzétevői útmutatóját](https://aka.ms/sellerguide).
1. Az [Azure Marketplace Nomination Form](https://aka.ms/ampnomination) (Azure Marketplace jelentkezési űrlapja) elküldése – A **Solution that you intend to publish?** (Közzétenni kívánt megoldás?) kérdésnél válassza a **Managed Application** (Felügyelt alkalmazás) lehetőséget. Az űrlap elküldése után a Marketplace bevezetési csapata áttekinti a jelentkezést, és ellenőrzi a kérelmet. A jóváhagyási folyamat egy–három napot vehet igénybe. Ha a jelentkezést jóváhagyják, egy promóciós kódot kap, amellyel mentesül a fejlesztői központ regisztrációs díjának kifizetése alól. Ha **nem** tölti ki a Marketplace jelentkezési űrlapját, 99 dollár regisztrációs díjat kell fizetnie.
1. Regisztrálás a [fejlesztői központban](http://dev.windows.com/registration?accountprogram=azure) – A Microsoft ellenőrzi, hogy a vállalat érvényes jogi személy-e, és hogy rendelkezik-e érvényes adóazonosítóval a bejegyzés országában. A jóváhagyási folyamat 5–10 napot vehet igénybe. A regisztrációs díj elkerülése érdekében használja a jelentkezési folyamat során érkező e-mailben kapott kódot. További információért tekintse meg az [Azure Marketplace közzétevői útmutatóját](https://aka.ms/sellerguide).
1. Bejelentkezés a [Cloud Partner Portalra](https://cloudpartner.azure.com) – A közzétevői profilban társítsa a fejlesztői központban regisztrált fiókot a Marketplace-beli közzétevői profilhoz. További információért tekintse meg az [Azure Marketplace közzétevői útmutatóját](https://aka.ms/sellerguide).

## <a name="create-a-new-azure-application-offer"></a>Új Azure-alkalmazásajánlat létrehozása

A partnerportálfiók létrehozását követően készen áll a felügyelt alkalmazást kínáló ajánlat létrehozására.

### <a name="set-up-an-offer"></a>Ajánlat beállítása

A felügyelt alkalmazásra vonatkozó ajánlat a közzétevőtől származó termékosztály-ajánlatnak felel meg. Ha új típusú alkalmazást szeretne a piactéren elérhetővé tenni, beállíthatja azt új ajánlatként. Az ajánlatok SKU-k gyűjteményei. Minden ajánlat saját entitásaként jelenik meg a piactéren.

1. Jelentkezzen be a [Felhőpartnerportálra](https://cloudpartner.azure.com/).

1. A bal oldali navigációs panelen válassza a **+ New offer** > **Azure Applications** (+ Új ajánlat > Azure alkalmazások) lehetőséget.

1. Az **Editor** (Szerkesztő) nézetben láthatja a szükséges űrlapokat. Az egyes űrlapokat a cikk alábbi részei ismertetik.

## <a name="offer-settings-form"></a>Offer Settings (Ajánlatbeállítások) űrlap

Az **Offer Settings** (Ajánlatbeállítások) űrlap mezői a következők:

* **Offer ID** (Ajánlat azonosítója): Ez az egyedi azonosító azonosítja az ajánlatot egy közzétevői profilban. Az azonosító a termék URL-címeiben, a Resource Manager-sablonokban és a számlázási jelentésekben látható. Csak kisbetűs alfanumerikus karakterekből és kötőjelekből (-) állhat. Az azonosító nem végződhet kötőjellel. Legfeljebb 50 karakterből állhat. Miután egy ajánlat elérhetővé válik, ezt a mezőt zárolja a rendszer.
* **Publisher ID** (Közzétevő azonosítója): A legördülő listát használva válassza ki azt a közzétevői profilt, amelyben közzé szeretné tenni ezt az ajánlatot. Miután egy ajánlat elérhetővé válik, ezt a mezőt zárolja a rendszer.
* **Name** (Név): Az ajánlat megjelenítendő neve a Marketplace piactéren és a portálon jelenik meg. Legfeljebb 50 karakterből állhat. A termék számára egy felismerhető márkanevet adjon meg. A vállalat nevét ne adja meg itt, kivéve, ha így forgalmazza a terméket. Ha az ajánlatot a saját webhelyén is forgalmazza, gondoskodjon arról, hogy a név pontosan úgy jelenik meg, mint a webhelyén.

Ha elkészült, válassza a **Save** (Mentés) elemet a megadott adatok mentéséhez.

## <a name="skus-form"></a>SKUs (SKU-k) űrlapja

A következő lépés az SKU-k hozzáadása az ajánlathoz.

Az SKU egy ajánlat legkisebb megvásárolható egysége. Az SKU-kat ugyanazon a termékosztályon (ajánlaton) belül használhatja az alábbiak megkülönböztetéséhez:

* a különféle támogatott funkciók,
* az ajánlat felügyelt-e vagy sem,
* a támogatott számlázási modellek.

Az SKU-k a fő ajánlat alatt jelennek meg a piactéren. Saját megvásárolható entitásként jelennek meg az Azure Portalon.

1. Válassza az **SKUs** > **New SKU** (SKU-k > Új SKU) elemet.

1. Adjon meg egy értéket az **SKU ID** (SKU-azonosító) mezőben. Az SKU-azonosító egy adott SKU egyedi azonosítója az ajánlaton belül. Az azonosító a termék URL-címeiben, a Resource Manager-sablonokban és a számlázási jelentésekben látható. Csak kisbetűs alfanumerikus karakterekből és kötőjelekből (-) állhat. Az azonosító nem végződhet kötőjellel, és legfeljebb 50 karakterből állhat. Miután egy ajánlat elérhetővé válik, ezt a mezőt zárolja a rendszer. Több SKU-val is rendelkezhet egy ajánlaton belül. Minden közzétenni kívánt rendszerkép esetében külön SKU-t kell megadni.

1. Töltse ki a következő űrlapon található **SKU Details** (SKU részletei) szakaszt:

   Töltse ki az alábbi mezőket:

   * **Title** (Cím): Adja meg az SKU címét. Ez a cím jelenik meg a katalógusban ennél az elemnél.
   * **Summary** (Összefoglalás): Adja meg az SKU rövid összefoglalását. Ez a szöveg a cím alatt jelenik meg.
   * **Description** (Leírás): Adja meg az SKU részletes leírását.
   * **SKU Type** (SKU típusa): A megengedett értékek: *Managed Application* (Felügyelt alkalmazás) és *Solution Templates* (Megoldássablonok). Ebben az esetben a *Managed Application* (Felügyelt alkalmazás) lehetőséget válassza.
   * **Country/Region availability** (Országonkénti/régiónkénti elérhetőség): Válassza ki azokat az országokat, ahol a felügyelt alkalmazás elérhető.
   * **Pricing** (Díjszabás): Adja meg az alkalmazás felügyeletének árát. Az ár beállítása előtt válassza ki az elérhető országokat.

1. Adjon hozzá egy új csomagot. Töltse ki a következő űrlapon található **Package Details** (Csomag részletei) szakaszt:

   Töltse ki az alábbi mezőket:

   * **Verzió**: Adja a feltöltött csomag verzióját. A következő formátumban kell lennie: `{number}.{number}.{number}{number}`.
   * **Csomagfájl (.zip)**: Ez a csomag két szükséges fájlt tartalmaz, amelyek egy .zip csomagba vannak tömörítve. Az egyik fájl a Resource Manager-sablon, amely a felügyelt alkalmazáshoz üzembe helyezendő erőforrásokat határozza meg. A másik fájl a [felhasználói felületet](create-uidefinition-overview.md) határozza meg a felügyelt alkalmazást a portálon keresztül üzembe helyező felhasználók számára. A felhasználói felületen elemeket ad meg, amelyek lehetővé teszik a felhasználók számára paraméterértékek megadását.
   * **PrincipalId** (Résztvevő-azonosító): Ez a tulajdonság egy olyan felhasználó, felhasználócsoport vagy alkalmazás Azure Active Directory- (Azure AD-) azonosítója, amely az ügyfél előfizetésén belüli erőforrásokhoz kap hozzáférést. A Role Definition (Szerepkör-definíció) az engedélyeket ismerteti.
   * **Role Definition** (Szerepkör-definíció): Ez a tulajdonság a beépített szerepkör-alapú hozzáférés-vezérlési (RBAC) szerepkörök listája, amelyet az Azure AD biztosít. Kiválaszthatja az erőforrásoknak az ügyfél nevében történő felügyeletéhez leginkább megfelelőbb szerepkört.
   * **Szabályzatbeállítások**: Alkalmazzon egy [Azure-szabályzatot](../azure-policy/azure-policy-introduction.md) a felügyelt alkalmazásokra az üzembe helyezett megoldások megfelelőségi követelményeinek megadásához. Válassza ki az alkalmazandó szabályzatokat az elérhető lehetőségek közül. **Szabályzatparaméterek** esetén adjon meg egy JSON-karakterláncot a paraméter értékeivel. A szabályzatdefiníciókról és a paraméterértékek formátumáról tekintse meg a következő dokumentumot: [Azure Policy-minták](../azure-policy/json-samples.md).

Több engedélyt is hozzáadhat. Javasoljuk, hogy hozzon létre egy AD-felhasználócsoportot, és adja meg annak azonosítóját a **PrincipalId** (Résztvevő-azonosító) tulajdonságban. Így több felhasználót is hozzáadhat a felhasználócsoporthoz anélkül, hogy frissítenie kellene az SKU-t.

Az RBAC-vel kapcsolatban az [RBAC Azure Portalon való használatának megkezdését ismertető](../role-based-access-control/overview.md) témakörben tekinthet meg további információt.

## <a name="marketplace-form"></a>Marketplace (Piactér) űrlap

A Marketplace (Piactér) űrlap az [Azure Marketplace](https://azuremarketplace.microsoft.com) és az [Azure Portal](https://portal.azure.com/) felületén megjelenő mezők megadását kéri.

### <a name="preview-subscription-ids"></a>Preview subscription IDs (Előzetes verzióhoz hozzáférő azonosítók)

Adja meg azon Azure-előfizetések azonosítóinak listáját, amelyek hozzáférhetnek az ajánlathoz, miután közzétette azt. Az engedélyezési listán szereplő előfizetéseket az ajánlat előzetes verziójának teszteléséhez használhatja, mielőtt elérhetővé tenné az ajánlatot mindenki számára. A partnerportálon egy legfeljebb 100 előfizetést tartalmazó engedélyezési listát állíthat össze.

### <a name="suggested-categories"></a>Suggested categories (Javasolt kategóriák)

Legfeljebb öt olyan kategóriát választhat ki a listából, amelyekhez az ajánlata a lehető legjobban kapcsolódik. Ezekkel a kategóriákkal sorolható be ajánlata az [Azure Marketplace](https://azuremarketplace.microsoft.com) piactéren és az [Azure Portalon](https://portal.azure.com/) elérhető termékkategóriákba.

#### <a name="azure-marketplace"></a>Azure Piactér

A felügyelt alkalmazás összefoglalása az alábbi mezőket jeleníti meg:

![A piactéren megjelenő összefoglalás](./media/publish-marketplace-app/publishvm10.png)

A felügyelt alkalmazás **Overview** (Áttekintés) lapján az alábbi mezők jelennek meg:

![Piactér áttekintése](./media/publish-marketplace-app/publishvm11.png)

A felügyelt alkalmazás **Plans + Pricing** (Csomagok és díjszabás) lapján az alábbi mezők jelennek meg:

![A piactéren elérhető csomagok](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Azure Portal

A felügyelt alkalmazás összefoglalása az alábbi mezőket jeleníti meg:

![A portálon megjelenő összefoglalás](./media/publish-marketplace-app/publishvm12.png)

A felügyelt alkalmazás áttekintése az alábbi mezőket jeleníti meg:

![A portálon megjelenő áttekintése](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Emblémával kapcsolatos irányelvek

Az alábbi irányelveket kövesse a Felhőpartnerportálra feltölteni kívánt emblémákra vonatkozóan:

*   Az Azure arculata egyszerű színpalettát használ. Törekedjen minél kevesebb alap- és másodlagos szín használatára az emblémában.
*   A portál témaszínei a fehér és a fekete. Ne használja ezeket a színeket az embléma háttérszíneként. Olyan színt használjon, amelynek hatására az embléma felkelti a figyelmet. Javasoljuk az egyszerű alapszínek használatát. *Ha átlátszó hátteret használ, az embléma és a szöveg ne legyen fehér, fekete vagy kék.*
*   Ne használjon színátmenetes hátteret az emblémában.
*   Ne helyezzen el szöveget az emblémára, még a vállalat vagy a márka nevét se. Az emblémának kétdimenziósnak kell érződnie, és a színátmenetek is kerülendők.
*   Ügyeljen arra, hogy az embléma ne legyen széthúzva.

#### <a name="hero-logo"></a>Főképembléma

A főképembléma nem kötelező. A közzétevő dönthet úgy, hogy nem tölt fel főképemblémát. A főképikon a feltöltését követően nem törölhető. Ekkor a partnernek követnie kell a főképikonokra vonatkozó Marketplace-irányelveket.

A főképembléma ikonjára vonatkozóan az alábbi irányelveket kövesse:

*   A közzétevő megjelenítendő neve, a csomag címe és az ajánlat részletes összefoglalása fehéren jelenik meg. Ezért ne használjon világos színt a főképikon hátteréhez. A fekete, fehér vagy átlátszó háttér nem engedélyezett a főképikonok esetén.
*   Az ajánlat meghirdetését követően az elemeket a rendszer szoftvere ágyazza be a főképemblémába. A beágyazott elemek közé tartozik a közzétevő megjelenítendő neve, a csomag címe, az ajánlat részletes összefoglalása és a **Create** (Létrehozás) gomb. Ezért ne adjon meg szöveget a főképembléma tervezése közben. Hagyja üresen a jobb oldali területet, mert a szöveget a rendszer szoftvere helyezi oda. A szöveg számára a jobb oldalon fenntartott üres helynek 415 x 100 képpont méretűnek kell lennie. A bal oldalról számítva 370 képpontnyi az eltolása.

    ![Példa a főképemblémára](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Support (Támogatás) űrlap

A **Support** (Támogatás) űrlapon a vállalat támogatási csapatának kapcsolattartási adatait adja meg. Ezek az adatok a fejlesztési csapat és az ügyfélszolgálat elérhetőségei lehetnek.

## <a name="publish-an-offer"></a>Ajánlat közzététele

Miután az összes szakaszt kitöltötte, válassza a **Publish** (Közzététel) lehetőséget azon folyamat elindításához, amely elérhetővé teszi az ajánlatot az ügyfelek számára.

## <a name="next-steps"></a>További lépések

* A felügyelt alkalmazások bemutatásáért tekintse meg a [felügyelt alkalmazások áttekintését](overview.md).
* A szolgáltatáskatalógusban elérhető felügyelt alkalmazások közzétételével kapcsolatban tekintse meg a [szolgáltatáskatalógusban elérhető felügyelt alkalmazások létrehozását és közzétételét](publish-service-catalog-app.md) ismertető témakört.