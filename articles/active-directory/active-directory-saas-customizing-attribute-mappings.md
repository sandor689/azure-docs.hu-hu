---
title: Azure AD-attribútum-leképezések testreszabása |} A Microsoft Docs
description: Ismerje meg, milyen attribútumleképezések SaaS-alkalmazásokhoz az Azure Active Directoryban, hogyan módosíthatja azokat az üzleti igények kielégítésében.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: barbkess
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 366b89d3db0db634e239ac2d99188c7ea0444c13
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365192"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Az SaaS-alkalmazásokhoz az Azure Active Directory-attribútumleképezések testreszabása Felhasználókiépítés
A Microsoft Azure AD külső SaaS-alkalmazások például a Salesforce, a Google Apps és a másokkal való támogatást nyújt. Ha felhasználókiépítése egy külső SaaS-alkalmazás engedélyezve van, az Azure Portalon szabályozza az attribútumértékek attribútum-leképezések formájában.

Nincs attribútumokat és az Azure AD felhasználói és minden egyes SaaS-alkalmazás felhasználói objektumok között attribútumleképezések előre konfigurált készletével. Néhány alkalmazás más típusú objektumok felhasználókon, például a csoportok kezelése. <br> 
 Az alapértelmezett attribútumleképezések igényeinek megfelelően testre szabhatja. Ez azt jelenti, hogy módosítsa vagy törölje a meglévő attribútumleképezések, vagy hozzon létre új attribútum-leképezések.
 
## <a name="editing-user-attribute-mappings"></a>Felhasználói attribútum-leképezések szerkesztése

Az Azure AD-portálon érhető el ez a funkció kattint egy **leképezések** konfigurációja **kiépítési** a a **kezelés** szakaszában egy  **A vállalati alkalmazás**.


![Salesforce][5] 

Kattintson egy **leképezések** konfigurációs, megnyílik a kapcsolódó **attribútum-hozzárendelési** képernyő. Számos attribútum-leképezések egy SaaS-alkalmazáshoz megfelelő működéséhez szükséges. A szükséges attribútumok a **törlése** funkció nem érhető el.


![Salesforce][6]  

A fenti példában láthatja, hogy a **felhasználónév** attribútum a Salesforce-ban felügyelt objektum megjelenik a **userPrincipalName** a társított Azure Active Directory objektum értékét.

Testre szabhatja a meglévő **attribútum-leképezések** leképezés kattintva. Ekkor megnyílik a **attribútum szerkesztése** képernyő.

![Salesforce][7]  


### <a name="understanding-attribute-mapping-types"></a>Attribútum-hozzárendelési típusainak ismertetése
Az attribútum-leképezések meghatározhatja, hogyan fel van töltve attribútumai külső SaaS-alkalmazásban. Támogatott négy különböző leképezés típusa van:

* **Közvetlen** – a target attribútum a csatolt objektum egy attribútum értéke megjelenik az Azure ad-ben.
* **Állandó** – a target attribútum feltöltése egy meghatározott karakterlánccal megadott.
* **Kifejezés** -target attribútum egy parancsfájl-szerű kifejezés eredménye alapján van feltöltve. 
  További információkért lásd: [írása a kifejezéseket az Azure Active Directoryban attribútumleképezések](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Nincs** -marad a célattribútum változatlan. Azonban ha target attribútum minden eddiginél üres, akkor megjelenik az Ön által megadott alapértelmezett értéket.

Egyéni attribútum-leképezések mellett az alábbi négy alapvető típusok, támogatja a fogalmat egy nem kötelező **alapértelmezett** érték-hozzárendelés. Az alapértelmezett érték-hozzárendelés biztosítja, hogy egy target attribútum feltöltése értékkel, ha hiba egy érték sem az Azure ad-ben és nem a célobjektum. A leggyakrabban használt beállítások mellett akkor hagyja üresen a mezőt.


### <a name="understanding-attribute-mapping-properties"></a>Attribútumleképzés tulajdonságok ismertetése

Az előző szakaszban Ön már vezettek be attribútum-leképezés típusa tulajdonsághoz.
Ez a tulajdonság mellett attribútum-leképezések is támogatják a következő attribútumokkal:

- **Adatforrás-attribútum** – a felhasználói attribútum a forrásrendszerből (Példa: Azure Active Directory).
- **Célattribútum** – a felhasználói attribútum a célrendszeren (Példa: ServiceNow).
- **Ezzel az attribútummal objektumok** – Ez a leképezés használandó egyedileg azonosíthatja azokat a felhasználókat, a forrás és cél közötti-e. Ez általában be van állítva a userPrincipalName vagy mail attribútum az Azure ad-ben, amely általában egy felhasználónév mezője célalkalmazás van hozzárendelve.
- **Megfeleltetési prioritás** – több egyező attribútumok beállítása. Ha több, akkor ez a mező által meghatározott sorrendben értékeli. Amint egyezést talál, további megfelelő attribútumok értékeli ki.
- **Leképezés alkalmazása**
    - **Mindig** – ezt a hozzárendelést alkalmazni, mind a felhasználó létrehozása és a frissítési műveletek
    - **Csak a létrehozásakor** -leképezés alkalmazása csak a felhasználó-létrehozási műveletek


## <a name="editing-group-attribute-mappings"></a>Csoport attribútum-leképezések szerkesztése

Alkalmazások, például a ServiceNow, a Box és a Google Apps, a kiválasztott számú támogatja a felhasználói objektumok mellett csoportházirend-objektumok létrehozására. Csoportobjektumokhoz is tartalmaz a csoport tulajdonságai, például a megjelenített nevek és e-mail-aliasok csoporttagok mellett.

![ServiceNow][8]  

Csoportos kiépítését lehet szükség esetén engedélyezhető vagy letiltható az eszközcsoport-leképezés szerinti kiválasztásával **leképezések**, és a beállítás **engedélyezve** a kívánt opcióhoz a **attribútumleképezés** képernyő.

Az attribútumok csoportobjektumokhoz részeként üzembe helyezett testre szabható az azonos felhasználói objektumok, korábban leírt módon. 

>[!TIP]
>Egy különálló fogalom a csoportobjektumokhoz (a tulajdonságok és a tagok) üzembe helyezése [csoportok hozzárendelése](manage-apps/assign-user-or-group-access-portal.md) alkalmazáshoz. Egy csoport hozzárendelése egy alkalmazáshoz, de csak kiépítése a felhasználói objektumok, a csoportban lévő lehetőség. Csoportok használata a hozzárendelések nem szükséges a teljes csoportobjektumokhoz kiépítését.


## <a name="editing-the-list-of-supported-attributes"></a>Támogatott attribútumok listáját szerkesztése

A támogatott az adott alkalmazásra vonatkozó felhasználói attribútumokat előre konfigurálva. A legtöbb alkalmazás felhasználói felügyeleti API-k nem támogatják a séma felderítési, ezért az Azure AD létesítési szolgáltatás nem képes dinamikusan generálható meghívni az alkalmazás által támogatott attribútumok listáját. 

Azonban az egyes alkalmazások támogatják az egyéni attribútumokat. Ahhoz, hogy az Azure AD létesítési szolgáltatás tudni olvasása és írása az egyéni attribútumokat, a definíciójukat meg kell adni az Azure Portalhoz a **speciális beállítások megjelenítése** alján, a jelölőnégyzet jelölését a  **Attribútum-hozzárendelési** képernyő.

Alkalmazások és rendszerek, amelyek támogatják az attribútumlista testre szabhatja a következők:

* Salesforce
* ServiceNow
* Munkanap
* Az Azure Active Directory ([Azure AD Graph API alapértelmezett attribútumok](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) és egyéni címtárbővítmények támogatottak)
* Támogató alkalmazások [SCIM 2.0](https://tools.ietf.org/html/rfc7643), ahol az attribútumok meghatározott a [core séma](https://tools.ietf.org/html/rfc7643) hozzá kell adni

>[!NOTE]
>Támogatott attribútumok listáját szerkesztési csak rendszergazdák számára, akik testreszabták a saját alkalmazások és rendszerek sémája és első kézből tudomásuk egyéni attribútumaik definiálva hogyan ajánlott. Egyes esetekben ehhez az API-k és fejlesztők egy alkalmazás vagy rendszer által biztosított eszközök ismeretét. 

![Szerkesztő][9]  

Támogatott attribútumok listáját szerkesztésekor a következő tulajdonságok áll rendelkezésre:

* **Név** – az attribútum a célobjektum sémában meghatározott rendszer nevét. 
* **Típus** – az attribútum tárolja, a célobjektum sémában meghatározott adatok típusát. Ez a következők egyike lehet:
   * *Bináris* -attribútum bináris adatokat tartalmaz.
   * *Logikai* -attribútum igaz vagy hamis értéket tartalmaz.
   * *Dátum és idő* -attribútum tartalmazza a dátum karakterláncot.
   * *Egész szám* -attribútumot tartalmaz egy egész számot.
   * *Hivatkozás* -attribútum által hivatkozott egy értéket egy másik táblájában a célalkalmazás Azonosítót tartalmaz.
   * *Karakterlánc* -szöveges karakterláncot attribútumot tartalmaz. 
* **Elsődleges kulcs?** -Az attribútum e a célobjektum sémában található elsődleges kulcs mezőként van definiálva.
* **Kötelező?** -Az attribútum e a célalkalmazás vagy a rendszer kitöltve szükség.
* **Többértékű?** – Az attribútum támogatja-e több érték.
* **Megkülönböztetése?** -Az attribútumok értékek e a kis-és nagybetűket módon értékeli ki.
* **API-kifejezés** – csak akkor használja, ha ehhez a megadott kiépítési összekötők (például a Workday) dokumentáció utasításai.
* **Hivatkozott objektumattribútum** – Ha ezt a hivatkozási attribútum, akkor ebben a menüben válassza ki a táblát és az attribútum a célalkalmazás társítva az attribútum értékét tartalmazó teszi. Például ha egy attribútumot, amelynek tárolt érték egy külön táblázatban "Részleg" objektumra hivatkozik, az "részleg" nevű, kiválasztott "Departments.Name". Vegye figyelembe, hogy a referencia-táblák és a egy adott alkalmazás támogatott elsődleges azonosító mezők előre konfigurált, és jelenleg nem szerkeszthető, az Azure portal használatával, de használatával szerkesztheti a [Graph API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Egy új attribútum hozzáadásához, görgessen a támogatott attribútumok listáját a végén, adja meg a mezőkben a megadott bemeneti felett, és válassza **attribútum hozzáadása**. Válassza ki **mentése** Ha hozzáadta az attribútumokat. Ezután kell töltse be újra a **kiépítési** válnak elérhetővé az attribútum-hozzárendelési szerkesztőt, az új attribútumok fülre.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Visszaállítás az alapértelmezett attribútumok és attribútum-leképezések

Meg kell újrakezdés, és a meglévő hozzárendelések biztonsági másolatot az alapértelmezett állapotba alaphelyzetbe állítása, kiválaszthatja a **visszaállítja az alapértelmezett hozzárendeléseket** jelölje be a jelölőnégyzetet, és a konfiguráció mentéséhez. Ezzel beállítja az összes leképezések, ha az alkalmazás korábban éppen most adták hozzá, az Azure AD-bérlő az alkalmazáskatalógusból. 

Ez a beállítás hatékonyan kényszeríti az összes olyan felhasználó újbóli szinkronizálás a kiépítési szolgáltatás futása közben. 

>[!IMPORTANT]
>Erősen ajánlott, amelyek **előkészítési állapotát** állítható **ki** Ez a beállítás meghívása előtt.


## <a name="what-you-should-know"></a>Tudnivalók

* A Microsoft Azure AD hatékony megvalósítását a szinkronizálási folyamat nyújt. Inicializált környezetben csak a frissítést igénylő objektumok feldolgozása során egy szinkronizálási ciklust. 

* Attribútum-leképezések frissítése egy szinkronizálási ciklust teljesítményére hatással van. Az attribútum-hozzárendelési konfigurációra egy frissítéshez kell újraértékeli az összes felügyelt objektumok. 

* Fontos, hogy az egymást követő módosítások számát az attribútumleképezések minimális ajánlott.


## <a name="next-steps"></a>További lépések

* [Felhasználói kiépítés és megszüntetés SaaS-alkalmazások automatizálása](active-directory-saas-app-provisioning.md)
* [Kifejezések írása attribútum-leképezések](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [A felhasználók átadásának Hatókörszűrő](active-directory-saas-scoping-filters.md)
* [SCIM használata a felhasználók és csoportok automatikus üzembe helyezésének engedélyezéséhez az Azure Active Directoryból az alkalmazásokba](manage-apps/use-scim-to-provision-users-and-groups.md)
* [Az SaaS-alkalmazások integrálásával foglalkozó oktatóanyagok listája](saas-apps/tutorial-list.md)

<!--Image references-->
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png
[8]: ./media/active-directory-saas-customizing-attribute-mappings/24.png
[9]: ./media/active-directory-saas-customizing-attribute-mappings/25.PNG

