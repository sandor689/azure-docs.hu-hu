---
title: Ajánlott eljárások az Azure SQL Data Sync |} A Microsoft Docs
description: Konfigurálása és futtatása az Azure SQL Data Sync bevált gyakorlatok megismeréséhez.
services: sql-database
ms.date: 07/03/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
manager: craigg
ms.openlocfilehash: 2b23f9f2edbec468ecbd1395bd138e1be801c6e5
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620800"
---
# <a name="best-practices-for-sql-data-sync"></a>Az SQL Data Synchez ajánlott eljárások 

Ez a cikk az Azure SQL Data Synchez ajánlott eljárásait ismerteti.

Az SQL Data Sync áttekintéséhez tekintse meg a [több felhőalapú és helyszíni adatbázis közötti, az Azure SQL Data Sync segítségével végzett adatszinkronizálást](sql-database-sync-data.md) ismertető cikket.

## <a name="security-and-reliability"></a> Biztonság és megbízhatóság

### <a name="client-agent"></a>Ügyfélügynök

-   Az ügyfél-ügynök telepítése a legalacsonyabb jogosultsági szintű felhasználói fiókkal, amely hálózati szolgáltatás hozzáfér.  
-   Az ügyfélügynök telepíthető olyan számítógépre, amely nem a helyszíni SQL Server-számítógépen.  
-   Egy helyszíni adatbázishoz nem regisztrál az egynél több ügynök.    
    -   Elkerülése érdekében, még akkor is, ha a különböző táblák különböző szinkronizálási csoportok szinkronizálását végzi.  
    -   A szinkronizálási csoportok törlésekor egy helyszíni adatbázisból regisztrálás több ügyfél ügynökök kockázatot kihívásokat.

### <a name="database-accounts-with-least-required-privileges"></a>Legkevesebb szükséges jogosultsággal adatbázisfiókhoz

-   **A szinkronizálási telepítő**. Tábla létrehozása vagy módosítása; Az ALTER Database; Hozzon létre eljárást; Válassza ki / Alter sémában. Hozzon létre felhasználó által meghatározott típus.

-   **Folyamatban lévő szinkronizálási**. Válassza ki / Insert / frissítése / törlése szinkronizálása, és szinkronizálási metaadat- és nyomon követi, táblázatokban; a kiválasztott táblák Hajtsa végre a szolgáltatás által létrehozott tárolt eljárásokat engedély Hajtsa végre felhasználó által definiált táblatípusokban engedéllyel.

-   **A megszüntetés**. Az ALTER szinkronizálási; táblák egy részének Válassza ki / szinkronizálási metaadat-táblát; a törlése Vezérlő a táblák és tárolt eljárások, felhasználó által meghatározott típusok szinkronizálása.

Az Azure SQL Database támogatja csak egyetlen hitelesítő adatokat. Ezen feladatokat belül ezt a korlátozást, fontolja meg a következő beállításokat:

-   Hitelesítő adatainak módosítása egyes fázisokat futtatni (például *credentials1* telepítő és *credentials2* a folyamatban lévő).  
-   Az engedély a hitelesítő adatok módosítása (azt jelenti, az engedély módosítása után a szinkronizálás be van állítva).

## <a name="setup"></a>Beállítás

### <a name="database-considerations-and-constraints"></a> Adatbázis megfontolandó szempontok és korlátozások

#### <a name="sql-database-instance-size"></a>Az SQL Database-példány mérete

Amikor létrehoz egy új SQL Database-példányban, állítsa be a maximális méretet, hogy mindig legyen nagyobb, mint az adatbázis központi telepítése. A nagyobb, mint az üzembe helyezett adatbázis maximális mérete nem állít be, ha a szinkronizálás sikertelen lesz. Bár az SQL Data Sync nem biztosít automatikus növekedés, futtathatja a `ALTER DATABASE` parancsot a létrehozásuk után az adatbázis méretének növeléséhez. Győződjön meg arról, hogy az SQL Database-példány mérete keretein belül marad.

> [!IMPORTANT]
> Az SQL Data Sync az egyes adatbázisok további metaadatokat tárol. Fontos Pro tato metadata tárolásához szükséges lemezterületet számításakor. Mennyisége hozzáadott terhelést kapcsolódó táblák szélességét (például keskeny táblákat szükség további terhelést) és a forgalom mennyisége.

### <a name="table-considerations-and-constraints"></a> Tábla megfontolandó szempontok és korlátozások

#### <a name="selecting-tables"></a>Táblák kiválasztása

Nem kell egy adatbázisban az egy szinkronizálási csoportban lévő összes táblát tartalmazza. A táblák, amelyeket a szinkronizálási csoport befolyásolja a hatékonyságot és a költségek. Táblák és a táblák függ, a szinkronizálási csoport csak akkor, ha a megköveteli, hogy azok tartalmazzák.

#### <a name="primary-keys"></a>Elsődleges kulcsok

Az egy szinkronizálási csoportban minden tábla elsődleges kulccsal kell rendelkeznie. Az SQL Data Sync szolgáltatás nem tudják szinkronizálni egy táblát, amely nem rendelkezik elsődleges kulccsal.

Mielőtt éles környezetben használja az SQL Data Sync, tesztelje a kezdeti és folyamatos szinkronizálás teljesítménye.

### <a name="provisioning-destination-databases"></a> Cél adatbázisok kiépítése

Az SQL Data Sync alapszintű adatbázis autoprovisioning biztosít.

Ez a szakasz bemutatja az SQL Data Sync kiépítés vonatkozó korlátozások.

#### <a name="autoprovisioning-limitations"></a>Autoprovisioning korlátozások

Az SQL Data Sync autoprovisioning meg a következő korlátozások vonatkoznak:

-   Csak a céltábla létrehozott oszlopok kiválasztása.  
    Minden oszlopot, amely nem része a szinkronizálási csoport nem kiépített a céltáblákba.
-   Indexek csak a kijelölt oszlopok jönnek létre.  
    Ha a forrás táblázatindexhez oszlopokat, amelyek a szinkronizálási csoport nem rendelkezik, nem kiépített ezeket az indexeket a céltáblákba.  
-   XML-típusú oszlopok indexei nem kiépítve.  
-   ELLENŐRZÉSI korlátozásokban nincs kiépítve.  
-   A forrástáblákból a meglévő eseményindítók nincsenek kiépítve.  
-   Nézetek és tárolt eljárások hozhatók létre az adatbázist.
-   AZ UPDATE CASCADE és ON DELETE CASCADE műveleteket a külső kulcsra vonatkozó megkötések nem újra létre kell hozni a céltáblákba.

#### <a name="recommendations"></a>Javaslatok

-   Az SQL Data Sync autoprovisioning funkció csak akkor használja, próbálja ki a szolgáltatást.  
-   Az éles környezetben üzembe az adatbázissémát.

### <a name="locate-hub"></a> Hol található a központi adatbázis

#### <a name="enterprise-to-cloud-scenario"></a>A felhőbe irányuló vállalati forgatókönyv

Késés minimalizálása érdekében hagyja a központi adatbázis megközelíti a lehető legnagyobb koncentráció a szinkronizálási csoport adatbázis-forgalomtól.

#### <a name="cloud-to-cloud-scenario"></a>A felhőbe irányuló felhőalapú forgatókönyv

-   Ha egy szinkronizálási csoportban található összes adatbázis egy adatközpontban, ugyanabban az adatközpontban a hub kell elhelyezkedniük. Ez a konfiguráció csökkenti a késést és az adatközpontok közötti adatátvitel költségeit.
-   Ha az adatbázisokat a szinkronizálási csoport több adatközpontban, ugyanabban az adatközpontban, az adatbázisok és az adatbázis-forgalom többsége a hub kell elhelyezni.

#### <a name="mixed-scenarios"></a>Vegyes forgatókönyvek

A fenti irányelvek érvényesek összetett szinkronizálási csoport konfigurációját, például azokkal, amelyek a vállalati felhő és a felhő-a-felhőbe forgatókönyvek vegyesen.

## <a name="sync"></a>Sync

### <a name="avoid-a-slow-and-costly-initial-synchronization"></a> Kerülje a lassú és költséges kezdeti szinkronizálás

Ebben a szakaszban bemutatjuk a kezdeti szinkronizálási csoport szinkronizálása. Útmutató: a vártnál tovább tart, és költségesebb, mint a szükséges egy kezdeti szinkronizálást megelőzése érdekében.

#### <a name="how-initial-sync-works"></a>Hogyan kezdeti szinkronizálás működése

Szinkronizálási csoport létrehozásakor kezdődik csak egy adatbázis adatait. Adatok több adatbázist is van, ha az SQL Data Sync tekinti minden sor egy ütközést, fel kell oldani. Az ütközések feloldásának hatására a kezdeti szinkronizálás go lassan. Ha az adatok több adatbázist is van, kezdeti szinkronizálás több napot és az adatbázis méretétől függően néhány hónapban között is eltarthat.

Ha az adatbázis különböző adatközpontokban, sorokban a különböző adatközpontok között kell keresnie. Ez növeli a költségét egy kezdeti szinkronizálást.

#### <a name="recommendation"></a>Ajánlás

Ha lehetséges indítsa el a szinkronizálási csoport adatbázisok csak az egyik az adatokkal.

### <a name="design-to-avoid-synchronization-loops"></a> Tervezési szinkronizálási hurkok elkerülése érdekében

Egy szinkronizálási ciklust fordul elő, ha egy szinkronizálási csoportban. a körkörös hivatkozás. Ebben az esetben egy adott adatbázis minden módosítása nélkül keringenek és körkörösen replikálja a rendszer az adatbázisokat a szinkronizálási csoport keresztül.   

Győződjön meg arról, hogy elkerülheti a szinkronizálási ciklusok, mivel okozhat teljesítménycsökkenést, és jelentősen növeli a költségeket.

### <a name="handling-changes-that-fail-to-propagate"></a> Módosítások propagálása sikertelen

#### <a name="reasons-that-changes-fail-to-propagate"></a>Oka, hogy a módosítások propagálása sikertelen

Módosítások propagálása a következő okok miatt meghiúsulhat:

-   Séma vagy az adattípus inkompatibilitás.
-   Beszúráskor nem nullázható oszlopban null értékű.
-   Külső kulcsra vonatkozó megkötések megsértése.

#### <a name="what-happens-when-changes-fail-to-propagate"></a>Mi történik, ha módosítások propagálása?

-   Szinkronizálja a csoport azt mutatja, hogy van egy **figyelmeztetés** állapota.
-   A portál felhasználói felületének naplófájl-megjelenítő részletei láthatók.
-   Ha a probléma nem oldódik 45 nap, az adatbázis elavult válik.

> [!NOTE]
> Ezek a változások soha nem propagálása. Az ebben a forgatókönyvben helyreállítás egyetlen módja, hogy a szinkronizálási csoport hozza létre újból.

#### <a name="recommendation"></a>Ajánlás

Szinkronizálási csoport és az adatbázis állapotát a portál és a naplófájlok felületén keresztül rendszeresen figyelni.


## <a name="maintenance"></a>Karbantartás

### <a name="avoid-out-of-date-databases-and-sync-groups"></a> Elavult adatbázisok elkerülése és a csoportok szinkronizálása

Szinkronizálási csoport vagy egy adatbázist a szinkronizálási csoport elavult válhat. A szinkronizálási csoport állapota mikor **elavult**, akkor leáll a működése. Ha egy adatbázis állapota van **elavult**, adatvesztés történhet. A legcélszerűbb elkerülése érdekében ebben a forgatókönyvben a helyreállítás helyett.

#### <a name="avoid-out-of-date-databases"></a>Elavult adatbázisok elkerülése

Egy adatbázis állapota **elavult** mikor volt a kapcsolat nélküli vagy több 45 napon belül. Elkerülése érdekében egy **elavult** állapota, adatbázis, győződjön meg arról, hogy az adatbázisok se offline 45 nap vagy több.

#### <a name="avoid-out-of-date-sync-groups"></a>Kerülje az elavult szinkronizálási csoportok

A szinkronizálási csoport állapota **elavult** Ha bármilyen változás a szinkronizálási csoport propagálása a szinkronizálási csoport többi 45 nap vagy több sikertelen. Elkerülése érdekében egy **elavult** állapotának a szinkronizálási csoport, rendszeresen tekintse meg a szinkronizálási csoport előzményeinek naplót. Győződjön meg arról, hogy az összes ütközések feloldása, és hogy módosítások sikeresen propagálja a szinkronizálási csoport adatbázisok.

Szinkronizálási csoport sikertelen lehet, a módosítás alkalmazása az alábbi okok valamelyike:

-   Séma inkompatibilitás táblák között.
-   Adatok inkompatibilitás táblák között.
-   Null értékű sor beszúrása egy oszlopot, amely nem engedélyezi null értékek használatát.
-   A sor frissítése, amely megsérti egy külső kulcsmegkötés értékkel.

Az elavult szinkronizálási csoportok megakadályozása:

-   A sémafrissítést az értékeket, amelyeket nem sikerült sorokat tartalmazza.
-   Frissítse a Külsőkulcs-értéket a sikertelen sorok szereplő értékek szerepeljenek.
-   Frissítse a sikertelen sorában az adatértékek, hogy a séma vagy a céladatbázis külső kulcsok kompatibilisek.

### <a name="avoid-deprovisioning-issues"></a> Kerülje a problémák megszüntetése

Bizonyos esetekben a regisztrációjának törlése az adatbázis egy ügyfélügynök-okozhat a szinkronizálás sikertelen lesz.

#### <a name="scenario"></a>Forgatókönyv

1. A szinkronizálási csoport egy SQL Database-példány és a egy helyszíni SQL Server-adatbázis, helyi ügynök 1 társított használatával lett létrehozva.
2. Az azonos helyszíni adatbázis regisztrálva van a helyi ügynök 2 (Ez az ügynök nem áll semmilyen szinkronizálási csoport társítva).
3. Regisztrációjának törlése a helyszíni adatbázis helyi ügynök 2 eltávolítja a nyomon követési és meta táblák szinkronizálása a helyszíni adatbázis csoportba tartozó.
4. Szinkronizálási csoportban műveletek sikertelenek lesznek, hiba: "az aktuális művelet nem sikerült, mert az adatbázis nem kiépített szinkronizálási szolgáltatás, vagy nem rendelkezik engedélyekkel a szinkronizálási konfiguráció táblázatokhoz."

#### <a name="solution"></a>Megoldás

Ebben a forgatókönyvben elkerülése érdekében nem regisztrál egy adatbázis több ügynökkel.

Ebben a forgatókönyvben helyreállítás:

1. Az összes szinkronizálási csoportból, amely tartozik, távolítsa el az adatbázist.  
2. Adja hozzá az adatbázis összes szinkronizálási csoportból eltávolított a programba.  
3. Telepítse minden érintett szinkronizálási csoport (Ez a művelet kiépíti az adatbázis).  

### <a name="modifying-your-sync-group"></a> Szinkronizálási csoport módosítása

Ne kísérelje meg törölni egy adatbázist a szinkronizálási csoport, és szerkessze a szinkronizálási csoport első üzembe helyezése egy, a módosítások nélkül.

Ehelyett először távolítsa el egy adatbázist egy szinkronizálási csoportot. Ezután telepítse a módosítást, és várjon, amíg befejeződik a megszüntetés. Ha megszüntetés befejeződött, a szinkronizálási csoport szerkesztheti, és üzembe helyezése a módosításokat.

Ha egy adatbázis eltávolítása, és szerkessze a szinkronizálási csoport első üzembe helyezése egy, a módosítások nélkül kísérli meg, egy- vagy a másik művelet sikertelen lesz. A portál felületén inkonzisztenssé válhatnak. Ha ez történik, frissítse az oldalt a megfelelő állapot visszaállításához.

## <a name="next-steps"></a>További lépések
Az SQL Data Sync szolgáltatással kapcsolatos további információkért lásd:

-   [Több felhőalapú és helyszíni adatbázis közötti adatszinkronizálás az Azure SQL Data Synckel](sql-database-sync-data.md)
-   [Az Azure SQL Data Sync beállítása](sql-database-get-started-sql-data-sync.md)
-   [Az Azure SQL Data Sync monitorozása a Log Analytics használatával](sql-database-sync-monitor-oms.md)
-   [Az Azure SQL Data Synckel hibaelhárítása](sql-database-troubleshoot-data-sync.md)  
-   Teljes PowerShell-példák az SQL Data Sync konfigurálásáról:  
    -   [A PowerShell használata több Azure SQL Database-adatbázis közötti szinkronizáláshoz](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [A PowerShell használata egy Azure-beli SQL Database-adatbázis és egy helyszíni SQL Server-adatbázis közötti szinkronizáláshoz](scripts/sql-database-sync-data-between-azure-onprem.md)  

Az SQL Database kapcsolatos további információkért lásd:

-   [SQL Database – áttekintés](sql-database-technical-overview.md)
-   [Az adatbázis életciklusának felügyelete](https://msdn.microsoft.com/library/jj907294.aspx)
