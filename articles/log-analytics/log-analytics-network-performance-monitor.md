---
title: Network Performance Monitor megoldás az Azure-ban |} A Microsoft Docs
description: Az Azure-ban a Network Performance Monitor segítségével közel valós időben észlelheti, és keresse meg a hálózati teljesítmény szűk keresztmetszeteit, a hálózatok teljesítményének figyelése.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.component: na
ms.openlocfilehash: 3640f5bb2c2e9457e269bd189cbec3b627ee7349
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626872"
---
# <a name="network-performance-monitor-solution-in-azure"></a>Network Performance Monitor megoldás az Azure-ban

![Hálózati Teljesítményfigyelő szimbólum](./media/log-analytics-network-performance-monitor/npm-symbol.png)


A Network Performance Monitor egy hibrid felhőalapú Hálózatfigyelő megoldás, amely segítséget nyújt a hálózati infrastruktúrában különböző pont közötti hálózati teljesítmény nyomon követése. Emellett segítséget nyújt a szolgáltatás és alkalmazás végpontok és a figyelő hálózati kapcsolatait figyelheti az Azure ExpressRoute teljesítményét. 

A Network Performance Monitor észleli a hálózati problémák, például az adatforgalom blackholing, útválasztási hibák és problémák, amelyek a hagyományos hálózati figyelési módszerek nem képes észlelni. A megoldás riasztásokat állít elő, és értesíti, amikor egy hálózati kapcsolat a tranzakciótól egy küszöbértéket. Biztosítja a hálózati teljesítménnyel kapcsolatos problémák időben történő észlelése és a egy adott hálózati szegmens vagy az eszköz a probléma okának localizes. 

A Network Performance Monitor három széles körű lehetőségeket biztosítja: 

* [A Teljesítményfigyelő](log-analytics-network-performance-monitor-performance-monitor.md): hálózati kapcsolat figyelése-alapú telepítések és a helyszíni helyeken, több adatközpontokban, és a fiókirodák és a többrétegű alkalmazások üzleti szempontból kritikus vagy mikroszolgáltatás-alapú. A Teljesítményfigyelő képes észlelni a hálózati problémákat, mielőtt felhasználói panaszok.

* [Szolgáltatás Kapcsolatfigyelő](log-analytics-network-performance-monitor-service-endpoint.md): figyelheti a felhasználóktól a kapcsolat a szolgáltatások érdeklik, határozza meg, milyen infrastruktúra képes legyen az elérési út és azonosítása, ahol a hálózati szűk keresztmetszeteket történik. Tudnivalók mint a felhasználók előtt, és tekintse meg a problémák pontos helyét a hálózati elérési út mentén. 

    Ez a funkció segítségével hajtsa végre a tesztek alapján a HTTP, HTTPS, TCP és az ICMP figyelése a közel valós idejű és korábbi adatát a rendelkezésre állás és a szolgáltatás válaszideje. A hozzájárulását a hálózatot a csomagvesztés és a késés is figyelheti. A hálózati topológia térképek elkülönítheti a hálózati lassulást. Azonosíthatja a probléma területek, amely a hálózati elérési úton történik a csomópont a szolgáltatásba, az egyes ugrások késési adatokkal. Beépített tesztek segítségével figyelheti a hálózati kapcsolat az Office 365 és Dynamics CRM és minden előzetes beállítás nélkül. Ez a képesség figyelemmel kísérheti a hálózati kapcsolat bármely TCP-kompatibilis végpont, például a websites, a SaaS-alkalmazások, a PaaS-alkalmazások és az SQL-adatbázisok.

* [ExpressRoute-figyelő](log-analytics-network-performance-monitor-expressroute.md): végpontok közötti kapcsolatait és teljesítményét a fiókirodák és az Azure közötti monitorozása az Azure expressroute-on keresztül.  

További információ a különböző funkciók által támogatott [Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) online is elérhető.
 
## <a name="supported-regions"></a>Támogatott régiók
Az NPM figyelheti a hálózatok és a egy munkaterületről, amely a következő régiók egyikében üzemel, a világ bármely részén alkalmazások közötti kapcsolat:
* Nyugat-Európa
* USA nyugati középső régiója
* USA keleti régiója
* Kelet-Japán
* Délkelet-Ázsia
* Dél-kelet-Ausztrália
* Dél-Egyesült Királyság
* Egyesült Államok kormányzati Virginia

ExpressRoute-figyelő érhető el a támogatott régiók listáját a [dokumentáció](https://docs.microsoft.com/azure/expressroute/how-to-npm?utm_swu=8117#regions).


## <a name="set-up-and-configure"></a>Telepítése és konfigurálása

### <a name="install-and-configure-agents"></a>Telepítse és konfigurálja az ügynökök 

Az alapszintű folyamatok használatával, ügynökök telepítése [Azure Log Analyticshez való csatlakozáshoz Windows számítógépek](log-analytics-windows-agents.md) és [Operations Manager csatlakoztatása a Log Analytics](log-analytics-om-agents.md).

### <a name="where-to-install-the-agents"></a>Az ügynökök telepítési helyét 

* **A Teljesítményfigyelő**: legalább egy csomópont telepítése az Operations Management Suite-ügynök minden egyes, amelyről a hálózati kapcsolat más alhálózatokra és figyelni kívánt alhálózati csatlakozik.

    Egy hálózati kapcsolat figyeléséhez, telepítse az ügynökök mindkét végponton, hogy a hivatkozás. Ha bizonytalan a hálózat topológiáját, telepítse az ügynököket a kritikus fontosságú számítási feladatokhoz, amelyekhez hozzá szeretné a hálózati teljesítmény nyomon követése a kiszolgálókon. Például ha azt szeretné, a webkiszolgáló és a egy futó SQL server közötti hálózati kapcsolat figyeléséhez, az ügynöktelepítéshez mindkét kiszolgálón. Ügynökök figyelik a hálózati kapcsolat (hivatkozások) gazdagép, nem magukat a gazdagépek között. 

* **Szolgáltatás Kapcsolatfigyelő**: az Operations Management Suite-ügynök telepítése minden egyes csomóponton, ahonnan a szolgáltatásvégpont a hálózati kapcsolat figyeléséhez. Ilyen például, hogy szeretné-e hálózati kapcsolatait figyelheti az Office 365-höz a címkével ellátott O1 O2 és O3 office helyekről. Telepítse az Operations Management Suite ügynök legalább egy csomópontja egyes O1 O2 és O3. 

* **ExpressRoute-figyelő**: legalább egy Operations Management Suite-ügynök telepítése az Azure virtuális hálózaton. A helyszíni alhálózati, amely ExpressRoute privát társviszony-létesítésen keresztül csatlakozik a legalább egy ügynököt is telepítheti.  

### <a name="configure-operations-management-suite-agents-for-monitoring"></a>Figyelés az Operations Management Suite-ügynökök konfigurálása 

A Network Performance Monitor szintetikus tranzakciók használatával a forrás és cél ügynökök közötti hálózati teljesítmény nyomon követése. Választhat a TCP és az ICMP protokoll Teljesítményfigyelő és a szolgáltatás Kapcsolatfigyelő képességeket figyelését. Csak a TCP figyelési protokollként az ExpressRoute-figyelő érhető el. Győződjön meg arról, hogy a tűzfal engedélyezi-e az Operations Management Suite-ügynökök az protokollt választja a figyeléshez használt közötti kommunikációt. 

* **A TCP protokoll**: Ha a TCP protokoll figyeléséhez, nyissa meg a tűzfalport az ügynökökhöz a Network Performance Monitor és az ExpressRoute-figyelő segítségével győződjön meg arról, hogy az ügynökök kapcsolódik egymáshoz. Nyissa meg a portot, futtassa a [EnableRules.ps1](https://aka.ms/npmpowershellscript) PowerShell-szkriptet egy PowerShell-ablakot rendszergazdai jogosultságokkal a paraméterek nélkül.

    A parancsfájl létrehozza a megoldáshoz szükséges beállításkulcsokat. Windows tűzfal-szabályokat, hogy a TCP-kapcsolatok létrehozása az egyes ügynökök is létrehoz. A beállításkulcsok, a szkript által létrehozott adja meg, hogy a naplófájl a hibakeresési naplók és a naplók fájl elérési útját. A parancsfájl a ügynök-kommunikációhoz használt TCP-portot is meghatározza. Ezek a kulcsok értékeit a parancsfájl által automatikusan beállítva. Ezek a kulcsok manuális ne módosítsa. A port, alapértelmezés szerint megnyílik az 8084. Azáltal, hogy a paraméter portszám a parancsfájlt egy egyéni portot is használhatja. Használja a portot, az összes olyan számítógépen, ahol a szkriptet futtatják. 

    >[!NOTE]
    > A szkript kizárólag Windows tűzfal helyben konfigurálja. Ha a hálózati tűzfal, ellenőrizze, hogy a Network Performance Monitor által használt TCP portra irányuló adatforgalmat lehetővé teszi.

    >[!NOTE]
    > Nem kell futtatnia a [EnableRules.ps1](https://aka.ms/npmpowershellscript ) a szolgáltatásvégpont-figyelő PowerShell parancsfájlt.

    

* **Az ICMP protokollt**: Ha az ICMP protokoll figyeléséhez, a következő tűzfalszabályokat, hogy megbízhatóan az ICMP engedélyezése:
    
   ```
   netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
   ```
 

### <a name="configure-the-solution"></a>A megoldás konfigurálása 

1. A Network Performance Monitor megoldás hozzáadása a munkaterületet a [Azure Marketplace-en](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview). A leírt folyamatot is használható [adja hozzá a Log Analytics solutions kövesse a megoldástárban](log-analytics-add-solutions.md). 
2. Nyissa meg a Log Analytics-munkaterületet, és válassza ki a **áttekintése** csempére. 
3. Válassza ki a **Network Performance Monitor** üzenettel csempe *megoldás további konfigurálást igényel*.

   ![Hálózati Teljesítményfigyelő csempe](media/log-analytics-network-performance-monitor/npm-config.png)

4. Az a **telepítő** lapon láthatja az Operations Management Suite-ügynökök telepítése és konfigurálása az ügynökök a figyeléshez a **közös beállítások** megtekintése. Amint azt korábban, ha telepítve van, és konfigurálni Operations Management Suite-ügynökök, válassza ki a **telepítő** használni kívánt megtekintése a funkció konfigurálása. 

   **A Teljesítményfigyelő**: válassza ki a szintetikus tranzakciókhoz használandó protokollt a **alapértelmezett** Teljesítményfigyelő szabályt, és válassza ki **mentés és folytatás**. A protokoll kiválasztása csak tárolja a rendszer által létrehozott alapértelmezett szabály. Válassza ki a protokollt, minden alkalommal, amikor olyan szabályt hoz létre a Teljesítményfigyelő explicit módon kell. Mindig áthelyezheti a **alapértelmezett** -szabályának beállításait a **Teljesítményfigyelő** (megjelenik a nap-0 konfiguráció befejezése után) lapra, és később a protokoll módosítása. Ha nem szeretné a rPerfomance figyelő képesség, letilthatja az alapértelmezett szabály az **alapértelmezett** -szabályának beállítások a **Teljesítményfigyelő** fülre.

   ![Teljesítmény-figyelő nézet](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
   **Szolgáltatás Kapcsolatfigyelő**: A képesség nyújt beépített, előre konfigurált teszteket az ügynököket az Office 365 és Dynamics 365, a hálózati kapcsolat figyeléséhez. Válassza ki a mellettük a jelölőnégyzetek bejelölésével figyelni kívánt Office 365 és Dynamics 365-szolgáltatások. Válassza ki a figyelni kívánt ügynököket, jelölje be **ügynökök hozzáadása**. Ha nem szeretné, ez a funkció vagy kívánt beállítás később használatával, nem bármit válassza ki, és válassza a **mentés és folytatás**.

   ![Szolgáltatásvégpont-figyelő nézet](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

   **ExpressRoute-figyelő**: válassza ki **felderítése most** minden az ExpressRoute privát társviszony-létesítéseket, amely csatlakozik a virtuális hálózatok az Azure-előfizetést a Log Analytics-munkaterülettel társított felderítéséhez. 

   >[!NOTE] 
   > A megoldás jelenleg csak az ExpressRoute privát társviszony-létesítéseket deríti fel. 

   >[!NOTE] 
   > Csak privát társviszony-létesítéseket a virtuális hálózatok, a Log Analytics-munkaterülettel társított az előfizetéssel társított csatlakozó felderített. Ha ExpressRoute kívül az előfizetés ehhez a munkaterülethez társított virtuális hálózatok csatlakoztatva van, hozzon létre egy Log Analytics-munkaterület ezen előfizetések. Ezeket a társviszony figyelése a Network Performance Monitor.

   ![ExpressRoute-figyelő nézet](media/log-analytics-network-performance-monitor/npm-express-route.png)

   A felderítés befejezése után egy táblázatban láthatók a felderített privát társviszony-létesítéseket. 

   ![Hálózati Teljesítményfigyelő beállítása lap](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
A társviszony-létesítéshez figyelési kezdetben letiltott állapotban van egy. Válassza ki, minden társviszonyhoz, amely a figyelni kívánt és a számukra az részletei nézeten a jobb figyelés konfigurálása. Válassza ki **mentése** a konfiguráció mentéséhez. További tudnivalókért tekintse meg a "Konfigurálása ExpressRoute-figyelés" cikket. 

A telepítés befejezése után az adatok feltöltése egy órát vesz igénybe 30 perc. Az üzenet jelenik meg a megoldás összesíti az adatokat a hálózatról, amíg *megoldás további konfigurálást igényel* a Network Performance Monitor **áttekintése** csempére. Miután az adatok gyűjtése és indexelt, a **áttekintése** csempén megváltozik, és tájékoztatja, hogy a hálózat állapotának összegzését az. Majd módosíthatja a csomópontok melyik Operations Management Suite, az ügynökök telepítve vannak, valamint az alhálózatok a környezetből felderített figyelését.

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Alhálózatok és a csomópontok figyelési beállításainak módosítása 

Legalább egy telepített ügynökkel rendelkező összes alhálózat szerepel a **célhálózat** lapot a konfiguráció lapon. 


A monitorozásának engedélyezése vagy letiltása az adott alhálózatok:

1. Válassza ki, vagy a jelet a jelölőnégyzetből a **alhálózat-azonosító**. Ezután győződjön meg arról, hogy **figyelés használatát** beállítás vagy törölt, szükség szerint. Válassza ki, vagy törölje a több alhálózattal. Ha le van tiltva, alhálózatok nem figyeli, és az ügynökök frissítve lett, hogy állítsa le a pingelés más ügynökök. 
2. Válassza ki a csomópontokat, hogy egy adott alhálózati a figyelni kívánt. Az alhálózat kiválasztása a listából, és helyezze át a szükséges csomópontokkal a nem monitorozott és felügyelt csomópontokat tartalmazó listák között. Az alhálózat egy egyéni leírást adhat hozzá.
3. Válassza ki **mentése** a konfiguráció mentéséhez. 

#### <a name="choose-nodes-to-monitor"></a>Válassza ki a csomópontok figyelése

A telepített ügynökkel rendelkező csomópontok közt a **csomópontok** fülre. 

1. Válassza ki, vagy törölje vagy monitorozásához leállíthatja a figyelést a csomóponton. 
2. Válassza ki **figyelés használatát**, vagy törölje a jelölést, szükség szerint. 
3. Kattintson a **Mentés** gombra. 


Adja meg a kívánt képességek:

- [A Teljesítményfigyelő](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [Szolgáltatásvégpont-figyelő](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [ExpressRoute-figyelő](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Adatok gyűjtése részletei
Kimaradások és késések adatokat gyűjthet, a Network Performance Monitor TCP SZIN-SYNACK – Nyugtázási kézfogás csomagokat használ, ha úgy dönt, hogy a TCP protokoll. A Network Performance Monitor ICMP ECHO ICMP ECHO REPLY használja, ha úgy dönt, hogy az ICMP protokoll. Útvonalkövetést topológiával kapcsolatos információk lekérése is szolgál.

Az alábbi táblázat adatgyűjtési módszerek és egyéb hogyan adatgyűjtés a Network Performance Monitor részleteit.

| Platform | Közvetlen ügynök | A System Center Operations Manager-ügynök | Azure Storage | Az Operations Manager szükséges? | A felügyeleti csoport Operations Manager-ügynök adatok küldött | A gyűjtés gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP-kézfogások/ICMP ECHO üzenetek 5 másodpercenként adatokat küldi át 3 percenként |
 

 
A megoldás szintetikus tranzakciók használatával a hálózat állapotának felmérése. Az Operations Management Suite-ügynökök telepítése egymással a hálózaton exchange TCP-csomagokat vagy az ICMP Echo különböző időpontokban. A figyeléshez kijelölt protokoll használja-e az ügynökök a TCP- vagy az ICMP Echo függ. A folyamat ügynökök üzenetváltási idő és a csomag adatvesztést, megtudhatja, ha van ilyen. Minden ügynök rendszeres időközönként is egy nyomkövetési útvonalat a hálózatot, amelyet meg kell vizsgálni a különböző útvonalak keresése más ügynökök hajt végre. Ezen adatok alapján az ügynökök is kikövetkeztetni a hálózati késés és a csomag bontásban. A tesztek öt másodpercenként ismétlődik. Adatok körülbelül 3 percig szerint összesítve az ügynökök a Log Analytics szolgáltatással való feltöltésük előtt.



>[!NOTE]
> Bár az ügynökök kommunikálnak egymással gyakran, jelentős hálózati forgalmat azok nem hoznak létre a tesztek végrehajtása közben. Ügynökök csak a TCP SZIN-SYNACK – Nyugtázási kézfogás csomagok meghatározni a veszteséget és késéseket támaszkodnak. Nincs adatcsomagok megtörténik. Ez a folyamat során ügynökök kommunikálnak egymással csak szükség esetén. Az ügynök kommunikációs topológia a hálózati forgalom csökkentése érdekében optimalizáltuk.

## <a name="use-the-solution"></a>A megoldás használatához 

### <a name="network-performance-monitor-overview-tile"></a>Hálózati Teljesítményfigyelő-áttekintés csempe 

Miután engedélyezte a Network Performance Monitor megoldás, a megoldás csempéjéről a **áttekintése** az oldal a hálózat állapotának gyors áttekintést nyújt. 

 ![Hálózati Teljesítményfigyelő-áttekintés csempe](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Network Performance Monitor-irányítópult 

* **Legfontosabb hálózatállapot-események**: ezen a lapon a rendszer és az idő a legutóbbi hálózatállapot-események és riasztások listáját tartalmazza a óta az események volt aktív. Egy adott állapottal kapcsolatos esemény vagy riasztás jön létre, amikor a figyelési szabály (adatvesztés, késés, válasz vagy sávszélesség-használat) kiválasztott metrika értéke meghaladja a küszöbértéket. 

* **ExpressRoute-figyelő**: ezen a lapon minden állapot-összefoglalók a különböző társviszony-létesítési ExpressRoute-kapcsolatok megoldást figyelőket. A **topológia** csempe a hálózaton keresztül figyelt ExpressRoute-Kapcsolatcsoportok hálózati elérési utak számát jeleníti meg. Válassza ki a csempére a **topológia** megtekintése.

* **Szolgáltatás Kapcsolatfigyelő**: ezen a lapon állapot-összefoglalók kínál a különböző tesztek hozott létre. A **topológia** csempe megjeleníti a figyelt végpontok száma. Válassza ki a csempére a **topológia** megtekintése.

* **Teljesítményfigyelő**: Ez az oldal nyújt az egészségügyi összesítések a **hálózati** hivatkozásokat és **alhálózati** hivatkozásokat, amelyek a megoldás figyeli. A **topológia** csempe figyelt hálózati elérési utak számát jeleníti meg a hálózaton. Válassza ki a csempére a **topológia** megtekintése. 

* **Gyakori lekérdezések**: ezen a lapon a keresési lekérdezések raw hálózatfigyelési adatokat közvetlenül készletét tartalmazza. Ezek a lekérdezések használhatja kiindulási pontként a saját testre szabott jelentéskészítési lekérdezések létrehozása. 

   ![Network Performance Monitor-irányítópult](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>Lehatolhat mélysége 

Kiválaszthatja, hogy a megoldás irányítópultján részletes különböző mutató hivatkozások le részletesebb érdeklődés minden olyan területéhez. Például amikor megjelenik egy riasztás vagy egy nem megfelelő állapotú hálózati kapcsolat megjelenjen az irányítópulton, válassza ki a további vizsgálat. A lap felsorolja az adott hálózati kapcsolathoz tartozó alhálózati kapcsolatok. Láthatja, hogy az adatveszteség, késés és minden alhálózati kapcsolat állapotát. Gyorsan talál problémákat okoz, hogy mely alhálózati hivatkozás. Válassza ki **csomóponti hivatkozások megtekintése** a nem megfelelő állapotú alhálózati hivatkozás csomóponti hivatkozások megtekintéséhez. Ezután tekintse meg az egyes csomópontok közötti hivatkozásokat, és a nem megfelelő állapotú csomóponti hivatkozások találhatók. 

Válassza ki **topológia megtekintése** a Ugrás ugrásra felépülő topológiát a forrás és cél-csomópontok közti útvonalak megtekintéséhez. A nem megfelelő útvonalak piros színnel jelennek meg. Megtekintheti az egyes ugrások százalékaránya, így gyorsan azonosíthatja a problémát, hogy egy adott részének a hálózati késés.

 

### <a name="network-state-recorder-control"></a>Hálózati állapotát rögzítő vezérlő

Minden egyes nézet megjeleníti a hálózati állapotfigyelő pillanatképet egy adott időpontban idő. Alapértelmezés szerint a legfrissebb állapot jelenik meg. Az a lap tetején látható a pont, amelynek az állapot jelenik meg időben. A hálózati állapotfigyelő pillanatképet egy korábbi időpontban megtekintéséhez jelölje ki **műveletek**. Emellett engedélyezheti vagy lapok automatikus frissítésének letiltása, amíg a legfrissebb állapotának megtekintéséhez. 

 ![Hálózati állapotát rögzítő](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Trenddiagramok 

A Lehatolás minden szinten láthatja a alkalmazni metrika alakulását. Adatvesztés, a késés, a válaszidő vagy a sávszélesség-használat lehet. A trend az időintervallum módosításához használja a vezérlő a diagram tetején. 

Trenddiagramok jelzik, hogy egy korábbi szempontjából a teljesítmény-mérőszám teljesítményét. Néhány hálózati probléma átmeneti jellegű és nehezen catch megnézzük a hálózathoz csak az aktuális állapotát. Problémák gyors surface és eltűnnek, mielőtt bárki megjegyzések csak olyan időben későbbi időpontban újra is. Az ilyen átmeneti problémák is rendszergazdái számára bonyolult lehet. A hibák gyakran megjelenjen megtévesztő nő az alkalmazás válaszideje, még ha az összes alkalmazás-összetevőket úgy tűnik, hogy zökkenőmentesen. 

Az ilyen típusú problémák egy trend diagram megnézzük egyszerűen észlelését. A probléma egy hirtelen megugrás a hálózati késés vagy csomagvesztés jelenik meg. Vizsgálja meg a probléma, a hálózati pillanatkép és a pont topológiája megtekintése a probléma fellépésének ideje az a hálózat állapotát rögzítő vezérlőelem használatával.

 
![Trenddiagramok](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topológia 

A Network Performance Monitor útvonalak között a forrás- és végpont egy interaktív topológia-térképként a Ugrás ugrásra felépülő topológiát jeleníti meg. A topológia térkép megtekintéséhez jelölje ki a **topológia** csempe a megoldás irányítópultján. Is kiválaszthat a **topológia megtekintése** a részletező lapokon található hivatkozásra. 

A topológia térképen jeleníti meg, hány útvonalak között a forrás és cél- és mit kell elérési utak adatcsomagok igénybe vehet. Minden egyes hálózati Ugrás százalékaránya késései is látható. Vörös színnel jelennek meg, amelynek a teljes elérési útja késés meghaladja a küszöbértéket (a megfelelő figyelési szabály beállítása) az összes elérési utat. 

Amikor kiválaszt egy csomópont- vagy vigye fölé a topológia-térképként, tekintse meg a csomópont tulajdonságait, például a teljes tartománynév és IP-címet. Válassza ki a ugrás IP-címének megtekintéséhez. A feladatátvételnek hálózati Ugrás a késés üzletmentében okainak alapján azonosíthatja. Szűrési útvonalak adott, a szűrők használatával összecsukható műveleti ablaktábláján. Egyszerűsítése érdekében a hálózati topológiákat, elrejtheti az ugrások műveleti ablaktábláján a csúszka használatával. Nagyítás, vagy a topológia térkép kicsinyítheti az egér kereke használatával. 

A topológia a gráfon látható 3. rétegbeli topológia, és nem tartalmazza 2. rétegbeli eszközök és kapcsolatok. 

 
![Topológia](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>A log Analytics-keresés 

Összes adat, amely grafikusan közzétett keretében a Network Performance Monitor-irányítópult és a részletezés oldal is érhető el natív módon [Log Analytics keresési](log-analytics-log-search-new.md). Az adatok interaktív elemzéseket végezhet a tárházban, és vesse össze a különböző forrásokból származó adatok. Emellett egyéni riasztások és nézetek létrehozása és az adatok exportálása az Excel, a Power bi-ban vagy a megosztható hivatkozás. A **gyakori lekérdezések** tartalmaz néhány hasznos lekérdezést, amely kiindulási pontként használhatja a saját lekérdezéseket és jelentéseket hozhat létre az irányítópulton. 

## <a name="alerts"></a>Riasztások

A Network Performance Monitor riasztási képességeit használja [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts).

Ez azt jelenti, hogy az összes riasztás kezelhetők [Műveletcsoportok](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#overview).  

Ha Ön egy NPM-felhasználó hoz létre riasztást az OMS-n keresztül: 
1. Megjelenik egy hivatkozás, amely átirányítja Önt az Azure Portalon. Kattintson rá a portál eléréséhez.
2. Kattintson a Network Performance Monitor megoldás csempére. 
3. Keresse meg a konfigurálása.  
4. Válassza ki a teszt a riasztás létrehozása, és kövesse az alábbiakban említett lépéseket.

Ha Ön egy NPM-felhasználó hoz létre riasztást, az Azure Portalon keresztül:  
1. Ha szeretné, adja meg az e-maileket közvetlenül, vagy választhatja azt is, hozhat létre riasztásokat Műveletcsoportok használatával.
2. Adja meg az e-maileket közvetlenül választja, ha egy művelet nevű csoport **NPM Email ActionGroup** jön létre, és az e-mail-azonosító van adva ehhez a csoporthoz művelet.
3. Ha Műveletcsoportok használata mellett dönt, akkor kiválaszthat egy korábban létrehozott művelet. Műveletcsoport létrehozása tudhat [itt.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#create-an-action-group-by-using-the-azure-portal) 
4. Ha a riasztás sikeresen létrejött, a riasztások kezelése hivatkozás segítségével a riasztásokat. 

##<a name="pricing"></a>Díjszabás

Díjszabási információk érhetők el [online](log-analytics-network-performance-monitor-pricing-faq.md).

## <a name="provide-feedback"></a>Visszajelzés küldése 

* **Uservoice-on:** bejegyzéseket tehet közzé, amelyeket szeretne dolgozhat a Network Performance Monitor funkciók ötleteit. Látogasson el a [UserVoice-lapunkon](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

* **Csatlakozzon a kohorsz:** mindig érdekel rendelkező új ügyfeleket a kohorsz csatlakozzon. Részeként, az új szolgáltatásokat és segítsen még jobbá tenni a Network Performance Monitor lehetőséget a korai hozzáférést kap. Ha érdekli a csatlakozás, töltse ki ezt [rövid felmérés](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>További lépések 
Tudjon meg többet [Teljesítményfigyelő](log-analytics-network-performance-monitor-performance-monitor.md), [szolgáltatás Kapcsolatfigyelő](log-analytics-network-performance-monitor-performance-monitor.md), és [ExpressRoute-figyelő](log-analytics-network-performance-monitor-expressroute.md). 
